# Atributos de calidad

| ID | Atributo de calidad | Escenario de calidad |
| :--- | :--- | :--- |
| **AC01** | Rendimiento y Latencia | El portal de consulta pública ciudadana y la resolución de códigos QR deben responder en menos de 200 ms bajo carga pico asistidas por caché Redis; las APIs transaccionales deben registrar tiempos P95 < 250 ms. |
| **AC02** | Disponibilidad (Uptime) | El sistema debe mantener una disponibilidad del 99.5% anual y del 99.9% durante festividades críticas (Semana Santa y carnavales) mediante redundancia en clúster sin puntos únicos de fallo (SPOF). |
| **AC03** | Escalabilidad Concurrente | El sistema debe escalar horizontalmente de forma automática para absorber incrementos de tráfico de hasta 1,200 solicitudes por minuto y 30,000 visitantes únicos sin experimentar degradación del servicio. |
| **AC04** | Durabilidad e Integridad | El sistema debe contar con tolerancia cero a la pérdida de información para actas de fiscalización y fotografías forenses mediante almacenamiento desacoplado de objetos S3 con durabilidad de 99.999999999%. |
| **AC05** | Seguridad y Control de Acceso | Toda petición hacia la API debe verificarse contra tokens JWT y políticas RBAC; las transferencias deben viajar bajo TLS 1.3, las bases de datos deben cifrarse con AES-256 en reposo y cumplir la Ley N° 29733 de Protección de Datos Personales. |
| **AC06** | Resiliencia en Desconexión (Offline-First) | La aplicación móvil debe operar de manera 100% autónoma en ausencia de señal de red móvil, reteniendo actas y fotografías en una base de datos local SQLite cifrada hasta la sincronización transaccional. |
| **AC07** | Inmutabilidad y Auditoría | Cada acción sensible (emisión de actas, registro de firmas, modificación de usuarios o anulación) debe almacenarse en una bitácora estructurada de solo anexado (Append-Only Log) garantizando su no repudio. |