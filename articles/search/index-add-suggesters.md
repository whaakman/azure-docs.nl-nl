---
title: Suggesties toevoegen aan een Azure Search-index
description: Hiermee kunt velden voor Queryacties, automatisch aangevulde, waarbij de voorgestelde query's worden samengesteld uit de tekst uit velden in een Azure Search-index.
ms.date: 02/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: fd4b29134fd45ed2888fbc81ded413ecf7286959
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308649"
---
# <a name="add-suggesters-to-an-azure-search-index"></a>Suggesties toevoegen aan een Azure Search-index

Een **suggestie** is een Azure Search-constructie ondersteuning van de 'search-als-u-type" [suggesties](https://docs.microsoft.com/rest/api/searchservice/suggestions) functie en de [automatisch aanvullen (preview)](search-autocomplete-tutorial.md) functie. Voordat u de suggesties-API aanroepen kunt, moet u definiëren een **suggestie** in een index om in te schakelen suggesties op specifieke velden.

Hoewel een **suggestie** heeft een aantal eigenschappen, is het vooral een verzameling van velden waarvoor u inschakelt de [suggesties API](https://docs.microsoft.com/rest/api/searchservice/suggestions). Bijvoorbeeld, een reis-app mogelijk wilt inschakelen typeahead zoeken op bestemmingen, plaatsen en bezienswaardigheden. Daarom heengaat alle drie de velden in de veldverzameling.

U kunt slechts één hebben **suggestie** resource voor elke index (met name een **suggestie** in de **suggesties** verzameling).

## <a name="creating-a-suggester"></a>Het maken van een suggestie

U kunt maken een **suggestie** op elk gewenst moment, maar de impact op uw index varieert op basis van de velden.

+ Nieuwe velden worden toegevoegd aan een suggestie als onderdeel van dezelfde update worden de minste indrukwekkende in dat er geen indexen vereist is.
+ Bestaande velden toegevoegd aan een suggestie echter Hiermee wijzigt u de definitie van het veld, blijkt volledig opnieuw opbouwen van de index.

**Suggesties** werken het beste wanneer specifieke documenten in plaats van losse termen of zinnen voorstellen gebruikt. De beste kandidaat velden zijn titels, namen en andere relatief korte zinnen die een item kunnen identificeren. Minder effectief zijn terugkerende velden, zoals categorieën en -tags, of zeer lange velden, zoals velden met beschrijvingen of opmerkingen hebt.

Nadat een suggestie is gemaakt, voegt u toe de [suggesties API](https://docs.microsoft.com/rest/api/searchservice/suggestions) in uw querylogica voor het aanroepen van de functie.

Eigenschappen die definieert een **suggestie** zijn onder andere het volgende:

|Eigenschap|Description|
|--------------|-----------------|
|`name`|De naam van de **suggestie**. Gebruik van de naam van de **suggestie** bij het aanroepen van de [suggesties &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/suggestions).|
|`searchMode`|De strategie gebruikt om te zoeken naar kandidaat zinnen. De enige modus die momenteel niet ondersteund is `analyzingInfixMatching`, die wordt uitgevoerd met het flexibele overeenkomende woordgroepen aan het begin of in het midden van zinnen.|
|`sourceFields`|Een lijst met een of meer velden die de bron van de inhoud voor suggesties. Alleen de velden van het type `Edm.String` en `Collection(Edm.String)` mogelijk bronnen voor suggesties. Alleen velden waarvoor een aangepaste taalanalyse ingesteld niet kunnen worden gebruikt. |

## <a name="suggester-example"></a>Voorbeeld van de suggestie
Een **suggestie** maakt deel uit van de indexdefinitie. Slechts één **suggestie** kan bestaan in de **suggesties** verzameling in de huidige versie, naast de **velden** verzameling en **scoringProfiles**.

```
{
  "name": "hotels",
  "fields": [
    . . .
  ],
  "suggesters": [
    {
      "name": "sg",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["hotelName", "category"]
    }
  ],
  "scoringProfiles": [
    . . .
  ]
}

```

## <a name="see-also"></a>Zie ook
[Index maken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  
[Index bijwerken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/update-index)  
[Suggesties &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/suggestions)  
[Index-bewerkingen &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/index-operations)  
[Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/)  
[Azure Search .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)
