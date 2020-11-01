TFM Mercadona: Detecccion de fallos en muebles frigorificos


![Logo](Images/mercadona_logo.png)






## Tabla de contenido

1. [Objetivo del proyecto](#Objetivo)
2. [Arquitectura](#Arquitectura)
3. [Base de datos](#datasets)
4. [Software & librerias utilizadas](#software)
    1. [Lista de Open Datasets](#opendata)
    2. [Datasets Propios](#customdata)
5. [Resultados](#resultados)

## Objetivo del proyecto <a name="Objetivo"></a>

Prescribir la arquitectura que permita el tratamiento de más de 3 TB de datos. 

Descripción de Variables obtenidas de un mueble frigorífico y central de frio.

Prueba de Concepto (POC) para viabilidad de un mantenimiento predictivo con histórico de 2 años.


## Arquitectura <a name="Arquitectura"></a>

Se utilizo Google Cloud Platform:

<img src="./Images/GCP.png" width="75%"><br/>

![Jupiter](Images/GCP2.png)


## Base de datos utilizadas <a name="datasets"></a>

Se utilizaron 3 bases de datos:

1. Alarmas :
Base de datos de alarmas de todos los centros con los siguientes campos:

| Field name|IDALARM|  TS      | TYPE     |DESCRIPTION| ELEMENT |  PARENT |TEMPLATE|CENTRO | NAME | NAME_ZONA|
| ----------|-------| ------- | -------- |-----------| --------|---------|------- |-------|------|----------|
| Type      |INTEGER|TIMESTAND|  STRING  |  STRING   |INTEGER  | INTEGER |STRING  |STRING |STRING |STRING   |


2. Variables (Telemetria de sensores de Muebles Frigorificos) :
Base de datos de la telemetria de los distintos muebles frigorificos obtenidos por controladores RX600 con siguientes campos:

| Field name|TS|ELEMENT| TAG_SONDA_PB1|TAG_SONDA_PB2|TAG_PRESION_SATURACION|TAG_TEMP_ASPIRACION|TAG_RECALENT_VALVULA|TAG_APERT_VALVULA |TAG_EQUIPO_STANDBY|TAG_PETICION_FRIO|TAG_COMUNICA|TAG_DESCARCHE|TAG_ALARMA|
| ----------|-------| ------- | -------- |-----------| --------|---------|------- |-------|------|----------|------|------|------|
| Type      |TIMESTAND|INTEGER|  FLOAT  |  FLOAT   |FLOAT  | FLOAT |FLOAT  |FLOAT |FLOAT |FLOAT   |FLOAT   |FLOAT   |FLOAT   |








## Authors
- Luis Araujo
- Miguel Ángel Aguilar
- Rocío Cort
- Victor Malvar









