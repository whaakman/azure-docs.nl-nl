---
title: C#-zelfstudie over synoniemen in Azure Search | Microsoft Docs
description: In deze zelfstudie voegt u de synoniemenfunctie toe aan een index in Azure Search.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 8340c4dc2a855911073905a3aea93e19fc7b520d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990558"
---
# <a name="tutorial-add-synonyms-for-azure-search-in-c"></a>Zelfstudie: Synoniemen toevoegen voor Azure Search in C#

Met synoniemen breidt u een query uit door termen te gebruiken die semantisch overeenkomen met de ingevoerde term. Mogelijk wilt u bijvoorbeeld dat met 'auto' ook documenten worden geretourneerd met de termen 'voertuig' of 'cabrio'. 

In Azure Search worden synoniemen gedefinieerd in een *synoniementoewijzing* op basis van *toewijzingsregels* waarmee equivalente termen worden gekoppeld. Deze zelfstudie bevat essentiële stappen voor het toevoegen en gebruiken van synoniemen met een bestaande index. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Synoniemen inschakelen door toewijzingsregels te maken en plaatsen 
> * Verwijzen naar een synoniemtoewijzing in een queryreeks

U kunt meerdere synoniementoewijzingen maken en deze als algemene serviceresource beschikbaar maken voor alle indexen. Daarna kunt u op veldniveau aangeven welke u wilt gebruiken. Bij het uitvoeren van query's doorzoekt Azure Search niet alleen een index, maar wordt er ook in een synoniementoewijzing gekeken als er een is opgegeven in de velden die voor de query zijn gebruikt.

> [!NOTE]
> Synoniemen worden ondersteund in de nieuwste API- en SDK-versie (API-versie=2017-11-11, SDK-versie 5.0.0). Azure Portal biedt er momenteel geen ondersteuning voor. Als u graag Azure Portal-ondersteuning voor synoniemen wilt, kunt u hieronder feedback verzenden via [UserVoice](https://feedback.azure.com/forums/263029-azure-search)

## <a name="prerequisites"></a>Vereisten

Voor de zelfstudie gelden de volgende vereisten:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Search-service](search-create-service-portal.md)
* [Microsoft.Azure.Search .NET-bibliotheek](https://aka.ms/search-sdk)
* [Weten hoe u Azure Search gebruikt via een .NET-toepassing](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Overzicht

Met 'voor en na'-query's wordt de meerwaarde van synoniemen aangetoond. In deze zelfstudie gebruikt u een voorbeeldtoepassing die query's uitvoert en resultaten retourneert in een voorbeeldindex. De voorbeeldtoepassing maakt een kleine index met de naam 'hotels' en vult deze met twee documenten. De toepassing voert zoekquery's uit met termen en zinnen die niet voorkomen in de index. Hierna wordt de synoniemenfunctie ingeschakeld en worden dezelfde zoekopdrachten opnieuw uitgevoerd. In de onderstaande code wordt de algehele stroom gedemonstreerd.

```csharp
  static void Main(string[] args)
  {
      SearchServiceClient serviceClient = CreateSearchServiceClient();

      Console.WriteLine("{0}", "Cleaning up resources...\n");
      CleanupResources(serviceClient);

      Console.WriteLine("{0}", "Creating index...\n");
      CreateHotelsIndex(serviceClient);

      ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

      Console.WriteLine("{0}", "Uploading documents...\n");
      UploadDocuments(indexClient);

      ISearchIndexClient indexClientForQueries = CreateSearchIndexClient();

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Adding synonyms...\n");
      UploadSynonyms(serviceClient);
      EnableSynonymsInHotelsIndex(serviceClient);
      Thread.Sleep(10000); // Wait for the changes to propagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");

      Console.ReadKey();
  }
```
De stappen voor het maken en vullen van de voorbeeldindex worden uitgelegd in [Azure Search gebruiken via een .NET-toepassing](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

## <a name="before-queries"></a>'Voor'-query's

In `RunQueriesWithNonExistentTermsInIndex` voert u zoekquery's uit met 'vijfsterren', 'internet' en 'voordelig EN hotel'.
```csharp
Console.WriteLine("Search the entire index for the phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
Geen van de twee geïndexeerde documenten bevat deze termen, dus wordt de volgende uitvoer gegenereerd met de eerste `RunQueriesWithNonExistentTermsInIndex`.
~~~
Search the entire index for the phrase "five star":

no document matched

Search the entire index for the term 'internet':

no document matched

Search the entire index for the terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a>Synoniemen inschakelen

U kunt synoniemen op basis van twee stappen inschakelen. Als eerste definieert en uploadt u synoniemregels en daarna configureert u velden voor het gebruik van synoniemen. Het proces wordt beschreven in `UploadSynonyms` en `EnableSynonymsInHotelsIndex`.

1. Voeg een synoniemtoewijzing toe aan de Search-service. In `UploadSynonyms` worden vier regels gedefinieerd voor de synoniemtoewijzing desc-synonymmap, die vervolgens naar de service worden geüpload.
```csharp
    var synonymMap = new SynonymMap()
    {
        Name = "desc-synonymmap",
        Format = "solr",
        Synonyms = "hotel, motel\n
                    internet,wifi\n
                    five star=>luxury\n
                    economy,inexpensive=>budget"
    };

    serviceClient.SynonymMaps.CreateOrUpdate(synonymMap);
```
Een synoniemtoewijzing moet voldoen aan de standaard-open-source-indeling `solr`. De indeling wordt in de sectie `Apache Solr synonym format` uitgelegd in [Synoniemen in Azure Search](search-synonyms.md).

2. Configureer doorzoekbare velden voor het gebruik van de synoniemtoewijzing in de indexdefinitie. In `EnableSynonymsInHotelsIndex` worden synoniemen ingeschakeld voor twee velden (`category` en `tags`) door de eigenschap `synonymMaps` in te stellen op de naam van de zojuist geüploade synoniemtoewijzing.
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
Wanneer u een synoniemtoewijzing toevoegt, is het niet nodig om indexen opnieuw op te bouwen. U kunt een synoniemtoewijzing toevoegen aan uw service en daarna de bestaande velddefinities aanpassen in een index om de nieuwe synoniemtoewijzing te gebruiken. Het toevoegen van nieuwe kenmerken heeft geen invloed op de beschikbaarheid van indexen. Ditzelfde is van toepassing op het uitschakelen van synoniemen voor een veld. U kunt eenvoudigweg de eigenschap `synonymMaps` instellen op een lege lijst.
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a>'Na'-query's

Wanneer de synoniemtoewijzing is geüpload en de index is bijgewerkt voor gebruik van de synoniemtoewijzing, wordt met de tweede `RunQueriesWithNonExistentTermsInIndex`-aanroep de volgende uitvoer gegenereerd:

~~~
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
Met de eerste query wordt het document opgehaald uit de regel `five star=>luxury`. Met de tweede query wordt de zoekopdracht uitgebreid met `internet,wifi` en met de derde met zowel `hotel, motel` als `economy,inexpensive=>budget` bij het zoeken naar de overeenkomende documenten.

Door synoniemen toe te voegen, wordt de zoekervaring volledig veranderd. In deze zelfstudie worden met de oorspronkelijke query's geen bruikbare resultaten geretourneerd, hoewel de documenten in de index wel relevant zijn. Door synoniemen in te schakelen, breidt u de index zodanig uit dat deze ook termen bevat die regelmatig worden gebruikt, zonder dat daarbij wijzigingen worden aangebracht aan de onderliggende gegevens in de index.

## <a name="sample-application-source-code"></a>Broncode van een voorbeeldtoepassing
U vindt de volledige broncode van de voorbeeldtoepassing die in deze walkthrough wordt gebruikt, op [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is de resourcegroep met de Azure Search-service te verwijderen. U kunt de resourcegroep nu verwijderen om alles daarin permanent te verwijderen. De naam van de resourcegroep staat in de portal op de pagina Overzicht van de Azure Search-service.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is de [REST-API voor synoniemen](https://aka.ms/rgm6rq) in C#-code gedemonstreerd om toewijzingsregels te maken en plaatsen en de synoniemtoewijzing vervolgens in een query aan te roepen. Aanvullende informatie vindt u in de referentiedocumentatie voor de [.NET-SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) en [REST-API](https://docs.microsoft.com/rest/api/searchservice/).

> [!div class="nextstepaction"]
> [Synoniemen gebruiken in Azure Search](search-synonyms.md)
