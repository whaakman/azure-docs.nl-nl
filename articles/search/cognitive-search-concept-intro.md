---
title: Cognitieve zoeken voor het ophalen van gegevens, natuurlijke taal worden verwerkt in Azure Search | Microsoft Docs
description: Ophalen van gegevens, verwerking van natuurlijke taal (NLP) en beeldverwerking doorzoekbare om inhoud te maken in Azure Search indexeren cognitieve vaardigheden gebruiken.
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: ca6c285348208a7ad24faf966073d641810039fc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641108"
---
# <a name="what-is-cognitive-search"></a>Wat is cognitieve zoeken?

Cognitieve search is een preview-functie van [Azure Search](search-what-is-azure-search.md), beschikbaar is op alle lagen in Zuid-centraal VS en West-Europa, waarmee AI wordt toegevoegd aan indexering werkbelastingen. Ophalen van gegevens, verwerking van natuurlijke taal en beeldverwerking tijdens het indexeren de latente informatie gevonden in niet-gestructureerde of niet-doorzoekbare inhoud en maakt het doorzoekbaar in Azure Search.

AI-integratie is via *cognitieve vaardigheden* die brondocumenten via sequentiële processen, in de route naar een zoekindex aanvullen. 

![Diagram van de pijplijn cognitieve zoeken](./media/cognitive-search-intro/cogsearch-architecture.png "cognitieve zoeken pipeline-overzicht")

Kwalificaties gebruikt tijdens het indexeren zijn vooraf gedefinieerde of aangepaste:

+ [Prefined vaardigheden](cognitive-search-predefined-skills.md) zijn gebaseerd op dezelfde AI algoritmen in cognitieve Services-API's gebruikt: [met de naam entiteit erkenning](cognitive-search-skill-named-entity-recognition.md), [uitpakken van sleutel woordgroep](cognitive-search-skill-keyphrases.md), en [OCR](cognitive-search-skill-ocr.md) zijn een paar. 

+ [Aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md) kunnen zijn ontwikkeld door u voor alle gespecialiseerde bewerkingen die u nodig hebt. Voorbeelden van aangepaste vaardigheden mogelijk een aangepaste entiteit module of document classificatie die gericht is op een specifiek domein zoals Financiën, wetenschappelijke publicaties of arts.

> [!NOTE]
> Cognitieve zoekopdrachten bevindt zich in de openbare preview-fase en momenteel wordt uitvoering van vaardigheden gratis aangeboden. De prijzen voor deze functionaliteit worden op een later moment bekend gemaakt.

## <a name="components-of-cognitive-search"></a>Onderdelen van cognitieve zoeken

De pijplijn cognitieve zoeken is gebaseerd op [Azure Search *indexeerfuncties* ](search-indexer-overview.md) die gegevensbronnen verkennen en verwerking van de end-to-end-index. Vaardigheden nu zijn gekoppeld aan indexeerfuncties, onderscheppen en uit te breiden documenten volgens de vaardigheden die u definieert. Zodra geïndexeerd, u kunt toegang tot inhoud via de search-aanvragen via alle [querytypen die worden ondersteund door Azure Search](search-query-overview.md).  Als u niet bekend met indexeerfuncties bent, helpt in deze sectie u de stappen doorlopen.

### <a name="source-data-and-document-cracking-phase"></a>Brongegevens en document kraken fase

Aan het begin van de pijplijn die u hebt niet-gestructureerde tekst of niet-tekstwaarden inhoud (zoals de installatiekopie en gescande documenten JPEG-bestanden). Gegevens moet in een Azure data storage-service die kan worden geopend door een indexeerfunctie bestaan. Indexeerfuncties kunnen 'crack' brondocumenten tekst ophalen uit de brongegevens.

![Document kraken fase](./media/cognitive-search-intro/document-cracking-phase-blowup.png "document kraken")

 Ondersteunde bronnen bevatten Azure blob-opslag, Azure table storage, Azure SQL Database en Azure Cosmos DB. Inhoud op basis van tekst kan worden geëxtraheerd uit de volgende bestandstypen: PDF, Word, PowerPoint, CSV-bestanden. Zie voor een volledige lijst [ondersteunde indelingen](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Cognitieve vaardigheden en verrijking fase

Verrijking is via *cognitieve vaardigheden* atomische bewerkingen worden uitgevoerd. Bijvoorbeeld, wanneer u tekstinhoud van een PDF-bestand hebt, kunt u toepassen entiteit erkenning taal wordt gedetecteerd of uitpakken van sleutel wachtwoordzin voor het produceren van nieuwe velden in de index die niet beschikbaar systeemeigen in de bron. Volledig, de verzameling met de vaardigheden die worden gebruikt in de pipeline heet een *vaardigheden*.  

![Verrijking fase](./media/cognitive-search-intro/enrichment-phase-blowup.png "verrijking fase")

Een vaardigheden is gebaseerd op [vooraf gedefinieerde cognitieve vaardigheden](cognitive-search-predefined-skills.md) of [aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md) u bieden en maak verbinding met de vaardigheden. Een vaardigheden kan minimaal of zeer complexe en niet alleen het type van de verwerking, maar ook de volgorde van bewerkingen bepaalt. Een vaardigheden plus de veldtoewijzingen gedefinieerd als onderdeel van een indexeerfunctie volledig Hiermee geeft u de pijplijn verrijking. Zie voor meer informatie over het binnenhalen van alle onderdelen samen [definiëren een vaardigheden](cognitive-search-defining-skillset.md).

De pijplijn genereert intern, een verzameling van verrijkt documenten. U kunt bepalen welke onderdelen van de verrijkt documenten moeten worden toegewezen aan worden geïndexeerd velden in uw search-index. Als u de belangrijkste zinnen extractie en de vaardigheden die entiteit erkenning toegepast, klikt u vervolgens deze nieuwe velden zou deel uitmaken van het document verrijkt en ze kunnen worden toegewezen aan de velden op uw index. Zie [aantekeningen](cognitive-search-concept-annotations-syntax.md) voor meer informatie over i/o-formaties.

### <a name="search-index-and-query-based-access"></a>Search-index en query's gebaseerde toegang

Tijdens de verwerking is voltooid, hebt u een zoekopdracht corpus die bestaan uit verrijkt documenten, tekst-zoekacties in Azure Search. [Query's in de index](search-query-overview.md) is hoe ontwikkelaars en gebruikers toegang hebben tot de verrijkt inhoud die is gegenereerd door de pijplijn. 

![De index met zoekpictogram](./media/cognitive-search-intro/search-phase-blowup.png "Index met zoekpictogram")

De index is, net als andere u voor Azure Search maken mogelijk: vullen met aangepaste analyzers, fuzzy zoekquery's aanroepen, gefilterde zoekopdracht toevoegen of experimenteren met score berekenen voor profielen voor het wijzigen van de zoekresultaten.

Indexen zijn gegenereerd op basis van een indexschema dat de velden, kenmerken definieert, en andere constructies die zijn gekoppeld aan een specifieke index, zoals scoren van profielen en synoniem wordt toegewezen. Als een index is gedefinieerd en ingevuld, kunt u stapsgewijs indexeren zodat nieuwe en bijgewerkte brondocumenten worden opgepikt. Bepaalde wijzigingen vereist helemaal opnieuw maken. U moet een kleine set gegevens gebruiken totdat het schemaontwerp voor een stabiel is. Zie [How to rebuild an index](search-howto-reindex.md) (Een index herbouwen) voor meer informatie.

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Belangrijke functies en concepten

| Concept | Beschrijving| Koppelingen |
|---------|------------|-------|
| Vaardigheden | Een op het hoogste niveau met een verzameling van vaardigheden resource met de naam. Een vaardigheden is de pijplijn verrijking. Deze wordt aangeroepen tijdens het indexeren door een indexeerfunctie. | [Een vaardigheden definiëren](cognitive-search-defining-skillset.md) |
| Cognitieve kwalificatie | Een atomic-transformatie in een pijplijn verrijking. Het is vaak het geval is, een onderdeel dat wordt opgehaald of infereert structuur en daarom uw kennis van de invoergegevens verbetert. Bijna altijd de uitvoer is op basis van tekst en de verwerking is natuurlijke taal verwerking of afbeelding die wordt uitgepakt of tekst wordt gegenereerd van de invoer van de installatiekopie. De uitvoer van een kwalificatie worden toegewezen aan een veld in een index of voor een downstream verrijking worden gebruikt als invoer. Een kwalificatie is vooraf gedefinieerd en geleverd door Microsoft of aangepast: gemaakt en geïmplementeerd door u. | [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) |
| Ophalen van gegevens | Omvat een breed scala aan verwerking, maar met betrekking tot cognitieve zoekopdracht op de benoemde entiteit erkenning kwalificatie doorgaans gebruikt wordt om gegevens te extraheren (een entiteit) vanuit een bron die deze informatie geen systeemeigen biedt. | [Benoemde entiteit erkenning kwalificatie](cognitive-search-skill-named-entity-recognition.md)| 
| Verwerking van installatiekopieën | Tekst van een installatiekopie, zoals de mogelijkheid voor het herkennen van een kenmerkende infereert of haalt tekst op basis van een installatiekopie. Algemene voorbeelden zijn OCR voor tekens uit een bestand gescande documenten (JPEG) op te heffen of herkent de naam van een straat in een foto met een straat teken. | [Afbeelding Analysis kwalificatie](cognitive-search-skill-image-analysis.md) of [OCR kwalificatie](cognitive-search-skill-ocr.md)
| Natuurlijke taalverwerking | Tekst voor inzichten en informatie over tekst invoer verwerken. Taal wordt gedetecteerd, gevoel analyse en uitpakken van sleutel woordgroep zijn vaardigheden die onder de verwerking van natuurlijke taal vallen.  | [Sleutel woordgroep extractie kwalificatie](cognitive-search-skill-keyphrases.md), [taal detectie kwalificatie](cognitive-search-skill-language-detection.md), [gevoel Analysis kwalificatie](cognitive-search-skill-sentiment.md) |
| Document kraken | Het proces van het uitpakken of tekstinhoud van niet-tekstwaarden bronnen maken tijdens het indexeren. (Optische tekenherkenning) is een voorbeeld, maar doorgaans deze verwijst naar indexeerfunctie kernfunctionaliteit zoals de indexeerfunctie inhoud uit de toepassingsbestanden haalt. De gegevensbron met bronbestandslocatie en de definitie van de indexeerfunctie die veld-verwijzingen zijn beide belangrijke factoren bij het document kraken. | Zie [indexeerfuncties](search-indexer-overview.md) |
| Vormgeven | Tekstfragmenten consolideren in een groter geheel, of omgekeerd uitgesplitst grotere tekstsegmenten in een beheersbare grootte voor verdere downstreamverwerking. | [Kwalificatie shaper](cognitive-search-skill-shaper.md), [tekst fusie kwalificatie](cognitive-search-skill-textmerger.md), [tekst kwalificatie splitsen](cognitive-search-skill-textsplit.md) |
| Verrijkt documenten | Een probleem van voorbijgaande interne structuur, niet rechtstreeks toegankelijk zijn in de code. Verrijkt documenten zijn gegenereerd tijdens de verwerking, maar alleen uiteindelijke uitvoer worden doorgevoerd in een search-index. Veldtoewijzingen bepalen welke gegevens worden toegevoegd aan de index. | Zie [verrijkt documenten openen](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Indexer |  Een Verkenner die doorzoekbare gegevens en metagegevens uit een externe gegevensbron haalt en een index op basis van veld naar toewijzingen tussen de index en de gegevensbron voor document kraken gevuld. Voor enrichments cognitieve zoeken, de indexeerfunctie een vaardigheden aanroept en het veldtoewijzingen koppelen verrijking uitvoer naar de doelvelden in de index bevat. De definitie van de indexeerfunctie bevat alle instructies en verwijzingen voor pipeline-bewerkingen, en de pijplijn wordt aangeroepen wanneer de indexeerfunctie worden uitgevoerd. | [Indexeerfuncties](search-indexer-overview.md) |
| Gegevensbron  | Een object dat wordt gebruikt door een indexeerfunctie verbinding maken met een externe gegevensbron van de ondersteunde typen in Azure. | Zie [indexeerfuncties](search-indexer-overview.md) |
| Index | Een corpus persistente zoeken in Azure Search gebouwd op basis van een indexschema dat veld structuur en het gebruik definieert. | [Indexen in Azure Search](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Waar moet ik beginnen?

**Stap 1: Maak een zoekservice in een regio die de API 's** 

+ Zuid-centraal VS
+ West-Europa

**Stap 2: Praktische ervaring naar de hoofdsleutel van de werkstroom**

+ [Quick Start (portal)](cognitive-search-quickstart-blob.md)
+ [Zelfstudie (HTTP-aanvragen)](cognitive-search-tutorial-blob.md)
+ [Voorbeeld van de aangepaste vaardigheden (C#)](cognitive-search-create-custom-skill-example.md)

**Stap 3: Controleer de API (alleen REST)**

Op dit moment worden alleen REST-API's geleverd. Gebruik `api-version=2017-11-11-Preview` op alle aanvragen. Gebruik de volgende API's voor het bouwen van een zoekoplossing cognitieve. Slechts twee API's worden toegevoegd of uitgebreid voor cognitieve zoeken. Andere API's hebben dezelfde syntaxis als de algemeen beschikbaar versies.

| REST-API | Beschrijving |
|-----|-------------|
| [Gegevensbron maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Een bron te identificeren van een externe gegevensbron brongegevens gebruikt voor het maken van verrijkt documenten bieden.  |
| [Vaardigheden maken (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Een resource coördinatie van het gebruik van [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) en [aangepaste cognitieve vaardigheden](cognitive-search-custom-skill-interface.md) gebruikt in een pijplijn verrijking tijdens het indexeren. |
| [Index maken](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Een schema uitdrukken van een Azure Search-index. Velden in de index aan velden in de brongegevens of velden geproduceerd tijdens de fase verrijking (bijvoorbeeld een veld voor de organisatienamen van de gemaakt door entiteit erkenning) toewijzen. |
| [Maak indexeerfunctie (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Een bron voor het definiëren van onderdelen die worden gebruikt tijdens het indexeren: met inbegrip van een gegevensbron, een vaardigheden veld koppelingen van de bron- en tussenliggende gegevensstructuren met doelindex en de index zelf. De indexeerfunctie wordt uitgevoerd, is de trigger voor gegevensopname en verrijking. De uitvoer is een zoekopdracht corpus op basis van het indexschema dat is gevuld met de brongegevens, verrijkt via vaardigheden.  |

**Controlelijst: Een gangbare werkstroom**

1. Subset uw Azure brongegevens in een representatieve steekproef. Indexeren van tijd vergt dus beginnen met een kleine, representatieve gegevensset en dit vervolgens incrementeel naarmate uw oplossing meer vormt krijgt.

1. Maak een [gegevensbronobject](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in Azure Search, een verbindingsreeks opgeven voor ophalen van gegevens.

1. Maak een [vaardigheden](https://docs.microsoft.com/rest/api/searchservice/create-skillset) met verrijking stappen.

1. Definieer de [indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index). De *velden* verzameling bevat velden uit de brongegevens. U moet ook aanvullende velden voor het opslaan van de gegenereerde waarden voor inhoud die is gemaakt tijdens de verrijking stub.

1. Definieer de [indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-skillset) verwijst naar de gegevensbron, de vaardigheden en de index.

1. Voeg binnen de indexeerfunctie *outputFieldMappings*. Deze sectie wordt de uitvoer van de vaardigheden (in stap 3) aan de velden invoeren in het schema van de index (in stap 4) toegewezen.

1. Verzenden *indexeerfunctie maken* vragen u zojuist hebt (een POST-aanvraag met een definitie van een indexeerfunctie in de aanvraagtekst) gemaakt voor de indexeerfunctie in Azure Search express. Deze stap is hoe u de indexeerfunctie uitvoeren, de pijplijn wordt aangeroepen.

1. Query's voor het evalueren van de resultaten en wijzigen van de code voor de update vaardigheden, schema of de configuratie van de indexeerfunctie worden uitgevoerd.

1. [Opnieuw instellen van de indexeerfunctie](search-howto-reindex.md) voordat het opnieuw opbouwen van de pijplijn.

Zie voor meer informatie over specifieke vragen of problemen [tips voor probleemoplossing](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Volgende stappen

+ [Documentatie cognitieve zoeken](cognitive-search-resources-documentation.md)
+ [Snelstartgids: Probeer cognitieve zoeken in een overzicht van de portal](cognitive-search-quickstart-blob.md)
+ [Zelfstudie: Meer informatie over het cognitieve zoeken-API 's](cognitive-search-tutorial-blob.md)