---
title: Azure Data Factory - voorbeelden
description: Bevat informatie over de voorbeelden die worden geleverd met de Azure Data Factory-service.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c0538b90-2695-4c4c-a6c8-82f59111f4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: e72cdee5b164fcd4944f4f1f95ff22bb5ceba6a4
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---samples"></a>Azure Data Factory - voorbeelden
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [PowerShell-voorbeelden in de Data Factory versie 2](../samples-powershell.md) en [codevoorbeelden in de galerie van Azure-codevoorbeelden](https://azure.microsoft.com/en-us/resources/samples/?service=data-factory).


## <a name="samples-on-github"></a>Voorbeelden op GitHub
De [GitHub Azure-DataFactory opslagplaats](https://github.com/azure/azure-datafactory) bevat enkele voorbeelden waarmee u snel extra met Azure Data Factory-service (of) wijzigen van de scripts en deze gebruiken in de eigen toepassing. De map Samples\JSON bevat JSON-codefragmenten voor veelvoorkomende scenario's.

| Voorbeeld | Beschrijving |
|:--- |:--- |
| [ADF Walkthrough](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) |Dit voorbeeld biedt een end-to-end-overzicht voor het verwerken van logboekbestanden met Azure Data Factory om gegevens te sluiten van de logboekbestanden in te insights. <br/><br/>In dit scenario de Data Factory-pijplijn verzamelt voorbeeld Logboeken, processen en verrijkt de gegevens uit logboeken met referentiegegevens, en transformeert de gegevens voor het evalueren van de effectiviteit van een marketingcampagne dat onlangs is gestart. |
| [JSON-voorbeelden](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) |Dit voorbeeld ziet u voorbeelden van JSON voor veelvoorkomende scenario's. |
| [Gegevens van de HTTP-Downloadprogramma voor het voorbeeld](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |In dit voorbeeld showcases downloaden van gegevens van een HTTP-eindpunt naar Azure Blob Storage met behulp van aangepaste .NET-activiteit. |
| [Cross-AppDomain punt Net activiteit voorbeeld](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Dit voorbeeld kunt u schrijft een aangepaste .NET-activiteit die is niet beperkt tot assembly-versies die worden gebruikt door het starten van de ADF (bijvoorbeeld WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, enz.). |
| [R-script uitvoeren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |Dit voorbeeld bevat de Data Factory aangepaste activiteit die kan worden gebruikt om aan te roepen RScript.exe. Dit voorbeeld werkt alleen met uw eigen HDInsight-cluster (niet op aanvraag) die al R is geïnstalleerd is. |
| [Aanroepen van de projecten Spark in HDInsight Hadoop-cluster](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) |Dit voorbeeld toont hoe u MapReduce-activiteit gebruikt om aan te roepen, een Spark-programma. Het spark-programma kopieert alleen gegevens uit een Azure Blob-container naar een andere. |
| [Twitter-analyse met behulp van Azure Machine Learning Batch score berekenen voor activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) |Dit voorbeeld laat zien hoe AzureMLBatchScoringActivity gebruiken om aan te roepen, een Azure Machine Learning-model dat twitter-gevoel analyse voert, score berekenen voorspelling enzovoort. |
| [Twitter-analyse met behulp van aangepaste activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Dit voorbeeld toont hoe u een aangepaste activiteit .NET aanroepen van een Azure Machine Learning-model dat twitter-gevoel analyse voert, score berekenen voorspelling enzovoort. |
| [Geparameteriseerde pijplijnen voor Azure Machine Learning](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) |Het voorbeeld biedt een end-to-end C#-code voor het implementeren van N pijplijnen voor score berekenen en retraining elk met een andere regio parameter waarbij de lijst met regio's afkomstig is van een bestand parameters.txt, dat opgenomen in dit voorbeeld is. |
| [Verwijzing voor gegevensvernieuwing voor Azure Stream Analytics-taken](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |Dit voorbeeld laat zien hoe Azure Data Factory en Azure Stream Analytics samen gebruiken voor het uitvoeren van de query's met referentiegegevens en de vernieuwing voor referentiegegevens volgens een schema instellen. |
| [Hybride-pijplijn met On-premises Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) |Dit voorbeeld worden een lokale Hadoop-cluster als een compute-doel voor het uitvoeren van taken in de Data Factory, net zoals u andere compute-doelen toevoegt zou als een HDInsight op basis van Hadoop-cluster in de cloud. |
| [JSON-hulpprogramma voor conversie](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) |Dit hulpprogramma kunt u JSONs converteren van eerdere versie dan 2015-07-01-preview nieuwste of 2015-07-01-preview (standaard). |
| [U-SQL-voorbeeldbestand invoer](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |Dit bestand is een voorbeeld-bestand gebruikt door een U-SQL-activiteit. |
| [Blob-bestand verwijderen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity) | Dit voorbeeld gepresenteerd een C#-bestand dat kan worden gebruikt als onderdeel van aangepaste .net-activiteit van ADF om bestanden te verwijderen van de bron Azure Blob-locatie als de bestanden zijn gekopieerd.|

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
U vindt de volgende Azure Resource Manager-sjablonen voor Data Factory op GitHub.

| Template | Beschrijving |
| --- | --- |
| [Kopiëren van Azure Blob-opslag naar Azure SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Implementatie van deze sjabloon maakt u een Azure data factory met een pijplijn waarmee gegevens uit de opgegeven Azure blob-opslag worden gekopieerd naar de Azure SQL database |
| [Kopiëren van Salesforce naar Azure Blob-opslag](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Implementatie van deze sjabloon maakt u een Azure data factory met een pijplijn waarmee gegevens uit de opgegeven Salesforce-account worden gekopieerd naar de Azure blob storage. |
| [Transformeer gegevens door het Hive-script uitvoeren op een Azure HDInsight-cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Implementatie van deze sjabloon maakt u een Azure data factory met een pijplijn waarmee gegevens worden omgezet door het voorbeeld Hive-script uitgevoerd op een Azure HDInsight Hadoop-cluster. |

## <a name="samples-in-azure-portal"></a>De voorbeelden in Azure-portal
U kunt de **steekproef pijplijnen** tegel op de startpagina van uw gegevensfactory voorbeeld pijplijnen en hun bijbehorende entiteiten (gegevenssets en gekoppelde services) implementeren in aan uw gegevensfactory.

1. Maak een gegevensfactory of open een bestaande gegevensfactory. Zie [gegevens kopiëren van Blob-opslag naar SQL Database met behulp van Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stappen voor het maken van een gegevensfactory.
2. In de **DATA FACTORY** blade voor de gegevensfactory klikt u op de **steekproef pijplijnen** tegel.

    ![Voorbeeld pijplijnen tegel](./media/data-factory-samples/SamplePipelinesTile.png)
3. In de **steekproef pijplijnen** blade, klikt u op de **voorbeeld** die u wilt implementeren.

    ![Voorbeeld pijplijnen-blade](./media/data-factory-samples/SampleTile.png)
4. Geef de configuratie-instellingen voor het voorbeeld. Bijvoorbeeld: uw Azure storage-account en de accountsleutel sleutel, naam van de Azure SQL-server, database, gebruikers-ID en wachtwoord, enzovoort.

    ![Voorbeeld-blade](./media/data-factory-samples/SampleBlade.png)
5. Nadat u klaar bent met het opgeven van de configuratie-instellingen, klikt u op **maken** voor het maken/implementeren van de steekproef pijplijnen en gekoppelde services/tabellen die worden gebruikt door de pijplijnen.
6. U ziet de status van implementatie op de voorbeeld-tegel waarop u hebt geklikt eerder op de **steekproef pijplijnen** blade.

    ![Implementatiestatus](./media/data-factory-samples/DeploymentStatus.png)
7. Als u ziet de **implementatie is voltooid** bericht op de tegel voor het voorbeeld, sluit de **steekproef pijplijnen** blade.  
8. Op **DATA FACTORY** blade ziet u dat de gekoppelde services, gegevenssets en pijplijnen worden toegevoegd aan uw gegevensfactory.  

    ![Blade Gegevensfactory](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>Voorbeelden Visual Studio
### <a name="prerequisites"></a>Vereisten
De volgende zaken moeten op uw computer zijn geïnstalleerd:

* Visual Studio 2013 of Visual Studio 2015
* Download de Azure SDK voor Visual Studio 2013 of Visual Studio 2015. Ga naar de [Azure-downloadpagina](https://azure.microsoft.com/downloads/) en klik in het gedeelte **.NET** op **VS 2013** of **VS 2015**.
* Download de nieuwste Azure Data Factory-invoegtoepassing voor Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Als u gebruikmaakt van Visual Studio 2013, kunt u de invoegtoepassing ook bijwerken met behulp van de volgende stappen: klik in het menu op **Extra** -> **Extensies en updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure Data Factory-hulpprogramma's voor Visual Studio** -> **Bijwerken**.

### <a name="use-data-factory-templates"></a>Gebruik Data Factory-sjablonen
1. Klik op **bestand** Wijs in het menu **nieuw**, en klik op **Project**.
2. In de **nieuw Project** dialoogvenster handelingen uit de volgende stappen uit:

   1. Selecteer **DataFactory** onder **sjablonen**.
   2. Selecteer **Data Factory sjablonen** in het rechterdeelvenster.
   3. Voer een **naam** voor het project.
   4. Selecteer een **locatie** voor het project.
   5. Klik op **OK**.

      ![Het dialoogvenster New Project](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. In de **Data Factory sjablonen** dialoogvenster Selecteer de voorbeeldsjabloon van de **Use Case sjablonen** sectie en op **volgende**. De volgende stappen maakt u met behulp van de **klant profilering** sjabloon. Stappen zijn vergelijkbaar voor de andere voorbeelden.

    ![Dialoogvenster voor Data Factory-sjablonen](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. In de **Data Factory-configuratie** dialoogvenster, klikt u op **volgende** op de **Data Factory-basisbeginselen** pagina.
5. Op de **Configure data factory** pagina, voert u de volgende stappen uit:
   1. Selecteer **maken van nieuwe Gegevensfactory**. U kunt ook selecteren **bestaande data factory gebruiken**.
   2. Voer een **naam** voor de data factory.
   3. Selecteer de **Azure-abonnement** in die u de gegevensfactory wilt wilt maken.
   4. Selecteer de **resourcegroep** voor de data factory.
   5. Selecteer de **VS-West**, **VS-Oost**, of **Noord-Europa** voor de **regio**.
   6. Klik op **Volgende**.
6. In de **gegevensarchieven configureren** pagina, geeft u een bestaande **Azure SQL-database** en **Azure storage-account** (of) maken van database-opslag en klik op volgende.
7. In de **compute configureren** pagina, Selecteer standaardwaarden en klik op **volgende**.
8. In de **samenvatting** pagina, controleert u alle instellingen en klik op **volgende**.
9. In de **Implementatiestatus** pagina, wacht totdat de implementatie is voltooid en klik op **voltooien**.
10. Klik met de rechtermuisknop op het project in Solution Explorer. Klik vervolgens op **Publish**.
11. Als u het dialoogvenster **Sign in to your Microsoft account** ziet, voert u uw referenties in voor het account met het Azure-abonnement en klikt u op **Sign in**.
12. Het volgende dialoogvenster wordt weergegeven:

    ![Het dialoogvenster Publish](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. Op de pagina **Configure data factory** voert u de volgende stappen uit:

    1. Bevestig dat **bestaande data factory gebruiken** optie.
    2. Selecteer de **gegevensfactory** moest u selecteert wanneer u de sjabloon.
    3. Klik op **Next** om over te schakelen naar de pagina **Publish Items**. (Druk op **TAB** als u het veld Naam wilt verlaten, maar de knop **Next** is uitgeschakeld.)
14. Op de pagina **Publish Items** controleert u of alle Data Factory-entiteiten zijn geselecteerd en klikt u op **Next** om over te schakelen naar de pagina **Summary**.     
15. Controleer de samenvatting en klik op **Next** om te beginnen met het implementatieproces en om de **implementatiestatus** te bekijken.
16. Op de pagina **Deployment Status** ziet u de status van het implementatieproces. Klik op Finish wanneer de implementatie is uitgevoerd.

Zie [(Visual Studio) van uw eerste gegevensfactory bouwen](data-factory-build-your-first-pipeline-using-vs.md) voor meer informatie over het gebruik van Visual Studio Data Factory-entiteiten schrijven en te publiceren naar Azure.          
