---
title: Gebruik en weergavevereisten voor de Bing zoeken-API 's
titleSuffix: Azure Cognitive Services
description: De vereisten voor het weergeven van de zoekresultaten van de Bing zoeken-API's in uw toepassingen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: 5575668f164b97142e7c4b2ddb2608c3173426a6
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738722"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Vereisten voor gebruik en weergave van Bing Zoeken-API's

Deze vereisten voor gebruik en weergave van toepassing op elke uitvoering van de inhoud en de bijbehorende gegevens uit de volgende Bing zoeken-API's, met inbegrip van relaties, metagegevens en andere signalen.

- Bing Aangepaste zoekopdrachten
- Bing Entiteiten zoeken
- Bing Afbeeldingen zoeken
- Bing Nieuws zoeken
- Bing Video's zoeken
- Bing Visuele zoekopdrachten
- Bing Webzoekopdrachten
- Bing Spellingcontrole
- Bing Automatische suggesties

## <a name="definitions"></a>Definities


|Termijn  |Description  |
|---------|---------|
|Antwoord     | Een categorie van de resultaten in een antwoord geretourneerd. Een antwoord van de Bing webzoekopdrachten-API kan bijvoorbeeld antwoorden bevatten in de categorieën van webpagina-resultaten, afbeelding, video, visueel element en nieuws. |
|Antwoord     | Alle antwoorden en de bijbehorende gegevens ontvangen in antwoord op één aanroep aan een Search-API. |
|Resultaat    | Een item van gegevens in een antwoord. De set gegevens die zijn verbonden met een enkele nieuwsartikel is bijvoorbeeld een resultaat in een nieuwsantwoord. |
|Zoek-API's    | gezamenlijk de Bing Custom Search, entiteiten zoeken, afbeeldingen zoeken, nieuws zoeken, video's zoeken, visuele zoekopdrachten, lokale bedrijven zoeken en Web zoeken-API's. |

## <a name="bing-spell-check-and-bing-autosuggest-api-restrictions"></a>Bing Spell Check en Automatische suggestie-API-beperkingen

Niet doen:

- Kopiëren, opslaan of alle gegevens die u van de Bing Spell Check- of Bing Automatische suggestie-API's ontvangt in de cache.
- Gegevens die u van Bing Spell Check- of Bing Automatische suggestie-API's als onderdeel van een machine learning- of vergelijkbare algoritmische activiteit ontvangt gebruiken. Gebruik deze gegevens niet te trainen, evalueren of nieuwe of bestaande services die worden geboden door u of van derden mogelijk te verbeteren.

## <a name="bing-search-apis"></a>Bing Zoeken-API's

> [!NOTE]
> De vereisten in deze sectie zijn van toepassing op alleen de zoeken-API's, wat exclusief Bing Spell Check- of Automatische suggestie voor Bing. 

### <a name="internet-search-experience-requirements"></a>Vereisten voor Internet zoeken ervaring

Alle gegevens die worden geretourneerd in antwoorden kan alleen worden gebruikt in internet zoekervaringen. Een internet-zoekervaring betekent dat de inhoud die wordt weergegeven: 

- Relevante en reageren op van de eindgebruiker directe query of een andere vermelding van hun interesse zoeken en voor welk doel (bijvoorbeeld een gebruiker aangegeven zoekquery) is. 

- Helpt gebruikers vinden en navigeer naar de gegevensbronnen van het antwoord. Bijvoorbeeld, biedt geklikt koppelingen van hyperlinks in het antwoord.

- Bevat verschillende resultaten voor de gebruiker te selecteren in. 

- Zijn in een plaatsing waarmee gebruikers om te zoeken.

- Bevat een zichtbare indicatie dat de inhoud een zoekresultaat internet is. Bijvoorbeeld, een instructie die de inhoud 'van het web is'.

- Bevat alle andere passende maatregelen om ervoor te zorgen dat de Bing zoeken-API-gegevens geen inbreuk maken op alle van toepassing zijnde wetten of rechten van derden. Raadpleeg uw juridische adviseurs om te bepalen welke metingen kunnen worden nodig.

De enige uitzondering op deze vereisten internet zoeken ervaring is voor de detectie van de URL, zoals verderop in dit artikel wordt beschreven. 

### <a name="restrictions"></a>Beperkingen

Niet doen:

- Kopiëren, opslaan of alle gegevens van antwoorden in de cache (met uitzondering van de bewaarperiode voor zover toegestaan door [continuïteit van de service](#continuity-of-service). 

- Gegevens ontvangen van de Search-API's als onderdeel van een machine learning- of vergelijkbare algoritmische activiteit gebruiken. Gebruik deze gegevens niet te trainen, evalueren of nieuwe of bestaande services die worden geboden door u of van derden mogelijk te verbeteren.

- De inhoud van de resultaten (andere dan te formatteren ze op een manier die niet in strijd is met alle andere vereiste), wijzigen, tenzij de wet of geaccepteerd door Microsoft. 

- Weglaten attribution informatie en URL's die zijn gekoppeld aan de resultaat-inhoud.

- Ordenen, met inbegrip van door het nalaten hiervan, worden de resultaten in een antwoord weergegeven als een order of de classificatie is opgegeven, tenzij de wet of geaccepteerd door Microsoft. 

    > [!NOTE]
    > Deze vereiste geldt niet voor opnieuw ordenen geïmplementeerd via de portal voor de Bing Custom Search-API.

- Weer andere inhoud in een deel van een reactie op een manier die een gebruiker te geloven dat de andere inhoud deel van het antwoord uitmaakt zou leiden. 

- Voor advertentiedoeleinden die niet is geleverd door Microsoft op een willekeurige pagina waarop een deel van een reactie. 

- Advertenties weergeven op pagina's met antwoorden:
    - Van de Bing afbeeldingen, nieuws zoeken, video's zoeken of visuele zoekopdrachten-API 's
    - Die zijn gefilterd of die beperkt zijn voornamelijk (of uitsluitend) naar afbeeldingen, nieuws en/of video- of visual search-resultaten.

### <a name="notices-and-branding"></a>Kennisgevingen en huisstijl 
Doen:

- Duidelijk zichtbaar zijn een functionele hyperlink naar de [privacyverklaring van Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839), in de buurt van elk punt in de gebruikerservaring (UX) die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekquery. Label van de hyperlink **privacyverklaring van Microsoft**.

- Duidelijk zichtbaar weer Bing huisstijl, consistent zijn met de [richtlijnen voor handelsmerken Bing](https://go.microsoft.com/fwlink/?linkid=833278), in de buurt van elk punt in de UX die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekquery. Dergelijke huisstijl moet duidelijk staat voor de gebruiker die door Microsoft is dat de zoekfunctie van internet.

- U kunt elk antwoord (of elk deel van een antwoord) weergegeven van de Bing webzoekopdrachten, afbeeldingen zoeken, nieuws zoeken, video's zoeken en visueel zoeken-API's aan Microsoft, kenmerk, tenzij Microsoft bij het schrijven voor uw gebruik iets anders aangeeft. Dit wordt beschreven in [richtlijnen voor handelsmerken Bing](https://go.microsoft.com/fwlink/?linkid=833278). 

Niet doen:

- Kenmerk antwoorden (of gedeelten van de antwoorden) weergegeven van de Bing Custom Search-API aan Microsoft, tenzij Microsoft bij het schrijven voor uw specifieke gebruik iets anders aangeeft.

### <a name="transferring-responses"></a>Overdracht van reacties

Als u een gebruiker om over te dragen van een reactie van een Search-API aan een andere gebruiker, zoals via een berichten-app of sociale media plaatsen, de volgende van toepassing inschakelen: 

- Overgedragen antwoorden moeten:
  - Bestaan uit de inhoud die niet worden gewijzigd van de inhoud van de antwoorden weergegeven aan de gebruiker overdragen. Wijzigingen in de opmaak zijn toegestaan.
  - Geen gegevens in de vorm van metagegevens bevatten.
  - Voor antwoorden van de Bing Web, afbeeldingen, nieuws, Video en Visual API's, is weergavetaal die wijzen op het antwoord verkregen via een internet-zoekervaring mogelijk gemaakt door Bing. Bijvoorbeeld, kunt u de taal, zoals 'Mogelijk gemaakt door Bing' of "Leren" meer informatie over deze afbeelding weergeven in Bing of kunt u het Bing-logo.
  - Voor antwoorden van de Bing Custom Search-API, weergegeven die wijzen op dat het antwoord is verkregen via een internet-zoekervaring taal. Bijvoorbeeld, kunt u de taal, zoals 'Meer informatie over dit zoekresultaat.' weergeven
  - De volledige-query gebruikt voor het genereren van het antwoord duidelijk zichtbaar weer.
  - Bevatten een prominente koppeling of een vergelijkbare attribution naar de onderliggende gegevensbron van het antwoord, rechtstreeks of via de zoekmachine (bing.com, m.bing.com of uw aangepaste search-service, zoals van toepassing).
- U mag niet de overdracht van antwoorden automatiseren. Een overdracht moet worden gestart door een actie van de gebruiker duidelijk waaruit blijkt een intent om over te dragen van een antwoord.
- U mag alleen een gebruiker om over te dragen van antwoorden die werden weergegeven in reactie op de overdracht gebruikersquery inschakelen.

### <a name="continuity-of-service"></a>Continuïteit van de service 

Niet kopiëren, opslaan of alle gegevens van antwoorden zoeken-API in de cache. Echter, zodat de continuïteit van de toegang tot de service en de rendering van rapportgegevens bewaren u resultaten uitsluitend in de volgende omstandigheden:

#### <a name="device"></a>Apparaat

Mogelijk is, schakelt u een gebruiker te bewaren van resultaten op een apparaat voor de laagste van (i) 24 uur vanaf het moment van de query, of (ii) totdat een gebruiker een andere query voor de bijgewerkte resultaten indient voorwaarde dat de ingehouden resultaten kunnen alleen worden gebruikt:

- Om in te schakelen van de gebruiker toegang tot resultaten die eerder zijn geretourneerd voor die gebruiker op het apparaat (bijvoorbeeld in het geval van service wordt onderbroken).
- Voor het opslaan van resultaten voor uw proactieve query persoonlijke in afwachting van de behoeften van de gebruiker, op basis van signalen van die gebruiker (bijvoorbeeld in het geval van verwachte service wordt onderbroken).

#### <a name="server"></a>Server

U kunt oplost die specifiek zijn voor één gebruiker veilig op een server die u beheert, en alleen de bewaarde resultaten weer te geven:

- Om in te schakelen van de gebruiker toegang tot een historisch rapport van de resultaten die eerder zijn geretourneerd voor die gebruiker in uw oplossing. De resultaten kunnen niet worden (i) behouden gedurende meer dan 21 dagen vanaf het moment van de eerste query van de eindgebruiker en (ii) in antwoord op de nieuwe of herhaalde query van een gebruiker weergegeven.
- Persoonlijke in afwachting van de behoeften van de gebruiker, op basis van signalen van die gebruiker voor het opslaan van resultaten die voor uw proactieve query zijn geretourneerd. U kunt deze resultaten voor de laagste van (i) 24 uur vanaf het moment van de query, of (ii) totdat een gebruiker een andere query voor de bijgewerkte resultaten indient opslaan.

Wanneer aanwezig zijn, kunnen geen resultaten voor een specifieke gebruiker worden commingled met de resultaten van een andere gebruiker. Dat wil zeggen, moeten de resultaten van elke gebruiker worden bewaard en afzonderlijk verkrijgbaar waren.

### <a name="general"></a>Algemeen 

Voor alle presentatie van de ingehouden resultaten:

- Bevatten een duidelijke, zichtbaar kennisgeving van de tijd waarop die de query is verzonden.
- Dit betekent dat de gebruiker met een knop of vergelijkbare opnieuw opvragen en verkrijgen van resultaten bijgewerkt. 
- De huisstijl van de presentatie van de resultaten Bing behouden.
- verwijderen (en indien nodig het vernieuwen met een nieuwe query) de opgeslagen resultaten binnen het opgegeven tijdsbestek.

### <a name="non-display-url-discovery"></a>Detectie van niet-weergave-URL 

U kunt de reacties op webzoekopdrachten alleen gebruiken in een internet-zoekervaring voor het enige doel van het detecteren van URL's van bronnen met informatie die reageren op een query van de gebruiker of de klant. U mag deze URL's kopiëren in een rapport of een vergelijkbaar antwoord dat u opgeeft:

- Alleen voor die gebruiker of de klant, in reactie op deze query.
- Alleen als deze aanzienlijke extra waardevol inhoud, relevant zijn voor de query bevat.

Gebruikt u de voorgaande secties in zoeken-API's en vereisten van de weergave niet van toepassing op dit gebruik niet weergegeven, met uitzondering van de volgende: 

- Niet in de cache, kopiëren of opslaan van gegevens of inhoud, of is afgeleid van het antwoord van de zoekactie dan het beperkte URL kopiëren die eerder zijn beschreven.
- Zorg ervoor dat uw gebruik van gegevens (inclusief de URL's) van de Search-API's ontvangen geen inbreuk maken op alle van toepassing zijnde wetten of rechten van derden.
- Gebruik niet de gegevens (inclusief de URL's) die worden ontvangen van de Search-API's als onderdeel van een search-index of machine learning of vergelijkbare algoritmische activiteit. Gebruik deze gegevens niet te maken van de trein, evalueren of services die worden geboden door u of van derden mogelijk te verbeteren.

## <a name="gdpr-compliance"></a>GDPR-naleving  

Met betrekking tot persoonlijke gegevens afhankelijk van de Europese Unie General Data Protection Regulation (GDPR) en die wordt verwerkt in verband met aanroepen naar de zoeken-API's, de Bing Spell Check-API of de Automatische suggestie-API, weten u dat u en Microsoft zijn domeincontrollers onafhankelijke gegevens onder de AVG. U bent onafhankelijk die verantwoordelijk is voor uw naleving van de AVG.  

