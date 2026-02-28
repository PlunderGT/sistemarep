# 🔧 SistemaRep — Sistema de Reporte de Reparación y Entrega de Equipos

## Resumen del Sistema

**SistemaRep** es una aplicación web para gestionar el ciclo completo de reparación de equipos electrónicos y de cómputo: desde la recepción del equipo, registro de diagnóstico y reparación, hasta la generación de etiquetas de identificación y la entrega formal al cliente.

## Objetivo

Digitalizar y estandarizar el proceso de taller técnico, eliminando papeles y hojas de cálculo manuales, proporcionando trazabilidad completa de cada equipo y sus características.

## Tecnologías Propuestas

| Capa | Tecnología |
|------|-----------|
| Frontend | React + Tailwind CSS |
| Backend | Node.js + Express |
| Base de datos | PostgreSQL |
| Autenticación | JWT |
| Etiquetas | Librería PDF/QR |
| Gestión de proyecto | Trello (Free) |

## Estructura del Repositorio

```
sistemarep/
├── README.md                  ← Este archivo
├── docs/
│   ├── system-brief.md        ← Visión, alcance y diagrama de contexto
│   └── requirements.md        ← Backlog, historias y criterios de aceptación
```

## Documentación

- 📄 [System Brief](docs/system-brief.md) — Visión, alcance y diagrama de contexto
- 📋 [Requirements & Backlog](docs/requirements.md) — Historias de usuario y criterios de aceptación
- 🗂️ [Tablero Trello](https://trello.com/b/SISTEMAREP) — Backlog gestionado en Trello

## Módulos Principales

1. **Recepción** — Registro del cliente y equipo con sus características
2. **Diagnóstico** — Asignación a técnico y registro de falla
3. **Reparación** — Seguimiento de trabajos realizados y repuestos usados
4. **Etiquetado** — Generación de etiqueta con QR por equipo
5. **Entrega** — Confirmación de entrega y firma digital del cliente

## Estado del Proyecto

🟡 **En planificación** — Documentación y backlog en construcción.

---
*Proyecto académico — Documentación profesional de ingeniería de software*
