# 21 KPIs Cybersecurity Managers — Tablero de Indicadores de Ciberseguridad

Tablero de gestión con **21 indicadores clave (KPI/KRI)** de ciberseguridad, mapeados a tres marcos de referencia:

- **NIST CSF 2.0** (Cybersecurity Framework)
- **ISO/IEC 27001:2022**
- **CIS Controls v8.1**

🔗 **Ver tablero publicado:** https://ronaldvp.github.io/21-KPIs-Cybersecurity-Managers/

---

## Propósito

Consolidar en un solo tablero visual el estado de los controles de seguridad de la organización, permitiendo a niveles ejecutivos (C-Level) y equipos de GRC dar seguimiento periódico a la postura de riesgo, con evidencia trazable por indicador (fuente de datos, fecha de captura, resultado de escáner o herramienta de referencia).

## Alcance de los indicadores

- Seguimiento de exposición a vulnerabilidades (referencia CISA KEV)
- Validación de configuración segura (CIS-CAT, SCM, Defender for Cloud)
- Gestión de riesgo de terceros (evidencia ISO 27001 / SOC 2 Tipo II)
- Indicadores de madurez de control por dominio

## Mapeo de marcos de referencia

| Marco | Uso en el tablero |
|---|---|
| NIST CSF 2.0 | Estructura de funciones (Identificar, Proteger, Detectar, Responder, Recuperar) como eje de clasificación de indicadores |
| ISO/IEC 27001:2022 | Referencia de controles Anexo A para evidencia de cumplimiento |
| CIS Controls v8.1 | Priorización técnica y validación de configuración (CIS-CAT) |

## Estructura del proyecto

```
21-KPIs-Cybersecurity-Managers/
├── index.html    # Tablero — documento autocontenido (HTML/CSS/JS embebido)
└── README.md     # Este archivo
```

## Uso

El tablero es un documento HTML autocontenido: se abre directamente en cualquier navegador, sin dependencias de backend ni instalación. Está publicado vía **GitHub Pages** para consulta y distribución a interesados.

Incluye modo claro/oscuro automático según preferencia del sistema.

## Público objetivo

- CISOs / Gerentes de Seguridad de la Información
- Equipos de GRC (Governance, Risk & Compliance)
- MSSPs que requieren un entregable de reporting ejecutivo estandarizado

## Nota de confidencialidad

⚠️ Este repositorio es **público**. La versión publicada debe contener únicamente **datos de ejemplo o anonimizados**. Si se utiliza esta plantilla con datos reales de un cliente u organización, el repositorio debe mantenerse **privado**, conforme al control de confidencialidad **ISO/IEC 27001:2022 – A.5.34** y a los acuerdos de confidencialidad (NDA) aplicables.

## Licencia / Uso

Este tablero es una plantilla de referencia. Ajusta los indicadores, umbrales y fuentes de datos según el alcance y la madurez del programa de seguridad de cada organización.

---
**Autor:** Ing. Ronald Vera Paz
