---
title: Een upgrade uitvoert naar de Azure Search .NET SDK versie 3 - Azure Search
description: Code migreren naar Azure Search .NET SDK versie 3 van oudere versies. Ontdek wat er nieuw is en welke wijzigingen in de code zijn vereist.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 4acf609ca1f81e69babfa1a319b43e20e84a8395
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317250"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Een upgrade naar de Azure Search .NET SDK versie 3
Als u versie 2.0 preview of ouder bent van de [Azure Search .NET SDK](https://aka.ms/search-sdk), in dit artikel ziet u een upgrade van uw toepassing voor het gebruik van versie 3.

Zie voor een meer algemeen overzicht van de SDK, inclusief voorbeelden [over het gebruik van Azure Search via een .NET-toepassing](search-howto-dotnet-sdk.md).

Versie 3 van de Azure Search .NET SDK bevat enkele wijzigingen van eerdere versies. Dit zijn doorgaans kleine, zodat u uw code wijzigt, moet alleen minimale inspanning vereisen. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe SDK-versie.

> [!NOTE]
> Als u versie 1.0.2-preview of ouder bent, moet u een upgrade uitvoeren naar versie 1.1 eerste, en vervolgens een upgrade uitvoeren naar versie 3. Zie [upgraden naar de Azure Search .NET SDK versie 1.1](search-dotnet-sdk-migration-version-1.md) voor instructies.
>
> Het exemplaar van uw Azure Search-service ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt echter ook doorgaan met een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK gebruikt, bepaalt de versie van de SDK die u gebruikt de corresponderende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven om uit te voeren die code zonder wijzigingen, zelfs als de service wordt bijgewerkt ter ondersteuning van een nieuwere API-versie.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Wat is er nieuw in versie 3
Versie 3 van de Azure Search .NET SDK is gericht op de meest recente algemeen beschikbare versie van de Azure Search REST API, specifiek 2016-09-01. Dit maakt het mogelijk te veel nieuwe functies van Azure Search via een .NET-toepassing, met inbegrip van de volgende gebruiken:

* [Analysevoorzieningen aanpassen](https://aka.ms/customanalyzers)
* [Azure Blob-opslag](search-howto-indexing-azure-blob-storage.md) en [Azure Table Storage](search-howto-indexing-azure-tables.md) ondersteuning voor indexeerfuncties
* Aanpassing van de indexeerfunctie via [veldtoewijzingen](search-indexer-field-mappings.md)
* ETags ondersteuning voor het veilig gelijktijdige bijwerken van definities van de index, Indexeerfuncties en gegevensbronnen inschakelen
* Ondersteuning voor het bouwen van index velddefinities declaratief door met het inrichten van uw modelklasse en met behulp van de nieuwe `FieldBuilder` klasse.
* Ondersteuning voor .NET Core en draagbare .NET-profiel 111

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het bijwerken
Update eerst uw NuGet-verwijzing voor `Microsoft.Azure.Search` met behulp van de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Nadat NuGet heeft de nieuwe pakketten en de bijbehorende afhankelijkheden gedownload, bouwt u uw project opnieuw. Afhankelijk van hoe uw code is opgebouwd, kan deze opnieuw is. Als dit het geval is, bent u klaar om te beginnen.

Als uw build mislukt, ziet u een build-fout als volgt uit:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

De volgende stap is om deze build-fout te herstellen. Zie [belangrijke wijzigingen in versie 3](#ListOfChanges) voor meer informatie over wat de fout veroorzaakt en hoe u dit herstelt.

Mogelijk ziet u aanvullende build waarschuwingen met betrekking tot verouderde methoden of eigenschappen. De waarschuwingen bevat instructies over wat u moet gebruiken in plaats van de afgeschafte functies. Als uw toepassing gebruikt bijvoorbeeld de `IndexingParameters.Base64EncodeKeys` eigenschap, moet u ontvangt een waarschuwing waarin wordt gemeld `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Als u eventuele fouten in de build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 3](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Belangrijke wijzigingen in versie 3
Er wordt een klein aantal belangrijke wijzigingen in versie 3 waarvoor code gewijzigd naast opnieuw opbouwen van uw toepassing.

### <a name="indexesgetclient-return-type"></a>Het retourtype Indexes.GetClient
De `Indexes.GetClient` methode heeft een nieuwe retourtype. Voorheen was het geretourneerd `SearchIndexClient`, maar dit is gewijzigd in `ISearchIndexClient` in versie 2.0-Preview-versie, en die wijziging wordt doorgevoerd in versie 3. Dit is de ondersteuning van klanten die willen model de `GetClient` methode voor eenheidstests door te retourneren een mock-implementatie van `ISearchIndexClient`.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

U kunt deze wijzigen in dit een build-fouten te herstellen:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>AnalyzerName, gegevenstype en anderen zijn niet langer impliciet worden geconverteerd naar tekenreeksen
Er zijn vele typen in de Azure Search .NET SDK, die zijn afgeleid van `ExtensibleEnum`. Eerder deze typen zijn alle impliciet worden geconverteerd naar type `string`. Echter, een fout is gedetecteerd op de `Object.Equals` -implementatie voor deze klassen, en het verhelpen van de fout vereist deze impliciete conversie uit te schakelen. Expliciete conversie naar `string` nog steeds is toegestaan.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

U kunt deze wijzigen in dit een build-fouten te herstellen:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Verouderde leden verwijderd

Mogelijk ziet u build fouten met betrekking tot methoden of eigenschappen die zijn gemarkeerd als verouderd in versie 2.0 preview en daarna verwijderd in versie 3. Als u dergelijke fouten optreden, als volgt op te lossen:

- Als u deze constructor: `ScoringParameter(string name, string value)`, in plaats daarvan deze knop bijvoorbeeld gebruiken: `ScoringParameter(string name, IEnumerable<string> values)`
- Als u de `ScoringParameter.Value` eigenschap, gebruikt de `ScoringParameter.Values` eigenschap of de `ToString` methode in plaats daarvan.
- Als u de `SearchRequestOptions.RequestId` eigenschap, gebruikt de `ClientRequestId` eigenschap in plaats daarvan.

### <a name="removed-preview-features"></a>Verwijderde preview-functies

Als u een van versie 2.0 preview-versie 3 upgrade, er rekening mee dat JSON en CSV-ondersteuning voor Blob-indexeerfuncties parseren is verwijderd omdat deze functies zijn nog in preview. Met name de volgende methoden van de `IndexingParametersExtensions` klasse zijn verwijderd:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Als uw toepassing een vaste afhankelijkheid van deze functies heeft, wordt het niet mogelijk om bij te werken naar versie 3 van de Azure Search .NET SDK. U kunt echter ook doorgaan met versie 2.0-preview. Echter Houd er rekening mee dat **we raden niet met behulp van preview SDK's in productietoepassingen**. Preview-functies zijn voor de evaluatie alleen en kunnen worden gewijzigd.

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van de Azure Search .NET SDK nodig hebt, raadpleegt u de [.NET procedures](search-howto-dotnet-sdk.md).

We stellen uw feedback over de SDK. Als u problemen ondervindt, gerust ons vragen voor meer informatie over op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat u het voorvoegsel van de titel van uw probleem met '[Azure Search]'.

Hartelijk dank voor het gebruik van Azure Search.
