---
title: In code een index maken met behulp van de .NET API - Azure Search
description: Lees hoe u een doorzoekbare volledige-tekstindex maakt met behulp van de Azure Search .NET SDK en C#-voorbeeldcode.
author: brjohnstmsft
manager: jlembicz
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/14/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: f0ac5ee77bf4a479e48bbe048d2a558d78b18fe3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58136407"
---
# <a name="quickstart-create-load-and-query-an-azure-search-index-using-the-net-sdk"></a>Quickstart: Maken, laden en query uitvoeren op een Azure Search-index met de .NET SDK
> [!div class="op_single_selector"]
> * [C#](search-create-index-dotnet.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [Postman (REST)](search-fiddler.md)
> * [Portal](search-create-index-portal.md)
> 

In dit artikel begeleidt u stapsgewijs door het proces voor het maken, laden en opvragen van een Azure Search [index](search-what-is-an-index.md) met behulp van de [Azure Search .NET SDK](https://aka.ms/search-sdk).

> [!NOTE]
> Alle voorbeeldcode in dit artikel is geschreven in C#. U vindt de volledige broncode [op GitHub](https://aka.ms/search-dotnet-howto). Voor een uitgebreidere walkthrough van de voorbeeldcode kunt ook meer lezen over de [.NET-SDK voor Azure Search](search-howto-dotnet-sdk.md).

## <a name="prerequisites"></a>Vereisten

+ [Maak een Azure Search-service](search-create-service-portal.md). U kunt een gratis service voor deze Quick Start.

+ [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), alle edities. Voorbeeldcode en instructies zijn getest op de gratis Community-versie.

+ URL-eindpunt en de admin api-sleutel van uw Search-service. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

    1. In de Azure-portal [Zoek uw service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in de lijst met Services.

    2. In **overzicht**, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://my-service-name.search.windows.net`.

    3. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel op uw aanvraag.

    ![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

    Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="1---create-a-new-project"></a>1 - een nieuw project maken

Maak in Visual Studio, een nieuwe Visual C# project. Een goede sjabloon voor deze Quick Start is Visual C# > aan de slag > Web-App. Deze sjabloon kunt u een bestand appsettings.json.  

Vervang de Standaardinhoud met het onderstaande voorbeeld in appsettings.json, en geef vervolgens de servicenaam en beheerder api-sleutel voor uw service. Voor de naam van de service hoeft u alleen de naam zelf. Bijvoorbeeld, als uw URL https://mydemo.search.windows.net, toevoegen `mydemo` naar de JSON-bestand.


```json
{
    "SearchServiceName": "Put your search service name here",
    "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
}
```

<a name="CreateSearchServiceClient"></a>

## <a name="2---create-an-instance-of-the-searchserviceclient-class"></a>2 - een exemplaar van de klasse SearchServiceClient maken
Als u de Azure Search .NET SDK wilt gebruiken, moet u een instantie van de klasse `SearchServiceClient` maken. Deze klasse heeft verschillende constructors. De gewenste constructor krijgt u de naam van uw zoekservice en een `SearchCredentials`-object als parameters. `SearchCredentials` verpakt uw api-sleutel.

Kopieer de volgende code naar het bestand Program.cs. De code hieronder maakt u een nieuwe `SearchServiceClient` met waarden voor de search-servicenaam en api-sleutel die zijn opgeslagen in het configuratiebestand van de toepassing (appsettings.json).

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

## <a name="3---define-an-index-schema"></a>3 - een indexschema definiëren
Uw index is met één aanroep naar de methode `Indexes.Create` gemaakt. Deze methode gebruikt een `Index`-object als parameter voor uw Azure Search-index. U moet een `Index`-object maken en dit als volgt initialiseren:

1. Stel de eigenschap `Name` van het `Index`-object in op de naam van de index.

2. Stel de eigenschap `Fields` van het `Index`-object in op een matrix van `Field`-objecten. De eenvoudigste manier om `Field`-objecten te maken is de `FieldBuilder.BuildForType`-methode aanroepen. Hierbij wordt een modelklasse doorgegeven voor de typeparameter. Een modelklasse heeft eigenschappen die worden toegewezen aan de velden van uw index. Hiermee kunt u documenten uit de zoekindex binden aan exemplaren van uw modelklasse.

> [!NOTE]
> Als u niet van plan bent om een modelklasse te gebruiken, moet u alsnog de index definiëren door rechtstreeks `Field`-objecten te maken. U kunt de naam van het veld aan de constructor doorgeven, samen met het gegevenstype (of analyse voor tekenreeksvelden). U kunt ook andere eigenschappen instellen, zoals `IsSearchable`, `IsFilterable`, enzovoort.
>
>

Wanneer u uw index opzet, is het belangrijk dat u in uw achterhoofd houdt wat de gebruiker en uw bedrijf nodig hebben, aangezien aan elk veld de [relevante eigenschappen](https://docs.microsoft.com/rest/api/searchservice/Create-Index) moeten worden toegewezen. Deze eigenschappen bepalen welke zoekfuncties (filteren, facetten, zoekacties in volledige tekst sorteren enzovoort) beschikbaar zijn in velden. Als u een eigenschap niet expliciet instelt, worden de bijbehorende zoekfuncties door de `Field`-klasse uitgeschakeld, tenzij u deze functies specifiek hebt ingeschakeld.

In ons voorbeeld is de index 'hotels' genoemd en zijn de velden gedefinieerd met een modelklasse. Elke eigenschap van de modelklasse heeft kenmerken die het zoekgerelateerde gedrag van het bijbehorende indexveld bepalen. De modelklasse wordt als volgt gedefinieerd:

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

Voor de bovenstaande indexdefinitie wordt gebruikgemaakt van een taalanalyse voor het veld `description_fr`, omdat dit veld is bedoeld voor het opslaan van Franse tekst. Zie het onderwerp [Language support](https://docs.microsoft.com/rest/api/searchservice/Language-support) (Taalondersteuning) en het bijbehorende [blogbericht](https://azure.microsoft.com/blog/language-support-in-azure-search/) voor meer informatie over taalanalyse.

> [!NOTE]
> De naam van elke eigenschap in de modelklasse wordt standaard ook gebruikt als de naam van het bijbehorende veld in de index. Als u alle eigenschapnamen wilt toewijzen aan camelCase-veldnamen, markeert u de klasse met het kenmerk `SerializePropertyNamesAsCamelCase`. Als u wilt toewijzen aan een andere naam, kunt u het kenmerk `JsonProperty` gebruiken, zoals de bovenstaande eigenschap `DescriptionFr`. Het kenmerk `JsonProperty` gaat vóór het kenmerk `SerializePropertyNamesAsCamelCase`.
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

## <a name="4---create-the-index-on-the-service"></a>4: de index van de service maken
Nu u een geïnitialiseerd `Index`-object hebt, kunt u de index maken door `Indexes.Create` aan te roepen op uw `SearchServiceClient`-object:

```csharp
serviceClient.Indexes.Create(definition);
```

Als de aanvraag is gelukt, wordt de methode als normaal geretourneerd. Als er een probleem met de aanvraag is, zoals een ongeldige parameter, genereert de methode `CloudException`.

Als u klaar bent met een index en deze weer wilt verwijderen, roept u de `Indexes.Delete`-methode aan op uw `SearchServiceClient`. De index hotels zou bijvoorbeeld als volgt kunnen worden verwijderd:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> In de voorbeeldcode in dit artikel is voor het gemak gebruikgemaakt van de synchrone methoden van de Azure Search .NET SDK. Het wordt aanbevolen in uw eigen toepassingen asynchrone methoden te gebruiken, zodat de toepassingen schaalbaar zijn en goed reageren. In de bovenstaande voorbeelden kunt u bijvoorbeeld `CreateAsync` en `DeleteAsync` gebruiken in plaats van `Create` en `Delete`.
> 
> 


## <a name="next-steps"></a>Volgende stappen
Als u een index voor Azure Search hebt gemaakt, kunt u [de inhoud naar de index uploaden](search-what-is-data-import.md), zodat u kunt beginnen met het zoeken van gegevens.

