# Entregable 2 — Guía Paso a Paso para el Cálculo y Seguimiento de Indicadores

**Documento base de capacitación** dirigido a equipos de Seguridad de la Información, Operaciones de TI, SOC, Continuidad y Gestión de Terceros.

**Objetivo del documento:** que cualquier integrante del equipo pueda calcular, sustentar y explicar cada indicador de forma idéntica, reproducible y defendible ante auditoría interna, auditoría externa y regulador.

**Versión:** 1.0 | **Clasificación:** Uso interno

---

## Parte I — Reglas generales de cálculo (aplican a los 21 indicadores)

Antes de calcular cualquier métrica, el equipo debe observar las siguientes siete reglas. La mayoría de los hallazgos de auditoría sobre indicadores no provienen de la fórmula, sino del incumplimiento de estas reglas.

1. **Definir el alcance antes que la fórmula.** Toda métrica requiere una población claramente delimitada (activos en alcance, cuentas elegibles, sistemas críticos). El alcance debe estar documentado y aprobado; no puede cambiar entre periodos sin nota explicativa.
2. **El denominador es la parte auditable.** Un numerador correcto sobre un denominador incompleto produce un indicador optimista y falso. El denominador debe provenir de una **fuente independiente** de la herramienta que produce el numerador (ejemplo: la cobertura de EDR se mide contra el inventario, no contra la consola del propio EDR).
3. **Fecha de corte única.** Todos los indicadores del mismo periodo se extraen con la misma fecha y hora de corte, declarada en el reporte (ejemplo: último día natural del mes, 23:59 hora local).
4. **Conservar la evidencia de extracción.** Cada cálculo se respalda con la consulta ejecutada, el archivo de datos crudos y la captura de la herramienta, almacenados en el repositorio de evidencia con retención mínima de 24 meses.
5. **Excepciones documentadas, no silenciosas.** Todo elemento excluido del cálculo requiere excepción formal con dueño, justificación, control compensatorio y vigencia. Las excepciones se reportan como cifra visible junto al indicador.
6. **Trazabilidad a cuatro ojos.** Quien extrae el dato no es quien aprueba el reporte. El propietario del indicador revisa y firma antes de la publicación.
7. **Reportar valor, tendencia y distancia al umbral.** Un número aislado no es información de gestión. El formato obligatorio es: *valor del periodo · variación contra periodo anterior · umbral aprobado · semáforo · acción*.

**Notación empleada en esta guía:**
`|X|` = cardinalidad (número de elementos) del conjunto X · `t` = marca de tiempo (timestamp) · ANS = Acuerdo de Nivel de Servicio (SLA)

---

## Parte II — Fichas de cálculo por indicador

---

### KPI-CIB-01 · Cierre de Riesgos Cibernéticos

**Fórmula de cálculo**

```
% Cierre en plazo = ( |Hallazgos de riesgo alto/crítico cerrados dentro del plazo aprobado|
                      ÷ |Hallazgos de riesgo alto/crítico cuyo plazo venció en el periodo| ) × 100
```

**Explicación conceptual paso a paso:**
1. Filtrar el registro de riesgos por severidad alta y crítica.
2. Seleccionar únicamente aquellos cuyo **plazo comprometido vence dentro del periodo** analizado (no todos los abiertos; de lo contrario se diluye el indicador).
3. Del subconjunto anterior, contar cuántos tienen estado "cerrado" con evidencia de validación y fecha de cierre ≤ fecha comprometida.
4. Dividir y multiplicar por 100.
5. **Excluir del numerador y del denominador** los riesgos con aceptación formal vigente; reportarlos por separado como riesgo residual aceptado.

**Fuentes de datos necesarias**
- Plataforma GRC o registro de riesgos (matriz de riesgos con fechas comprometidas).
- Plan de tratamiento de riesgos aprobado (ISO 27001 cl. 6.1.3 y 8.3).
- Actas del Comité de Seguridad (aprobaciones de prórroga y de aceptación de riesgo).
- Sistema de tickets, como evidencia de la acción técnica ejecutada.

**Frecuencia de medición:** mensual para seguimiento operativo; trimestral para reporte al Comité.

**Ejemplo práctico**
En el primer trimestre vencen 42 planes de tratamiento de riesgo alto o crítico. Al corte, 34 están cerrados con evidencia validada, 5 siguen abiertos y vencidos, y 3 recibieron prórroga aprobada en acta del 12 de febrero.

> Cálculo: los 3 con prórroga formal no se computan como incumplimiento del periodo; se recalendarizan.
> `34 ÷ (42 − 3) = 34 ÷ 39 = 87.2 %`

*Interpretación:* contra una meta de ≥ 95 %, existe una brecha de 7.8 puntos y 5 riesgos altos vencidos sin tratamiento ni aceptación formal. **Acción:** escalar los 5 casos al Comité para decisión explícita —remediar con fecha firme o aceptar formalmente el riesgo residual—. Un riesgo alto vencido sin decisión documentada es un hallazgo de auditoría, no un retraso operativo.

---

### KPI-CIB-02 · Cobertura del Inventario de Activos

**Fórmula de cálculo**

```
% Cobertura = ( |Activos críticos en inventario con atributos obligatorios completos y propietario asignado|
                ÷ |Activos críticos identificados por descubrimiento independiente| ) × 100
```

**Explicación conceptual paso a paso:**
1. Construir el denominador mediante **descubrimiento activo**: barrido de red, agentes, consultas a directorio y a los planos de control de nube. Nunca usar la CMDB como denominador de sí misma.
2. Deduplicar por identificador estable (número de serie, UUID de máquina virtual, ARN o *resource ID* de nube).
3. Del total descubierto, contar los que existen en el inventario oficial **con los campos obligatorios completos**: propietario, clasificación de la información, criticidad, ambiente y responsable técnico.
4. Calcular el porcentaje y, además, reportar la **brecha de reconciliación** (activos descubiertos que no están en la CMDB y registros de CMDB que ya no existen).

**Fuentes de datos necesarias**
- CMDB o inventario oficial de activos.
- Herramienta de descubrimiento de red y escáner de vulnerabilidades.
- Consola EDR y gestor de dispositivos (Intune, SCCM, JAMF).
- Directorio corporativo (Active Directory / Entra ID) e inventarios de nube (AWS Config, Azure Resource Graph, GCP Asset Inventory).
- Registro de altas y bajas de recursos humanos y de compras, para validar propietarios.

**Frecuencia de medición:** mensual, con reconciliación formal trimestral.

**Ejemplo práctico**
El descubrimiento consolidado detecta 1 480 activos únicos en ambientes productivos. La CMDB contiene 1 402 registros, de los cuales 1 331 tienen propietario asignado y atributos completos.

> `1 331 ÷ 1 480 = 89.9 %`
> Brecha adicional: 78 activos descubiertos sin registro alguno en CMDB y 71 registros incompletos.

*Interpretación:* casi 1 de cada 10 activos críticos es invisible para el modelo de control. **Acción:** bloquear la publicación de indicadores dependientes del inventario (KPI-04, 06, 09, 10) hasta alcanzar ≥ 95 %, ya que sus denominadores heredan este error. Asignar propietario a los 78 activos huérfanos en un plazo de 30 días o desconectarlos.

---

### KPI-CIB-03 · Exposición de la Superficie de Ataque

**Fórmula de cálculo**

```
Conteo de exposición = |Activos accesibles desde Internet con vulnerabilidad explotable|
                     + |Activos accesibles desde Internet sin propietario identificado|

% Exposición = ( Conteo de exposición ÷ |Total de activos accesibles desde Internet| ) × 100
```

**Explicación conceptual paso a paso:**
1. Enumerar la superficie externa: rangos IP públicos, registros DNS y subdominios, certificados emitidos (registros de transparencia), servicios publicados en nube y aplicaciones SaaS con dominio corporativo.
2. Cruzar con resultados de escaneo externo para identificar vulnerabilidades con explotación viable (exploit público disponible, presencia en catálogo KEV o EPSS por encima del umbral definido).
3. Identificar los activos expuestos que no tienen propietario en la CMDB (*TI en la sombra*).
4. Sumar ambos conjuntos evitando doble conteo y expresar el resultado en valor absoluto y en porcentaje.

**Fuentes de datos necesarias**
- Plataforma de gestión de superficie de ataque externa (EASM) o escaneo perimetral programado.
- Zonas DNS, registros de certificados y registros del WAF/CDN.
- Inventario de direcciones IP públicas y balanceadores en nube.
- CMDB, para determinar la titularidad.

**Frecuencia de medición:** semanal (la superficie externa cambia por despliegues y por adopción de nube).

**Ejemplo práctico**
El descubrimiento externo identifica 212 activos expuestos. De ellos, 18 presentan vulnerabilidades con exploit público y 7 carecen de propietario (2 de ellos aparecen en ambas condiciones).

> `18 + 7 − 2 = 23 activos en exposición`
> `23 ÷ 212 = 10.8 %`

*Interpretación:* frente a una línea base previa de 31 activos, la reducción es de 25.8 %, tendencia favorable. Sin embargo, los 7 activos sin propietario representan el riesgo de mayor severidad: nadie los parcha, nadie los monitorea y no están en el alcance de ningún control. **Acción:** asignar titularidad en 15 días o retirarlos de Internet; remediar los 18 hallazgos explotables conforme al ANS de exposición externa.

---

### KPI-CIB-04 · Antigüedad de Vulnerabilidades

**Fórmula de cálculo**

```
Antigüedad_i = Fecha de corte − Fecha de primera detección de la vulnerabilidad i

Mediana de antigüedad = valor central del conjunto {Antigüedad_i} de vulnerabilidades críticas abiertas
% Fuera de ANS = ( |Vulnerabilidades críticas con Antigüedad_i > ANS| ÷ |Vulnerabilidades críticas abiertas| ) × 100
```

**Explicación conceptual paso a paso:**
1. Extraer todas las vulnerabilidades críticas (y altas, si el alcance lo incluye) **abiertas** al corte, sobre activos en alcance.
2. Para cada una, calcular los días transcurridos desde la **primera detección**, no desde la última detección del escáner. Este es el error más frecuente: los escáneres reinician la fecha en cada corrida, lo que rejuvenece artificialmente el inventario de deuda.
3. Ordenar el conjunto y obtener la **mediana** y el **percentil 90**.
4. Contar cuántas superan el ANS aprobado y expresarlo en porcentaje.

**Fuentes de datos necesarias**
- Escáner de vulnerabilidades (Tenable, Qualys, Rapid7 u homólogo), con histórico de primera detección habilitado.
- Sistema de tickets de remediación (fechas de asignación y cierre).
- CMDB, para determinar el alcance y la criticidad del activo.
- Registro de excepciones aprobadas.

**Frecuencia de medición:** quincenal en operación; mensual en reporte de gestión.

**Ejemplo práctico**
Al corte existen 60 vulnerabilidades críticas abiertas en servidores productivos. Al ordenar sus antigüedades, la mediana es de 46 días y el percentil 90 de 118 días. El ANS aprobado para críticas es de 30 días. 23 hallazgos superan ese plazo.

> `% fuera de ANS = 23 ÷ 60 = 38.3 %`

*Interpretación:* la mediana duplica el compromiso interno y existe una cola de deuda severa (10 % de los hallazgos supera los 118 días). Si se hubiera reportado el promedio simple, la presencia de casos extremos habría elevado el valor y ocultado la distribución. **Acción:** aislar el grupo del percentil 90 en un plan de choque con fecha firme, y analizar la causa raíz del retraso (¿ventanas de mantenimiento insuficientes, dependencias de proveedor, o ausencia de propietario técnico?).

---

### KPI-CIB-05 · Cumplimiento de Vulnerabilidades Explotadas Conocidas (KEV)

**Fórmula de cálculo**

```
% Cumplimiento KEV = ( |Vulnerabilidades KEV aplicables remediadas o mitigadas dentro del plazo|
                       ÷ |Total de vulnerabilidades KEV aplicables identificadas| ) × 100
```

**Explicación conceptual paso a paso:**
1. Descargar el catálogo vigente de vulnerabilidades explotadas conocidas (CISA KEV) y cruzarlo por CVE contra los resultados del escáner.
2. Filtrar por aplicabilidad real: la tecnología debe estar presente y la configuración debe hacer explotable el defecto.
3. Determinar el plazo: el más restrictivo entre la fecha límite del catálogo y el ANS interno.
4. Contar las remediadas (parche aplicado y verificado) o mitigadas mediante control compensatorio formalmente aprobado.
5. Calcular el porcentaje. Reportar en paralelo el número absoluto de KEV pendientes, ya que en este indicador **una sola pendiente puede ser material**.

**Fuentes de datos necesarias**
- Catálogo CISA KEV (u otra fuente de inteligencia de explotación activa).
- Resultados del escáner de vulnerabilidades con CVE y evidencia de reescaneo posterior a la remediación.
- Registro de cambios (evidencia del parche o de la mitigación aplicada).
- Registro de controles compensatorios aprobados.

**Frecuencia de medición:** semanal, con alerta inmediata ante la publicación de una KEV aplicable.

**Ejemplo práctico**
En el mes se identifican 14 CVE del catálogo KEV aplicables al entorno. Doce se remedian dentro del plazo; una se mitiga con regla de WAF y segmentación aprobadas por el Comité; una permanece sin atender en un servidor de un proveedor externo.

> `(12 + 1) ÷ 14 = 92.9 %`

*Interpretación:* el 92.9 % es irrelevante frente al dato material: **existe una vulnerabilidad con explotación activa comprobada, sin mitigar, en el entorno**. Este indicador no se gestiona por promedio sino por excepción. **Acción:** escalar el caso a nivel de dirección con fecha firme contractual del proveedor; evaluar el aislamiento del sistema mientras persista la exposición. Documentar la decisión en acta, ya que la aceptación tácita de una KEV es indefendible ante auditoría y ante la aseguradora cibernética.

---

### KPI-CIB-06 · Cumplimiento de Configuración Segura

**Fórmula de cálculo**

```
% Activos conformes = ( |Activos que cumplen ≥ U % de los controles del baseline aprobado|
                        ÷ |Activos en alcance del baseline| ) × 100

% Cumplimiento promedio = Σ (controles cumplidos por activo ÷ controles aplicables por activo) ÷ |Activos| × 100
```
Donde `U` es el umbral de conformidad por activo aprobado por la organización (referencia habitual: 95 %).

**Explicación conceptual paso a paso:**
1. Determinar la línea base aplicable por tipo de tecnología (CIS Benchmark, guía del fabricante o estándar interno endurecido), en su versión aprobada.
2. Evaluar cada activo y obtener su porcentaje de conformidad individual.
3. Calcular las dos vistas: la proporción de activos que superan el umbral (visión binaria, útil para gestión) y el cumplimiento promedio (visión de grado, útil para tendencia).
4. Separar y reportar las **desviaciones críticas** (por ejemplo, autenticación débil o cifrado deshabilitado), que deben tratarse por excepción sin importar el promedio.

**Fuentes de datos necesarias**
- Herramienta de gestión de configuración segura (CIS-CAT, SCM, Defender for Cloud, Wazuh u homólogo).
- Plataforma de administración de configuración (Ansible, Puppet, Intune, SCCM, Group Policy).
- Repositorio documental de líneas base aprobadas, con versión y fecha.
- Registro de excepciones técnicas vigentes.

**Frecuencia de medición:** mensual; continua para activos expuestos a Internet.

**Ejemplo práctico**
Se evalúan 900 servidores contra el baseline aprobado. 738 alcanzan al menos 95 % de conformidad; el cumplimiento promedio del parque es de 93.1 %; se detectan 11 servidores con desviaciones críticas (SMBv1 habilitado y cifrado en tránsito deshabilitado).

> `738 ÷ 900 = 82.0 % de activos conformes`

*Interpretación:* el promedio de 93.1 % transmite una falsa sensación de control; la lectura correcta es que **el 18 % del parque no cumple el umbral** y que existen 11 desviaciones críticas explotables. **Acción:** remediar las 11 desviaciones críticas en un plazo de 7 días; incorporar el baseline a la imagen base de despliegue para atacar la causa raíz, no los síntomas.

---

### KPI-CIB-07 · Cobertura de MFA Resistente a Suplantación

**Fórmula de cálculo**

```
% Cobertura = ( |Cuentas elegibles con método resistente a suplantación registrado y exigido por política|
                ÷ |Total de cuentas elegibles| ) × 100
```

**Explicación conceptual paso a paso:**
1. Definir "elegible": cuentas humanas activas con acceso a sistemas corporativos. Excluir cuentas de servicio (que se gobiernan con otro control) y documentar la exclusión.
2. Identificar los métodos que califican como **resistentes a suplantación**: FIDO2/WebAuthn, llaves de seguridad físicas, Windows Hello for Business, autenticación basada en certificados. **No califican** SMS, llamada de voz, correo electrónico ni códigos temporales (TOTP), por ser susceptibles a intermediación en tiempo real.
3. Verificar dos condiciones simultáneas: el método está **registrado** y la política de acceso condicional lo **exige** para ese usuario. Un método registrado pero no forzado no cuenta.
4. Calcular globalmente y, de forma obligatoria, segmentado para cuentas privilegiadas.

**Fuentes de datos necesarias**
- Proveedor de identidad (Entra ID, Okta, Ping) — reporte de métodos de autenticación registrados.
- Políticas de acceso condicional y su alcance de asignación.
- Registro de recursos humanos (altas, bajas y personal activo), para validar el denominador.
- Inventario de cuentas privilegiadas del PAM.

**Frecuencia de medición:** mensual; semanal durante el despliegue del programa.

**Ejemplo práctico**
La organización tiene 2 400 cuentas humanas elegibles; 1 980 tienen método resistente registrado y exigido. De las 150 cuentas privilegiadas, 150 cumplen.

> `Global: 1 980 ÷ 2 400 = 82.5 %`
> `Privilegiadas: 150 ÷ 150 = 100 %`

*Interpretación:* el resultado es favorable en el segmento de mayor impacto, que es el criterio correcto de priorización. Sin embargo, 420 cuentas conservan métodos susceptibles a intermediación. **Acción:** cerrar la brecha en dos oleadas (accesos remotos y aplicaciones financieras primero), y deshabilitar por política el registro de métodos no resistentes para impedir la regresión del indicador.

---

### KPI-CIB-08 · Cobertura de Revisión de Accesos Privilegiados

**Fórmula de cálculo**

```
% Recertificación = ( |Cuentas privilegiadas revisadas y recertificadas dentro del ciclo|
                      ÷ |Total de cuentas privilegiadas identificadas| ) × 100

% Depuración = ( |Accesos revocados o reducidos tras la revisión| ÷ |Cuentas revisadas| ) × 100
```

**Explicación conceptual paso a paso:**
1. Enumerar el universo privilegiado: administradores de dominio y de nube, cuentas de acceso a bases de datos productivas, cuentas de emergencia (*break-glass*) y accesos de terceros con privilegio.
2. Ejecutar la campaña de recertificación con el responsable del recurso, no con el área de TI, para preservar la independencia de la revisión.
3. Contar las cuentas con decisión registrada (mantener, reducir o revocar) dentro de la ventana del ciclo. Una revisión sin decisión registrada **no cuenta** como recertificada.
4. Calcular la cobertura y, como indicador complementario de eficacia, el porcentaje de depuración. Una tasa de depuración de 0 % es señal de una revisión realizada por mero trámite.

**Fuentes de datos necesarias**
- Solución PAM y plataforma de gobierno de identidades (IGA): campañas de certificación con evidencia de aprobación.
- Grupos privilegiados de Active Directory / Entra ID y roles IAM de nube.
- Bitácoras de cambios en la membresía de grupos privilegiados durante el ciclo.
- Matriz de roles y responsabilidades aprobada.

**Frecuencia de medición:** trimestral para cuentas privilegiadas; semestral para accesos estándar.

**Ejemplo práctico**
El universo privilegiado es de 312 cuentas. En la campaña trimestral se recertifican 289 con decisión registrada; 23 quedan sin respuesta del responsable. De las revisadas, 24 accesos fueron revocados y 9 reducidos de nivel.

> `Cobertura: 289 ÷ 312 = 92.6 %`
> `Depuración: 33 ÷ 289 = 11.4 %`

*Interpretación:* la tasa de depuración de 11.4 % indica una revisión con criterio real, no simbólica. Las 23 cuentas sin respuesta constituyen el riesgo relevante: privilegio activo sin dueño que lo valide. **Acción:** aplicar la regla de revocación automática por silencio administrativo tras el segundo recordatorio, práctica estándar en entornos regulados, con procedimiento documentado de restablecimiento.

---

### KPI-CIB-09 · Cobertura de Detección en Puntos Finales

**Fórmula de cálculo**

```
% Cobertura EDR = ( |Equipos elegibles con agente instalado, activo y con telemetría recibida en las últimas 72 h|
                    ÷ |Total de equipos elegibles según inventario independiente| ) × 100
```

**Explicación conceptual paso a paso:**
1. Construir el denominador desde el inventario y el directorio, **no desde la consola del EDR** (si el agente no está instalado, el equipo no aparece en su consola y la cobertura se reportaría como 100 % de forma espuria).
2. Definir elegibilidad por sistema operativo soportado y tipo de dispositivo; documentar las exclusiones (equipos industriales, dispositivos médicos, sistemas heredados) con control compensatorio.
3. Contar los agentes que cumplen las **tres** condiciones: instalado, en estado activo y con telemetría reciente. Un agente instalado pero silencioso no protege.
4. Reportar el porcentaje y la lista nominal de la brecha.

**Fuentes de datos necesarias**
- Consola EDR/XDR (estado de salud del agente y última comunicación).
- CMDB, Active Directory y plataforma de gestión de dispositivos.
- Bitácoras de DHCP y de acceso a red, para detectar equipos activos ausentes del inventario.

**Frecuencia de medición:** semanal.

**Ejemplo práctico**
El inventario reporta 3 150 equipos elegibles. La consola muestra 3 072 agentes instalados, de los cuales 3 024 reportaron telemetría en las últimas 72 horas.

> `3 024 ÷ 3 150 = 96.0 %`
> Brecha: 78 sin agente y 48 instalados pero silenciosos = 126 equipos sin detección efectiva.

*Interpretación:* 126 equipos constituyen puntos ciegos aprovechables para el establecimiento de persistencia. Los 48 agentes silenciosos son más preocupantes que los 78 sin agente, pues generan la ilusión de cobertura. **Acción:** investigar los agentes silenciosos como posible indicador de manipulación (*tampering*) antes de tratarlos como falla operativa; establecer control de admisión a red que impida la conexión de equipos sin agente activo.

---

### KPI-CIB-10 · Cobertura de Registros (Bitácoras)

**Fórmula de cálculo**

```
% Cobertura de registro = ( |Sistemas críticos que envían el 100 % de las fuentes de registro requeridas, con ingesta verificada|
                            ÷ |Total de sistemas críticos en alcance| ) × 100
```

**Explicación conceptual paso a paso:**
1. Definir el catálogo de **fuentes obligatorias por tipo de sistema** (autenticación, cambios de configuración, accesos privilegiados, operaciones sobre datos, tráfico de red, plano de control de nube).
2. Verificar no solo la conectividad, sino la **recepción efectiva y la calidad del evento**: campos completos, formato esperado y marca de tiempo sincronizada.
3. Un sistema cuenta como cubierto únicamente si envía **todas** sus fuentes obligatorias. La cobertura parcial se reporta como no conforme.
4. Complementar con dos métricas de salud: latencia de ingesta y porcentaje de fuentes con interrupción mayor a 24 horas en el periodo.

**Fuentes de datos necesarias**
- SIEM: inventario de fuentes, reportes de salud de conectores y volumen de ingesta por origen.
- CMDB, para la lista de sistemas críticos.
- Catálogo de casos de uso de detección (define qué registros son indispensables).
- Configuración de sincronización horaria (NTP), requisito previo para cualquier correlación.

**Frecuencia de medición:** semanal para salud; mensual para el indicador de gestión.

**Ejemplo práctico**
Existen 180 sistemas clasificados como críticos. 162 envían todas sus fuentes obligatorias con ingesta verificada; 11 envían parcialmente; 7 no envían registro alguno. Además, 4 fuentes presentaron interrupciones superiores a 24 horas no detectadas oportunamente.

> `162 ÷ 180 = 90.0 %`

*Interpretación:* el 10 % de los sistemas críticos no es investigable forensemente. Los 7 sin registro imposibilitan reconstruir un incidente que los involucre, con implicaciones legales y regulatorias directas. Las interrupciones no detectadas revelan ausencia de monitoreo del propio monitoreo. **Acción:** implementar alertamiento por ausencia de eventos (*heartbeat*) por fuente; condicionar el paso a producción de cualquier sistema crítico a la integración previa con el SIEM.

---

### KPI-CIB-11 · Tiempo Medio de Detección (MTTD)

**Fórmula de cálculo**

```
MTTD = Σ ( t_detección(i) − t_inicio_actividad_maliciosa(i) ) ÷ n
```
para los `n` incidentes validados del periodo. Se recomienda reportar además **mediana y percentil 90**.

**Explicación conceptual paso a paso:**
1. Considerar únicamente incidentes **validados** (descartar falsos positivos), segmentados por severidad.
2. Establecer `t_inicio` como la primera evidencia verificable de actividad maliciosa documentada en el informe de causa raíz. Si no se determina forensemente, se usa la primera evidencia disponible en telemetría y **se declara la limitación** en el reporte.
3. Establecer `t_detección` como el momento en que se genera la alerta que efectivamente originó la investigación, no cuando el analista la atendió.
4. Calcular el promedio, la mediana y el percentil 90 por estrato de severidad.

**Fuentes de datos necesarias**
- SIEM/SOAR: marcas de tiempo de alerta y de creación del caso.
- Sistema de gestión de incidentes (tickets).
- Informes de análisis de causa raíz y reportes forenses.
- Registros de la plataforma EDR, con la línea de tiempo del proceso.

**Frecuencia de medición:** mensual; análisis de tendencia trimestral.

**Ejemplo práctico**
En el mes se validan 9 incidentes de severidad alta. La suma de los tiempos de detección es de 63.5 horas. Los valores individuales incluyen un caso de 26 horas (compromiso de credenciales detectado por comportamiento anómalo) y ocho casos por debajo de 6 horas.

> `MTTD = 63.5 ÷ 9 = 7.06 horas` · `Mediana = 3.2 horas` · `P90 = 26 horas`

*Interpretación:* la mediana de 3.2 horas refleja el desempeño típico; el promedio de 7.06 está sesgado por un solo caso. El valor de gestión está en el percentil 90: **existe una clase de amenaza (abuso de credenciales válidas) que evade la detección durante más de un día.** **Acción:** desarrollar casos de uso de detección basados en comportamiento de identidad (viaje imposible, elevación anómala de privilegios, acceso fuera de patrón), en lugar de optimizar un promedio ya aceptable.

---

### KPI-CIB-12 · Tiempo Medio de Contención (MTTC)

**Fórmula de cálculo**

```
MTTC = Σ ( t_contención(i) − t_validación_incidente(i) ) ÷ n
```

**Explicación conceptual paso a paso:**
1. `t_validación` es el momento en que se confirma que la alerta corresponde a un incidente real (no la hora de la alerta original, ya medida en el MTTD).
2. `t_contención` es el momento en que se detiene la propagación: aislamiento del equipo, deshabilitación de la cuenta, bloqueo de la ruta de red o revocación de la sesión. **No** es el momento de la erradicación ni de la recuperación.
3. Ambas marcas de tiempo deben provenir de un registro automatizado (SOAR o bitácora del EDR), no del llenado manual del ticket.
4. Segmentar por severidad y por tipo de contención (automática contra manual), dato que sustenta la inversión en automatización.

**Fuentes de datos necesarias**
- SOAR y bitácoras de acciones de respuesta (aislamiento, bloqueo, deshabilitación de cuenta).
- Sistema de gestión de incidentes con registro de estados y marcas de tiempo.
- Bitácoras de firewall, proxy y proveedor de identidad, como evidencia técnica de la contención.

**Frecuencia de medición:** mensual.

**Ejemplo práctico**
Para los mismos 9 incidentes de severidad alta, la suma de tiempos de contención es de 31.5 horas. Seis incidentes se contuvieron automáticamente mediante aislamiento por SOAR (promedio 0.4 h) y tres requirieron intervención manual fuera de horario (promedio 9.1 h).

> `MTTC = 31.5 ÷ 9 = 3.5 horas`
> `Automatizados: 0.4 h · Manuales: 9.1 h`

*Interpretación:* la contención automatizada es 22 veces más rápida. El promedio global oculta ese hallazgo, que es el argumento de negocio central. **Acción:** extender los libros de respuesta automatizados a los tres escenarios que hoy requieren intervención manual, y evaluar la cobertura de guardia fuera de horario. El caso de negocio se sustenta en la reducción del tiempo de exposición, no en el ahorro de horas de analista.

---

### KPI-CIB-13 · Tiempo Medio de Recuperación (MTTR)

**Fórmula de cálculo**

```
MTTR = Σ ( t_restauración_servicio(i) − t_contención(i) ) ÷ m
```
para los `m` incidentes del periodo **con afectación a servicios**.

**Explicación conceptual paso a paso:**
1. Incluir únicamente incidentes que degradaron o interrumpieron un servicio; los incidentes contenidos sin impacto operativo no aplican y deben excluirse del denominador.
2. `t_restauración` es el momento en que el servicio vuelve a operar con funcionalidad y rendimiento normales, confirmado por el dueño del servicio, no por el equipo técnico.
3. Contrastar cada resultado individual contra el **RTO comprometido** de ese servicio (vínculo directo con KPI-CIB-16).
4. Reportar promedio, caso de mayor duración y número de incumplimientos de RTO.

**Fuentes de datos necesarias**
- Sistema de gestión de incidentes y de gestión de cambios.
- Herramientas de monitoreo de disponibilidad (confirmación objetiva de la restauración).
- Plan de continuidad del negocio y catálogo de servicios con RTO aprobado.
- Confirmación formal del dueño del proceso de negocio.

**Frecuencia de medición:** mensual; revisión trimestral contra el plan de continuidad.

**Ejemplo práctico**
Cinco de los nueve incidentes afectaron servicios. La suma de tiempos de recuperación es de 96 horas; el caso mayor fue de 52 horas (restauración de un servidor de archivos desde respaldo). Dos servicios excedieron su RTO comprometido de 8 horas.

> `MTTR = 96 ÷ 5 = 19.2 horas`

*Interpretación:* el promedio no es el dato relevante: **dos servicios incumplieron el compromiso de continuidad aprobado por la dirección**, lo que constituye materialización de riesgo de continuidad operativa con posible impacto contractual. **Acción:** revisar la estrategia de respaldo del servidor de archivos (el tiempo de restauración excede la capacidad del medio) y recalibrar los RTO que resultaron técnicamente inalcanzables, con aprobación formal del dueño del proceso.

---

### KPI-CIB-14 · Tasa de Recurrencia de Incidentes

**Fórmula de cálculo**

```
% Recurrencia = ( |Incidentes del periodo vinculados a una causa raíz previamente identificada (≤ 12 meses)|
                  ÷ |Total de incidentes validados del periodo| ) × 100
```

**Explicación conceptual paso a paso:**
1. Mantener un catálogo de causas raíz con identificador único, derivado de los análisis posteriores a incidentes.
2. Al clasificar cada incidente nuevo, verificar contra el catálogo si su causa ya fue identificada y si existía una acción correctiva comprometida.
3. Un incidente es recurrente si la causa raíz ya era conocida, **independientemente de si la acción correctiva fue implementada o no**; esa distinción se reporta como desglose (acción no implementada / acción implementada pero inefectiva).
4. Calcular la tasa y compararla contra los tres trimestres previos.

**Fuentes de datos necesarias**
- Sistema de gestión de incidentes con campo de causa raíz normalizado.
- Registro de acciones correctivas y no conformidades (ISO 27001 cl. 10.2).
- Informes de análisis posterior al incidente (*post-mortem*).
- Actas de seguimiento del Comité de Seguridad.

**Frecuencia de medición:** trimestral.

**Ejemplo práctico**
En el trimestre se validan 48 incidentes; 6 se vinculan a causas raíz ya catalogadas: 4 corresponden a acciones correctivas comprometidas y no implementadas, y 2 a acciones implementadas que resultaron inefectivas.

> `6 ÷ 48 = 12.5 %` (trimestre anterior: 17.0 %)

*Interpretación:* la tendencia mejora 4.5 puntos, pero el desglose señala el problema real: **el 67 % de la recurrencia proviene de acciones correctivas comprometidas que nunca se ejecutaron**. Esto no es una falla técnica, es una falla de gobierno y de seguimiento. **Acción:** incorporar las acciones correctivas al tablero de seguimiento del Comité con propietario y fecha firme; los 2 casos de acción inefectiva requieren reapertura del análisis de causa raíz, ya que el diagnóstico original fue incorrecto.

---

### KPI-CIB-15 · Éxito de Pruebas de Restauración de Respaldos

**Fórmula de cálculo**

```
% Éxito de restauración = ( |Pruebas de restauración exitosas con validación de integridad por el dueño del dato|
                            ÷ |Pruebas de restauración programadas en el periodo| ) × 100
```

**Explicación conceptual paso a paso:**
1. El denominador son las pruebas **programadas**, no las ejecutadas. Una prueba omitida cuenta como fallida; de lo contrario, el indicador premia la inacción.
2. Una prueba es exitosa solo si se restaura el dato, se verifica su integridad y funcionalidad, y el **dueño del dato o del proceso** firma la validación. La confirmación de la consola de respaldo no basta.
3. Registrar el tiempo real de restauración de cada prueba; alimenta el KPI-CIB-16.
4. Incluir al menos una prueba anual de restauración completa desde copia aislada o inmutable, escenario de recuperación ante ataque de cifrado extorsivo.

**Fuentes de datos necesarias**
- Consola de respaldos (Veeam, Commvault, Rubrik, Azure Backup u homólogo): bitácoras de trabajos y de restauraciones.
- Calendario aprobado de pruebas de restauración.
- Actas de prueba firmadas por el dueño del dato.
- Registro de tickets de incidencias durante las pruebas.

**Frecuencia de medición:** mensual para ejecución; trimestral para reporte de gestión.

**Ejemplo práctico**
El calendario trimestral contempla 24 pruebas sobre sistemas críticos. Se ejecutan 22; 21 concluyen exitosamente con validación firmada; 1 falla por corrupción del conjunto de respaldo; 2 no se ejecutaron por falta de ventana operativa.

> `21 ÷ 24 = 87.5 %`

*Interpretación:* contra una meta de 100 %, la brecha se compone de una falla técnica real y de dos omisiones de proceso. El caso de corrupción es el más severo: **un respaldo no verificado es una hipótesis de recuperación, no un control.** **Acción:** analizar la causa de la corrupción y extender la verificación a todo el conjunto de respaldos de ese sistema; formalizar ventanas de prueba obligatorias en el calendario de cambios para eliminar la excusa operativa.

---

### KPI-CIB-16 · Logro de Objetivos de Recuperación (RTO/RPO)

**Fórmula de cálculo**

```
% Logro RTO/RPO = ( |Recuperaciones (reales o probadas) que cumplen simultáneamente RTO y RPO|
                    ÷ |Total de recuperaciones evaluadas en el periodo| ) × 100
```

**Explicación conceptual paso a paso:**
1. El cumplimiento debe ser **simultáneo**: recuperar a tiempo (RTO) pero con pérdida de datos superior a la tolerada (RPO) es incumplimiento.
2. Para cada evento, calcular: tiempo real de recuperación contra RTO aprobado, y antigüedad del punto de recuperación utilizado contra RPO aprobado.
3. Integrar tanto recuperaciones reales derivadas de incidentes como pruebas planificadas, distinguiéndolas en el reporte.
4. Reportar de manera segmentada por criticidad de servicio; un incumplimiento en un servicio de nivel 1 no es comparable con uno de nivel 3.

**Fuentes de datos necesarias**
- Catálogo de servicios con RTO y RPO aprobados por el dueño del proceso (derivados del análisis de impacto al negocio, BIA).
- Actas de pruebas de continuidad y de recuperación ante desastres.
- Bitácoras de restauración con marca de tiempo del punto de recuperación utilizado.
- Registros de incidentes con afectación de servicio.

**Frecuencia de medición:** trimestral, más medición en cada evento real.

**Ejemplo práctico**
Se evalúan 18 recuperaciones (15 pruebas y 3 reales). 15 cumplen ambos objetivos; 2 cumplen RTO pero exceden el RPO (pérdida de 6 horas de transacciones contra un RPO comprometido de 1 hora); 1 excede el RTO.

> `15 ÷ 18 = 83.3 %`

*Interpretación:* los dos casos de incumplimiento de RPO implican **pérdida irrecuperable de información transaccional**, con posible impacto contable, contractual y regulatorio. La frecuencia de respaldo actual es incompatible con el compromiso asumido. **Acción:** elevar la frecuencia de respaldo o implementar replicación continua para esos servicios, o bien renegociar formalmente el RPO con el dueño del proceso. Mantener un RPO comprometido que la infraestructura no puede sostener constituye un riesgo aceptado de facto sin documentar.

---

### KPI-CIB-17 · Aseguramiento de Proveedores

**Fórmula de cálculo**

```
% Aseguramiento = ( |Proveedores críticos con debida diligencia de ciberseguridad vigente y concluida|
                    ÷ |Total de proveedores críticos identificados| ) × 100
```

**Explicación conceptual paso a paso:**
1. Determinar la criticidad del proveedor con criterios objetivos: acceso a datos personales o confidenciales, conectividad a la red interna, soporte a procesos críticos y dificultad de sustitución.
2. Definir "vigente": evaluación concluida en los últimos 12 meses (o 24 meses si el proveedor presenta certificación ISO/IEC 27001 o informe SOC 2 Tipo II vigente y aplicable al servicio contratado).
3. Definir "concluida": cuestionario respondido, evidencias revisadas, hallazgos documentados y plan de remediación acordado. Un cuestionario recibido sin analizar **no** cuenta.
4. Complementar con dos indicadores: porcentaje de contratos críticos con cláusulas de seguridad y notificación de incidentes, y número de hallazgos abiertos de proveedores.

**Fuentes de datos necesarias**
- Registro maestro de terceros y catálogo de proveedores críticos.
- Plataforma o expediente de evaluación de terceros (cuestionarios, evidencias, certificaciones).
- Repositorio contractual (cláusulas de seguridad, acuerdos de nivel de servicio, derecho de auditoría).
- Sistema de compras y cuentas por pagar, para validar la integridad del universo de proveedores.

**Frecuencia de medición:** trimestral.

**Ejemplo práctico**
Se identifican 64 proveedores críticos. 51 cuentan con debida diligencia vigente; 8 tienen evaluación vencida hace más de 12 meses; 5 nunca han sido evaluados, pese a tener acceso a datos productivos.

> `51 ÷ 64 = 79.7 %`

*Interpretación:* el 20 % de la cadena de suministro crítica opera sin aseguramiento. Los 5 proveedores nunca evaluados representan riesgo no gestionado y, adicionalmente, un incumplimiento del control A.5.19 de ISO/IEC 27001. **Acción:** evaluar los 5 casos en un plazo de 45 días con prioridad por volumen de datos accedidos; incorporar la evaluación de seguridad como requisito bloqueante en el flujo de alta de proveedores para prevenir la reincidencia estructural.

---

### KPI-CIB-18 · Aseguramiento de la Cadena de Suministro de Software

**Fórmula de cálculo**

```
% Aseguramiento de software = ( |Software crítico con SBOM vigente Y procedencia verificada|
                                ÷ |Total de software crítico en producción| ) × 100
```

**Explicación conceptual paso a paso:**
1. Delimitar el software crítico: aplicaciones de negocio, componentes de infraestructura, bibliotecas de terceros en sistemas expuestos y herramientas con privilegios elevados sobre el entorno.
2. Verificar la existencia de **SBOM vigente** (inventario de componentes en formato CycloneDX o SPDX) correspondiente a la versión desplegada, no a una versión anterior.
3. Verificar la **procedencia**: firma digital del artefacto, atestación de compilación o descarga desde repositorio autorizado con integridad comprobada.
4. Ambas condiciones deben cumplirse. Calcular el porcentaje y reportar por separado los componentes con vulnerabilidades conocidas identificadas vía el SBOM.

**Fuentes de datos necesarias**
- Repositorio de artefactos y canalización de integración y despliegue continuo (CI/CD).
- Herramienta de análisis de composición de software (SCA) y repositorio de SBOM.
- Registro de firmas, atestaciones y claves de verificación.
- Inventario de aplicaciones y catálogo de software aprobado.

**Frecuencia de medición:** mensual para desarrollo interno; trimestral para software adquirido.

**Ejemplo práctico**
Se identifican 45 aplicaciones críticas en producción. 32 cuentan con SBOM vigente; de ellas, 27 tienen además procedencia verificada mediante firma. El análisis de los SBOM disponibles revela 3 componentes con vulnerabilidades críticas conocidas.

> `27 ÷ 45 = 60.0 %`

*Interpretación:* el 40 % del software crítico se opera sin conocimiento de sus componentes, condición que impide responder en horas ante la divulgación de una vulnerabilidad de biblioteca ampliamente utilizada. El valor del SBOM es precisamente el tiempo de respuesta. **Acción:** generar SBOM obligatorio en la canalización de compilación para desarrollo interno (impacto inmediato y bajo costo) y exigirlo contractualmente a proveedores de software crítico en las próximas renovaciones. Remediar los 3 componentes vulnerables conforme al ANS de vulnerabilidades críticas.

---

### KPI-CIB-19 · Tasa de Configuraciones Deficientes en Nube

**Fórmula de cálculo**

```
% Configuración deficiente = ( |Recursos de nube que incumplen la línea base de seguridad aprobada|
                               ÷ |Total de recursos de nube evaluados| ) × 100
```

**Explicación conceptual paso a paso:**
1. Definir la línea base por proveedor y servicio (CIS Benchmark para AWS, Azure, GCP o estándar interno aprobado), con versión y fecha.
2. Evaluar de forma continua mediante la herramienta de postura de seguridad en la nube (CSPM) y verificar que su alcance cubra **todas** las suscripciones, cuentas y regiones, incluidas las de desarrollo y las heredadas de adquisiciones.
3. Calcular la tasa global y, obligatoriamente, el desglose por severidad. Un 3 % compuesto por hallazgos informativos es distinto de un 3 % con almacenamiento público expuesto.
4. Medir además el **tiempo de permanencia** de los hallazgos críticos, indicador más significativo que la tasa en entornos con alta rotación de recursos.

**Fuentes de datos necesarias**
- Plataforma CSPM (Wiz, Prisma Cloud, Defender for Cloud, Security Hub u homólogo).
- Servicios nativos de cumplimiento: AWS Config, Azure Policy, GCP Security Command Center.
- Análisis de infraestructura como código (Checkov, tfsec) en la canalización de despliegue.
- Inventario de suscripciones, cuentas y proyectos de nube, con propietario asignado.

**Frecuencia de medición:** continua con alertamiento; reporte semanal y consolidación mensual.

**Ejemplo práctico**
Se evalúan 12 400 recursos en tres cuentas de nube. 372 incumplen la línea base: 18 críticos (entre ellos, 2 depósitos de almacenamiento con acceso público y 4 grupos de seguridad con el puerto de administración remota abierto a Internet), 94 altos y 260 medios. La permanencia mediana de los hallazgos críticos es de 9 días.

> `372 ÷ 12 400 = 3.0 %` (tolerancia aprobada: ≤ 2 %)

*Interpretación:* la tasa global excede la tolerancia en 1 punto, pero el dato material es la **permanencia mediana de 9 días en hallazgos críticos**: una exposición de almacenamiento público durante nueve días es tiempo suficiente para su descubrimiento y explotación automatizada. **Acción:** implementar remediación automática para el conjunto de reglas críticas (acceso público y puertos administrativos expuestos) y desplazar el control hacia la fase de despliegue mediante políticas preventivas y validación de infraestructura como código, en lugar de corregir en producción.

---

### KPI-CIB-20 · Tasa de Reporte de Phishing

**Fórmula de cálculo**

```
% Reporte = ( |Simulaciones reportadas por el canal oficial| ÷ |Correos de simulación entregados| ) × 100
% Clic    = ( |Simulaciones con clic| ÷ |Correos de simulación entregados| ) × 100
Razón reporte/clic = |Reportes| ÷ |Clics|
```

**Explicación conceptual paso a paso:**
1. El denominador son los correos **entregados**, no los enviados. Los mensajes bloqueados por el filtro no llegaron al usuario y distorsionan el indicador.
2. Contar como reporte válido únicamente el realizado por el canal oficial (botón de reporte o buzón institucional), dentro de la ventana definida.
3. Calcular las tres cifras. **La razón reporte/clic es el indicador de mayor valor**: mide si la organización detecta más rápido de lo que cae.
4. Segmentar por área, nivel jerárquico y grado de dificultad de la campaña. Comparar campañas de dificultad distinta carece de validez estadística.

**Fuentes de datos necesarias**
- Plataforma de simulación de phishing (KnowBe4, Proofpoint, Defender for Office u homólogo).
- Registros del botón de reporte y del buzón de seguridad.
- Registros de la pasarela de correo (confirmación de entrega).
- Directorio organizacional, para la segmentación por área.

**Frecuencia de medición:** por campaña (recomendado: mensual), con consolidación trimestral.

**Ejemplo práctico**
Se entregan 2 300 correos de simulación de dificultad media. 989 son reportados por el botón oficial, 138 usuarios hacen clic y 22 ingresan credenciales.

> `Reporte: 989 ÷ 2 300 = 43.0 %` · `Clic: 138 ÷ 2 300 = 6.0 %` · `Razón: 989 ÷ 138 = 7.2 : 1`

*Interpretación:* la razón de 7.2:1 es sólida: por cada usuario que cae, siete alertan al equipo de seguridad, lo que habilita contención temprana. El dato crítico son los **22 usuarios que entregaron credenciales**, población que requiere intervención dirigida, no capacitación masiva. **Acción:** reforzamiento individual para esos 22 casos y verificación de que sus cuentas cuenten con MFA resistente a suplantación (vínculo con KPI-CIB-07). Medir la reducción del tiempo hasta el primer reporte, no solo el volumen de reportes.

---

### KPI-CIB-21 · Exposición a IA No Autorizada (*Shadow AI*)

**Fórmula de cálculo**

```
Eventos de exposición = |Eventos con datos sensibles dirigidos a servicios de IA no aprobados|
% Exposición IA = ( Eventos de exposición ÷ |Total de eventos de fuga de datos detectados| ) × 100
Usuarios únicos = |Usuarios distintos involucrados en eventos de exposición|
```

**Explicación conceptual paso a paso:**
1. Mantener un **catálogo de servicios de IA aprobados**; sin ese catálogo, la categoría "no aprobado" no puede determinarse y el indicador no es calculable.
2. Identificar el tráfico hacia servicios de IA generativa mediante el agente de seguridad en la nube (CASB/SSE), el proxy o los registros DNS.
3. Cruzar con las políticas de prevención de fuga de datos para detectar los eventos que involucran información clasificada como confidencial, personal o regulada.
4. Reportar las **tres dimensiones**: número de eventos, usuarios únicos y volumen o número de registros sensibles expuestos. Una sola de ellas induce conclusiones erróneas.
5. Clasificar los eventos por tipo de dato (código fuente, datos personales, información financiera, estrategia comercial).

**Fuentes de datos necesarias**
- Plataforma CASB/SSE (Netskope, Zscaler, Defender for Cloud Apps u homólogo).
- Solución de prevención de fuga de datos (endpoint, correo y navegador).
- Registros de proxy y DNS, para identificar servicios de IA no catalogados.
- Catálogo de aplicaciones de IA aprobadas y política de uso de inteligencia artificial.
- Extensiones de navegador y registros de dispositivos administrados.

**Frecuencia de medición:** mensual, con alertamiento inmediato ante eventos con datos regulados.

**Ejemplo práctico**
En el mes se detectan 1 870 eventos de fuga de datos; 214 se dirigen a servicios de IA generativa no aprobados, involucrando a 63 usuarios únicos. Por tipo de dato: 141 eventos de código fuente, 52 de información comercial y 21 con datos personales de clientes.

> `214 ÷ 1 870 = 11.4 %` · `63 usuarios únicos` · `21 eventos con datos personales`

*Interpretación:* frente a una línea base de 297 eventos en el mes anterior, la tendencia decrece 27.9 %. No obstante, los 21 eventos con datos personales de clientes constituyen **exposición con implicaciones regulatorias** (Ley Federal de Protección de Datos Personales y, en su caso, RGPD según jurisdicción del titular) y deben tratarse como incidentes, no como métrica. **Acción:** evaluar los 21 casos bajo el procedimiento de incidentes de privacidad. En paralelo, el volumen concentrado en código fuente revela una necesidad no atendida: ofrecer una herramienta de IA aprobada y segura reduce la exposición más eficazmente que el bloqueo, que solo desplaza la actividad a dispositivos personales fuera de toda visibilidad.

---

## Parte III — Ficha técnica obligatoria por indicador

Antes de publicar cualquier indicador, el propietario debe completar y mantener vigente la siguiente ficha. Sin ella, la métrica no es auditable.

| Campo | Contenido requerido |
|---|---|
| Código y nombre | Identificador correlativo y denominación oficial |
| Objetivo de negocio | Qué decisión de gestión habilita este indicador |
| Fórmula exacta | Numerador, denominador y unidad de medida |
| Definición del alcance | Población incluida y criterios de exclusión |
| Fuentes de datos | Sistema de origen, consulta o reporte específico, y responsable de la extracción |
| Frecuencia y fecha de corte | Periodicidad y momento exacto del corte |
| Umbral aprobado | Valor objetivo, rangos de semáforo y fecha de aprobación |
| Propietario / Custodio | Quien responde por el resultado / quien extrae el dato (roles distintos) |
| Ruta de escalamiento | A quién se notifica y en qué plazo ante incumplimiento del umbral |
| Vínculo a control | Control ISO/IEC 27001, función NIST CSF y control CIS correspondiente |
| Limitaciones conocidas | Supuestos y sesgos declarados de la medición |
| Control de versiones | Versión, fecha de vigencia y registro de cambios a la definición |
