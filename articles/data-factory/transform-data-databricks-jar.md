---
title: Gegevens transformeren met Databricks-Jar - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformeren door het uitvoeren van een Databricks-Jar.
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
ms.openlocfilehash: 8a271359f09ca63e1a0c3a143994739ee7db8aab
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014179"
---
# <a name="transform-data-by-running-a-jar-activity-in-azure-databricks"></a>Gegevens transformeren met behulp van een Jar-activiteit in Azure Databricks

De Azure Databricks Jar-activiteit in een [Data Factory-pijplijn](concepts-pipelines-activities.md) wordt uitgevoerd een Spark-Jar in uw Azure Databricks-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark.

Bekijk de volgende video voor een inleiding en demonstratie van deze functie van 11 minuten:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-jar-activity-definition"></a>De definitie van de Databricks-Jar-activiteit

Hier volgt de voorbeeld-JSON-definitie van een Databricks Jar-activiteit:

```json
{
    "name": "SparkJarActivity",
    "type": "DatabricksSparkJar",
    "linkedServiceName": {
        "referenceName": "AzureDatabricks",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mainClassName": "org.apache.spark.examples.SparkPi",
        "parameters": [ "10" ],
        "libraries": [
            {
                "jar": "dbfs:/docs/sparkpi.jar"
            }
        ]
    }
}

```

## <a name="databricks-jar-activity-properties"></a>Databricks-Jar activiteitseigenschappen

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie gebruikt:

|Eigenschap|Description|Vereist|
|:--|---|:-:|
|naam|Naam van de activiteit in de pijplijn.|Ja|
|description|Tekst die beschrijft wat de activiteit doet.|Nee|
|type|Voor Databricks Jar-activiteit is het activiteitstype DatabricksSparkJar.|Ja|
|linkedServiceName|De naam van de Databricks gekoppelde Service waarop de Jar-activiteit wordt uitgevoerd. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.|Ja|
|mainClassName|De volledige naam van de klasse met de belangrijkste methode moet worden uitgevoerd. Deze klasse moet worden opgenomen in een JAR opgegeven als een bibliotheek.|Ja|
|parameters|De parameters die worden doorgegeven aan de belangrijkste methode.  Dit is een matrix met tekenreeksen.|Nee|
|Bibliotheken|Een lijst met bibliotheken die u moet worden geïnstalleerd op het cluster dat de taak wordt uitgevoerd. Kan het zijn een reeks < string, object >|Ja (ten minste één met de methode mainClassName)|

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

Normaal gesproken de Jar-bibliotheken worden opgeslagen onder dbfs: / FileStore/jars tijdens het gebruik van de gebruikersinterface. U kunt een lijst alle via de CLI: *databricks fs ls dbfs: / FileStore/taak-JAR-bestanden* 



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Tapewisselaar voor kopiëren met behulp van Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)
Databricks CLI gebruiken [(installatiestappen)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Voorbeeld - JAR kopiëren naar dbfs: *dbfs cp SparkPi-assembly-0.1.jar dbfs:/docs/sparkpi.jar*
