# Escenarios de atributos de calidad de SIFITUR Huamanga

## EQ-01. Rendimiento: consulta pública

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC01 |
| Fuente | Turista / ciudadano |
| Estímulo | Consulta un prestador o resuelve su QR. |
| Artefacto | Portal ciudadano, PublicQueryService y CacheRepository. |
| Entorno | Carga de 1,200 solicitudes/minuto; datos representativos; caché estabilizada. |
| Respuesta esperada | Devuelve exclusivamente la ficha pública autorizada. |
| Medida | P95 < 200 ms para consulta pública; P95 < 250 ms en API transaccional sin transferencia de archivos. |
| Verificación | Medir latencia del servicio, errores, aciertos de caché y tasa de solicitudes durante 30 minutos. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-02. Disponibilidad: continuidad institucional

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC02 |
| Fuente | Usuarios y monitor de servicio |
| Estímulo | Falla una instancia del backend durante una festividad. |
| Artefacto | Gateway, instancias NestJS y operaciones críticas. |
| Entorno | Despliegue redundante y monitoreo activo. |
| Respuesta esperada | Redirige solicitudes a instancias saludables y mantiene la consulta y el registro institucional. |
| Medida | Disponibilidad anual ≥ 99.5 %; durante festividades ≥ 99.9 %. Sobre 365 días, 99.5 % permite 43.8 horas de indisponibilidad. |
| Verificación | Sondas de consulta y registro; calcular tiempo disponible / tiempo observado; simular caída de instancia. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-03. Escalabilidad: demanda estacional

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC03 |
| Fuente | Aumento del tráfico ciudadano |
| Estímulo | Crece la tasa de consulta en campañas turísticas. |
| Artefacto | Balanceador, backend, Redis y réplica de lectura. |
| Entorno | Carga ascendente hasta 1,200 solicitudes/minuto (20 por segundo). |
| Respuesta esperada | Distribuye carga y agrega instancias cuando el uso de CPU supera 70 %. |
| Medida | Sostener 1,200 solicitudes/minuto durante 30 minutos sin pérdida de registros y con los P95 de EQ-01. |
| Verificación | Prueba de carga incremental; registrar CPU, memoria, latencia, errores y número de instancias. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-04. Durabilidad: custodia de evidencias

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC04 |
| Fuente | Inspector y fallo de almacenamiento |
| Estímulo | Se interrumpe el procesamiento tras recibir una fotografía o acta. |
| Artefacto | S3, referencias PostgreSQL y cola local móvil. |
| Entorno | Reintentos después de fallos; objetos versionados y copias configuradas. |
| Respuesta esperada | Recupera el procesamiento sin perder originales ni confirmar evidencias inexistentes. |
| Medida | Cero objetos confirmados perdidos en los ensayos; 100 % de hashes coincidentes. La durabilidad de 99.999999999 % anual es el objetivo del servicio S3 contratado. |
| Verificación | Interrumpir cada etapa, restaurar copias y comprobar objetos, referencias y hashes. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-05. Seguridad: acceso por rol y expediente

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC05 |
| Fuente | Usuario sin sesión, rol incorrecto o expediente ajeno |
| Estímulo | Intenta consultar o modificar información privada. |
| Artefacto | Guard JWT/RBAC, controladores y casos de uso privados. |
| Entorno | Rutas privadas con TLS y persistencia cifrada; consulta QR pública separada. |
| Respuesta esperada | Deniega la operación, no cambia información y no expone campos privados. |
| Medida | Rechazar 100 % de casos no autorizados de la matriz; QR público accesible sin JWT; TLS 1.3 y AES-256 verificados en el entorno. |
| Verificación | Pruebas por rol, pertenencia y token; inspeccionar respuestas y configuración de cifrado. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-06. Resiliencia: inspección desconectada

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC06 |
| Fuente | Inspector de campo |
| Estímulo | Pierde señal al diligenciar un acta y luego restablece conexión. |
| Artefacto | App Flutter, SQLite cifrada y sincronización del backend. |
| Entorno | Sesión local habilitada; móvil sin red durante la captura. |
| Respuesta esperada | Conserva fichas, firma y fotos; al reconectar revalida permisos y sincroniza por operación única. |
| Medida | 100 % de registros de la prueba recuperados; cero actas duplicadas después de tres reenvíos por operación; conflicto explícito si cambia el contenido. |
| Verificación | Desconectar red, reiniciar la app, capturar, reconectar y repetir el envío. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-07. Auditoría: trazabilidad de cambios

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC07 |
| Fuente | Usuario autorizado |
| Estímulo | Crea, modifica o anula un acta. |
| Artefacto | Bitácora append-only y transacción de inspección. |
| Entorno | Operación privada; hora de recepción del servidor y tiempo de captura separados. |
| Respuesta esperada | Registra actor, acción, entidad, fecha y origen; restringe modificación y borrado. |
| Medida | 100 % de mutaciones confirmadas con evento de auditoría; cero UPDATE o DELETE permitidos al usuario de aplicación sobre la bitácora. |
| Verificación | Relacionar transacciones con eventos; probar permisos de escritura y revisión de integridad. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-08. Eficiencia de costos: carga y almacenamiento

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC08 |
| Fuente | Administrador TI |
| Estímulo | Dimensiona capacidad y revisa consumo institucional. |
| Artefacto | Clúster, caché, réplica, objetos y mensajería. |
| Entorno | Misma carga objetivo, región y volumen de archivos en escenarios comparables. |
| Respuesta esperada | Presenta costos mensuales por recurso y utiliza software libre en los componentes indicados. |
| Medida | 100 % de recursos con estimación de costo y consumo; presupuesto mensual institucional pendiente de definición. |
| Verificación | Inventariar recursos, solicitudes, GB almacenados, transferencia y horas de cómputo; comparar con presupuesto aprobado. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## EQ-09. Mantenibilidad: sustitución de proveedor

| Elemento | Descripción |
|---|---|
| Atributo de calidad | AC09 |
| Fuente | Equipo de desarrollo |
| Estímulo | Sustituye el almacenamiento o notificador de un módulo. |
| Artefacto | Puertos de dominio, adaptadores e InspeccionesModule. |
| Entorno | Pruebas del núcleo ejecutadas sin NestJS ni acceso a red. |
| Respuesta esperada | Agrega una implementación y actualiza la composición conservando las reglas. |
| Medida | Cero cambios en dominio y casos de uso; 100 % de pruebas de contrato y regresión aprobadas. |
| Verificación | Comparar cambios, revisar importaciones y ejecutar pruebas con ambos adaptadores. |
| Estado | Objetivo de diseño; verificación en el entorno de despliegue. |

## Resumen de escenarios

| Código | Atributo | Decisiones relacionadas |
|---|---|---|
| EQ-01 | AC01 | ADR-003 |
| EQ-02 | AC02 | ADR-001 |
| EQ-03 | AC03 | ADR-001, ADR-003 |
| EQ-04 | AC04 | ADR-006 |
| EQ-05 | AC05 | ADR-007 |
| EQ-06 | AC06 | ADR-005 |
| EQ-07 | AC07 | ADR-007 |
| EQ-08 | AC08 | ADR-001, ADR-003 |
| EQ-09 | AC09 | ADR-002, ADR-004 |
