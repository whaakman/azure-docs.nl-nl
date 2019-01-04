---
title: Gegevens transformeren met Databricks Notebook - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformeren door het uitvoeren van een Databricks-notebook.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: 8ab6dad36bf47430a925d21ca2464286e7e70002
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022067"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Gegevens transformeren door het uitvoeren van een Databricks-notebook

De Azure Databricks Notebook-activiteit in een [Data Factory-pijplijn](concepts-pipelines-activities.md) een Databricks-notebook in uw Azure Databricks-werkruimte wordt uitgevoerd. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark.

## <a name="databricks-notebook-activity-definition"></a>De definitie van de Databricks Notebook-activiteit

Hier volgt de voorbeeld-JSON-definitie van een Databricks Notebook-activiteit:

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
            },
            "libraries": [
                {
                "jar": "dbfs:/docs/library.jar"
                }
            ]
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Eigenschappen van de Databricks Notebook-activiteit

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie gebruikt:

|Eigenschap|Description|Vereist|
|---|---|---|
|naam|Naam van de activiteit in de pijplijn.|Ja|
|description|Tekst die beschrijft wat de activiteit doet.|Nee|
|type|Voor Databricks Notebook-activiteit is het activiteitstype DatabricksNotebook.|Ja|
|linkedServiceName|De naam van de Databricks gekoppelde Service waarop het Databricks-notebook wordt uitgevoerd. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.|Ja|
|notebookPath|Het absolute pad van het notitieblok om te worden uitgevoerd in de Databricks-werkruimte. Dit pad moet beginnen met een slash.|Ja|
|baseParameters|Een matrix met sleutel-waardeparen. Algemene parameters kunnen worden gebruikt voor elke activiteit die wordt uitgevoerd. Als de notebook heeft een parameter die niet is opgegeven, wordt de standaardwaarde van de notebook worden gebruikt. Meer informatie over parameters in vinden [Databricks-Notebooks](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Nee|
|Bibliotheken|Een lijst met bibliotheken die u moet worden geïnstalleerd op het cluster dat de taak wordt uitgevoerd. Kan het zijn een reeks \<string, object >.|Nee|


## <a name="supported-libraries-for-databricks-activities"></a>Ondersteunde-bibliotheken voor Databricks-activiteiten

In de bovenstaande definitie van de activiteit Databricks, geeft u deze typen bibliotheek: *jar*, *ei*, *maven*, *pypi*,  *cran*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "http://cran.us.r-project.org"
            }
        }
    ]
}

```

Zie voor meer informatie de [documentatie voor Databricks](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) voor bibliotheek typen.

## <a name="how-to-upload-a-library-in-databricks"></a>Over het uploaden van een bibliotheek in Databricks

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Met behulp van de gebruikersinterface van de Databricks-werkruimte](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Als u het pad dbfs van de bibliotheek toegevoegd met behulp van de gebruikersinterface, kunt u de [Databricks CLI (installatie)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Normaal gesproken de Jar-bibliotheken worden opgeslagen onder dbfs: / FileStore/jars tijdens het gebruik van de gebruikersinterface. U kunt een lijst alle via de CLI: *databricks fs ls dbfs: / FileStore/JAR-bestanden*.



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Tapewisselaar voor kopiëren met behulp van Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Voorbeeld: *databricks fs cp SparkPi-assembly-0.1.jar dbfs: / FileStore/JAR-bestanden*
