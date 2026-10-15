# Arquitectura inicial del sistema

## Diagrama de arquitectura

```mermaid
flowchart TD
%% =========================
%% ACTORES
%% =========================
subgraph ACTORES["ACTORES"]
    Inspector["Inspector de Campo\n(Autenticado)"]
    Coordinador["Coordinador DIRCETUR / MPH\n(Autenticado)"]
    Prestador["Prestador Turístico\n(Autenticado)"]
    Admin["Administrador de TI\n(Autenticado)"]
    Turista["Turista / Ciudadano\n(Público No Autenticado)"]
end

%% =========================
%% PRESENTACIÓN
%% =========================
subgraph PRESENTACION["PRESENTACIÓN (Canales Web y Móvil)"]
    AppMovil["App Móvil Fiscalización\n(Flutter / SQLite Offline Cifrado)"]
    ConsolaAdmin["Consola Web Administrativa\n(React SPA / TypeScript)"]
    PortalPrestador["Portal Web del Administrado\n(React SPA / Descargos)"]
    PortalPublico["Portal Web Ciudadano / Visor QR\n(Next.js SSR / Consulta Abierta)"]
end

%% =========================
%% GATEWAY Y SEGURIDAD
%% =========================
subgraph SEGURIDAD["GATEWAY Y SEGURIDAD PERIMETRAL"]
    Gateway["API Gateway & Reverse Proxy\n(NGINX / SSL TLS 1.3 / Rate Limiting)"]
    AuthGuard["Seguridad Perimetral y Control de Roles\n(Auth JWT & RBAC Guard / WAF)"]
end

%% =========================
%% LÓGICA DE NEGOCIO
%% =========================
subgraph NEGOCIO["LÓGICA DE NEGOCIO (Backend API Application)"]
    AuthService["Gestión de Usuarios, Roles (RBAC) y Credenciales"]
    PadronService["Gestión de Padrón, Entidades y Vigencias"]
    OperativosService["Planificación de Operativos y Rutas"]
    FiscalizacionService["Inspección Técnica y Actas Digitales"]
    SubsanacionesService["Control de Subsanaciones y Plazos"]
    RiesgoService["Motor de Cálculo Automatizado de Riesgo"]
    AuditoriaService["Servicio de Auditoría y Trazabilidad"]
end

%% =========================
%% DATOS Y PERSISTENCIA
%% =========================
subgraph DATOS["DATOS Y PERSISTENCIA"]
    BDPrimaria["Base de Datos Transaccional\n(PostgreSQL Primaria - Escrituras)"]
    BDReplica["Réplica de Base de Datos\n(PostgreSQL - Solo Lectura / Reportes)"]
    CacheRedis["Caché en Memoria\n(Redis - QR, Padrón y Sesiones)"]
    StorageS3["Almacenamiento de Objetos\n(S3 - Fotos con GPS y PDFs Firmados)"]
    ColaTareas["Cola de Tareas Asíncronas\n(RabbitMQ - Sellado y Notificaciones)"]
end

%% =========================
%% SISTEMAS EXTERNOS
%% =========================
subgraph EXTERNOS["SISTEMAS EXTERNOS"]
    SUNAT["Plataforma RUC SUNAT"]
    MINCETUR["Directorio Nacional MINCETUR"]
    OSM["Servidor OpenStreetMap (GIS)"]
    Notificaciones["Pasarela SMS / Correo Oficial"]
end

%% =========================
%% FLUJO PRINCIPAL
%% =========================
Inspector --> AppMovil
Coordinador --> ConsolaAdmin
Prestador --> PortalPrestador
Admin --> ConsolaAdmin
Turista --> PortalPublico

AppMovil -->|"HTTPS / REST (JWT)"| Gateway
ConsolaAdmin -->|"HTTPS / REST (JWT)"| Gateway
PortalPrestador -->|"HTTPS / REST (JWT)"| Gateway
PortalPublico -->|"HTTPS / REST (Público)"| Gateway

Gateway --> AuthGuard
AuthGuard --> NEGOCIO

NEGOCIO --> DATOS
NEGOCIO -->|"integraciones seguras"| EXTERNOS

%% =========================
%% DISTRIBUCIÓN HORIZONTAL
%% =========================
Inspector ~~~ Coordinador
Coordinador ~~~ Prestador
Prestador ~~~ Admin
Admin ~~~ Turista

AppMovil ~~~ ConsolaAdmin
ConsolaAdmin ~~~ PortalPrestador
PortalPrestador ~~~ PortalPublico

AuthService ~~~ PadronService
PadronService ~~~ OperativosService
OperativosService ~~~ FiscalizacionService
FiscalizacionService ~~~ SubsanacionesService
SubsanacionesService ~~~ RiesgoService
RiesgoService ~~~ AuditoriaService

BDPrimaria ~~~ BDReplica
BDReplica ~~~ CacheRedis
CacheRedis ~~~ StorageS3
StorageS3 ~~~ ColaTareas

SUNAT ~~~ MINCETUR
MINCETUR ~~~ OSM
OSM ~~~ Notificaciones

%% =========================
%% ESTILOS
%% =========================
style ACTORES fill:#222, stroke:#fff, stroke-width: 2px,color:#fff
style PRESENTACION fill:#222, stroke:#fff, stroke-width: 2px,color:#fff
style SEGURIDAD fill:#222, stroke:#fff, stroke-width: 2px,color:#fff
style NEGOCIO fill:#222, stroke:#fff, stroke-width: 2px,color:#fff
style DATOS fill:#222, stroke:#fff, stroke-width: 2px,color:#fff
style EXTERNOS fill:#222, stroke:#fff, stroke-width: 2px,color:#fff

style Inspector fill:#222, stroke:#fff,color:#fff
style Coordinador fill:#222, stroke:#fff,color:#fff
style Prestador fill:#222, stroke:#fff,color:#fff
style Admin fill:#222, stroke:#fff,color:#fff
style Turista fill:#222, stroke:#fff,color:#fff

style AppMovil fill:#222, stroke:#fff,color:#fff
style ConsolaAdmin fill:#222, stroke:#fff,color:#fff
style PortalPrestador fill:#222, stroke:#fff,color:#fff
style PortalPublico fill:#222, stroke:#fff,color:#fff

style Gateway fill:#222, stroke:#fff,color:#fff
style AuthGuard fill:#222, stroke:#fff,color:#fff

style AuthService fill:#222, stroke:#fff,color:#fff
style PadronService fill:#222, stroke:#fff,color:#fff
style OperativosService fill:#222, stroke:#fff,color:#fff
style FiscalizacionService fill:#222, stroke:#fff,color:#fff
style SubsanacionesService fill:#222, stroke:#fff,color:#fff
style RiesgoService fill:#222, stroke:#fff,color:#fff
style AuditoriaService fill:#222, stroke:#fff,color:#fff

style BDPrimaria fill:#222, stroke:#fff,color:#fff
style BDReplica fill:#222, stroke:#fff,color:#fff
style CacheRedis fill:#222, stroke:#fff,color:#fff
style StorageS3 fill:#222, stroke:#fff,color:#fff
style ColaTareas fill:#222, stroke:#fff,color:#fff

style SUNAT fill:#222, stroke:#fff,color:#fff
style MINCETUR fill:#222, stroke:#fff,color:#fff
style OSM fill:#222, stroke:#fff,color:#fff
style Notificaciones fill:#222, stroke:#fff,color:#fff
```
