---
title: Gedetailleerde richtlijnen voor het gebruik van Azure Machine Learning gegevens voorbereidingen uitoefening | Microsoft Docs
description: Dit document bevat informatie over het uitvoeren van eerder ontworpen gegevensbronnen en gegevens voorbereidingen pakketten
services: machine-learning
author: hughz
ms.author: cforbe
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/13/2018
ms.openlocfilehash: 76bf850df5c8f4a93268dd06f7b9254bbd2cfce6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="data-preparation-operationalization"></a>Gegevens voorbereiden uitoefening 

## <a name="operationalization-scenario"></a>Uitoefening Scenario

Hieronder vindt u de verschillende gegevens voorbereiding uitoefening scenario's die een gebruiker kan tegenkomen.

### <a name="develop-your-model-based-on-training-data"></a>Het model op basis van trainingsgegevens ontwikkelen

Stel dat u wilt maken en implementeren van een realtime scoreprofiel API-service. De eerste stap is het ontwikkelen van een model voor score berekenen op basis van een set trainingsgegevens. Een voorbeeld van uw trainingsgegevens gegevens Prep geïmporteerd als een nieuwe gegevensbron. Als de gegevens is voorbereid, betekent dit dat het pakket DataPrep voorbereidende stappen op de bevat. Met een AzureML experimenteren-Account, kan de gebruiker op een machine learning-model voor het genereren van labels voor elke invoer in de trainingsgegevens herhalen.

Als de functies van de gegevens moeten worden bijgewerkt, retourneert de gebruiker aan het pakket DataPrep voorbereiden van de gegevens in een nieuwe manier en opslaan van deze stappen. Dit proces herhaling voor het genereren van nieuwe functies en hun machine learning-model aanpassingen wordt vervolgd totdat het model nauwkeurig scores hun testgegevens. 

### <a name="deploy-your-model-to-the-azure-model-management-service"></a>Het model implementeren op de Azure-Model-Management-Service

U hebt nu de gegevens van de klant die u wilt beoordelen in realtime. Met behulp van de Azure-Model-Management-Service, kunt u dit model van de AzureML Python CLI implementeren als een service. De geïmplementeerde service beschrijft een REST-eindpunt voor het ontvangen van gegevens van de klant in realtime en retourneren van de bijbehorende labels van het getrainde model uitvoer.

Het eindpunt model accepteert voor eenvoudig te gebruiken, gegevens in de JSON-indeling. De invoer moet een JSON-tekenreeks die een 2-dimensionale matrix van gegevens, die wordt doorgegeven aan de transformatie ReadJSONLiteral en geconverteerd naar een gegevensbron DataPrep vertegenwoordigt. Her gegevenspakket prep gemaakt tijdens de fase experimenteren kan vervolgens worden uitgevoerd met de gestreamde JSON-gegevens. De resulterende dataframe kan vervolgens worden doorgegeven aan het getrainde model voor score berekenen.

## <a name="read-json-literal-transformation"></a>Letterlijke JSON-transformatie lezen

### <a name="description"></a>Beschrijving

Voor uitoefening doeleinden gegevens Prep bevat een **ReadJsonLiteral** transformatie voor het uitvoeren van een activiteit en het genereren van een Pandas of Spark Dataframe. Deze transformatie bevatten de unieke neemt als invoer bestaande gegevens voorbereiden pakket en een JSON-gegevensbron. Deze transformatie is toegankelijk via de DataPrep Python CLI.

### <a name="instructions"></a>Instructies

#### <a name="pythoncli"></a>PythonCLI

Open in de Azure Machine Learning-Workbench de opdrachtregelinterface (bestand > opdrachtprompt openen).

In dit voorbeeld wordt het TrainingPreparationSteps gegevens prep pakket voor het voorbereiden van de gegevens en de volume-functie toevoegen aan elke invoer gebruikt.

```
>>> import azureml.dataprep.package as azdp

>>> azdp.run_on_data.__doc__
"Generate a dataframe based on a selected dataflow in a package using in-memory data sources.
'user_config' is expected as a dictionary that maps the absolute path of a dsource file to an in-memory data source represented as a list of lists."

>>> real_time_customer_data = [[1.0, 1.5, 2.5], [2.5, 1.5, 3]]
>>> azdp.run_on_data({ "C:\\TrainingSampleData.dsource": real_time_customer_data}, "C:\\TrainingPreparationSteps.dprep")
    Height   Width   Depth  Volumne
0   1.0       1.5    2.5    3.75
1   2.5       1.5    3.0    11.25
```

`run_on_data()` de volgende optionele parameters
 - `dataflow_idx`: de index van de gewenste gegevensstroom uit te voeren in het pakket opgeven
 - `secrets`: geheime store woordenlijst (sleutel: secretId, waarde: opgeslagen geheim)
 - `spark`: een spark-sessie voor de uitvoering van spark bieden

#### <a name="input"></a>Invoer

2-dimensionale matrix-invoer ('matrix met matrices'). De invoer moet in Python, een lijst met lijsten. Aangezien JSON geen systeemeigen-datumtype wordt Python datetime.datetime objecten omgezet in op datumtekenreeksen ISO-indeling. De invoer moet een letterlijke JSON-tekenreeks die een 2D-JSON-matrix vertegenwoordigt, voor REST-eindpunten.

#### <a name="output"></a>Uitvoer

Een Pandas of DataFrame Spark. Het type DataFrame wordt bepaald door het framework dat is geselecteerd in de configuratie van de uitvoering (`.runconfig`). De resulterende dataframe kan worden doorgegeven als invoer voor het getrainde model voor score berekenen.
