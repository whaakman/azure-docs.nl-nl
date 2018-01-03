---
title: Gedetailleerde richtlijnen voor het gebruik van de uitvoering van de Azure Machine Learning gegevens voorbereidingen API | Microsoft Docs
description: Dit document bevat informatie over het uitvoeren van eerder ontworpen gegevensbronnen en gegevens voorbereidingen pakketten
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 488fe5697992bf21f9b748d272a6bda50eb1eb90
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="execute-data-sources-and-data-preparations-packages-from-python"></a>Gegevensbronnen en gegevens voorbereidingen pakketten van Python uitvoeren

Een Azure Machine Learning-gegevensbronnen of Azure Machine Learning gegevens voorbereidingen pakket binnen Python gebruiken:

1. Ga naar de **gegevens** tabblad van uw project.

2. Met de rechtermuisknop op de juiste bron.

3. Kies **toegangscode gegevensbestand genereren.**

Deze actie wordt een korte pythonscript dat wordt uitgevoerd van het pakket en retourneert een dataframe gemaakt.

## <a name="data-sources"></a>Gegevensbronnen

De `azureml.dataprep.datasource` -module bevat één functie voor het uitvoeren van een gegevensbron en retourneren een dataframe: `load_datasource(path, secrets=None, spark=None)`.
- `path`is het pad naar de gegevensbron (.dsource-bestand).
- `secrets`is een optionele woordenboek die sleutels wordt toegewezen aan geheimen.
- `spark`is een optionele bool die bepaalt of een Spark-dataframe of een dataframe Pandas retourneren. Standaard bepaalt Azure Machine Learning Workbench welk soort dataframe om terug te keren tijdens runtime op basis van context.

## <a name="data-preparations-packages"></a>Gegevens voorbereidingen pakketten

De `azureml.dataprep.package` -module bevat drie functies die een gegevensstroom uit een pakket gegevens voorbereidingen worden uitgevoerd.

### <a name="execution-functions"></a>Functies worden uitgevoerd

- `submit(package_path, dataflow_idx=0, secrets=None, spark=None)`de opgegeven gegevensstroom voor uitvoering worden verzonden, maar retourneert geen een dataframe.
- `run(package_path, dataflow_idx=0, secrets=None, spark=None)`de opgegeven gegevensstroom wordt uitgevoerd en de resultaten retourneert als een dataframe.
- `run_on_data(user_config, package_path, dataflow_idx=0, secrets=None, spark=None)`de opgegeven gegevensstroom op basis van een gegevensbron in het geheugen wordt uitgevoerd en de resultaten retourneert als een dataframe. De `user_config` argument is een dictionary waarmee het absolute pad van een gegevensbron (.dsource-bestand) worden toegewezen aan een gegevensbron van in het geheugen weergegeven als een lijst met lijsten.

### <a name="common-arguments"></a>Algemene argumenten

- `package_path`is het pad naar het pakket gegevens voorbereidingen (.dprep-bestand).
- `dataflow_idx`is de op nul gebaseerde index van de gegevensstroom van welke in het pakket uit te voeren. Als de opgegeven gegevensstroom verwijst naar andere gegevensoverdrachten of gegevensbronnen, worden deze ook uitgevoerd.
- `secrets`is een optionele woordenboek die sleutels wordt toegewezen aan geheimen.
- `spark`is een optionele bool die bepaalt of een Spark-dataframe of een dataframe Pandas retourneren. Standaard bepaalt Workbench van welke soort dataframe om terug te keren tijdens runtime op basis van de context.
