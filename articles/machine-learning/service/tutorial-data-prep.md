---
title: 'Zelfstudie 1: gegevens voorbereiden voor modellering met Azure Machine Learning service'
description: In het eerste deel van de zelfstudie leert u hoe u gegevens in Python voorbereidt voor regressiemodellering met de Azure ML-SDK.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: cforbe
ms.author: cforbe
ms.reviewer: trbye
ms.date: 12/04/2018
ms.openlocfilehash: 700dfa9fded30fd09eab69a15abf54fb420c5c06
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52883848"
---
# <a name="tutorial-1-prepare-data-for-regression-modeling"></a>Zelfstudie 1: gegevens voorbereiden voor regressiemodellering

In deze zelfstudie leert u hoe u gegevens voorbereidt voor regressiemodellering met de Azure Machine Learning Data Prep SDK. Voer diverse transformaties uit om twee verschillende gegevenssets van NYC Taxi te filteren en combineren. Het einddoel van deze zelfstudie bestaat uit het voorspellen van de kosten van een taxirit door een model te trainen aan de hand van gegevenseigenschappen, waaronder het tijdstip van ophalen, dag van de week, aantal passagiers en de coördinaten. Deze zelfstudie is deel één van een serie van twee.

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Stelt u een Python-omgeving in en importeert u pakketten
> * Laadt u twee gegevenssets met verschillende veldnamen
> * Schoont u gegevens op om afwijkingen te verwijderen
> * Transformeert u gegevens met behulp van intelligente transformaties om nieuwe eigenschappen te maken
> * Slaat u uw gegevensstroomobject op voor gebruik in een regressiemodel

U kunt uw gegevens in Python voorbereiden met de [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk).

## <a name="get-the-notebook"></a>De notebook ophalen

Voor uw gemak is deze zelfstudie beschikbaar gemaakt als een [Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/regression-part1-data-prep.ipynb). Voer het `regression-part1-data-prep.ipynb`-notebook uit in Azure Notebooks of op uw eigen Jupyter-notebookserver.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

## <a name="import-packages"></a>Pakketten importeren

U begint met het importeren van de SDK.


```python
import azureml.dataprep as dprep
```

## <a name="load-data"></a>Gegevens laden

Download twee verschillende gegevenssets van NYC Taxi in gegevensstroomobjecten.  Deze gegevenssets bevatten enigszins afwijkende velden. Met methode `auto_read_file()` wordt het invoerbestandstype automatisch herkend.


```python
dataset_root = "https://dprepdata.blob.core.windows.net/demo"

green_path = "/".join([dataset_root, "green-small/*"])
yellow_path = "/".join([dataset_root, "yellow-small/*"])

green_df = dprep.read_csv(path=green_path, header=dprep.PromoteHeadersMode.GROUPED)
# auto_read_file will automatically identify and parse the file type, and is useful if you don't know the file type
yellow_df = dprep.auto_read_file(path=yellow_path)

display(green_df.head(5))
display(yellow_df.head(5))
```

## <a name="cleanse-data"></a>Gegevens opschonen

U vult nu een aantal variabelen in met snelkoppelingstransformaties die voor alle gegevensstromen geldig zijn. De variabele `drop_if_all_null` wordt gebruikt om records te verwijderen waarvan alle velden null zijn. De variabele `useful_columns` bevat een array met kolombeschrijvingen die in elke gegevensstroom worden bewaard.

```python
all_columns = dprep.ColumnSelector(term=".*", use_regex=True)
drop_if_all_null = [all_columns, dprep.ColumnRelationship(dprep.ColumnRelationship.ALL)]
useful_columns = [
    "cost", "distance""distance", "dropoff_datetime", "dropoff_latitude", "dropoff_longitude",
    "passengers", "pickup_datetime", "pickup_latitude", "pickup_longitude", "store_forward", "vendor"
]
```

U gebruikt eerst de gegevens van de groene taxi’s en geeft deze een geldige vorm die kan worden gecombineerd met de gegevens van de gele taxi’s. Maak een tijdelijke gegevensstroom `tmp_df`. Roep de functies `replace_na()`, `drop_nulls()` en `keep_columns()` aan met de variabelen van de snelkoppelingstransformatie die u hebt gemaakt. Geef verder alle kolommen in het gegevensframe een andere naam, zodat ze overeenkomen met de namen in `useful_columns`.


```python
tmp_df = (green_df
    .replace_na(columns=all_columns)
    .drop_nulls(*drop_if_all_null)
    .rename_columns(column_pairs={
        "VendorID": "vendor",
        "lpep_pickup_datetime": "pickup_datetime",
        "Lpep_dropoff_datetime": "dropoff_datetime",
        "lpep_dropoff_datetime": "dropoff_datetime",
        "Store_and_fwd_flag": "store_forward",
        "store_and_fwd_flag": "store_forward",
        "Pickup_longitude": "pickup_longitude",
        "Pickup_latitude": "pickup_latitude",
        "Dropoff_longitude": "dropoff_longitude",
        "Dropoff_latitude": "dropoff_latitude",
        "Passenger_count": "passengers",
        "Fare_amount": "cost",
        "Trip_distance": "distance"
     })
    .keep_columns(columns=useful_columns))
tmp_df.head(5)
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
      <th>leverancier</th>
      <th>pickup_datetime</th>
      <th>dropoff_datetime</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>passagiers</th>
      <th>kosten</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 08:14:37</td>
      <td>2013-08-01 09:09:06</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>21,25</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 09:13:00</td>
      <td>2013-08-01 11:38:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>74.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-01 09:48:00</td>
      <td>2013-08-01 09:49:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-01 10:38:35</td>
      <td>2013-08-01 10:38:51</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-01 11:51:45</td>
      <td>2013-08-01 12:03:52</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>8.5</td>
    </tr>
  </tbody>
</table>
</div>



Overschrijf de variabele `green_df` met de transformaties die in de vorige stap op `tmp_df` zijn uitgevoerd.


```python
green_df = tmp_df
```

Voer dezelfde transformatiestappen uit op de gegevens van de gele taxi’s.


```python
tmp_df = (yellow_df
    .replace_na(columns=all_columns)
    .drop_nulls(*drop_if_all_null)
    .rename_columns(column_pairs={
        "vendor_name": "vendor",
        "VendorID": "vendor",
        "vendor_id": "vendor",
        "Trip_Pickup_DateTime": "pickup_datetime",
        "tpep_pickup_datetime": "pickup_datetime",
        "Trip_Dropoff_DateTime": "dropoff_datetime",
        "tpep_dropoff_datetime": "dropoff_datetime",
        "store_and_forward": "store_forward",
        "store_and_fwd_flag": "store_forward",
        "Start_Lon": "pickup_longitude",
        "Start_Lat": "pickup_latitude",
        "End_Lon": "dropoff_longitude",
        "End_Lat": "dropoff_latitude",
        "Passenger_Count": "passengers",
        "passenger_count": "passengers",
        "Fare_Amt": "cost",
        "fare_amount": "cost",
        "Trip_Distance": "distance",
        "trip_distance": "distance"
    })
    .keep_columns(columns=useful_columns))
tmp_df.head(5)
```

Overschrijf opnieuw `yellow_df` met `tmp_df` en roep de functie `append_rows()` aan voor de gegevens van de groene taxi’s om de gegevens van de gele taxi’s toe te voegen om zodoende een samengesteld gegevensframe te maken.


```python
yellow_df = tmp_df
combined_df = green_df.append_rows([yellow_df])
```

### <a name="convert-types-and-filter"></a>Typen en filters converteren 

Bekijk de overzichtsstatistieken met de coördinaten voor het afhalen en afzetten om te zien hoe de gegevens zijn verdeeld. Definieer eerst een `TypeConverter`-object om de velden lengte/breedte te wijzigen in het type decimaal. Roep vervolgens de functie `keep_columns()` aan om de uitvoer te beperken tot de velden lengte/breedte. Roep vervolgens `get_profile()` aan.


```python
decimal_type = dprep.TypeConverter(data_type=dprep.FieldType.DECIMAL)
combined_df = combined_df.set_column_types(type_conversions={
    "pickup_longitude": decimal_type,
    "pickup_latitude": decimal_type,
    "dropoff_longitude": decimal_type,
    "dropoff_latitude": decimal_type
})
combined_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude", 
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min.</th>
      <th>Max.</th>
      <th>Count</th>
      <th>Ontbrekend aantal</th>
      <th>Niet-ontbrekend aantal</th>
      <th>Ontbrekend percentage</th>
      <th>Aantal fouten</th>
      <th>Leeg aantal</th>
      <th>0,1%-kwantiel</th>
      <th>1%-kwantiel</th>
      <th>5%-kwantiel</th>
      <th>25%-kwantiel</th>
      <th>50%-kwantiel</th>
      <th>75%-kwantiel</th>
      <th>95%-kwantiel</th>
      <th>99%-kwantiel</th>
      <th>99,9%-kwantiel</th>
      <th>Standaardafwijking</th>
      <th>Gemiddelde</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115,179337</td>
      <td>0,000000</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>-88,114046</td>
      <td>-73,961840</td>
      <td>-73,961964</td>
      <td>-73,947693</td>
      <td>-73,922097</td>
      <td>-73,846670</td>
      <td>0,000000</td>
      <td>0,000000</td>
      <td>0,000000</td>
      <td>18,792672</td>
      <td>-68,833579</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0,000000</td>
      <td>40,919121</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,000000</td>
      <td>40,682889</td>
      <td>40,675541</td>
      <td>40,721075</td>
      <td>40,756159</td>
      <td>40,803909</td>
      <td>40.849406</td>
      <td>40,870681</td>
      <td>40,891244</td>
      <td>10,345967</td>
      <td>37,936742</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115,179337</td>
      <td>0,000000</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>-87,699611</td>
      <td>-73,984734</td>
      <td>-73,985777</td>
      <td>-73,956250</td>
      <td>-73,928948</td>
      <td>-73,866208</td>
      <td>0,000000</td>
      <td>0,000000</td>
      <td>0,000000</td>
      <td>18,696526</td>
      <td>-68,896978</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0,000000</td>
      <td>41,008934</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>7722,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,000000</td>
      <td>40,662763</td>
      <td>40,654851</td>
      <td>40,717821</td>
      <td>40,756534</td>
      <td>40,784688</td>
      <td>40,852437</td>
      <td>40,879289</td>
      <td>40,937291</td>
      <td>10,290780</td>
      <td>37,963774</td>
    </tr>
  </tbody>
</table>



In de uitvoer van de overzichtsstatistieken ziet u dat er coördinaten ontbreken en coördinaten die niet in New York City zijn. Filter de coördinaten eruit die niet aanwezig zijn in de stadsgrens door kolomfilteropdrachten binnen de functie `filter()` te koppelen en voor elk veld minimum- en maximumgrenzen te definiëren. Roep vervolgens opnieuw `get_profile()` aan om de transformatie te verifiëren.


```python
tmp_df = (combined_df
    .drop_nulls(
        columns=["pickup_longitude", "pickup_latitude", "dropoff_longitude", "dropoff_latitude"],
        column_relationship=dprep.ColumnRelationship(dprep.ColumnRelationship.ANY)
    ) 
    .filter(dprep.f_and(
        dprep.col("pickup_longitude") <= -73.72,
        dprep.col("pickup_longitude") >= -74.09,
        dprep.col("pickup_latitude") <= 40.88,
        dprep.col("pickup_latitude") >= 40.53,
        dprep.col("dropoff_longitude") <= -73.72,
        dprep.col("dropoff_longitude") >= -74.09,
        dprep.col("dropoff_latitude") <= 40.88,
        dprep.col("dropoff_latitude") >= 40.53
    )))
tmp_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude", 
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min.</th>
      <th>Max.</th>
      <th>Count</th>
      <th>Ontbrekend aantal</th>
      <th>Niet-ontbrekend aantal</th>
      <th>Ontbrekend percentage</th>
      <th>Aantal fouten</th>
      <th>Leeg aantal</th>
      <th>0,1%-kwantiel</th>
      <th>1%-kwantiel</th>
      <th>5%-kwantiel</th>
      <th>25%-kwantiel</th>
      <th>50%-kwantiel</th>
      <th>75%-kwantiel</th>
      <th>95%-kwantiel</th>
      <th>99%-kwantiel</th>
      <th>99,9%-kwantiel</th>
      <th>Standaardafwijking</th>
      <th>Gemiddelde</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74,078156</td>
      <td>-73,736481</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>-74,076314</td>
      <td>-73,962542</td>
      <td>-73,962893</td>
      <td>-73,948975</td>
      <td>-73,927856</td>
      <td>-73,866662</td>
      <td>-73,830438</td>
      <td>-73,823160</td>
      <td>-73,769750</td>
      <td>0,048711</td>
      <td>-73,913865</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40,575485</td>
      <td>40,879852</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>40,632884</td>
      <td>40,713105</td>
      <td>40,711600</td>
      <td>40,721403</td>
      <td>40,758142</td>
      <td>40,805145</td>
      <td>40,848855</td>
      <td>40,867567</td>
      <td>40,877690</td>
      <td>0,048348</td>
      <td>40,765226</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74,085747</td>
      <td>-73,720871</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>-74,078828</td>
      <td>-73,985650</td>
      <td>-73,985813</td>
      <td>-73,959041</td>
      <td>-73,936681</td>
      <td>-73,884846</td>
      <td>-73,815507</td>
      <td>-73,776697</td>
      <td>-73,733471</td>
      <td>0,055961</td>
      <td>-73,920718</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40,583530</td>
      <td>40,879734</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>40,597741</td>
      <td>40,695376</td>
      <td>40,695115</td>
      <td>40,727549</td>
      <td>40,758160</td>
      <td>40,788378</td>
      <td>40,850372</td>
      <td>40,867968</td>
      <td>40,878586</td>
      <td>0,050462</td>
      <td>40,759487</td>
    </tr>
  </tbody>
</table>



Overschrijf `combined_df` met de transformaties die u op `tmp_df` hebt uitgevoerd.


```python
combined_df = tmp_df
```

### <a name="split-and-rename-columns"></a>Kolommen splitsen en een andere naam geven

Kijk naar het gegevensprofiel voor kolom `store_forward`.


```python
combined_df.keep_columns(columns='store_forward').get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min.</th>
      <th>Max.</th>
      <th>Count</th>
      <th>Ontbrekend aantal</th>
      <th>Niet-ontbrekend aantal</th>
      <th>Ontbrekend percentage</th>
      <th>Aantal fouten</th>
      <th>Leeg aantal</th>
      <th>0,1%-kwantiel</th>
      <th>1%-kwantiel</th>
      <th>5%-kwantiel</th>
      <th>25%-kwantiel</th>
      <th>50%-kwantiel</th>
      <th>75%-kwantiel</th>
      <th>95%-kwantiel</th>
      <th>99%-kwantiel</th>
      <th>99,9%-kwantiel</th>
      <th>Standaardafwijking</th>
      <th>Gemiddelde</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>J</td>
      <td>7059,0</td>
      <td>99,0</td>
      <td>6960,0</td>
      <td>0,014025</td>
      <td>0,0</td>
      <td>0,0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>



Aan de uitvoer van het gegevensprofiel van `store_forward` ziet u dat de gegevens inconsistent zijn en dat er ontbrekende of null-waarden zijn. Vervang deze waarden met behulp van de functies `replace()` en `fill_nulls()`, en vervang ze in beide gevallen door de tekenreeks N.


```python
combined_df = combined_df.replace(columns="store_forward", find="0", replace_with="N").fill_nulls("store_forward", "N")
```

Splits de tijdstippen voor afhalen en afzetten in kolommen voor datum respectievelijk tijdstip. Gebruik `split_column_by_example()` om de splitsing uit te voeren. In dit geval wordt de optionele parameter `example` van `split_column_by_example()` weggelaten. De functie zal daarom aan de hand van de gegevens automatisch bepalen waar moet worden gesplitst.


```python
tmp_df = (combined_df
    .split_column_by_example(source_column="pickup_datetime")
    .split_column_by_example(source_column="dropoff_datetime"))
tmp_df.head(5)
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
      <th>leverancier</th>
      <th>pickup_datetime</th>
      <th>pickup_datetime_1</th>
      <th>pickup_datetime_2</th>
      <th>dropoff_datetime</th>
      <th>dropoff_datetime_1</th>
      <th>dropoff_datetime_2</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>passagiers</th>
      <th>kosten</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>N</td>
      <td>-73,937767</td>
      <td>40,758480</td>
      <td>-73,937767</td>
      <td>40,758480</td>
      <td>1</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>2013-08-01</td>
      <td>17:24:00</td>
      <td>2013-08-01 17:25:00</td>
      <td>2013-08-01</td>
      <td>17:25:00</td>
      <td>N</td>
      <td>-73,937927</td>
      <td>40,757843</td>
      <td>-73,937927</td>
      <td>40,757843</td>
      <td>1</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>2013-08-06</td>
      <td>06:51:19</td>
      <td>2013-08-06 06:51:36</td>
      <td>2013-08-06</td>
      <td>06:51:36</td>
      <td>N</td>
      <td>-73,937721</td>
      <td>40,758404</td>
      <td>-73,937721</td>
      <td>40,758369</td>
      <td>1</td>
      <td>3,3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>2013-08-06</td>
      <td>13:26:34</td>
      <td>2013-08-06 13:26:57</td>
      <td>2013-08-06</td>
      <td>13:26:57</td>
      <td>N</td>
      <td>-73,937691</td>
      <td>40,758419</td>
      <td>-73,937790</td>
      <td>40,758358</td>
      <td>1</td>
      <td>3,3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>2013-08-06</td>
      <td>13:27:53</td>
      <td>2013-08-06 13:28:08</td>
      <td>2013-08-06</td>
      <td>13:28:08</td>
      <td>N</td>
      <td>-73,937805</td>
      <td>40,758396</td>
      <td>-73,937775</td>
      <td>40,758450</td>
      <td>1</td>
      <td>3,3</td>
    </tr>
  </tbody>
</table>
</div>



Geef de kolommen die door `split_column_by_example()` zijn gegenereerd een nieuwe, zinvolle naam.


```python
tmp_df_renamed = (tmp_df
    .rename_columns(column_pairs={
        "pickup_datetime_1": "pickup_date",
        "pickup_datetime_2": "pickup_time",
        "dropoff_datetime_1": "dropoff_date",
        "dropoff_datetime_2": "dropoff_time"
    }))
tmp_df_renamed.head(5)
```

Overschrijf `combined_df` met de uitgevoerde transformaties en roep `get_profile()` aan om de volledige overzichtsstatistieken te zien nadat alle transformaties zijn uitgevoerd.


```python
combined_df = tmp_df_renamed
combined_df.get_profile()
```

## <a name="transform-data"></a>Gegevens transformeren

Splits de afhaal- en afzetgegevens verder op in dag van de week, dag van de maand en maand. Gebruik de functie `derive_column_by_example()` voor de dag van de week. Deze functie gebruikt als parameter een array van voorbeeldobjecten die de invoergegevens en de gewenste uitvoergegevens definiëren. Vervolgens bepaalt de functie automatisch de gewenste transformatie. Gebruik de functie `split_column_by_example()` zonder voorbeeldparameter voor kolommen met afhaal- en afzettijden, gesplitst in uren, minuten en seconden.

Als u deze nieuwe functies hebt gegenereerd, verwijdert u met `drop_columns()` de oorspronkelijke velden ten faveure van de nieuw gegenereerde functies. Geef de resterende velden een nieuwe, nauwkeurige naam.


```python
tmp_df = (combined_df
    .derive_column_by_example(
        source_columns="pickup_date", 
        new_column_name="pickup_weekday", 
        example_data=[("2009-01-04", "Sunday"), ("2013-08-22", "Thursday")]
    )
    .derive_column_by_example(
        source_columns="dropoff_date",
        new_column_name="dropoff_weekday",
        example_data=[("2013-08-22", "Thursday"), ("2013-11-03", "Sunday")]
    )
          
    .split_column_by_example(source_column="pickup_time")
    .split_column_by_example(source_column="dropoff_time")
    # the following two split_column_by_example calls reference the generated column names from the above two calls
    .split_column_by_example(source_column="pickup_time_1")
    .split_column_by_example(source_column="dropoff_time_1")
    .drop_columns(columns=[
        "pickup_date", "pickup_time", "dropoff_date", "dropoff_time", 
        "pickup_date_1", "dropoff_date_1", "pickup_time_1", "dropoff_time_1"
    ])
          
    .rename_columns(column_pairs={
        "pickup_date_2": "pickup_month",
        "pickup_date_3": "pickup_monthday",
        "pickup_time_1_1": "pickup_hour",
        "pickup_time_1_2": "pickup_minute",
        "pickup_time_2": "pickup_second",
        "dropoff_date_2": "dropoff_month",
        "dropoff_date_3": "dropoff_monthday",
        "dropoff_time_1_1": "dropoff_hour",
        "dropoff_time_1_2": "dropoff_minute",
        "dropoff_time_2": "dropoff_second"
    }))

tmp_df.head(5)
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
      <th>leverancier</th>
      <th>pickup_datetime</th>
      <th>pickup_weekday</th>
      <th>pickup_hour</th>
      <th>pickup_minute</th>
      <th>pickup_second</th>
      <th>dropoff_datetime</th>
      <th>dropoff_weekday</th>
      <th>dropoff_hour</th>
      <th>dropoff_minute</th>
      <th>dropoff_second</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>passagiers</th>
      <th>kosten</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>Donderdag</td>
      <td>17</td>
      <td>22</td>
      <td>00</td>
      <td>2013-08-01 17:22:00</td>
      <td>Donderdag</td>
      <td>17</td>
      <td>22</td>
      <td>00</td>
      <td>N</td>
      <td>-73,937767</td>
      <td>40,758480</td>
      <td>-73,937767</td>
      <td>40,758480</td>
      <td>1</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>Donderdag</td>
      <td>17</td>
      <td>24</td>
      <td>00</td>
      <td>2013-08-01 17:25:00</td>
      <td>Donderdag</td>
      <td>17</td>
      <td>25</td>
      <td>00</td>
      <td>N</td>
      <td>-73,937927</td>
      <td>40,757843</td>
      <td>-73,937927</td>
      <td>40,757843</td>
      <td>1</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>Dinsdag</td>
      <td>06</td>
      <td>51</td>
      <td>19</td>
      <td>2013-08-06 06:51:36</td>
      <td>Dinsdag</td>
      <td>06</td>
      <td>51</td>
      <td>36</td>
      <td>N</td>
      <td>-73,937721</td>
      <td>40,758404</td>
      <td>-73,937721</td>
      <td>40,758369</td>
      <td>1</td>
      <td>3,3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>Dinsdag</td>
      <td>13</td>
      <td>26</td>
      <td>34</td>
      <td>2013-08-06 13:26:57</td>
      <td>Dinsdag</td>
      <td>13</td>
      <td>26</td>
      <td>57</td>
      <td>N</td>
      <td>-73,937691</td>
      <td>40,758419</td>
      <td>-73,937790</td>
      <td>40,758358</td>
      <td>1</td>
      <td>3,3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>Dinsdag</td>
      <td>13</td>
      <td>27</td>
      <td>53</td>
      <td>2013-08-06 13:28:08</td>
      <td>Dinsdag</td>
      <td>13</td>
      <td>28</td>
      <td>08</td>
      <td>N</td>
      <td>-73,937805</td>
      <td>40,758396</td>
      <td>-73,937775</td>
      <td>40,758450</td>
      <td>1</td>
      <td>3,3</td>
    </tr>
  </tbody>
</table>
</div>



Uit de bovenstaande gegevens ziet u dat de componenten van de datums en de tijdstippen voor afhalen en afzetten die uit de afgeleide transformaties afkomstig zijn, juist zijn. Verwijder kolom `pickup_datetime` en `dropoff_datetime`. U hebt ze niet langer nodig.


```python
tmp_df = tmp_df.drop_columns(columns=["pickup_datetime", "dropoff_datetime"])
```

Gebruik de functionaliteit type-inferentie om automatisch het gegevenstype van elk veld te controleren. Geef vervolgens de resultaten van de inferentie weer.


```python
type_infer = tmp_df.builders.set_column_types()
type_infer.learn()
type_infer
```




    {'pickup_weekday': [FieldType.STRING],
     'pickup_hour': [FieldType.DECIMAL],
     'pickup_second': [FieldType.DECIMAL],
     'dropoff_hour': [FieldType.DECIMAL],
     'dropoff_minute': [FieldType.DECIMAL],
     'dropoff_second': [FieldType.DECIMAL],
     'store_forward': [FieldType.STRING],
     'pickup_minute': [FieldType.DECIMAL],
     'dropoff_weekday': [FieldType.STRING],
     'vendor': [FieldType.STRING],
     'pickup_longitude': [FieldType.DECIMAL],
     'pickup_latitude': [FieldType.DECIMAL],
     'dropoff_longitude': [FieldType.DECIMAL],
     'dropoff_latitude': [FieldType.DECIMAL],
     'passengers': [FieldType.DECIMAL],
     'cost': [FieldType.DECIMAL]}



Deze resultaten zien er op basis van de gegevens correct uit. Pas nu de typeconversies op de gegevensstroom toe.


```python
tmp_df = type_infer.to_dataflow()
tmp_df.get_profile()
```

Op dit punt aangekomen, hebt u een volledig getransformeerd en voorbereid gegevensstroomobject om in een machine-learningmodel te gebruiken. De SDK bevat een functie voor objectserialisatie. Deze wordt als volgt gebruikt.


```python
dflow_prepared = tmp_df
package = dprep.Package([dflow_prepared])
package.save(".\dflow")
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder bestand `dflow` (ongeacht of lokaal of in Azure Notebooks werkt) uit de huidige map als u niet wilt doorgaan met deel twee van de zelfstudie. Als u doorgaat met deel twee, hebt u bestand `dflow` in de huidige map nodig.

## <a name="next-steps"></a>Volgende stappen

In deel één van deze zelfstudie hebt u:

> [!div class="checklist"]
> * De ontwikkelomgeving ingesteld
> * Gegevenssets geladen en opgeschoond
> * Slimme transformaties gebruikt om de logica te voorspellen op basis van een voorbeeld
> * Gegevenssets samengevoegd en verpakt voor het trainen van de machine learning

U kunt deze trainingsgegevens gebruiken in het volgende deel van de zelfstudiereeks:

> [!div class="nextstepaction"]
> [Zelfstudie 2: regressiemodel trainen](tutorial-auto-train-models.md)
