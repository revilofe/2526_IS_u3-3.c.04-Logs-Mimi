# Actividad práctica: investigación de un incidente a partir de logs

En esta actividad práctica, se te presenta un escenario de investigación de incidentes basado en un conjunto de logs simulados. Tu tarea es analizar estos logs, identificar patrones sospechosos y construir una narrativa coherente sobre lo que ha ocurrido, utilizando la metodología de investigación de incidentes vista en clase.

reponde en [informe.md](./informe.md)

## 1. Contexto del caso

El caso comienza en el **SOC**, a partir de una alerta que ha sido considerada lo bastante relevante como para abrir una investigación. En una situación real, este punto de partida suele producirse después de que las herramientas de monitorización y correlación hayan reunido actividad sospechosa procedente de varios sistemas y la hayan puesto en manos del analista para su triage. La investigación, por tanto, no arranca “desde cero”, sino desde un **conjunto de evidencias ya recogidas y acotadas** por el equipo de seguridad.

Para este ejercicio se ha seleccionado una **ventana temporal de 1 hora**, dentro de la cual se concentran los eventos más relevantes del incidente. Esta forma de trabajo encaja con una práctica habitual en investigación: fijar un rango temporal concreto, ordenar los eventos desde los más antiguos y empezar a correlacionar lo ocurrido alrededor del inicio observable del caso. El objetivo no es revisar todos los logs de toda la organización, sino centrarse primero en la franja temporal donde el comportamiento anómalo empieza a tomar forma.

La evidencia entregada mezcla tres tipos de telemetría especialmente útiles en una investigación inicial sobre Windows:

* **autenticaciones**,
* **creación de procesos**,
* **conexiones de red**.

Este enfoque tiene sentido porque, en incidentes reales, los ataques suelen dejar huellas aparentemente dispersas en varios dispositivos, y el valor del análisis aparece precisamente al **correlacionar eventos de diferentes categorías y diferentes equipos**.

En este escenario, los eventos proceden de varios sistemas Windows de la organización, y está construido para simular una situación en la que el analista recibe un conjunto de eventos ya priorizados porque contienen actividad que merece revisión inmediata. Es decir, no estás ante un simple ejercicio de lectura de logs, sino ante una **investigación de un posible compromiso en varios equipos**. El escenario se plantea precisamente como una mezcla de eventos de proceso, autenticación y red dentro de una ventana de ataque simulada de una hora.

Tu trabajo consiste en **investigar el incidente con método**, reconstruir qué ha pasado y justificar cada conclusión con evidencias concretas. No se trata de responder preguntas sueltas de forma aislada, sino de construir una explicación técnica coherente a partir de los datos disponibles.

## 2. Qué se espera de ti

Debes actuar como **analista junior de ciberseguridad / analista SOC** y elaborar un informe técnico que responda, como mínimo, a estas preguntas:

1. ¿Se observa uso de comandos PowerShell codificados?
2. ¿Aparece la ejecución de herramientas y con qué finalidad?
3. ¿Existe actividad de logon anómala o sospechosa?
4. ¿Hay indicios de movimiento lateral?
5. ¿Cuál es la secuencia más probable del incidente?
6. ¿Qué tácticas y técnicas de **MITRE ATT&CK** encajan mejor?
7. ¿Qué nivel de criticidad tiene el caso y qué acciones iniciales propondrías?


## 3. Metodología obligatoria de resolución

Debes resolver el caso siguiendo las [**10 fases de investigación de incidentes**](https://revilofe.github.io/section2/u03/teoria/IS-U3.3.2.-InvestigacionDeIncidentes/#31-las-10-fases-de-trabajo) vistas en clase.
Aunque en este escenario algunas fases no pueden ejecutarse como en una investigación real completa, **debes incluirlas igualmente**, indicando la **asunción razonable** que haces en cada una.

## 4. Recursos entregados

Para resolver la actividad dispones de:

### 4.1. Logs del escenario

Se te entrega el conjunto de [logs del **escenario 1**](./Escenario-001.csv), que corresponde a una simulación de ataque de una hora con eventos de proceso, autenticación y red.

### 4.2. Guía teórica de investigación de incidentes

Debes aplicar la metodología explicada en clase sobre:

* hipótesis inicial;
* correlación;
* línea temporal;
* Cyber Kill Chain;
* modelo de diamante;
* y uso de MITRE ATT&CK.

### 4.3. MITRE ATT&CK

Debes utilizar MITRE ATT&CK para proponer, al menos, un mapeo razonado de las técnicas observadas.


## 5. Qué debes entregar

Debes entregar un **informe técnico individual** en formato **Markdown**.

### Estructura obligatoria

Tu informe debe incluir estos apartados:

1. **Resumen ejecutivo** (breve, claro y sin tecnicismos)
2. **Hipótesis inicial**
3. **Delimitación del alcance**
4. **Eventos relevantes**
5. **Correlación preliminar**
6. **Normalización y desconflicción**
7. **Segunda correlación**
8. **Línea temporal**
9. **Análisis con MITRE ATT&CK**
10. **Conclusión final**
11. **Acciones iniciales recomendadas de contención**
12. **Anexo de Inventario de evidencias**


## 6. Requisitos mínimos de calidad

Tu informe debe cumplir, como mínimo, estas condiciones:

* citar al menos **5 evidencias concretas** de los logs;
* construir una **línea temporal razonada** del incidente;
* incluir al menos **3 técnicas MITRE ATT&CK** justificadas;
* diferenciar claramente entre:

    * **hecho observado**,
    * **inferencia razonable**,
    * **hipótesis no confirmada**;
* y explicar qué información adicional pedirías en una investigación real.


## 7. Cómo debes presentar las evidencias

Cada evidencia relevante debe presentarse con este formato:

* **Id** (para referenciarla luego en el informe)
* **Hora**
* **Host**
* **Usuario**
* **EventID**
* **Tipo de evento**
* **Proceso**
* **Comando o actividad**
* **Interpretación**

### Ejemplo de formato

* Id: 001
* Hora: 10:03
* Host: SERVER-01
* Usuario: izzmier
* EventID: 4688
* Tipo: Process Creation
* Proceso: cmd.exe
* Comando: `powershell -enc ...`
* Interpretación: ejecución sospechosa de PowerShell codificado


## 8. Criterio metodológico importante

En este ejercicio **no se valora solo acertar “qué malware parece” o “qué herramienta sale”**.

Se valora, sobre todo, que demuestres que sabes:

* investigar con orden;
* correlacionar eventos;
* justificar conclusiones;
* y reconocer los límites de la evidencia.

Dicho de forma menos elegante, el objetivo no es ver el evento clave en dos segundos, sino quien saber explicar **qué significa, dónde aparece, con qué se relaciona y hasta dónde se puede afirmar sin inventarse media intrusión**.


## 9. Plantilla mínima para MITRE ATT&CK

Debes incluir una tabla como esta en tu informe, que refleje el mapeo entre la secuencia de eventos clave y MITRE.

| Evidencia id | Host | Usuario | Interpretación | Táctica MITRE | Técnica MITRE |
|--------------|------|---------|----------------|---------------|---------------|


Eje: ejemplo de tabla

| id  | Evidencia                                                                                | Host      | Usuario | Interpretación                                                           | Táctica MITRE | Técnica MITRE |
|-----|------------------------------------------------------------------------------------------|-----------|---------|--------------------------------------------------------------------------|---------------|---------------|
| 001 | Comando PowerShell codificado `powershell -enc ...` en SERVER-01 a las 10:03 por izzmier | SERVER-01 | izzmier | Ejecución de comando codificado, indicador fuerte de actividad maliciosa | Execution     | T1059.001     |

No copies directamente una respuesta externa. El valor está en justificar por qué propones cada mapeo.


## 10. Qué no debes hacer

No se aceptará como resolución válida un informe que:

* se limite a enumerar logs sin analizarlos;
* afirme como hechos cosas que no están demostradas;
* invente el vector inicial sin evidencia;
* o copie una “kill chain” sin conectar los eventos.


## 11. Resultado esperado

Al terminar la actividad, tu informe debería permitir responder con claridad a estas tres preguntas:

* **Qué ha pasado**
* **Qué evidencia lo sostiene**
* **Qué harías a continuación como analista**
   *  Medidas de contención iniciales

