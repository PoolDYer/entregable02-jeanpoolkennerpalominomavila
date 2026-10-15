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