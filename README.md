TFM Mercadona: Detecccion de fallos en muebles frigorificos


![Logo](Images/mercadona_logo.png)



![Exercise architecture](Images/novofrio-expositor-02.jpg)


## Tabla de contenido

1. [Objetivo del proyecto](#Objetivo)
2. [Arquitectura](#Arquitectura)
3. [Software & librerias utilizadas](#software)
4. [Datasets](#datasets)
    1. [Lista de Open Datasets](#opendata)
    2. [Datasets Propios](#customdata)
5. [Resultados](#resultados)

## Objetivo del proyecto <a name="Objetivo"></a>

Prescribir la arquitectura que permita el tratamiento de mas de 3 TB de datos. 

Descripcion de Variables obtenidas de un mueble frigorifico y central de frio.

Prueba de Concepto (POC) para viabilidad de un mantenimiento predictivo con historico de 2 años.​


## Arquitectura <a name="Arquitectura"></a>

Se utilizo Google Cloud Platform:

BigQuery y Google Cloud Storage -> Almacenamiento de datos

AI Platform (Júpiter Notebooks)-> Procesamiento, Análisis y limpieza de los datos

<img src="./Images/jupiter.png" width="25%"><br/>

![Jupiter](Images/jupiter.png)


