# NEXUS32 — Estado de la entrega (5/11/2026)

Describe lo que está hecho, lo que no y por qué, con las cifras medidas. **Desde el 5/11 el resultado del DRC final (sección 3b) es el congelado:** el archivo de diseño no cambió desde el 3/11 13:11:53 y el paquete `NEXUS32_ENTREGA` se arma a partir de él. Las cifras de las secciones 3 y 4 son del 3/11 y siguen vigentes porque el archivo es el mismo (hash comprobado el 5/11). Las carpetas del paquete conservan el nombre `BORRADOR_2026-11-03` porque así se generaron.

## 1. Referencia

- Archivo de diseño: `NEXUS32\NEXUS32.PcbDoc`, guardado el 3/11/2026 13:11:53, SHA-256 `63744a39…`. El `.PrjPcb` no se ha guardado desde el 28/10 (AE76FDDB…).
- 4 capas (Top, L2_GND, L3_PWR, Bottom), 159,995 × 119,990 mm, 220 componentes, 128 redes, 658 pines con pad y red correctos (686 pads), 50 reglas de diseño, todas habilitadas.
- Calendario: congelación el **miércoles 4/11 a las 18:00**; entrega el jueves 5/11 a las 23:59 [SUPUESTO: la fecha real no está confirmada].

## 2. Fases del plan (`planificacion\04_PLAN_CONSTRUCCION.md`)

| Fase | Estado | Evidencia y reservas |
|---|---|---|
| F0 Respaldo y entorno | Completa | `_respaldos\` con un respaldo por lote; checkpoints de Altium |
| F1 Decisiones y datos | Completa con reservas | Las hojas de datos del DS1307 (Analog Devices) y del MP1584 rev. 1.1 no se pudieron leer [PENDIENTE: A1, Fase 1]. Las preguntas P-02 a P-09, P-11, P-12 y P-13 al docente siguen abiertas y se usa el valor por defecto de cada una (`05_PREGUNTAS_DOCENTE.md`) |
| F2 Librerías | Completa | 44 símbolos y 38 huellas con STEP; desviaciones D-01 a D-13 |
| F3 Esquemáticos y ERC | Completa | 12 hojas jerárquicas cerradas el 28/10. ERC: 55 avisos justificados uno a uno (D-18, `Q4_erc.md`); «0 errores» sin confirmar en el panel Messages |
| F4 PCB: apilado, contorno, agujeros, zonas, colocación | Completa | Colocación R3c aplicada y R4 aprobada el 30/10; D-30 |
| F5 Ruteo | Completo con excepciones | Solo SQW sin rutear; 3 violaciones de `DiffPairsRouting`; 4 conexiones de pads sin chapar (ver sección 4) |
| F6 DRC, DFM y 3D | **En curso** | Medición de hoy hecha (sección 3). Faltan el DRC final del miércoles 10:00 y la revisión visual 3D de D5, D10 y D11 |
| F7 Entregables JLCPCB | **Borrador** | `NEXUS32\Manufacturing\BORRADOR_2026-11-03\`; el paquete final se genera tras el congelado |

## 3. Cifras medidas hoy

| Comprobación | Resultado |
|---|---|
| DRC completo con todas las reglas salvo las dos de serigrafía (43 s) | **16 violaciones**: `DiffPairsRouting` 3 y `UnRoutedNet` 13 (SQW y los pads sin chapar de D-58). Igual que la suma de los 12 grupos |
| DRC por grupos: separaciones, cortos, anchos, vías y agujeros, Component Clearance, antenas, astillas de máscara, polígonos y planos | 0 |
| Serigrafía contra serigrafía / contra máscara | **136 / 140** (antes de S2: 199 / 437) |
| `P2_redes` | 128 redes, 658 de 658 pines, 0 con red distinta, solo SQW sin rutear |
| `F5_verificar_gui.py` | 14 de 14 |
| `pcb_check_3d` | PASS: 193 de 220 con cuerpo 3D; sin cuerpo MH1–MH4 y TP1–TP23; aviso de orientación en D5, D10 y D11 |
| `pcb_check_ground` | FAIL falso positivo: no modela los planos |
| `pcb_check_dfm` | FAIL con dos falsos positivos conocidos: anillo en MH1–MH4 (NPTH) y 21 separaciones en pads redondos modelados como rectángulos |
| Gerber del borrador | 11 archivos, formato 4:4 en mm, contorno 159,9946 × 119,9896 mm; `GBO` y `GBP` vacíos (sin serigrafía ni pasta en Bottom, ningún pad SMD en Bottom); `GTS` 690 aperturas = 690 pads; `GTP` 630 aperturas = 630 pads SMD de Top, de los que 23 son puntos de prueba. `GTO` tiene 102 coordenadas fuera del contorno: 9 designadores (MH1, MH2, MH4, U6, J2, J4, J5, J8, J9) |
| Taladros | 429 agujeros: 369 vías, 4 ranuras, 48 redondos chapados y 8 sin chapar (coincide con la placa) |
| BOM_JLCPCB.csv (regenerado 3/11 tarde) | 81 líneas y 193 componentes; **81 líneas con código LCSC (193 componentes), 0 [PENDIENTE]**. 4 líneas (R1, R2, R3 y 43,2 kΩ, todas al 0,1 %) llevan una pieza con stock menor que 10 000 por decisión del estudiante (D-68); resolución línea a línea en `planificacion\evidencia\F5_bom_resolucion_2026-11-03.csv` |
| CPL_JLCPCB.csv | 193 componentes, mm y grados, sin editar rotaciones; sin MH ni TP |
| Stock y precio de JLCPCB (3/11 tarde, 73 códigos) | 52 extendidos (2 preferidos) y 21 básicos; ninguno con stock 0; 3 con bajada de ≥ 50 % desde el 25/10: MCP3208-CI/SL (121), ENC28J60-I/SO (297) y TPS26600PWPR (3395); 15 códigos con stock < 10 000 (11 de la tabla de F1 y los 4 de D-68). Detalle en `F5_stock_jlc_2026-11-03_analisis.csv` |
| **Coste informativo [SUPUESTO]** | **No es un presupuesto: la entrega son archivos.** Supuestos: 5 placas; un cargo de 3,07 USD por código distinto de pieza extendida (la página de JLCPCB no dice si es por código ni cómo trata las preferidas); precio por tramo de cantidad del catálogo del 3/11. Con 52 extendidas: 159,64 USD de cargo; piezas 204,44 USD (40,89 por placa), con las 81 líneas con código. Detalle en `F5_bom_costes_2026-11-03.csv` |

## 3b. DRC final del 5/11 (13:12 a 13:30, solo lectura, puente reiniciado por el estudiante)

| Comprobación | Resultado |
|---|---|
| Archivo | `NEXUS32.PcbDoc` 63744a39… (3/11 13:11:53) y `NEXUS32.PrjPcb` ae76fddb…, sin cambios antes y después; vigilante de hash activo |
| Proyecto enfocado y documento activo | `NEXUS32.PrjPcb` (16 documentos) y `NEXUS32.PcbDoc`; el estudiante cerró `DISPOSABLE_TEST` |
| `F5_verificar_gui.py` | 14 de 14 |
| ERC (`proj_compile` + `proj_run_erc`) | **0 errores y 55 avisos**, el mismo reparto documentado el 28/10 (D-18, `Q4_erc.md`): 2 floating input pins (U1-6, U1-7), 34 no driving source, 17 off-grid, 2 unconnected (U1-6, U1-7). 0 sin explicar |
| Esquemático contra PCB | 220 componentes en cada lado, `in_sync` verdadero, 0 extra; 128 redes iguales; 658 pines con la misma red, 0 distintos (`F5_sch_vs_pcb.py`) |
| PCB contra el diseño (`P2_redes.py`) | 128 redes, 686 pads, 658 de 658 pines con red correcta; solo SQW sin rutear |
| BOM del diseño contra `BOM_JLCPCB.csv` | 193 designadores, 0 diferencias de valor o huella; los 27 que solo están en el diseño son MH1–MH4 y TP1–TP23 (excluidos a propósito, D-27 y D-09) |
| DRC completo sin las 2 reglas de serigrafía (52 s) | **16 violaciones**: `DiffPairsRouting` 3 y `UnRoutedNet` 13, idénticas una a una a las del 3/11 |
| DRC por 12 grupos | separaciones, cortos, anchos, vías y agujeros, Component Clearance, antenas, astillas, polígonos y planos: **0**; `UnRoutedNet` 13; resto (`DiffPairsRouting`) 3; serigrafía contra serigrafía **136** y contra máscara **140** (igual que el 3/11). La suma sin serigrafía (16) coincide con el DRC completo. Reglas restauradas: 50, sin diferencias |
| `pcb_check_3d` | PASS: 193 de 220 con cuerpo 3D; sin cuerpo MH1–MH4 y TP1–TP23; aviso de orientación en D5, D10 y D11 (la revisión visual 3D sigue pendiente de la GUI) |
| `pcb_check_ground` | FAIL falso positivo conocido: no modela los planos (GND «152 trozos») |
| `pcb_check_dfm` | FAIL: anillo en MH1–MH4 (no chapados) y J8.S1, y **63 separaciones** (el 3/11 se anotaron 21). **Falsos positivos comprobados:** los ejemplos «pista SCK / vía (2872,3848)», «INT_ENC / vía (3966,784)» y «SDA / vía (2738,4046)» no existen: en esas vías solo hay pistas de la propia red de la vía (`pcb_query`), y el DRC de Altium da 0 en separaciones y cortos. **[PENDIENTE]** no se explicó por qué el recuento pasó de 21 a 63 |
| Serigrafía (paso 9 y 9b del plan) | No se repitió la copia del panel Messages: mismo archivo y mismos recuentos 136 y 140; el residuo sigue descrito en D-64 |

## 4. Excepciones aceptadas (detalle en `DESVIACIONES.md`)

| # | Excepción | Cifra |
|---|---|---|
| D-46 | SQW sin conectar (U8.7 a U6.4); jumper manual y nota en el README final | 1 conexión |
| D-48 | Pares Ethernet: TPOUT acoplado con meandro; TPIN sin acoplar | 2 violaciones de `DiffPairsRouting` |
| D-52 | Condición para generar las salidas | — |
| D-56 | DRC por grupos (el completo se corta en 500) y regla de vías tapadas | — |
| D-57 | Par USB sin acoplar | 655,65 mil frente a 500 |
| D-58 | GND de pads sin chapar (J8-S1 ×2, J6-SH ×2) | 4 conexiones y 8 avisos |
| D-59 | Incidente de J3 (cerrado) | — |
| D-60 | `audit_find_signal_vias_without_return` marca 2 de 3 vías de USB_DN | — |
| D-61 | Máscara 0 mil en J8, U1, U10, U11 y U14 | astillas 28 → 0 |
| D-62 | Autoplace de designadores aceptado | 92 movidos; 41 más cerca de otro componente |
| D-63 | `SilkToSolderMaskClearance` a 5,91 mil | 341 → 143 → 140 |
| D-64 | Residuo de serigrafía contra máscara | 140 filas: 57 designadores (126) y 14 arcos de marca de pin 1 |
| D-65 | Pasta en los 23 puntos de prueba | se queda; solo los estaña, siguen accesibles a la sonda |
| D-66 | 9 designadores fuera del contorno (MH1, MH2, MH4, U6, J2, J4, J5, J8, J9) | residuo estético aceptado; efecto en JLCPCB [PENDIENTE] |
| D-67 | D1, D2, D4 = SS56, C18199169 | tabla de trazabilidad alineada con D-19; extendida y preferida |
| D-68 | Colores de LED y 31 líneas de BOM resueltas por catálogo | 4 líneas al 0,1 % con umbral de stock bajado; D5 puede verse tenue (hasta 0,9 mA) |
| D-69 | C4 y C5 (10 µF 1210) de 50 V | el doble de 27 V no se alcanza; el SMBJ28A recorta en 45,4 V; efecto de la polarización sobre la capacidad [SUPUESTO] |

## 5. Lo que sigue pendiente y por qué

| Pendiente | Por qué | Dueño |
|---|---|---|
| Polaridad de BT1 | Se comprueba en la vista previa de JLCPCB cuando haya CPL | Estudiante |
| Fecha real de entrega | No confirmada | Estudiante |
| Que el módulo OLED admita 3,3 V | El pin 2 de J3 entrega V3V3; el orden de pines ya está cerrado (P-10) | Estudiante |
| Stock de las 4 líneas de 0,1 % (D-68) | Piezas con 7132, 9803, 4732 y 5419 unidades; repetir la consulta el día del pedido | Estudiante |
| Rotaciones y posiciones del CPL | Solo se corrigen en el CPL tras la vista previa de JLCPCB | M2, con las capturas del estudiante |
| Gerber y STEP sin abrir en un visor | Solo hay chequeo offline | GUI |
| D5, D10 y D11 en 3D | `pcb_check_3d` avisó de su orientación | GUI |
| DRC final completo | Miércoles 4/11 a las 10:00 | M2 y GUI |
| Efecto de la serigrafía sobre pads | La página de JLCPCB no dice qué hace con ella (D-64) | [PENDIENTE] |
| Serigrafía fuera del contorno | 9 designadores quedan fuera; JLCPCB no lo documenta | [PENDIENTE] |
| Limpieza de la carpeta del proyecto | Hoy hay `History\`, `Project Logs…`, `Project Outputs…`, `NEXUS32.canvas.json` y 7 hojas `NEXUS32__*.SchDoc` sueltas (REQ-069) | M2, al empaquetar |
| Abrir el proyecto desde una copia | Sin comprobar (REQ-082) | M2, al empaquetar |
| Pedido y revisión de ingeniería de JLCPCB | Requiere la cuenta del estudiante (REQ-095) | Estudiante |
| Preguntas P-02 a P-09, P-11 a P-13 | Sin respuesta del docente | Estudiante |
| Nota del jumper de SQW, nota de J3 y advertencia de los relés (≤ 30 VDC y 1 A, D-50) en el README final | Se redactan con el paquete final | M2 |
| Captura lateral de J4 y J5 | Su orientación es [SUPUESTO] | Estudiante |

## 6. Cumplimiento de requisitos

El estado de los 108 requisitos está en `CUMPLIMIENTO_REQUISITOS.md` y `.csv`: **9 CUMPLE**, **9 CUMPLE CON EXCEPCIÓN**, **64 EN DISEÑO** (implementados según los documentos de F1 a F3, **no re-verificados hoy**), **15 PARCIAL**, **9 ABIERTO**, **1 NO COMPROBADO** y **1 RESUELTO**. Los requisitos que decían «condicionado a P-10»: REQ-005, 028 y 052 cerrados en esa condición, porque solo dependían del orden de pines; REQ-053, 076, 097, 098 y 101 siguen abiertos por la ubicación o las cotas del módulo OLED (REQ-053 también por los 3,3 V).

## 7. Qué no es este documento

No es la memoria del proyecto, no sustituye al README del paquete final y no declara la placa «lista para fabricar». La decisión de fabricar depende del DRC final, de la revisión visual de las salidas y del pedido del estudiante.

## Actualización A3 (6/11 a 7/11/2026) - solo en la copia `A3_TRABAJO` y en `NEXUS32_ENTREGA_A3`

Esta sección no cambia lo anterior: `NEXUS32` y `NEXUS32_ENTREGA` conservan las cifras de arriba. Describe la copia de trabajo A3: PcbDoc `86DC6825B9CC13E3...` (guardado el 6/11 a las 16:54:24), `NEXUS32.PrjPcb` `AE76FDDB...` sin cambios. **Solo vale si el estudiante decide usar esta copia; la entrega vigente sigue siendo `NEXUS32_ENTREGA`.** Las salidas de fabricación de esta placa están en `Manufacturing\A3_2026-11-07` (ver su `README.md`) y el paquete completo con manifiesto SHA-256 en `NEXUS32_ENTREGA_A3`.

### DRC por grupos con las 50 reglas habilitadas (7/11, 20:00-20:10)

| Medida | Entrega (3/11) | Copia A3 (7/11) |
|---|---|---|
| Clearance, cortos, anchos, vías y agujeros, Component Clearance, `NetAntennae`, astillas de máscara, polígonos y planos | 0 | **0** |
| `UnRoutedNet` | 13 | **1** (SQW) |
| `DiffPairsRouting` | 3 (USB, TPOUT, TPIN) | **2** (TPOUT, TPIN) |
| **Violaciones eléctricas** | **16** | **3** |
| Serigrafía (silk-silk / silk-máscara) | 136 / 140 | 136 / 140 (**sin trabajar**, D-71) |
| `P2_redes` | 128 redes, 658 de 658 | 128 redes, 658 de 658, 0 con red distinta |
| Componentes esquemático / PCB; reglas | 220 / 220; 50 | 220 / 220; 50 |

### Las 3 excepciones eléctricas

| Excepción | Medida | Dónde |
|---|---|---|
| SQW (U6.4 a la vía de SQW) sin conectar; única ruta legal = vía dentro del pad, prohibida por D-43; jumper manual | 1 fila `UnRoutedNet` | D-46, `FASE1_SQW.md` |
| TPOUT (Ethernet) sin acoplar | 2241,311 mil sin acoplar (máximo 500) | D-48, D-52, D-70 |
| TPIN (Ethernet) sin acoplar | 1289,334 mil sin acoplar (máximo 500) | D-48, D-52, D-70 |

El par USB (655,652 mil en la entrega) quedó acoplado en la ronda 2 y ya no es excepción. Para TPOUT y TPIN faltan unos 1741 y 789 mil por acoplar y no hay forma de lograrlo sin mover piezas (J6, D6, R47-R50); el estudiante confirmó que se queden como excepción medida.

### Qué cambió de diseño respecto a la entrega

1. Los 4 pads sin chapar (J8-S1 ×2, J6-SH ×2) pasaron a llamarse `0` y quedaron sin red, en el PcbDoc y en la librería de la copia (`NEXUS32.PcbLib` `ECDE3256`): `UnRoutedNet` 13 -> 1.
2. Par USB: un tramo acoplado de 176 mil a 10,000 mil de hueco (`DiffPairsRouting` 3 -> 2, `NetAntennae` sigue en 0).
3. Nada más: ninguna pieza se movió; la serigrafía y los 14 arcos de pin 1 no se tocaron (D-71); BOM y CPL salen idénticos byte a byte a los de la entrega.

**Pads `J6.0` y `J8.0` (2 + 2) sin pin en el esquemático.** `P2_redes` y `F5_sch_vs_pcb.py` muestran por eso 2 avisos «pad sin pin en el símbolo» (J6 y J8). Es lo esperado. Una futura sincronización (ECO, Update PCB, Update From PCB Libraries) no debe «arreglarlo»: ver el aviso de D-58 en `DESVIACIONES.md`.

### Paquete de fabricación A3

`Manufacturing\A3_2026-11-07`: 11 Gerber, 3 archivos de taladros (429 agujeros, idénticos al borrador), IPC-D-356A (en pulgadas con resolución de 0,1 mil, a diferencia del borrador en mm), STEP, `BOM_JLCPCB.csv`, `CPL_JLCPCB.csv` y el ZIP de subida. Los Gerber difieren de los del borrador solo en GTL (par USB) y GP1 (4 coronas de aislamiento en los pads sin red). Comprobaciones en `Manufacturing\A3_2026-11-07\Verificacion`. Pendiente, como en el borrador: ver los Gerber y el STEP en un visor, y las rotaciones del CPL en la vista previa de JLCPCB.
