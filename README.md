TFM Mercadona: Detecccion de fallos en muebles frigorificos


![Logo](Images/mercadona_logo.png)






## Tabla de contenido

1. [Objetivo del proyecto](#Objetivo)
2. [Arquitectura](#Arquitectura)
3. [Base de datos](#datasets)
4. [librerias utilizadas](#software)
5. [Notebooks utilizados](#Notebooks)

## Objetivo del proyecto <a name="Objetivo"></a>


Prescribir la arquitectura que permita el tratamiento de más de 3 TB de datos. 

Descripción de Variables obtenidas de un mueble frigorífico y central de frio.

Prueba de Concepto (POC) para viabilidad de un mantenimiento predictivo con histórico de 2 años.


## Arquitectura <a name="Arquitectura"></a>

Se utilizo Google Cloud Platform:

<img src="./Images/GCP.png" width="75%"><br/>

![Jupiter](Images/GCP2.png)


## Base de datos utilizadas <a name="datasets"></a>


Se utilizaron 5 bases de datos:

    1. Historico de alarmas
    2. Telemetría de muebles frigoríficos
    3. Telemetría de central de frío
    4. Base de datos relacionales Zonas, centros, Tiendas y elementos
    5. Base de datos relacionales de Tiendas y sus coordenadas

1. PIC_TRACK_ALARM_ENRICH :
Base de datos de alarmas de todos los centros

| Field name|IDALARM|  TS      | TYPE     |DESCRIPTION| ELEMENT |  PARENT |TEMPLATE|CENTRO | NAME | NAME_ZONA|
| ----------|-------| ------- | -------- |-----------| --------|---------|------- |-------|------|----------|
| Type      |INTEGER|TIMESTAND|  STRING  |  STRING   |INTEGER  | INTEGER |STRING  |STRING |STRING |STRING   |


2. PIC_TRACK_VARIABLES_PREP (Telemetría de sensores de Muebles Frigorificos):
Base de datos de la telemetría de los distintos muebles frigoríficos obtenidos por controladores RX600

| Field name|TS|ELEMENT| TAG_SONDA_PB1|TAG_SONDA_PB2|TAG_PRESION_SATURACION|TAG_TEMP_ASPIRACION|TAG_RECALENT_VALVULA|TAG_APERT_VALVULA |TAG_EQUIPO_STANDBY|TAG_PETICION_FRIO|TAG_COMUNICA|TAG_DESCARCHE|TAG_ALARMA|
| ----------|-------| ------- | -------- |-----------| --------|---------|------- |-------|------|----------|------|------|------|
| Type      |TIMESTAND|INTEGER|  FLOAT  |  FLOAT   |FLOAT  | FLOAT |FLOAT  |FLOAT |FLOAT |FLOAT   |FLOAT   |FLOAT   |FLOAT   |

3. PIC_TRACK_VARIABLES_PREP_CENTRALFRIO (Telemetria de sensores de la central frigorífica):
Base de datos de la telemetría de la central frigorífica


| Field name|TS|  ELEMENT| NAME_CENTRO |TAG_ALARMA| TAG_COMPRESOR1 |  TAG_COMPRESOR2 |TAG_COMPRESOR3|TAG_COMPRESOR4 | TAG_COMPRESOR5 | TAG_COMPRESOR6|TAG_COMUNICA|	TAG_CONV_SETPOINT_ASP|	TAG_CONV_SETPOINT_COND|	TAG_CONV_SONDA_ASP|	TAG_CONV_SONDA_COND|	TAG_PASOMANUAL|	TAG_PASOMANUAL_AVERIA|TAG_PASOMANUAL_MTTO|TAG_POTENCIA_COMP1|TAG_POTENCIA_COMP2|TAG_POTENCIA_COMP3|TAG_POTENCIA_COMP4|	TAG_POTENCIA_COMP5|	TAG_POTENCIA_COMP6|	TAG_POTENCIA_INVERTER|	TAG_SETPOINT_ASP|TAG_SETPOINT_COND|	TAG_SONDA_ASP|	TAG_SONDA_COND|	TAG_SONDA_TEMP_EXT|	TAG_SONDA_TEMP_SUBENF|TAG_VENTILADOR1|	TAG_VENTILADOR2|	TAG_VENTILADOR3|TAG_VENTILADOR4|TAG_VENTILADOR5|	
| ----|-----| ----- | ------ |-----| ------|------|----- |-------|------|------|------|------|------|-----------| --------|---------|------- |-------|------|----------|------|------|------|------|------|------|------|-------|------|------|------|-------|------|------|------|------|
| Type  |TIMESTAND|INTEGER|  FLOAT  |  FLOAT   |FLOAT  | FLOAT |FLOAT  |FLOAT |FLOAT |FLOAT   |FLOAT   |FLOAT   |FLOAT |  FLOAT   |FLOAT  | FLOAT |FLOAT  |FLOAT |FLOAT |FLOAT   |FLOAT   |FLOAT   |FLOAT  |  FLOAT   |FLOAT   |FLOAT   |FLOAT  |FLOAT   |FLOAT   |FLOAT   |FLOAT  |  FLOAT   |FLOAT   |FLOAT   |FLOAT  |  FLOAT  |    


4. PIC_ELEMENT_LOCATION:

Base de datos con los identificados unicos por tiendas, elementos, zonas y centros

| Field name|ID_ELEMENT	|IDENTIFIER_ELEMENT	|NAME_ELEMENT	|DESCRIPTION_ELEMENT	|ABBREVIATION_ELEMENT	|ORDERPOSITION_ELEMENT	|PARENT_ELEMENT	|TEMPLATE_ELEMENT	|TEMPLATEPROPERTIES_ELEMENT	|TYPE_ELEMENT	|ID_LOCATION	|IDENTIFIER_LOCATION	|NAME_LOCATION	|DESCRIPTION_LOCATION	|PARENT_LOCATION	|TEMPLATE_LOCATION	|TYPE_LOCATION	|	
| ----------|-------| ------- | -------- |-----------| --------|---------|------- |-------|------|----------|------|------|------|-----------| --------|---------| --------|
| Type      |INTEGER|STRING|  STRING  |  STRING   |STRING  | INTEGER |INTEGER  |STRING |STRING |STRING   |INTEGER  |STRING |STRING |STRING   |INTEGER  |STRING |STRING |



5. PIC_TIENDA_COORDENADA:

Identificador de tiendas, altitud, longitud y altura con respecto al mar

| Field name|ALTITUD|  LONGITUD| ALTURA |TIENDA  |
| ----------|-------| -------- | ------ |------  |
| Type      |FLOAT  |FLOAT     |  FLOAT |INTEGER |

## librerias utilizadas <a name="software"></a>


* **python version 3.7 ** 
* **Pandas** : librería Python para Dataframes análisis
* **Numpy** : El paquete fundamental para la informática científica con Python
* **sklearn** : Scikit-learn es una biblioteca para aprendizaje automático de software libre
* **google** : Biblioteca de cliente de Cloud Storage para Python
* **matplotlib** : Biblioteca para la generación de gráficos a partir de datos contenidos en listas o arrays 
* **seaborn** : Biblioteca de visualización de datos de Python basada en matplotlib
* **dateutil** : Biblioteca para analisis de series temporales


## Notebooks utilizados <a name="Notebooks"></a>

    1. [Descripción de Variables](#paginadescripcion)
    2. Preprocesamiento de datos (Data Cleaning)
    3. Clusterización
    4. Comparación de Modelos 




1. Descripción de Variables 

Notebook utilizacion para la descriopccion de la telemetria optenida de los muebles frigorificos y murales de frio.

Identifiamos:
Outliers
Sensores descalibrados
Variables sin información
Variables dependientes

<img src="./Images/Descripcion.png" width="100%"><br/>

2. Preprocesamiento de datos (Data Cleaning)


Procedemos a generar un dataset el cual será utilizado para el entrenamiento del algoritmo, de la siguiente forma:


Utilizamos la base de datos de las alarmas y filtramos de la siguiente manera:

<img src="./Images/df_alarm.png" width="100%"><br/>
* filtramos las alarmas que habían sido generadas por murales de carne y murales de pescados ambos con controladores RX600.  

| Field name|DESCRIPTION|  
| ----------|-------|
| Example      |Alarma Alta Temperatura en servicio  |

*filtramos el comienzo de la Alarma con la descripción Begin que cumpla la siguiente condición:    
    * No pueden haber estado inhabilitadas antes de su activación. sino la misma debería ser considerada como mantenimiento preventivo.

**Alarma Critica**
| Field name|TimeStand|TYPE|    
| ----------|-------|-------|
| Example      |1|EnabledNotif |
| Example      |2|InhibitNotif|
| Example      |3|**EnabledNotif**|
| Example      |4|Being  |
| Example      |5|End  |

**Alarma Desabilitada antes de su inicio**
| Field name|TimeStand|TYPE|    
| ----------|-------|-------|
| Example      |1|EnabledNotif |
| Example      |2|EnabledNotif|
| Example      |3|**InhibitNotif**|
| Example      |4|Being  |
| Example      |5|End  |

Dicho filtrado se realiza de la siguiente forma:


```
    for i, row  in df_Alarms.iterrows():
if (df_Alarms.loc[i,'ALTA_TEMPERATURA']) =='Begin' and  (df_Alarms.loc[i-1,'ALTA_TEMPERATURA'])!='InhibitNotif':
    df_Alarms.at[i,'new'] = 'True'
elif (df_Alarms.loc[i,'ALTA_TEMPERATURA']) =='Begin' and  (df_Alarms.loc[i-1,'ALTA_TEMPERATURA'])=='InhibitNotif':
    df_Alarms.at[i,'new'] = 'True but InhibitNotif by user'
else:
    df_Alarms.at[i,'new'] = 'False'     
``` 
* filtramos las alarmas que se hayan activado dentro de una misma locación en un rango menor a 10 días, evitando considerar alarmas críticas que no hayan podido ser reparadas.

```
    for i, row  in df_Alarms.iterrows():
        Days_before = **10**
        if i == 0:
            df_Alarms.at[i,'new2'] = 'True'
        elif (df_Alarms.loc[i,'ELEMENT']) == (df_Alarms.loc[i-1,'ELEMENT']):
            if ((df_Alarms.loc[i,'TS'])-(df_Alarms.loc[i-1,'TS'])).days >= Days_before:
                df_Alarms.at[i,'new2'] = 'True'
            else:
                df_Alarms.at[i,'new2'] = 'Too Short' 
        else:
            df_Alarms.at[i,'new2'] = 'True'  
``` 



## Authors
- Luis Araujo
- Miguel Ángel Aguilar
- Rocío Cort
- Victor Malvar









