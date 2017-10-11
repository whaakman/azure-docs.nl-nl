---
title: Een index maken (.NET-API - Azure Search) | Microsoft Docs
description: Een index in code maken met behulp van de Azure Search .NET SDK.
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: fac41903c3e5731d17f832ff58145fe74dfa29f1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Een index voor Azure Search maken met behulp van de .NET SDK
> [!div class="op_single_selector"]
> * [Overzicht](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Dit artikel begeleidt u stapsgewijs door het proces van het maken van een [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) voor Azure Search met behulp van de [Azure Search .NET SDK](https://aka.ms/search-sdk).

Voordat u de stappen in dit artikel uitvoert en een index maakt, moet u eerst [een Azure Search-service](search-create-service-portal.md) hebben gemaakt.

> [!NOTE]
> Alle voorbeeldcode in dit artikel is geschreven in C#. U vindt de volledige broncode [op GitHub](http://aka.ms/search-dotnet-howto). Voor een uitgebreidere walkthrough van de voorbeeldcode kunt ook meer lezen over de [.NET-SDK voor Azure Search](search-howto-dotnet-sdk.md).


## <a name="identify-your-azure-search-services-admin-api-key"></a>De admin api-sleutel voor de Azure Search-service vaststellen
Nu u een Azure Search-service hebt ingericht, bent u bijna klaar om aanvragen te verzenden voor uw service-eindpunt met de .NET SDK. Eerst moet u een van de admin api-sleutels ophalen die is gegenereerd voor de zoekservice die u hebt ingericht. De .NET SDK verzendt deze api-sleutel bij elke aanvraag voor uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

1. Als u de API-sleutels van uw service wilt opzoeken, meldt u zich aan bij [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van uw Azure Search-service
3. Klik op het pictogram Sleutels

Uw service heeft zowel *administratorsleutels* als *querysleutels*.

* De primaire en secundaire *administratorsleutels* verlenen volledige rechten voor alle bewerkingen, inclusief de mogelijkheid voor het beheren van de service, het maken en verwijderen van indexen, indexeerfuncties en gegevensbronnen. Er zijn twee sleutels, zodat u de secundaire sleutel kunt gebruiken als u de primaire sleutel opnieuw aan het genereren bent en vice versa.
* Uw *querysleutels* geven alleen-lezentoegang tot indexen en documenten. Deze sleutels worden doorgaans verleend aan clienttoepassingen die zoekaanvragen verlenen.

Als u een index wilt maken, kunt u de primaire of secundaire administratorsleutel gebruiken.

<a name="CreateSearchServiceClient"></a>

## <a name="create-an-instance-of-the-searchserviceclient-class"></a>Een instantie van de klasse SearchServiceClient maken
Als u de Azure Search .NET SDK wilt gebruiken, moet u een instantie van de klasse `SearchServiceClient` maken. Deze klasse heeft verschillende constructors. De gewenste constructor krijgt u de naam van uw zoekservice en een `SearchCredentials`-object als parameters. `SearchCredentials` verpakt uw api-sleutel.

Met de code hieronder wordt een nieuwe `SearchServiceClient` gemaakt die werkt met de naam van de zoekservice en de api-sleutel die zijn opgeslagen in het configuratiebestand van de toepassing (`appsettings.json` in het geval van de [voorbeeldtoepassing](http://aka.ms/search-dotnet-howto)):

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

## <a name="define-your-azure-search-index"></a>Uw Azure Search-index definiëren
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

## <a name="create-the-index"></a>De index maken
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

