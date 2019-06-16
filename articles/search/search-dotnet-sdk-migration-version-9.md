---
title: Een upgrade uitvoert naar de Azure Search .NET SDK versie 9 - Azure Search
description: Code migreren naar Azure Search .NET SDK-versie 9 van oudere versies. Ontdek wat er nieuw is en welke wijzigingen in de code zijn vereist.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: a59deed4ac0cec669ddc5e0335f7274586c702e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65541767"
---
# <a name="upgrade-to-the-azure-search-net-sdk-version-9"></a>Een upgrade uitvoert naar de Azure Search .NET SDK versie 9

Als u versie 7.0-preview of ouder bent van de [Azure Search .NET SDK](https://aka.ms/search-sdk), in dit artikel ziet u een upgrade van uw toepassing versie 9 te gebruiken.

> [!NOTE]
> Als u gebruiken versie 8.0 preview wilt-functies die nog niet algemeen beschikbaar moeten worden geëvalueerd, kunt u ook de instructies in dit artikel om te upgraden naar 8.0-Preview-versie van eerdere versies te volgen.

Zie voor een meer algemeen overzicht van de SDK, inclusief voorbeelden [over het gebruik van Azure Search via een .NET-toepassing](search-howto-dotnet-sdk.md).

Versie 9 van de Azure Search .NET SDK bevat veel wijzigingen van eerdere versies. Sommige hiervan zijn belangrijke wijzigingen, maar ze moeten alleen relatief kleine wijzigingen in uw code nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe SDK-versie.

> [!NOTE]
> Als u versie 4.0-preview of ouder bent, moet u eerst upgraden naar versie 5, en vervolgens een upgrade naar versie 9. Zie [upgraden naar de Azure Search .NET SDK versie 5](search-dotnet-sdk-migration-version-5.md) voor instructies.
>
> Het exemplaar van uw Azure Search-service ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt echter ook doorgaan met een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK gebruikt, bepaalt de versie van de SDK die u gebruikt de corresponderende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven om uit te voeren die code zonder wijzigingen, zelfs als de service wordt bijgewerkt ter ondersteuning van een nieuwere API-versie.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-9"></a>Wat is er nieuw in versie 9
Versie 9 van de Azure Search .NET SDK is gericht op de meest recente algemeen beschikbare versie van de Azure Search REST API, specifiek 2019-05-06. Dit maakt het mogelijk het gebruik van nieuwe functies van Azure Search via een .NET-toepassing, met inbegrip van het volgende:

* [Cognitieve zoekopdrachten](cognitive-search-concept-intro.md) is een functie van AI in Azure Search gebruikt om tekst te extraheren uit afbeeldingen, blobs en andere ongestructureerde gegevensbronnen - verrijken van de inhoud naar meer doorzoekbaar maken in een Azure Search-index worden gebruikt.
* Ondersteuning voor [complexe typen](search-howto-complex-data-types.md) kunt u vrijwel alle geneste JSON-structuur in een Azure Search-index te modelleren.
* [Automatisch aanvullen](search-autocomplete-tutorial.md) biedt een alternatief voor de **voorstellen** API voor het implementeren van de search-as-u-type gedrag. Automatisch aanvullen 'is voltooid"het woord of woordgroep die een gebruiker is momenteel te typen.
* [Parseermodus JsonLines](search-howto-index-json-blobs.md), onderdeel van Azure Blob indexeren, maakt u een search-document per JSON-entiteit die is gescheiden door een nieuwe regel.

### <a name="new-preview-features-in-version-80-preview"></a>Nieuwe preview-functies in versie 8.0-preview
API-versie 2017-11-11-Preview is bedoeld voor versie 8.0-Preview-versie van de Azure Search .NET SDK. Deze versie bevat dezelfde functies van versie 9, plus:

* [Door de klant beheerde versleutelingssleutels](search-security-manage-encryption-keys.md) voor servicezijde versleuteling-at-rest een nieuwe preview-functie is. Naast de ingebouwde versleuteling-at-rest beheerd door Microsoft, kunt u een extra versleutelingslaag waar u de exclusieve eigenaar van de sleutels zijn toepassen.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het bijwerken
Update eerst uw NuGet-verwijzing voor `Microsoft.Azure.Search` met behulp van de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Nadat NuGet heeft de nieuwe pakketten en de bijbehorende afhankelijkheden gedownload, bouwt u uw project opnieuw. Afhankelijk van hoe uw code is opgebouwd, kan deze opnieuw is. Als dit het geval is, bent u klaar om te beginnen.

Als uw build mislukt, moet u om elke build-fout te herstellen. Zie [belangrijke wijzigingen in versie 9](#ListOfChanges) voor meer informatie over het oplossen van elke mogelijke fout bouwen.

Mogelijk ziet u aanvullende build waarschuwingen met betrekking tot verouderde methoden of eigenschappen. De waarschuwingen bevat instructies over wat u moet gebruiken in plaats van de afgeschafte functies. Als uw toepassing gebruikt bijvoorbeeld de `DataSourceType.DocumentDb` eigenschap, moet u ontvangt een waarschuwing met de tekst 'dit lid is afgeschaft. Gebruik cosmos DB in plaats daarvan'.

Als u een build-fouten of waarschuwingen hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 9](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-9"></a>Belangrijke wijzigingen in versie 9

Er zijn enkele belangrijke wijzigingen in versie 9 waarvoor wijzigingen in de code naast het opnieuw opbouwen van uw toepassing.

> [!NOTE]
> De lijst hieronder wijzigingen is niet volledig. Enkele wijzigingen zullen waarschijnlijk niet leiden tot fouten in de build, maar zijn technisch belangrijke omdat ze binaire compatibiliteit met assembly's die afhankelijk van eerdere versies van de Azure Search .NET SDK-assembly's zijn opsplitsen. Dergelijke wijzigingen worden hieronder niet wordt vermeld. Bouw uw toepassing bij een upgrade naar versie 9 om te voorkomen dat een binair compatibiliteitsproblemen.

### <a name="immutable-properties"></a>Onveranderbare eigenschappen

De openbare eigenschappen van verschillende modelklassen zijn nu onveranderbaar. Als u maken van aangepaste exemplaren van deze klassen wilt voor het testen, kunt u de nieuwe parameters constructors gebruiken:

  - `AutocompleteItem`
  - `DocumentSearchResult`
  - `DocumentSuggestResult`
  - `FacetResult`
  - `SearchResult`
  - `SuggestResult`

### <a name="changes-to-field"></a>Wijzigingen in veld

De `Field` klasse is gewijzigd nu dat deze kunnen ook complexe velden bestaan.

De volgende `bool` eigenschappen nu null-waarden zijn:

  - `IsFilterable`
  - `IsFacetable`
  - `IsSearchable`
  - `IsSortable`
  - `IsRetrievable`
  - `IsKey`

Dit is omdat deze eigenschappen nu moeten `null` in het geval van complexe velden. Als u de code die deze eigenschappen lezen hebt, heeft voorbereid voor het afhandelen van `null`. Houd er rekening mee dat alle andere eigenschappen van `Field` zijn altijd en doorgaan met null-waarde, en sommige worden ook `null` in het geval van complexe velden--name de volgende:

  - `Analyzer`
  - `SearchAnalyzer`
  - `IndexAnalyzer`
  - `SynonymMaps`

De constructor zonder parameters van `Field` heeft aangebracht `internal`. Vanaf nu op elke `Field` een expliciete en het gegevenstype op het moment van de bouw vereist.

### <a name="simplified-batch-and-results-types"></a>Vereenvoudigde batch en resultaten typen

In versie 7.0-Preview-versie en eerdere versies, zijn de verschillende categorieën waarin de groepen van documenten gestructureerd in parallelle klassehiërarchieën:

  -  `DocumentSearchResult` en `DocumentSearchResult<T>` overgenomen van `DocumentSearchResultBase`
  -  `DocumentSuggestResult` en `DocumentSuggestResult<T>` overgenomen van `DocumentSuggestResultBase`
  -  `IndexAction` en `IndexAction<T>` overgenomen van `IndexActionBase`
  -  `IndexBatch` en `IndexBatch<T>` overgenomen van `IndexBatchBase`
  -  `SearchResult` en `SearchResult<T>` overgenomen van `SearchResultBase`
  -  `SuggestResult` en `SuggestResult<T>` overgenomen van `SuggestResultBase`

De afgeleide typen zonder een generiek typeparameter zijn bedoeld om te worden gebruikt in scenario's 'dynamisch getypeerde' en aangenomen dat het gebruik van de `Document` type.

Beginnen met de Preview-versie 8.0-versie, zijn de basisklassen en afgeleide klassen van niet-algemene alle verwijderd. Voor dynamisch getypeerde scenario's, kunt u `IndexBatch<Document>`, `DocumentSearchResult<Document>`, enzovoort.
 
### <a name="removed-extensibleenum"></a>Verwijderde ExtensibleEnum

De `ExtensibleEnum` basisklasse is verwijderd. Alle klassen die is afgeleid zijn, zoals nu structs, `AnalyzerName`, `DataType`, en `DataSourceType` bijvoorbeeld. Hun `Create` methoden zijn ook verwijderd. U kunt alleen aanroepen naar verwijderen `Create` omdat deze typen impliciet worden geconverteerd van tekenreeksen zijn. Als die in compilatiefouten resulteert, kunt u de conversieoperator via casten om op te heffen typen expliciet aanroepen. U kunt bijvoorbeeld code als volgt wijzigen:

```csharp
var index = new Index()
{
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("message", AnalyzerName.Create("my_email_analyzer")) { IsSearchable = true }
    },
    ...
}
```

in dit:

```csharp
var index = new Index()
{
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("message", (AnalyzerName)"my_email_analyzer") { IsSearchable = true }
    },
    ...
}
```

Eigenschappen die worden vastgehouden optionele waarden van de volgende typen zijn nu expliciet hebt getypt als null-waarden, zodat ze blijven optioneel.

### <a name="removed-facetresults-and-hithighlights"></a>Verwijderde FacetResults en HitHighlights

De `FacetResults` en `HitHighlights` klassen zijn verwijderd. Facet resultaten zijn nu getypeerd als `IDictionary<string, IList<FacetResult>>` en drukt u op die zijn gemarkeerd als `IDictionary<string, IList<string>>`. Een snelle manier om het oplossen van fouten in de build geïntroduceerd door deze wijziging is het toevoegen van `using` aliassen aan de bovenkant van elk bestand dat gebruikmaakt van de verwijderde typen. Bijvoorbeeld:

```csharp
using FacetResults = System.Collections.Generic.IDictionary<string, System.Collections.Generic.IList<Models.FacetResult>>;
using HitHighlights = System.Collections.Generic.IDictionary<string, System.Collections.Generic.IList<string>>;
```

### <a name="change-to-synonymmap"></a>Synonymmap, die vervolgens wijzigen 

De `SynonymMap` constructor heeft niet langer een `enum` parameter voor `SynonymMapFormat`. Deze opsomming alleen heeft één waarde, en is daarom redundante. Als er fouten als gevolg van dit bouwen, verwijdert u gewoon verwijzingen naar de `SynonymMapFormat` parameter.

### <a name="miscellaneous-model-class-changes"></a>Diverse model klasse wijzigingen

De `AutocompleteMode` eigenschap van `AutocompleteParameters` is niet meer toegestaan. Als u code waarmee deze eigenschap om te worden toegewezen hebt `null`, kunt u deze gewoon verwijderen en de eigenschap wordt automatisch op de standaardwaarde is geïnitialiseerd.

De volgorde van de parameters voor de `IndexAction` constructor is gewijzigd voor deze constructor is automatisch gegenereerd. In plaats van de constructor, wordt u aangeraden de factorymethoden `IndexAction.Upload`, `IndexAction.Merge`, enzovoort.

### <a name="removed-preview-features"></a>Verwijderde preview-functies

Als u een van versie 8.0 preview-versie 9 upgrade, rekening mee dat versleuteling met de klant beheerde sleutels is verwijderd omdat deze functie nog in preview is. Met name de `EncryptionKey` eigenschappen van `Index` en `SynonymMap` zijn verwijderd.

Als uw toepassing een vaste afhankelijkheid van deze functie heeft, wordt het niet mogelijk om bij te werken naar versie 9 van de Azure Search .NET SDK. U kunt echter ook doorgaan met versie 8.0-preview. Echter Houd er rekening mee dat **we raden niet met behulp van preview SDK's in productietoepassingen**. Preview-functies zijn voor de evaluatie alleen en kunnen worden gewijzigd.

> [!NOTE]
> Als u versleutelde gemaakt indexen of synoniem kaarten met behulp van versie 8.0-preview van de SDK, kunt u nog steeds zich ze gebruiken en wijzigen van de definities die met behulp van versie 9 van de SDK zonder negatieve gevolgen voor hun status voor schijfversleuteling. Versie 9 van de SDK verzendt de `encryptionKey` eigenschap met de REST-API, en als gevolg hiervan de REST-API wordt de versleutelingsstatus van de bron niet gewijzigd. 

### <a name="behavioral-change-in-data-retrieval"></a>Gedragswijziging in het ophalen van gegevens

Als u de 'dynamisch getypeerde" `Search`, `Suggest`, of `Get` API's die exemplaren van het type retourneren `Document`, houd er rekening mee dat ze nu lege JSON-matrices te deserialiseren `object[]` in plaats van `string[]`.

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van de Azure Search .NET SDK nodig hebt, raadpleegt u de [.NET procedures](search-howto-dotnet-sdk.md).

We stellen uw feedback over de SDK. Als u problemen ondervindt, gerust ons vragen voor meer informatie over op [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat u het voorvoegsel van de titel van uw probleem met '[Azure Search]'.

Hartelijk dank voor het gebruik van Azure Search.
