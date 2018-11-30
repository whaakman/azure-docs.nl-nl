---
title: Voorbereiden van gegevens met Machine Learning Data Prep SDK voor Python - Azure
description: Informatie over het gebruik van de Azure Machine Learning Data Prep SDK voor Python laden van gegevens van verschillende indelingen en die gegevens schrijven naar een locatie voor uw modellen voor toegang tot deze beter bruikbaar transformeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 11/27/2018
ms.openlocfilehash: 91d0f3565db484504a67a3b6ae0989b9291cd24f
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446421"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Gegevens voorbereiden voor modellen met Azure Machine Learning

In dit artikel leert u over de use cases en de unieke eigenschappen van de Azure Machine Learning Data Prep SDK. Voorbereiden van gegevens is het belangrijkste onderdeel van een machine learning-werkstroom. Echte gegevens worden vaak opgesplitst, inconsistente of kan niet worden gebruikt als trainingsgegevens zonder significante opschonen en transformeren. Corrigeren van fouten en afwijkingen in de onbewerkte gegevens en het bouwen van nieuwe functies die relevant voor het probleem dat u wilt oplossen zijn, wordt de nauwkeurigheid van model verhogen. De SDK is ontworpen om vertrouwd te raken voor gebruikers van andere algemene gegevens prep-bibliotheken, terwijl bieden voordelen voor belangrijke scenario's en onderhouden van interoperabiliteit met die andere bibliotheken.

U kunt uw gegevens in met behulp van Python voorbereiden de [SDK van Azure Machine Learning Data Prep](https://aka.ms/data-prep-sdk).

## <a name="azure-machine-learning-data-prep-sdk"></a>Azure Machine Learning gegevensvoorbereiding SDK

De [SDK van Azure Machine Learning Data Prep](https://aka.ms/data-prep-sdk) is een Python-bibliotheek biedt:

* Intelligente tijdbesparende transformaties zoals Fuzzy groeperen, afgeleide kolom per voorbeeld, automatisch splitsen, slimme lezen-bestand en onregelmatige rechts schema-verwerking.
* Een enkele API die geschikt is voor gegevens lokaal kleine of grote hoeveelheden gegevens in de cloud, met **paar aan zonder codewijzigingen**.
* De mogelijkheid om effectiever schalen op een enkele computer door het gebruik van een streaming-benadering voor het verwerken van de gegevens, in plaats van in het geheugen wordt geladen.

### <a name="install-the-sdk"></a>De SDK installeren

Installeer de SDK in uw Python-omgeving met behulp van de volgende opdracht uit.

```shell
pip install azureml-dataprep
```

Gebruik de volgende code voor het importeren van het pakket.

```python
import azureml.dataprep as dprep
```

### <a name="examples-and-reference"></a>Naslaginformatie over en voorbeelden

Zie voor meer informatie over de functies van deze SDK en -modules, de [Data Prep SDK-referentiedocumenten](https://aka.ms/data-prep-sdk).

De volgende voorbeelden markeert u enkele van de unieke functies van de SDK, met inbegrip van:

* Automatische detectie
* Intelligente transformaties
* Samenvattende statistieken
* Functionaliteit voor cross-omgeving


#### <a name="automatic-file-type-detection"></a>Automatische detectie

Gebruik de `smart_read_file()` functie voor het laden van uw gegevens zonder te geven van het bestandstype. Deze functie automatisch herkend en parseert het bestandstype.

```python
dataflow = dprep.smart_read_file(path="<your-file-path>")
```

#### <a name="intelligent-transforms"></a>Intelligente transformaties

De SDK gebruiken voor splitsen en afleiden van kolommen op zowel voorbeeld en Deductie feature-engineering automatiseren. Stel dat u hebt een veld in de gegevensstroom-object met de naam `datetime` met een waarde van `2018-09-15 14:30:00`.

Automatisch splitsen de `datetime` veld, de volgende functie aanroepen.

```python
new_dataflow = dataflow.split_column_by_example(source_column="datetime")
```

Door de voorbeeld-parameter niet definieert, de functie wordt automatisch verdeeld over de `datetime` veld in twee nieuwe velden `datetime_1` en `datetime_2`. De resulterende waarden zijn `2018-09-15` en `14:30:00`, respectievelijk. Het is ook mogelijk om te geven een voorbeeld-patroon en de SDK wordt voorspellen en uw beoogde transformatie worden uitgevoerd. Met behulp van dezelfde `datetime` -object, de volgende code wordt een nieuwe kolom maken `datetime_weekday` voor de weekdag op basis van de opgegeven voorbeeld.

```python
new_dataflow = dataflow.derive_column_by_example(
        source_columns="datetime", 
        new_column_name="datetime_weekday", 
        example_data=[("2009-01-04 10:12:00", "Sunday"), ("2013-08-22 17:00:00", "Thursday")]
    )
```

#### <a name="summary-statistics"></a>Samenvattende statistieken

U kunt snel samenvattende statistieken voor een gegevensstroom met één regel code genereren. Deze methode biedt een handige manier om te begrijpen van uw gegevens en hoe deze wordt gedistribueerd.

```python
dataflow.get_profile()
```

Deze functie aanroepen op een object gegevensstroom leidt uitvoer, zoals de volgende tabel.

![Samenvattende statistieken uitvoer](./media/concept-data-preparation/output-example.png)

## <a name="multiple-environment-compatibilities"></a>Meerdere omgeving compatibiliteiten

De SDK ook kan voor objecten worden geserialiseerd en geopend in de gegevensstroom *eventuele* Python-omgeving. De omgeving waar deze wordt geopend kan afwijken van de omgeving waarin deze opgeslagen. Deze functionaliteit kan eenvoudig worden overgedragen tussen Python-omgevingen en snelle integratie met Azure Machine Learning-modellen.

Gebruik de volgende code om op te slaan van de gegevensstroom-objecten.

```python
package = dprep.Package([dataflow_1, dataflow_2])
package.save("<your-local-path>")
```

Gebruik de volgende code om te openen van het pakket in een omgeving en ophalen van een lijst van objecten van de gegevensstroom.

```python
package = dprep.Package.open("<your-local-path>")
dataflow_list = package.dataflows
```

## <a name="data-preparation-pipeline"></a>Voorbereiding van gegevenspijplijn

Als u gedetailleerde voorbeelden en de code voor elke stap voorbereiding weergeven, gebruikt u de volgende handleidingen:

1. [Gegevens laden](how-to-load-data.md), wat kan zijn in verschillende indelingen
2. [Transformeren](how-to-transform-data.md) deze in een beter bruikbaar structuur
3. [Schrijven](how-to-write-data.md) die gegevens naar een locatie die toegankelijk is voor uw modellen

![Gegevens voorbereiden](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Volgende stappen

Bekijk een [voorbeeld notebook](https://github.com/Microsoft/AMLDataPrepDocs/tree/master/tutorials/getting-started/getting-started.ipynb) van gegevens voor te bereiden met behulp van de Azure Machine Learning Data Prep SDK.

Azure Machine Learning Data Prep SDK [referentiedocumentatie](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py).
