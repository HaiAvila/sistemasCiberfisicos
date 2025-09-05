---
title: "Resultados"
parent: "Arq.1 — Punto a Multipunto"
layout: default
nav_order: 4
---

# Resultados (Ejercicio 4)

**Fotos de montaje**
![Foto montaje](/assets/img/arquitecturas/ej4/montaje.jpg)

**Logs de pruebas**
- Carpeta: `/assets/logs/`  

**Tabla resumen (ejemplo)**  

| Caso | Acción | Resultado esperado | Resultado obtenido | Observaciones |
|------|--------|--------------------|--------------------|---------------|
| 1 | Switch1 → LED1 | LED1 encendido | ✅ | OK |
| 2 | Switch2 → LED2 | LED2 encendido | ✅ | OK |
| 3 | Switch1+Switch2 → LED1+LED2 | Ambos encendidos | ⚠️ | LED2 parpadea |

**Videos de implementación**
- [UART — 3 plataformas]({{ "/practica-1/arquitecturas/4-punto-a-multipunto/videos-uart/" | relative_url }})
