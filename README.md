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

El **DRC** (Design Rule Check) es la revisión automática de Altium: compara la placa con 50 reglas (distancias mínimas, anchos de pista, tamaño de agujeros, largo de pares) y cada incumplimiento sale como un aviso.

### Lo que está en 0 (decide si la placa se fabrica y funciona)
- **Cortos:** ninguna pista toca otra que no debe.
- **Separaciones:** todo el cobre guarda la distancia mínima de JLCPCB.
- **Anchos:** ninguna pista es más fina de lo permitido.
- **Vías y agujeros:** tamaños y distancias válidos para fabricar.

### 3 excepciones eléctricas (documentadas)
| Aviso | Qué significa | Por qué se acepta |
|---|---|---|
| **SQW sin rutear** (1) | SQW es la salida de onda cuadrada del reloj de tiempo real. No se usa en el circuito, por eso no tiene conexión y Altium la marca como red sin rutear. | Decisión consciente (D-46). No afecta al funcionamiento. |
| **Par diferencial TPOUT** (1) | Los dos hilos de Ethernet deberían tener casi el mismo largo. Altium mide un exceso de 2241,311 mil (unos 57 mm). | Ethernet es de 10 Mb/s (ENC28J60). Corregirlo obligaba a mover pistas ya cerradas y arriesgar otras reglas (D-48, D-52, D-70). |
| **Par diferencial TPIN** (1) | Igual que el anterior, con un exceso de 1289,334 mil (unos 33 mm). | Mismo motivo. |

Las cifras de los pares son lo que sobra respecto a lo permitido, no el largo total del par.

### Serigrafía: 136 + 140 avisos
La serigrafía es la tinta blanca de rótulos y contornos de la placa.
- **136, serigrafía contra serigrafía:** dos textos o dibujos se pisan.
- **140, serigrafía contra máscara:** un texto cae sobre un pad sin barniz y la tinta no se imprime ahí.

No afecta al cobre ni al funcionamiento ni a la fabricación; en el peor caso, un designador queda poco legible. No se trabajó (D-71).

**En una frase:** la placa cumple todas las reglas de cobre y de fabricación; quedan 3 excepciones eléctricas justificadas y avisos de legibilidad de la serigrafía que no afectan al funcionamiento.

