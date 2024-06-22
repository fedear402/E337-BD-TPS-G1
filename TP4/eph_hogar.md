## Identificadoras
-CODUSU: Vivienda
-NRO_HOGAR: hogar dentro de la vivienda de un CODUSU

ANO4*: Año de relevamiento (no nos importa)
TRIMESTRE*: Ventana de observación (no nos importa)

## Ubicación
REGION*: one-hot
MAS_500*: one-hot
AGLOMERADO: one-hot

---
## SOBRE LA VIVIENDA (COMUNES A LOS HOGARES EN ESE CODUSU)
### TIPO (CASA, DPTO, ETC)
IV1: one-hot
IV1_Esp*:
### N_AMBIENTES de vivienda
IV2*: (99 to NaN)
### PISOS
IV3: material -> one-hot
IV3_Esp*:
### TECHO
IV4: 7 cubiertas -> one-hot (9 to NaN)
IV5: cielorraso
### AGUA
IV6: agua -> one-hot
IV7: red_bomba -> one-hot
IV7_Esp*:
### BAÑO:


### Zona
IV12_1: basural
IV12_2: inundable
IV12_3: villa

## CARACTERISTICAS DEL HOGAR

II1: ambientes_hogar (99 to NaN)
II2: para_dormir 
II3*:
II3_1: lug_trabajo 
II4_1: cocina (d)
II4_2: lavadero (d)
II4_3: garage (d)
II5*:
II5_1: duerme_clg
II6*:
II6_1: trabaja_clg
II7: tenencia (one-hot)
II7_ESP*
II8: Combustible (one-hot)
II8_Esp*
II9: tenencia_baño (one-hot)

## ESTRATEGIAS (9 to NaN)
V1 - V19_B dummies binarias

V1: trabajo 
V2: jubilacion
V21: aguinaldo_jubilacion
V22: retroactivo_jubilacion
V3: indemnizacion
V4: seguro
V5: apoyo_dinero_externo
V6: apoyo_material_externo
V7: apoyo_material_cercanos
V8: alquiler
V9: ganancia_sin_trabajar
V10: rentas
V11: beca
V12: cuota_alimentaria
V13: ahorros
V14: prestamo_cercano
V15: prestamo_externo
V16: cuotas
V17: venta_pertenencias
V18: otro_efectivo
V19_A: menores_10a_trabaja
V19_B: menores_10a_pide

### RESUMEN
IX_Tot: miembros
IX_Men10: menores_10an
IX_Mayeq10: mayores_10an


DECIL = 12 -> NO RESPUESTA
### INGRESOS TOTALES
ITF: ingreso_total_familiar 
DECIFR: t_decil_total
IDECIFR: t_decil_interior
RDECIFR: t_decil_region
GDECIFR: t_decil_ma500
PDECIFR: t_decil_me500
ADECIFR: t_decil_aglomerado

### INGRESOS PER CAPITA
IPCF: ingreso_per_capita 
DECCFR: pc_decil_total
IDECCFR: pc_decil_interior
RDECCFR: pc_decil_region
GDECCFR: pc_decil_ma500
PDECCFR: pc_decil_me500
ADECCFR: pc_decil_aglomerado

---
PONDIH: ponderador total y per capita

---

### ORGANIZACION
VII1_1
VII1_2*
VII2_1
VII2_2*
VII2_3*
VII2_4*