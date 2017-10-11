---
title: Maken van een index (portal - Azure Search) | Microsoft Docs
description: Een index maken met de Azure-Portal.
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: a7d98ab0937a7d3f932d5df34c19ae091129804e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Een Azure Search-index met de Azure Portal maken
> [!div class="op_single_selector"]
> * [Overzicht](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Gebruik de ontwerpfunctie ingebouwde index in Azure-portal als prototype of maak een [search-index](search-what-is-an-index.md) uit te voeren op uw Azure Search-service. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) en [Azure Search-service](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Uw search-service zoeken
1. Aanmelden bij de Azure portal-pagina en bekijk de [zoeken services voor uw abonnement](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Selecteer uw Azure Search-service.

## <a name="name-the-index"></a>Naam van de index

1. Klik op de **toevoegen index** knop in de opdrachtbalk boven aan de pagina.
2. Naam van uw Azure Search-index. 
   * Met een letter beginnen.
   * Gebruik alleen kleine letters, getallen of streepjes ('-').
   * De naam om 60 tekens te beperken.

  De naam van de index maakt deel uit van de eindpunt-URL gebruikt voor verbindingen naar de index en voor de verzending van HTTP-aanvragen in de Azure Search REST-API.

## <a name="define-the-fields-of-your-index"></a>De velden van de index definiëren

Index samenstelling bevat een *verzameling velden* die doorzoekbare gegevens in uw index definieert. Meer specifiek, bevat deze de structuur van documenten die u afzonderlijk kunt uploaden. De verzameling van velden bevat de vereiste en optionele velden, met de naam en hebt getypt, met indexkenmerken om te bepalen hoe het veld kan worden gebruikt.

1. In de **Index toevoegen** blade, klikt u op **velden >** op dia open de blade van de definitie van veld. 

2. Accepteer de gegenereerde *sleutel* veld van het type Edm.String. Standaard wordt het veld naam *id* , maar u kunt de naam wijzigen als de tekenreeks voldoet aan [naamgevingsregels](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Het veld met een sleutel is verplicht voor elke Azure Search-index en het moet een tekenreeks.

3. Toevoegen van velden om op te geven volledig de documenten die u zult uploaden. Als u documenten bestaan uit een *id*, *naam hotel*, *adres*, *stad*, en *regio*, maak een overeenkomende veld voor elk criterium in de index. Controleer de [ontwerpen richtlijnen in de onderstaande sectie](#design) voor hulp bij het instellen van kenmerken.

4. Voeg alle velden die worden gebruikt intern desgewenst in filterexpressies. Kenmerken op het veld kunnen worden ingesteld op de velden uitsluiten van zoekopdrachten.

5. Wanneer u klaar bent, klikt u op **OK** opslaan en de index maken.

## <a name="tips-for-adding-fields"></a>Tips voor het toevoegen van velden

Maken van een index in de portal is intensieve toetsenbord. Stappen minimaliseren door deze werkstroom te volgen:

1. Bouw de lijst met velden eerst door te voeren namen en instellen van gegevenstypen.

2. Vervolgens, gebruik de selectievakjes boven aan elk kenmerk bulksgewijs inschakelen van de instelling voor alle velden en selectief Schakel vervolgens de selectievakjes voor de enkele velden waarvoor dit niet. Tekenreeksvelden zijn bijvoorbeeld meestal doorzoekbaar. Zo kunt u klikken op **ophalen mogelijk** en **doorzoekbaar** tot zowel de waarden van het veld in de zoekresultaten te retourneren, evenals zoeken in volledige tekst in het veld toestaan. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Richtlijnen voor het instellen van kenmerken

Hoewel u nieuwe velden op elk gewenst moment toevoegen kunt, worden bestaande velddefinities vergrendeld voor de levensduur van de index. Om deze reden ontwikkelaars doorgaans de portal gebruiken voor eenvoudige indexen te maken, testen ideeën of met behulp van de portal-pagina's voor het opzoeken van een instelling. Veelvuldig herhaling via een ontwerp voor de index is efficiënter als u een code gebaseerde aanpak volgt zodat u de index eenvoudig kunt herstellen.

Analyzers en suggestiefunctie zijn gekoppeld aan velden voordat de index wordt opgeslagen. Zorg ervoor dat door elke tabbladen pagina taalanalyse of suggestiefunctie toevoegen aan de indexdefinitie van de te klikken.

Tekenreeksvelden zijn vaak gemarkeerd als **doorzoekbaar** en **ophalen mogelijk**.

Dit zijn enkele velden die worden gebruikt om zoekresultaten te verfijnen **sorteerbaar**, **Filterable**, en **geschikt voor facetten**.

Veldkenmerken bepalen hoe een veld wordt gebruikt, zoals of deze wordt gebruikt in een zoekopdracht in volledige tekst, facetnavigatie, sorteerbewerkingen, enzovoort. De volgende tabel beschrijft elk kenmerk.

|Kenmerk|Beschrijving|  
|---------------|-----------------|  
|**doorzoekbare**|Volledige-tekstindex doorzoekbare onderworpen aan lexicale analysis zoals woordafbreking tijdens het indexeren. Als u een doorzoekbaar veld ingesteld op een waarde zoals 'mooi day', intern dit verdeeld in de afzonderlijke tokens 'mooi' en 'day'. Zie voor meer informatie [hoe volledige tekst zoeken works](search-lucene-query-architecture.md).|  
|**Filterbaar**|Waarnaar wordt verwezen in **$filter** query's. Filterbaar velden van het type `Edm.String` of `Collection(Edm.String)` niet worden bewerkt woordafbreking, zodat vergelijkingen zijn voor exact overeenkomt met alleen. Bijvoorbeeld, als u zo'n veld f om 'mooi day' instellen `$filter=f eq 'sunny'` wordt geen overeenkomsten gevonden maar `$filter=f eq 'sunny day'` wordt. |  
|**sorteerbaar**|Het systeem worden standaard resultaten gesorteerd op score, maar u kunt sorteren op basis van velden in de documenten kunt configureren. Velden van het type `Collection(Edm.String)` kan niet worden **sorteerbaar**. |  
|**geschikt voor facetten**|Doorgaans gebruikt in een weergave van zoekresultaten met een aantal treffers per categorie (bijvoorbeeld hotels in Haarlem). Deze optie kan niet worden gebruikt met velden van het type `Edm.GeographyPoint`. Velden van het type `Edm.String` die zijn **Filterbaar**, **sorteerbaar**, of **geschikt voor facetten** kan niet groter zijn dan 32 kB lang. Zie voor meer informatie [Create Index (REST-API)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**sleutel**|Unieke id voor documenten in de index. Het sleutelveld moet precies één veld worden gekozen en moet van het type `Edm.String`.|  
|**ophalen mogelijk**|Hiermee wordt bepaald of het veld in een zoekresultaat kan worden geretourneerd. Dit is handig als u wilt gebruiken een veld (zoals *winstmarge*) als een filter wilt sorteren of score berekenen mechanisme, maar niet wilt dat het veld zichtbaar zijn voor de eindgebruiker. Dit kenmerk moet `true` voor `key` velden.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>De index hotels gebruikt in de voorbeeld-API-secties maken

Documentatie voor Azure Search API bevat voorbeelden van code met een eenvoudige *hotels* index. In de onderstaande schermafbeeldingen ziet u de indexdefinitie, met inbegrip van de Franse taal analyzer opgegeven tijdens de definitie van de index, die u kunt opnieuw maken als een oefening in de portal.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Volgende stappen

Nadat een Azure Search-index is gemaakt, kunt u in de volgende stap: [doorzoekbare gegevens uploaden naar de index](search-what-is-data-import.md).

U kunt ook u kan ook rekening houden met uitvoerig stil op indexen. Naast de verzameling van velden bevat een index ook analyzers, suggestiefunctie scoreprofiel profielen en CORS-instellingen. De portal biedt tabpagina's voor het definiëren van de meest voorkomende elementen: velden, analyzers en suggestiefunctie. Als u wilt maken of wijzigen van andere elementen, kunt u de REST-API of de .NET SDK.

## <a name="see-also"></a>Zie ook

 [Hoe zoeken in de volledige tekst werkt](search-lucene-query-architecture.md)  
 [Search-service REST-API](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

