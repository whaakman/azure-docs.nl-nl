---
title: Gebruik en weergavevereisten - Project antwoord zoeken
titlesuffix: Azure Cognitive Services
description: Gebruik en weergavevereisten voor het eindpunt van het Project antwoord zoeken.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 085cb20e4dad92ed55b5ba0914c677aa50f3ac97
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875328"
---
# <a name="project-answer-search-use-and-display-requirements"></a>Project antwoord zoeken gebruiken en weergavevereisten

Gebruik en weergave vereisten van toepassing op elke uitvoering van de inhoud en de bijbehorende gegevens, bijvoorbeeld, relaties, metagegevens en andere signalen, beschikbaar via aanroepen naar de Bing Knowledge zoeken, Bing Custom Search, entiteiten zoeken, afbeeldingen zoeken, Nieuws zoeken, video's zoeken, visuele zoekopdrachten, Web, zoeken, Spell Check en Automatische suggestie-API's. Implementatiegegevens die betrekking hebben op deze vereisten vindt u in de documentatie voor specifieke functies en de resultaten.

## <a name="1-bing-spell-check-and-bing-autosuggest-api"></a>1. Bing Spell Check- en Bing Automatische suggestie-API.

Niet doen:

- kopiëren, opslaan of alle gegevens die u van de Bing Spell Check- of Bing Automatische suggestie-API's ontvangt in de cache
- Gebruik de gegevens die u van de Bing Spell Check- of Bing Automatische suggestie-API's als onderdeel van een machine learning- of vergelijkbare algoritmische activiteit trainen ontvangt, evalueren, of een nieuwe of bestaande services die u of andere bedrijven bieden mogelijk te verbeteren.

## <a name="2-definitions"></a>2. Definities

- "antwoord" verwijst naar een categorie van de resultaten in een antwoord geretourneerd. Bijvoorbeeld, kan een reactie van de Bing webzoekopdrachten-API antwoorden bevatten in de categorieën van webpagina-resultaten, afbeelding, video en nieuws;
- 'response' betekent dat alle antwoorden en bijbehorende gegevens ontvangen in antwoord op één aanroep aan een Search-API
- "resultaat" verwijst naar een artikel van gegevens in een antwoord. De set gegevens die zijn verbonden met een enkele nieuwsartikel is bijvoorbeeld een resultaat in een nieuwsantwoord.
- ' Zoeken-API's ' betekent gezamenlijk de Bing Custom Search, entiteiten zoeken, afbeeldingen zoeken, nieuws zoeken, video's zoeken, visuele zoekopdrachten- en Web zoeken-API's. 


## <a name="3-search-apis"></a>3. Zoek-API's

De vereisten in deze sectie 3 van toepassing op het zoeken-API's.

**EEN. Zoekfunctie voor Internet.** Alle gegevens die worden geretourneerd in antwoorden kan alleen worden gebruikt in internet zoekervaringen. Een internet-zoekervaring betekent dat de inhoud weergegeven, indien van toepassing: 
- relevante en reageren op van de eindgebruiker directe query of een andere indicatie van de gebruiker zoeken bezienswaardigheden en een doel (bijvoorbeeld gebruiker aangegeven zoekquery); 
- helpt gebruikers bij het vinden en navigeer naar de bronnen van gegevens (bijvoorbeeld de URL's die worden geïmplementeerd als hyperlinks, zodat de inhoud of attribution een link opvallende weergegeven met de gegevens is); of, als u van de Bing Entity Search API koppelen zichtbaar aan de bing.com-URL die is opgegeven in het antwoord waarmee de gebruiker om te navigeren naar de lijst met zoekresultaten voor de relevante query op bing.com;
- bevat verschillende resultaten voor de eindgebruiker te selecteren in (bijvoorbeeld, verschillende resultaten van het nieuwsantwoord worden weergegeven of alle resultaten als minder dan verschillende worden geretourneerd); 
- is beperkt tot een bedrag dat geschikt is voor de search-doel (bijvoorbeeld afbeelding miniaturen worden miniatuur-de grootte van de weergave van de gebruiker); dienen 
- bevat zichtbare indicatie voor de eindgebruiker of de inhoud is zoekresultaten op Internet (bijvoorbeeld een overzicht dat de inhoud 'van het web is"); en
- een andere combinatie van maatregelen om ervoor te zorgen van uw gebruik van gegevens ontvangen van het zoeken-API's geen inbreuk maken op alle van toepassing zijnde wetten of rechten van derden (bijvoorbeeld, als afhankelijk zijn van een Creative Commons-licentie die voldoet aan de toepasselijke licentie bevat voorwaarden). Raadpleeg uw juridische adviseurs om te bepalen welke metingen kunnen worden nodig.
De enige uitzondering op het internet zoeken ervaring vereiste is voor de detectie van de URL, zoals beschreven in de sectie 3E (niet-weergave-URL-discovery) volgende. 

**B. Beperkingen.** Niet doen:

- kopiëren, opslaan of in de cache geen gegevens van antwoorden (met uitzondering van de bewaarperiode voor zover toegestaan door de volgende van de sectie 'Continuïteit van de Service'); 
- Gebruik gegevens ontvangen van het zoeken-API's als onderdeel van een machine learning- of vergelijkbare algoritmische activiteit trainen, evalueren, of een nieuwe of bestaande services die u of andere bedrijven bieden mogelijk te verbeteren.
- wijzigen van de inhoud van de resultaten (andere dan te formatteren ze op een manier die niet in strijd is met alle andere vereiste), tenzij de wet of goedgekeurd door Microsoft. 
- weglaten attribution en URL's die zijn gekoppeld aan de resultaat-inhoud:
- opnieuw wilt rangschikken, met inbegrip van door het nalaten hiervan, resultaten in een antwoord weergegeven als een order of classificatie wordt geleverd (voor de Bing Custom Search-API met deze regel geldt niet voor opnieuw ordenen geïmplementeerd via de portal customsearch.ai), tenzij de wet of overeengekomen door Microsoft ;
- andere inhoud binnen een deel van een reactie op een manier die een eindgebruiker te geloven dat de andere inhoud deel van het antwoord uitmaakt; zou leiden weergeven 
- voor advertentiedoeleinden die niet is geleverd door Microsoft op een willekeurige pagina waarop een deel van een reactie; -advertenties met antwoorden (i) van de Bing afbeeldingen, nieuws of Video zoeken-API's; weergeven of (ii) die zijn gefilterd of die beperkt zijn voornamelijk (of uitsluitend) tot afbeeldingen, nieuws en/of video resultaten.

**C. Kennisgevingen en huisstijl.** 

- Een functionele hyperlink naar de privacyverklaring van Microsoft, beschikbaar op duidelijk zichtbaar opnemen https://go.microsoft.com/fwlink/?LinkId=521839, in de buurt van elk punt in de gebruikerservaring (UX) die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekquery. De hyperlink "Microsoft Privacy Statement" van label.
- Duidelijk zichtbaar weer Bing huisstijl, consistent zijn met de richtlijnen die beschikbaar zijn op https://go.microsoft.com/fwlink/?linkid=833278, in de buurt van elk punt in de UX die een gebruiker de mogelijkheid biedt voor het invoeren van een zoekquery.  Dergelijke huisstijl moet duidelijk geven aan de gebruiker dat Microsoft wordt de zoekervaring internet is ingeschakeld.
- U kunt elk antwoord (of elk deel van een antwoord) weergegeven van de Bing Web, afbeeldingen, nieuws en Video-API's naar Microsoft, zoals beschreven in van het kenmerk https://go.microsoft.com/fwlink/?linkid=833278, tenzij Microsoft bij het schrijven voor uw gebruik iets anders aangeeft. 
- Niet kenmerk antwoorden (of gedeelten van de antwoorden) weergegeven van de Bing Custom Search-API aan Microsoft, tenzij Microsoft iets anders aangeeft bij het schrijven voor uw specifieke gebruik.


**D. Antwoorden worden overgebracht.** Als u een gebruiker om over te dragen van een reactie van een Search-API aan een andere gebruiker, zoals via een berichten-app of sociale media plaatsen, de volgende van toepassing inschakelen: 
- Overgedragen antwoorden moeten:
  - Bestaan uit de inhoud die niet worden gewijzigd van de inhoud van de antwoorden weergegeven aan de gebruiker overdragen (wijzigingen in de opmaak zijn toegestaan);
  - Geen gegevens bevatten in de metagegevens van formulier.
  - Voor antwoorden van de Bing Web, afbeeldingen, nieuws en Video-API's, weergavetaal die wijzen op het antwoord is verkregen via een internet-zoekervaring mogelijk gemaakt door Bing (bijvoorbeeld "Mogelijk gemaakt door Bing," "meer informatie over deze installatiekopie op Bing ', of met behulp van de Bing-logo);
  - Voor antwoorden van de Bing Custom Search-API, is weergavetaal die wijzen op het antwoord verkregen via een internet-zoekervaring (bijvoorbeeld ' meer informatie over dit zoekresultaat");
  - De volledige-query gebruikt voor het genereren van het antwoord; prominent weergegeven en
  - Bevatten een prominente koppeling of een vergelijkbare attribution naar de onderliggende gegevensbron van het antwoord, rechtstreeks of via de zoekmachine (bing.com, m.bing.com of uw aangepaste search-service, zoals van toepassing).
- U mag niet de overdracht van antwoorden automatiseren. Een overdracht moet worden gestart door een actie van de gebruiker duidelijk waaruit blijkt een intent om over te dragen van een antwoord.
- U mag alleen een gebruiker om over te dragen van antwoorden die werden weergegeven in reactie op de overdracht gebruikersquery inschakelen.

**E. De continuïteit van de service.** Niet kopiëren, opslaan of alle gegevens van antwoorden zoeken-API in de cache. Echter, zodat de continuïteit van de toegang tot de service en de rendering van rapportgegevens bewaren u resultaten uitsluitend in de volgende omstandigheden:

**Het apparaat.** U mogelijk een eindgebruiker oplost op een apparaat voor de laagste van (i) 24 uur vanaf het moment van de query of (ii) tot en met een eindgebruiker dient een andere query voor de bijgewerkte resultaten voorwaarde dat de ingehouden resultaten kunnen alleen worden gebruikt:

- om in te schakelen van de eindgebruiker toegang tot resultaten die eerder zijn geretourneerd naar die eindgebruikers op het apparaat (bijvoorbeeld in het geval van service wordt onderbroken); of
- voor het opslaan van resultaten voor uw proactieve query persoonlijke in afwachting van de behoeften van de eindgebruiker op basis van signalen die van de eindgebruiker (bijvoorbeeld in het geval van verwachte service wordt onderbroken).

**Server.** U bewaren resultaten die specifiek zijn voor een enkele gebruiker veilig op een server die u beheren en weergeven van de ingehouden resultaten alleen:

- om in te schakelen van de eindgebruiker toegang tot een historisch rapport van de resultaten eerder geretourneerd voor die gebruiker in uw oplossing, mits de resultaten niet (i) gedurende meer dan 21 dagen vanaf het moment van de eerste query van de eindgebruiker behouden mogelijk en (ii) in reactie op een einde u weergegeven Ser van nieuwe of herhaalde query. of
- persoonlijke in afwachting van een eindgebruiker behoeften op basis van signalen die van de gebruiker voor de laagste van (i) 24 uur vanaf het moment van de query of (ii) tot en met een eindgebruiker dient een andere query voor de bijgewerkte resultaten voor het opslaan van resultaten die voor uw proactieve query zijn geretourneerd.

Indien aanwezig, resultaten voor een specifieke gebruiker kunnen niet worden commingled met de resultaten van een andere gebruiker, dat wil zeggen, de resultaten van elke gebruiker moeten worden bewaard en afzonderlijk verkrijgbaar waren in.

**Algemene.** Voor alle presentatie van de ingehouden resultaten:

- een duidelijke, zichtbaar kennisgeving van de tijd waarop die de query is verzonden, bevatten
- presenteren aan de gebruiker een knop of een vergelijkbare manier opnieuw opvragen en de bijgewerkte resultaten verkrijgen 
- de huisstijl van de presentatie van de resultaten, Bing behouden en
- verwijderen (en indien nodig het vernieuwen met een nieuwe query) de opgeslagen resultaten binnen het opgegeven tijdsbestek.

**F. Detectie van de URL niet weergeven.** U kunt de reacties op webzoekopdrachten alleen gebruiken in een internet-zoekervaring voor het enige doel van het detecteren van URL's van bronnen met informatie die reageren op een query van de gebruiker of de klant. U kunt deze URL's kopiëren in een rapport of een vergelijkbaar antwoord dat u (i) alleen voor die gebruiker opgeeft of -klant bent, bevat in reactie op deze query en (ii) die belangrijke aanvullende waardevolle inhoud die relevant zijn voor de query. De vereisten in secties 3A via 3E van deze vereisten voldoet voor gebruik en de weergave niet van toepassing op het gebruik van deze niet weergegeven, met uitzondering van: 

- U wordt niet in de cache, kopiëren of alle gegevens worden opgeslagen of inhoud op of afgeleid zijn van het antwoord van de zoekactie dan het beperkte URL kopiëren die eerder zijn beschreven;
- Zorg ervoor dat uw gebruik van gegevens (inclusief de URL's) van de Search-API's ontvangen geen inbreuk maken op alle van toepassing zijnde wetten of rechten van derden; en
- U mag de gegevens (inclusief de URL's) die worden ontvangen van de Search-API's als onderdeel van een search-index of machine learning- of vergelijkbare algoritmische activiteit geen gebruik maken van de trein, evalueren of services die u of andere bedrijven bieden mogelijk te verbeteren.

## <a name="next-steps"></a>Volgende stappen
[Antwoord zoeken-overzicht](overview.md)
