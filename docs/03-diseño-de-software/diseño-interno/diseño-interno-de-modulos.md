# Diseño interno de módulos

> **Proyecto:** SIFITUR Huamanga · **Backend:** Node.js + NestJS + TypeScript · **Módulo de referencia:** Inspecciones

## 1. Propósito

Definir las clases, contratos, reglas y colaboración interna para registrar una inspección sincronizada y emitir su acta PDF. El módulo responde a RF03, RF04 y RF09–RF11.

## 2. Estructura de carpetas

```text
src/modules/inspecciones/
├── dominio/
│   ├── inspeccion.ts
│   ├── evidencia.ts
│   ├── contratos.ts
│   └── errores.ts
├── aplicacion/
│   ├── registrar-inspeccion.caso-uso.ts
│   └── procesar-acta.caso-uso.ts
├── infraestructura/
│   ├── repositorio-memoria.ts
│   ├── almacen-memoria.ts
│   ├── generador-actas-pdf.ts
│   └── notificador-consola.ts
├── presentacion/
│   └── inspection.controller.ts
├── inspecciones.module.ts
└── index.ts
```

| Archivo / clase | Capa | Responsabilidad |
|---|---|---|
| Inspeccion | Dominio | Validar identidad de captura, operación, prestador, firma y evidencias. |
| Evidencia | Dominio | Validar origen cámara, GPS y hash del archivo original. |
| RepositorioInspecciones | Dominio | Registrar operación única, consulta y tareas de emisión/notificación. |
| AlmacenEvidencias | Dominio | Verificar objetos y conservar el acta emitida. |
| GeneradorActas | Dominio | Solicitar bytes del acta PDF sin conocer la biblioteca. |
| Notificador | Dominio | Solicitar entrega de la copia al correo del administrado. |
| RegistrarInspeccionCasoUso | Aplicación | Autorizar al inspector, verificar idempotencia y evidencias y registrar. |
| ProcesarActaCasoUso | Aplicación | Generar, almacenar y confirmar PDF; reintentar la notificación. |
| InspectionController | Presentación | Traducir HTTP, actor autenticado y resultado del caso de uso. |
| InspeccionesModule | Composición | Conectar contratos y adaptadores con inyección NestJS. |

## 3. Capas y regla de dependencia

![Paquetes del módulo Inspecciones](../../../img/paquetes-modulo-inspecciones.png)

Fuente editable: [paquetes-modulo-inspecciones.svg](../../../img/fuentes-svg/paquetes-modulo-inspecciones.svg).

Dominio no importa aplicación, presentación ni infraestructura. Aplicación solo utiliza el dominio. Infraestructura implementa sus contratos. Presentación invoca los casos de uso. La raíz de composición conecta las implementaciones concretas.

## 4. Diagrama de clases

![Clases del módulo Inspecciones](../../../img/clases-modulo-inspecciones.png)

Fuente editable: [clases-modulo-inspecciones.svg](../../../img/fuentes-svg/clases-modulo-inspecciones.svg).

## 5. Reglas de negocio

| ID | Regla | Entidad / caso de uso |
|---|---|---|
| RN-01 | Una inspección identifica operación, inspector, prestador y correo del administrado. | Inspeccion |
| RN-02 | La captura incluye al menos una evidencia de cámara con coordenadas válidas y hash SHA-256. | Evidencia |
| RN-03 | La firma manuscrita digitalizada debe estar vinculada a la inspección. | Inspeccion |
| RN-04 | Un reenvío idéntico conserva el mismo registro; reutilizar operación con otro contenido genera conflicto. | RegistrarInspeccionCasoUso y repositorio |
| RN-05 | La confirmación de recepción exige comprobar los objetos referenciados. | RegistrarInspeccionCasoUso |
| RN-06 | La emisión conserva PDF y hash antes de confirmar el estado emitida. | ProcesarActaCasoUso |
| RN-07 | Fallar al notificar no revierte un acta emitida; se mantiene la tarea pendiente para reintento. | ProcesarActaCasoUso |

## 6. Flujo: sincronizar inspección

![Secuencia de sincronización y emisión](../../../img/secuencia-registrar-inspeccion.png)

Fuente editable: [secuencia-registrar-inspeccion.svg](../../../img/fuentes-svg/secuencia-registrar-inspeccion.svg).

| Paso | Acción | Capa |
|---|---|---|
| 1 | Enviar operación estable y referencias de evidencias desde la app. | Presentación móvil |
| 2 | Obtener actor autorizado y validar entrada REST. | Presentación backend |
| 3 | Consultar duplicados y comparar huella del comando. | Aplicación |
| 4 | Validar entidades y comprobar hashes de objetos. | Aplicación / dominio |
| 5 | Confirmar inspección, auditoría y tarea outbox. | Contrato de repositorio |
| 6 | Responder 201 para nueva operación o 200 para repetición idéntica. | Presentación |
| 7 | Generar y almacenar PDF; confirmar hash y estado emitida. | Trabajador / aplicación |
| 8 | Enviar copia al correo y marcar la notificación entregada. | Adaptador externo |

## 7. Código de referencia

El ejemplo ejecutable está en [tecnologia/boilerplate-clean-arquitecture](../../../tecnologia/boilerplate-clean-arquitecture/README.md).

```ts
export interface AlmacenEvidencias {
  verificar(id: string, hash: string): Promise<boolean>;
  guardarActa(id: string, bytes: Uint8Array): Promise<void>;
}
```

```ts
const inspeccion = Inspeccion.crear(comando);
for (const evidencia of inspeccion.evidencias) {
  if (!await almacen.verificar(evidencia.id, evidencia.hash)) {
    throw new IntegridadError('Evidencia ausente o alterada');
  }
}
return repositorio.registrar(inspeccion, inspeccion.huella());
```

La implementación institucional del repositorio registra inspección, auditoría y outbox en una transacción PostgreSQL. La persistencia S3 y la entrega RabbitMQ se coordinan por estados y reintentos; no comparten una transacción ACID con PostgreSQL.
