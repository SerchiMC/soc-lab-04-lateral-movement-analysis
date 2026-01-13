# SOC Lab 04 – Troubleshooting Notes

---

## 🇪🇸 Versión en Español

---

## Introducción

Este documento recoge los **problemas técnicos reales** encontrados durante la ejecución del laboratorio y las **decisiones tomadas** para resolverlos.

No se trata de errores del lab, sino de situaciones habituales en entornos Windows que afectan directamente a la **visibilidad de eventos** y, por tanto, al análisis SOC.

---

## ICMP bloqueado por firewall

**Situación observada**

Durante la fase de reconocimiento sin credenciales, el host objetivo no respondía a solicitudes ICMP (`ping`), a pesar de existir conectividad de red.

**Análisis**

La ausencia de respuesta ICMP no implica que el sistema no esté activo o accesible, sino que el firewall puede estar bloqueando explícitamente este tipo de tráfico.

**Acción tomada**

Se revisó la configuración del Firewall de Windows en el host objetivo y se habilitó la regla de entrada correspondiente a **ICMP Echo Request**.

**Resultado**

Tras el ajuste, el host comenzó a responder correctamente a las solicitudes ICMP.

**Lectura SOC**

Un host que no responde a ICMP puede seguir siendo accesible por otros servicios. La falta de respuesta no debe interpretarse como inexistencia del sistema.

---

## Auditoría de creación de procesos (evento 4688) no habilitada

**Situación observada**

Durante la generación de actividad legítima, no aparecían eventos **4688 (Process Creation)** en los registros de seguridad.

**Análisis**

La auditoría de creación de procesos no está habilitada por defecto en muchos sistemas Windows. Sin esta auditoría, la visibilidad sobre el uso real del sistema es limitada.

**Acción tomada**

Se habilitó la directiva de auditoría correspondiente a **seguimiento de procesos (eventos correctos)** y se forzó la actualización de directivas.

**Resultado**

A partir de ese momento comenzaron a registrarse eventos 4688 asociados a procesos legítimos, como `explorer.exe`.

**Lectura SOC**

La ausencia de eventos no siempre indica ausencia de actividad, sino posibles carencias de configuración en la auditoría.

---

## Aplicaciones modernas y generación de eventos

**Situación observada**

Al utilizar aplicaciones como Bloc de notas, no siempre se generaban eventos de creación de proceso claramente identificables como `notepad.exe`.

**Análisis**

En versiones modernas de Windows, algunas aplicaciones están empaquetadas como apps del sistema, lo que puede hacer que los procesos aparezcan con nombres distintos o no generen eventos fácilmente atribuibles.

**Decisión**

Para el baseline de actividad legítima se utilizó `explorer.exe` como referencia clara de uso humano del sistema.

**Lectura SOC**

No todas las aplicaciones generan evidencias limpias en los logs. El análisis debe adaptarse al comportamiento real del sistema.

---

## Movimiento lateral sin uso interactivo

**Situación observada**

Durante el movimiento lateral, se generó una autenticación remota válida, pero no existía actividad interactiva posterior en el host objetivo.

**Análisis**

El acceso se realizó mediante autenticación de red técnica, sin inicio de sesión interactivo ni ejecución de procesos visibles.

**Resultado**

Se observó un evento **4624 tipo 3 (Network)** sin eventos 4688 asociados.

**Lectura SOC**

En este escenario, la señal de alerta no es un evento anómalo aislado, sino la **ausencia de actividad esperada** tras una autenticación válida.

---

## Conclusión

Los problemas documentados en este archivo reflejan situaciones comunes en entornos reales. Lejos de invalidar el laboratorio, aportan contexto y refuerzan la importancia de:

• Configurar adecuadamente la auditoría.  
• Interpretar los logs dentro de su contexto.  
• No asumir que la falta de eventos equivale a falta de actividad.  

---

---

## 🇬🇧 English Version

---

## Introduction

This document summarizes the **real technical issues** encountered during the execution of the lab and the **decisions made** to address them.

These situations are common in Windows environments and directly affect **event visibility** and SOC-level analysis.

---

## ICMP blocked by firewall

**Observed situation**

During the credential-less reconnaissance phase, the target host did not respond to ICMP requests (`ping`), despite having network connectivity.

**Analysis**

Lack of ICMP response does not imply that a system is offline. Firewalls often block ICMP traffic by default.

**Action taken**

The Windows Firewall configuration on the target host was reviewed and the **ICMP Echo Request (Inbound)** rule was enabled.

**Result**

The host began responding to ICMP requests.

**SOC insight**

A host may be reachable even if ICMP is blocked. Absence of ICMP response should not be interpreted as system unavailability.

---

## Process creation auditing (event 4688) not enabled

**Observed situation**

During legitimate activity generation, no **4688 (Process Creation)** events were present in the security logs.

**Analysis**

Process creation auditing is not enabled by default on many Windows systems, limiting visibility into real system usage.

**Action taken**

The audit policy for **process tracking (success events)** was enabled and group policies were refreshed.

**Result**

Events 4688 began to appear, associated with legitimate processes such as `explorer.exe`.

**SOC insight**

Missing events often indicate auditing gaps rather than absence of activity.

---

## Modern applications and event generation

**Observed situation**

When using applications such as Notepad, process creation events were not always clearly identified as `notepad.exe`.

**Analysis**

Modern Windows applications may be packaged as system apps, causing processes to appear under different names or generate less obvious events.

**Decision**

`explorer.exe` was used as the primary reference for legitimate user activity.

**SOC insight**

Not all applications produce clean, easily identifiable log entries. Analysts must adapt to real system behavior.

---

## Lateral movement without interactive usage

**Observed situation**

During lateral movement, a valid remote authentication occurred, but no interactive activity followed on the target host.

**Analysis**

The access was performed using a technical network authentication, without interactive logon or visible process execution.

**Result**

A **4624 Logon Type 3 (Network)** event was observed without subsequent 4688 events.

**SOC insight**

In this scenario, the alerting signal is not an unusual event, but the **absence of expected activity** after authentication.

---

## Conclusion

The issues documented here reflect common real-world conditions. Rather than invalidating the lab, they reinforce the importance of:

• Proper audit configuration.  
• Contextual log interpretation.  
• Understanding that absence of events can itself be meaningful.  

---
