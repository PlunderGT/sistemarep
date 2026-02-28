# Requirements & Product Backlog — SistemaRep

## 1. Enlace al Tablero Trello

🗂️ **Tablero oficial del proyecto:** 
https://trello.com/invite/b/69a34b8346e8b7122a205171/ATTIef6e87fa0f57e08a98ddd892ba1ff049160174B1/modernos-y-tecnologicos

> **Nota:** Reemplazar el enlace con la URL real del tablero Trello una vez creado. El tablero debe contener las columnas: `Backlog`, `MVP (Must)`, `Should Have`, `Could Have`, `En Progreso`, `Hecho`.

---

## 2. Backlog de Historias de Usuario

### Priorización MoSCoW

| # | Historia de Usuario | Prioridad | Módulo |
|---|---------------------|-----------|--------|
| US-01 | Como recepcionista, quiero registrar un cliente con sus datos de contacto, para tener su ficha disponible al ingresar equipos | **Must** | Clientes |
| US-02 | Como recepcionista, quiero registrar un equipo con marca, modelo, número de serie y características, para documentar el activo recibido | **Must** | Recepción |
| US-03 | Como sistema, quiero generar automáticamente una etiqueta con QR al registrar un equipo, para que pueda imprimirse e identificar físicamente el equipo | **Must** | Etiquetado |
| US-04 | Como técnico, quiero ver la lista de órdenes asignadas a mí con su estado, para organizar mi trabajo diario | **Must** | Panel Técnico |
| US-05 | Como técnico, quiero actualizar el estado de una orden de trabajo (En diagnóstico / En reparación / Listo), para reflejar el avance real de la reparación | **Must** | Reparación |
| US-06 | Como sistema, quiero enviar un correo electrónico al cliente cuando su equipo esté listo, para avisarle sin que el taller deba llamar manualmente | **Should** | Notificaciones |
| US-07 | Como recepcionista, quiero registrar la entrega del equipo con fecha y firma digital del cliente, para tener respaldo legal de que el equipo fue devuelto | **Should** | Entrega |
| US-08 | Como administrador, quiero consultar un reporte de órdenes por estado y por técnico, para supervisar la productividad del taller | **Could** | Reportes |

---

## 3. Criterios de Aceptación — Historias Must

---

### US-02 — Registro de Equipo con Características

**Historia completa:**  
_"Como recepcionista, quiero registrar un equipo con marca, modelo, número de serie y características, para documentar el activo recibido."_

#### Criterio 1 — Registro exitoso con todos los campos obligatorios

```
Given que el recepcionista está en el formulario de nuevo equipo
  And ha seleccionado un cliente existente en el sistema
When completa los campos: marca, modelo, número de serie, tipo de equipo, falla reportada
  And hace clic en "Guardar equipo"
Then el sistema crea la orden de trabajo con estado "Recibido"
  And muestra un mensaje de confirmación con el número de orden generado
  And el equipo aparece en la lista de equipos pendientes
```

#### Criterio 2 — Validación de campos obligatorios vacíos

```
Given que el recepcionista está en el formulario de nuevo equipo
When intenta guardar sin completar el campo "Número de serie"
Then el sistema muestra un mensaje de error: "El número de serie es obligatorio"
  And no crea la orden de trabajo
  And el formulario permanece abierto con los datos ingresados
```

#### Criterio 3 — Número de serie duplicado

```
Given que existe un equipo registrado con número de serie "SN-2024-001"
When el recepcionista intenta registrar otro equipo con el mismo número de serie
  And hace clic en "Guardar equipo"
Then el sistema muestra una advertencia: "Ya existe un equipo con este número de serie"
  And permite al recepcionista confirmar si desea continuar o corregir el dato
```

#### Criterio 4 — Registro de características adicionales opcionales

```
Given que el recepcionista está completando el formulario
When agrega notas en el campo "Características adicionales" (ej: "Sin cargador, pantalla rayada")
  And guarda el equipo
Then el sistema almacena las características adicionales
  And estas se muestran en la etiqueta generada y en el detalle de la orden
```

---

### US-03 — Generación Automática de Etiqueta con QR

**Historia completa:**  
_"Como sistema, quiero generar automáticamente una etiqueta con QR al registrar un equipo, para que pueda imprimirse e identificar físicamente el equipo."_

#### Criterio 1 — Etiqueta generada al crear la orden

```
Given que el recepcionista ha completado el formulario de registro del equipo
When el sistema guarda exitosamente la orden de trabajo
Then el sistema genera automáticamente una etiqueta en formato PDF
  And la etiqueta contiene: número de orden, cliente, marca, modelo, número de serie y fecha de recepción
  And incluye un código QR que enlaza al detalle de la orden
  And muestra un botón "Imprimir etiqueta" visible en pantalla
```

#### Criterio 2 — Contenido mínimo requerido en la etiqueta

```
Given que se ha generado la etiqueta de un equipo registrado
When el recepcionista la visualiza en pantalla o la imprime
Then la etiqueta muestra claramente: 
  - Nombre del taller (logo si está configurado)
  - Número de orden (ej: ORD-2026-0042)
  - Nombre del cliente
  - Marca y modelo del equipo
  - Número de serie
  - Fecha de recepción
  - Código QR funcional
```

#### Criterio 3 — Regeneración de etiqueta desde la orden

```
Given que existe una orden de trabajo previamente creada
When el recepcionista accede al detalle de la orden
  And hace clic en "Regenerar etiqueta"
Then el sistema genera una nueva versión del PDF con los datos actuales
  And la etiqueta regenerada reemplaza a la anterior en el sistema
```

#### Criterio 4 — Etiqueta imprimible en tamaño estándar

```
Given que se ha generado la etiqueta en PDF
When el recepcionista abre el archivo para imprimir
Then el PDF está formateado para papel A6 (105 × 148 mm)
  And el código QR ocupa al menos 2 × 2 cm para garantizar su lectura
  And la fuente mínima utilizada es de 10pt para legibilidad
```

---

### US-05 — Actualización de Estado de Orden de Trabajo *(Must adicional)*

**Historia completa:**  
_"Como técnico, quiero actualizar el estado de una orden de trabajo, para reflejar el avance real de la reparación."_

#### Criterio 1 — Cambio de estado válido

```
Given que el técnico tiene asignada la orden ORD-2026-0042 con estado "Recibido"
When selecciona el nuevo estado "En diagnóstico" desde el panel de la orden
  And confirma el cambio
Then el sistema actualiza el estado de la orden a "En diagnóstico"
  And registra la fecha y hora del cambio con el nombre del técnico
  And el historial de cambios de estado es visible en el detalle de la orden
```

#### Criterio 2 — Transición de estado a "Listo" dispara notificación

```
Given que el técnico ha completado la reparación de la orden ORD-2026-0042
When cambia el estado a "Listo para entrega"
Then el sistema actualiza el estado en la base de datos
  And dispara automáticamente el envío de email de notificación al cliente
  And muestra confirmación al técnico: "Estado actualizado. Notificación enviada al cliente."
```

---

## 4. MVP Rationale

Las historias clasificadas como **Must** (US-01 a US-05) conforman el núcleo del MVP porque representan el flujo de valor mínimo e indispensable para que un taller técnico pueda operar digitalmente: recibir un equipo, documentarlo con sus características, identificarlo físicamente mediante una etiqueta con QR, asignarlo a un técnico y registrar el avance de la reparación. Sin estos cinco elementos, el sistema no aporta valor real sobre un proceso manual. Las historias **Should** (US-06 y US-07) —notificación por email y registro formal de entrega— se incluyen en la segunda iteración porque, aunque mejoran significativamente la experiencia, el taller puede operar sin ellas en una fase piloto. La historia **Could** (US-08) de reportes administrativos se posterga porque requiere datos acumulados para ser útil y no impacta la operación diaria del taller en sus primeras semanas de uso.

---

## 5. Definición de Hecho (Definition of Done)

Una historia de usuario se considera **Done** cuando:
- El código está integrado en la rama `main` sin errores de build
- Todos los criterios de aceptación Given/When/Then han sido verificados manualmente
- La funcionalidad ha sido revisada por al menos un par (code review)
- El caso de uso está documentado en el README del módulo correspondiente
- No existen bugs críticos abiertos relacionados con la historia

---

*Documento versionado — v1.0 — Febrero 2026*
