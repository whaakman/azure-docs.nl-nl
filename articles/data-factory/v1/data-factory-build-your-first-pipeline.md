---
title: 'Data Factory-zelfstudie: eerste gegevens pijplijn | Microsoft Docs'
description: Deze Azure Data Factory-zelfstudie laat zien hoe u maken en plannen van een gegevensfactory dat gegevens met Hive-script op een Hadoop-cluster verwerkt.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.assetid: 81f36c76-6e78-4d93-a3f2-0317b413f1d0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 553750ce024929b285485c63156f8a8a23dff3ce
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-build-your-first-pipeline-to-transform-data-using-hadoop-cluster"></a>Zelfstudie: Bouw uw eerste pijplijn voor het transformeren van gegevens met Hadoop-cluster
> [!div class="op_single_selector"]
> * [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service gebruikt, welke nog een preview is, raadpleegt u [Snelstartgids: een gegevensfactory maken met Azure Data Factory versie 2](../quickstart-create-data-factory-dot-net.md).

In deze zelfstudie bouwt u uw eerste Azure-gegevensfactory met een pipeline gegevens. De pijplijn transformeert invoergegevens door het Hive-script uitgevoerd op een cluster met Azure HDInsight (Hadoop) voor het produceren van uitvoergegevens.  

Dit artikel bevat een overzicht en vereisten voor de zelfstudie. Nadat u de vereisten hebt voltooid, kunt u de zelfstudie met een van de volgende hulpprogramma's / SDK's kunt doen: Azure portal, Visual Studio, PowerShell, Resource Manager-sjabloon, REST-API. Selecteer een van de opties in de vervolgkeuzelijst aan het begin (of) koppelingen aan het einde van dit artikel om de zelfstudie met een van deze opties wilt.    

## <a name="tutorial-overview"></a>Overzicht van de zelfstudie
In deze zelfstudie voert u de volgende stappen uit:

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer gegevenspijplijnen verplaatsen en transformeer gegevens bevatten. 

    In deze zelfstudie maakt u een pijplijn in de data factory. 
2. Maak een **pijplijn**. Een pijplijn kan een of meer activiteiten bevatten (bijvoorbeeld: kopieeractiviteit, HDInsight Hive-activiteit). Dit voorbeeld gebruikt de HDInsight Hive-activiteit die een Hive-script wordt uitgevoerd op een HDInsight Hadoop-cluster. Het script maakt eerst een tabel die verwijst naar de onbewerkte web logboekgegevens opgeslagen in Azure blob storage en vervolgens de onbewerkte gegevens partitioneert op jaar en maand.

    In deze zelfstudie wordt de Hive-activiteit in de pijplijn gebruikt om gegevens te transformeren door een Hive-query uitgevoerd op een Azure HDInsight Hadoop-cluster. 
3. Maak **gekoppelde services**. U maakt een gekoppelde service om een gegevensopslag of een rekenservice te koppelen aan de gegevensfactory. In een gegevensopslag zoals Azure Storage worden invoer-/uitvoergegevens bewaard van activiteiten in de pijplijn. Een compute-service zoals HDInsight Hadoop-cluster processen/transformaties gegevens.

    In deze zelfstudie maakt u twee gekoppelde services: **Azure Storage** en **Azure HDInsight**. Azure Storage gekoppelde service wordt een Azure Storage-Account waarin de invoer-en uitvoergegevens aan de gegevensfactory. Azure HDInsight gekoppelde service wordt een Azure HDInsight-cluster dat wordt gebruikt voor het transformeren van gegevens aan de gegevensfactory. 
3. Maak **gegevenssets** voor invoer en uitvoer. Een invoergegevensset vertegenwoordigt de invoer voor een activiteit in de pijplijn en een uitvoergegevensset vertegenwoordigt de uitvoer voor de activiteit.

    In deze zelfstudie Geef de invoer- en uitvoergegevenssets locaties van de invoer- en gegevens in Azure Blob Storage. De gekoppelde Azure Storage-service wordt aangegeven wat Azure Storage-Account wordt gebruikt. Een invoergegevensset geeft aan waar de invoerbestanden zich bevinden en een uitvoergegevensset geeft aan waar de uitvoerbestanden worden geplaatst. 


Zie [Inleiding tot Azure Data Factory](data-factory-introduction.md) artikel voor een gedetailleerd overzicht van Azure Data Factory.
  
Hier volgt de **diagramweergave** van de voorbeeld-gegevensfactory die u in deze zelfstudie bouwt. **MyFirstPipeline** heeft één activiteit van het type Hive die verbruikt **AzureBlobInput** dataset als invoer en produceert **AzureBlobOutput** gegevensset als uitvoer. 

![Zelfstudie voor de Data Factory-diagramweergave](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


In deze zelfstudie **inputdata** map van de **adfgetstarted** Azure blob-container bevat één bestand met de naam input.log. Dit logboekbestand bevat vermeldingen van drie maanden: januari, februari en maart 2016. Hier ziet u de voorbeeldrijen per maand in het invoerbestand. 

```
2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Wanneer het bestand is verwerkt via de pijplijn met HDInsight Hive-activiteit, wordt met de activiteit een Hive-script uitgevoerd op de HDInsight-cluster waardoor de invoer wordt gesplitst in jaren en maanden. Met het script worden drie uitvoermappen gemaakt die een bestand bevatten met de invoer uit elke maand.  

```
adfgetstarted/partitioneddata/year=2016/month=1/000000_0
adfgetstarted/partitioneddata/year=2016/month=2/000000_0
adfgetstarted/partitioneddata/year=2016/month=3/000000_0
```

Van de regels van het voorbeeld hierboven is het eerste beheerpunt (met 2016-01-01) is geschreven naar het bestand 000000_0 in de maand = 1 map. Op dezelfde manier is de tweede regel geschreven naar het bestand in de map maand=2 en de derde naar het bestand in de map maand=3.  

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u aan de volgende vereisten voldoen:

1. U moet beschikken over een **Azure-abonnement**. Als u geen Azure-abonnement hebt, kunt u in enkele minuten een gratis proefaccount maken. Zie het artikel [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) voor informatie over het verkrijgen van een gratis proefaccount.
2. U moet gebruikmaken van **Azure Storage**. U gebruikt een Azure-opslagaccount om de gegevens in deze zelfstudie op te slaan. Zie het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen account Azure-opslagaccount hebt. Nadat u het opslagaccount hebt gemaakt, noteer de **accountnaam** en **toegangssleutel**. Zie [View, copy and regenerate storage access keys](../../storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys) (Toegangssleutels voor opslag weergeven, kopiëren en opnieuw genereren).
3. Downloaden en bekijken van het Hive-query-bestand (**HQL**) te vinden op: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Deze query transformeert invoergegevens uitvoergegevens te produceren. 
4. Downloaden en bekijken van de voorbeeld-bestand voor invoer (**input.log**) te vinden op: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Maak een blobcontainer met de naam **adfgetstarted** in uw Azure-blobopslag. 
6. Uploaden **partitionweblogs.hql** van het bestand in de **script** map in de **adfgetstarted** container. Gebruik hulpprogramma's zoals [Microsoft Azure Storage Explorer](http://storageexplorer.com/). 
7. Uploaden **input.log** van het bestand in de **inputdata** map in de **adfgetstarted** container. 

Nadat u de vereisten hebt voltooid, selecteer een van de volgende hulpprogramma's / SDK's om de zelfstudie wilt: 

- [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Azure-portal en Visual Studio bieden GUI-methode van het bouwen van uw gegevensfactory. Terwijl opties PowerShell, Resource Manager-sjabloon en de REST-API biedt script of programma manier van het bouwen van uw gegevensfactory.

> [!NOTE]
> Met de gegevenspijplijn in deze zelfstudie worden invoergegevens getransformeerd in uitvoergegevens. Er worden bijvoorbeeld geen gegevens gekopieerd van een brongegevensarchief naar een doelgegevensarchief. Zie [Zelfstudie: gegevens kopiëren van Blob Storage naar SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor informatie over het kopiëren van gegevens met Azure Data Factory.
> 
> U kunt twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. Zie [Planning en uitvoering in Data Factory](data-factory-scheduling-and-execution.md) voor gedetailleerde informatie. 





  
