---
title: Een index maken (portal - Azure Search) | Microsoft Docs
description: Een index maken met Azure Portal.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: quickstart
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: ea05cd34e5ba3f55b2f5400f1252f729401c5d6a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Een Azure Search-index maken met Azure Portal
> [!div class="op_single_selector"]
> * [Overzicht](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Gebruik de ingebouwde indexdesigner in Azure Portal om een prototype van een zoekindex of een echte [zoekindex](search-what-is-an-index.md) te maken die in uw Azure Search-service kan worden uitgevoerd. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u beschikt over een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) en [Azure Search-service](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Uw zoekservice vinden
1. Meld u aan bij de pagina van Azure Portal en controleer de [zoekservices voor uw abonnement](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Selecteer uw Azure Search-service.

## <a name="name-the-index"></a>De index een naam geven

1. Klik op de knop **Index toevoegen** in de opdrachtbalk bovenaan de pagina.
2. Geef uw Azure Search-index een naam. 
   * Begin met een letter.
   * Gebruik alleen kleine letters, cijfers of streepjes (-).
   * Zorg ervoor dat de naam maximaal 60 tekens bevat.

  De naam van de index wordt een onderdeel van de eindpunt-URL die wordt gebruikt om verbinding te maken met de index en om HTTP-aanvragen in de REST API voor Azure Search te verzenden.

## <a name="define-the-fields-of-your-index"></a>De velden van uw index definiëren

De index bevat onder andere een *Veldenverzameling* die de doorzoekbare gegevens in uw index definieert. Om precies te zijn, geeft deze de structuur aan van de documenten die u afzonderlijk uploadt. De verzameling Velden bevat de vereiste en optionele velden, met een naam en type en met indexkenmerken, zodat u kunt bepalen hoe het veld kan worden gebruikt.

1. Op de blade **Index toevoegen** klikt u op **Velden >** om de blade voor velddefinities open te schuiven. 

2. Accepteer het gegenereerde *sleutel*veld van het type Edm.String. De standaardnaam van het veld is *id*, maar u kunt de naam wijzigen, zolang de tekenreeks maar voldoet aan de [naamgevingsregels](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Voor elke Azure Search-index is een sleutelveld verplicht en het moet een tekenreeks zijn.

3. Voeg velden toe om een volledige beschrijving te geven van de documenten die u gaat uploaden. Als documenten bestaan uit een *id*, *hotelnaam*, *adres*, *plaats* en *regio*, maakt u een corresponderend veld voor elk kenmerk in de index. Lees de [ontwerprichtlijnen in de sectie hieronder](#design) voor hulp bij het instellen van kenmerken.

4. Desgewenst kunt u velden toevoegen die intern worden gebruikt in filterexpressies. Kenmerken in het veld kunnen zo worden ingesteld dat velden worden uitgesloten van zoekbewerkingen.

5. Als u klaar bent, klikt u op **OK** om de index op te slaan en te maken.

## <a name="tips-for-adding-fields"></a>Tips voor het toevoegen van velden

Voor het maken van een index in de portal moet u uw toetsenbord veel gebruiken. Aan de hand van deze werkstroom kunt u het aantal stappen tot een minimum beperken:

1. Eerst maakt u een lijst met velden door namen in te voeren en gegevenstypen in te stellen.

2. Daarna gebruikt u de selectievakjes boven elk kenmerk om de instelling voor alle velden bulkgewijs in te schakelen en schakelt u de vakjes uit voor de paar velden waarvoor u de instelling niet wilt inschakelen. Tekenreeksvelden zijn bijvoorbeeld meestal doorzoekbaar. Zo kunt u klikken op **Ophalen mogelijk** en **Doorzoekbaar** om zowel de waarden van het veld in de zoekresultaten te retourneren als het zoeken in de volledige tekst van het veld toe te staan. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Ontwerprichtlijnen voor het instellen van kenmerken

Hoewel u op elk gewenst moment nieuwe velden kunt toevoegen, worden bestaande velddefinities voor de hele levensduur van de index vergrendeld. Daarom gebruiken ontwikkelaars de portal doorgaans om eenvoudige indexen te maken, om ideeën uit te testen of om een instelling op te zoeken met behulp van de portalpagina's. Een frequente iteratie van een index-ontwerp is efficiënter als u een op code gebaseerde benadering hanteert, zodat u uw index eenvoudig kunt herbouwen.

Voordat de index wordt opgeslagen, worden er analyses en suggesties aan velden gekoppeld. Klik op alle tabbladen om taalanalyses of suggesties aan uw indexdefinitie toe te voegen.

Tekenreeksvelden zijn vaak gemarkeerd als **Doorzoekbaar** en **Ophalen mogelijk**.

Velden die worden gebruikt om zoekopdrachten te verfijnen, zijn onder andere **Sorteerbaar**, **Filteren mogelijk** en **Facetten mogelijk**.

Veldkenmerken bepalen hoe een veld wordt gebruikt, bijvoorbeeld of het wordt gebruikt voor zoeken in volledige tekst, facetnavigatie, sorteerbewerkingen, enzovoort. In de volgende tabel wordt elk kenmerk beschreven.

|Kenmerk|Beschrijving|  
|---------------|-----------------|  
|**doorzoekbaar**|Zoeken in volledige tekst mogelijk, onderworpen aan lexicale analyse, zoals het afbreken van woorden tijdens het indexeren. Als u een doorzoekbaar veld instelt op een waarde als 'zonnige dag', wordt de waarde intern gesplitst in de afzonderlijke tokens 'zonnige' en 'dag'. Zie [Hoe zoeken in de volledige tekst werkt](search-lucene-query-architecture.md) voor meer informatie.|  
|**filteren mogelijk**|Hier wordt naar verwezen in **$filter**-query's. Bij filterbare velden van het type `Edm.String` of `Collection(Edm.String)` worden woorden niet afgebroken, dus vergelijkingen gelden alleen voor exacte overeenkomsten. Als u zo'n veld bijvoorbeeld instelt op 'zonnige dag', worden er met `$filter=f eq 'sunny'` geen overeenkomsten gevonden, maar met `$filter=f eq 'sunny day'` wel. |  
|**sorteerbaar**|Het systeem sorteert de resultaten standaard op score, maar u kunt het sorteren configureren op basis van velden in de documenten. Velden van het type `Collection(Edm.String)` kunnen niet **sorteerbaar** zijn. |  
|**facetable**|Wordt doorgaans gebruikt bij een weergave van zoekresultaten met een treffertelling per categorie (bijvoorbeeld hotels in een specifieke stad). Deze optie kan niet worden gebruikt bij velden van het type `Edm.GeographyPoint`. Velden van het type `Edm.String` die **filterbaar** of **sorteerbaar** zijn, of waarvoor **Facetten mogelijk** zijn, kunnen een lengte hebben van maximaal 32 kB. Zie [Index maken (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie.|  
|**sleutel**|Unieke id voor documenten binnen de index. Er moet precies één veld worden uitgekozen als sleutelveld. Dit veld moet van het type `Edm.String` zijn.|  
|**retrievable**|Hiermee bepaalt u of het veld in een zoekresultaat kan worden geretourneerd. Dit is handig als u een veld (zoals *winstmarge*) wilt gebruiken als filter-, sorteer- of scoremechanisme, maar niet wilt dat het veld zichtbaar is voor de eindgebruiker. Dit kenmerk moet `true` zijn voor `key`-velden.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>De index hotels maken die in de secties van de voorbeeld-API’s wordt gebruikt

API-documentatie voor Azure Search bevat voorbeelden van code met een eenvoudige index *hotels*. In de schermopnamen hieronder kunt u de indexdefinitie zien, inclusief de Franse taalanalyse die tijdens de indexdefinitie is genoemd. Deze kunt u bij wijze van oefening opnieuw maken in de portal.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Volgende stappen

Nadat u een Azure Search-index hebt gemaakt, kunt u doorgaan met de volgende stap: [het uploaden van doorzoekbare gegevens in de index](search-what-is-data-import.md).

U kunt er ook voor kiezen om meer over indexen te leren. Naast de Veldenverzameling bevat een index ook analyses, suggesties, scoreprofielen en CORS-instellingen. De portal biedt tabbladen voor het definiëren van de meest voorkomende elementen: velden, analyses en suggesties. Als u andere elementen wilt maken of wijzigen, kunt u de REST API of de .NET SDK gebruiken.

## <a name="see-also"></a>Zie ook

 [Hoe zoeken in de volledige tekst werkt](search-lucene-query-architecture.md)  
 [REST API voor Search-service](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

