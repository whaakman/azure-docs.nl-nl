---
title: 'Data Factory-zelfstudie: Eerste gegevenspijplijn | Microsoft Docs'
description: Deze Azure Data Factory-zelfstudie leert u hoe u het maken en plannen van een data factory die gegevens verwerkt met behulp van Hive-script op een Hadoop-cluster.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.assetid: 81f36c76-6e78-4d93-a3f2-0317b413f1d0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: d9d9e68b7e74ba7725e97162d01e1a35314fdd0f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57544951"
---
# <a name="tutorial-build-your-first-pipeline-to-transform-data-using-hadoop-cluster"></a>Zelfstudie: Uw eerste pijplijn voor het transformeren van gegevens met behulp van Hadoop-cluster maken
> [!div class="op_single_selector"]
> * [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Quickstart: een gegevensfactory maken met Azure Data Factory](../quickstart-create-data-factory-dot-net.md).

In deze zelfstudie bouwt u uw eerste Azure-gegevensfactory met een gegevenspijplijn. De pijplijn transformeert invoergegevens door het Hive-script uitvoeren op een Azure HDInsight (Hadoop)-cluster om uitvoergegevens te produceren.  

Dit artikel bevat een overzicht en vereisten voor de zelfstudie. Nadat u de vereisten hebt voltooid, kunt u de zelfstudie met behulp van een van de volgende hulpprogramma's / SDK's kunt doen: Azure-portal, Visual Studio, PowerShell en Resource Manager-sjabloon, REST-API. Selecteer een van de opties in de vervolgkeuzelijst aan het begin (of) koppelingen aan het einde van dit artikel om de zelfstudie met behulp van een van deze opties.    

## <a name="tutorial-overview"></a>Overzicht van de zelfstudie
In deze zelfstudie voert u de volgende stappen uit:

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen die verplaatsen en transformeren van gegevens bevatten. 

    In deze zelfstudie maakt u een pijplijn in de data factory. 
2. Maak een **pijplijn**. Een pijplijn kan één of meer activiteiten bevatten (voorbeelden: Activiteit, HDInsight Hive-activiteit kopiëren). In dit voorbeeld wordt de HDInsight Hive-activiteit die een Hive-script wordt uitgevoerd op een HDInsight Hadoop-cluster. Het script maakt u eerst een tabel die verwijst naar de onbewerkte logboekgegevens die zijn opgeslagen in Azure blob-opslag en vervolgens de onbewerkte gegevens partitioneert op jaar en maand.

    In deze zelfstudie wordt de pijplijn de Hive-activiteit gebruikt om gegevens te transformeren door het uitvoeren van een Hive-query op een Azure HDInsight Hadoop-cluster. 
3. Maak **gekoppelde services**. U maakt een gekoppelde service om een gegevensopslag of een rekenservice te koppelen aan de gegevensfactory. In een gegevensopslag zoals Azure Storage worden invoer-/uitvoergegevens bewaard van activiteiten in de pijplijn. Een compute-service, zoals HDInsight Hadoop-cluster processen/transformaties gegevens.

    In deze zelfstudie maakt maken u twee gekoppelde services: **Azure Storage** en **Azure HDInsight**. Azure Storage gekoppelde service koppelt een Azure Storage-Account waarin de invoer/uitvoer-gegevens aan de data factory. Azure HDInsight gekoppelde service koppelt een Azure HDInsight-cluster dat wordt gebruikt voor het transformeren van gegevens naar de data factory. 
3. Maak **gegevenssets** voor invoer en uitvoer. Een invoergegevensset vertegenwoordigt de invoer voor een activiteit in de pijplijn en een uitvoergegevensset vertegenwoordigt de uitvoer voor de activiteit.

    In deze zelfstudie Geef de invoer- en uitvoergegevenssets locaties van de invoer- en gegevens in de Azure Blob-opslag. De gekoppelde Azure Storage-service geeft aan wat Azure Storage-Account wordt gebruikt. Een invoergegevensset geeft aan waar de invoerbestanden zich bevinden en een uitvoergegevensset geeft aan waar de uitvoerbestanden worden geplaatst. 


Zie [Inleiding tot Azure Data Factory](data-factory-introduction.md) artikel voor een gedetailleerd overzicht van Azure Data Factory.
  
Hier volgt de **diagramweergave** van de voorbeeld-gegevensfactory die u in deze zelfstudie maakt. **MyFirstPipeline** heeft één activiteit van het type Hive verbruikt **AzureBlobInput** -gegevensset als invoer en produceert **AzureBlobOutput** gegevensset als uitvoer. 

![Zelfstudie voor de Data Factory-diagramweergave](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


In deze zelfstudie **inputdata** map van de **adfgetstarted** Azure blob-container bevat één bestand met de naam input.log. Dit logboekbestand bevat de invoer van drie maanden: Januari, februari en maart 2016. Hier ziet u de voorbeeldrijen per maand in het invoerbestand. 

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

In het voorbeeld hierboven wordt weergegeven, wordt de eerste regel (met 2016-01-01) geschreven naar het bestand 000000_0 in de maand = 1 map. Op dezelfde manier is de tweede regel geschreven naar het bestand in de map maand=2 en de derde naar het bestand in de map maand=3.  

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u aan de volgende vereisten voldoen:

1. U moet beschikken over een **Azure-abonnement**. Als u geen Azure-abonnement hebt, kunt u in enkele minuten een gratis proefaccount maken. Zie het artikel [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) voor informatie over het verkrijgen van een gratis proefaccount.
2. U moet gebruikmaken van **Azure Storage**. U gebruikt een Azure-opslagaccount om de gegevens in deze zelfstudie op te slaan. Zie het artikel [Een opslagaccount maken](../../storage/common/storage-quickstart-create-account.md) als u geen account Azure-opslagaccount hebt. Nadat u de storage-account hebt gemaakt, noteer de **accountnaam** en **toegangssleutel**. Zie [View, copy and regenerate storage access keys](../../storage/common/storage-account-manage.md#access-keys) (Toegangssleutels voor opslag weergeven, kopiëren en opnieuw genereren).
3. Downloaden en bekijken van de Hive-querybestand (**HQL**) dat zich bevindt in: [ https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql ](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Deze query transformeert invoergegevens om uitvoergegevens te produceren. 
4. Downloaden en bekijken van de voorbeeld-invoerbestand (**input.log**) dat zich bevindt in: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Maak een blobcontainer met de naam **adfgetstarted** in de Azure Blob-opslag. 
6. Uploaden **partitionweblogs.hql** van het bestand in de **script** map in de **adfgetstarted** container. Gebruik hulpprogramma's zoals [Microsoft Azure Storage Explorer](https://storageexplorer.com/). 
7. Uploaden **input.log** van het bestand in de **inputdata** map in de **adfgetstarted** container. 

Nadat u de vereisten hebt voltooid, selecteert u een van de volgende hulpprogramma's / SDK's voor de zelfstudie: 

- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Azure portal en Visual Studio bieden GUI manier om uw data factory's te ontwikkelen. Terwijl de opties voor PowerShell, Resource Manager-sjabloon en REST-API biedt scripting/programmeren manier om uw data factory's te ontwikkelen.

> [!NOTE]
> Met de gegevenspijplijn in deze zelfstudie worden invoergegevens getransformeerd in uitvoergegevens. Er worden bijvoorbeeld geen gegevens gekopieerd van een brongegevensarchief naar een doelgegevensarchief. Meer informatie over het kopiëren van gegevens met behulp van Azure Data Factory vindt u in [Tutorial: Copy data from Blob storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Zelfstudie: gegevens kopiëren vanuit blobopslag naar SQL Database).
> 
> U kunt twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. Zie [Planning en uitvoering in Data Factory](data-factory-scheduling-and-execution.md) voor gedetailleerde informatie. 





  
