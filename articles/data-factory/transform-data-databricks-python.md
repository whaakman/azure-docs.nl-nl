---
title: Gegevens transformeren met Databricks-Python - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformeren door het uitvoeren van een Databricks-Python.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: fa19ff6e6293e1d5d082ee40d55b456500fcb1ad
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076034"
---
# <a name="transform-data-by-running-a-python-activity-in-azure-databricks"></a>Gegevens transformeren met behulp van een Python-activiteit in Azure Databricks

De Azure Databricks Python-activiteit in een [Data Factory-pijplijn](concepts-pipelines-activities.md) een Python-bestand in uw Azure Databricks-cluster wordt uitgevoerd. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark.

## <a name="databricks-python-activity-definition"></a>Definitie van de activiteit Databricks Python

Hier volgt de voorbeeld-JSON-definitie van een Databricks-Python-activiteit:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksSparkPython",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "pythonFile": "dbfs:/docs/pi.py",
            "parameters": [
                "10"
            ],
            "libraries": [
                {
                    "pypi": {
                        "package": "tensorflow"
                    }
                }
            ]
        }
    }
}
```

## <a name="databricks-python-activity-properties"></a>Databricks Python activiteitseigenschappen

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie gebruikt:

|Eigenschap|Beschrijving|Vereist|
|---|---|---|
|naam|Naam van de activiteit in de pijplijn.|Ja|
|description|Tekst die beschrijft wat de activiteit doet.|Nee|
|type|Voor Databricks Python-activiteit is het activiteitstype DatabricksSparkPython.|Ja|
|linkedServiceName|De naam van de Databricks gekoppelde Service op de Python-activiteit wordt uitgevoerd. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.|Ja|
|pythonFile|De URI van het Python-bestand moet worden uitgevoerd. Alleen DBFS paden worden ondersteund.|Ja|
|parameters|Opdrachtregelparameters die worden doorgegeven aan het Python-bestand. Dit is een matrix met tekenreeksen.|Nee|
|Bibliotheken|Een lijst met bibliotheken die u moet worden geïnstalleerd op het cluster dat de taak wordt uitgevoerd. Kan het zijn een reeks < string, object >|Nee|

## <a name="supported-libraries-for-databricks-activities"></a>Ondersteunde-bibliotheken voor databricks-activiteiten

In de bovenstaande definitie van de activiteit Databricks geeft u deze typen bibliotheek: *jar*, *ei*, *maven*, *pypi*,  *cran*.

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

Raadpleeg voor meer informatie [documentatie voor Databricks](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) voor bibliotheek typen.

## <a name="how-to-upload-a-library-in-databricks"></a>Over het uploaden van een bibliotheek in Databricks

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Met behulp van de gebruikersinterface van de Databricks-werkruimte](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Als u het pad dbfs van de bibliotheek toegevoegd met behulp van de gebruikersinterface, kunt u [Databricks CLI (installatie)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Normaal gesproken de Jar-bibliotheken worden opgeslagen onder dbfs: / FileStore/jars tijdens het gebruik van de gebruikersinterface. U kunt een lijst alle via de CLI: *databricks fs ls dbfs: / FileStore/JAR-bestanden* 



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Tapewisselaar voor kopiëren met behulp van Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Voorbeeld: *databricks fs cp SparkPi-assembly-0.1.jar dbfs: / FileStore/JAR-bestanden*