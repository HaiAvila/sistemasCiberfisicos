---
title: "UART"
layout: default
nav_order: 10
has_children: true
---

# UART — Objetivo y entregables

**Objetivo:**  
1. Identificar el **baud rate máximo estable** en cada plataforma.  
2. Medir la **latencia round-trip** enviando 1000 mensajes por par de dispositivos.  
3. Representar los resultados con **gráficas**, **tablas comparativas**, **diagramas eléctricos** y **códigos implementados**.

**Entregables de esta sección:**
- Diagrama de **arquitectura** y **esquema eléctrico** (KiCad).
- **Códigos** (maestro/esclavo) por plataforma.
- **6 gráficas** de latencia (todas las combinaciones de plataformas).
- **Tabla resumen** con métricas (mean, median, p95, std, jitter).

**Subsecciones:**
1. [Arquitectura y conexiones]({{ "/uart/arquitectura/" | relative_url }})
2. [Firmware]({{ "/uart/firmware/" | relative_url }})
3. [Pruebas]({{ "/uart/pruebas/" | relative_url }})
4. [Resultados]({{ "/uart/resultados/" | relative_url }})
