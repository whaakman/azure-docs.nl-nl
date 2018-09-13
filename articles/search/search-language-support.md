---
title: Azure Search multi taal indexeren | Microsoft Docs
description: Azure Search ondersteunt 56 talen, door gebruik te maken van taal analyzers van Lucene en Natural Language Processing-technologie van Microsoft.
author: yahnoosh
manager: jlembicz
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: jlembicz
ms.openlocfilehash: 38f93f5415282d2f976d9f3acc2b0a7aeead6c3d
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35914924"
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Een index voor documenten in meerdere talen in Azure Search maken
> [!div class="op_single_selector"]
>
> * [Portal](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

De kracht van taalanalyse unleashing is net zo gemakkelijk als een eigenschap van de instelling op een doorzoekbaar veld van de indexdefinitie. Nu kunt u deze stap in de portal doen.

Hieronder ziet u schermafbeeldingen van de Azure Portal-blades voor Azure Search die toestaan dat gebruikers een indexschema definiëren. Gebruikers kunnen vanaf deze blade, maken alle velden en de eigenschap analyzer ingesteld voor elk van deze.

> [!IMPORTANT]
> U kunt alleen een taalanalyse instellen tijdens de velddefinitie van het, zoals in bij het maken van een nieuwe index vanaf het begin van, of als u een nieuw veld toevoegt aan een bestaande index. Zorg ervoor dat u volledig alle kenmerken, met inbegrip van de analysefunctie voor, tijdens het maken van het veld opgeven. Kunt u zich niet aan de kenmerken bewerken of het type analyzer wijzigen nadat u uw wijzigingen hebt opgeslagen.
>
>

## <a name="define-a-new-field-definition"></a>De velddefinitie van een nieuw definiëren
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en opent u de serviceblade van uw search-service.
2. Klik op **index toevoegen** in de opdrachtbalk aan de bovenkant van het dashboard van de start van een nieuwe index of open een bestaande index voor het instellen van een analyzer voor nieuwe velden die u aan een bestaande index toevoegt.
3. De velden-blade wordt weergegeven, zodat u de opties voor het definiëren van het schema van de index, met inbegrip van het tabblad Analyzer gebruikt voor het kiezen van een taalanalyse.
4. In de velden, start u de velddefinitie van een door een naam geven, het kiezen van het gegevenstype en het instellen van kenmerken aan het veld markeert als volledige tekst kan worden doorzocht, opgehaald in de zoekresultaten, kan worden gebruikt in de navigatiestructuur facet, sorteerbaar, enzovoort.
5. Voordat u doorgaat met het volgende veld, opent u de **Analyzer** tabblad.

![][1]
*Als u wilt een analyzer selecteren, klikt u op het tabblad Analyzer op de blade velden*

## <a name="choose-an-analyzer"></a>Kies een analyzer
1. Ga naar het veld dat u wilt definiëren.
2. Als u het veld als doorzoekbare nog niet hebt gemarkeerd, klikt u op het selectievakje nu als u wilt markeren als **doorzoekbaar**.
3. Klik op het gebied Analyzer om de lijst met beschikbare analyzers weer te geven.
4. Kies de analyzer die u wilt gebruiken.

![][2]
*Selecteer een van de ondersteunde analyzers voor elk veld*

Standaard wordt gebruikt voor alle doorzoekbare velden de [Standard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) welke taalneutraal is. De volledige lijst met ondersteunde analyzers Zie [taalondersteuning in Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Nadat de taal-analysefunctie voor een veld is geselecteerd, wordt deze elke aanvraag indexeren en zoeken voor dat veld gebruikt. Wanneer een query is uitgegeven voor meerdere velden met behulp van verschillende analyzers, wordt de query onafhankelijk worden verwerkt door de juiste analyzers voor elk veld.

Veel web- en mobiele toepassingen dienen gebruikers over de hele wereld met behulp van verschillende talen. Het is mogelijk voor het definiëren van een index voor een scenario als volgt met het maken van een veld voor elke taal die wordt ondersteund.

![][3]
*Definitie van de index met een van beschrijvingsveld voor elke taal die wordt ondersteund*

Als de taal van de agent een query uitvoert bekend is, een search-aanvraag kan worden gericht op een bepaald veld met de **searchFields** queryparameter. De volgende query wordt alleen voor de beschrijving in pools worden verleend:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2017-11-11`

U kunt de index van de portal een query met behulp van **Search explorer** te plakken in een query die vergelijkbaar is met de hierboven weergegeven. Search explorer is beschikbaar in de opdrachtbalk in de serviceblade. Zie [query uitvoeren op uw Azure Search-index in de portal](search-explorer.md) voor meer informatie.

Soms de taal van de agent een query uitvoert niet bekend is, in welk geval de query kan worden uitgegeven op basis van alle velden tegelijk. Indien nodig, voorkeur voor de resultaten in een bepaalde taal kan worden gedefinieerd met behulp van [scoreprofielen](https://msdn.microsoft.com/library/azure/dn798928.aspx). In het onderstaande voorbeeld wordt de overeenkomsten gevonden in de beschrijving in het Engels hoger worden beoordeeld ten opzichte van overeenkomsten in Pools en Frans:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2017-11-11`

Als u een .NET-ontwikkelaar bent, houd er rekening mee dat u kunt taalanalyse met behulp van de [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search). De nieuwste versie bevat ondersteuning voor de taalanalyse Microsoft.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
