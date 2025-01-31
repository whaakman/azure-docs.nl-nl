---
title: 'Zelfstudie: Een geautomatiseerde machine learning-model te verrijken'
titleSuffix: Azure Open Datasets
description: Leer hoe u het gemak van Azure Open gegevenssets samen met de kracht van Azure Machine Learning-service te maken van een regressiemodel om te voorspellen NYC taxi fare prijzen.
services: open-datasets
ms.service: open-datasets
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 05/02/2019
ms.openlocfilehash: a1df79c59ede8cd9ad72a2ebb2edb4bdb64b802a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67588976"
---
# <a name="tutorial-build-a-regression-model-with-automated-machine-learning-and-open-datasets"></a>Zelfstudie: Bouw een regressiemodel met geautomatiseerde machine learning en Open gegevenssets

In deze zelfstudie gaat u gebruikmaken van het gemak van Azure Open gegevenssets samen met de kracht van Azure Machine Learning-service te maken van een regressiemodel om te voorspellen NYC taxi fare prijzen. Eenvoudig openbaar beschikbare taxi downloaden, kerstmotief weerbericht van gegevens en een geautomatiseerde machine learning-experiment, met behulp van Azure Machine Learning-service configureren. Dit proces accepteert training gegevens en configuratie-instellingen en automatisch doorloopt combinaties van verschillende functie normalisering/standaardisatie methoden, modellen en hyperparameter instellingen om naar het beste model.

In deze zelfstudie leert u de volgende taken:

- Een werkruimte configureren in de Azure Machine Learning-service
- Een lokale Python-omgeving instellen
- Toegang, transformeren en join-gegevens met behulp van Azure Open-gegevenssets
- Een geautomatiseerde machine learning-regressiemodel trainen
- Berekenen van de nauwkeurigheid van model

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie vereist de volgende vereisten.

* Een werkruimte van Azure Machine Learning-service
* Een omgeving met Python 3.6

### <a name="create-a-workspace"></a>Een werkruimte maken

Ga als volgt de [instructies](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace#portal) voor het maken van een werkruimte via de Azure-portal, als u er nog geen hebt. Na het maken, noteer de naam van de werkruimte, de naam van resourcegroep en abonnements-ID.

### <a name="create-a-python-environment"></a>Een Python-omgeving maken

In dit voorbeeld wordt een Anaconda-omgeving met Jupyter-notebooks, maar u kunt deze code uitvoeren in een omgeving 3.6.x en met een teksteditor of IDE. Gebruik de volgende stappen uit om te maken van een nieuwe omgeving.

1. Als u dit nog niet, [downloaden](https://www.anaconda.com/distribution/) en Anaconda installeert, en kies **Python 3.7-versie**.
1. Open een prompt Anaconda en maak een nieuwe omgeving. Het duurt enkele minuten om de omgeving te maken terwijl onderdelen en pakketten worden gedownload.
    ```
    conda create -n tutorialenv python=3.6.5
    ```
1. Activeer de omgeving.
    ```
    conda activate tutorialenv
    ```
1. Schakel omgevingsspecifieke IPython kernels.
    ```
    conda install notebook ipykernel
    ```
1. Maak de kernel.
    ```
    ipython kernel install --user
    ```
1. Installeer de pakketten die u nodig hebt voor deze zelfstudie. Deze pakketten zijn grote en duurt 5-10 minuten om te installeren.
    ```
    pip install azureml-sdk[automl] azureml-contrib-opendatasets
    ```
1. Start een laptop-kernel uit uw omgeving.
    ```
    jupyter notebook
    ```

Nadat u deze stappen hebt voltooid, kloon de [Open gegevenssets notebook opslagplaats](https://github.com/Azure/OpenDatasetsNotebooks) en open de **tutorials/taxi-automl/01-tutorial-opendatasets-automl.ipynb** laptop uit te voeren.

## <a name="download-and-prepare-data"></a>Download en gegevens voorbereiden

Importeer de vereiste pakketten. Het pakket Open gegevenssets bevat een klasse voor elke gegevensbron (`NycTlcGreen` bijvoorbeeld) voor het filteren van eenvoudig datumparameters voordat u downloadt.


```python
from azureml.contrib.opendatasets import NycTlcGreen
import pandas as pd
from datetime import datetime
from dateutil.relativedelta import relativedelta
```

Beginnen met het maken van een dataframe voor het opslaan van de gegevens over taxi's. Als u werkt in een niet-Spark-omgeving, Open gegevenssets staat slechts één maand van gegevens op een tijdstip met bepaalde klassen om te voorkomen dat het downloaden `MemoryError` met grote gegevenssets. Voor het downloaden van een jaar aan gegevens over taxi's, één maand op een tijdstip en voordat u dit toevoegt aan iteratief ophalen `green_taxi_df` willekeurig voorbeeld 2000 records uit elke maand om te voorkomen dat het dataframe aanzienlijk groter. Bekijk het voorbeeld van de gegevens.

>[!NOTE]
> Open gegevenssets heeft het spiegelen van de klassen voor het werken in Spark-omgevingen waar de grootte en het geheugen worden niet een probleem.

```python
green_taxi_df = pd.DataFrame([])
start = datetime.strptime("1/1/2016","%m/%d/%Y")
end = datetime.strptime("1/31/2016","%m/%d/%Y")

for sample_month in range(12):
    temp_df_green = NycTlcGreen(start + relativedelta(months=sample_month), end + relativedelta(months=sample_month)) \
        .to_pandas_dataframe()
    green_taxi_df = green_taxi_df.append(temp_df_green.sample(2000))

green_taxi_df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendorID</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>puLocationId</th>
      <th>doLocationId</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>...</th>
      <th>paymentType</th>
      <th>fareAmount</th>
      <th>extra</th>
      <th>mtaTax</th>
      <th>improvementSurcharge</th>
      <th>tipAmount</th>
      <th>tollsAmount</th>
      <th>ehailFee</th>
      <th>totalAmount</th>
      <th>tripType</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20 17:38:28</td>
      <td>2016-01-20 17:46:33</td>
      <td>1</td>
      <td>0.98</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.921715</td>
      <td>40.766682</td>
      <td>-73.916908</td>
      <td>...</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01 21:53:28</td>
      <td>2016-01-02 00:00:00</td>
      <td>1</td>
      <td>3.08</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.979973</td>
      <td>40.677071</td>
      <td>-73.934349</td>
      <td>...</td>
      <td>2.0</td>
      <td>11.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>12.8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01 00:50:23</td>
      <td>2016-01-01 01:05:37</td>
      <td>1</td>
      <td>2.44</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.863045</td>
      <td>40.882923</td>
      <td>-73.839836</td>
      <td>...</td>
      <td>2.0</td>
      <td>12.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>13.8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04 17:50:03</td>
      <td>2016-01-04 18:03:43</td>
      <td>1</td>
      <td>2.87</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.977730</td>
      <td>40.684647</td>
      <td>-73.931259</td>
      <td>...</td>
      <td>1.0</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>13.8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1</td>
      <td>2016-01-13 08:48:20</td>
      <td>2016-01-13 08:52:16</td>
      <td>1</td>
      <td>0.50</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.942589</td>
      <td>40.841423</td>
      <td>-73.943672</td>
      <td>...</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>0,0</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>5.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1049431</th>
      <td>2</td>
      <td>2016-01-29 17:16:18</td>
      <td>2016-01-29 17:27:52</td>
      <td>1</td>
      <td>2.25</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.830894</td>
      <td>40.759434</td>
      <td>-73.842422</td>
      <td>...</td>
      <td>2.0</td>
      <td>10.5</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>12.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>646563</th>
      <td>2</td>
      <td>2016-01-14 00:45:30</td>
      <td>2016-01-14 00:54:16</td>
      <td>1</td>
      <td>1.93</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.927109</td>
      <td>40.762848</td>
      <td>-73.909302</td>
      <td>...</td>
      <td>1.0</td>
      <td>8.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>9.8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>438204</th>
      <td>1</td>
      <td>2016-01-09 14:25:02</td>
      <td>2016-01-09 14:32:48</td>
      <td>2</td>
      <td>0.80</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.881195</td>
      <td>40.741779</td>
      <td>-73.872086</td>
      <td>...</td>
      <td>2.0</td>
      <td>6.5</td>
      <td>0,0</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>7.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>303784</th>
      <td>2</td>
      <td>2016-01-25 18:13:47</td>
      <td>2016-01-25 18:23:50</td>
      <td>1</td>
      <td>1.04</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.954376</td>
      <td>40.805729</td>
      <td>-73.939117</td>
      <td>...</td>
      <td>1.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>1.5</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>11.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>269105</th>
      <td>2</td>
      <td>2016-01-24 20:46:50</td>
      <td>2016-01-24 21:04:03</td>
      <td>6</td>
      <td>2.82</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.845200</td>
      <td>40.722134</td>
      <td>-73.810638</td>
      <td>...</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>2.0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>16.3</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>10 rijen × 23 kolommen</p>
</div>



Nu dat de initiële gegevens zijn geladen, definieert u een functie voor het maken van verschillende functies op basis van tijd uit de ophalen datum / tijdveld. Dit maakt u nieuwe velden voor het maandnummer van de, dag van maand, dag van week en tijdstip van de dag en het model rekening houden met tijd op basis van periodieke variatie staat. De functie wordt ook een statische functie voor de landcode voor deelname aan feestdagen gegevens toegevoegd. Gebruik de `apply()` functie van het gegevensframe iteratief toepassen de `build_time_features()` functie voor elke rij in de gegevens over taxi's.


```python
def build_time_features(vector):
    pickup_datetime = vector[0]
    month_num = pickup_datetime.month
    day_of_month = pickup_datetime.day
    day_of_week = pickup_datetime.weekday()
    hour_of_day = pickup_datetime.hour
    country_code = "US"

    return pd.Series((month_num, day_of_month, day_of_week, hour_of_day, country_code))

green_taxi_df[["month_num", "day_of_month","day_of_week", "hour_of_day", "country_code"]] = green_taxi_df[["lpepPickupDatetime"]].apply(build_time_features, axis=1)
green_taxi_df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendorID</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>puLocationId</th>
      <th>doLocationId</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>...</th>
      <th>tipAmount</th>
      <th>tollsAmount</th>
      <th>ehailFee</th>
      <th>totalAmount</th>
      <th>tripType</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20 17:38:28</td>
      <td>2016-01-20 17:46:33</td>
      <td>1</td>
      <td>0.98</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.921715</td>
      <td>40.766682</td>
      <td>-73.916908</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>1.0</td>
      <td>1</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01 21:53:28</td>
      <td>2016-01-02 00:00:00</td>
      <td>1</td>
      <td>3.08</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.979973</td>
      <td>40.677071</td>
      <td>-73.934349</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>12.8</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01 00:50:23</td>
      <td>2016-01-01 01:05:37</td>
      <td>1</td>
      <td>2.44</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.863045</td>
      <td>40.882923</td>
      <td>-73.839836</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>13.8</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04 17:50:03</td>
      <td>2016-01-04 18:03:43</td>
      <td>1</td>
      <td>2.87</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.977730</td>
      <td>40.684647</td>
      <td>-73.931259</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>13.8</td>
      <td>1.0</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1</td>
      <td>2016-01-13 08:48:20</td>
      <td>2016-01-13 08:52:16</td>
      <td>1</td>
      <td>0.50</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.942589</td>
      <td>40.841423</td>
      <td>-73.943672</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>5.3</td>
      <td>1.0</td>
      <td>1</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>1049431</th>
      <td>2</td>
      <td>2016-01-29 17:16:18</td>
      <td>2016-01-29 17:27:52</td>
      <td>1</td>
      <td>2.25</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.830894</td>
      <td>40.759434</td>
      <td>-73.842422</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>12.3</td>
      <td>1.0</td>
      <td>1</td>
      <td>29</td>
      <td>4</td>
      <td>17</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>646563</th>
      <td>2</td>
      <td>2016-01-14 00:45:30</td>
      <td>2016-01-14 00:54:16</td>
      <td>1</td>
      <td>1.93</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.927109</td>
      <td>40.762848</td>
      <td>-73.909302</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>9.8</td>
      <td>1.0</td>
      <td>1</td>
      <td>14</td>
      <td>3</td>
      <td>0</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>438204</th>
      <td>1</td>
      <td>2016-01-09 14:25:02</td>
      <td>2016-01-09 14:32:48</td>
      <td>2</td>
      <td>0.80</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.881195</td>
      <td>40.741779</td>
      <td>-73.872086</td>
      <td>...</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>7.3</td>
      <td>1.0</td>
      <td>1</td>
      <td>9</td>
      <td>5</td>
      <td>14</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>303784</th>
      <td>2</td>
      <td>2016-01-25 18:13:47</td>
      <td>2016-01-25 18:23:50</td>
      <td>1</td>
      <td>1.04</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.954376</td>
      <td>40.805729</td>
      <td>-73.939117</td>
      <td>...</td>
      <td>1.5</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>11.3</td>
      <td>1.0</td>
      <td>1</td>
      <td>25</td>
      <td>0</td>
      <td>18</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>269105</th>
      <td>2</td>
      <td>2016-01-24 20:46:50</td>
      <td>2016-01-24 21:04:03</td>
      <td>6</td>
      <td>2.82</td>
      <td>Geen</td>
      <td>Geen</td>
      <td>-73.845200</td>
      <td>40.722134</td>
      <td>-73.810638</td>
      <td>...</td>
      <td>2.0</td>
      <td>0,0</td>
      <td>NaN</td>
      <td>16.3</td>
      <td>1.0</td>
      <td>1</td>
      <td>24</td>
      <td>6</td>
      <td>20</td>
      <td>VS</td>
    </tr>
  </tbody>
</table>
<p>10 rijen × 28 kolommen</p>
</div>

Verwijder enkele van de kolommen die u niet voor modellen of extra functie gebouw hoeft. Wijzig de naam van het tijdveld voor ophalen keer, en daarnaast de tijd omzetten in met behulp van middernacht `pandas.Series.dt.normalize`. U doet dit op alle tijd functies zodat de datum/tijd-component kan later worden gebruikt als sleutel bij het toevoegen van gegevenssets samen met een dagelijkse granulariteit.

```python
columns_to_remove = ["lpepDropoffDatetime", "puLocationId", "doLocationId", "extra", "mtaTax",
                     "improvementSurcharge", "tollsAmount", "ehailFee", "tripType", "rateCodeID",
                     "storeAndFwdFlag", "paymentType", "fareAmount", "tipAmount"
                    ]
for col in columns_to_remove:
    green_taxi_df.pop(col)

green_taxi_df = green_taxi_df.rename(columns={"lpepPickupDatetime": "datetime"})
green_taxi_df["datetime"] = green_taxi_df["datetime"].dt.normalize()
green_taxi_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendorID</th>
      <th>datetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>dropoffLatitude</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20</td>
      <td>1</td>
      <td>0.98</td>
      <td>-73.921715</td>
      <td>40.766682</td>
      <td>-73.916908</td>
      <td>40.761257</td>
      <td>8.8</td>
      <td>1</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>3.08</td>
      <td>-73.979973</td>
      <td>40.677071</td>
      <td>-73.934349</td>
      <td>40.671654</td>
      <td>12.8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>2.44</td>
      <td>-73.863045</td>
      <td>40.882923</td>
      <td>-73.839836</td>
      <td>40.868336</td>
      <td>13.8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04</td>
      <td>1</td>
      <td>2.87</td>
      <td>-73.977730</td>
      <td>40.684647</td>
      <td>-73.931259</td>
      <td>40.694248</td>
      <td>13.8</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>VS</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1</td>
      <td>2016-01-13</td>
      <td>1</td>
      <td>0.50</td>
      <td>-73.942589</td>
      <td>40.841423</td>
      <td>-73.943672</td>
      <td>40.834396</td>
      <td>5.3</td>
      <td>1</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>VS</td>
    </tr>
  </tbody>
</table>
</div>

### <a name="enrich-with-holiday-data"></a>Verrijken met gegevens voor feestdagen

Nu dat u gegevens over taxi's gedownload en ongeveer voorbereid hebt, toevoegen in de feestdagen gegevens als aanvullende functies. Vakantiedag specifieke functies helpt de nauwkeurigheid van model, als belangrijkste feestdagen tijden waar aanzienlijk taxi vraag toeneemt en levering wordt beperkt. De gegevensset vakantie is relatief klein, dus de volledige set ophalen met behulp van de `PublicHolidays` constructor zonder parameters voor het filteren van de klasse. Voorbeeld van de gegevens om te controleren of de indeling.

```python
from azureml.contrib.opendatasets import PublicHolidays
# call default constructor to download full dataset
holidays_df = PublicHolidays().to_pandas_dataframe()
holidays_df.head(5)
```

    ActivityStarted, to_pandas_dataframe
    Looking for parquet files...
    Reading them into Pandas dataframe...
    Reading Processed/part-00000-tid-1353805596865908763-9ee4e95b-0d55-4292-addd-a0e19d7c32cb-3559-c000.snappy.parquet under container holidaydatacontainer
    Done.
    ActivityCompleted: Activity=to_pandas_dataframe, HowEnded=Success, Duration=1799.89 [ms]

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>countryOrRegion</th>
      <th>holidayName</th>
      <th>isPaidTimeOff</th>
      <th>countryRegionCode</th>
      <th>normalizeHolidayName</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40688</th>
      <td>Albanië</td>
      <td>Dag van jaar</td>
      <td>Geen</td>
      <td>AL</td>
      <td>Dag van jaar</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40689</th>
      <td>Algerije</td>
      <td>Dag van jaar</td>
      <td>Geen</td>
      <td>DZ</td>
      <td>Dag van jaar</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40690</th>
      <td>Andorra</td>
      <td>Dag van jaar</td>
      <td>Geen</td>
      <td>AD</td>
      <td>Dag van jaar</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40691</th>
      <td>Angola</td>
      <td>Dag van jaar</td>
      <td>Geen</td>
      <td>AO</td>
      <td>Dag van jaar</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40692</th>
      <td>Argentinië</td>
      <td>Dag van jaar</td>
      <td>Geen</td>
      <td>AR</td>
      <td>Dag van jaar</td>
      <td>2008-01-01</td>
    </tr>
  </tbody>
</table>
</div>



Wijzig de naam van de `countryRegionCode` en `date` kolommen overeenkomen met de namen van het betreffende veld uit de gegevens over taxi's en ook de tijd normaliseren, zodat deze kan worden gebruikt als een sleutel. Koppel vervolgens de feestdagen-gegevens met de gegevens over taxi's door het uitvoeren van een left-join met behulp van de Pandas `merge()` functie. Hiermee behoudt u alle records uit `green_taxi_df`, maar toevoegen in de feestdagen gegevens indien deze voor de bijbehorende bestaat `datetime` en `country_code`, die in dit geval is, altijd `"US"`. Voorbeeld van de gegevens om te controleren of ze correct zijn samengevoegd.

```python
holidays_df = holidays_df.rename(columns={"countryRegionCode": "country_code", "date": "datetime"})
holidays_df["datetime"] = holidays_df["datetime"].dt.normalize()
holidays_df.pop("countryOrRegion")
holidays_df.pop("holidayName")

taxi_holidays_df = pd.merge(green_taxi_df, holidays_df, how="left", on=["datetime", "country_code"])
taxi_holidays_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendorID</th>
      <th>datetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>dropoffLatitude</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
      <th>isPaidTimeOff</th>
      <th>normalizeHolidayName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2016-01-20</td>
      <td>1</td>
      <td>0.98</td>
      <td>-73.921715</td>
      <td>40.766682</td>
      <td>-73.916908</td>
      <td>40.761257</td>
      <td>8.8</td>
      <td>1</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>VS</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>3.08</td>
      <td>-73.979973</td>
      <td>40.677071</td>
      <td>-73.934349</td>
      <td>40.671654</td>
      <td>12.8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21</td>
      <td>VS</td>
      <td>True</td>
      <td>Dag van jaar</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>2.44</td>
      <td>-73.863045</td>
      <td>40.882923</td>
      <td>-73.839836</td>
      <td>40.868336</td>
      <td>13.8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>VS</td>
      <td>True</td>
      <td>Dag van jaar</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2016-01-04</td>
      <td>1</td>
      <td>2.87</td>
      <td>-73.977730</td>
      <td>40.684647</td>
      <td>-73.931259</td>
      <td>40.694248</td>
      <td>13.8</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>VS</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2016-01-13</td>
      <td>1</td>
      <td>0.50</td>
      <td>-73.942589</td>
      <td>40.841423</td>
      <td>-73.943672</td>
      <td>40.834396</td>
      <td>5.3</td>
      <td>1</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>VS</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

### <a name="enrich-with-weather-data"></a>Verrijken met weergegevens

U kunt nu surface weergegevens van NOAA toevoegen aan de gegevens over taxi's en feestdagen. Gebruik een soortgelijke benadering voor het ophalen van de weergegevens door één maand op een tijdstip iteratief te downloaden. Daarnaast geven de `cols` parameter met een matrix met tekenreeksen voor het filteren van de kolommen die u wilt downloaden. Dit is een zeer grote gegevensset met surface weergegevens van over de hele wereld, dus voordat u elke maand toevoegt, filteren de lat/lange velden in de buurt met behulp van NYC de `query()` functie van het gegevensframe. Dit zorgt ervoor dat de `weather_df` te groot niet ophalen.

```python
from azureml.contrib.opendatasets import NoaaIsdWeather

weather_df = pd.DataFrame([])
start = datetime.strptime("1/1/2016","%m/%d/%Y")
end = datetime.strptime("1/31/2016","%m/%d/%Y")

for sample_month in range(12):
    tmp_df = NoaaIsdWeather(cols=["temperature", "precipTime", "precipDepth", "snowDepth"], start_date=start + relativedelta(months=sample_month), end_date=end + relativedelta(months=sample_month))\
        .to_pandas_dataframe()
    print("--weather downloaded--")

    # filter out coordinates not in NYC to conserve memory
    tmp_df = tmp_df.query("latitude>=40.53 and latitude<=40.88")
    tmp_df = tmp_df.query("longitude>=-74.09 and longitude<=-73.72")
    print("--filtered coordinates--")
    weather_df = weather_df.append(tmp_df)

weather_df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>wban</th>
      <th>precipTime</th>
      <th>snowDepth</th>
      <th>temperatuur</th>
      <th>Breedtegraad</th>
      <th>precipDepth</th>
      <th>Lengtegraad</th>
      <th>datetime</th>
      <th>usaf</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1754979</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>7.2</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 00:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754980</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.7</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 01:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754981</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.7</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 02:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754982</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.1</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 03:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754983</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.6</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 04:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754984</th>
      <td>94741</td>
      <td>24.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40.85</td>
      <td>5.0</td>
      <td>-74.061</td>
      <td>2016-01-01 04:59:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754985</th>
      <td>94741</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40.85</td>
      <td>NaN</td>
      <td>-74.061</td>
      <td>2016-01-01 04:59:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754986</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.6</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 05:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754987</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 06:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754988</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>40.85</td>
      <td>0,0</td>
      <td>-74.061</td>
      <td>2016-01-01 07:51:00</td>
      <td>725025</td>
    </tr>
  </tbody>
</table>
</div>

Het opnieuw aanroepen `pandas.Series.dt.normalize` op de `datetime` veld in de weergegevens, zodat deze overeenkomt met de verificatiesleutel in `taxi_holidays_df`. De overbodige kolommen verwijderen en filteren van records waarin de temperatuur hoger is `NaN`.

Naast de weergegevens groeperen, zodat u dagelijks waarden weer samengevoegde hebben. Definieer een dict `aggregations` definiëren hoe elk veld op het niveau van een dagelijkse aggregeren. Voor `snowDepth` en `temperature` het gemiddelde te nemen en voor `precipTime` en `precipDepth` nemen de dagelijkse maximum. Gebruik de `groupby()` functie samen met de aggregaties om de gegevens te groeperen. Voorbeeld van de gegevens om ervoor te zorgen dat er één record per dag.

```python
weather_df["datetime"] = weather_df["datetime"].dt.normalize()
weather_df.pop("usaf")
weather_df.pop("wban")
weather_df.pop("longitude")
weather_df.pop("latitude")

# filter out NaN
weather_df = weather_df.query("temperature==temperature")

# group by datetime
aggregations = {"snowDepth": "mean", "precipTime": "max", "temperature": "mean", "precipDepth": "max"}
weather_df_grouped = weather_df.groupby("datetime").agg(aggregations)
weather_df_grouped.head(10)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>snowDepth</th>
      <th>precipTime</th>
      <th>temperatuur</th>
      <th>precipDepth</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-01</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>5.197345</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-02</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>2.567857</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-03</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>3.846429</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-04</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>0.123894</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>-7.206250</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>-0.896396</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>3.180645</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>4.384091</td>
      <td>0,0</td>
    </tr>
    <tr>
      <th>2016-01-09</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>6.710274</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2016-01-10</th>
      <td>NaN</td>
      <td>24.0</td>
      <td>10.943655</td>
      <td>254.0</td>
    </tr>
  </tbody>
</table>
</div>

> [!NOTE]
> De voorbeelden in deze zelfstudie samenvoegen van gegevens met behulp van Pandas functies en aangepaste aggregaties, maar de Open gegevenssets SDK heeft klassen die zijn ontworpen om eenvoudig samen te voegen en verrijken van gegevenssets. Zie de [notebook](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) voor codevoorbeelden van deze ontwerppatronen.

### <a name="cleanse-data"></a>Gegevens opschonen

De taxi en feestdagen gegevens die u voorbereid met de nieuwe weergegevens samenvoegen. Deze keer dat u hoeft alleen de `datetime` sleutel en een links-deelname van de gegevens opnieuw uit te voeren. Voer de `describe()` functie op het nieuwe gegevensframe om te zien samenvattende statistieken voor elk veld.

```python
taxi_holidays_weather_df = pd.merge(taxi_holidays_df, weather_df_grouped, how="left", on=["datetime"])
taxi_holidays_weather_df.describe()
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendorID</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>dropoffLatitude</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>snowDepth</th>
      <th>precipTime</th>
      <th>temperatuur</th>
      <th>precipDepth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>1671.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
    </tr>
    <tr>
      <th>gemiddelde</th>
      <td>1.786583</td>
      <td>6.576208</td>
      <td>1.582588</td>
      <td>20.505491</td>
      <td>84.936413</td>
      <td>-36.232825</td>
      <td>21.723144</td>
      <td>7.863018</td>
      <td>6.500000</td>
      <td>15.113708</td>
      <td>3.240250</td>
      <td>13.664125</td>
      <td>11.764141</td>
      <td>13.258875</td>
      <td>13.903524</td>
      <td>1056.644458</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.409728</td>
      <td>9.086857</td>
      <td>2.418177</td>
      <td>108.847821</td>
      <td>70.678506</td>
      <td>37.650276</td>
      <td>19.104384</td>
      <td>10.648766</td>
      <td>3.452124</td>
      <td>8.485155</td>
      <td>1.956895</td>
      <td>6.650676</td>
      <td>15.651884</td>
      <td>10.339720</td>
      <td>9.474396</td>
      <td>2815.592754</td>
    </tr>
    <tr>
      <th>min.</th>
      <td>1.000000</td>
      <td>-60.000000</td>
      <td>-1.000000</td>
      <td>-74.179482</td>
      <td>0,000000</td>
      <td>-74.190704</td>
      <td>0,000000</td>
      <td>-52.800000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0,000000</td>
      <td>0,000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>-13.379464</td>
      <td>0,000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>0.330000</td>
      <td>-73.946680</td>
      <td>40.717712</td>
      <td>-73.945429</td>
      <td>1.770000</td>
      <td>1.000000</td>
      <td>3.750000</td>
      <td>8.000000</td>
      <td>2.000000</td>
      <td>9.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>6.620773</td>
      <td>0,000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>4.000000</td>
      <td>0.830000</td>
      <td>1.500000</td>
      <td>40.814129</td>
      <td>0.500000</td>
      <td>21.495000</td>
      <td>2.000000</td>
      <td>6.500000</td>
      <td>15.000000</td>
      <td>3.000000</td>
      <td>15.000000</td>
      <td>4.428571</td>
      <td>6.000000</td>
      <td>13.090753</td>
      <td>10.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.000000</td>
      <td>9.000000</td>
      <td>1.870000</td>
      <td>89.000000</td>
      <td>129.000000</td>
      <td>1.000000</td>
      <td>40.746146</td>
      <td>11.300000</td>
      <td>9.250000</td>
      <td>22.000000</td>
      <td>5.000000</td>
      <td>19.000000</td>
      <td>12.722222</td>
      <td>24.000000</td>
      <td>22.944737</td>
      <td>132.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.000000</td>
      <td>460.000000</td>
      <td>51.950000</td>
      <td>265.000000</td>
      <td>265.000000</td>
      <td>6.000000</td>
      <td>58.600000</td>
      <td>498.000000</td>
      <td>12.000000</td>
      <td>30.000000</td>
      <td>6.000000</td>
      <td>23.000000</td>
      <td>67.090909</td>
      <td>24.000000</td>
      <td>31.303665</td>
      <td>9999.000000</td>
    </tr>
  </tbody>
</table>
</div>

In de samenvattende statistieken, kunt u zien dat er verschillende velden die de uitschieters en-waarden bevatten die u de nauwkeurigheid van model beperkt zijn. Eerst de lat/lange velden filteren om te worden binnen de dezelfde grenzen die u hebt gebruikt voor het filteren van Weergegevens. De `tripDistance` veld heeft een beschadigde gegevens, omdat de minimale waarde negatief is. De `passengerCount` veld heeft een beschadigde gegevens ook met de maximale waarde wordt 210 passagiers. Ten slotte de `totalAmount` veld heeft negatieve waarden, die niet zinvol zijn in de context van het model.

Deze afwijkingen met behulp van queryfuncties filteren en verwijder vervolgens de laatste paar kolommen niet nodig voor de training.

```python
final_df = taxi_holidays_weather_df.query("pickupLatitude>=40.53 and pickupLatitude<=40.88")
final_df = final_df.query("pickupLongitude>=-74.09 and pickupLongitude<=-73.72")
final_df = final_df.query("tripDistance>0 and tripDistance<75")
final_df = final_df.query("passengerCount>0 and passengerCount<100")
final_df = final_df.query("totalAmount>0")

columns_to_remove_for_training = ["datetime", "pickupLongitude", "pickupLatitude", "dropoffLongitude", "dropoffLatitude", "country_code"]
for col in columns_to_remove_for_training:
    final_df.pop(col)
```

Bel `describe()` opnieuw op de gegevens om ervoor te zorgen opschonen gewerkt zoals verwacht. U hebt nu een voorbereid en opgeschoonde set taxi, vakantie en weergegevens moet worden gebruikt voor machine learning-modeltraining.

```python
final_df.describe()
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendorID</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>snowDepth</th>
      <th>precipTime</th>
      <th>temperatuur</th>
      <th>precipDepth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>1490.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
    </tr>
    <tr>
      <th>gemiddelde</th>
      <td>1.786910</td>
      <td>1.343476</td>
      <td>2.848488</td>
      <td>14.689039</td>
      <td>3.499788</td>
      <td>14.948916</td>
      <td>3.234254</td>
      <td>13.647344</td>
      <td>12.508581</td>
      <td>11.855929</td>
      <td>10.301433</td>
      <td>208.432384</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.409508</td>
      <td>1.001232</td>
      <td>2.895960</td>
      <td>10.289832</td>
      <td>1.707865</td>
      <td>8.442438</td>
      <td>1.958477</td>
      <td>6.640280</td>
      <td>16.203195</td>
      <td>10.125701</td>
      <td>8.553512</td>
      <td>1284.892832</td>
    </tr>
    <tr>
      <th>min.</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.010000</td>
      <td>3.300000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0,000000</td>
      <td>0,000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>-13.379464</td>
      <td>0,000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>1.070000</td>
      <td>8.160000</td>
      <td>2.000000</td>
      <td>8.000000</td>
      <td>2.000000</td>
      <td>9.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>3.504580</td>
      <td>0,000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>1.900000</td>
      <td>11.300000</td>
      <td>3.000000</td>
      <td>15.000000</td>
      <td>3.000000</td>
      <td>15.000000</td>
      <td>4.250000</td>
      <td>6.000000</td>
      <td>10.168182</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>3.550000</td>
      <td>17.800000</td>
      <td>5.000000</td>
      <td>22.000000</td>
      <td>5.000000</td>
      <td>19.000000</td>
      <td>15.647059</td>
      <td>24.000000</td>
      <td>16.966923</td>
      <td>41.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.000000</td>
      <td>6.000000</td>
      <td>51.950000</td>
      <td>150.300000</td>
      <td>6.000000</td>
      <td>30.000000</td>
      <td>6.000000</td>
      <td>23.000000</td>
      <td>67.090909</td>
      <td>24.000000</td>
      <td>26.524107</td>
      <td>9999.000000</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="train-a-model"></a>Een model trainen

U kunt nu de voorbereide gegevens gebruiken met het trainen van een geautomatiseerde machine learning-model. Begin door op te splitsen `final_df` in functies (X-waarden) en labels (y-waarde), die voor dit model zijn de kosten voor telecommunicatie tarief.

```python
y_df = final_df.pop("totalAmount")
x_df = final_df
```

Nu splitst u de gegevens in trainings- en testsets met behulp van de functie `train_test_split()` in de `scikit-learn`-bibliotheek. Met de parameter `test_size` wordt het percentage gegevens bepaald dat moet worden toegewezen aan testen. De `random_state` parameter wordt een seed ingesteld op de generator van willekeurige getallen, zodat uw splitsingen trainen en testen deterministisch worden.


```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=222)
```

### <a name="load-workspace-and-configure-experiment"></a>Laden van de werkruimte en het configureren van experiment

Laden van uw Azure Machine Learning service werkruimte met de `get()` functie met de gegevens van uw abonnement en de werkruimte. Maak een experiment in uw werkruimte opslaan en de uitvoering van uw model controleren.


```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment

workspace = Workspace.get(subscription_id="<your-subscription-id>", name="<your-workspace-name>", resource_group="<your-resource-group>")
experiment = Experiment(workspace, "opendatasets-ml")
```

Maak een configuratieobject voor het gebruik van het experiment de `AutoMLConfig` klasse. U koppelt uw trainingsgegevens, en ook instellingen en parameters waarmee de trainingsproces opgeven. De parameters hebben de volgende doeleinden:

* `task`: het type van het experiment dat moet worden uitgevoerd.
* `X`: functies training.
* `y`: training van labels.
* `iterations`: aantal herhalingen om uit te voeren. Elke herhaling probeert combinaties van verschillende functie normalisering/standaardisatie methoden en verschillende modellen met behulp van meerdere hyperparameter-instellingen.
* `primary_metric`: primaire metrische waarde voor het optimaliseren van tijdens het trainen van het model. Best passend model wordt gekozen op basis van deze metrische gegevens.
* `preprocess`: Hiermee bepaalt u of de ingevoerde gegevens (afhandeling van gegevens ontbreken, tekst converteren naar Numeriek, enz.) door het experiment kunt voorverwerken
* `n_cross_validations`: Aantal kruisvalidaties dat moet worden uitgevoerd wanneer er geen validatiegegevens worden opgegeven.


```python
from azureml.train.automl import AutoMLConfig

automl_config = AutoMLConfig(task="regression",
                             X=X_train.values,
                             y=y_train.values.flatten(),
                             iterations=20,
                             primary_metric="spearman_correlation",
                             preprocess=True,
                             n_cross_validations=5
                            )
```

### <a name="submit-experiment"></a>Experiment verzenden

Het experiment voor training verzenden. Nadat u het experiment hebt verzonden, doorloopt het proces verschillende machine learning-algoritmen en hyperparameter-instellingen conform de gedefinieerde beperkingen. Het optimale model worden gekozen door het optimaliseren van de gedefinieerde nauwkeurigheid metrische gegevens. Geeft de `automl_config` object naar het experimentcanvas. Stel de uitvoer van `True` om uitgevoerd tijdens het experiment weer te geven.

Na het indienen van het experiment ziet u live uitvoer voor de trainingsproces. Voor elke herhaling ziet u de model-type en de functie normalisering/normalisatie-methode, de uitvoeringsduur en de nauwkeurigheid van de training. In het veld `BEST` wordt de beste trainingsscore op basis van uw type metrische waarde bijgehouden.

```python
training_run = experiment.submit(automl_config, show_output=True)
```

    Running on local machine
    Parent Run ID: AutoML_5c35f2a7-e479-4e7f-a131-ed4cb51e29d1
    Current status: DatasetEvaluation. Gathering dataset statistics.
    Current status: FeaturesGeneration. Generating features for the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetCrossValidationSplit. Generating individually featurized CV splits.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: ModelSelection. Beginning model selection.

    ****************************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ****************************************************************************************************

     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler RandomForest                      0:00:07       0.9081    0.9081
             1   StandardScalerWrapper DecisionTree             0:00:05       0.9121    0.9121
             2   StandardScalerWrapper LightGBM                 0:00:04       0.9318    0.9318
             3   StandardScalerWrapper LightGBM                 0:00:07       0.9286    0.9318
             4   MaxAbsScaler LightGBM                          0:00:05       0.9246    0.9318
             5   MaxAbsScaler LightGBM                          0:00:05       0.9199    0.9318
             6   MaxAbsScaler RandomForest                      0:00:07       0.9327    0.9327
             7   StandardScalerWrapper ElasticNet               0:00:04       0.9371    0.9371
             8   MaxAbsScaler LightGBM                          0:00:05       0.9327    0.9371
             9   MaxAbsScaler SGD                               0:00:04       0.9077    0.9371
            10   MaxAbsScaler LightGBM                          0:00:04       0.9340    0.9371
            11   StandardScalerWrapper LightGBM                 0:00:04       0.8301    0.9371
            12   MaxAbsScaler DecisionTree                      0:00:05       0.9214    0.9371
            13   StandardScalerWrapper DecisionTree             0:00:04       0.9201    0.9371
            14   MaxAbsScaler DecisionTree                      0:00:05       0.9179    0.9371
            15   MaxAbsScaler ExtremeRandomTrees                0:00:05       0.9052    0.9371
            16   StandardScalerWrapper DecisionTree             0:00:04       0.9282    0.9371
            17   StandardScalerWrapper ElasticNet               0:00:04       0.9319    0.9371
            18   VotingEnsemble                                 0:00:16       0.9380    0.9380
            19   StackEnsemble                                  0:00:17       0.9376    0.9380

### <a name="retrieve-the-fitted-model"></a>Het model ophalen

Aan het einde van alle herhalingen van de training maakt de geautomatiseerde machine learning-proces een algoritme ensembles van alle afzonderlijke wordt uitgevoerd, met gezakt of stapelen. De gemonteerd ensembles ophalen in de variabele `fitted_model`, en de beste afzonderlijke uitvoeren in de variabele `best_run`.

```python
best_run, fitted_model = training_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="test-model-accuracy"></a>De nauwkeurigheid van model testen

Het model gemonteerd ensembles voor voorspellingen op de testgegevensset om te voorspellen taxi tarieven gebruiken. De functie `predict()` maakt gebruik van het model en de waarden van y, taxi fare kosten, voorspelt voor de `X_test` gegevensset.


```python
y_predict = fitted_model.predict(X_test.values)
```

Bereken de root mean squared fout van de resultaten. Gebruik de `y_test` dataframe, en deze converteren naar een lijst met `y_actual` om te vergelijken met de voorspelde waarden. Met de functie `mean_squared_error` wordt de gemiddelde gekwadrateerde fout berekend tussen twee matrices met waarden. Duurt de vierkantswortel van het resultaat geeft een fout in de dezelfde eenheden als de variabele y, kosten. Wordt ongeveer hoe ver de taxi fare voorspellingen van de werkelijke tarieven terwijl sterk weging van grote fouten.


```python
from sklearn.metrics import mean_squared_error
from math import sqrt

y_actual = y_test.values.flatten().tolist()
rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```




    4.178568987067901



Voer de volgende code uit om MAPE (het gemiddelde absolute foutpercentage) te berekenen met behulp van de volledige gegevenssets `y_actual` en `y_predict`. Met deze statistiek wordt een absoluut verschil tussen elke voorspelde en werkelijke waarde berekent en worden alle verschillen opgeteld. Dit bedrag geeft vervolgens aan als een percentage van het totaal van de werkelijke waarden.


```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

    Model MAPE:
    0.14923619644924357

    Model Accuracy:
    0.8507638035507564

Gezien het feit dat we hebben een relatief klein aantal gegevens ten opzichte van de volledige gegevensset gebruikt (n = 11748), de nauwkeurigheid van model is tamelijk hoog 85%, met RMSE op rond + - $4.00 fout bij het voorspellen van taxi fare prijs. Als volgende stap te verbeteren nauwkeurigheid, gaat u terug naar de tweede cel van dit notitieblok en verhoog de grootte van de steekproef van 2000 records per maand, en uitvoeren van de gehele mogelijke experimenteren opnieuw opnieuw trainen van het model met meer gegevens.

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet dat de bronnen die u hebt gemaakt wilt, verwijderen, zodat u geen kosten.

1. Selecteer **Resourcegroepen** links in Azure Portal.
1. Selecteer de resourcegroep die u eerder hebt gemaakt uit de lijst.
1. Selecteer **Resourcegroep verwijderen**.
1. Voer de naam van de resourcegroup in. Selecteer vervolgens **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* Zie de Azure Open gegevenssets [notitieblokken](https://github.com/Azure/OpenDatasetsNotebooks) voor meer voorbeelden van code.
* Ga als volgt de [procedures](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train) geautomatiseerd voor meer informatie over machine learning in Azure Machine Learning-service.
