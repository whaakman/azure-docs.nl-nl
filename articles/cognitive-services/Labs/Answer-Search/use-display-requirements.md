---
title: Vereisten gebruiken en weer geven-project antwoorden zoeken
titlesuffix: Azure Cognitive Services
description: Vereisten gebruiken en weer geven voor het zoek eindpunt van het project antwoord.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: 2b42d61fd887f166a08b78510d5eaacb8a7cdcb8
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706708"
---
# <a name="project-answer-search-use-and-display-requirements"></a>Vereisten voor project antwoorden zoeken en weer geven

De vereisten voor gebruik en weer gave zijn van toepassing op alle implementaties van de inhoud en de bijbehorende informatie, bijvoorbeeld relaties, meta gegevens en andere signalen, die beschikbaar zijn via aanroepen van de Bing kennis zoeken, Bing Aangepaste zoekopdrachten, Entiteiten zoeken, Afbeeldingen zoeken, Nieuws zoeken, Video's zoeken, Visual Search, Webzoekopdrachten, Spellingcontrole en Automatische suggestie-Api's. Implementatie gegevens met betrekking tot deze vereisten vindt u in de documentatie voor specifieke functies en resultaten.

## <a name="1-bing-spell-check-and-bing-autosuggest-api"></a>1. Bing Spellingcontrole en Automatische suggestie-API voor Bing.

Niet doen:

- u kunt alle gegevens die u ontvangt van de Bing Spellingcontrole of Bing Automatische suggesties-Api's kopiëren, opslaan of in de cache plaatsen
- Gebruik gegevens die u van de Bing Spellingcontrole of Bing Automatische suggesties Api's ontvangt als onderdeel van een machine learning of vergelijk bare algoritme activiteit voor het trainen, evalueren of verbeteren van nieuwe of bestaande services die u of derden kunnen aanbieden.

## <a name="2-definitions"></a>2. Definities

- "antwoord" verwijst naar een categorie resultaten die in een antwoord wordt geretourneerd. Een reactie van de Bing Webzoekopdrachten-API kan bijvoorbeeld antwoorden bevatten in de categorieën resultaten van webpagina's, afbeelding, video en nieuws.
- "antwoord" betekent elke en alle antwoorden en bijbehorende gegevens die zijn ontvangen als reactie op één aanroep van een zoek-API;
- "Result" verwijst naar een gegevens item in een antwoord. De set met gegevens die zijn gekoppeld aan één nieuws artikel is bijvoorbeeld een antwoord op een nieuws.
- "Zoek-Api's" betekent gezamenlijk, de Bing Aangepaste zoekopdrachten, Entiteiten zoeken, Afbeeldingen zoeken, Nieuws zoeken, Video's zoeken, Visual Search en Webzoekopdrachten-Api's. 


## <a name="3-search-apis"></a>3. Zoek-API's

De vereisten in deze sectie 3 zijn van toepassing op de zoek-Api's.

**ÉÉN. Zoek ervaring op internet.** Alle gegevens die in reacties worden geretourneerd, mogen alleen worden gebruikt in Internet zoek functies. Een Internet-Zoek ervaring betekent dat de inhoud wordt weer gegeven, indien van toepassing: 
- is relevant en reageert op de directe query van de eind gebruiker of op een andere vermelding van de zoek interesse en het doel van de gebruiker (bijvoorbeeld door de gebruiker aangegeven Zoek query). 
- helpt gebruikers bij het zoeken naar en navigeren naar de gegevens bronnen (de gegeven Url's worden bijvoorbeeld als Hyper links geïmplementeerd, zodat de inhoud of de toewijzing een klik bare koppeling op een zicht bare manier wordt weer gegeven met de gegevens). of, indien van Bing Entiteiten zoeken-API, de beschik bare koppeling naar de bing.com-URL in het antwoord waarmee de gebruiker naar de zoek resultaten voor de relevante query op bing.com kan navigeren.
- bevat meerdere resultaten voor de eind gebruiker om te selecteren (bijvoorbeeld verschillende resultaten van het nieuws antwoord worden weer gegeven of alle resultaten als er minder dan één worden geretourneerd); 
- is beperkt tot een hoeveelheid die geschikt is voor het zoek doel (bijvoorbeeld afbeeldings miniaturen zijn in verhouding tot de weer gave van de gebruiker). 
- bevat zicht bare indicatie van de eind gebruiker dat de inhoud de zoek resultaten van Internet omvat (bijvoorbeeld een verklaring dat de inhoud ' van het web ' is); maar
- omvat alle andere Combi Naties van maat regelen die nodig zijn om ervoor te zorgen dat uw gebruik van gegevens die zijn ontvangen van de zoek-Api's geen overtreding zijn van toepasselijke wetten of rechten van derden (bijvoorbeeld als er wordt gebruikgemaakt van een Creative Commons-licentie, die voldoet aan de toepasselijke licentie voor waarden). Neem contact op met uw juridische adviseur om te bepalen welke maat regelen mogelijk geschikt zijn.
De enige uitzonde ring op de vereiste voor Internet zoekopdracht ervaring is voor URL-detectie zoals beschreven in sectie 3E (detectie van niet-weer gave-URL'S). 

**B. Restrictie.** Niet doen:

- gegevens uit antwoorden kopiëren, opslaan of in de cache bewaren (behalve Bewaar periode voor zover toegestaan door de sectie continuïteit van de service); 
- Gebruik gegevens die zijn ontvangen van de zoek-Api's als onderdeel van een machine learning of vergelijk bare algoritme activiteit voor het trainen, evalueren of verbeteren van nieuwe of bestaande services die u of derden kunnen aanbieden.
- Wijzig de inhoud van de resultaten (met uitzonde ring van het opnieuw Format teren op een manier die geen andere vereiste heeft geschonden), tenzij wettelijk vereist of door micro soft zijn overeengekomen; 
- toewijzing en Url's die zijn gekoppeld aan resultaten inhoud weglaten;
- Volg orde, met inbegrip van weglating, resultaten die worden weer gegeven in een antwoord wanneer een order of rang orde wordt opgegeven (voor de Bing Custom Search-API, is deze regel niet van toepassing op het opnieuw ordenen van de implementatie via de customsearch.ai-Portal), tenzij vereist door de wet of door micro soft zijn overeengekomen ;
- andere inhoud weer geven binnen een deel van een reactie op een manier die een eind gebruiker zou kunnen vermoeden dat de andere inhoud deel van het antwoord is. 
- advertenties weer geven die niet door micro soft zijn verschaft op een pagina waarop een deel van een antwoord wordt weer gegeven; -alle advertenties weer geven met antwoorden (i) van de Bing-installatie kopie, nieuws of Video's zoeken-Api's; of (II) die voornamelijk (of uitsluitend) worden gefilterd of beperkt tot afbeeldingen, nieuws en/of video resultaten.

**G. Kennisgevingen en merken.** 

- U kunt een functionele Hyper Link toevoegen aan de privacyverklaring van micro soft, https://go.microsoft.com/fwlink/?LinkId=521839 die beschikbaar is op, bijna elk punt in de gebruikers ervaring (UX) waarmee een gebruiker een zoek opdracht kan invoeren. Label de Hyper Link micro soft privacyverklaring.
- De Bing-huis stijl duidelijk weer geven, in overeenstemming met https://go.microsoft.com/fwlink/?linkid=833278 de richt lijnen die beschikbaar zijn op, bijna elk punt in de UX, waarmee een gebruiker een zoek opdracht kan invoeren.  Een dergelijke branding moet duidelijk aangeven aan welke gebruiker micro soft de Internet zoekmachine inschakelt.
- U kunt elk antwoord (of gedeelte van een antwoord) dat wordt weer gegeven door de Bing Web-, afbeeldings-, nieuws-en video- https://go.microsoft.com/fwlink/?linkid=833278 api's naar micro soft kenmerken, tenzij micro soft schrijft dat er iets anders is geschreven voor uw gebruik. 
- Geen kenmerk Reacties (of delen van antwoorden) die worden weer gegeven van de Bing Custom Search-API naar micro soft, tenzij micro soft schrijft dat er iets anders is voor uw specifieke gebruik.


**!. Reacties worden overgedragen.** Als u een gebruiker in staat stelt om een reactie over te dragen van een zoek-API naar een andere gebruiker, zoals via een bericht-app of een boeking van de sociale media, is het volgende van toepassing: 
- Verzonden reacties moeten:
  - Bestaan uit inhoud die niet is gewijzigd ten opzichte van de inhoud van de antwoorden die worden weer gegeven aan de overdrachts gebruiker (wijzigingen in de opmaak zijn toegestaan).
  - Geen gegevens in het formulier meta gegevens bevatten.
  - Voor antwoorden van de Bing Web-, afbeeldings-, nieuws-en video-Api's is de weergave taal die het antwoord aangeeft, verkregen via een Internet Zoek ervaring die is ingeschakeld door Bing (bijvoorbeeld ' Powered by Bing ', ' "meer informatie over deze installatie kopie op Bing ' of het Bing-logo gebruiken).
  - Voor reacties van de Bing Custom Search-API is de weergave taal die het antwoord aangeeft, verkregen via een Internet-Zoek ervaring (bijvoorbeeld "meer informatie over dit Zoek resultaat").
  - De volledige query die wordt gebruikt voor het genereren van het antwoord prominent weer geven. maar
  - Neem een prominente koppeling of soort gelijke toewijzing aan de onderliggende bron van het antwoord op, hetzij rechtstreeks, hetzij via de zoek machine (bing.com, m.bing.com of uw aangepaste zoek service, indien van toepassing).
- U kunt de overdracht van antwoorden niet automatiseren. Een overdracht moet duidelijk worden geïnitieerd door een actie van een gebruiker om een evidencing te kunnen overdragen.
- U kunt alleen een gebruiker in staat stellen reacties over te dragen die worden weer gegeven als reactie op de query van de overdrachts gebruiker.

**E. Continuïteit van de service.** Geen gegevens uit de zoek-API-antwoorden kopiëren, opslaan of in de cache plaatsen. Om continuïteit van service toegang en gegevens rendering mogelijk te maken, kunt u de resultaten echter alleen onder de volgende omstandigheden behouden:

**Apparaatconfiguratie.** U kunt een eind gebruiker in staat stellen om de resultaten te bewaren op een apparaat met een lagere (i) 24 uur na het tijdstip van de query of (II) totdat een eind gebruiker een andere query voor bijgewerkte resultaten indient, mits de Inge houden resultaten alleen mogen worden gebruikt:

- om ervoor te zorgen dat de eind gebruiker toegang krijgt tot resultaten die eerder zijn geretourneerd naar die eind gebruiker op dat apparaat (bijvoorbeeld in het geval van een onderbreking van de service). of
- om de resultaten op te slaan die zijn geretourneerd voor uw proactieve query, worden de behoeften van de eind gebruiker gepersonaliseerd op basis van de signalen van de eind gebruiker (bijvoorbeeld in het geval van een verwachte service onderbreking).

**Naam.** U kunt resultaten die specifiek zijn voor een enkele eind gebruiker veilig bewaren op een server die u beheert en alleen de behouden resultaten weer geven:

- om ervoor te zorgen dat de eind gebruiker toegang krijgt tot een historisch rapport met resultaten dat in uw oplossing eerder naar die gebruiker is geretourneerd, mits de resultaten mogelijk niet (i) worden bewaard gedurende meer dan 21 dagen vanaf het moment waarop de eerste query van de eind gebruiker en (II) worden weer gegeven als reactie op een end u de nieuwe of herhaalde query van ser; of
- om de resultaten op te slaan die voor uw proactieve query worden geretourneerd, is het aan te raden de behoeften van een eind gebruiker op basis van de signalen van die eind gebruiker te bewaren, voor een lagere periode (i) 24 uur vanaf het moment van de query of (II) totdat een eind gebruiker een andere query verzendt voor bijgewerkte resultaten.

Wanneer het behouden blijft, kunnen de resultaten voor een specifieke gebruiker niet worden commingled met de resultaten van een andere gebruiker, dat wil zeggen dat de resultaten van elke gebruiker afzonderlijk moeten worden bewaard en geleverd.

**Algemene.** Voor alle presentatie van behouden resultaten:

- een duidelijke, zicht bare kennisgeving van het tijdstip van verzen ding van de query toevoegen
- Zorg ervoor dat de gebruiker een knop of een vergelijk bare manier geeft om bijgewerkte resultaten opnieuw op te vragen en te verkrijgen. 
- Bewaar de Bing-huis stijl in de presentatie van de resultaten en
- de opgeslagen resultaten binnen de opgegeven tijds bestek verwijderen (en zo nodig vernieuwen met een nieuwe query).

**F. Niet-weer gave van URL-detectie.** U kunt Zoek reacties alleen in een niet-Internet zoek opdracht gebruiken voor het detecteren van Url's van gegevens bronnen die reageren op een query van uw gebruiker of klant. U kunt dergelijke Url's in een rapport of een soortgelijk antwoord dat u (i) verstrekt, alleen naar die gebruiker of klant kopiëren als reactie op die query en (II), met inbegrip van belang rijke extra waardevolle inhoud die relevant is voor de query. De vereisten in de secties 3A tot en met 3E van deze gebruiks-en weergave vereisten zijn niet van toepassing op dit niet-weer gave-gebruik, met uitzonde ring van: 

- Het is niet mogelijk om gegevens of inhoud op te slaan in de cache of te kopiëren van, of af te leiden van, het antwoord op de zoek opdracht, met uitzonde ring van de beperkte URL-kopie die eerder is beschreven.
- Zorg ervoor dat uw gebruik van gegevens (inclusief de Url's) die zijn ontvangen van de zoek-Api's geen inbreuk maakt op toepasselijke wetten of rechten van derden; maar
- U mag de gegevens (inclusief de Url's) die zijn ontvangen van de zoek-Api's, niet gebruiken als onderdeel van een zoek index of machine learning of vergelijk bare algoritmen voor het maken van Train-, evaluatie-of Verbeter services die u of derden kunnen aanbieden.

## <a name="next-steps"></a>Volgende stappen
[Overzicht van antwoorden zoeken](overview.md)
