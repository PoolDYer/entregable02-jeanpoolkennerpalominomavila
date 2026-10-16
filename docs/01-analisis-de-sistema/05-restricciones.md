# Restricciones

| ID | Restricción | Descripción |
| :--- | :--- | :--- |
| **RC01** | Arquitectura Híbrida Web y Móvil | El sistema debe desarrollarse con una aplicación móvil para inspectores (compilada en Flutter) y portales web responsivos (React SPA para gestión y Next.js SSR para portal público). |
| **RC02** | Autenticación y Esquema RBAC | La plataforma debe implementar control de acceso basado en roles (RBAC) con tokens JWT de corta duración y refresh tokens tanto en canales web como en la app móvil. |
| **RC03** | Persistencia Móvil Cifrada | La base de datos local del terminal móvil debe estar implementada en SQLite con cifrado robusto para resguardar las actas y credenciales retenidas ante robo o pérdida del equipo. |
| **RC04** | Comunicación mediante API REST | Toda interacción entre las aplicaciones clientes (móvil y web) y la capa backend debe ejecutarse mediante servicios API REST protegidos bajo protocolo HTTPS/TLS 1.3. |
| **RC05** | Almacenamiento de Objetos Desacoplado | Las fotografías con metadatos GPS y las actas firmadas en PDF deben almacenarse en repositorios desacoplados compatibles con S3, sin guardarse en discos de cómputo locales. |
| **RC06** | Integración con Plataformas del Estado | La solución debe integrarse mediante adaptadores desacoplados a la Plataforma RUC de SUNAT, al Directorio de MINCETUR, a servicios GIS OpenStreetMap y a pasarelas SMS/Correo. |
| **RC07** | Cumplimiento Normativo y Legal | El diseño del tratamiento de datos y almacenamiento de expedientes debe ajustarse estrictamente a las disposiciones de la Ley N° 29733 (Ley de Protección de Datos Personales en el Perú). |
| **RC08** | Backend y persistencia institucional | Backend Node.js/NestJS, PostgreSQL primaria y réplica, Redis y RabbitMQ. |
| **RC09** | Control de versiones | Documentación, código y fuentes de diagramas versionados con Git en el repositorio del proyecto. |

## Parámetros institucionales y supuestos

| Aspecto | Tratamiento en el diseño |
|---|---|
| Políticas de riesgo y plazos | Se almacenan versionadas. No se atribuyen ponderaciones ni plazos sancionadores específicos a la institución sin una regla aprobada. |
| Retención de evidencias | La propuesta exige custodia duradera. El plazo concreto, acceso y disposición final se fijan con la política archivística y de datos personales. |
| Presupuesto | No existe monto aprobado en la propuesta; AC08 exige estimar y contrastar consumo antes del despliegue. |
| Dependencias externas | Acceso a API, cuotas, credenciales y capacidades de proveedores pendientes de validación técnica. |
| Documentación de diseño | Las rutas de clases y módulos especifican la implementación prevista; este entregable contiene documentación y diagramas. |
