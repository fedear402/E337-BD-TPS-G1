Vamos a eliminar las variables:
ANO4
TRIMESTRE
REGION
MAS_500
IV1_ESP
IV3_ESP
IV7_ESP
II3
II5
II6
II7_ESP
II8_ESP
IX_Mayeq10
DECIFR
IDECIFR
RDECIFR
GDECIFR
PDECIFR
ADECIFR
IPCF
DECCFR
IDECCFR
RDECCFR
GDECCFR
PDECCFR
ADECCFR

Eliminamos 5 observaciones que corresponden a 4 servicios domésticos habitando en la vivienda y 1 pensionista que habitan una vivienda para la cual la información está contada en el otro hogar del mismo CODUSU y no proveen información de varias características que no son ingresos. Son una porción tan pequeña de la muestra total por lo que conviene sacarlos (de ambas).

La variable II1 indica ambientes en el hogar mientras que la variable IV2 indica ambientes en la vivienda. Es posible que sean colineales, por lo que podemos eliminar alguna de las dos o combinarlas de alguna manera. Vamos a eliminar la variable IV2 y quedarnos con los ambientes del hogar.

Luego están las variables  IV8-IV11 que especifican a gran detalle como es el baño (la ubicacion, si es inodoro, letrina, si tiene cloaca etc). Si incluimos todos esos datos nos quedarian 27 variables con one-hot encoding teniendo en cuenta cuales se siguen de las anteriores. El problema de eso es que va a quedar sumamente 'sparse' y se podría simplificar a algo menos granular ya que es posible que el dato tan específico del baño sea una sobre-especifcación. En la literatura encontramos que se usan 4 o 5 variables sobre el baño. Por ejemplo acá: https://documents1.worldbank.org/curated/en/946321468286464228/text/363070ESW0P0890LIC00PAPA01001502007.txt, el banco mundial usa (para predecir consumo):  
(base category: without toilet)
Private toilet with sewer system/septic tank 
No private toilet with sewer system/septic tank
Private toilet with letrine or pit
No private toilet with letrine or pit

Vamos a implementar algo similar combinando categorías:

(base) IV8=2 no tiene baño / letrina
tiene dentro de la vivienda con cloaca ( IV8=1, IV9=1, IV11=1)
tiene fuera de la vivienda con cloaca ( IV8=1, IV9=2 OR IV9=3, IV11=1)
tiene dentro de la vivienda sin cloaca ( IV8=1, IV9=1, IV11=1)
tiene fuera de la vivienda sin cloaca ( IV8=1, IV9=2 OR IV9=3, IV11=2 OR IV11=3 OR IV11=4)

Eliminamos II3 ya que II3_1 provee la misma información y de manera ordinal (con 0 indicando que usa 0 habitaciones como lugar de trabajo). Lo mismo con II5 e II6 

Las tres variables en IV12 (si esta en un basural, en una villa o en una zona indundable) consideramos que pueden ser muy utiles para predecir ingresos.

La variable IX_Tot está divida IX_Men10 y IX_Mayeq10. Sería conveniente no tener todas ya que debería haber multicolinealidad perfecta si incluimos las 3. Dejamos el total y reemplazamos las divisiones con una variable que indique la proporcion de menores de 10 en la casa. IX_Men10/IX_Tot. De esa manera se capturaría también implicitamente el efecto de la cantidad de mayores de 10. Pensamos que esas dos pueden ser buenos predictores de ingresos.

Las variables de estrategias V1-V19_B pueden ser todas utiles aunque pensamos que es posible que si no respondieron el ingreso nominal tampoco hayan respondido alguna de estas ya que constituyen una descripción del ingreso y sus fuentes. Si alguien no quiere decir cuánto gana quizás tampoco quiera decir cómo lo ganó.

Las variables de organizacion ['VII1_1', 'VII1_2', 'VII2_1', 'VII2_2', 'VII2_3', 'VII2_4'] indican que miembro en particular realiza/ayuda con las tareas domesticas. Como es irrelevante las convertimos en dos dummy. La primera dummies 1 si VII1_1=96 (servicio realiza las tareas domesticas) y la otra es 1 VII2_1=96 (servicio ayuda con las tareas domesticas). Para indicar si el hogar cuenta con servicio domestico que puede servir de predictor de ingresos


---
## VARIABLES INCLUIDAS
Abajo especificamos las variables que dejamos y como las convertimos a varias dummy con one-hot encoding el nombre que tiene cada una (entre parentesis la categoria omitida)

### AGLOMERADO
32 = CABA 
(GBA)
### IV1
01 = casa
02 = departamento 
03 = inquilinato
04 = hotel_pension 
05 = local
(otro)

### IV3
01 = mosaico_baldosa_madera_cerámica_alfombra
02 = cemento_ladrillo_fijo
(ladrillo suelto / tierra)

### IV4
1=membrana / cubierta asfáltica 
2=baldosa / losa sin cubierta 
3=pizarra / teja 
4=chapa de metal sin cubierta 
5=chapa de fibrocemento / plástico 
6=chapa de cartón 
7=saña / tabla / paja con barro / paja sola 
(otro)

### IV5
1=cielorraso
(no)

### IV6
01 = agua_dentro_vivienda
02 = agua_dentro_terreno
(fuera del terreno)

### IV7
1=red
2=bomba_motor
3=bomba_manual
(OTRO)

### IV8-IV11
(base) IV8=2 no tiene baño / letrina
tiene dentro de la vivienda con cloaca ( IV8=1, IV9=1, IV11=1)
tiene fuera de la vivienda con cloaca ( IV8=1, IV9=2 OR IV9=3, IV11=1)
tiene dentro de la vivienda sin cloaca ( IV8=1, IV9=1, IV11=1)
tiene fuera de la vivienda sin cloaca ( IV8=1, IV9=2 OR IV9=3, IV11=2 OR IV11=3 OR IV11=4)

### IV12_1
1= basural
(no)

### IV12_2
1 = inundable
(no)
### IV12_3
1=villa
(no)


### II7
01 = Propietario_viv_terr
02 = Propietario_viv
03 = Inquilino
04 = Ocupante_imp
05 = Ocupante_dep
06 = Ocupante_gratuito
07 = Ocupante_dehecho
08 = sucesión
(otro)


### II4_1
1=cocina
(no)
### II4_2
1=lavadero
(no)
### II4_3
1=garage
(no)

### II8
01 = gas_red
02 = gas_tubo_garrafa 
03 = Kerosene_leña_carbón
(otro)

### II9
01 = bano_hogar
02 = bano_comp_viv
03 = bano_comp_otras
(No tiene baño)

### VII1_1
96 = servicio_realiza
(no)

### VII2_1
96 = servicio_ayuda
(no)

V1: trabajo 
V2: jubilacion
V21: aguinaldo_jubilacion
V22: retroactivo_jubilacion
V3: indemnizacion
V4: seguro
V5: apoyo_dinero_externo
V6: apoyo_material_externo
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

## VARIABLES DISCRETAS Y CONTINUAS
### II1
ambientes

### II2
ambientes_duerme

### II3_1
ambientes_trabaja

### II5_1
clg_duerme

### II6_1
clg_trabaja

### IX_Tot
miembros

### IX_Men10/IX_Tot
menores





````
# AGLOMERADO ahora es CABA y es 0 1

eph_hog['CABA'] = (eph_hog['AGLOMERADO'] == 32).astype(int)

eph_hog.drop('AGLOMERADO', axis=1, inplace=True)

  
  

####### RENOMBRAMOS LAS QUE SON SOBRE LA VIVIENDA

  

# IV1: tipo de vivienda

iv1_dummies = pd.get_dummies(eph_hog['IV1'], prefix='IV1')

iv1_dummies.rename(columns={

'IV1_1': 'IV1_casa',

'IV1_2': 'IV1_departamento',

'IV1_3': 'IV1_inquilino',

'IV1_4': 'IV1_hotel_pension',

'IV1_5': 'IV1_local',

'IV1_6': 'IV1_otro'

}, inplace=True)

eph_hog = pd.concat([eph_hog, iv1_dummies], axis=1)

eph_hog.drop('IV1', axis=1, inplace=True)

  

# IV3 suelo

eph_hog['IV3_mosaico_baldosa_madera_cerámica_alfombra'] = (eph_hog['IV3'] == 1).astype(int)

eph_hog['IV3_cemento_ladrillo_fijo'] = (eph_hog['IV3'] == 2).astype(int)

eph_hog.drop('IV3', axis=1, inplace=True)

  

# IV4: Techo de la vivienda

iv4_dummies = pd.get_dummies(eph_hog['IV4'], prefix='IV4')

iv4_categories = {

'IV4_1': 'IV4_membrana_cubierta',

'IV4_2': 'IV4_baldosa_losa',

'IV4_3': 'IV4_pizarra_teja',

'IV4_4': 'IV4_chapa_metal',

'IV4_5': 'IV4_chapa_plastico',

'IV4_6': 'IV4_chapa_carton',

'IV4_7': 'IV4_tabla_paja'

}

iv4_dummies.rename(columns=iv4_categories, inplace=True)

eph_hog = pd.concat([eph_hog, iv4_dummies], axis=1)

eph_hog.drop('IV4', axis=1, inplace=True)

  

# IV5 cielorraso

eph_hog['IV5_cielorraso'] = (eph_hog['IV5'] == 1).astype(int)

eph_hog.drop('IV5', axis=1, inplace=True)

  

# IV6 agua

eph_hog['IV6_agua_dentro_vivienda'] = (eph_hog['IV6'] == 1).astype(int)

eph_hog['IV6_agua_dentro_terreno'] = (eph_hog['IV6'] == 2).astype(int)

eph_hog.drop('IV6', axis=1, inplace=True)

  

# IV7 si tiene agua por red o no

eph_hog['IV7_red'] = (eph_hog['IV7'] == 1).astype(int)

eph_hog['IV7_bomba_motor'] = (eph_hog['IV7'] == 2).astype(int)

eph_hog['IV7_bomba_manual'] = (eph_hog['IV7'] == 3).astype(int)

eph_hog.drop('IV7', axis=1, inplace=True)

  

# IV8-IV11 resumidas en 4 como describimos en el reporte

eph_hog['IV8_IV11_tiene_dentro_vivienda_cloaca'] = ((eph_hog['IV8'] == 1) & (eph_hog['IV9'] == 1) & (eph_hog['IV11'] == 1)).astype(int)

eph_hog['IV8_IV11_tiene_fuera_vivienda_cloaca'] = ((eph_hog['IV8'] == 1) & ((eph_hog['IV9'] == 2) | (eph_hog['IV9'] == 3)) & (eph_hog['IV11'] == 1)).astype(int)

eph_hog['IV8_IV11_tiene_dentro_vivienda_sin_cloaca'] = ((eph_hog['IV8'] == 1) & (eph_hog['IV9'] == 1) & ((eph_hog['IV11'] == 2) | (eph_hog['IV11'] == 3) | (eph_hog['IV11'] == 4))).astype(int)

eph_hog['IV8_IV11_tiene_fuera_vivienda_sin_cloaca'] = ((eph_hog['IV8'] == 1) & ((eph_hog['IV9'] == 2) | (eph_hog['IV9'] == 3)) & ((eph_hog['IV11'] == 2) | (eph_hog['IV11'] == 3) | (eph_hog['IV11'] == 4))).astype(int)

eph_hog.drop(['IV8', 'IV9', 'IV10', 'IV11'], axis=1, inplace=True)

  

# IV12 zona

eph_hog['IV12_1_basural'] = (eph_hog['IV12_1'] == 1).astype(int)

eph_hog['IV12_2_inundable'] = (eph_hog['IV12_2'] == 1).astype(int)

eph_hog['IV12_3_villa'] = (eph_hog['IV12_3'] == 1).astype(int)

eph_hog.drop(['IV12_1', 'IV12_2', 'IV12_3'], axis=1, inplace=True)

  

##### AHORA LAS QUE SON SOBRE EL HOGAR

  

# II4 clg

eph_hog['II4_1_cocina'] = (eph_hog['II4_1'] == 1).astype(int)

eph_hog['II4_2_lavadero'] = (eph_hog['II4_2'] == 1).astype(int)

eph_hog['II4_3_garage'] = (eph_hog['II4_3'] == 1).astype(int)

eph_hog.drop(['II4_1', 'II4_2', 'II4_3'], axis=1, inplace=True)

  

# II7 sobre condicion de ocupacion

ii7_dummies = pd.get_dummies(eph_hog['II7'], prefix='II7')

ii7_dummies.rename(columns={

'II7_1': 'II7_Propietario_viv_terr',

'II7_2': 'II7_Propietario_viv',

'II7_3': 'II7_Inquilino',

'II7_4': 'II7_Ocupante_imp',

'II7_5': 'II7_Ocupante_dep',

'II7_6': 'II7_Ocupante_gratuito',

'II7_7': 'II7_Ocupante_dehecho',

'II7_8': 'II7_sucesión'

}, inplace=True)

eph_hog = pd.concat([eph_hog, iv1_dummies], axis=1)

eph_hog.drop('II7', axis=1, inplace=True)

  

# II8 sobre el combustible

ii8_dummies = pd.get_dummies(eph_hog['II8'], prefix='II8')

ii8_dummies.rename(columns={

'II8_1' :'II8_gas_no_tiene',

'II8_1': 'II8_gas_red',

'II8_2': 'II8_gas_tubo_garrafa',

'II8_3': 'II8_kerosene_lena_carbon',

'II8_4': 'II8_electrico'

}, inplace=True)

eph_hog = pd.concat([eph_hog, ii8_dummies], axis=1)

eph_hog.drop('II8', axis=1, inplace=True)

  

# II9: Condición sobre si el baño es compartido

ii9_dummies = pd.get_dummies(eph_hog['II9'], prefix='II9')

ii9_categories = {

'II9_0': 'II9_bano_no_tiene',

'II9_1': 'II9_bano_hogar',

'II9_2': 'II9_bano_comp_viv',

'II9_3': 'II9_bano_comp_terr',

'II9_4': 'II9_bano_comp_otro_terr'

}

ii9_dummies.rename(columns=ii9_categories, inplace=True)

eph_hog = pd.concat([eph_hog, ii9_dummies], axis=1)

eph_hog.drop('II9', axis=1, inplace=True)

  
  

# VII1 y VII1_2 para la variable de servicio domestico que inventamos

eph_hog['VII1_1_servicio_realiza'] = (eph_hog['VII1_1'] == 96).astype(int)

eph_hog['VII2_1_servicio_ayuda'] = (eph_hog['VII2_1'] == 96).astype(int)

eph_hog.drop(['VII1_1', 'VII1_2', 'VII2_1', 'VII2_2', 'VII2_3', 'VII2_4'], axis=1, inplace=True)

  
  

# Ahora renombramos las que no son binarias:

renombrar = {

'II1': 'II1_ambientes',

'II2': 'II2_ambientes_duerme',

'II3_1': 'II3_1_ambientes_trabaja',

'II5_1': 'II5_1_clg_duerme',

'II6_1': 'II6_1_clg_trabaja'

}

eph_hog.rename(columns=renombrar, inplace=True)

  
  

# estrategias

renombrar = {

'V1': 'V1_trabajo',

'V2': 'V2_jubilacion',

'V21': 'V21_aguinaldo_jubilacion',

'V22': 'V22_retroactivo_jubilacion',

'V3': 'V3_indemnizacion',

'V4': 'V4_seguro',

'V5': 'V5_apoyo_dinero_externo',

'V6': 'V6_apoyo_material_externo',

'V7': 'V7_apoyo_material_cercanos',

'V8': 'V8_alquiler',

'V9': 'V9_ganancia_sin_trabajar',

'V10': 'V10_rentas',

'V11': 'V11_beca',

'V12': 'V12_cuota_alimentaria',

'V13': 'V13_ahorros',

'V14': 'V14_prestamo_cercano',

'V15': 'V15_prestamo_externo',

'V16': 'V16_cuotas',

'V17': 'V17_venta_pertenencias',

'V18': 'V18_otro_efectivo',

'V19_A': 'V19_A_menores_10a_trabaja',

'V19_B': 'V19_B_menores_10a_pide'

}

eph_hog.rename(columns=renombrar, inplace=True)

cols = list(renombrar.values())

eph_hog[cols] = eph_hog[cols].applymap(lambda x: 0 if x == 2 else 1) # las hacemos binarias 0 1

  

pd.options.display.max_info_columns = 1000

print(eph_hog.info())
````
