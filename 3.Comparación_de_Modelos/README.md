TFM Mercadona: Detecccion de fallos en muebles frigorificos

<img src="../Images/mercadona_logo.png" width="40%"><br/>



## 3.  Comparación de Modelos <a name="Comparación_de_Modelos"></a>

Para la compraracion de modelos se utlizo la libreria **sklearn** y **xgboost** y se compararon los siguentes algoritmos:
*Regresión logística
*SVM (Support vector machine)
*Random Forest 
*XGboost

El notebook realiza los siguentes pasos:

1. [Generar valores de ventanas](#ventanas)
2. [Convertimos los registros a series temporales.](#Cectordetiempo)
3. [Generamos la etiqueta de fallo y normales ](#resampledata)
4. [Estandarización de los valores de train y test](#scalevalues)
5. [Entrenamiento de los modelos](#Entrenarmodelo)
6. [Evaluacion de modelos](#Evaluarmodelo)



![gif](https://github.com/luisnose/TFM_Mercadona_DeteccionFallo_CentralesFrigorifica/blob/main/Images/Vectordetiempo.gif)


## 1. Generar valores de ventanas<a name="ventanas"></a>

* HoraIni =  Tiempo de antelacion al fallo
* HoraFin = Fin de la ventana de fallo
* IniNor = tiempo de incio de ventana de estado normal   *Se toman dias antes del fallo para evitar tomar valores en estado de fallo
* Window = Minutos de tamaño del vector de tiempo
* Steps = Cantidad de muestras dentro del vector de tiempo


## 2. Convertimos los registros a series temporales<a name="Cectordetiempo"></a>

```
def transform_to_supervised(df, previous_steps=1, dropnan=True):

 
    # original column names
    col_names = df.columns
    
    # list of columns and corresponding names we'll build from 
    # the originals found in the input DataFrame
    cols, names = list(), list()

    # input sequence (t-n, ... t-1)
    for i in range(previous_steps, 0,(int(-Window/Steps))):
        cols.append(df.shift(i))
        names += [('%s(t-%d)' % (col_name, i)) for col_name in col_names]

    # put all the columns together into a single aggregated DataFrame
    agg = pd.concat(cols, axis=1)
    agg.columns = names

    # drop rows with NaN values
    if dropnan:
        agg.dropna(inplace=True)

    return agg

def To_TimeSeries(data):
    cycles = data['cycle_number'].unique()
    df_time = pd.DataFrame([])
    for i  in range(len(cycles)):
        df_loop = data.loc[data['cycle_number'] == (i+1)]
        df_time_fail = df_loop[(df_loop['RUL'].isin(fail) )]
        df_time_normal = df_loop[(df_loop['RUL'].isin(normal))]
# Counter till 60 jumping with a step of 10 (in total 6 samples): t-60, t-50... t-10, t                                                                       
        df_time_fail = transform_to_supervised(df_time_fail[sequence_cols], int(Window), dropnan=False)
        df_time_normal = transform_to_supervised(df_time_normal[sequence_cols], int(Window), dropnan=False)
    
        df_time = df_time.append(df_time_normal).reset_index(drop=True)
        df_time = df_time.append(df_time_fail).reset_index(drop=True)
    return(df_time)



df = To_TimeSeries(Data_frame)
```


## 3. Generamos la etiqueta de fallo y normales<a name="resampledata"></a> ]

Se etiqueta como uno (1) las muestras de fallo y como cero (0) las muestras en estado normal

indicando que los valores anteriores a la hora fin sean etiquetados como uno y los valores posteriores como cero

```
df['label1'] = np.where(df['RUL'] <= HoraFin, 1, 0 )

```

## 4. Estandarización<a name="scalevalues"></a> 


La estandarización de conjuntos de datos es un requisito común para muchos estimadores de aprendizaje automático implementados en scikit-learn; podrían comportarse mal si las características individuales no se parecen más o menos a datos estándar distribuidos normalmente: Gaussiano con media cero y varianza unitaria.

```
cols=list(df_time.columns)
sequence_cols= cols[2:-3]

from sklearn.preprocessing import scale

y_test = test_data['label1']
X_test = pd.DataFrame(scale(test_data.filter(sequence_cols,axis = 1)))
X_test.columns = sequence_cols


y_train = train_data['label1']
X_train = pd.DataFrame(scale(train_data.filter(sequence_cols,axis = 1)))
X_train.columns = sequence_cols
```


## 5. Entrenamiento de los modelos <a name="Entrenarmodelo"></a> 

Cada uno de los algoritmos mencionados se entrenan y se guardan los pesos en una fichero.


´´´
import joblib
from time import time
from sklearn.metrics import accuracy_score, precision_score, recall_score
from numpy import loadtxt
from xgboost import XGBClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score


model = XGBClassifier()
eval_set = [(X_test, y_test)]
model.fit(X_train, y_train, early_stopping_rounds=20, eval_metric="logloss", eval_set=eval_set, verbose=True)
# make predictions for test data
y_pred = model.predict(X_test)
predictions = [round(value) for value in y_pred]
# evaluate predictions
accuracy = accuracy_score(y_test, predictions)
print("Accuracy: %.2f%%" % (accuracy * 100.0))

joblib.dump(model, './Models/I{}h_F{}h_W{}m_S5m_XGB_model.pkl'.format(HoraIni, HoraFin+1,Window))


from matplotlib import pyplot
print(model.feature_importances_)
# plot
pyplot.bar(range(len(model.feature_importances_)), model.feature_importances_)
pyplot.show()
´´´

## 6. Evaluacion de modelos <a name="Evaluarmodelo"></a> 

Se cargan los pesos de cada modelo y evaluarlo con ciclos de alarmas con los cuales no haya sido entrenado

´´´
models = {}
#'SVM', 'MLP',
for mdl in ['SVM','LR','RF','XGB']:
    models[mdl] = joblib.load('./Models/I{}h_F{}h_W{}m_S5m_{}_model.pkl'.format(HoraIni, HoraFin+1,Window,mdl))
    
    
def evaluate_model(name, model, features, labels):
    start = time()
    pred = model.predict(features)
    end = time()
    accuracy = round(accuracy_score(labels, pred), 3)
    precision = round(precision_score(labels, pred), 3)
    recall = round(recall_score(labels, pred), 3)
    print('{} -- Accuracy: {} / Precision: {} / Recall: {} / Latency: {}ms'.format(name,
                                                                                   accuracy,
                                                                                   precision,
                                                                                   recall,
                                                                                   round((end - start)*1000, 1)))

    
for name, mdl in models.items():
    evaluate_model(name, mdl, X_test, y_test)
    ´´´