---
title: 'Zelfstudie voor een regressiemodel: Gegevens voorbereiden'
titleSuffix: Azure Machine Learning service
description: In het eerste deel van deze zelfstudie leert u hoe u in Python gegevens voorbereidt voor regressiemodellering met de Azure Machine Learning-SDK.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sihhu
ms.author: MayMSFT
ms.reviewer: trbye
ms.date: 02/04/2019
ms.custom: seodec18
ms.openlocfilehash: 0ee49299dbbe8095ef98be51dc8619c28891abc5
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361384"
---
# <a name="tutorial-prepare-data-for-regression-modeling"></a>Zelfstudie: Gegevens voorbereiden voor regressiemodellering

In deze zelfstudie leert u hoe u gegevens voorbereidt voor regressiemodellering met de [Azure Machine Learning Data Prep SDK voor Python](https://aka.ms/data-prep-sdk). U gaat verschillende transformaties uitvoeren om twee verschillende gegevenssets van NYC Taxi te filteren en combineren.

Deze zelfstudie is **deel één van een serie van twee**. Nadat u deze serie met zelfstudies hebt voltooid, kunt u de kosten van een taxirit voorspellen door een model te trainen op gegevenskenmerken. Deze kenmerken zijn onder andere de dag en het tijdstip van ophalen, het aantal passagiers en de vertreklocatie.

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Een Python-omgeving inrichten en pakketten importeren
> * Twee gegevenssets met verschillende veldnamen laden
> * Gegevens opschonen om afwijkingen te verwijderen
> * Gegevens transformeren door met behulp van intelligente transformaties nieuwe kenmerken te maken
> * Het gegevensstroomobject opslaan voor gebruik in een regressiemodel

## <a name="prerequisites"></a>Vereisten

Ga naar [De ontwikkelomgeving instellen](#start) om de notebook-stappen te doorlopen, of gebruik de onderstaande instructies om het notebook op te halen en uit te voeren op Azure Notebooks of uw eigen notebookserver. U hebt het volgende nodig om het notebook uit te voeren:

* Een Python 3.6 notebook-server met het volgende geïnstalleerd:
    * De Azure Machine Learning Data Prep SDK voor Python
* Het notebook voor de zelfstudie

Haal al deze vereisten op uit een van de secties hieronder.

* [Azure Notebooks](#azure) gebruiken
* [Uw eigen Notebook-server](#server) gebruiken

### <a name="azure"></a>Azure Notebooks gebruiken: Gratis Jupyter-notebooks in de cloud

U kunt eenvoudig aan de slag met Azure Notebooks. De Azure Machine Learning Data Prep SDK is al voor u geïnstalleerd en geconfigureerd in [Azure Notebooks](https://notebooks.azure.com/). De installatie en toekomstige updates worden automatisch beheerd via Azure-services.

Nadat u de onderstaande stappen hebt uitgevoerd, voert u het notebook **tutorials/regression-part1-data-prep.ipynb** in het project **Aan de slag** uit.

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

### <a name="server"></a>Uw eigen Jupyter Notebook-server gebruiken

Volg deze stappen om een lokale Jupyter Notebook-server te maken op uw computer.  Nadat u de stappen hebt uitgevoerd, voert u het notebook **tutorials/regression-part1-data-prep.ipynb** uit.

1. Volledige installatie van de stappen in [ [Azure Machine Learning Python-quickstart](quickstart-run-local-notebook.md) ](setup-create-workspace.md#python) om een Miniconda-omgeving te maken.  U kunt het gedeelte **Een werkruimte maken** nu desgewenst overslaan, maar dit gedeelte is wel vereist voor [deel 2](tutorial-auto-train-models.md) van deze reeks zelfstudies.
1. Installeer de Data Prep SDK in uw omgeving met `pip install azureml-dataprep`.
1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Start de notebookserver vanuit de gekloonde map.

    ```shell
    jupyter notebook
    ```

## <a name="start"></a>De ontwikkelomgeving instellen

De configuratie van uw ontwikkelomgeving kan worden uitgevoerd met een Python-notebook. De configuratie bestaat uit de volgende acties:

* De SDK installeren
* Python-pakketten importeren

### <a name="install-and-import-packages"></a>Installeren en importeren van pakketten

Gebruik het volgende nodig om pakketten te installeren als u deze nog niet hebt.

```shell
pip install azureml-dataprep
```

Importeer de SDK.

```python
import azureml.dataprep as dprep
```

## <a name="load-data"></a>Gegevens laden

Download twee verschillende gegevenssets van NYC Taxi in gegevensstroomobjecten. De gegevenssets hebben iets afwijkende velden. Met de methode `auto_read_file()` wordt automatisch het type invoerbestand herkend.

```python
from IPython.display import display
dataset_root = "https://dprepdata.blob.core.windows.net/demo"

green_path = "/".join([dataset_root, "green-small/*"])
yellow_path = "/".join([dataset_root, "yellow-small/*"])

green_df_raw = dprep.read_csv(path=green_path, header=dprep.PromoteHeadersMode.GROUPED)
# auto_read_file automatically identifies and parses the file type, which is useful when you don't know the file type.
yellow_df_raw = dprep.auto_read_file(path=yellow_path)

display(green_df_raw.head(5))
display(yellow_df_raw.head(5))
```

`Dataflow`-objecten zijn vergelijkbaar met gegevensframes en vertegenwoordigen een reeks traag geëvalueerde, onveranderbare bewerkingen voor gegevens. Bewerkingen kunnen worden toegevoegd door het aanroepen van de verschillende transformatie- en filtermethoden die beschikbaar zijn. Wanneer een bewerking wordt toegevoegd aan een `Dataflow`, is het resultaat altijd een nieuw `Dataflow`-object.

## <a name="cleanse-data"></a>Gegevens opschonen

U gaat nu enkele variabelen vullen met snelkoppelingstransformaties die op alle gegevensstromen van toepassing zijn. De variabele `drop_if_all_null` wordt gebruikt om records te verwijderen waarvan alle velden null zijn. De variabele `useful_columns` bevat een matrix met kolombeschrijvingen die in elke gegevensstroom worden bewaard.

```python
all_columns = dprep.ColumnSelector(term=".*", use_regex=True)
drop_if_all_null = [all_columns, dprep.ColumnRelationship(dprep.ColumnRelationship.ALL)]
useful_columns = [
    "cost", "distance", "dropoff_datetime", "dropoff_latitude", "dropoff_longitude",
    "passengers", "pickup_datetime", "pickup_latitude", "pickup_longitude", "store_forward", "vendor"
]
```

U gebruikt eerst de gegevens van de groene taxi’s en geeft deze een geldige vorm die kan worden gecombineerd met de gegevens van de gele taxi’s. Roep de functies `replace_na()`, `drop_nulls()` en `keep_columns()` aan met de variabelen voor snelkoppelingstransformaties die u eerder hebt gemaakt. Geef verder alle kolommen in het gegevensframe een andere naam, zodat deze overeenkomen met de namen in de variabele `useful_columns`.


```python
green_df = (green_df_raw
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
green_df.head(5)
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
      <th>afstand</th>
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
      <td>.00</td>
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
      <td>.00</td>
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
      <td>.00</td>
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
      <td>.00</td>
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
      <td>.00</td>
      <td>8.5</td>
    </tr>
  </tbody>
</table>
</div>

Voer dezelfde transformatiestappen uit op de gegevens van de gele taxi’s. Met deze functies wordt ervoor gezorgd dat lege gegevens uit de gegevensset worden verwijderd; dit vergroot de nauwkeurigheid van het Machine Learning-model.

```python
yellow_df = (yellow_df_raw
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
yellow_df.head(5)
```

Roep de functie `append_rows()` aan voor de groene taxi's om de gegevens van de gele taxi's toe te voegen. Er wordt een nieuw, gecombineerd dataframe gemaakt.

```python
combined_df = green_df.append_rows([yellow_df])
```

### <a name="convert-types-and-filter"></a>Typen en filters converteren

Bekijk de overzichtsstatistieken met de coördinaten voor het afhalen en afzetten om te zien hoe de gegevens zijn verdeeld. Definieer eerst een `TypeConverter`-object om de velden voor de lengte- en de breedtegraad te wijzigen in het type decimaal. Roep vervolgens de functie `keep_columns()` aan om de uitvoer te beperken tot de velden voor de lengte- en breedtegraad. Roep vervolgens `get_profile()` aan. Met deze functieaanroepen wordt een beknopte weergave van de gegevensstroom gemaakt waarbij alleen de velden voor de lengte- en breedtegraad worden weergegeven. Dit maakt het eenvoudiger om te controleren op ontbrekende coördinaten en coördinaten die buiten het bereik liggen.


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



In de uitvoer van de overzichtsstatistieken ziet u dat er ontbrekende coördinaten zijn en coördinaten die zich niet in New York City bevinden (dit wordt bepaald op basis van een subjectieve analyse). Filter de coördinaten uit voor locaties die zich buiten de stadsgrens bevinden. Schakel de kolomfilteropdrachten aaneen binnen de functie `filter()` en definieer de minimum- en maximumgrenzen voor elk veld. Roep vervolgens opnieuw de functie `get_profile()` aan om de transformatie te verifiëren.


```python
latlong_filtered_df = (combined_df
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
latlong_filtered_df.keep_columns(columns=[
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

### <a name="split-and-rename-columns"></a>Kolommen splitsen en een andere naam geven

Kijk naar het gegevensprofiel voor kolom `store_forward`. Dit veld is een Booleaanse vlag. De vlag is `Y` als de taxi na een rit geen verbinding heeft met de server en de rit dus heeft moeten opslaan in het geheugen. Wanneer er weer verbinding is, wordt de rit doorgestuurd naar de server.


```python
latlong_filtered_df.keep_columns(columns='store_forward').get_profile()
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



Aan de uitvoer van het gegevensprofiel in de kolom `store_forward` ziet u dat de gegevens inconsistent zijn en dat er ontbrekende of null-waarden zijn. Gebruik de functies `replace()` en `fill_nulls()` om deze waarden te vervangen door de tekenreeks 'N':


```python
replaced_stfor_vals_df = latlong_filtered_df.replace(columns="store_forward", find="0", replace_with="N").fill_nulls("store_forward", "N")
```

Voer de functie `replace` uit op het veld `distance`. Hiermee worden de afstandswaarden die onjuist zijn gelabeld als `.00` opnieuw ingedeeld, en worden null-waarden met nullen ingevuld. Converteer het veld `distance` naar een numerieke indeling. Deze onjuiste gegevenspunten zijn waarschijnlijk het resultaat van afwijkingen in het gegevensverzamelingssysteem in taxi's.


```python
replaced_distance_vals_df = replaced_stfor_vals_df.replace(columns="distance", find=".00", replace_with=0).fill_nulls("distance", 0)
replaced_distance_vals_df = replaced_distance_vals_df.to_number(["distance"])
```

Splits de datum/tijd-waarden voor afhalen en afzetten in kolommen voor datum respectievelijk tijdstip. Gebruik de functie `split_column_by_example()` voor het maken van de splitsing. In dit geval wordt de optionele parameter `example` van de functie `split_column_by_example()` weggelaten. De functie zal daarom aan de hand van de gegevens automatisch bepalen waar moet worden gesplitst.


```python
time_split_df = (replaced_distance_vals_df
    .split_column_by_example(source_column="pickup_datetime")
    .split_column_by_example(source_column="dropoff_datetime"))
time_split_df.head(5)
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
      <th>afstand</th>
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
      <td>0,0</td>
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
      <td>0,0</td>
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
      <td>0,0</td>
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
      <td>0,0</td>
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
      <td>0,0</td>
      <td>3,3</td>
    </tr>
  </tbody>
</table>
</div>

Geef de kolommen die door de functie `split_column_by_example()` zijn gegenereerd een nieuwe, zinvolle naam.

```python
renamed_col_df = (time_split_df
    .rename_columns(column_pairs={
        "pickup_datetime_1": "pickup_date",
        "pickup_datetime_2": "pickup_time",
        "dropoff_datetime_1": "dropoff_date",
        "dropoff_datetime_2": "dropoff_time"
    }))
renamed_col_df.head(5)
```

Roep de functie `get_profile()` aan om de volledige overzichtsstatistieken te zien na alle opschoningsstappen.

```python
renamed_col_df.get_profile()
```

## <a name="transform-data"></a>Gegevens transformeren

Splits de afhaal- en afzetgegevens verder op in dag van de week, dag van de maand en maand. De waarde voor de dag van de week kunt u ophalen met de functie `derive_column_by_example()`. Deze functie accepteert een matrixparameter met voorbeeldobjecten die de invoergegevens en de gewenste uitvoer definiëren. Vervolgens bepaalt de functie automatisch de gewenste transformatie. Gebruik de functie `split_column_by_example()` zonder voorbeeldparameter voor kolommen met afhaal- en afzettijden, gesplitst in uren, minuten en seconden.

Na het genereren van de nieuwe functies, gebruikt u de functie `drop_columns()` voor het verwijderen van de oorspronkelijke velden aangezien de zojuist gegenereerde functies de voorkeur hebben. Wijzig de naam van de rest van de velden om deze beter herkenbaar te maken.

Door gegevens op deze manier te transformeren om nieuwe, tijdgebaseerde functies te maken, kan de nauwkeurigheid van het Machine Learning-model worden verbeterd. Als u bijvoorbeeld een nieuwe functie maakt voor weekdagen, kan er een relatie worden gemaakt tussen dagen van de week en de taxiritprijs. Vaak is de ritprijs duurder op bepaalde weekdagen omdat de vraag dan groter is.


```python
transformed_features_df = (renamed_col_df
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
    # The following two calls to split_column_by_example reference the column names generated from the previous two calls.
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

transformed_features_df.head(5)
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
      <th>afstand</th>
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
      <td>0,0</td>
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
      <td>0,0</td>
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
      <td>0,0</td>
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
      <td>0,0</td>
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
      <td>0,0</td>
      <td>3,3</td>
    </tr>
  </tbody>
</table>
</div>

Uit de gegevens kunt u afleiden dat de componenten van de datums en de tijdstippen voor afhalen en afzetten die uit de afgeleide transformaties afkomstig zijn, juist zijn. Verwijder de kolommen `pickup_datetime` en `dropoff_datetime` omdat deze niet meer nodig zijn (nauwkeurige tijdelementen, zoals uren, minuten en seconden zijn juist handiger voor modeltraining).


```python
processed_df = transformed_features_df.drop_columns(columns=["pickup_datetime", "dropoff_datetime"])
```

Gebruik de functionaliteit type-inferentie om automatisch het gegevenstype van elk veld te controleren. Geef vervolgens de resultaten van de inferentie weer.


```python
type_infer = processed_df.builders.set_column_types()
type_infer.learn()
type_infer
```

De resulterende uitvoer van `type_infer` is als volgt.

    Column types conversion candidates:
    'pickup_weekday': [FieldType.STRING],
    'pickup_hour': [FieldType.DECIMAL],
    'pickup_minute': [FieldType.DECIMAL],
    'pickup_second': [FieldType.DECIMAL],
    'dropoff_hour': [FieldType.DECIMAL],
    'dropoff_minute': [FieldType.DECIMAL],
    'dropoff_second': [FieldType.DECIMAL],
    'store_forward': [FieldType.STRING],
    'pickup_longitude': [FieldType.DECIMAL],
    'dropoff_longitude': [FieldType.DECIMAL],
    'passengers': [FieldType.DECIMAL],
    'distance': [FieldType.DECIMAL],
    'vendor': [FieldType.STRING],
    'dropoff_weekday': [FieldType.STRING],
    'pickup_latitude': [FieldType.DECIMAL],
    'dropoff_latitude': [FieldType.DECIMAL],
    'cost': [FieldType.DECIMAL]

De inferentieresultaten lijken juist op basis van de gegevens. Pas nu de typeconversies toe op de gegevensstroom.


```python
type_converted_df = type_infer.to_dataflow()
type_converted_df.get_profile()
```

Voer voordat u de gegevensstroom verpakt nog twee filters uit op de gegevensset. Om onjuist vastgelegde gegevenspunten te voorkomen, filtert u de gegevensstroom op records waarin zowel de waarde voor de variabele `cost` als voor `distance` groter is dan nul. Met deze stap wordt de nauwkeurigheid van het Machine Learning-model enorm verbeterd omdat gegevenspunten zonder kosten of afstand de voorspellingsnauwkeurigheid in grote mate kunnen beïnvloeden.

```python
final_df = type_converted_df.filter(dprep.col("distance") > 0)
final_df = final_df.filter(dprep.col("cost") > 0)
```

U beschikt nu over een volledig getransformeerd en voorbereid gegevensstroomobject dat in een machine learning-model kan worden gebruikt. De SDK bevat ook functionaliteit voor objectserialisatie, zoals u kunt zien in de volgende code.

```python
import os
file_path = os.path.join(os.getcwd(), "dflows.dprep")

package = dprep.Package([final_df])
package.save(file_path)
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u verder wilt gaan met deel twee van de zelfstudie, moet u het bestand **dflows.dprep** in de huidige map niet verwijderen.

Als u nu wilt stoppen met de zelfstudie, verwijdert u het bestand **dflows.dprep** in de huidige map. Verwijder dit bestand altijd, ongeacht of u lokaal werkt of in [Azure Notebooks](https://notebooks.azure.com/).

## <a name="next-steps"></a>Volgende stappen

In deel één van deze zelfstudie hebt u:

> [!div class="checklist"]
> * De ontwikkelomgeving instellen.
> * Gegevenssets geladen en opgeschoond.
> * Slimme transformaties gebruikt om de logica te voorspellen op basis van een voorbeeld.
> * Gegevenssets samengevoegd en verpakt voor het trainen van machine learning.

U kunt de trainingsgegevens nu gaan gebruiken in deel twee van de zelfstudie:

> [!div class="nextstepaction"]
> [Zelfstudie (deel 2): Geautomatiseerde machine learning gebruiken om uw regressiemodel te bouwen](tutorial-auto-train-models.md)
