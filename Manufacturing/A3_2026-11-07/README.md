# NEXUS32 — Paquete de fabricación A3 (7/11/2026)

Salidas de fabricación regeneradas desde la placa de la copia de trabajo A3 (`NEXUS32.PcbDoc` SHA-256 `86dc6825b9cc13e311113e57b5a9fb231b72487856be0da0d28ad1612706c4d1`, guardado el 6/11/2026 16:54:24). **Sustituye a `Manufacturing\BORRADOR_2026-11-03`, que corresponde a la placa original (`63744a39…`).** Este paquete solo vale si el estudiante decide usar la copia A3; la entrega vigente sigue siendo `NEXUS32_ENTREGA` hasta que lo decida. Nada se ha subido a JLCPCB.

## 1. Archivo de diseño de referencia

- 4 capas (Top, L2_GND, L3_PWR, Bottom), 160 × 120 mm, 220 componentes, 128 redes, 658 de 658 pines con la red correcta, 50 reglas habilitadas (sin cambios respecto al borrador).
- `NEXUS32.PrjPcb` SHA-256 `ae76fddb…` (sin cambios). `Libraries\NEXUS32.PcbLib` `ecde3256…` (los 4 pads sin chapar se llaman `0`, sin red).
- Lo que Altium tenía **en memoria** al exportar es idéntico a lo guardado: 2865 pistas, 369 vías y 60 arcos sin una sola diferencia (`Verificacion\memoria_vs_disco.txt`).
- Cambios de diseño respecto al borrador: (a) los 4 pads sin chapar J6-SH ×2 y J8-S1 ×2 pasaron a llamarse `0` y quedaron sin red (Fase 2); (b) el par USB quedó acoplado (Fase 1 de la ronda 2: un tramo de 176 mil a 10,000 mil de hueco). Nada más.

## 2. DRC por grupos con las 50 reglas (7/11, 20:00–20:10, `Verificacion\DRC_por_grupos_2026-11-07.json`)

| Grupo | Borrador (placa original, 3/11) | A3 final (7/11) |
|---|---|---|
| Clearance, cortos, anchos, vías y agujeros, Component Clearance, `NetAntennae`, astillas de máscara, polígonos y planos | 0 | **0** |
| `UnRoutedNet` | 13 | **1** (SQW) |
| `DiffPairsRouting` | 3 (USB, TPOUT, TPIN) | **2** (TPOUT 2241,311 mil y TPIN 1289,334 mil; máximo 500) |
| **Violaciones eléctricas** | **16** | **3** |
| Serigrafía contra serigrafía / contra máscara | 136 / 140 | 136 / 140 (**sin trabajar, D-71**) |

Las 50 reglas quedaron habilitadas antes y después (sin diferencias de «enabled»). El DRC devuelve como mucho 100 filas por grupo; en serigrafía los recuentos 136 y 140 son los totales (`violation_count`).

## 3. Excepciones que acompañan a este paquete (detalle en `Documentation\DESVIACIONES.md`)

| # | Excepción | Medida |
|---|---|---|
| D-46 | SQW sin conectar: U6.4 (pad 1821, 4261 mil) a la vía de SQW; la única ruta legal es una vía dentro del pad, que D-43 prohíbe. Se deja el jumper manual | 1 fila `UnRoutedNet` |
| D-48, D-52, D-70 | Pares Ethernet TPOUT y TPIN sin acoplar lo exigido por `DiffPairsRouting` (hueco 10 mil, máximo 500 mil sin acoplar); medidos, sin solución sin mover piezas | TPOUT 2241,311 mil; TPIN 1289,334 mil |
| D-71, D-64 | Serigrafía: 57 designadores contra pads vecinos y 14 arcos de pin 1; **no se trabajó** (decisión del estudiante, 7/11); no hay designadores ocultos | 136 + 140 filas |
| D-58 | J6.0 ×2 y J8.0 ×2 (pads sin chapar, sin red): **no tienen pin en el esquemático**; una sincronización futura (ECO) no debe «arreglarlo» | 4 pads |

El par USB deja de ser excepción: su tramo sin acoplar pasó de 655,652 mil a dentro del límite (D-57 queda resuelta en esta copia).

## 4. Salidas y su comprobación

Gerber, taladros, IPC-356 y STEP los exportó el estudiante por la GUI de Altium el 7/11 entre las 20:20 y las 20:24, sobre la placa A3 sin guardar nada (PcbDoc y PrjPcb conservan su hash). BOM y CPL se generaron sin GUI. Las comprobaciones son offline y de solo lectura; los resultados completos están en `Verificacion\comprobaciones_salidas.txt`.

| Salida | Carpeta | Resultado de la comprobación |
|---|---|---|
| Gerber, 11 archivos (GTL, GBL, GP1, GP2, GTS, GBS, GTO, GBO, GTP, GBP, GM) | `Gerber\` | Todos en formato 4:4 y mm (`F5_gerber_check.py`); contorno `GM` de 159,9946 × 119,9896 mm; destellos idénticos al borrador (GTS 690 = 690 pads; GTP 630 = 630 pads SMD de Top, 23 de ellos puntos de prueba, D-65). Comparados **por contenido** con los del borrador (Altium renumera los D-codes): GBL, GP2, GTS, GBS, GTO, GBO, GTP, GBP y GM sin ninguna diferencia. **Solo difieren dos capas, y solo en lo que cambió el diseño:** GTL (4 trazos del par USB sustituidos por 5, en x 13,1–20,1 mm, y 98,5–100,4 mm) y GP1 (4 alivios térmicos de J6-SH ×2 y J8-S1 ×2 pasan a coronas de aislamiento, porque esos pads ya no tienen red GND). GTO sigue con 102 coordenadas fuera del contorno (D-66). **No se han abierto en un visor de Gerber** [PENDIENTE] |
| Taladros (NC Drill, plated, non-plated y ranuras) | `Taladros\` | **429 agujeros**: 369 vías (0,305 mm) + 4 ranuras de 0,61 mm + 48 agujeros redondos chapados + 8 NPTH (4 de 3,2 mm, 2 de 3,25 mm, 2 de 0,61 mm). Los tres `.TXT` y el `.DRR` son **idénticos línea a línea** a los del borrador |
| IPC-D-356A | `IPC356\` | Comparado con el del borrador tras pasar ambos a mm: de los 690 registros de pad, 686 son idénticos en posición, red, agujero y cara; los otros 4 son los pads renombrados (`SH`/`S1` → `0`: misma posición, red GND → N/C, sin chapar). Las 369 vías coinciden todas (0 diferencias; desplazamiento máximo 0,0004 mm). **Diferencia de unidades:** el borrador salió en milímetros (`UNITS CUST 1`) y este sale en pulgadas con resolución de 0,1 mil (`UNITS CUST`); ambas son válidas en IPC-D-356A. Si se quiere en mm, repetir el informe cambiando las unidades en su cuadro. El `.REP` repite el aviso «Pad name A1B12 / B1A12 too long» del borrador (nombres de pad de J6) |
| IPC-356, `.csv` | `IPC356\` | **60 bytes = solo la cabecera** (`"Net","Name","X Coord","Y Coord","Side","Hole Size","Type"`), igual que en el borrador: lista los pads o vías marcados como *Fabrication Testpoint* en sus propiedades, y en esta placa no hay ninguno marcado (TP1–TP23 son componentes, no pads con esa propiedad). No es un error |
| STEP | `3D\NEXUS32.step` | 53 553 361 bytes, cabecera Open CASCADE, 1 116 252 líneas (las mismas que en el borrador). Difiere del borrador en la marca de tiempo, los nombres internos de producto y la numeración de entidades a partir de la línea 931 883; no se rastreó pieza por pieza. **No abierto en un visor** [PENDIENTE]; el aviso de orientación de D5, D10 y D11 del `pcb_check_3d` del borrador no se volvió a medir con esta placa |
| BOM | `JLCPCB_SUBIDA\BOM_JLCPCB.csv` | Regenerado desde el BOM del proyecto A3: 81 filas, 193 designadores (sin MH1–MH4 ni TP1–TP23), 0 inconsistencias de huella o comentario; códigos LCSC heredados por designador (D-19, D-67, D-68). **Idéntico byte a byte** al del borrador porque no cambió ningún componente. No se reconsultó stock ni precio en JLCPCB |
| CPL | `JLCPCB_SUBIDA\CPL_JLCPCB.csv` | Regenerado leyendo el `Components6` del PcbDoc: 193 piezas, mm con 4 decimales y grados, sin editar rotaciones. Coincide con `pcb_export_coordinates` del puente (0 diferencias) y es **idéntico byte a byte** al del borrador (no se movió ninguna pieza). Rotaciones y polaridad de BT1: siguen [PENDIENTE] de la vista previa de JLCPCB; los ajustes irán solo en `CPL_CORRECCIONES.md` |
| ZIP de subida | `JLCPCB_SUBIDA\NEXUS32_Gerber_Taladros_A3.zip` | Los 11 Gerber y los 3 `.TXT` de taladros (14 archivos), con el mismo contenido que los sueltos (SHA-256) |

**Ajustes de la GUI (informe del estudiante).** El cuadro de Gerber de Altium 26 no tiene las pestañas General/Layers/Apertures del texto de partida sino «Layers to plot» y «Advanced»: estaban marcadas las 11 capas, mm y 0,0001 mm (4:4), y no existe «Embedded apertures». En taladros se cambió a coordenadas absolutas con archivos separados de chapados y no chapados; apareció un cuadro «Import Mill/Route Data» que se aceptó sin cambios. En el informe de puntos de prueba se marcaron CSV e IPC-D-356A y no el texto. Los archivos resultantes cumplen lo que se pedía (4:4 en mm, 11 capas, 429 agujeros idénticos al borrador).

## 5. Qué no incluye

- Serigrafía sin trabajar y arcos de pin 1 sin tocar (Fases 4 y 5 canceladas por el estudiante, D-71).
- Ningún PDF ni imagen de esquema o placa.
- Los archivos auxiliares de Altium (`.apr`, `.APR_LIB`, `.EXTREP`, `.REP` de Gerber, `Status Report.Txt`, informes DRC) quedaron en `A3_TRABAJO\NEXUS32\Project Outputs for NEXUS32` y no forman parte del paquete.
- El hash de cada archivo de la entrega está en `MANIFIESTO.csv` (SHA-256); `MANIFIESTO.md` lista lo excluido y por qué.
