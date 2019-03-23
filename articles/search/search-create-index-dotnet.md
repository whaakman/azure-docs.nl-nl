---
title: Maken van een index in C# -Azure Search
description: Informatie over het maken van een doorzoekbare index van de volledige tekst in C# met behulp van de Azure Search .NET SDK.
author: heidisteen
manager: cgronlun
ms.author: heidist
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/22/2019
ms.openlocfilehash: a5861faaf26962d34d1c356e29dce1be40f8716b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370581"
---
# <a name="quickstart-1---create-an-azure-search-index-in-c"></a>Quickstart: 1 - een Azure Search-index in makenC#

In dit artikel begeleidt u bij het proces voor het maken van [een Azure Search-index](search-what-is-an-index.md) met behulp van C# en de [.NET SDK](https://aka.ms/search-sdk). Dit is de eerste les in een oefening 3-onderdeel voor het maken, laden en query een index. Maken van een index wordt bereikt door het uitvoeren van deze taken:

> [!div class="checklist"]
> * Maak een [ `SearchServiceClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient?view=azure-dotnet) object verbinding maken met een service voor zoeken.
> * Maak een [ `Index` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) object om door te geven als een parameter voor `Indexes.Create`.
> * Roep de `Indexes.Create` methode voor `SearchServiceClient` voor het verzenden van de `Index` naar een service.

## <a name="prerequisites"></a>Vereisten

[Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start.

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), alle edities. Voorbeeldcode en instructies zijn getest op de gratis Community-versie.

Lees het URL-eindpunt en de admin api-sleutel van uw search-service. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

  1. In de Azure-portal in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

  2. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

  ![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="1---open-the-project"></a>1 - opent u het project

Download de voorbeeldcode [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) vanuit GitHub. 

Vervang de Standaardinhoud met het onderstaande voorbeeld in appsettings.json, en geef vervolgens de servicenaam en beheerder api-sleutel voor uw service. Voor de naam van de service hoeft u alleen de naam zelf. Bijvoorbeeld, als uw URL https://mydemo.search.windows.net, toevoegen `mydemo` naar de JSON-bestand.


```json
{
    "SearchServiceName": "Put your search service name here",
    "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
}
```

Wanneer deze waarden zijn ingesteld, kunt u F5 build de oplossing voor het uitvoeren van de console-app. De overige stappen in deze oefening en die Ga als volgt zijn een verkenning van de werking van deze code. 

U kunt ook verwijzen naar [over het gebruik van Azure Search via een .NET-toepassing ](search-howto-dotnet-sdk.md) voor meer dekking van de SDK-gedrag gedetailleerde. 

<a name="CreateSearchServiceClient"></a>

## <a name="2---create-a-client"></a>2 - een-client maken

Als u wilt gaan met de Azure Search .NET SDK, maak een instantie van de `SearchServiceClient` klasse. Deze klasse heeft verschillende constructors. De gewenste constructor krijgt u de naam van uw zoekservice en een `SearchCredentials`-object als parameters. `SearchCredentials` verpakt uw api-sleutel.

De volgende code vindt u in het bestand Program.cs. Deze maakt u een nieuwe `SearchServiceClient` met waarden voor de search-servicenaam en api-sleutel die zijn opgeslagen in het configuratiebestand van de toepassing (appsettings.json).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

`SearchServiceClient` heeft een `Indexes`-eigenschap. Deze eigenschap bevat alle methoden die u nodig hebt om een index voor Azure Search te maken, weergeven, bijwerken of verwijderen.

> [!NOTE]
> De klasse `SearchServiceClient` beheert verbindingen met uw zoekservice. Als u wilt voorkomen dat er te veel verbindingen worden geopend, kunt u proberen één instantie van `SearchServiceClient` in uw toepassing te delen, indien mogelijk. De bijbehorende methoden zijn thread-safe om delen in te schakelen.
> 
> 

<a name="DefineIndex"></a>

## <a name="3---construct-index"></a>3 - Index maken
Één aanroep naar de `Indexes.Create` methode maakt u een index. Deze methode wordt gebruikt als parameter een `Index` -object dat een Azure Search-index definieert. Maak een `Index` object en dit als volgt initialiseren:

1. Stel de eigenschap `Name` van het `Index`-object in op de naam van de index.

2. Stel de eigenschap `Fields` van het `Index`-object in op een matrix van `Field`-objecten. De eenvoudigste manier om `Field`-objecten te maken is de `FieldBuilder.BuildForType`-methode aanroepen. Hierbij wordt een modelklasse doorgegeven voor de typeparameter. Een modelklasse heeft eigenschappen die worden toegewezen aan de velden van uw index. Hiermee kunt u documenten uit de zoekindex binden aan exemplaren van uw modelklasse.

> [!NOTE]
> Als u niet van plan bent om een modelklasse te gebruiken, moet u alsnog de index definiëren door rechtstreeks `Field`-objecten te maken. U kunt de naam van het veld aan de constructor doorgeven, samen met het gegevenstype (of analyse voor tekenreeksvelden). U kunt ook andere eigenschappen, zoals instellen `IsSearchable`, `IsFilterable`, een paar te noemen.
>
>

Het is belangrijk dat u uw gebruiker en uw bedrijf nodig waarmee u rekening moet bij het ontwerpen van uw index. Elk veld moet worden toegewezen de [kenmerken](https://docs.microsoft.com/rest/api/searchservice/Create-Index) die bepalen welke zoekfuncties (filteren, facetten, sorteren, enzovoort) zijn in velden. Als u een eigenschap niet expliciet instelt, worden de bijbehorende zoekfuncties door de `Field`-klasse uitgeschakeld, tenzij u deze functies specifiek hebt ingeschakeld.

In dit voorbeeld wordt de indexnaam van de "hotels" is en velden zijn gedefinieerd met behulp van een modelklasse. Elke eigenschap van de modelklasse heeft kenmerken die het zoekgerelateerde gedrag van het bijbehorende indexveld bepalen. De modelklasse wordt als volgt gedefinieerd:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

De kenmerken voor elke eigenschap zijn zorgvuldig gekozen op basis van de gebruiksverwachtingen in een toepassing. Het is bijvoorbeeld waarschijnlijk dat gebruikers die een hotel zoeken, geïnteresseerd zijn in overeenkomende sleutelwoorden in het veld `description`. Daarom hebben we ingesteld dat gebruikers in dat veld in volledige tekst kunnen zoeken door het kenmerk `IsSearchable` toe te voegen aan de eigenschap `Description`.

In een index van het type `string` moet precies één veld als *sleutelveld* zijn aangewezen door het kenmerk `Key` toe te voegen (zie `HotelId` in het bovenstaande voorbeeld).

Voor de bovenstaande indexdefinitie wordt gebruikgemaakt van een taalanalyse voor het veld `description_fr`, omdat dit veld is bedoeld voor het opslaan van Franse tekst. Zie voor meer informatie, [taalanalyse toevoegen aan een Azure Search-index](index-add-language-analyzers.md).

> [!NOTE]
> Standaard is de naam van elke eigenschap in de modelklasse komt overeen met de naam van het veld in de index. Als u alle eigenschapnamen wilt toewijzen aan camelCase-veldnamen, markeert u de klasse met het kenmerk `SerializePropertyNamesAsCamelCase`. Als u wilt toewijzen aan een andere naam, kunt u het kenmerk `JsonProperty` gebruiken, zoals de bovenstaande eigenschap `DescriptionFr`. Het kenmerk `JsonProperty` gaat vóór het kenmerk `SerializePropertyNamesAsCamelCase`.
> 
> 

Nu u een modelklasse hebt gedefinieerd, kunt u heel eenvoudig een indexdefinitie maken:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="4---call-indexescreate"></a>4 - aanroep Indexes.Create
Nu dat u een geïnitialiseerd hebt `Index` object, de index maken door het aanroepen van `Indexes.Create` op uw `SearchServiceClient` object:

```csharp
serviceClient.Indexes.Create(definition);
```

Als de aanvraag is gelukt, wordt de methode als normaal geretourneerd. Als er een probleem met de aanvraag is, zoals een ongeldige parameter, genereert de methode `CloudException`.

Wanneer u klaar bent met een index en wilt verwijderen, roept de `Indexes.Delete` methode voor uw `SearchServiceClient`. Bijvoorbeeld:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> In de voorbeeldcode in dit artikel is voor het gemak gebruikgemaakt van de synchrone methoden van de Azure Search .NET SDK. Het wordt aanbevolen in uw eigen toepassingen asynchrone methoden te gebruiken, zodat de toepassingen schaalbaar zijn en goed reageren. In de bovenstaande voorbeelden kunt u bijvoorbeeld `CreateAsync` en `DeleteAsync` gebruiken in plaats van `Create` en `Delete`.
> 
> 

## <a name="next-steps"></a>Volgende stappen
In deze quickstart maakt u een lege Azure Search-index op basis van een schema dat gegevenstypen voor velden en het gedrag definieert gemaakt. De index is een 'bare bEen' index die bestaan uit een naam en een verzameling van velden toegeschreven. Een realistischer index zou bevatten andere elementen, zoals [scoreprofielen](index-add-scoring-profiles.md), [suggesties](index-add-suggesters.md) voor ondersteuning van typeahead [synoniemen](search-synonyms.md), en mogelijk [ analysevoorzieningen](index-add-custom-analyzers.md). Het is raadzaam om deze mogelijkheden te bezoeken, wanneer u bekend bent met de basiswerkstroom.

De volgende snelstartgids in deze reeks wordt uitgelegd hoe u de index met doorzoekbare inhoud laden.

> [!div class="nextstepaction"]
> [Gegevens laden in een Azure Search-index metC#](search-import-data-dotnet.md)