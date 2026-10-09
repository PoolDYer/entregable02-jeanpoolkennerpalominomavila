# Entregable 01

## nombre
Jean Pool Kenner Palomino Mavila    

## Descripción
**SIFITUR Huamanga** (Sistema Integral de Fiscalización de Hospedajes y Servicios Turísticos en Huamanga) es una plataforma tecnológica institucional diseñada para modernizar, articular y transparentar el proceso integral de empadronamiento, inspección técnica in situ, control de vigencias documentales (licencias municipales, ITSE y constancias de MINCETUR) y seguimiento de subsanaciones en la provincia de Huamanga, en coordinación con la DIRCETUR Ayacucho y la Municipalidad Provincial de Huamanga. La solución articula un ecosistema multicanal compuesto por una aplicación móvil con soporte *offline-first* que permite a los inspectores levantar actas digitales con firmas manuscritas y capturar evidencias fotográficas con GPS forzado en zonas sin conectividad, una consola web administrativa para la planificación de operativos territoriales y evaluación de descargos, y un portal web ciudadano de acceso público orientado a resolver en menos de 200 ms la verificación de formalidad mediante el escaneo de sellos QR oficiales. Sustentada en una arquitectura desacoplada de cómputo sin estado (*stateless*), aceleración en memoria Redis, persistencia transaccional con réplicas de lectura, procesamiento asíncrono y almacenamiento redundante de objetos en S3, la plataforma está dimensionada para absorber picos estacionales de hasta 30,000 visitantes únicos durante festividades como Semana Santa e interoperar con plataformas clave del Estado (SUNAT, MINCETUR, OpenStreetMap y pasarelas de notificación), garantizando seguridad perimetral mediante control de acceso por roles (RBAC), auditoría inmutable en bitácoras *append-only* y estricto cumplimiento de la Ley N° 29733 de Protección de Datos Personales.

## Caso de estudio
Sistema Integral de Fiscalización de Hospedajes y Servicios Turísticos en Huamanga (SIFITUR Huamanga)

## Curso
Arquitectura de Software

## Documentación del laboratorio 03

Este repositorio contiene la propuesta documental de SIFITUR Huamanga. Las tecnologías, métricas y diagramas describen el diseño previsto; todavía no existe una aplicación ejecutable ni evidencia de cumplimiento de los objetivos de calidad.

| Entregable | Documento |
|---|---|
| Necesidad del negocio | [Problema, objetivos y alcance](analisis-de-sistema/00-necesidad-del-negocio.md) |
| Requisitos | [Actores](analisis-de-sistema/01-actores.md), [historias de usuario](analisis-de-sistema/02-historias-del-usuario.md), [requisitos funcionales](analisis-de-sistema/03-requisitos-funcionales.md) y [restricciones](analisis-de-sistema/05-restricciones.md) |
| Atributos de calidad | [Escenarios y objetivos](analisis-de-sistema/04-atributos-de-calidad.md) |
| Drivers arquitectónicos | [Necesidades que condicionan el diseño](analisis-de-sistema/06-driver-arquitectonicos.md) |
| Decisiones arquitectónicas | [ADR, alternativas y consecuencias](analisis-de-sistema/07-decisiones-arquitectonicas.md) |
| Estilo arquitectónico | [Backend modular y estructura global](arquitectura/estilo-arquitectonico.md) |
| Enfoque Clean Architecture | [Responsabilidades, estructura propuesta y dependencias](arquitectura/enfoque/enfoque-arquitectonico.md) |

La [arquitectura inicial](arquitectura/arquitectura-inicial.md) se conserva como antecedente. El [informe de revisión](docs/revision-laboratorio-03.md) explica la correspondencia con la Guía 03, la equivalencia de requisitos entre la propuesta Word y el repositorio, y las observaciones pendientes de aclaración.

Los diagramas están escritos en Mermaid y pueden visualizarse en GitHub o en un visor Markdown compatible. El laboratorio se centra en analizar y justificar la arquitectura; no requiere desarrollar funcionalidades nuevas.
