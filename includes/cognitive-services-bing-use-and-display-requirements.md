---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: MikeDodaro
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 04/19/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 174af83686eba665a729246be7a477b9a5054f30
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345960"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Bing zoeken-API gebruiken en weergeven van vereisten

Gebruik en de weergave vereisten gelden geen implementatie van de inhoud en de bijbehorende gegevens. Bijvoorbeeld, zijn de vereisten van toepassing op relaties, metagegevens en andere signalen. Dit kunnen zijn beschikbaar via aanroepen naar de volgende API's:

- Bing Aangepaste zoekopdrachten
- Bing Entiteiten zoeken
- Bing Afbeeldingen zoeken
- Bing Nieuws zoeken
- Bing Video's zoeken
- Bing Visuele zoekopdrachten
- Bing Webzoekopdrachten
- Bing Spellingcontrole
- Bing Automatische suggesties

U vindt de implementatiegegevens die zijn gerelateerd aan deze vereisten in de documentatie voor specifieke functies en de resultaten.     

## <a name="bing-spell-check-and-bing-autosuggest-apis"></a>Spellingcontrole van Bing en Bing voor Automatische suggestie van API 's

Niet doen:

- Kopiëren, opslaan of alle gegevens die u van spellingcontrole van Bing of API's voor Automatische suggestie van Bing ontvangt in de cache.
- Gegevens die u van spellingcontrole van Bing of API's voor Automatische suggestie van Bing als onderdeel van een machine learning of vergelijkbare algoritmische activiteit ontvangt worden gebruikt. Gebruikt deze gegevens niet om te trainen, evalueren of nieuwe of bestaande services die u of derden biedt mogelijk te verbeteren.

## <a name="definitions"></a>Definities

- *Antwoord* verwijst naar een categorie met resultaten in een antwoord geretourneerd. Bijvoorbeeld, kunt een reactie van de Web zoeken-API van Bing antwoorden opnemen in de categorieën van webpagina-resultaten, image, video, visual en nieuws.   
- *Antwoord* betekent dat alle antwoorden en bijbehorende gegevens die zijn ontvangen als reactie op één aanroep naar een zoeken-API.
- *Resultaat* verwijst naar een item van gegevens in een antwoord. De set gegevens die zijn verbonden met een enkele nieuwsbericht is bijvoorbeeld een resultaat in een nieuwsantwoord.
- *Zoeken-API's* : gezamenlijk de Bing aangepaste zoekactie entiteit zoeken, zoeken naar afbeelding, nieuws zoeken, Video zoeken, Visual zoeken en Web zoeken-API's. 


## <a name="search-apis"></a>Zoek-API's

De vereisten in deze sectie zijn van toepassing op de zoeken-API's. Zoeken-API's bevatten geen spellingcontrole van Bing of Automatische suggestie van Bing. De vereisten voor deze twee API's worden behandeld in de vorige sectie.

### <a name="internet-search-experience"></a>Internet-zoekfunctie

Alle gegevens die worden geretourneerd in antwoorden kan alleen worden gebruikt in internet zoeken ervaringen. Een zoekfunctie internet betekent dat de inhoud weergegeven, indien van toepassing: 
- Wordt reageert op de directe query van de eindgebruiker of andere indicatie van de gebruiker zoeken belang en doel (bijvoorbeeld een gebruiker aangegeven zoekquery). 
- Helpt gebruikers zoeken en navigeer naar de bronnen van gegevens (de URL's die worden bijvoorbeeld geïmplementeerd als hyperlinks, zodat de inhoud of de bestemming een link opvallende weergegeven met de gegevens is). Of, als gebruikt Bing-API van zoekservice entiteit, zichtbaar voor koppelingen naar de opgegeven in het antwoord waarmee de gebruiker om te navigeren naar de zoekresultaten voor de relevante query op bing.com bing.com-URL.
- Bevat verschillende resultaten voor de gebruiker te selecteren in (bijvoorbeeld verschillende resultaten van het nieuwsantwoord worden weergegeven of alle resultaten als minder dan enkele worden geretourneerd). 
- Is beperkt tot een bedrag geschikt is voor het leveren van het doel van de zoekopdracht (bijvoorbeeld installatiekopie miniaturen zijn miniatuurformaat in verhouding tot de weergave van de gebruiker). 
- Bevat een zichtbare indicatie voor de gebruiker dat de inhoud is zoekresultaten op internet (bijvoorbeeld, een overzicht dat de inhoud 'van de web is').
- Bevat een andere combinatie van metingen om ervoor te zorgen het gebruik van gegevens ontvangen van de zoeken-API's niet in strijd toepasbare wet- of rechten van derden. Bijvoorbeeld, als een op een advertentie Commons-licentie, voldoen u aan de geldende licentievoorwaarden. Raadpleeg uw juridisch adviseurs om te bepalen welke metingen kunnen worden nodig.
De enige uitzondering op het internet zoeken ervaring vereiste is voor detectie van de URL, zoals verderop in dit artikel wordt beschreven. 

### <a name="restrictions"></a>Beperkingen

Niet doen:

- Kopiëren, opslaan of geen gegevens van reacties (met uitzondering van de bewaarperiode voor zover toegestaan door de sectie 'Continuïteit van de Service' verderop in dit artikel) in de cache. 
- Gegevens ontvangen van de zoeken-API's als onderdeel van een machine learning of vergelijkbare algoritmische activiteit worden gebruikt. Gebruikt deze gegevens niet om te trainen, evalueren of nieuwe of bestaande services die u of derden biedt mogelijk te verbeteren.
- Wijzigen van de inhoud van de resultaten (andere waarde dan om te formatteren ze op een manier die niet in strijd is met andere behoeften), tenzij wettelijk verplicht of Ga hiermee akkoord door Microsoft. 
- Toekenning en URL's die zijn gekoppeld aan resultaat inhoud weglaten.
- De volgorde wijzigen, met inbegrip van weglating, resultaten weergegeven in een antwoord wanneer een order of een ranking is opgegeven, tenzij vereist door de wet of Ga hiermee akkoord door Microsoft. (Voor de API van Bing aangepaste zoekservice met deze regel geldt niet voor volgorde geïmplementeerd via de portal customsearch.ai.)
- Weer andere inhoud binnen een deel van een reactie op een manier die een gebruiker om te geloven dat de inhoud van andere deel van het antwoord uitmaakt zou leiden. 
- Reclame die op elke pagina die wordt weergegeven een deel van een antwoord niet is geleverd door Microsoft worden weergegeven. 
- Adverteren met reacties (i) van de Bing-installatiekopie, nieuws zoeken, Video zoeken of Visual zoeken-API's; weergeven of (ii) die zijn gefilterd of beperkt voornamelijk (of uitsluitend) naar de installatiekopie, nieuws en/of video of visuele resultaten.

### <a name="notices-and-branding"></a>Kennisgevingen en huisstijl 

- Een functionele hyperlink naar prominent opnemen de [privacyverklaring van Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839), in de buurt van elk punt in de gebruikerservaring (UX) die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekopdracht. Label van de hyperlink **privacyverklaring van Microsoft**.
- Prominent weergegeven Bing huisstijl consistent zijn met de [richtlijnen voor handelsmerken Bing](https://go.microsoft.com/fwlink/?linkid=833278), in de buurt van elk punt in de UX die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekopdracht. Dergelijke huisstijl moet duidelijk geven aan de gebruiker Microsoft aansturen van de internet-zoekfunctie.
- U kunt elke antwoord (of deel uitmaken van een antwoord) van de Bing webpagina's zoeken, zoeken naar afbeelding, nieuws zoeken, Video zoeken en visuele zoeken-API's naar Microsoft, weergegeven kenmerk tenzij Microsoft iets anders aangeeft in te schrijven voor het gebruik. Dit wordt beschreven in [richtlijnen voor handelsmerken Bing](https://go.microsoft.com/fwlink/?linkid=833278). 
- Niet kenmerk antwoorden (of delen van antwoorden) weergegeven van de API van Bing aangepaste zoeken naar Microsoft, tenzij Microsoft iets anders aangeeft in schrijven voor het gebruik van bepaalde.

### <a name="transferring-responses"></a>Overdracht van reacties

Als u een gebruiker een antwoord van een zoeken-API voor een andere gebruiker, zoals overdragen via een messaging-app of sociale media boeken, de volgende van toepassing inschakelen: 
- Verzonden reacties moet het volgende doen:
  - Bestaan uit de inhoud die is ongewijzigd van de inhoud van de antwoorden voor de overgedragen gebruiker weergegeven. Wijzigingen in de opmaak zijn toegestaan.
  - Geen gegevens in de vorm metagegevens bevatten.
  - Voor reacties uit Bing Web, afbeeldingen, nieuws, Video en Visual-API's, is weergavetaal die wijzen op het antwoord verkregen via een internet-zoekfunctie mogelijk gemaakt door Bing. Bijvoorbeeld, kunt u de taal zoals 'Aangedreven door Bing' of 'Informatie' meer informatie over deze installatiekopie op Bing weergeven of kunt u het Bing-logo.
  - Voor antwoorden van de API van Bing aangepaste zoeken weergegeven die wijzen op dat het antwoord is verkregen via een internet-zoekervaring taal. Bijvoorbeeld, kunt u weergavetaal zoals "Meer informatie over dit zoekresultaat."
  - De volledige query gebruikt voor het genereren van het antwoord worden duidelijk weergegeven.
  - Bevatten een opvallende koppeling of vergelijkbare toekenning naar de onderliggende bron van het antwoord, rechtstreeks of via de search-engine (bing.com, m.bing.com of uw aangepaste search-service, indien van toepassing).
- U kunt de overdracht van antwoorden mogelijk niet automatiseren. Een overdracht moet worden gestart door een gebruikersactie duidelijk waaruit blijkt de opzet om over te dragen van een antwoord.
- U mag alleen een gebruiker om over te dragen antwoorden die werden weergegeven in reactie op de overgebracht gebruikersquery inschakelen.

### <a name="continuity-of-service"></a>Continuïteit van de service 

Geen kopiëren, opslaan, of geen gegevens van de API van zoekservice antwoorden in de cache. Echter, zodat de continuïteit van de Servicetoegang en de rendering van rapportgegevens u mogelijk oplost uitsluitend de volgende voorwaarden:

**het apparaat.** U kunt dit inschakelen van een gebruiker oplost op een apparaat voor de laagste van (i) 24 uur vanaf het moment van de query of (ii) totdat een gebruiker een andere query voor bijgewerkte resultaten verzendt mits terugkerende resultaten kunnen alleen worden gebruikt:

- De gebruiker toegang tot resultaten die eerder zijn geretourneerd voor die gebruiker op het apparaat (bijvoorbeeld in geval van een onderbreking van de service) inschakelen.
- Voor het opslaan van resultaten geretourneerd voor uw proactieve query persoonlijke in afwachting van de behoeften van de gebruiker, op basis van die gebruiker signalen (bijvoorbeeld in geval van een onderbreking van de verwachte service).

**-Server.** U kunt oplost die specifiek zijn voor één gebruiker veilig op een server die u beheert en alleen de terugkerende resultaten weer te geven:

- Inschakelen van de gebruiker toegang tot een historisch rapport van de resultaten die eerder zijn geretourneerd voor die gebruiker in uw oplossing. De resultaten kunnen niet worden (i) voor meer dan 21 dagen na het tijdstip van de eerste query van de eindgebruiker behouden en (ii) weergegeven in reactie op een gebruiker nieuwe of herhaalde query.
- Persoonlijke in afwachting van de behoeften van de gebruiker, op basis van die gebruiker signalen voor het opslaan van resultaten die voor uw proactieve query zijn geretourneerd. U kunt deze resultaten voor de laagste van (i) 24 uur vanaf het moment van de query of (ii) totdat een gebruiker verzendt een andere query voor bijgewerkte resultaten opslaan.

Wanneer behouden, kunnen geen resultaten voor een specifieke gebruiker worden commingled met resultaten voor een andere gebruiker. Dat wil zeggen, moeten de resultaten van elke gebruiker worden bewaard en apart geleverd.

### <a name="general"></a>Algemeen 

Voor alle presentatie van terugkerende resultaten:

- Bevatten een duidelijke, zichtbaar aankondiging van de tijd waarop die de query is verzonden.
- De gebruiker met een knop of vergelijkbare betekent opnieuw doorzoeken en verkrijgen aanwezig bijgewerkt resultaten. 
- De huisstijl van de presentatie van de resultaten Bing behouden.
- verwijderen (en indien nodig het vernieuwen met een nieuwe query) de resultaten opgeslagen binnen de opgegeven tijdsduur.

### <a name="non-display-url-discovery"></a>Detectie van de URL niet weergeven 

U mag alleen zoekresultaten in een internet-zoekfunctie gebruiken voor enig doel van het detecteren van URL's van informatiebronnen reageert op een query van de gebruiker of de klant. U kunt deze URL's in een rapport of een vergelijkbaar antwoord die u opgeeft kopiëren:

- Alleen voor die gebruiker of een klant in reactie op deze query.
- Alleen als het een aanzienlijke extra waardevolle inhoud, relevant zijn voor de query bevat.

De voorgaande secties in zoeken-API's gebruiken en weergeven vereisten gelden niet voor dit gebruik niet weergeven, met uitzondering van het volgende: 

- Geen cache, kopiëren of opslaan van gegevens of inhoud, of zijn afgeleid van het antwoord van de zoekactie dan het beperkte URL kopiëren die eerder zijn beschreven.
- Zorg ervoor dat uw gebruik van gegevens (inclusief de URL's) ontvangen van de zoeken-API's niet in strijd toepasbare wet- of rechten van derden.
- Gebruik niet de gegevens (inclusief de URL's) die worden ontvangen van de zoeken-API's als onderdeel van een search-index of de machine learning of vergelijkbare algoritmische activiteit. Gebruikt deze gegevens niet train maken, evalueren of services die u of derden biedt mogelijk te verbeteren.

## <a name="gdpr-compliance"></a>GDPR naleving  

Met betrekking tot persoonlijke gegevens onderworpen aan de Europese Unie algemene Data Protection regelgeving (GDPR) en die in verband met aanroepen naar de zoeken-API's, spellen controleren Bing-API of voor Automatische suggestie van Bing-API is verwerkt, begrijpt u dat u en Microsoft zijn onafhankelijke gegevens domeincontrollers onder de GDPR. U bent onafhankelijk verantwoordelijk is voor dat u voldoet aan de GDPR.  

