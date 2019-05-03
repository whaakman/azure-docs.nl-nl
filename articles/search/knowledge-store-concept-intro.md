---
title: Overzicht - Azure Search en Knowledge Store-Inleiding
description: Verrijkt documenten verzenden naar Azure-opslag waar u kunt weergeven, vorm en verrijkt documenten in Azure Search en andere toepassingen gebruiken.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: overview
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: 3000016de934aaa3faab96821f9747ea4b571ef7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026996"
---
# <a name="what-is-knowledge-store-in-azure-search"></a>Wat is kennis Store in Azure Search?

Kennis Store is een optionele functie van Azure Search, momenteel in openbare preview, die wordt opgeslagen als verrijkt documenten en metagegevens die zijn gemaakt door een-op basis van AI indexering pijplijn [(cognitief zoeken)](cognitive-search-concept-intro.md). Kennis Store wordt ondersteund door Azure storage-account dat u als onderdeel van de pijplijn configureert. Wanneer ingeschakeld, de search-service maakt gebruik van dit opslagaccount wordt gebruikt om een weergave van elk document verrijkt in cache. 

Als u cognitief zoeken in het verleden hebt gebruikt, weet u al dat kennis en vaardigheden kunnen worden gebruikt om een document via een reeks enrichments te verplaatsen. Het resultaat is een Azure Search-index of (nieuw in deze Preview-versie) projecties in een winkel kennis.

Projecties zijn uw mechanisme voor het structureren van gegevens voor gebruik in een downstream-app. U kunt [Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) die is gebouwd voor Azure storage of alle Apps die verbinding met Azure storage maakt, die wordt geopend nieuwe mogelijkheden voor het gebruik van verrijkt documenten. Enkele voorbeelden zijn data science-pijplijnen en aangepaste analyse.

![Kennis Store in overzichtsdiagram van gegevenspijplijn](./media/knowledge-store-concept-intro/annotationstore_sans_internalcache.png "kennis Store in overzichtsdiagram van gegevenspijplijn")

Voor het gebruik van kennis Store toevoegen een `knowledgeStore` element in een set vaardigheden die stap voor stap bewerkingen in een pijplijn voor indexering definieert. Tijdens de uitvoering Azure Search wordt een spatie in uw Azure storage-account gemaakt en gevuld met de definities en inhoud die is gemaakt door de pijplijn.

## <a name="benefits-of-knowledge-store"></a>Voordelen van kennis Store

Een kennis biedt u structuur, context en inhoud - afgelezen uit ongestructureerde en semi-gestructureerde gegevensbestanden, zoals blobs, bestanden die hebben ondergaan analyse, of zelfs van gestructureerde gegevens die naar nieuwe formulieren is omgevormd opslaan. In een [stapsgewijze](knowledge-store-howto.md) die zijn geschreven voor deze Preview-versie, kunt u eerste hand hoe een compacte JSON-document is gepartitioneerd in substructuren, geregenereerd in nieuwe structuren en anders beschikbaar gesteld voor downstream bekijken processen zoals machine learning en data science-werkbelastingen.

Hoewel het is handig om te zien wat een indexering op basis van AI-pijplijn kunt maken, is de ware kracht van kennis Store de mogelijkheid om te zetten om gegevens. U kunt beginnen met een eenvoudige set vaardigheden en vervolgens herhalen worden steeds meer niveaus van de structuur, die u vervolgens tot nieuwe structuren, verbruikbare in andere apps naast Azure Search combineren kunt toevoegen.

De voordelen van kennis Store zijn geïnventariseerd, het volgende:

+ Verrijkt documenten in gebruiken [analyses en hulpprogramma's voor rapportage](#tools-and-apps) dan zoeken. Power BI met Power Query is een aantrekkelijke keuze, maar een hulpprogramma of de app die u verbinding met Azure storage maken kunt kunt ophalen uit een store kennis die u maakt.

+ Een pijplijn AI indexeren tijdens het opsporen van stappen en vaardigheden definities verfijnen. Een store kennis ziet u het product van de definitie van een vaardigheden in een pijplijn AI-indexering. De resultaten kunt u een betere vaardigheden ontwerpen omdat ziet u precies wat de enrichments er als volgt uitzien. U kunt [Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) in Azure storage om de inhoud van een winkel kennis weer te geven.

+ De vorm van de gegevens naar nieuwe formulieren. Het omzetten van is overgegaan in kennis en vaardigheden, maar het punt is dat een set vaardigheden nu deze mogelijkheid kan bieden. De [Shaper vaardigheid](cognitive-search-skill-shaper.md) in Azure Search is uitgebreid om te voldoen aan deze taak. Omvormen, kunt u een projectie die met uw beoogde gebruik van de gegevens overeenstemt behoud van de relaties definiëren.

> [!Note]
> Niet bekend bent met AI gebaseerde indexeren met cognitieve Services? Azure Search kan worden geïntegreerd met Cognitive Services Beeld- en taal functies voor het uitpakken en verrijken van gegevens met behulp van optische tekenherkenning (OCR) via afbeeldingsbestanden, herkenning van entity's en sleuteltermextractie uit tekstbestanden, en meer. Zie voor meer informatie, [wat cognitief zoeken is?](cognitive-search-concept-intro.md).

## <a name="create-a-knowledge-store"></a>Maak een winkel kennis

Een store kennis maakt deel uit van de definitie van een vaardigheden. In deze Preview-versie te maken vereist dat de REST-API met behulp van `api-version=2019-05-06-Preview` of de **gegevens importeren** wizard in de portal.

Hiermee geeft u op de volgende JSON een `knowledgeStore`, die deel uitmaakt van een set vaardigheden, die wordt opgeroepen door een indexeerfunctie (niet weergegeven). De specificatie van projecties binnen de `knowledgeStore` wordt bepaald of tabellen of objecten worden gemaakt in Azure storage.

Als u al bekend met AI gebaseerde indexering bent, bepaalt de definitie van de vaardigheden te maken, organisatie, en inhoud van elk verrijkt document.

```json
{
  "name": "my-new-skillset",
  "description": 
  "Example showing knowledgeStore placement, supported in api-version=2019-05-06-Preview. You need at least one skill, most likely a Shaper skill if you are modulating data structures.",
  "skills":
  [
    {
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document/content/phrases/*",
    "inputs": [
        {
        "name": "text",
        "source": "/document/content/phrases/*"
        },
        {
        "name": "sentiment",
        "source": "/document/content/phrases/*/sentiment"
        }
    ],
    "outputs": [
        {
        "name": "output",
        "targetName": "analyzedText"
        }
    ]
    },
  ],
  "cognitiveServices": 
    {
    "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
    "description": "mycogsvcs resource in West US 2",
    "key": "<your key goes here>"
    },
  "knowledgeStore": { 
    "storageConnectionString": "<your connection string goes here>", 
    "projections": [ 
        { 
            "tables": [  
            { "tableName": "Reviews", "generatedKeyName": "ReviewId", "source": "/document/Review" , "sourceContext": null, "inputs": []}, 
            { "tableName": "Sentences", "generatedKeyName": "SentenceId", "source": "/document/Review/Sentences/*", "sourceContext": null, "inputs": []}, 
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/Review/Sentences/*/KeyPhrases", "sourceContext": null, "inputs": []}, 
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/Review/Sentences/*/Entities/*" ,"sourceContext": null, "inputs": []} 

            ], 
            "objects": [ 
                { 
                "storageContainer": "Reviews", 
                "format": "json", 
                "source": "/document/Review", 
                "key": "/document/Review/Id" 
                } 
            ]      
        }    
    ]     
    } 
}
```

## <a name="components-backing-a-knowledge-store"></a>Onderdelen van een back-up een winkel kennis

Als u wilt een winkel kennis maakt, moet u de volgende services en artefacten.

### <a name="1---source-data"></a>1 - brongegevens

De gegevens of documenten die u wilt verrijken moeten bestaan in een Azure-gegevensbron wordt ondersteund door Azure Search-indexeerfuncties: 

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

* [Azure Cosmos DB](search-howto-index-cosmosdb.md)

* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)

[Azure-tabelopslag](search-howto-indexing-azure-tables.md) kan worden gebruikt voor uitgaande gegevens in een store kennis, maar kan niet worden gebruikt als een resource voor inkomende gegevens naar een indexering op basis van AI-pijplijn.

### <a name="2---azure-search-service"></a>2 - azure Search-service

U moet ook een Azure Search-service en de REST-API maken en configureren van objecten die worden gebruikt voor gegevensverrijking. De REST-API voor het maken van een winkel kennis is `api-version=2019-05-06-Preview`.

Azure Search biedt de functie indexeerfunctie en indexeerfuncties worden gebruikt om het hele proces end-to-end, wat resulteert in persistente, verrijkt documenten in Azure storage te stimuleren. Indexeerfuncties gebruiken een gegevensbron, een index en een set vaardigheden - die allemaal vereist zijn voor het maken en invullen van een winkel kennis.

| Object | REST-API | Description |
|--------|----------|-------------|
| gegevensbron | [Gegevensbron maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Een bron voor het identificeren van een externe Azure gegevensbron met de brongegevens die worden gebruikt voor het maken van geavanceerde documenten bieden.  |
| Vaardigheden | [Vaardighedenset maken (api-version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Een resource coördinatie van het gebruik van [ingebouwde vaardigheden](cognitive-search-predefined-skills.md) en [aangepaste cognitieve vaardigheden](cognitive-search-custom-skill-interface.md) gebruikt in een pijplijn verrijking tijdens het indexeren. |
| index | [Index maken](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Een schema aan een Azure Search-index. Velden in de index toewijzen aan velden in de brongegevens of aan velden die tijdens de fase verrijking (bijvoorbeeld een veld voor organisatienamen die zijn gemaakt door herkenning entiteit). |
| indexeerfunctie | [Indexeerfunctie maken (api-version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Een resource voor het definiëren van onderdelen die worden gebruikt tijdens het indexeren: met inbegrip van een gegevensbron, een set vaardigheden, Veldkoppelingen van de bron- en om gegevens tussentijds structuren met doelindex en de index zelf. De indexeerfunctie wordt uitgevoerd, wordt de trigger voor gegevensopname en verrijking. De uitvoer is een search-index op basis van het indexschema, ingevuld met de brongegevens, verrijkt via kennis en vaardigheden.  |

### <a name="3---cognitive-services"></a>3 - cognitive Services

Enrichments opgegeven in een set vaardigheden zijn gebaseerd op de Computer Vision en taal-functies in Cognitive Services. Cognitive Services-functionaliteit wordt gebruikt tijdens het indexeren via uw vaardigheden uit. Een set vaardigheden is een samenstelling van vaardigheden en vaardigheden zijn gebonden aan specifieke functies voor Computer Vision en taal. Voor het integreren van Cognitive Services, zult u [een Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md) naar een set vaardigheden.

### <a name="4---storage-account"></a>4 - storage-account

Azure Search maakt onder uw Azure Storage-account, een Blob-container of tabellen, afhankelijk van hoe u een set vaardigheden configureert. Als uw data is afkomstig uit Azure Blob of Table storage, bent u al ingesteld. Anders moet u een Azure storage-account maken. Tabellen en objecten in Azure-opslag bevatten de verrijkt documenten die zijn gemaakt door de indexing op basis van AI-pijplijn.

Het opslagaccount dat is opgegeven in de vaardigheden. In `api-version=2019-05-06-Preview`, de definitie van een vaardigheden bevat een definitie van de store kennis zodat u gegevens kunt opgeven.

<a name="tools-and-apps"></a>

### <a name="5---access-and-consume"></a>5 - openen en gebruiken

Zodra de enrichments bestaat in storage, een hulpprogramma of de technologie die met Azure Blob verbindt of Table storage kan worden gebruikt om te verkennen, analyseren of de inhoud in beslag nemen. Hieronder volgt een start:

+ [Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) om geavanceerde documentstructuur en de inhoud weer te geven. Houd rekening met deze als de basislijn-hulpprogramma voor het weergeven van inhoud kennis.

+ [Power BI met Power Query](https://support.office.com/article/connect-to-microsoft-azure-blob-storage-power-query-f8165faa-4589-47b1-86b6-7015b330d13e) voor query's in natuurlijke taal, of gebruik de rapportage en analyse-hulpprogramma's hebt u numerieke gegevens.

+ [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) voor het verder bewerken.

+ Azure Search-index voor zoeken in volledige tekst via de inhoud die u hebt geïndexeerd met [cognitief zoeken](cognitive-search-concept-intro.md).

## <a name="document-persistence"></a>Document persistentie

In de storage-account, kunnen de enrichments worden uitgedrukt als tabellen in Azure Table storage of objecten in Azure Blob-opslag. Intrekken dat enrichments, wanneer deze zijn opgeslagen, om gegevens te laden in andere databases en -hulpprogramma's, kunnen worden gebruikt als een bron

+ Tabelopslag is handig als u wilt dat een schema-bewuste weergave van de gegevens in tabelvorm. Als u wilt wijzigen of elementen weer op nieuwe manieren, biedt Table storage de benodigde granulatie.

+ BLOB-opslag maakt een allesomvattende JSON-weergave van elk document. U kunt allebei opslagopties die in één vaardigheden gebruiken om op te halen van een uitgebreid scala aan expressies.

+ Azure Search persistente inhoud in een index. Als uw scenario is het niet-zoekgerelateerde, bijvoorbeeld als het doel is analyse in een ander hulpprogramma, kunt u de index die wordt de pijplijn gemaakt verwijderen. Maar u kunt ook houden van de index en gebruiken van een ingebouwde hulpprogramma zoals [Search Explorer](search-explorer.md) als een derde medium (achter Storage Explorer en uw app analytics) voor interactie met uw inhoud.

Verrijkt documenten bevatten, samen met inhoud van het document, de metagegevens van de versie van de vaardigheden die de enrichments geproduceerd.  

## <a name="inside-a-knowledge-store"></a>In een store kennis

De store kennis bestaat uit de cache van een aantekening en projecties. De *cache* wordt gebruikt door de service intern de cache opslaan van de resultaten van vaardigheden en het bijhouden van wijzigingen. Een *projectie* definieert het schema en de structuur van de enrichments die overeenkomen met het bedoelde gebruik. Er is een cache per kennis store, maar meerdere projecties. 

De cache is altijd een blob-container, maar projecties kunnen worden gelede tabellen of-objecten:

+ Als een object, de projectie wijst naar blobopslag, waar de projectie is opgeslagen naar een container, waarin de objecten of hiërarchische weergaven in de JSON voor scenario's zoals een data science-pijplijn zijn.

+ Als een tabel wijst de projectie naar Table storage. Een weergave in tabelvorm bewaart relaties voor scenario's zoals het analyseren van gegevens of exporteren als gegevensframes voor machine learning. De verrijkt projecties kunnen vervolgens worden eenvoudig worden geïmporteerd in andere gegevensarchieven. 

U kunt meerdere projecties maken in een winkel kennis om verschillende constituencies in uw organisatie mogelijk te maken. Een ontwikkelaar mogelijk toegang tot de volledige JSON-weergave van een geavanceerde document, terwijl de datawetenschappers of analisten kunt gedetailleerde of modulaire gegevensstructuren gevormde door uw vaardigheden uit.

Bijvoorbeeld, als een van de doelstellingen van het proces verrijking ook maken een gegevensset die wordt gebruikt voor een model te trainen, is projectie van de gegevens in de store van het object een manier om de gegevens in uw data science-pijplijnen gebruiken. U kunt ook als u wilt maken van een snelle Power BI-dashboard op basis van de geavanceerde documenten werkt de tabellaire projectie goed.

<!---
## Data lifecycle and billing

Each time you run the indexer, the cache in Azure storage is updated if the skillset definition or underlying source data has changed. As input documents are edited or deleted, changes are propagated through the annotation cache to the projections, ensuring that your projected data is a current representation of your inputs at the end of the indexer run. 

Generally speaking, pipeline processing can be an all-or-nothing operation, but Azure Search can process incremental changes, which saves you time and money.

If a document is new or updated, all skills are run. If only the skillset changes, reprocessing is scoped to just those skills and documents affected by your edit.

### Changes to a skillset
Suppose that you have a pipeline composed of multiple skills, operating over a large body of static data (for example, scanned documents), that takes 8 hours and costs $200 to create the knowledge store. Now suppose you need to tweak one of the skills in the skillset. Rather than starting over, Azure Search can determine which skill is affected, and reprocess only that skill. Cached data and projections that are unaffected by the change remain intact in the knowledge store.

### Changes in the data
Scenarios can vary considerably, but let's suppose instead of static data, you have volatile data that changes between indexer invocations. Given no changes to the skillset, you are charged for processing the delta of new and modified document. The timestamp information varies by data source, but for illustration, in a Blob container, Azure Search looks at the `lastmodified` date to determine which blobs need to be ingested.

> [!Note]
> While you can edit the data in the projections, any edits will be overwritten on the next pipeline invocation, assuming the document in source data is updated. 

### Deletions

Although Azure Search creates and updates structures and content in Azure storage, it does not delete them. Projections and cached documents continue to exist even when the skillset is deleted. As the owner of the storage account, you should delete a projection if it is no longer needed. 

### Tips for development

+ Start small with a representative sample of your data as you make significant changes to skillset composition. As your design finalizes, you can slowly add more data during later-stage development, and then roll in the entire data set when you are comfortable with the pipeline composition.

+ Retain control over indexer invocation. Indexers can run on a schedule, which is helpful for solutions that are rolled into production, but less helpful if you are actively developing your pipeline. During development, avoid schedules so that you don’t lose track of cache or projection state. Once your solution is in production and skillset composition is static, you can put the indexer on a schedule to pick up routine changes in the external source data. 

-->

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

We raden aan de gratis service voor educatieve doeleinden, maar houd er rekening mee dat het aantal gratis transacties beperkt tot 20 documenten per dag, per abonnement is.

Wanneer u meerdere services gebruikt, maakt u al uw services van in dezelfde regio voor de beste prestaties en om kosten te minimaliseren. U betaalt geen voor de bandbreedte voor binnenkomende of uitgaande gegevens die u naar een andere service in dezelfde regio gaat.

**Stap 1: [Maak een Azure Search-resource](search-create-service-portal.md)** 

**Stap 2: [Een Azure storage-account maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)** 

**Stap 3: [Een Cognitive Services-resource maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)** 

**Stap 4: [Aan de slag met de portal](cognitive-search-quickstart-blob.md) - of - [aan de slag met voorbeeldgegevens met behulp van REST en Postman](knowledge-store-howto.md)** 

U kunt REST `api-version=2019-05-06-Preview` te maken van een op basis van AI-pijplijn die kennis Store bevat. De nieuwste preview-API, het object vaardigheden beschikt u over de `knowledgeStore` definitie.

## <a name="takeaways"></a>Opgedane kennis

Kennis Store biedt tal van voordelen zoals, maar niet beperkt tot het gebruik van de geavanceerde documenten in scenario's dan zoeken inschakelen, kosten van besturingselementen en drift in uw proces verrijking beheren. Deze functies zijn allemaal beschikbaar voor gebruik door een storage-account toe te voegen aan uw vaardigheden en het gebruik van de bijgewerkte expressietaal, zoals beschreven in [aan de slag met kennis Store](knowledge-store-howto.md). 

## <a name="next-steps"></a>Volgende stappen

De eenvoudigste manier voor het maken van geavanceerde documenten, is via de **gegevens importeren** wizard.

> [!div class="nextstepaction"]
> [Snelstart: Probeer cognitief zoeken in een kennismaken met de portal](cognitive-search-quickstart-blob.md)
