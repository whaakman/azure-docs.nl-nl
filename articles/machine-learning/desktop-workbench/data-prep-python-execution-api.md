---
title: Uitgebreide handleiding voor het gebruik van de uitvoerings-API voor Azure Machine Learning Data Preparations | Microsoft Docs
description: Dit document bevat informatie over het uitvoeren van eerder ontworpen gegevensbronnen en Data Preparations pakketten
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 9f15f949ba76240da2788c1a01e7086ba3b42fd1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644281"
---
# <a name="execute-data-sources-and-data-preparations-packages-from-python"></a>Gegevensbronnen en Data Preparations-pakketten uitvoeren met Python

Een Azure Machine Learning-gegevensbronnen of Azure Machine Learning Data Preparations-pakket in Python gebruiken:

1. Ga naar de **gegevens** tabblad van het project.

2. Met de rechtermuisknop op de juiste bron.

3. Kies **Data Access Code File genereren.**

Deze actie wordt een korte pythonscript dat het pakket wordt uitgevoerd en retourneert een dataframe gemaakt.

## <a name="data-sources"></a>Gegevensbronnen

De `azureml.dataprep.datasource` -module bevat één functie voor het uitvoeren van een gegevensbron en een dataframe retourneren: `load_datasource(path, secrets=None, spark=None)`.
- `path` is het pad naar de gegevensbron (.dsource-bestand).
- `secrets` is een optionele woordenlijst die wordt toegewezen sleutels aan geheimen.
- `spark` is een optionele bool waarmee wordt aangegeven of om een Spark dataframe of een Pandas dataframe te retourneren. Standaard bepaalt Azure Machine Learning Workbench van welke soort gegevensframe om terug te keren tijdens runtime op basis van context.

## <a name="data-preparations-packages"></a>Voorbereidingen gegevenspakketten

De `azureml.dataprep.package` -module bevat drie functies die een gegevensstroom uit een Data Preparations-pakket worden uitgevoerd.

### <a name="execution-functions"></a>Uitvoering van functies

- `submit(package_path, dataflow_idx=0, secrets=None, spark=None)` verzendt de stroom van de opgegeven gegevens voor de uitvoering van maar niet een dataframe weer.
- `run(package_path, dataflow_idx=0, secrets=None, spark=None)` de opgegeven gegevensstroom wordt uitgevoerd en worden de resultaten geretourneerd als een dataframe.
- `run_on_data(user_config, package_path, dataflow_idx=0, secrets=None, spark=None)` de opgegeven gegevensstroom op basis van een gegevensbron in het geheugen wordt uitgevoerd en worden de resultaten geretourneerd als een dataframe. De `user_config` argument is een woordenlijst die het absolute pad van een gegevensbron (.dsource-bestand) wordt toegewezen aan de bron van een in-memory-gegevens wordt weergegeven als een lijst met lijsten.

### <a name="common-arguments"></a>Algemene argumenten

- `package_path` is het pad naar het Data Preparations-pakket (.dprep-bestand).
- `dataflow_idx` is de op nul gebaseerde index van de gegevensstroom van welke in het pakket uit te voeren. Als de opgegeven gegevensstroom verwijst naar andere gegevensstromen of gegevensbronnen, worden ze ook uitgevoerd.
- `secrets` is een optionele woordenlijst die wordt toegewezen sleutels aan geheimen.
- `spark` is een optionele bool waarmee wordt aangegeven of om een Spark dataframe of een Pandas dataframe te retourneren. Standaard bepaalt Workbench van welke soort gegevensframe om terug te keren tijdens runtime op basis van context.
