---
title: Project gebruik antwoord zoeken en weer te geven vereisten - cognitieve Microsoft-Services | Microsoft Docs
description: Gebruik en vereisten voor het eindpunt Project antwoord zoeken weergeven.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 6e8eaaaa2c83a1420f2de86b23e15f4f19f7a565
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345325"
---
# <a name="project-answer-search-use-and-display-requirements"></a>Project antwoord zoeken gebruiken en weergeven van vereisten

Gebruik en weergeven vereisten gelden voor implementatie van de inhoud en de bijbehorende gegevens, bijvoorbeeld relaties, metagegevens en andere signalen, beschikbaar via aanroepen voor het zoeken naar Bing kennisartikel, Bing aangepaste zoekactie, entiteit zoeken, afbeeldingen zoeken Nieuws zoeken, Video, Visual zoeken Web zoeken, spellingcontrole en API's voor Automatische suggestie. Implementatiegegevens die zijn gerelateerd aan deze vereisten vindt u in de documentatie voor specifieke functies en de resultaten.

## <a name="1-bing-spell-check-and-bing-autosuggest-api"></a>1. Spellingcontrole van Bing en Bing voor Automatische suggestie van API.

Niet doen:

- kopiëren, opslaan of alle gegevens die u van de spellingcontrole van Bing of API's voor Automatische suggestie van Bing ontvangt in de cache
- gebruik van gegevens die u van de spellingcontrole van Bing of API's voor Automatische suggestie van Bing als onderdeel van een machine learning of vergelijkbare algoritmische activiteit ontvangt te trainen, te evalueren of te verbeteren nieuwe of bestaande services die u of van derden kunnen bieden.

## <a name="2-definitions"></a>2. Definities

- "antwoord" verwijst naar een categorie met resultaten in een antwoord geretourneerd. Bijvoorbeeld, kan een reactie van de Web zoeken-API van Bing antwoorden in de categorieën van webpagina-resultaten, image, video en nieuws; bevatten
- "antwoord" betekent dat alle antwoorden en bijbehorende gegevens die zijn ontvangen als reactie op één aanroep van een zoeken-API
- 'resultaat' verwijst naar een item van de informatie in een antwoord. De set gegevens die zijn verbonden met een enkele nieuwsbericht is bijvoorbeeld een resultaat in een nieuwsantwoord.
- ' Zoeken-API's ' betekent gezamenlijk de Bing aangepaste zoekactie entiteit zoeken, zoeken naar afbeelding, nieuws zoeken, Video zoeken, Visual zoeken en Web zoeken-API's. 


## <a name="3-search-apis"></a>3. Zoek-API's

De vereisten in deze sectie 3 van toepassing op de zoeken-API's.

**EEN. Internet-zoekfunctie.** Alle gegevens die worden geretourneerd in antwoorden kan alleen worden gebruikt in internet zoeken ervaringen. Een zoekfunctie internet betekent dat de inhoud weergegeven, indien van toepassing: 
- relevant en reageert op de directe query van de eindgebruiker of andere indicatie van de gebruiker zoeken belang en doel (bijvoorbeeld gebruiker aangegeven zoekquery); 
- helpt gebruikers zoeken en navigeer naar de bronnen van gegevens (bijvoorbeeld de URL's die worden toegepast als hyperlinks zodat de inhoud of de bestemming een link opvallende weergegeven met de gegevens); of, als het van Bing entiteit Search API koppelen zichtbaar aan de bing.com-URL in het antwoord waarmee de gebruiker om te navigeren naar de zoekresultaten voor de relevante query op bing.com;
- bevat verschillende resultaten voor de eindgebruiker te selecteren in (bijvoorbeeld verschillende resultaten van het nieuwsantwoord worden weergegeven of alle resultaten als minder dan enkele worden geretourneerd); 
- beperkt tot een bedrag geschikt is voor het leveren van het doel van de zoekopdracht (bijvoorbeeld afbeelding miniaturen zijn miniatuurformaat in verhouding tot de weergave van de gebruiker). 
- bevat zichtbare indicatie voor de eindgebruiker dat de inhoud is zoekresultaten op Internet (bijvoorbeeld, een overzicht dat de inhoud 'van het web is"); en
- bevat een andere combinatie van metingen om ervoor te zorgen het gebruik van gegevens ontvangen van de zoeken-API's niet in strijd toepasbare wet- of de rechten van derden (bijvoorbeeld, als afhankelijk te zijn van een advertentie Commons-licentie, voldoen aan de toepasselijke licentievoorwaarden voorwaarden). Raadpleeg uw juridisch adviseurs om te bepalen welke metingen kunnen worden nodig.
De enige uitzondering aan de vereiste internet zoeken ervaring is voor de detectie van de URL, zoals beschreven in de sectie 3E (niet-weergave-URL discovery) volgende. 

**B. Beperkingen.** Niet doen:

- kopiëren, opslaan of geen gegevens van reacties (met uitzondering van de bewaarperiode voor zover toegestaan door de sectie 'Continuïteit van de Service' volgende); in de cache 
- gegevens ontvangen van de zoeken-API's als onderdeel van een machine learning of vergelijkbare algoritmische activiteit gebruiken om te trainen, evalueren of het verbeteren van de nieuwe of bestaande services die u of derden kan bieden.
- wijzigen van de inhoud van de resultaten (andere dan om te formatteren ze op een manier die niet in strijd is met andere behoeften), tenzij wettelijk verplicht of door Microsoft overeengekomen. 
- weglaten toekenning en URL's die zijn gekoppeld aan de inhoud van resultaat;
- volgorde, met inbegrip van weglating, resultaten weergegeven in een antwoord als een order of een ranking beschikbaar is (voor de API van Bing aangepaste zoekservice met deze regel geldt niet voor volgorde geïmplementeerd via de portal customsearch.ai), tenzij wettelijk verplicht of met Microsoft overeengekomen ;
- andere inhoud binnen een deel van een reactie op een manier die een eindgebruiker van mening bent dat de inhoud van andere deel van het antwoord uitmaakt; zou leiden weergeven 
- adverteert die niet is geleverd door Microsoft op een pagina met willekeurig deel van een reactie; weergeven -advertenties met reacties (i) van de Bing-installatiekopie, nieuws of Video zoeken-API's; weergeven of (ii) die zijn gefilterd of beperkt voornamelijk (of uitsluitend) naar de installatiekopie, nieuws en/of video leidt.

**C. Kennisgevingen en huisstijl.** 

- Een functionele hyperlink naar de privacyverklaring van Microsoft, beschikbaar op prominent opnemen https://go.microsoft.com/fwlink/?LinkId=521839, in de buurt van elk punt in de gebruikerservaring (UX) die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekopdracht. Label van de hyperlink "Microsoft Privacy Statement".
- Prominent weergegeven Bing huisstijl consistent zijn met de richtlijnen die beschikbaar zijn op https://go.microsoft.com/fwlink/?linkid=833278, in de buurt van elk punt in de UX die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekopdracht.  Dergelijke huisstijl moet duidelijk geven aan de gebruiker Microsoft aansturen van de internet-zoekfunctie.
- Kan u elk antwoord (of deel uitmaken van een antwoord) weergegeven uit het Bing Web, Image, nieuws en Video-API's naar Microsoft, zoals beschreven in kenmerk https://go.microsoft.com/fwlink/?linkid=833278, tenzij Microsoft iets anders aangeeft in te schrijven voor het gebruik. 
- Niet kenmerk antwoorden (of delen van antwoorden) weergegeven van de API van Bing aangepaste zoeken naar Microsoft, tenzij Microsoft iets anders aangeeft in schrijven voor het gebruik van bepaalde.


**D. Bezig met het verzenden van antwoorden.** Als u een gebruiker een antwoord van een zoeken-API voor een andere gebruiker, zoals overdragen via een messaging-app of sociale media boeken, de volgende van toepassing inschakelen: 
- Verzonden reacties moet het volgende doen:
  - Bestaan uit de inhoud die is van de inhoud van de antwoorden weergegeven voor de gebruiker overgebracht ongewijzigd (opmaakwijzigingen zijn toegestaan);
  - Geen gegevens bevatten in metagegevens formulier.
  - Op de antwoorden van de Bing Web, Image, nieuws en Video-API's, weergavetaal die wijzen op het antwoord is verkregen via een internet-zoekfunctie mogelijk gemaakt door Bing (bijvoorbeeld "Aangedreven door Bing," "meer informatie over deze installatiekopie op Bing ', of met behulp van het logo Bing);
  - Voor antwoorden van de API van Bing aangepaste zoeken is weergavetaal die wijzen op het antwoord verkregen via een internet-zoekfunctie (bijvoorbeeld "meer informatie over dit zoekresultaat");
  - Volledige query gebruikt voor het genereren van het antwoord; prominent wilt weergeven en
  - Bevatten een opvallende koppeling of vergelijkbare toekenning naar de onderliggende bron van het antwoord, rechtstreeks of via de search-engine (bing.com, m.bing.com of uw aangepaste search-service, indien van toepassing).
- U kunt de overdracht van antwoorden mogelijk niet automatiseren. Een overdracht moet worden gestart door een gebruikersactie duidelijk waaruit blijkt de opzet om over te dragen van een antwoord.
- U mag alleen een gebruiker om over te dragen antwoorden die werden weergegeven in reactie op de overgebracht gebruikersquery inschakelen.

**E. De continuïteit van de service.** Geen kopiëren, opslaan, of geen gegevens van de API van zoekservice antwoorden in de cache. Echter, zodat de continuïteit van de Servicetoegang en de rendering van rapportgegevens u mogelijk oplost uitsluitend de volgende voorwaarden:

**Het apparaat.** U kunt dit oplost op een apparaat voor de laagste van (i) 24 uur vanaf het moment van de query of (ii) totdat een eindgebruiker een andere query voor bijgewerkte resultaten indient mits terugkerende resultaten kunnen alleen worden gebruikt door een eindgebruiker inschakelen:

- inschakelen van de eindgebruiker toegang tot resultaten die eerder zijn geretourneerd voor deze gebruiker op het apparaat (bijvoorbeeld in geval van een onderbreking van de service); of
- voor het opslaan van resultaten geretourneerd voor uw proactieve query persoonlijke in afwachting van de behoeften van de eindgebruiker op basis van die van de eindgebruiker signalen (bijvoorbeeld in geval van een onderbreking van de verwachte service).

**-Server.** U kan resultaten die specifiek zijn voor een enkele eindgebruikers veilig op een server die u beheren en weergeven van de terugkerende resultaten alleen bewaren:

- de eindgebruiker toegang tot een historisch rapport van de resultaten eerder geretourneerd voor die gebruiker in uw oplossing, mits de resultaten niet (i) gedurende meer dan 21 dagen na het tijdstip van de eerste query van de eindgebruiker behouden mogelijk en (ii) weergegeven in reactie op een end u inschakelen Ser van nieuwe of herhaalde query; of
- persoonlijke in afwachting van de behoeften van de eindgebruiker op basis van die van de eindgebruiker signalen voor de laagste van (i) 24 uur vanaf het moment van de query of (ii) totdat een eindgebruiker een andere query voor bijgewerkte resultaten verzendt voor het opslaan van resultaten die voor uw proactieve query zijn geretourneerd.

Wanneer behouden, resultaten voor een specifieke gebruiker kunnen niet worden commingled met de resultaten van een andere gebruiker, dat wil zeggen, de resultaten van elke gebruiker moeten worden bewaard en apart geleverd.

**Algemeen.** Voor alle presentatie van terugkerende resultaten:

- een duidelijke, zichtbaar tekst van de tijd dat de query is verzonden, opnemen
- presenteren aan de gebruiker een knop of een vergelijkbare manier opnieuw doorzoeken en bijgewerkte resultaten krijgen 
- de huisstijl van de presentatie van de resultaten Bing behouden en
- verwijderen (en indien nodig het vernieuwen met een nieuwe query) de resultaten opgeslagen binnen de opgegeven tijdsduur.

**F. Detectie van de URL niet weergeven.** U mag alleen zoekresultaten in een internet-zoekfunctie gebruiken voor enig doel van het detecteren van URL's van informatiebronnen reageert op een query van de gebruiker of de klant. U kunt deze URL's in een rapport of een vergelijkbaar antwoord die u (i) alleen voor die gebruiker opgeeft kopiëren of klant, omvat in reactie op die query en (ii) die belangrijke aanvullende waardevolle inhoud relevant zijn voor de query. De vereisten in secties 3A tot en met 3E van deze vereisten gebruiken en weergeven niet van toepassing op het gebruik van deze niet weergegeven, met uitzondering van: 

- U wordt niet in de cache, te kopiëren of opslaan van gegevens of inhoud op of afgeleid zijn van het antwoord van de zoekactie dan het beperkte URL kopiëren die eerder zijn beschreven;
- Zorg ervoor dat uw gebruik van gegevens (inclusief de URL's) ontvangen van de zoeken-API's niet in strijd toepasbare wet- of rechten van derden; en
- U moet de gegevens (inclusief de URL's) die worden ontvangen van de zoeken-API's als onderdeel van een search-index of de machine learning of vergelijkbare algoritmische activiteit niet gebruiken om de trein maken, evalueren of services die u of derden bieden verbeteren.

## <a name="next-steps"></a>Volgende stappen
[Overzicht van antwoord zoeken](overview.md)
