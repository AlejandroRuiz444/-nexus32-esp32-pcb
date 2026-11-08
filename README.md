# NEXUS32

Tarjeta de 160 x 120 mm, 4 capas (Top, L2_GND, L3_PWR, Bottom), con ESP32, Ethernet ENC28J60, USB-C, ADC MCP3208, relés y salidas.
220 componentes, 128 redes, 658 pines. Altium Designer 26.9.1.

Curso: Diseño Electrónico, UTP. Docente: Ing. Johan Riascos S.
Autor: Yesid Alejandro Yela Ruiz.

## Contenido
- `Documentation/`: desviaciones (D-01 a D-71), estado de la entrega y notas del diseño.
- `Libraries/`: símbolos, huellas y modelos 3D.
- `Manufacturing/A3_2026-11-07/`: Gerber (11 capas), taladros, IPC-356, BOM, CPL, STEP y README de fabricación.
- `NEXUS32.PrjPcb`, `NEXUS32.PcbDoc` y hojas `.SchDoc`: proyecto Altium.
- `MANIFIESTO.csv` / `MANIFIESTO.md`: tamaño y SHA-256 de cada archivo.

## Estado del DRC
- Cortos, separaciones, anchos, vías y agujeros: 0.
- Eléctricas: 3 excepciones documentadas (SQW sin rutear, pares diferenciales TPOUT y TPIN).
- Serigrafía: 136 contra serigrafía y 140 contra máscara; no afecta al cobre ni a la fabricación.
