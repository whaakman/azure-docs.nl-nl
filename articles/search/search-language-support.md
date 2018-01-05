---
title: Taal van de meerdere Azure Search | Microsoft Docs
description: Azure Search biedt ondersteuning voor 56 talen gebruik taalanalyse van Lucene en het verwerken van natuurlijke taal-technologie van Microsoft.
services: search
documentationcenter: 
author: yahnoosh
manager: pablocas
editor: 
ms.assetid: 55a00b44-804d-41bb-9c96-e6ea498616f5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: jlembicz
ms.openlocfilehash: dbbab31bac66ce73dbf9883992713a2c16581e19
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Een index voor documenten in meerdere talen in Azure Search maken
> [!div class="op_single_selector"]
>
> * [Portal](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

De kracht van taalanalyse unleashing is net zo eenvoudig als een eigenschap van de instelling voor een doorzoekbaar veld in de definitie van de index. Nu kunt u deze stap in de portal doen.

Hieronder vindt u schermafbeeldingen van de Azure Portal-blades voor Azure Search die toestaan dat gebruikers een indexschema definiëren. Gebruikers kunnen via deze blade alle velden maken en stel de eigenschap analyzer voor elk van deze.

> [!IMPORTANT]
> U kunt alleen een taalanalyse ingesteld tijdens de definitie van veld als in bij het maken van een nieuwe index van de grond van of als u een nieuw veld toevoegt aan een bestaande index. Zorg ervoor dat u alle kenmerken, zoals de analyzer tijdens het maken van het veld volledig opgeven. Het niet mogelijk zijn de kenmerken bewerken of wijzigen van het type analyzer nadat u uw wijzigingen hebt opgeslagen.
>
>

## <a name="define-a-new-field-definition"></a>De velddefinitie van een nieuw definiëren
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en open de service-blade van uw zoekservice.
2. Klik op **toevoegen index** in de opdrachtbalk aan de bovenkant van het servicedashboard start een nieuwe index of open een bestaande index om in te stellen van een analyzer op nieuwe velden die u aan een bestaande index toevoegt.
3. De velden blade wordt weergegeven, zodat u de opties voor het definiëren van het schema van de index, met inbegrip van het tabblad Analyzer gebruikt voor het kiezen van een taalanalyse.
4. In velden, start u de velddefinitie van een door een naam geven, het kiezen van het gegevenstype en instellen van kenmerken markeert het veld als volledige tekst kan worden doorzocht, worden opgehaald in de zoekresultaten, kan worden gebruikt in de navigatiestructuur facet, sorteerbaar, enzovoort.
5. Voordat u doorgaat naar het volgende veld, opent u de **Analyzer** tabblad.

![][1]
*Als u wilt een analyzer selecteert, klikt u op het tabblad Analyzer op de blade velden*

## <a name="choose-an-analyzer"></a>Kies een analyzer
1. Ga naar het veld dat u definieert.
2. Als u het veld als doorzoekbaar nog niet hebt gemarkeerd, klikt u op het selectievakje nu om verder te markeren als **doorzoekbaar**.
3. Klik op het gebied van de analysefunctie voor om de lijst met beschikbare analyzers weer te geven.
4. Kies de analyzer die u wilt gebruiken.

![][2]
*Selecteer een van de ondersteunde analyzers voor elk veld*

De doorzoekbare velden gebruikt standaard de [standaard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) welke taal networkdirect is. De volledige lijst met ondersteunde analyzers Zie [taalondersteuning in Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Nadat de taalanalyse voor een veld is geselecteerd, wordt deze aan elke aanvraag indexeren en de zoekcriteria voor dat veld gebruikt. Wanneer een query op basis van meerdere velden met behulp van verschillende analyzers is uitgegeven, wordt de query onafhankelijk worden verwerkt door de juiste analyzers voor elk veld.

Veel webtoepassingen en mobiele toepassingen fungeren gebruikers overal ter wereld met behulp van verschillende talen. Het is mogelijk voor het definiëren van een index voor een scenario als volgt door het maken van een veld voor elke taal die wordt ondersteund.

![][3]
*Definitie van de index met een beschrijvingsveld voor elke taal ondersteund*

Als de taal van de agent een query uitvoert bekend is, een zoekaanvraag kan worden gericht op een bepaald veld met de **searchFields** queryparameter. De volgende query worden uitgegeven alleen op basis van de beschrijving in pools:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

U kunt uw index van de portal een query met **Search explorer** te plakken in een query die lijkt op de hierboven weergegeven. Search explorer is beschikbaar in de opdrachtbalk in de blade van de service. Zie [query uitvoeren op uw Azure Search-index in de portal](search-explorer.md) voor meer informatie.

Soms de taal van de agent een query uitvoert niet bekend is, in welk geval de query kan worden uitgegeven op basis van alle velden tegelijkertijd. Indien nodig, de voorkeur voor de resultaten in een bepaalde taal kan worden gedefinieerd met behulp van [score berekenen voor profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx). In het onderstaande voorbeeld overeenkomsten gevonden in de beschrijving in het Engels score hoger ten opzichte van de overeenkomsten in Pools en Frans:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

Als u een .NET-ontwikkelaar bent, houd er rekening mee dat u kunt configureren dat taalanalyse met behulp van de [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search). De meest recente versie biedt ondersteuning voor de taalanalyse Microsoft.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
