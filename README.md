# SOC Lab 04 – Movimiento Lateral / Lateral Movement (Analytical Case Study)

---

## 🇪🇸 Versión en Español

---

## Descripción general

Este laboratorio forma parte de mi **Progressive SOC Track**, una serie de labs conectados cuyo objetivo es mostrar, paso a paso, la evolución de un incidente de seguridad desde el punto de vista defensivo.

A diferencia de laboratorios centrados en la ejecución técnica u ofensiva, este lab se enfoca en el **análisis SOC**, el estudio de logs y la comprensión de patrones anómalos a partir de actividad aparentemente legítima.

En este punto de la historia, el atacante **ya dispone de credenciales válidas**. No necesita explotar vulnerabilidades ni ejecutar malware visible. El reto para el SOC es detectar **movimiento lateral técnico** en un escenario de bajo ruido.

---

## Posición dentro de la historia SOC

Este laboratorio continúa el hilo iniciado en los labs anteriores:

• `soc-lab-01-iam` – Identidad y control de accesos  
• `soc-lab-02-endpoint` – Actividad en endpoint  
• `soc-lab-03-persistence-analysis` – Persistencia y mantenimiento de acceso  

En este cuarto lab se analiza qué ocurre cuando el atacante comienza a **moverse lateralmente dentro del entorno**, utilizando credenciales válidas y técnicas discretas.

---

## Objetivo del laboratorio

El objetivo principal de este lab es desarrollar criterio defensivo para:

• Definir un baseline de actividad legítima.  
• Identificar autenticaciones remotas sin uso interactivo asociado.  
• Entender por qué la ausencia de eventos puede ser una señal relevante.  
• Analizar patrones completos en lugar de eventos aislados.  
• Pensar y razonar como un analista SOC.

No se busca comprometer sistemas, sino **interpretar correctamente los registros de seguridad**.

---

## Entorno de trabajo

El laboratorio se desarrolla en un entorno Windows con Active Directory:

• Host SOC (origen): `LAB-SOC-CL01`  
• Host objetivo: `CLIENTE-IAM01`  
• Usuario: cuenta de dominio estándar  

Se utilizan únicamente herramientas nativas del sistema, principalmente el **Visor de eventos de Windows**.

---

## Desarrollo del laboratorio

### Actividad legítima y baseline

En primer lugar, se genera actividad normal de un usuario en su equipo habitual.  
Esta actividad incluye inicio de sesión interactivo, uso real del sistema y cierre de sesión.

A partir de esta actividad se construye un **baseline**, que define cómo se comporta un usuario humano cuando utiliza el sistema de forma legítima.

Este baseline será la referencia para detectar comportamientos anómalos en fases posteriores.

---

### Reconocimiento sin credenciales

Desde el host SOC se realiza una comprobación básica de conectividad hacia el host objetivo **sin autenticación**.

Esta fase representa reconocimiento previo y no genera eventos de inicio de sesión ni actividad interactiva en el sistema objetivo.

---

### Movimiento lateral técnico

A continuación, se produce un acceso remoto autenticado desde el host SOC hacia el host objetivo utilizando credenciales válidas.

Este acceso se caracteriza por:

• Autenticación de red correcta.  
• Ausencia de sesión interactiva.  
• Ausencia de uso humano del sistema.  

El movimiento lateral se realiza de forma puntual y sin ejecución visible de procesos.

---

### Evidencia del evento anómalo

En el host objetivo se analizan los registros de seguridad generados por el acceso remoto.

Se observa un evento de autenticación de red válido, pero **no existe actividad interactiva asociada** ni creación de procesos posteriores.

La anomalía no reside en un evento concreto, sino en la **ruptura del patrón esperado** definido en el baseline.

---

## Conclusión SOC

El análisis demuestra que una autenticación válida **sin actividad interactiva posterior**, en contraste con el patrón normal de uso del usuario, es indicativa de **movimiento lateral técnico**.

Este tipo de comportamiento puede pasar desapercibido si el análisis se limita a eventos aislados y no se tiene en cuenta el contexto completo.

---

## Conclusiones finales

Este laboratorio pone de manifiesto que:

• El movimiento lateral no siempre genera señales evidentes.  
• La ausencia de eventos puede ser tan relevante como su presencia.  
• El análisis de patrones completos es clave en un entorno SOC.  

El foco del lab no está en la ejecución técnica, sino en el **razonamiento defensivo y la toma de decisiones**.

---

---

## 🇬🇧 English Version

---

## General description

This laboratory is part of my **Progressive SOC Track**, a series of connected labs designed to show, step by step, the evolution of a security incident from a defensive perspective.

Unlike labs focused on technical execution or offensive tooling, this lab emphasizes **SOC analysis**, log interpretation and the identification of anomalous patterns derived from seemingly legitimate activity.

At this stage of the story, the attacker **already possesses valid credentials**. No vulnerability exploitation or visible malware is required. The challenge for the SOC is detecting **technical lateral movement** in a low-noise scenario.

---

## Position within the SOC story

This lab continues the storyline introduced in previous labs:

• `soc-lab-01-iam` – Identity and access control  
• `soc-lab-02-endpoint` – Endpoint activity  
• `soc-lab-03-persistence-analysis` – Persistence and continued access  

This fourth lab focuses on what happens when the attacker begins to **move laterally within the environment**, using valid credentials and stealthy techniques.

---

## Lab objective

The main objective of this lab is to develop defensive reasoning to:

• Define a legitimate activity baseline.  
• Identify remote authentications without interactive usage.  
• Understand why the absence of events can be meaningful.  
• Analyze full patterns instead of isolated events.  
• Think and reason like a SOC analyst.

The goal is not system exploitation, but **correct interpretation of security logs**.

---

## Environment

The lab is conducted in a Windows Active Directory environment:

• SOC host (source): `LAB-SOC-CL01`  
• Target host: `CLIENTE-IAM01`  
• User: standard domain account  

Only native system tools are used, primarily the **Windows Event Viewer**.

---

## Lab development

### Legitimate activity and baseline

First, normal user activity is generated on the user’s regular workstation.  
This activity includes interactive logon, real system usage and logoff.

From this activity, a **baseline** is established, defining how a legitimate human user behaves.

This baseline is later used as a reference point to detect anomalies.

---

### Credential-less reconnaissance

From the SOC host, a basic connectivity check to the target host is performed **without authentication**.

This phase represents preliminary reconnaissance and does not generate logon or interactive activity events on the target system.

---

### Technical lateral movement

An authenticated remote access is then performed from the SOC host to the target host using valid credentials.

This access is characterized by:

• Successful network authentication.  
• No interactive session.  
• No human system usage.  

The lateral movement is isolated and does not involve visible process execution.

---

### Anomalous event evidence

On the target host, security logs generated by the remote access are analyzed.

A valid network authentication event is observed, but **no interactive activity or subsequent process creation** is present.

The anomaly lies not in a single event, but in the **break of the expected baseline pattern**.

---

## SOC conclusion

The presence of a valid authentication **without subsequent interactive activity**, when contrasted with the user’s normal behavior, is indicative of **technical lateral movement**.

This type of activity can remain unnoticed if analysis focuses solely on individual events instead of full context.

---

## Final conclusions

This lab highlights that:

• Lateral movement does not always produce obvious signals.  
• The absence of events can be as important as their presence.  
• Full pattern analysis is essential in a SOC environment.  

The lab prioritizes **defensive reasoning and analytical thinking** over technical execution.

---

