---
title: Cognitieve zoekopdrachten voor het ophalen van gegevens, verwerking van natuurlijke taal AI in Azure Search | Microsoft Docs
description: Uitpakken van inhoud, (NLP) voor de verwerking van natuurlijke taal en afbeeldingsverwerking activeren voor doorzoekbare inhoud maken in Azure Search indexeren met cognitieve vaardigheden en AI-algoritmen
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: heidist
ms.openlocfilehash: 68d546fc4c853f1a19230b8aee7e86519aaa7e4c
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729031"
---
# <a name="what-is-cognitive-search"></a>Wat is cognitive search?

Cognitief zoeken maakt doorzoekbare gegevens buiten niet-doorzoekbare inhoud samenvoegt met AI-algoritmen te koppelen aan een pijplijn voor indexering. AI-integratie is via *cognitieve vaardigheden*, brondocumenten onderweg naar een search-index uit te breiden. 

**Verwerking van natuurlijke taal** vaardigheden opnemen [entiteit erkenning](cognitive-search-skill-named-entity-recognition.md), taaldetectie, [sleutel vindt er sleuteltermextractie plaats](cognitive-search-skill-keyphrases.md), tekstbewerkingen en detectie van gevoelens. Met deze kennis van wordt ongestructureerde tekst gestructureerd, wordt toegewezen aan doorzoekbare en filterbare velden in een index.

**Verwerking van afbeeldingen** bevat [OCR](cognitive-search-skill-ocr.md) en identificatie van [visuele kenmerken](cognitive-search-skill-image-analysis.md), zoals gezichtsdetectie, afbeelding interpretatie, image recognition (beroemdheden en oriëntatiepunten) of kenmerken zoals kleuren of afdrukstand. U kunt tekst-representaties van afbeeldingsinhoud, kan worden doorzocht met behulp van de querymogelijkheden van Azure Search maken.

![Overzichtsdiagram van gegevenspijplijn cognitief zoeken](./media/cognitive-search-intro/cogsearch-architecture.png "overzicht van de pijplijn Cognitief zoeken")

De cognitieve vaardigheden in Azure Search zijn gebaseerd op de dezelfde AI-algoritmen in Cognitive Services API's gebruikt: [entiteit Recognition-API met de naam](cognitive-search-skill-named-entity-recognition.md), [API van Key woordgroep extractie](cognitive-search-skill-keyphrases.md), en [OCR API](cognitive-search-skill-ocr.md) zijn een paar. 

Natuurlijke taal en verwerking van afbeeldingen wordt toegepast tijdens de fase van de gegevensopname met steeds deel van de samenstelling van een document in een doorzoekbare index in Azure Search resultaten. Gegevens worden als een Azure data source en vervolgens via een indexing-pijplijn met behulp van afhankelijk van wat wordt gepusht [ingebouwde vaardigheden](cognitive-search-predefined-skills.md) u nodig hebt. De architectuur worden uitgebreid, zodat als de ingebouwde vaardigheden niet voldoende, kunt u maken en koppelen [aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md) aangepaste verwerking te integreren. Voorbeelden zijn een aangepaste entiteit module of document classificatie die gericht is op een specifiek domein, zoals Financiën, wetenschappelijke publicaties of diergeneeskunde.

> [!NOTE]
> Cognitief zoeken is een openbare preview. Uitvoering van vaardigheden en uitpakken van de installatiekopie en normalisering worden momenteel gratis aangeboden. Op een later tijdstip, worden de prijzen van deze mogelijkheden aangekondigd. 

## <a name="components-of-cognitive-search"></a>Onderdelen van cognitief zoeken

Cognitive search is een preview-functie van [Azure Search](search-what-is-azure-search.md), beschikbaar op alle lagen in Zuid-centraal VS en West-Europa. 

De pijplijn cognitief zoeken is gebaseerd op [Azure Search *indexeerfuncties* ](search-indexer-overview.md) die gegevensbronnen verkennen en verwerking van de end-to-end-index. Vaardigheden zijn nu gekoppeld aan indexeerfuncties, onderscheppen en verrijkende documenten op basis van de vaardigheden die u definieert. Nadat geïndexeerd, u toegang tot inhoud via search-aanvragen via alle [query's uitvoeren die worden ondersteund door Azure Search](search-query-overview.md).  Als u niet bekend bent met indexeerfuncties, doorloopt in deze sectie u de stappen.

### <a name="source-data-and-document-cracking-phase"></a>Brongegevens en het document kraken fase

Aan het begin van de pijplijn, beschikt u over ongestructureerde tekst of niet-tekstuele inhoud (zoals afbeeldingen en gescande documenten JPEG-bestanden). Gegevens moeten zich in een Azure data storage-service die kan worden geopend door een indexeerfunctie. Indexeerfuncties kunnen 'gekraakt' brondocumenten om uit te pakken tekst van de brongegevens.

![Document kraken fase](./media/cognitive-search-intro/document-cracking-phase-blowup.png "documenten kraken")

 Ondersteunde bronnen omvatten Azure blob-opslag, Azure table storage, Azure SQL Database en Azure Cosmos DB. Inhoud op basis van tekst kan worden geëxtraheerd uit de volgende bestandstypen: PDF-bestanden, Word, PowerPoint, CSV-bestanden. Zie voor een volledige lijst [ondersteunde indelingen](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Cognitieve vaardigheden en verrijking fase

Verrijking is via *cognitieve vaardigheden* atomische bewerkingen uitvoeren. Bijvoorbeeld, wanneer u de tekstinhoud van een PDF-bestand hebt, kunt u toepassen entiteit erkenning taaldetectie of sleutel vindt er sleuteltermextractie plaats voor het produceren van nieuwe velden in de index die niet beschikbaar systeemeigen in de bron zijn. Kan worden overgeslagen, de verzameling van vaardigheden die wordt gebruikt in de pijplijn heet een *vaardigheden*.  

![Verrijking fase](./media/cognitive-search-intro/enrichment-phase-blowup.png "verrijking fase")

Een set vaardigheden is gebaseerd op [vooraf gedefinieerde cognitieve vaardigheden](cognitive-search-predefined-skills.md) of [aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md) u opgeeft en verbinding maken met de vaardigheden. Een set vaardigheden kan minimale of zeer complex zijn, en bepaalt niet alleen het type van de verwerking, maar ook de volgorde van bewerkingen. Een set vaardigheden plus de veldtoewijzingen gedefinieerd als onderdeel van een indexeerfunctie volledig Hiermee geeft u de pijplijn verrijking. Zie voor meer informatie over het binnenhalen van al deze onderdelen samen, [een set vaardigheden definiëren](cognitive-search-defining-skillset.md).

Intern, genereert de pijplijn een verzameling verrijkt documenten. U kunt bepalen welke onderdelen van de geavanceerde documenten moeten worden toegewezen aan indexeerbare velden in uw search-index. Als u de extractie van sleuteluitdrukkingen en de vaardigheden die entiteit erkenning toegepast, klikt u vervolgens die nieuwe velden deel van de geavanceerde document zou worden en ze kunnen worden toegewezen aan de velden in uw index. Zie [aantekeningen](cognitive-search-concept-annotations-syntax.md) voor meer informatie over i/o-ontstaan wachtrijen.

### <a name="search-index-and-query-based-access"></a>Search-index en query's gebaseerde toegang

Wanneer de verwerking is voltooid, hebt u een zoekverzameling die bestaan uit geavanceerde documenten, tekst-zoekacties in Azure Search. [Query's in de index](search-query-overview.md) is hoe ontwikkelaars en gebruikers toegang de verrijkt inhoud worden gegenereerd door de pijplijn tot. 

![Index met zoekpictogram](./media/cognitive-search-intro/search-phase-blowup.png "Index met zoekpictogram")

De index is, net als andere u voor Azure Search maken kunt: aanvullen met aangepaste analyse, fuzzy zoekopdrachten aanroepen, gefilterde zoekfunctionaliteit toevoegen of experimenteren met scoreprofielen om te zetten om de lijst met zoekresultaten.

Indexen zijn gegenereerd op basis van een indexschema waarin de velden, kenmerken, en andere constructies die is gekoppeld aan een specifieke index, zoals scoreprofielen en synoniem wordt toegewezen. Zodra een index is gedefinieerd en is ingevuld, kunt u Incrementeel indexeren om te pikken nieuwe en bijgewerkte brondocumenten. Bepaalde wijzigingen moeten een volledig opnieuw is gebouwd. Totdat de schemaontwerp stabiel is, moet u een kleine gegevensset gebruiken. Zie [How to rebuild an index](search-howto-reindex.md) (Een index herbouwen) voor meer informatie.

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Belangrijke functies en concepten

| Concept | Beschrijving| Koppelingen |
|---------|------------|-------|
| Vaardigheden | Een op het hoogste niveau met de naam resource met een verzameling van vaardigheden. Een set vaardigheden is de pijplijn verrijking. Deze wordt aangeroepen tijdens het indexeren van een indexeerfunctie. | [Een set vaardigheden definiëren](cognitive-search-defining-skillset.md) |
| Cognitieve vaardigheden | Een atomische transformatie in een pijplijn verrijking. Het is vaak het geval is, een onderdeel dat wordt opgehaald of bepaalt welke structuur en worden daarom verbetert uw kennis van de ingevoerde gegevens. Bijna altijd de uitvoer is op basis van tekst en de verwerking is verwerking van natuurlijke taal of verwerking van afbeeldingen die worden geëxtraheerd of tekst van de invoer van de afbeelding genereert. Uitvoer van een kwalificatie kan worden toegewezen aan een veld in een index of gebruikt als invoer voor een downstream verrijking. Een kwalificatie is vooraf gedefinieerd en geleverd door Microsoft of aangepast: gemaakt en geïmplementeerd door u. | [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) |
| Ophalen van gegevens | Omvat een breed scala aan verwerking, maar die betrekking hebben op cognitief zoeken, de kwalificatie van de herkenning van benoemde entiteiten doorgaans gebruikt wordt om gegevens te extraheren (een entiteit) van een bron die deze informatie wordt niet systeemeigen bieden. | [Vraagstukken voor herkenning van named Entity](cognitive-search-skill-named-entity-recognition.md)| 
| Verwerking van installatiekopieën | Bepaalt welke tekst van een installatiekopie, zoals de mogelijkheid een oriëntatiepunten herkennen of haalt tekst op basis van een installatiekopie. Algemene voorbeelden zijn OCR voor tekens uit een bestand gescande documenten (JPEG) op te heffen of herkennen van een straatnaam in een foto met een straat teken. | [Afbeelding Analysis vaardigheid](cognitive-search-skill-image-analysis.md) of [OCR-kwalificatie](cognitive-search-skill-ocr.md)
| Natuurlijke taalverwerking | Tekst te verwerken voor inzichten en informatie over teksten. Taaldetectie en sentimentanalyse, sleuteltermextractie zijn vaardigheden die onder de verwerking van natuurlijke taal vallen.  | [Woordgroep extractie kwalificatie Key](cognitive-search-skill-keyphrases.md), [taal detectie vaardigheid](cognitive-search-skill-language-detection.md), [Sentiment-analyse vaardigheid](cognitive-search-skill-sentiment.md) |
| Document kraken | Het proces van het uitpakken of het maken van de tekstinhoud van niet-tekstuele bronnen tijdens het indexeren. Optische tekenherkenning (OCR) is een voorbeeld, maar in het algemeen deze gegevensset verwijst naar indexeerfunctie Kernfunctionaliteit als de indexeerfunctie inhoud uit toepassingsbestanden haalt. De gegevensbron met de locatie van bronbestand en de definitie van de indexeerfunctie veldtoewijzingen, bieden zijn beide belangrijke factoren in document kraken. | Zie [indexeerfuncties](search-indexer-overview.md) |
| Vormgeven | Tekstfragmenten consolideren in een groter geheel, of omgekeerd opdelen grotere tekstsegmenten in een handelbare hoeveelheid voor verdere downstream-verwerkingen. | [Shaper vaardigheid](cognitive-search-skill-shaper.md), [tekst fusie vaardigheid](cognitive-search-skill-textmerger.md), [tekst bekwaamheid splitsen](cognitive-search-skill-textsplit.md) |
| Verrijkt documenten | Een tijdelijke interne structuur, niet rechtstreeks toegankelijk zijn in de code. Verrijkt documenten zijn gegenereerd tijdens de verwerking, maar alleen uiteindelijke uitvoer zijn opgeslagen in een search-index. Veldtoewijzingen te bepalen welke gegevens worden toegevoegd aan de index. | Zie [toegang tot geavanceerde documenten](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Indexer |  Een Verkenner die doorzoekbare gegevens en metagegevens geëxtraheerd uit een externe gegevensbron en een index op basis van veld naar toewijzingen tussen de index en de gegevensbron voor documenten kraken vult. Voor cognitief zoeken enrichments, de indexeerfunctie roept een set vaardigheden en de veldtoewijzingen koppelen verrijking uitvoer naar de doelvelden in de index bevat. Definitie van de indexeerfunctie bevat alle instructies en verwijzingen voor bewerkingen van de pijplijn en de pijplijn wordt aangeroepen wanneer u de indexeerfunctie uitvoeren. | [Indexeerfuncties](search-indexer-overview.md) |
| Gegevensbron  | Een object dat wordt gebruikt door een indexeerfunctie om te verbinden met een externe gegevensbron van de ondersteunde typen in Azure. | Zie [indexeerfuncties](search-indexer-overview.md) |
| Index | Een permanente zoekverzameling in Azure Search, gebouwd op basis van een indexschema dat veld structuur en het gebruik definieert. | [Indexen in Azure Search](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Waar moet ik beginnen?

**Stap 1: Een service voor zoeken in een regio die de API's maken** 

+ US - zuid-centraal
+ Europa -west

**Stap 2: Praktische ervaring naar het hoofdniveau van de werkstroom**

+ [Quick Start (portal)](cognitive-search-quickstart-blob.md)
+ [Zelfstudie (HTTP-aanvragen)](cognitive-search-tutorial-blob.md)
+ [Voorbeeld van de aangepaste vaardigheden (C#)](cognitive-search-create-custom-skill-example.md)

**Stap 3: Controleer de API (alleen REST)**

Op dit moment worden alleen REST-API's geleverd. Gebruik `api-version=2017-11-11-Preview` op alle aanvragen. Gebruik de volgende API's om een oplossing voor cognitief zoeken. Slechts twee API's worden toegevoegd of uitgebreid voor cognitief zoeken. Andere API's hebben dezelfde syntaxis als de algemeen beschikbare versie.

| REST-API | Beschrijving |
|-----|-------------|
| [Gegevensbron maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Een bron voor het identificeren van een externe gegevensbron brongegevens gebruikt voor het maken van geavanceerde documenten bieden.  |
| [Vaardighedenset maken (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Een resource coördinatie van het gebruik van [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) en [aangepaste cognitieve vaardigheden](cognitive-search-custom-skill-interface.md) gebruikt in een pijplijn verrijking tijdens het indexeren. |
| [Index maken](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Een schema aan een Azure Search-index. Velden in de index toewijzen aan velden in de brongegevens of aan velden die tijdens de fase verrijking (bijvoorbeeld een veld voor organisatienamen die zijn gemaakt door herkenning entiteit). |
| [Indexeerfunctie maken (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Een resource voor het definiëren van onderdelen die worden gebruikt tijdens het indexeren: met inbegrip van een gegevensbron, een set vaardigheden, Veldkoppelingen van de bron- en om gegevens tussentijds structuren met doelindex en de index zelf. De indexeerfunctie wordt uitgevoerd, wordt de trigger voor gegevensopname en verrijking. De uitvoer is een zoekverzameling op basis van het indexschema, ingevuld met de brongegevens, verrijkt via kennis en vaardigheden.  |

**Controlelijst: Een werkstroom**

1. Deel uw Azure-brongegevens in een representatieve steekproef. Indexeren van wordt tijd dus beginnen met een kleine, representatieve gegevensset en vervolgens dit gebeurt stapsgewijs omhoog als uw oplossing zich verder ontwikkelt.

1. Maak een [gegevensbronobject](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in Azure Search voor een verbindingsreeks voor het ophalen van gegevens.

1. Maak een [vaardigheden](https://docs.microsoft.com/rest/api/searchservice/create-skillset) met verrijking stappen.

1. Definieer de [indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index). De *velden* verzameling velden uit de brongegevens bevat. U moet ook aanvullende velden voor het opslaan van de gegenereerde waarden voor inhoud die is gemaakt tijdens de verrijking stub.

1. Definieer de [indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-skillset) verwijst naar de gegevensbron, vaardigheden en index.

1. Voeg binnen de indexeerfunctie *outputFieldMappings*. In deze sectie wordt de uitvoer van de vaardigheden (in stap 3) de invoer-velden in het schema van de index (in stap 4) toegewezen.

1. Verzenden *indexeerfunctie maken* vragen u zojuist hebt (een POST-aanvraag met de definitie van een indexeerfunctie in de aanvraagtekst) gemaakt voor de indexeerfunctie Azure Search express. Deze stap is hoe u de indexeerfunctie uitvoeren, aanroepen van de pijplijn.

1. Voer zoekopdrachten uit voor het evalueren van de resultaten en code update kennis en vaardigheden, schema of configuratie van de indexeerfunctie te wijzigen.

1. [Opnieuw instellen van de indexeerfunctie](search-howto-reindex.md) voordat het opnieuw opbouwen van de pijplijn.

Zie voor meer informatie over specifieke vragen of problemen [tips voor probleemoplossing](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Volgende stappen

+ [Documentatie voor cognitief zoeken](cognitive-search-resources-documentation.md)
+ [Snelstartgids: Probeer cognitief zoeken in een kennismaken met de portal](cognitive-search-quickstart-blob.md)
+ [Zelfstudie: Informatie over de cognitief zoeken-API 's](cognitive-search-tutorial-blob.md)
