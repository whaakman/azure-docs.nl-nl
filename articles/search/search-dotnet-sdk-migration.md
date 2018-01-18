---
title: Een upgrade naar de Azure Search .NET SDK versie 3 | Microsoft Docs
description: Een upgrade naar de Azure Search .NET SDK versie 3
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/15/2018
ms.author: brjohnst
ms.openlocfilehash: 48238788e06057ccaba41d1d3f500b5c10c93cb7
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Een upgrade naar de Azure Search .NET SDK versie 3
Als u versie 2.0 preview of oudere van de [Azure Search .NET SDK](https://aka.ms/search-sdk), dit artikel helpt u bij het bijwerken van uw toepassing versie 3 gebruiken.

Zie voor een meer algemene overzicht van de SDK, inclusief voorbeelden [het gebruik van Azure Search vanuit een .NET-toepassing](search-howto-dotnet-sdk.md).

Versie 3 van de Azure Search .NET SDK bevat een aantal wijzigingen uit eerdere versies. Dit zijn vooral kleine, zodat u uw code wijzigt, moet alleen minimale inspanning nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe versie van de SDK.

> [!NOTE]
> Als u versie 1.0.2-preview of ouder, moet u een upgrade uitvoeren naar versie 1.1 eerste en vervolgens een upgrade uitvoeren naar versie 3. Zie [upgraden naar Azure Search .NET SDK versie 1.1](search-dotnet-sdk-migration-version-1.md) voor instructies.
>
> Uw Azure Search-service-exemplaar ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt blijven gebruiken van een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK, bepaalt de versie van de SDK die u in de desbetreffende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven code uit te voeren die zonder wijzigingen zelfs als de service is bijgewerkt zodat een nieuwere versie van de API wordt ondersteund.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Wat is er nieuw in versie 3
Versie 3 van de Azure Search .NET SDK gericht is op de meest recente versie van de Azure Search REST API, speciaal 2016-09-01 van algemeen beschikbaar. Dit maakt het mogelijk te veel nieuwe functies van Azure Search vanuit een .NET-toepassing, waaronder de volgende gebruiken:

* [Analysevoorzieningen aanpassen](https://aka.ms/customanalyzers)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) en [Azure Table Storage](search-howto-indexing-azure-tables.md) indexeerfunctie-ondersteuning
* Aanpassing van de indexeerfunctie via [veld toewijzingen](search-indexer-field-mappings.md)
* ETags ondersteunen om in te schakelen veilige gelijktijdige bijwerken van definities van de index, Indexeerfuncties en gegevensbronnen
* Ondersteuning voor het bouwen van de index velddefinities declaratief met versieren van uw modelklasse en het gebruik van de nieuwe `FieldBuilder` klasse.
* Ondersteuning voor .NET Core en draagbare .NET-profiel 111

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het upgraden
Eerst uw NuGet-verwijzing voor bijwerken `Microsoft.Azure.Search` via de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Zodra de NuGet heeft de nieuwe pakketten en afhankelijkheden zijn gedownload, opnieuw worden opgebouwd uw project. Afhankelijk van hoe uw code is gestructureerd, deze mogelijk opnieuw worden opgebouwd is. Zo ja, bent u nu klaar voor!

Als uw build is mislukt, ziet u een build-fout als volgt:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

De volgende stap is om deze build-fout te corrigeren. Zie [wijzigingen op te splitsen in versie 3](#ListOfChanges) voor meer informatie over wat de fout veroorzaakt en hoe u dit herstelt.

Mogelijk ziet u extra build-waarschuwingen verouderd methoden of eigenschappen. De waarschuwingen bevat instructies over wat u moet gebruiken in plaats van de afgeschafte functies. Als uw toepassing gebruikt bijvoorbeeld de `IndexingParameters.Base64EncodeKeys` eigenschap, krijgt u een waarschuwing dat`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Als u eventuele fouten in de build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 3](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Wijzigingen op te splitsen in versie 3
Er is een klein aantal wijzigingen op te splitsen in versie 3 waarvoor code wijzigingen naast opnieuw opbouwen van uw toepassing.

### <a name="indexesgetclient-return-type"></a>Het retourtype Indexes.GetClient
De `Indexes.GetClient` methode heeft een nieuwe retourtype. Voorheen geretourneerd `SearchIndexClient`, maar dit is gewijzigd in `ISearchIndexClient` in preview-versie 2.0 en die wijziging geldt ook voor versie 3. Dit is de ondersteuning van klanten die willen model de `GetClient` methode voor de eenheidstests door te retourneren van een mock-implementatie van `ISearchIndexClient`.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

U kunt deze wijzigen in dit herstel de fouten build:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>AnalyzerName, gegevenstype en andere zijn niet langer impliciet te converteren naar tekenreeksen
Er zijn vele typen in de Azure Search .NET SDK, die zijn afgeleid van `ExtensibleEnum`. Eerder deze typen zijn alle impliciet omgezet naar type `string`. Echter een probleem is gedetecteerd op de `Object.Equals` implementatie voor deze klassen en het verhelpen van de fout vereist deze impliciete conversie uit te schakelen. Expliciete conversie naar `string` is wel toegestaan.

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

U kunt deze wijzigen in dit herstel de fouten build:

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

### <a name="removed-obsolete-members"></a>Verouderd leden verwijderd

Mogelijk ziet u build fouten met betrekking tot methoden of eigenschappen die zijn gemarkeerd als verouderd in versie 2.0-preview en vervolgens verwijderd in versie 3. Als u dergelijke fouten optreden, wordt hier hoe u deze op te lossen:

- Als u deze constructor: `ScoringParameter(string name, string value)`, gebruik in plaats daarvan deze:`ScoringParameter(string name, IEnumerable<string> values)`
- Als u de `ScoringParameter.Value` eigenschap, gebruikt de `ScoringParameter.Values` eigenschap of de `ToString` methode in plaats daarvan.
- Als u de `SearchRequestOptions.RequestId` eigenschap, gebruikt de `ClientRequestId` eigenschap in plaats daarvan.

### <a name="removed-preview-features"></a>Verwijderde preview-functies

Let als bij een van versie 2.0 preview-versie 3 upgrade of JSON- en CSV-ondersteuning voor Blob indexeerfuncties parseren is verwijderd omdat deze functies zijn nog steeds in preview. In het bijzonder de volgende methoden van de `IndexingParametersExtensions` klasse zijn verwijderd:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Als uw toepassing een vaste afhankelijkheid van deze functies heeft, kunt u zich niet bijwerken naar versie 3 van de Azure Search .NET SDK. U kunt blijven gebruiken preview-versie 2.0. Echter, neem Houd rekening met het **we raden niet met behulp van voorbeeld SDK's in productietoepassingen**. Preview-functies voor evaluatie alleen zijn en kunnen worden gewijzigd.

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van Azure Search .NET SDK nodig hebt, raadpleegt u de [.NET How-to](search-howto-dotnet-sdk.md).

Uw feedback is Welkom op de SDK. Als u problemen ondervindt, gerust ons om hulp te vragen op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat het voorvoegsel van de titel van uw probleem met '[Azure Search]'.

Hartelijk dank voor het gebruik van Azure Search.
