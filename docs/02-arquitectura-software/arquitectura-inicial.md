# Arquitectura inicial de SIFITUR Huamanga

## Diagrama de arquitectura

![Arquitectura de alto nivel de SIFITUR Huamanga](../../img/arquitectura-alto-nivel.png)

Fuente editable: [arquitectura-alto-nivel.svg](../../img/fuentes-svg/arquitectura-alto-nivel.svg).

## Descripción

La arquitectura es cliente-servidor multicanal. La aplicación móvil permite la inspección offline; la consola administrativa coordina operativos; el portal del administrado recibe descargos; el portal ciudadano verifica formalidad y códigos QR.

| Componente | Responsabilidad | Riesgo mitigado |
|---|---|---|
| CDN | Distribuir recursos estáticos del portal ciudadano. | Latencia y carga repetitiva. |
| Gateway y balanceador NGINX/WAF | Terminar TLS, filtrar y repartir peticiones entre instancias. | Saturación de una instancia y accesos perimetrales indebidos. |
| Backend NestJS sin estado | Ejecutar módulos institucionales en instancias equivalentes. | Dependencia de una máquina de cómputo. |
| PostgreSQL primaria | Confirmar escrituras, auditoría y outbox. | Inconsistencias transaccionales del expediente. |
| Réplica de lectura | Atender reportes y consultas que admiten desfase. | Saturación por lecturas analíticas. |
| Redis | Mantener la proyección pública del padrón y QR. | Consultas repetitivas a la base institucional. |
| S3 compatible | Custodiar originales fotográficos y actas PDF. | Pérdida de archivos al sustituir servidores. |
| RabbitMQ y trabajador | Generar documentos y despachar notificaciones mediante tareas recuperables. | Bloqueo de solicitudes por procesamiento pesado. |

La réplica no sustituye copias de seguridad. La conmutación de la primaria, retención de objetos y redundancia de gateway, caché y cola se configuran como parte del despliegue.
