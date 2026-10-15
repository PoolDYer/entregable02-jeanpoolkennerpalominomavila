# Decisiones arquitectónicas de SIFITUR Huamanga

## Contexto y estado

Estas decisiones desarrollan el paso 3 de la Guía 03 para el caso SIFITUR. Su estado es **propuesto para el diseño académico**: todavía no existe implementación ni evidencia de cumplimiento de los objetivos de calidad. Los identificadores DA01 a DA06 conservan el significado del [catálogo existente](06-driver-arquitectonicos.md); DA06 corresponde a interoperabilidad, no a mantenibilidad como en el ejemplo Marketplace.

## Catálogo de decisiones

| ID | Decisión | Driver relacionado | Justificación | Resultado previsto |
|---|---|---|---|---|
| ADR-001 | Backend como monolito modular con instancias sin estado. | DA03; RNF-08 de la propuesta. | Mantener una unidad de despliegue del negocio y permitir replicar su ejecución sin administrar microservicios desde el inicio. | Módulos de identidad, padrón, operativos, fiscalización, subsanaciones, riesgo, consulta y auditoría. |
| ADR-002 | Clean Architecture por módulo. | DA02, DA05, DA06; objetivo de mantenibilidad de la guía. | Aislar reglas de fiscalización de frameworks, persistencia y proveedores. | Dominio, aplicación, presentación e infraestructura con dependencias hacia el interior. |
| ADR-003 | Consulta pública con caché y réplica de lectura. | DA03, DA04. | Absorber el predominio de lecturas y mantener las escrituras institucionales separadas de los reportes. | Redis para fichas públicas y PostgreSQL de lectura para consultas tolerantes a desfase. |
| ADR-004 | Integraciones mediante puertos y adaptadores. | DA06. | Evitar que cambios o indisponibilidad de proveedores invadan los casos de uso. | Adaptadores SUNAT, MINCETUR, GIS y notificaciones. |
| ADR-005 | Registro móvil offline y sincronización idempotente. | DA02. | Continuar la captura de actas sin red y evitar duplicados cuando se reintenta el envío. | SQLite cifrada, cola local y conciliación con el backend. |
| ADR-006 | Evidencias en objetos y tareas asíncronas recuperables. | DA05. | Separar archivos pesados del cómputo y preservar su relación con el expediente. | S3 compatible, RabbitMQ, trabajador e historial de procesamiento. |
| ADR-007 | Autorización por rol y auditoría de acciones sensibles. | DA01, DA05. | Proteger expedientes y mantener trazabilidad de cambios. | JWT para rutas privadas, RBAC en backend y registro de auditoría con acceso restringido. |

## ADR-001 Backend modular

**Contexto:** la propuesta estima 35 inspectores, 12 coordinadores, 4 administradores, 600 prestadores y una capacidad objetivo de 1,200 solicitudes por minuto. Se necesita controlar el costo operativo y atender picos estacionales.

**Decisión:** una aplicación backend Node.js/NestJS organiza módulos de negocio con interfaces explícitas. Sus instancias se replican detrás del gateway y balanceador. La aplicación móvil, los portales, los almacenes y el trabajador asíncrono constituyen unidades separadas; monolito modular describe el backend, no todo el ecosistema.

**Alternativas:** microservicios por módulo o monolito sin límites internos. Se prefiere la modularidad porque permite separar responsabilidades con menor carga de operación que los microservicios. La existencia de una cola no transforma automáticamente el backend en microservicios.

**Consecuencias:** el backend se publica y escala como unidad. Un cambio requiere validar su integración completa. El trabajador puede escalar por separado para tareas pesadas. Debe evitarse guardar sesiones o archivos definitivos en memoria o disco de una instancia.

## ADR-002 Dependencias hacia el núcleo

**Contexto:** las reglas de riesgo, vigencias y subsanaciones deben poder revisarse sin depender de Flutter, NestJS, Redis o S3.

**Decisión:** el dominio conserva entidades y reglas; aplicación orquesta casos de uso y define puertos; presentación transforma entradas y salidas; infraestructura implementa los puertos. La raíz de composición conecta las implementaciones.

**Alternativas:** servicios de negocio que usan directamente ORM y SDK, o arquitectura hexagonal. Clean Architecture se selecciona para cumplir el enfoque trabajado en la guía y expresar de manera explícita las cuatro responsabilidades.

**Consecuencias:** aumenta el número de contratos y transformaciones. A cambio, reglas y casos de uso pueden probarse con adaptadores en memoria. La mantenibilidad es un objetivo adicional de la guía; no se inventa ni se renumera un driver existente.

## ADR-003 Lecturas públicas y consistencia

**Contexto:** el patrón previsto es 95 % lectura y 5 % escritura; AC01 define consulta pública menor a 200 ms y APIs transaccionales con P95 menor a 250 ms.

**Decisión:** Redis almacena únicamente la proyección pública del padrón y QR. PostgreSQL primaria confirma las escrituras; la réplica atiende reportes y consultas que toleren desfase. Al cambiar la formalidad se invalida o actualiza la ficha pública mediante una tarea registrada de forma durable junto con la transacción.

**Alternativas:** todas las consultas a la primaria o caché sin invalidación. Se prioriza descargar las lecturas manteniendo un mecanismo explícito de actualización.

**Consecuencias:** caché y réplica pueden mostrar datos anteriores al último cambio. Se debe acordar un límite de desactualización; mientras no se valide, no se promete consulta estrictamente instantánea. Las decisiones sobre sanciones y estados actuales consultan la primaria. Ante fallo de Redis se consulta la base con límites de carga. CDN se aplica a recursos estáticos y solo a respuestas públicas con políticas apropiadas.

## ADR-004 Servicios externos desacoplados

**Contexto:** RC06 identifica SUNAT, MINCETUR, OpenStreetMap y SMS/correo.

**Decisión:** aplicación define contratos de validación tributaria, consulta sectorial, cartografía y notificación; infraestructura implementa los clientes. Se configuran tiempos límite, reintentos acotados y circuit breaker. Las notificaciones se envían mediante tareas asíncronas con identificador de envío.

**Alternativa:** consumir proveedores desde controladores o entidades. Se descarta porque acopla el negocio al protocolo y al proveedor.

**Consecuencias:** una consulta externa no disponible queda pendiente de validación y no se interpreta como establecimiento conforme. El registro local del expediente puede conservarse. Acceso, permisos, cuotas, contratos de API y disponibilidad de cada integración requieren confirmación antes de implementarla.

## ADR-005 Trabajo desconectado

**Contexto:** RF03 y RF09 exigen persistencia local y conciliación al recuperar red.

**Decisión:** la aplicación móvil guarda actas y referencias de evidencias en SQLite cifrada. Cada operación tiene identificador único, versión y estado de sincronización. El servidor registra el identificador procesado para que un reintento no cree otra acta. Un conflicto de versión se devuelve para revisión; no se sobrescribe silenciosamente un acta emitida.

**Alternativa:** exigir conexión para capturar o reenviar todo el lote sin control de duplicados. No satisface continuidad ni integridad.

**Consecuencias:** el inspector puede capturar con una sesión local previamente habilitada bajo una política de vigencia. Esa sesión no prueba una autorización remota actual ni habilita llamadas con JWT vencido: al sincronizar se revalida la identidad y el permiso. La hora del dispositivo se conserva como dato de captura y se distingue de la hora de recepción del servidor.

## ADR-006 Custodia de evidencias

**Contexto:** RF10 y RF11 y AC04 requieren fotos, firmas digitalizadas y actas relacionadas con el expediente.

**Decisión:** los objetos se guardan con identificadores estables, metadatos y hashes; PostgreSQL conserva sus referencias y estados. La confirmación final exige verificar la persistencia del objeto y la transacción del expediente. Una tabla de salida transaccional (outbox) registra las tareas a publicar en RabbitMQ; el trabajador procesa de forma idempotente y conserva fallos para recuperación.

**Alternativas:** archivos en el disco del servidor o publicar mensajes sin registro transaccional. Se descartan por fragilidad ante cambios de instancia y pérdidas entre escritura y publicación.

**Consecuencias:** PostgreSQL, S3 y RabbitMQ no participan de una única transacción ACID. El flujo necesita estados pendientes, reintentos y conciliación. No se elimina la copia local hasta confirmar la recepción durable. Se preserva el archivo original; versiones optimizadas para visualizar no lo sustituyen. Versionado, retención, copias y restauración deben configurarse y verificarse. Un hash detecta modificaciones respecto de un valor confiable; no constituye por sí mismo una firma digital certificada ni garantiza no repudio.

## ADR-007 Seguridad y auditoría

**Contexto:** existen rutas privadas para inspectores, coordinadores, administradores y prestadores, y RF13 exige consulta ciudadana sin inicio de sesión.

**Decisión:** el gateway aplica controles perimetrales; el backend verifica JWT y permisos sobre cada operación privada y cada expediente. La consulta pública usa una proyección con campos autorizados y no exige JWT. Las mutaciones sensibles registran usuario, acción, expediente y tiempo del servidor; para captura offline se conserva además el tiempo local declarado.

**Alternativa:** validar solo en la interfaz o aplicar autenticación obligatoria al portal QR. La primera deja accesos sin protección efectiva y la segunda contradice RF13.

**Consecuencias:** RBAC se complementa con verificación de pertenencia del expediente. Auditoría requiere permisos de solo inserción para la aplicación, acceso de lectura controlado y detección de alteraciones; llamarla append-only no basta. La revocación inmediata de permisos no puede garantizarse en un móvil desconectado. El objetivo de protección de datos indicado en las fuentes necesita evaluación específica antes de afirmar cumplimiento.

## Validación prevista

| Decisiones | Evidencia a obtener durante implementación |
|---|---|
| ADR-001, ADR-003 | Prueba con 1,200 solicitudes/minuto, latencia P95 y fallo de una instancia; medir retraso de réplica e invalidación. |
| ADR-002, ADR-004 | Reglas probadas sin framework ni red; sustitución de adaptadores y simulación de caída externa. |
| ADR-005 | Captura sin red, reconexión, reenvío repetido y conflicto de versiones sin pérdida ni duplicados. |
| ADR-006 | Recuperación después de fallar cada paso, verificación de hashes y restauración de evidencias. |
| ADR-007 | Consulta QR sin token; denegación de acceso a expedientes ajenos y trazabilidad de cambios. |

Estas pruebas se proponen como criterios futuros; no se declaran ejecutadas en este entregable documental.

## Documentos relacionados

- [Estilo arquitectónico](../arquitectura/estilo-arquitectonico.md).
- [Enfoque Clean Architecture](../arquitectura/enfoque/enfoque-arquitectonico.md).
- [Informe de revisión y correspondencia con la guía](../docs/revision-laboratorio-03.md).
