---
title: Een Azure Search-index maken in Azure portal - Azure Search
description: Informatie over het maken van een index voor Azure Search met behulp van een ingebouwde portal index ontwerpfuncties.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6a2bac71c37cc750eb24e3492ecdcdf0b2333cce
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338832"
---
# <a name="create-an-azure-search-index-in-the-portal"></a>Een Azure Search-index maken in de portal

Azure Search bevat een ingebouwde index designer in de portal handig voor prototypen of maken een [search-index](search-what-is-an-index.md) die worden gehost op uw Azure Search-service. Het hulpprogramma wordt gebruikt voor de bouw schema. Wanneer u de definitie van de opslaat, wordt een lege index wordt volledig uitgedrukt in Azure Search. Hoe u deze laden met doorzoekbare gegevens is aan u.

De indexdesigner is slechts één methode voor het maken van een index. Via een programma, kunt u een index met behulp van de [.NET](search-create-index-dotnet.md) of [REST](search-create-index-rest-api.md) API's.

## <a name="start-index-designer"></a>Indexdesigner te starten

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en open het servicedashboard. Klik in de koppelingsbalk op **Alle services** als u in het huidige abonnement naar bestaande 'zoekservices' wilt zoeken. 

2. Klik op de **index toevoegen** koppeling in de opdrachtbalk aan de bovenkant van de pagina.

   ![Index koppeling toevoegen in de opdrachtbalk](media/search-create-index-portal/add-index.png "index koppeling toevoegen in de opdrachtbalk")

3. Geef uw Azure Search-index een naam. Namen van de index wordt verwezen in indexeren en query-bewerkingen. De naam van de index wordt een onderdeel van de eindpunt-URL die wordt gebruikt om verbinding te maken met de index en om HTTP-aanvragen in de REST API voor Azure Search te verzenden.

   * Begin met een letter.
   * Gebruik alleen kleine letters, cijfers of streepjes (-).
   * Zorg ervoor dat de naam maximaal 60 tekens bevat.

## <a name="add-fields"></a>Velden toevoegen

De index bevat onder andere een *Veldenverzameling* die de doorzoekbare gegevens in uw index definieert. De Veldenverzameling helemaal, Hiermee geeft u de structuur van documenten die u afzonderlijk uploadt. Een Veldenverzameling bevat de vereiste en optionele velden, met de naam en type en met indexkenmerken die bepalen hoe het veld kan worden gebruikt.

1. Velden toevoegen om aan te geven de documenten die u uploadt, instellen van een [gegevenstype](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor elk criterium. Bijvoorbeeld, als documenten bestaan uit een *hotel-id*, *hotelnaam*, *adres*, *plaats*, en *regio*, een corresponderend veld voor elk kenmerk in de index te maken. Controleer de [ontwerprichtlijnen in de onderstaande sectie](#design) voor hulp bij het instellen van kenmerken.

2. Geef een *sleutel* veld van het type Edm.String. Waarden voor dit veld moeten een unieke identificatie van elk document. De standaardnaam van het veld is *id*, maar u kunt de naam wijzigen, zolang de tekenreeks maar voldoet aan de [naamgevingsregels](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Bijvoorbeeld, als uw Veldenverzameling bevat *hotel-id*, kiest u die voor uw sleutel. Voor elke Azure Search-index is een sleutelveld verplicht en het moet een tekenreeks zijn.

3. Kenmerken instellen voor elk veld. De indexdesigner niet van toepassing op kenmerken die ongeldig voor het gegevenstype zijn, maar niet voorstellen wat moet worden opgenomen. Bekijk de instructies in de volgende sectie om te begrijpen wat de kenmerken voor zijn.

    API-documentatie voor Azure Search bevat voorbeelden van code met een eenvoudige index *hotels*. In de onderstaande schermafbeelding ziet u de definitie van de index, met inbegrip van de Franse taalanalyse die tijdens de indexdefinitie die u kunt het opnieuw maken als een oefening in de portal.

    ![Hotels demo index](media/search-create-index-portal/field-definitions.png "Hotels demo index")

4. Wanneer u klaar bent, klikt u op **maken** opslaan en de index te maken.

<a name="design"></a>

## <a name="set-attributes"></a>Kenmerken instellen

Hoewel u op elk gewenst moment nieuwe velden kunt toevoegen, worden bestaande velddefinities voor de hele levensduur van de index vergrendeld. Daarom gebruiken ontwikkelaars de portal doorgaans om eenvoudige indexen te maken, om ideeën uit te testen of om een instelling op te zoeken met behulp van de portalpagina's. Een frequente iteratie van een index-ontwerp is efficiënter als u een op code gebaseerde benadering hanteert, zodat u uw index eenvoudig kunt herbouwen.

Voordat de index wordt opgeslagen, worden er analyses en suggesties aan velden gekoppeld. Zorg ervoor dat taalanalyses of suggesties aan uw indexdefinitie toevoegen, terwijl u deze maakt.

Tekenreeksvelden zijn vaak gemarkeerd als **Doorzoekbaar** en **Ophalen mogelijk**. Velden die worden gebruikt om zoekopdrachten te verfijnen, zijn onder andere **Sorteerbaar**, **Filteren mogelijk** en **Facetten mogelijk**.

Veldkenmerken bepalen hoe een veld wordt gebruikt, bijvoorbeeld of het wordt gebruikt voor zoeken in volledige tekst, facetnavigatie, sorteerbewerkingen, enzovoort. In de volgende tabel wordt elk kenmerk beschreven.

|Kenmerk|Description|  
|---------------|-----------------|  
|**doorzoekbaar**|Zoeken in volledige tekst mogelijk, onderworpen aan lexicale analyse, zoals het afbreken van woorden tijdens het indexeren. Als u een doorzoekbaar veld instelt op een waarde als 'zonnige dag', wordt de waarde intern gesplitst in de afzonderlijke tokens 'zonnige' en 'dag'. Zie [Hoe zoeken in de volledige tekst werkt](search-lucene-query-architecture.md) voor meer informatie.|  
|**filteren mogelijk**|Hier wordt naar verwezen in **$filter**-query's. Bij filterbare velden van het type `Edm.String` of `Collection(Edm.String)` worden woorden niet afgebroken, dus vergelijkingen gelden alleen voor exacte overeenkomsten. Als u zo'n veld bijvoorbeeld instelt op 'zonnige dag', worden er met `$filter=f eq 'sunny'` geen overeenkomsten gevonden, maar met `$filter=f eq 'sunny day'` wel. |  
|**sorteerbaar**|Het systeem sorteert de resultaten standaard op score, maar u kunt het sorteren configureren op basis van velden in de documenten. Velden van het type `Collection(Edm.String)` kunnen niet **sorteerbaar** zijn. |  
|**facetable**|Wordt doorgaans gebruikt bij een weergave van zoekresultaten met een treffertelling per categorie (bijvoorbeeld hotels in een specifieke stad). Deze optie kan niet worden gebruikt bij velden van het type `Edm.GeographyPoint`. Velden van het type `Edm.String` die **filterbaar** of **sorteerbaar** zijn, of waarvoor **Facetten mogelijk** zijn, kunnen een lengte hebben van maximaal 32 kB. Zie [Index maken (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie.|  
|**sleutel**|Unieke id voor documenten binnen de index. Er moet precies één veld worden uitgekozen als sleutelveld. Dit veld moet van het type `Edm.String` zijn.|  
|**retrievable**|Hiermee bepaalt u of het veld in een zoekresultaat kan worden geretourneerd. Dit is handig als u een veld (zoals *winstmarge*) wilt gebruiken als filter-, sorteer- of scoremechanisme, maar niet wilt dat het veld zichtbaar is voor de eindgebruiker. Dit kenmerk moet `true` zijn voor `key`-velden.|  

## <a name="next-steps"></a>Volgende stappen

Nadat u een Azure Search-index hebt gemaakt, kunt u doorgaan met de volgende stap: [het uploaden van doorzoekbare gegevens in de index](search-what-is-data-import.md).

U kunt ook u kunt ook rekening houden met een [stil indexen](search-what-is-an-index.md). Naast de Veldenverzameling bevat een index ook analyses, suggesties, scoreprofielen en CORS-instellingen. De portal biedt tabbladen voor het definiëren van de meest voorkomende elementen: Velden, analyses en suggesties. Als u andere elementen wilt maken of wijzigen, kunt u de REST API of de .NET SDK gebruiken.

## <a name="see-also"></a>Zie ook

 [Hoe zoeken in de volledige tekst werkt](search-lucene-query-architecture.md)  
 [REST API voor Search-service](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

