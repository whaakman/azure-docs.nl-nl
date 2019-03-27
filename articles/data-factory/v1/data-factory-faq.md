---
title: Azure Data Factory - Veelgestelde vragen
description: Veelgestelde vragen over Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 518e3fa842c5283dc20a6111773bd55451f026b6
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485846"
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Azure Data Factory - Veelgestelde vragen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [veelgestelde vraag - Data Factory](../frequently-asked-questions.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="general-questions"></a>Algemene vragen
### <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory?
Data Factory is een cloudgebaseerde gegevensintegratie service die **automatiseert de verplaatsing en transformatie van gegevens**. Net als bij een factory die wordt uitgevoerd apparatuur wordt gebruikt om grondstoffen te transformeren in eindproducten, beheert Data Factory bestaande services die onbewerkte gegevens verzamelen en transformeren in kant-en-klare informatie.

Data Factory kunt u gegevensgestuurde werkstromen maken om te verplaatsen van gegevens tussen zowel on-premises en cloudlocaties voor gegevensopslag, evenals verwerken/transformeren gegevens met behulp van rekenservices zoals Azure HDInsight en Azure Data Lake Analytics. Nadat u een pijplijn die de actie die u nodig hebt maken, kunt u plannen dat het periodiek wordt uitgevoerd (elk uur, dagelijks, wekelijks enz.).   

Zie voor meer informatie, [overzicht en belangrijkste concepten](data-factory-introduction.md).

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Waar vind ik prijsinformatie voor Azure Data Factory?
Zie [pagina met prijsinformatie van Data Factory] [ adf-pricing-details] voor de prijsinformatie voor Azure Data Factory.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Hoe ga ik aan de slag met Azure Data Factory?
* Zie voor een overzicht van Azure Data Factory, [Inleiding tot Azure Data Factory](data-factory-introduction.md).
* Voor een zelfstudie over het **kopiëren en verplaatsen van gegevens** met activiteit kopiëren, Zie [gegevens kopiëren van Azure Blob Storage naar Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Voor een zelfstudie over het **gegevens transformeren** met behulp van HDInsight Hive-activiteit. Zie [gegevens verwerken met Hive-script uitgevoerd op Hadoop-cluster](data-factory-build-your-first-pipeline.md)

### <a name="what-is-the-data-factorys-region-availability"></a>Wat is de beschikbaarheid in regio's van de Data Factory?
Data Factory is beschikbaar in **VS West** en **Noord-Europa**. De reken- en -services die worden gebruikt door data factory's kunnen zich in andere regio's. Zie [ondersteunde regio's](data-factory-introduction.md#supported-regions).

### <a name="what-are-the-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Wat zijn de beperkingen voor aantal van de data factory's / pijplijnen/activiteiten/gegevenssets?
Zie **Azure Data Factory-limieten** sectie van de [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../../azure-subscription-service-limits.md#data-factory-limits) artikel.

### <a name="what-is-the-authoringdeveloper-experience-with-azure-data-factory-service"></a>Wat is de ervaring van ontwerpen of ontwikkelaar met Azure Data Factory-service?
U kunt de auteur/data factory's maken met een van de volgende hulpprogramma's / SDK's:

* **Azure-portal** blades van de Data Factory in Azure portal bieden uitgebreide gebruikersinterface waarmee u kunt data factory's ad gekoppelde services maken. De **Data Factory-Editor**, die maakt ook deel uit van de portal, kunt u eenvoudig gekoppelde services, tabellen, gegevenssets en pijplijnen kunt maken door JSON-definities voor de artefacten op te geven. Zie [bouw uw eerste pijplijn met behulp van Azure portal](data-factory-build-your-first-pipeline-using-editor.md) voor een voorbeeld van het gebruik van de portal/editor maken en implementeren van een data factory.
* **Visual Studio** u Visual Studio kunt gebruiken om te maken van een Azure data factory. Zie [bouw uw eerste pijplijn met Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) voor meer informatie.
* **Azure PowerShell** Zie [maken en te bewaken, Azure Data Factory met Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) voor een zelfstudie/overzicht voor het maken van een data factory met behulp van PowerShell. Zie [Data Factory Cmdlet Reference] [ adf-powershell-reference] inhoud op MSDN-bibliotheek voor een uitgebreide documentatie over Data Factory-cmdlets.
* **.NET-klassebibliotheek** programmatisch kunt u data factory's maken met behulp van Data Factory .NET SDK. Zie [maken, bewaken en beheren van data factory's met .NET SDK](data-factory-create-data-factories-programmatically.md) voor een overzicht van het maken van een data factory met .NET SDK. Zie [Data Factory Class Library Reference] [ msdn-class-library-reference] voor een uitgebreide documentatie over Data Factory .NET SDK.
* **REST-API** u kunt ook de REST-API die worden weergegeven door de Azure Data Factory-service maken en implementeren van data factory's gebruiken. Zie [API-naslaginformatie voor Data Factory-REST] [ msdn-rest-api-reference] voor een uitgebreide documentatie over Data Factory-REST API.
* **Azure Resource Manager-sjabloon** Zie [zelfstudie: Uw eerste Azure-gegevensfactory bouwen met Azure Resource Manager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md) voor meer informatie.

### <a name="can-i-rename-a-data-factory"></a>Kan ik de naam van een data factory wijzigen?
Nee. Net als andere Azure-resources, kan niet de naam van een Azure data factory worden gewijzigd.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-to-another"></a>Kan ik een data factory verplaatsen van één Azure-abonnement naar een andere?
Ja. Gebruik de **verplaatsen** knop op uw data factory-blade zoals weergegeven in het volgende diagram:

![Data factory verplaatsen](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-the-compute-environments-supported-by-data-factory"></a>Wat zijn de omgevingen die door Data Factory worden ondersteund?
De volgende tabel bevat een lijst met compute-omgevingen wordt ondersteund door Data Factory en de activiteiten die kunnen worden uitgevoerd op deze.

| Compute-omgeving | activities |
| --- | --- |
| [On-demand HDInsight-cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) of [uw eigen HDInsight-cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](data-factory-compute-linked-services.md#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Machine Learning-activiteiten: Batchuitvoering en resources bijwerken](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL Data Warehouse](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [SQL Server](data-factory-compute-linked-services.md#sql-server-linked-service) |[Opgeslagen procedure](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Hoe Azure Data Factory vergelijken met SQL Server Integration Services (SSIS)? 
Zie de [vs Azure Data Factory. SSIS](https://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) presentatie op basis van een van onze MVP's (meest waardevolle Professionals): Reza Rad. Sommige van de meest recente wijzigingen in Data Factory kan niet worden weergegeven in het PowerPoint-presentatie. We zijn voortdurend meer mogelijkheden toegevoegd aan Azure Data Factory. We zijn voortdurend meer mogelijkheden toegevoegd aan Azure Data Factory. We zullen deze updates opnemen in de vergelijking van data-integratie-technologieën van Microsoft enige tijd later dit jaar.   

## <a name="activities---faq"></a>Activiteiten - Veelgestelde vragen
### <a name="what-are-the-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Wat zijn de verschillende typen activiteiten die u in een Data Factory-pijplijn gebruiken kunt?
* [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) om gegevens te verplaatsen.
* [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) te verwerken/transformeren van gegevens.

### <a name="when-does-an-activity-run"></a>Wanneer wordt een activiteit uitgevoerd?
De **beschikbaarheid** configuratie-instelling in de tabel met uitvoer bepaalt wanneer de activiteit wordt uitgevoerd. Als invoergegevenssets zijn opgegeven, de activiteit wordt gecontroleerd of alle afhankelijkheden van de invoergegevens zijn voldaan (dat wil zeggen, **gereed** staat) voordat het wordt gestart.

## <a name="copy-activity---faq"></a>Kopieeractiviteit - Veelgestelde vragen
### <a name="is-it-better-to-have-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Is het beter om een pijplijn met meerdere activiteiten of een afzonderlijke pijplijn voor elke activiteit?
Pijplijnen zijn mag bundelen gerelateerde activiteiten. Als de gegevenssets waarmee ze zijn verbonden niet worden gebruikt door een willekeurige andere activiteit buiten de pijplijn, kunt u de activiteiten in één pijplijn. Op deze manier u niet moet keten pijplijn actieve perioden zodat deze worden uitgelijnd met elkaar. De integriteit van gegevens in de tabellen die intern is aan de pijplijn is ook beter behouden bij het bijwerken van de pijplijn. Update van de pijplijn in feite stopt alle activiteiten in de pijplijn, verwijdert u deze en maakt deze opnieuw. Vanuit het perspectief ontwerpen, kan het ook eenvoudiger om te zien van de stroom van gegevens binnen de gerelateerde activiteiten in een JSON-bestand voor de pijplijn zijn.

### <a name="what-are-the-supported-data-stores"></a>Wat zijn de ondersteunde gegevensarchieven?
De kopieeractiviteit in Data Factory kopieert gegevens van een brongegevensarchief naar een sinkgegevensarchief. Data Factory ondersteunt de volgende gegevensarchieven. Gegevens vanuit elke willekeurige bron kunnen naar een sink worden geschreven. Klik op een gegevensarchief voor informatie over het kopiëren van gegevens naar en van dat archief.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Gegevensarchieven met een * kunnen zich on-premises of op Azure IaaS bevinden. Hiervoor moet u [Data Management Gateway](data-factory-data-management-gateway.md) installeren op een on-premises/Azure IaaS-computer.

### <a name="what-are-the-supported-file-formats"></a>Wat zijn de ondersteunde bestandsindelingen?
[!INCLUDE [data-factory-file-format](../../../includes/data-factory-file-format.md)]

### <a name="where-is-the-copy-operation-performed"></a>Waar wordt de kopieerbewerking uitgevoerd?
Zie [wereldwijd beschikbaar gegevensverplaatsing](data-factory-data-movement-activities.md#global) sectie voor meer informatie. Kort gezegd, wanneer een on-premises-gegevensarchief is betrokken, wordt de kopieerbewerking uitgevoerd door de Data Management Gateway in uw on-premises omgeving. En wanneer de verplaatsing van gegevens tussen twee cloudopslagvoorzieningen, de kopieerbewerking wordt uitgevoerd in de regio het dichtst bij de sink-locatie in hetzelfde geografische gebied.

## <a name="hdinsight-activity---faq"></a>HDInsight-activiteit - Veelgestelde vragen
### <a name="what-regions-are-supported-by-hdinsight"></a>Welke regio's worden ondersteund door HDInsight?
Zie de sectie geografische beschikbaarheid in het volgende artikel: of [prijsinformatie voor HDInsight][hdinsight-supported-regions].

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>Welke regio wordt gebruikt door een on-demand HDInsight-cluster?
De on-demand HDInsight-cluster wordt gemaakt in dezelfde regio waar de opslag die u hebt opgegeven om te worden gebruikt met het cluster zich bevindt.    

### <a name="how-to-associate-additional-storage-accounts-to-your-hdinsight-cluster"></a>Hoe kan ik extra opslagaccounts met uw HDInsight-cluster koppelen?
Als u uw eigen HDInsight-Cluster (BYOC - Bring Your Own Cluster), raadpleegt u de volgende onderwerpen:

* [Met behulp van een HDInsight-Cluster met alternatieve Opslagaccounts en Metastores][hdinsight-alternate-storage]
* [Extra Opslagaccounts gebruiken met HDInsight Hive][hdinsight-alternate-storage-2]

Als u een cluster op aanvraag die is gemaakt door de Data Factory-service gebruikt, geeft u extra opslagaccounts voor het HDInsight gekoppelde service zodat de Data Factory-service namens registreren kan. Gebruik in de JSON-definitie voor de gekoppelde service op aanvraag, **, additionallinkedservicenames,** eigenschap opgeven van alternatieve storage-accounts, zoals wordt weergegeven in de volgende JSON-fragment:

```JSON
{
    "name": "MyHDInsightOnDemandLinkedService",
    "properties":
    {
        "type": "HDInsightOnDemandLinkedService",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ]
        }
    }
}
```
In het bovenstaande voorbeeld vertegenwoordigen otherLinkedServiceName1 en otherLinkedServiceName2 van gekoppelde services waarvan definities referenties die het HDInsight-cluster nodig heeft bevatten voor toegang tot andere storage-accounts.

## <a name="slices---faq"></a>Segmenten - Veelgestelde vragen
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Waarom wordt mijn invoersegmenten niet in de status gereed hebben?
Een veelvoorkomende fout is de instelling **externe** eigenschap **waar** voor de invoergegevensset wanneer de invoergegevens zich buiten de data factory (niet geproduceerd door de data factory).

In het volgende voorbeeld, hoeft u alleen om in te stellen **externe** op ' True ' voor **dataset1**.  

**DataFactory1** Pipeline-1: dataset1 -> activiteit1 -> dataset2 -> activiteit2 dataset3 pijplijn 2 ->: dataset3 -> activiteit3 dataset4 ->

Als u een andere data factory met een pijplijn die dataset4 (die worden geproduceerd door de pijplijn 2 in data factory, 1) nodig hebt, moet u dataset4 markeren als een externe gegevensset omdat de gegevensset wordt geproduceerd door een andere data factory (DataFactory1, niet DataFactory2).  

**DataFactory2**    
Pijplijn 1: dataset4 -> activity4 dataset5 ->

Als de eigenschap external juist is ingesteld, controleert u of de ingevoerde gegevens in de opgegeven locatie in de definitie van de invoergegevensset bestaat.

### <a name="how-to-run-a-slice-at-another-time-than-midnight-when-the-slice-is-being-produced-daily"></a>Het uitvoeren van een segment op een later tijdstip dan middernacht wanneer het segment dagelijks wordt geproduceerd?
Gebruik de **offset** eigenschap om op te geven van de tijd die u wilt dat het segment wordt geproduceerd. Zie [beschikbaarheid van gegevenssets](data-factory-create-datasets.md#dataset-availability) sectie voor meer informatie over deze eigenschap. Hier volgt een kort voorbeeld:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Dagelijkse segmenten beginnen bij **6 AM** in plaats van de standaard-middernacht.     

### <a name="how-can-i-rerun-a-slice"></a>Hoe kan ik een segment opnieuw uitvoeren?
U kunt een segment opnieuw uitvoeren in een van de volgende manieren:

* Monitor and Manage App gebruiken een activiteitsvenster of het segment opnieuw uit te voeren. Zie [opnieuw uitvoeren geselecteerd activiteitsvensters](data-factory-monitor-manage-app.md#perform-batch-actions) voor instructies.   
* Klik op **uitvoeren** in de opdrachtbalk op de **GEGEVENSSEGMENT** blade voor het segment in de Azure-portal.
* Voer **Set AzDataFactorySliceStatus** cmdlet met de Status ingesteld op **wachten** voor het segment.   

    ```powershell
    Set-AzDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
  Zie [Set AzDataFactorySliceStatus] [ set-azure-datafactory-slice-status] voor meer informatie over de cmdlet.

### <a name="how-long-did-it-take-to-process-a-slice"></a>Hoe lang het voordat een segment verwerken?
Activiteitsvensterverkenner in de Monitor & Manage App gebruiken om te weten hoe lang het heeft geduurd voor het verwerken van een gegevenssegment. Zie [Activiteitsvensterverkenner](data-factory-monitor-manage-app.md#activity-window-explorer) voor meer informatie.

U kunt ook in Azure portal het volgende doen:  

1. Klik op **gegevenssets** tegel op de **DATA FACTORY** blade voor uw data factory.
2. Klik op de specifieke gegevensset in de **gegevenssets** blade.
3. Selecteer het segment dat u geïnteresseerd in van bent de **recente segmenten** lijst op de **tabel** blade.
4. Klik op de activiteit die wordt uitgevoerd vanuit de **uitvoeringen van activiteit** lijst op de **GEGEVENSSEGMENT** blade.
5. Klik op **eigenschappen** tegel op de **DETAILS uitvoering van activiteit** blade.
6. U ziet de **duur** veld met een waarde. Deze waarde is de tijd voor het verwerken van het segment.   

### <a name="how-to-stop-a-running-slice"></a>Het stoppen van een actief segment?
Als u stoppen met de pijplijn wilt wordt uitgevoerd, kunt u [stand-by-AzDataFactoryPipeline](/powershell/module/az.datafactory/suspend-azdatafactorypipeline) cmdlet. Op dit moment stopt onderbreken van de pijplijn niet de segment-uitvoeringen die uitgevoerd worden. Zodra u klaar bent met de uitvoeringen wordt uitgevoerd, wordt geen extra segment opgehaald.

Als u echt onmiddellijk stoppen alle uitvoeringen wilt, is de enige manier om de pijplijn verwijderen en opnieuw maken. Als u verwijderen van de pijplijn wilt, hoeft u niet om tabellen en gekoppelde services die worden gebruikt door de pijplijn te verwijderen.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/module/az.datafactory/
[azure-portal]: https://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/module/az.datafactory/set-Azdatafactoryslicestatus

[adf-pricing-details]: https://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: https://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: https://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
