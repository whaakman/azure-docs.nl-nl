---
title: Gegevens transformeren met Databricks laptop - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformatie gegevens door het uitvoeren van een laptop Databricks.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: d4a57e45d5ddf55906fcf575df39135a227418ec
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Transformeer gegevens door het uitvoeren van een laptop Databricks

De Azure Databricks Notebook activiteit in een [Data Factory-pijplijn](concepts-pipelines-activities.md) een laptop Databricks in uw werkruimte Databricks Azure wordt uitgevoerd. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten toont. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark.

## <a name="databricks-notebook-activity-definition"></a>De definitie van de activiteit Databricks Notebook

Hier volgt de voorbeeld-JSON-definitie van een activiteit van de Notebook Databricks:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            }
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>De activiteitseigenschappen Databricks Notebook

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie:

|Eigenschap|Beschrijving|Vereist|
|---|---|---|
|naam|De naam van de activiteit in de pijplijn.|Ja|
|description|Tekst die beschrijft wat de activiteit doet.|Nee|
|type|Voor de activiteit van de Notebook Databricks is het activiteitstype DatabricksNotebook.|Ja|
|linkedServiceName|Naam van de gekoppelde Service van Databricks waarop de notebook Databricks wordt uitgevoerd. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.|Ja|
|notebookPath|Het absolute pad van de notebook om te worden uitgevoerd in de werkruimte Databricks. Dit pad moet beginnen met een slash.|Ja|
|baseParameters|Een matrix met sleutel-waardeparen. Base parameters kunnen worden gebruikt voor elke activiteit die wordt uitgevoerd. Als de laptop een parameter die niet is opgegeven duurt, wordt de standaardwaarde van de notebook worden gebruikt. Meer informatie over parameters in vinden [Databricks notitieblokken](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Nee|
