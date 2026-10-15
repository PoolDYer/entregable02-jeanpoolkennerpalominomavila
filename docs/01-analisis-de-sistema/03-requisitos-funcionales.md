# Requisitos funcionales

## Catálogo de Requisitos Funcionales
| ID | Requisito funcional |
| :--- | :--- |
| **RF01** | El sistema debe permitir la autenticación de usuarios mediante credenciales seguras, generando tokens JWT protegidos criptográficamente para el acceso a las APIs. |
| **RF02** | El sistema debe permitir la administración integral de usuarios, asignación de perfiles y privilegios mediante control de acceso basado en roles (RBAC). |
| **RF03** | El sistema debe permitir retener la sesión localmente de forma cifrada en la aplicación móvil para habilitar la autenticación y operación en modo desconectado (offline). |
| **RF04** | El sistema debe registrar en una bitácora inmutable de solo anexado toda operación de creación, actualización o anulación de actas con usuario, marca temporal e IP. |
| **RF05** | El sistema debe permitir registrar, actualizar y consultar centralizadamente a prestadores turísticos (hospedajes, restaurantes, agencias y guías) vinculados a su RUC. |
| **RF06** | El sistema debe controlar las vigencias de licencias municipales, certificados ITSE y registros turísticos, generando alertas preventivas ante vencimientos a menos de 30 días. |
| **RF07** | El sistema debe calcular de manera algorítmica el nivel de criticidad o riesgo (Bajo, Medio, Alto, Crítico) de cada establecimiento evaluando sus antecedentes e infracciones. |
| **RF08** | El sistema debe permitir programar operativos tácticos, segmentar polígonos territoriales y sincronizar hojas de ruta digitales hacia los terminales móviles de los inspectores asignados. |
| **RF09** | El sistema debe permitir el diligenciamiento completo de actas de fiscalización móvil en ausencia de conectividad y conciliarlas transaccionalmente al detectar red. |
| **RF10** | El sistema debe capturar fotografías in situ exclusivamente desde la cámara del terminal móvil, estampando de forma indeleble coordenadas GPS, fecha y hora oficial. |
| **RF11** | El sistema debe generar actas digitales en formato PDF inalterable con captura de firma manuscrita sobre pantalla táctil y sellado de integridad mediante hash criptográfico. |
| **RF12** | El sistema debe gestionar el cómputo de plazos perentorios de subsanación y permitir la carga de pruebas y descargos por parte de los prestadores turísticos. |
| **RF13** | El sistema debe proveer un portal web ciudadano responsivo y público para la consulta de prestadores autorizados y la resolución inmediata de códigos QR sin requerir inicio de sesión. |
| **RF14** | El sistema debe generar mapas de calor GIS y tableros con métricas de informalidad distrital y porcentaje de cobertura inspectiva. |

## Matriz de Trazabilidad (Historias de Usuario vs Requisitos Funcionales)
| Historia de usuario | Requisitos funcionales relacionados |
| :--- | :--- |
| **HU01** Autenticación y sesión segura | RF01, RF02 |
| **HU02** Gestión de usuarios y control RBAC | RF02, RF04 |
| **HU03** Restablecimiento de credenciales | RF01, RF02 |
| **HU04** Consulta pública ciudadana y lector QR | RF13 |
| **HU05** Fiscalización móvil sin conexión | RF01, RF03, RF09 |
| **HU06** Fotos con GPS y firmas en actas digitales | RF10, RF11 |
| **HU07** Programación de operativos y hojas de ruta | RF07, RF08 |
| **HU08** Evaluación de descargos y plazos de ley | RF06, RF12 |
| **HU09** Envío de subsanaciones por administrados | RF05, RF12 |
| **HU10** Tableros analíticos y mapas de calor GIS | RF14 |
| **HU11** Auditoría e inmutabilidad de registros | RF04 |