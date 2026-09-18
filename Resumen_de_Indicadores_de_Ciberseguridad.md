# Entregable 1 — Tabla Resumen de Indicadores de Ciberseguridad

**Documento:** Catálogo normalizado de KPI de ciberseguridad
**Fuente de origen:** Tabla "21 KPIs for Cybersecurity Managers" (Excellog.Biz), traducida, normalizada y enriquecida
**Marcos de referencia:** NIST CSF 2.0 · ISO/IEC 27001:2022 (Anexo A) · CIS Controls v8.1
**Elaborado por:** Auditoría de Sistemas e Información / Consultoría en Gestión de Métricas
**Versión:** 1.0 | **Clasificación:** Uso interno

---

## 1. Nota metodológica

La tabla original presenta cinco columnas (número, nombre, qué mide, por qué importa y meta). Para efectos de gestión, auditoría y reporte a la alta dirección, el catálogo se reestructura incorporando:

- **Código correlativo** con prefijo `KPI-CIB-##` para trazabilidad documental.
- **Categoría/Dimensión** alineada a función NIST CSF 2.0 y control ISO/IEC 27001:2022, de modo que cada métrica sea auditable contra un control existente y no como indicador aislado.
- **Descripción objetiva** redactada en lenguaje de negocio, sin anglicismos innecesarios (se conserva entre paréntesis el término técnico de uso común en la industria cuando su traducción puede generar ambigüedad operativa).
- **Objetivo/Meta ideal**, respetando la naturaleza de umbral aprobado por la organización que plantea la fuente original, e incorporando un valor de referencia de industria como punto de partida para la calibración.

---

## 2. Tabla resumen de indicadores

| Código | Nombre del Indicador | Categoría / Dimensión | Descripción | Objetivo / Meta Ideal |
|---|---|---|---|---|
| **KPI-CIB-01** | Cierre de Riesgos Cibernéticos | Gobierno y Gestión de Riesgos — NIST **GV.RM / ID.RA** · ISO 27001 cl. 6.1.3, 8.3 | Proporción de hallazgos de riesgo alto o crítico que se resuelven dentro del plazo de tratamiento formalmente aprobado. Verifica que la decisión de tratamiento del riesgo se convierta en acción concluida y no solo en un acuerdo documental. | ≥ 95 % de cierre dentro del plazo aprobado; cero riesgos críticos vencidos sin aceptación formal. |
| **KPI-CIB-02** | Cobertura del Inventario de Activos | Identificación — NIST **ID.AM** · ISO 27001 A.5.9, A.5.10 · CIS 1 y 2 | Porcentaje de activos críticos correctamente registrados en el inventario, con atributos obligatorios completos y propietario (*owner*) asignado. | 100 % de activos críticos inventariados, vigentes y con propietario nominado. |
| **KPI-CIB-03** | Exposición de la Superficie de Ataque | Identificación / Gestión de Exposición — NIST **ID.RA** · ISO 27001 A.8.8, A.8.20 | Cantidad y proporción de activos expuestos a Internet que presentan fallas explotables o cuya propiedad se desconoce. Mide la exposición alcanzable por un atacante antes de que sea explotada. | Reducción sostenida respecto de una línea base verificada; cero activos expuestos sin propietario identificado. |
| **KPI-CIB-04** | Antigüedad de Vulnerabilidades | Protección / Gestión de Vulnerabilidades — NIST **ID.RA-08, PR.PS** · ISO 27001 A.8.8 · CIS 7 | Tiempo transcurrido desde la detección de vulnerabilidades críticas aún no resueltas en los activos dentro del alcance. Evidencia retrasos de remediación y acumulación de exposición. | Mediana de antigüedad ≤ el ANS (SLA) de remediación aprobado; ≤ 5 % de hallazgos críticos fuera de ANS. |
| **KPI-CIB-05** | Cumplimiento de Vulnerabilidades Explotadas Conocidas (KEV) | Protección / Gestión de Vulnerabilidades — NIST **ID.RA-08** · ISO 27001 A.8.8 · CIS 7 | Porcentaje de vulnerabilidades con evidencia pública de explotación activa (catálogo CISA KEV u homólogo) remediadas o mitigadas dentro del plazo autorizado. | 100 % remediadas o mitigadas compensatoriamente dentro del plazo aprobado. |
| **KPI-CIB-06** | Cumplimiento de Configuración Segura | Protección / Endurecimiento — NIST **PR.PS-01** · ISO 27001 A.8.9 · CIS 4 | Porcentaje de activos que cumplen las líneas base de configuración endurecida aprobadas por la organización. Reduce exposición evitable por parámetros inseguros de fábrica o por desvío de configuración. | ≥ 90 % de cumplimiento del *benchmark* aprobado en activos críticos; cero desviaciones críticas sin excepción autorizada. |
| **KPI-CIB-07** | Cobertura de MFA Resistente a Suplantación | Protección / Identidad y Accesos — NIST **PR.AA-03** · ISO 27001 A.5.17, A.8.5 · CIS 6 | Porcentaje de cuentas elegibles protegidas con factores de autenticación resistentes a suplantación (FIDO2/WebAuthn, certificados). Cierra rutas de robo de credenciales y secuestro de sesión. | 100 % de cuentas privilegiadas y de acceso remoto; 100 % de cuentas elegibles en el plazo del plan. |
| **KPI-CIB-08** | Cobertura de Revisión de Accesos Privilegiados | Protección / Identidad y Accesos — NIST **PR.AA-05** · ISO 27001 A.5.15, A.5.18, A.8.2 · CIS 5 y 6 | Porcentaje de cuentas privilegiadas revisadas y recertificadas conforme al calendario establecido. Limita el acceso elevado excesivo, obsoleto o no autorizado. | 100 % de cuentas privilegiadas recertificadas dentro del ciclo definido (trimestral o superior). |
| **KPI-CIB-09** | Cobertura de Detección en Puntos Finales | Detección — NIST **DE.CM-01** · ISO 27001 A.8.7, A.8.16 · CIS 10 y 13 | Porcentaje de equipos finales elegibles con agente EDR instalado, activo y reportando telemetría a la consola aprobada. | 100 % de equipos elegibles cubiertos y reportando en las últimas 72 horas. |
| **KPI-CIB-10** | Cobertura de Registros (Bitácoras) | Detección — NIST **DE.CM, DE.AE** · ISO 27001 A.8.15, A.8.16 · CIS 8 | Porcentaje de sistemas críticos que envían al SIEM las fuentes de registro requeridas por el catálogo de casos de uso. Habilita investigación, correlación y reconstrucción de eventos. | 100 % de sistemas críticos integrados con las fuentes obligatorias y con ingesta verificada. |
| **KPI-CIB-11** | Tiempo Medio de Detección (MTTD) | Detección — NIST **DE.AE-02** · ISO 27001 A.5.25, A.8.16 | Tiempo promedio entre el inicio de la actividad maliciosa y su detección efectiva. Mide la velocidad de detección en los ambientes monitoreados. | ≤ objetivo de detección aprobado (referencia de industria: ≤ 24 h para amenazas de alta severidad). |
| **KPI-CIB-12** | Tiempo Medio de Contención (MTTC) | Respuesta — NIST **RS.MA, RS.MI** · ISO 27001 A.5.26 · CIS 17 | Tiempo promedio entre la validación del incidente y su contención efectiva. Refleja la velocidad de respuesta antes de que el impacto se expanda. | ≤ objetivo de contención aprobado (referencia: ≤ 4 h para incidentes de severidad alta). |
| **KPI-CIB-13** | Tiempo Medio de Recuperación (MTTR) | Recuperación — NIST **RC.RP** · ISO 27001 A.5.29, A.5.30 | Tiempo promedio para restaurar los servicios afectados una vez contenido el incidente. Mide la recuperación operativa tras eventos de ciberseguridad. | ≤ objetivo de recuperación aprobado y consistente con el RTO comprometido por servicio. |
| **KPI-CIB-14** | Tasa de Recurrencia de Incidentes | Respuesta / Mejora Continua — NIST **RS.AN, ID.IM** · ISO 27001 cl. 10.1, 10.2 · A.5.27 | Porcentaje de incidentes repetidos vinculados a causas raíz previamente identificadas. Evidencia si las acciones correctivas realmente impiden la reincidencia. | Reducción trimestre contra trimestre; ≤ 10 % de incidentes recurrentes sobre el total. |
| **KPI-CIB-15** | Éxito de Pruebas de Restauración de Respaldos | Recuperación / Continuidad — NIST **RC.RP-03, PR.DS-11** · ISO 27001 A.8.13, A.5.29 · CIS 11 | Porcentaje de pruebas de restauración exitosas sobre sistemas y datos críticos. Valida que los respaldos efectivamente soportan una recuperación real. | 100 % de pruebas críticas aprobadas, con validación firmada por el dueño del dato. |
| **KPI-CIB-16** | Logro de Objetivos de Recuperación (RTO/RPO) | Recuperación / Continuidad — NIST **RC.RP-02** · ISO 27001 A.5.29, A.5.30 | Porcentaje de recuperaciones (reales o probadas) que cumplen los objetivos aprobados de tiempo (RTO) y de punto de recuperación (RPO). | 100 % de recuperaciones dentro de RTO y RPO comprometidos para servicios críticos. |
| **KPI-CIB-17** | Aseguramiento de Proveedores | Gobierno / Terceros — NIST **GV.SC** · ISO 27001 A.5.19–A.5.22 · CIS 15 | Porcentaje de proveedores críticos con debida diligencia de ciberseguridad vigente y concluida. Reduce la exposición no gestionada de la cadena de suministro. | 100 % de proveedores críticos con evaluación vigente (antigüedad ≤ 12 meses) y cláusulas contractuales de seguridad. |
| **KPI-CIB-18** | Aseguramiento de la Cadena de Suministro de Software | Gobierno / Desarrollo Seguro — NIST **GV.SC-06, ID.RA-09** · ISO 27001 A.8.28, A.8.30, A.5.21 | Porcentaje de software crítico con procedencia verificada (firma/atestación) e inventario de componentes (SBOM) actualizado. Reduce el riesgo de dependencias y de origen no verificado. | 100 % del software crítico con SBOM vigente y procedencia verificada antes de su despliegue productivo. |
| **KPI-CIB-19** | Tasa de Configuraciones Deficientes en Nube | Protección / Seguridad en Nube — NIST **PR.PS-01, PR.IR-01** · ISO 27001 A.5.23, A.8.9 · CIS 4 | Porcentaje de recursos de nube que incumplen las líneas base de configuración aprobadas. Revela exposición evitable en entornos altamente dinámicos. | ≤ tolerancia aprobada (referencia: ≤ 2 % de recursos no conformes; cero hallazgos críticos > 72 h). |
| **KPI-CIB-20** | Tasa de Reporte de Phishing | Protección / Cultura y Concientización — NIST **PR.AT-01** · ISO 27001 A.6.3 · CIS 14 | Porcentaje de correos de simulación de phishing que los colaboradores reportan correctamente por el canal oficial. Mejora la detección temprana de ingeniería social. | ≥ meta de reporte aprobada (referencia: ≥ 40 % de reporte y razón reporte/clic ≥ 3:1). |
| **KPI-CIB-21** | Exposición a IA No Autorizada (*Shadow AI*) | Gobierno / Protección de Datos — NIST **GV.RM, PR.DS** · ISO 27001 A.5.34, A.8.12 · ISO/IEC 42001 | Eventos de datos sensibles dirigidos a herramientas o servicios de inteligencia artificial no aprobados. Da seguimiento a la fuga de información por adopción no gobernada de IA. | Reducción sostenida respecto de una línea base verificada; cero eventos con datos clasificados como confidenciales o regulados. |

---

## 3. Puntos ambiguos, incompletos o sujetos a interpretación

Conforme a la instrucción de señalar explícitamente los elementos poco claros del material de origen, se identifican los siguientes. **Todos requieren definición formal en la ficha técnica del indicador antes de su publicación**, ya que sin ellos la métrica no es auditable ni comparable entre periodos.

| # | Elemento ambiguo en la fuente | Riesgo que introduce | Interpretación recomendada (mejores prácticas) |
|---|---|---|---|
| 1 | La columna de metas emplea de forma recurrente la expresión "aprobado" sin valores numéricos (p. ej. "≤ ANS de remediación aprobado"). | La métrica queda sin umbral objetivo; permite ajuste retroactivo del criterio para simular cumplimiento. | Fijar umbrales cuantitativos en una **matriz de umbrales aprobada por el Comité de Seguridad**, con control de versiones y fecha de vigencia. Un umbral no versionado es un hallazgo de auditoría. |
| 2 | **KPI-CIB-01**: mezcla "hallazgos de riesgo alto resueltos" con "decisiones de riesgo aceptadas". Son dos poblaciones distintas. | Riesgos aceptados podrían contarse como cerrados, inflando el indicador. | Medir **cierre de planes de tratamiento en plazo**, excluyendo del numerador los riesgos aceptados y reportándolos como indicador separado de riesgo residual aceptado. |
| 3 | **KPI-CIB-03**: no precisa si la unidad de medida es conteo absoluto o porcentaje, ni qué constituye "falla explotable". | Falta de comparabilidad entre periodos; criterio subjetivo de explotabilidad. | Reportar **doble unidad**: conteo absoluto de activos expuestos en riesgo y porcentaje sobre el total expuesto. Definir "explotable" como vulnerabilidad con exploit público conocido o EPSS ≥ umbral definido. |
| 4 | **KPI-CIB-04**: no especifica si se usa media, mediana o máximo. | La media se distorsiona con valores atípicos y oculta hallazgos muy antiguos. | Usar **mediana y percentil 90**, más el conteo de hallazgos fuera de ANS. Evitar la media aritmética simple. |
| 5 | **KPI-CIB-11 (MTTD)**: "desde la actividad maliciosa" supone conocer el instante inicial del compromiso, dato que solo se obtiene tras análisis forense. | Indicador irreproducible o estimado de forma inconsistente entre analistas. | Definir el origen del cronómetro como **primera evidencia verificable en telemetría** documentada en el informe de causa raíz; declarar el método en la ficha técnica. |
| 6 | **KPI-CIB-11 a 13**: no delimitan el alcance por severidad. | Incidentes menores de alto volumen diluyen la lectura de incidentes graves. | Segmentar por **severidad (crítica/alta/media)** y reportar el resultado por estrato, no consolidado. |
| 7 | **KPI-CIB-14**: no define el denominador ni la ventana temporal para considerar "recurrente". | Manipulable ampliando el denominador o acortando la ventana. | Denominador: total de incidentes validados del periodo. Ventana: **12 meses** desde el cierre de la causa raíz original. |
| 8 | **KPI-CIB-17**: no define la vigencia de la debida diligencia ni el criterio de "proveedor crítico". | Proveedores evaluados hace años podrían contarse como vigentes. | Vigencia máxima de **12 meses** (o 24 con certificación vigente ISO 27001/SOC 2 Tipo II) y criterio de criticidad basado en acceso a datos, conectividad y dependencia operativa. |
| 9 | **KPI-CIB-21**: "eventos de datos sensibles" no precisa si la unidad es evento, usuario o volumen de información. | Un solo usuario con alto volumen puede distorsionar la tendencia. | Reportar **tres dimensiones**: número de eventos, usuarios únicos involucrados y volumen o número de registros sensibles expuestos. |
| 10 | La fuente no incluye responsable (*owner*), periodicidad, ni fórmula de ninguna métrica. | Sin propietario y sin fórmula, la métrica no es gobernable ni auditable. | Completar la **ficha técnica por indicador** (propietario, fórmula, fuentes, frecuencia, umbral, escalamiento). Ver Entregable 2. |

> **Nota sobre legibilidad de la imagen:** el contenido de la tabla original es legible en su totalidad; no se identificaron celdas ilegibles. Las ambigüedades listadas son de **definición metodológica**, no de calidad de imagen.

---

## 4. Lectura ejecutiva: impacto de negocio y nivel de riesgo

Priorización sugerida para presentación a la Dirección General y al Comité de Riesgos. El nivel de riesgo refleja la exposición que representa **no medir o medir deficientemente** cada indicador.

| Prioridad | Indicadores | Impacto de negocio si el indicador se encuentra fuera de meta | Nivel de riesgo |
|---|---|---|---|
| **1 — Atención inmediata** | KPI-05 (KEV), KPI-07 (MFA), KPI-09 (EDR), KPI-10 (Registros) | Rutas de ataque activamente explotadas y ausencia de visibilidad. Materializan compromiso con interrupción operativa, extorsión y pérdida de datos. Debilitan la defendibilidad ante el regulador y ante la aseguradora cibernética. | **Crítico** |
| **2 — Corto plazo (≤ 90 días)** | KPI-02 (Inventario), KPI-03 (Superficie), KPI-04 (Antigüedad), KPI-15 (Respaldos), KPI-16 (RTO/RPO) | Sin inventario confiable ninguna otra métrica es válida; sin restauración probada la continuidad del negocio es una hipótesis no verificada. Impacto directo en disponibilidad de servicios críticos. | **Alto** |
| **3 — Mediano plazo (≤ 180 días)** | KPI-01, KPI-06, KPI-08, KPI-11, KPI-12, KPI-13, KPI-17, KPI-19 | Deterioro de la eficacia del programa de seguridad, exposición de terceros y acumulación de deuda de configuración. Impacto financiero por sanciones contractuales y costo de respuesta. | **Alto / Medio** |
| **4 — Consolidación** | KPI-14, KPI-18, KPI-20, KPI-21 | Madurez del programa, cultura organizacional y riesgos emergentes. Su ausencia no genera compromiso inmediato pero impide demostrar mejora continua (ISO 27001 cl. 9 y 10). | **Medio** |

### Recomendaciones accionables

1. **Formalizar la matriz de umbrales** (30 días). Cada meta "aprobada" debe traducirse en un valor numérico autorizado por el Comité de Seguridad, con control de versiones. Sin esto, 15 de los 21 indicadores carecen de criterio de cumplimiento verificable.
2. **Nombrar propietario y custodio de datos por indicador** (30 días). El propietario responde por el resultado; el custodio, por la extracción y la integridad del dato. Deben ser roles distintos para preservar segregación de funciones.
3. **Automatizar la extracción de las 10 métricas de mayor volumen** (90 días). Todo indicador calculado manualmente en hoja de cálculo es un riesgo de integridad; priorizar KPI-02, 04, 05, 06, 09, 10, 19.
4. **Corregir el alcance antes de publicar la línea base** (60 días). KPI-02 debe alcanzar ≥ 95 % de cobertura verificada antes de publicar cualquier indicador cuyo denominador dependa del inventario (KPI-04, 06, 09, 10).
5. **Establecer tablero ejecutivo trimestral con tendencia y no solo valor puntual** (60 días). La alta dirección requiere dirección del cambio y distancia al umbral, no fotografías mensuales aisladas.
6. **Incorporar los indicadores al programa de auditoría interna** (ISO 27001 cl. 9.2), con verificación independiente de al menos cinco métricas por ciclo mediante recálculo, conforme al Entregable 3.
