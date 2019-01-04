---
title: Azure Data Factory - voorbeelden
description: Bevat informatie over de voorbeelden die worden geleverd met de Azure Data Factory-service.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: c0538b90-2695-4c4c-a6c8-82f59111f4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 03127dc777588f669ef07af52c8f73d986bfe0ea
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020809"
---
# <a name="azure-data-factory---samples"></a>Azure Data Factory - voorbeelden
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [PowerShell-voorbeelden in Data Factory](../samples-powershell.md) en [codevoorbeelden in de galerie met Azure-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=data-factory).


## <a name="samples-on-github"></a>Voorbeelden op GitHub
De [GitHub Azure-Data Factory-opslagplaats](https://github.com/azure/azure-datafactory) bevat enkele voorbeelden waarmee u snel opbouwen met Azure Data Factory-service (of) wijzigen van de scripts en deze gebruiken in de eigen toepassing. De map Samples\JSON bevat JSON-fragmenten voor algemene scenario's.

| Voorbeeld | Description |
|:--- |:--- |
| [ADF-overzicht](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) |In dit voorbeeld bevat een end-to-end-overzicht voor het verwerken van de logboekbestanden met behulp van Azure Data Factory om in te schakelen van gegevens van de logboekbestanden in tot inzichten. <br/><br/>In dit scenario maakt de Data Factory-pijplijn verzamelt voorbeeldlogboeken, processen en verrijkt de gegevens uit logboeken met referentiegegevens, en transformeert de om te evalueren van de effectiviteit van een marketingcampagne die onlangs is gestart. |
| [JSON-voorbeelden](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) |In dit voorbeeld toont de JSON-voorbeelden voor algemene scenario's. |
| [Voorbeeld van gegevens van HTTP-Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |In dit voorbeeld demonstreert het downloaden van gegevens van een HTTP-eindpunt naar Azure Blob Storage met behulp van aangepaste .NET-activiteit. |
| [Cross-AppDomain Dot Net activiteit voorbeeld](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |In dit voorbeeld kunt u maken van een aangepaste .NET-activiteit die niet tot assembly-versies die worden gebruikt door het startprogramma voor ADF (bijvoorbeeld WindowsAzure.Storage verze 4.3.0, Newtonsoft.Json v6.0.x, enzovoort beperkt is). |
| [R-script uitvoeren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |In dit voorbeeld bevat de aangepaste activiteit van Data Factory die kan worden gebruikt om aan te roepen RScript.exe. In dit voorbeeld werkt alleen met uw eigen (niet) HDInsight-cluster op aanvraag al met R is geïnstalleerd op. |
| [Aanroepen van Spark-taken in HDInsight Hadoop-cluster](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) |In dit voorbeeld ziet u hoe u MapReduce-activiteit om aan te roepen een Spark-programma. Het spark-programma kopieert alleen gegevens van een Azure Blob-container naar een andere. |
| [Twitter-analyse met behulp van Azure Machine Learning Batch Scoring-activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) |In dit voorbeeld ziet u hoe u AzureMLBatchScoringActivity aanroepen van een Azure Machine Learning-model waarmee u twitter-sentimentanalyse, score, voorspelling enzovoort. |
| [Twitter-analyse met behulp van aangepaste activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |In dit voorbeeld ziet u hoe u een aangepaste .NET-activiteit om aan te roepen van een Azure Machine Learning-model waarmee u twitter-sentimentanalyse, score, voorspelling enzovoort. |
| [Met parameters pijplijnen voor Azure Machine Learning](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) |Het voorbeeld biedt een end-to-end C# code voor het implementeren van de N-pijplijnen voor het scoren en opnieuw trainen van elk met een andere regio parameter waarin de lijst met regio's afkomstig is van een bestand parameters.txt, dat opgenomen in dit voorbeeld is. |
| [Naslaginformatie over gegevensvernieuwing voor Azure Stream Analytics-taken](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |Dit voorbeeld laat zien hoe u Azure Data Factory en Azure Stream Analytics samen gebruiken voor het uitvoeren van de query's met referentiegegevens en de vernieuwing van referentiegegevens volgens een schema instellen. |
| [Hybride-pijplijn met On-premises Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) |Het voorbeeld wordt een on-premises Hadoop-cluster als een compute-doel voor het uitvoeren van taken in Data Factory, net zoals u andere compute-doelen toevoegen, zoals een HDInsight op basis van Hadoop-cluster in de cloud. |
| [JSON-hulpprogramma voor conversie](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) |Dit hulpprogramma kunt u de betreffende JSON's converteren naar de eerdere versie dan 2015-07-01-preview voor de meest recente of 2015-07-01-preview (standaard). |
| [U-SQL-bestand voor voorbeeld van invoer](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |Dit bestand is een voorbeeldbestand dat wordt gebruikt door een U-SQL-activiteit. |
| [, Blobbestand verwijderen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity) | In dit voorbeeld worden gepresenteerd een C# -bestand dat kan worden gebruikt als onderdeel van de aangepaste .net-activiteit ADF om bestanden te verwijderen uit Azure Blob-locatie van de bron nadat de bestanden zijn gekopieerd.|

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
U vindt de volgende Azure Resource Manager-sjablonen voor Data Factory op GitHub.

| Template | Description |
| --- | --- |
| [Kopiëren van Azure Blob Storage naar Azure SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Implementeren van deze sjabloon maakt u een Azure-gegevensfactory met een pijplijn waarmee gegevens uit de opgegeven Azure-blobopslag worden gekopieerd naar de Azure SQL database |
| [Kopiëren van Salesforce worden naar Azure Blob-opslag](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Implementeren van deze sjabloon maakt u een Azure-gegevensfactory met een pijplijn waarmee gegevens uit de opgegeven Salesforce-account gekopieerd naar de Azure blob-opslag. |
| [Gegevens transformeren met Hive-script uitgevoerd op een Azure HDInsight-cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Implementeren van deze sjabloon maakt u een Azure-gegevensfactory met een pijplijn waarmee gegevens worden getransformeerd door de voorbeeld-Hive-script uitgevoerd op een Azure HDInsight Hadoop-cluster. |

## <a name="samples-in-azure-portal"></a>Voorbeelden in Azure portal
U kunt de **voorbeeld pijplijnen** tegel op de startpagina van uw data factory voorbeeldpijplijnen en hun bijbehorende entiteiten (gegevenssets en gekoppelde services) implementeren aan uw data factory.

1. Een data factory maken of open een bestaande gegevensfactory. Zie [gegevens kopiëren van Blob Storage naar SQL Database met behulp van Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stappen voor het maken van een data factory.
2. In de **DATA FACTORY** blade voor de data factory, klikt u op de **voorbeeld pijplijnen** tegel.

    ![Voorbeeld van pijplijnen tegel](./media/data-factory-samples/SamplePipelinesTile.png)
3. In de **voorbeeld pijplijnen** blade, klikt u op de **voorbeeld** die u wilt implementeren.

    ![Voorbeeld pijplijnen-blade](./media/data-factory-samples/SampleTile.png)
4. Configuratie-instellingen voor het voorbeeld opgeven. Bijvoorbeeld: uw naam en sleutel van Azure storage-account, Azure SQL-servernaam, database, gebruikers-ID en wachtwoord, enzovoort.

    ![Voorbeeld van blade](./media/data-factory-samples/SampleBlade.png)
5. Nadat u klaar bent met de configuratie-instellingen op te geven, klikt u op **maken** maken/implementatie van de voorbeeldpijplijnen en gekoppelde services/tabellen die worden gebruikt door de pijplijnen.
6. U ziet de status van de implementatie van op de voorbeeld-tegel die u hebt geklikt op eerder de **voorbeeld pijplijnen** blade.

    ![Implementatiestatus](./media/data-factory-samples/DeploymentStatus.png)
7. Als u ziet de **implementatie is voltooid** bericht op de tegel voor het voorbeeld, sluit de **voorbeeld pijplijnen** blade.  
8. Op **DATA FACTORY** blade ziet u dat de gekoppelde services, gegevenssets en pijplijnen worden toegevoegd aan uw gegevensfactory.  

    ![Blade Gegevensfactory](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>Voorbeelden in Visual Studio
### <a name="prerequisites"></a>Vereisten
De volgende zaken moeten op uw computer zijn geïnstalleerd:

* Visual Studio 2013 of Visual Studio 2015
* Download de Azure SDK voor Visual Studio 2013 of Visual Studio 2015. Ga naar de [Azure-downloadpagina](https://azure.microsoft.com/downloads/) en klik in het gedeelte **.NET** op **VS 2013** of **VS 2015**.
* Download de nieuwste Azure Data Factory-invoegtoepassing voor Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Als u Visual Studio 2013 gebruikt, kunt u de invoegtoepassing ook bijwerken door de volgende stappen: In het menu, klikt u op **extra** -> **extensies en Updates** -> **Online** -> **voorVisualStudio-galerie**  ->  **Microsoft Azure Data Factory-hulpprogramma's voor Visual Studio** -> **Update**.

### <a name="use-data-factory-templates"></a>Gebruik Data Factory-sjablonen
1. Klik op **bestand** Wijs in het menu **nieuw**, en klikt u op **Project**.
2. Voer in het dialoogvenster **Nieuw project** de volgende stappen uit:

   1. Selecteer **DataFactory** onder **sjablonen**.
   2. Selecteer **Data Factory sjablonen** in het rechter deelvenster.
   3. Voer een **naam** in voor het project.
   4. Selecteer een **locatie** voor het project.
   5. Klik op **OK**.

      ![Het dialoogvenster New Project](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. In de **Data Factory sjablonen** dialoogvenster vak, selecteert u de voorbeeldsjabloon uit de **Use-Case-sjablonen** uit en klikt u op **volgende**. De volgende stappen helpen u bij met behulp van de **klant profilering** sjabloon. Stappen zijn vergelijkbaar voor de andere voorbeelden.

    ![In het dialoogvenster van Data Factory-sjablonen](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. In de **configuratie van Data Factory** dialoogvenster, klikt u op **volgende** op de **Data Factory basisbeginselen** pagina.
5. Op de **Configure data factory** pagina, de volgende stappen uit:
   1. Selecteer **maken van nieuwe Data Factory**. U kunt ook selecteren **gebruik bestaande data factory**.
   2. Voer een **naam** voor de data factory.
   3. Selecteer de **Azure-abonnement** in die u de data factory wilt moet worden gemaakt.
   4. Selecteer de **resourcegroep** voor de data factory.
   5. Selecteer de **VS-West**, **VS-Oost**, of **Noord-Europa** voor de **regio**.
   6. Klik op **volgende**.
6. In de **configureren gegevensarchieven** pagina, geeft u een bestaande **Azure SQL-database** en **Azure storage-account** (of) maken van database-opslag, en klik op volgende.
7. In de **berekening configureren** pagina, selecteert u standaardwaarden en klikt u op **volgende**.
8. In de **samenvatting** pagina, Controleer de instellingen en klik op **volgende**.
9. In de **Implementatiestatus** pagina, wacht totdat de implementatie is voltooid en op **voltooien**.
10. Klik met de rechtermuisknop op het project in Solution Explorer. Klik vervolgens op **Publish**.
11. Als u het dialoogvenster **Sign in to your Microsoft account** ziet, voert u uw referenties in voor het account met het Azure-abonnement en klikt u op **Sign in**.
12. Het volgende dialoogvenster wordt weergegeven:

    ![Het dialoogvenster Publish](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. Op de pagina **Configure data factory** voert u de volgende stappen uit:

    1. Bevestig dat **gebruik bestaande data factory** optie.
    2. Selecteer de **gegevensfactory** moest u selecteert bij het gebruik van de sjabloon.
    3. Klik op **Next** om over te schakelen naar de pagina **Publish Items**. (Druk op **TAB** als u het veld Naam wilt verlaten, maar de knop **Next** is uitgeschakeld.)
14. Op de pagina **Publish Items** controleert u of alle Data Factory-entiteiten zijn geselecteerd en klikt u op **Next** om over te schakelen naar de pagina **Summary**.     
15. Controleer de samenvatting en klik op **Next** om te beginnen met het implementatieproces en om de **implementatiestatus** te bekijken.
16. Op de pagina **Deployment Status** ziet u de status van het implementatieproces. Klik op Finish wanneer de implementatie is uitgevoerd.

Zie [(Visual Studio) van uw eerste gegevensfactory bouwen](data-factory-build-your-first-pipeline-using-vs.md) voor meer informatie over het gebruik van Visual Studio naar Data Factory-entiteiten maken en publiceren naar Azure.          
