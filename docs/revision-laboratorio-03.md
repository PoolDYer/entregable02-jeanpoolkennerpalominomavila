# Revisión documental del laboratorio 03

## Resultado y alcance

La revisión de la Guía 03, la propuesta SIFITUR Huamanga y los documentos de ambos proyectos identificó tres archivos faltantes respecto al ejemplo Marketplace: decisiones arquitectónicas, estilo y enfoque. Se agregó además un documento de necesidad del negocio para desarrollar el primer entregable solicitado por la guía, que estaba resumido en el README.

Este informe registra correspondencias y observaciones de contenido. Las nuevas decisiones son propuestas de diseño y no acreditan una implementación, pruebas de carga ni cumplimiento normativo. Los documentos anteriores se conservan como antecedentes y las diferencias detectadas se explican aquí.

## Fuentes revisadas

- `GUIA-003-ASF.pdf`: nueve páginas, incluidos el esquema del proceso arquitectónico y el diagrama del estilo global. El laboratorio pide analizar y justificar, sin desarrollar funcionalidades nuevas.
- `Propuesta SIFITUR Huamanga.docx`: caracterización del proceso, dimensionamiento, requisitos, tablas y descripción de las vistas HLD y C4. Las cifras se toman como estimaciones académicas.
- Todos los Markdown propios de `Marketplace-arquitSoft-02-jeanpoolkennerpalominomavila`: doce documentos, incluido el README del boilerplate. No se consideran documentación del proyecto los archivos de dependencias instaladas ni metadatos Git.
- Todos los archivos propios versionados originalmente en `entregable01-jeanpoolkennerpalominomavila`: ocho Markdown y `.gitignore`. No hay código de aplicación SIFITUR ni imágenes de diagramas adicionales en esos archivos.

## Correspondencia con los entregables de la guía

| Entregable | Evidencia original | Resultado documental |
|---|---|---|
| 1. Necesidad del negocio | README con descripción general. | [Necesidad del negocio](../analisis-de-sistema/00-necesidad-del-negocio.md): problema, objetivos, proceso, participantes y alcance. |
| 2. Requisitos | Actores, historias, requisitos funcionales y restricciones. | Conservados; se documentan diferencias de trazabilidad más adelante. |
| 3. Atributos de calidad | `04-atributos-de-calidad.md`, AC01-AC07. | Conservados; métricas tratadas como objetivos pendientes de validación. |
| 4. Drivers arquitectónicos | `06-driver-arquitectonicos.md`, DA01-DA06. | Conservados; decisiones vinculadas a su significado en SIFITUR. |
| 5. Decisiones arquitectónicas | No existía archivo específico. | [Decisiones arquitectónicas](../analisis-de-sistema/07-decisiones-arquitectonicas.md), siete ADR con contexto, alternativas y consecuencias. |
| 6. Estilo arquitectónico | Diagrama de arquitectura inicial, sin selección explícita. | [Estilo arquitectónico](../arquitectura/estilo-arquitectonico.md), selección, módulos, límites y diagrama global. |
| Enfoque Clean Architecture | No existía archivo específico. | [Enfoque](../arquitectura/enfoque/enfoque-arquitectonico.md), capas, estructura propuesta y dependencias de código. |

La guía ubica el estilo dentro de `arquitectura/` y el enfoque dentro de `arquitectura/enfoque/`. Se sigue esa ruta. Marketplace almacena el estilo en una subcarpeta `estilo/` y usa un nombre acentuado para el enfoque; no se necesita duplicar los documentos para reproducir esas variantes.

## Revisión de los Markdown originales de SIFITUR

| Archivo | Contenido verificado | Observación |
|---|---|---|
| [README](../README.md) | Descripción, autor, caso y curso. | Resumía tecnologías y objetivos, pero faltaba un índice de entregables. Se incorporó navegación. |
| [Actores](../analisis-de-sistema/01-actores.md) | Cinco actores humanos y cuatro sistemas externos. | Contempla denuncias y emisión de resoluciones que no tienen requisitos detallados específicos. |
| [Historias de usuario](../analisis-de-sistema/02-historias-del-usuario.md) | HU01-HU11. | HU03 trata recuperación por correo, pero RF01/RF02 no especifican ese flujo. |
| [Requisitos funcionales](../analisis-de-sistema/03-requisitos-funcionales.md) | RF01-RF14 y matriz HU-RF. | Falta desarrollar criterios de aceptación para recuperación, denuncias, resoluciones y exportaciones. |
| [Atributos de calidad](../analisis-de-sistema/04-atributos-de-calidad.md) | Rendimiento, disponibilidad, escala, integridad, seguridad, offline y auditoría. | AC05 exige JWT para toda petición, en conflicto con RF13 público. AC07 atribuye no repudio al registro append-only sin detallar controles. |
| [Restricciones](../analisis-de-sistema/05-restricciones.md) | Canales, JWT, SQLite, REST, objetos, integraciones y protección de datos. | Acceso a integraciones y condiciones de operación necesitan confirmación. Las tecnologías provienen de la propuesta, no de mediciones comparativas. |
| [Drivers](../analisis-de-sistema/06-driver-arquitectonicos.md) | DA01 seguridad, DA02 offline, DA03 escala, DA04 latencia, DA05 evidencias y DA06 interoperabilidad. | Mezcla necesidad con tácticas ya seleccionadas. Redis, WAF y réplicas deben entenderse como respuestas propuestas, no como consecuencias únicas posibles. |
| [Arquitectura inicial](../arquitectura/arquitectura-inicial.md) | Canales, gateway, módulos, datos e integraciones en Mermaid. | El paso general por AuthGuard debe permitir rutas públicas. El nuevo estilo explicita tareas, límites y unidades de ejecución. |
| `.gitignore` | Exclusiones de archivos del sistema y configuración local del editor. | Adecuado para el repositorio documental actual; no existen fuentes ejecutables que requieran nuevas exclusiones. |

## Equivalencia de requisitos entre propuesta y repositorio

Se conserva la numeración del repositorio para evitar romper sus enlaces y matrices. Los códigos `RF-xx` de Word no equivalen numéricamente a `RFxx` de Markdown.

| Requisito de la propuesta Word | Requisito del repositorio | Correspondencia |
|---|---|---|
| RF-01 Padrón | RF05 | Registro y consulta de prestadores. |
| RF-02 Vigencias | RF06 | Control documental y alertas menores a 30 días. |
| RF-03 Riesgo | RF07 | Clasificación de criticidad. |
| RF-04 Operativos | RF08 | Campañas, polígonos y rutas. |
| RF-05 Fiscalización offline | RF03, RF09 | Sesión local y captura/sincronización. |
| RF-06 Evidencias y GPS | RF10 | Cámara obligatoria y metadatos. |
| RF-07 Actas y firma digitalizada | RF11 | PDF, firma manuscrita y hash; envío por correo cubierto en ADR-004/ADR-006. |
| RF-08 Subsanaciones | RF12 | Descargos y control de plazos. |
| RF-09 Consulta y QR | RF13 | Consulta pública sin sesión. |
| RF-10 Tableros y GIS | RF14 | Indicadores y mapas; exportación Excel, GeoJSON y PDF no aparece explícitamente en RF14. |

RF01, RF02 y RF04 desarrollan autenticación, permisos y auditoría en el repositorio; no tienen códigos funcionales equivalentes directos en la tabla de Word, aunque seguridad y auditoría aparecen en sus requisitos no funcionales.

## Observaciones de consistencia y tratamiento

| Hallazgo | Tratamiento en la documentación nueva | Pendiente |
|---|---|---|
| La propuesta indica 99.5 % de disponibilidad anual y máximo 43 minutos de caída anual. | No se reproduce esa equivalencia. En un año de 365 días, 99.5 % permite 43.8 horas; 99.9 % permite 8.76 horas. | Elegir una meta coherente y definir ventana y exclusiones de medición. |
| 30,000 visitantes únicos se describen como usuarios concurrentes en partes del análisis. | Se distingue visitantes acumulados, accesos diarios y solicitudes por minuto. | Medir concurrencia real, mezcla de operaciones y tamaño de cargas. |
| AC05 solicita JWT en toda API, pero RF13 exige acceso anónimo. | ADR-007 diferencia rutas privadas con JWT de rutas públicas de consulta limitada. | Armonizar AC05 cuando se actualice el catálogo original. |
| Sesión offline puede coexistir con permisos revocados en servidor. | ADR-005 y ADR-007 separan captura local de autorización final al sincronizar. | Definir vigencia offline y resolución institucional de operaciones rechazadas. |
| Captura offline exige fecha y hora oficial. | Se distinguen tiempo local declarado y recepción del servidor. | Acordar validación de reloj y política ante desfases o ubicación no disponible. |
| PDF inalterable y hash se presentan como garantía de valor probatorio. | Se documentan integridad, versiones y custodia sin equiparar hash con firma digital certificada. | Definir requisitos de autenticidad, conservación y validación con responsables institucionales. |
| Objetos, base y cola se presentan como conciliación transaccional única. | ADR-006 usa estados, outbox, idempotencia y recuperación; la transacción ACID se limita a PostgreSQL. | Diseñar y probar fallos parciales y restauración. |
| Réplica de lectura se presenta como conmutación automática del primario. | El estilo distingue réplica, copias de seguridad y mecanismos de alta disponibilidad. | Configurar y probar failover y restauración; definir RPO/RTO. |
| Consulta QR inmediata puede leer caché o réplica desactualizada. | ADR-003 incorpora invalidación durable y limita qué lecturas toleran desfase. | Acordar el límite de desactualización del estado público. |
| La propuesta C4 detalla tres actores en algunas vistas; el repositorio identifica cinco. | El estilo conserva inspector, coordinador, administrador, prestador y ciudadano. | Mantener los cinco actores al ampliar las vistas C4. |
| RNF-08 de costos no tiene atributo equivalente explícito en AC01-AC07. | ADR-001 lo cita como origen en la propuesta sin inventar otro AC. | Definir presupuesto y métricas de costo antes de dimensionar operación. |

## Revisión del ejemplo Marketplace

| Markdown revisados | Contenido y hallazgo relevante para la adaptación |
|---|---|
| `README.md` | Autor, descripción y caso GoPet. Sirve de referencia de presentación, no de contenido de negocio SIFITUR. |
| `analisis-de-sistema/01-actores.md` | Cliente, seller, administrador y servicios externos. Termina con una cerca de código sin apertura. |
| `analisis-de-sistema/02-historias-del-usuario.md` | HU01-HU06 del marketplace. Termina con una cerca de código sin apertura. |
| `analisis-de-sistema/03-requisitos-funcionales.md` | RF01-RF08 y matriz de trazabilidad. No debe trasladarse el flujo carrito/pedido a fiscalización. |
| `analisis-de-sistema/04-atributos-de-calidad.md` | Cinco atributos; sus escenarios no incluyen umbrales medibles. |
| `analisis-de-sistema/05-restricciones.md` | Aplicación web, Git, REST, pagos y envío. Las integraciones de SIFITUR son institucionales, no comerciales. |
| `analisis-de-sistema/06-driver-arquitectonicos.md` | Repite DA05 para mantenibilidad, mientras el archivo de decisiones lo cita como DA06. Se evita copiar ese error. |
| `analisis-de-sistema/07-decisiones-arquitectonicas.md` | Cuatro decisiones en tabla. SIFITUR amplía contexto, alternativas y consecuencias, además de offline y evidencias. |
| `arquitectura/arquitectura-inicial.md` | Diagrama en capas; coloca integraciones desde datos. En SIFITUR la integración se hace por adaptadores de aplicación. |
| `arquitectura/estilo/estilo-arquitectonico.md` | Monolito Node.js/Express y capas descendentes. Se adapta a NestJS y se distingue flujo de ejecución de dependencia de código. |
| `arquitectura/enfoque/enfoque-arquitectónico.md` | Clean Architecture, contratos y composición; mezcla rutas en inglés con carpetas españolas del boilerplate. El diagrama conecta un pago simulado a API y no coincide completamente con el README del ejemplo. |
| `boilerplate-main/README.md` | Cuatro capas, contratos en dominio, adaptadores y comandos del ejemplo Angular. La auditoría es documental; no se afirma haber ejecutado sus pruebas. |

Los defectos del proyecto de referencia se registran para evitar reproducirlos; no se modificó Marketplace ni se trasladó su boilerplate a SIFITUR.

## Criterios de entrega y límites

- Los seis entregables y el enfoque cuentan con documentos localizables desde el README.
- Los nuevos documentos usan los identificadores RF, AC, RC y DA existentes, y declaran por separado los objetivos añadidos desde la guía o la propuesta.
- Los diagramas global y de dependencias se incluyen en Mermaid como representaciones distintas.
- La estructura de código se presenta como propuesta, sin crear funcionalidades.
- Cada archivo terminado se registra en un commit propio con mensaje `docs:` descriptivo. El README también recibe un commit separado.
- Antes de publicar se comprueban enlaces locales, cercas de código, espacios de diff y contenido por commit. Las pruebas de funcionamiento de SIFITUR quedan para la implementación.

Este entregable cubre el alcance documental de la sección VII de la Guía 03. Las observaciones pendientes requieren acuerdos o desarrollo posterior; no quedan resueltas por añadir diagramas.
