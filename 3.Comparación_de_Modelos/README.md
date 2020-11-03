TFM Mercadona: Detecccion de fallos en muebles frigorificos

<img src="../Images/mercadona_logo.png" width="40%"><br/>



## 3.  Comparación de Modelos <a name="Comparación_de_Modelos"></a>

Para la compraracion de modelos se utlizo la libreria **sklearn** y **xgboost**:

se comprobaron distintas etiquetas con distintas ventanas de tiempo:

HoraIni = 1 #Hrs from failure
HoraFin = 1 #Hrs from failure
IniNor= 3*24 #Hrs start first sample of Normal
Window =30 #minutes of time series vector
Steps = 6#steps used on the same record (a record will have variables from t to the end of window according to the steps)


1. [Filtrado de alarmas críticas de la tabla de alarma.](#filtradoalarma)
2. [Eliminamos outliers de la telemetría en la tabla de muebles frigoríficos y central de frio.](#outliers2)
3. [Realizamos un resample para generar las muestra y crear una data con un timestamp peridorico cada 1 min.](#resampledata)
4. [Rellenamos los valores faltantes de la tabla:](#missingvalues)
    * Para Variables categóricos realizamos un .fillna(method='ffill') donde propagamos los valores con el mismo valor
    * Para Variables continuas, realizamos una interpolación de valores
5. [Realizamos un merge de las variables de cada mural con sus valores de la central de frio](#mergedata)
6. [Extraemos por medio de un merge, ventanas de 10 días de telemetría antes de las alarmas críticas filtradas en el primer paso](#mergealarmas)
7. [Generamos 2 columnas nuevas:](#lables2)
    * (Clycle_number) Una Columna para identificar los ciclos (1 para cada alarma crítica)
    * (RUL)Una para identificar por min, la cantidad de minutos restantes hasta el momento del fallo
8. [Cargamos el data set final en Google Cloud Storage](#GCS_Uploda)




![gif](https://github.com/luisnose/TFM_Mercadona_DeteccionFallo_CentralesFrigorifica/tree/main/Images/Vectordetiempo.gif)