---
title: Azure Mobile Engagement Troubleshooting Guide - Push/Reach
description: Problemen met het gebruiker interactie en notification in Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3f1886b7-1fdd-47f4-b6b0-d79f158d5ef3
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: ef6f34404b97a6972fc136262920a1bdbc4117b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Handleiding voor het oplossen van Push- en bereikproblemen
Hieronder vindt u mogelijke problemen die optreden bij hoe Azure Mobile Engagement gegevens naar uw gebruikers verzendt.

## <a name="push-failures"></a>Mislukte push
### <a name="issue"></a>Probleem
* Pushmeldingen werken (per app buiten de app, of beide) niet.

### <a name="causes"></a>Oorzaken
* Aantal keren dat een push-fout wordt aangegeven dat Azure Mobile Engagement, bereik of een andere geavanceerde functie van Azure Mobile Engagement is niet correct worden geïntegreerd, of een upgrade is vereist in de SDK herstellen van een bekend probleem met een nieuw besturingssysteem of de apparaat-platform.
* Een In-App-push en alleen een push uit van de toepassing om te bepalen of er een probleem In App of buiten App testen.
* Test u uit de gebruikersinterface en de API als een stap om te zien welke aanvullende foutinformatie beschikbaar beide plaatsen.
* Buiten de App werken pushes alleen als zowel Azure Mobile Engagement en het bereik zijn geïntegreerd in de SDK.
* Pushes werkt niet als certificaten ongeldig zijn of PROD tegenover gebruikt. DEV correct (alleen iOS). (**Opmerking:** 'out of app' pushmeldingen kunnen niet worden bezorgd bij iOS-, hebt u de ontwikkeling (DEV) en de Productieversies (PROD) van uw toepassing is geïnstalleerd op hetzelfde apparaat, omdat het beveiligingstoken die zijn gekoppeld aan uw certificaat mogelijk ongeldig worden gemaakt van Apple. U lost dit probleem, ontwikkeling en PROD versies van uw toepassing te verwijderen en alleen de ene versie op uw apparaat opnieuw te installeren.)
* Buiten App push aantallen zijn verwerkt op verschillende platforms (iOS ziet u minder informatie dan Android als systeemeigen pushes zijn uitgeschakeld op een apparaat, de API kan vindt u meer informatie dan de gebruikersinterface voor de push-statistieken).
* Buiten de App kunnen pushes worden geblokkeerd door klanten op niveau van het besturingssysteem (iOS en Android).
* Buiten de App wordt pushes weergegeven als uitgeschakeld in de gebruikersinterface van Azure Mobile Engagement als ze zijn niet correct worden geïntegreerd, maar in de achtergrond van de API mislukken.
* In-App werken pushes alleen als zowel Azure Mobile Engagement en het bereik zijn geïntegreerd in de SDK.
* GCM en ADM pushes werken alleen als Azure Mobile Engagement en de server zijn geïntegreerd in de SDK (alleen Android).
* In de App en buiten App moeten pushes afzonderlijk worden getest om te bepalen of er een probleem met de Push- of bereik.
* In de App pushes vereisen dat de app openen om te worden ontvangen.
* In-App zijn pushes vaak setup moet worden gefilterd door een opt-in- of opt-out app info-tag.
* Als u een aangepaste categorie in Reach in-app-meldingen worden weergegeven, u moet de juiste levenscyclus van de melding volgen, anders kan de melding niet worden gewist wanneer de gebruiker genegeerd.
* Als u een campagne met geen einddatum start en een apparaat ontvangt de app in melding maar matcht niet weergeven het nog, de gebruiker wordt nog steeds ontvangen het bericht de volgende keer dat ze zich aanmelden bij de app, zelfs als u de campagne handmatig beëindigen.
* Bevestig dat u echt gebruiken van de Push-API in plaats van de Reach API wilt (omdat de Reach API vaker wordt gebruikt) en dat u de parameters 'nettolading' en 'melder' niet zijn verwarrend voor problemen met de Push-API.
* Test uw pushcampagne met zowel een apparaat is verbonden via Wi-Fi- en 3G de netwerkverbinding als mogelijke bron van de problemen elimineren.

## <a name="push-testing"></a>Push testen
### <a name="issue"></a>Probleem
* Pushes kunnen worden verzonden naar een specifiek apparaat op basis van een apparaat-ID.

### <a name="causes"></a>Oorzaken
* Testapparaten setup anders voor elk platform zijn, maar veroorzaakt door een gebeurtenis in uw app op een testapparaat en zoekt u uw apparaat-ID in de portal moeten werken om uw apparaat-ID vinden voor alle platforms.
* Testapparaten werken anders met IDFA vs. IDFV (alleen iOS).

## <a name="push-customization"></a>Push-aanpassing
### <a name="issue"></a>Probleem
* Geavanceerde push inhoud item werkt niet (badge, ring, Trillen, afbeeldingen, enzovoort).
* Koppelingen van pushes werken (buiten de app in een website, naar een locatie in app-app) niet.
* Push-statistieken weergeven dat een push niet naar zo veel mensen zoals verwacht verzonden is (te veel of is niet voldoende).
* Push gedupliceerd en twee keer ontvangen.
* Kan testapparaat niet registreren voor Azure Mobile Engagement Pushes (met uw eigen app Prod of DEV).

### <a name="causes"></a>Oorzaken
* Als u wilt koppelen aan een specifieke locatie in de app is vereist 'categorieën' (alleen Android).
* Grondige koppelen schema's om gebruikers te leiden naar een alternatieve locatie wanneer u een push-melding op moeten zijn gemaakt en beheerd door uw toepassing en het besturingssysteem van het apparaat niet door de Mobile Engagement rechtstreeks. (**Opmerking:** buiten de app meldingen kunnen geen koppeling rechtstreeks naar op locaties met iOS-app zoals dat bij Android.)
* De installatiekopie van de externe servers moeten kunnen voor gebruik van HTTP 'GET' en 'HEAD"voor grote afbeelding pushes (alleen Android) werken.
* U kunt in uw code, schakel de Azure Mobile Engagement-agent uit wanneer het toetsenbord wordt geopend en hebben de code die de Azure Mobile Engagement-agent opnieuw te activeren als het toetsenbord is gesloten, zodat het toetsenbord niet van invloed op de weergave van de melding (alleen iOS).
* Sommige items werken niet in de test simulaties, maar alleen echte campagnes (badge, ring, Trillen, afbeeldingen, enzovoort).
* Er is geen servergegevens wordt geregistreerd wanneer u met de knop 'test' pushes. Gegevens worden alleen geregistreerd voor echte pushcampagnes.
* Om te isoleren van uw probleem, oplossen met: test, simuleren, en een echte campagne omdat ze elk iets anders werkt.
* Levering getallen kan van invloed zijn de tijdsduur die uw "in de app' en 'elke keer' campagnes zijn gepland omdat een campagne worden alleen gebruikers die zich 'in de app" terwijl de campagne wordt uitgevoerd (en gebruikers die hun apparaatinstellingen instellen voor het ontvangen van meldingen 'out hebben geleverd van de app').
* De verschillen tussen hoe Android en iOS verwerkt buiten de app-meldingen maakt het moeilijk zijn direct push statistieken vergelijken tussen de Android en iOS-versie van uw toepassing. Android biedt meer OS niveau melding informatie dan iOS. Android rapporten wanneer een systeemeigen melding is ontvangen, waarop is geklikt of verwijderd uit het meldingencentrum, maar iOS wordt deze informatie niet gerapporteerd, tenzij de melding wordt geklikt. 
* De belangrijkste reden 'pushed' getallen anders dan andere dan 'geleverd' getallen voor campagnes bereikt dat 'in-app' en 'out of app' meldingen worden geteld anders zijn. 'In-app' meldingen worden verwerkt door de Mobile Engagement, maar 'out of app' meldingen worden verwerkt door het meldingencentrum in het besturingssysteem van uw apparaat.

## <a name="push-targeting"></a>Push-doelen
### <a name="issue"></a>Probleem
* Ingebouwd in als doel niet werkt zoals verwacht.
* App-gegevens Tag targeting werkt niet zoals verwacht.
* Geografische locatie als doel werkt niet zoals verwacht.
* Taalopties werken niet zoals verwacht.

### <a name="causes"></a>Oorzaken
* Zorg ervoor dat u app-info labels via de Azure Mobile Engagement UI of de API hebt geüpload.
* Beperking van de pushsnelheid of push quotum op toepassingsniveau, of het beperken van de doelgroep op het niveau van de campagne kunt voorkomen dat een persoon ontvangt een specifieke push zelfs als ze voldoen aan uw doel criteria. 
* Het instellen van een 'taal' is anders dan die gericht is op basis van land of landinstellingen, dit ook anders is dan die gericht is op basis van geografische locatie op basis van een telefoon locatie of GPS-locatie.
* Het bericht in de standaardtaal' ' is verzonden naar een klant die geen hun apparaat instellen op een van de alternatieve talen die u opgeeft.

## <a name="push-scheduling"></a>Push plannen
### <a name="issue"></a>Probleem
* Push planning werkt niet zoals verwacht (verzonden te vroeg of vertraagde).

### <a name="causes"></a>Oorzaken
* Tijdzones kunnen problemen met het plannen, met name wanneer u de tijdzone van de eindgebruikers.
* Geavanceerde push-functies kunnen pushes uitstellen.
* Die gericht is op basis van de telefoon instellingen (in plaats van de labels van App-Info) kunnen uitstellen duwt omdat Azure Mobile Engagement u gegevens opvraagt uit de real-time telefoon moet mogelijk voor het verzenden van een push.
* Campagnes gemaakt zonder een einddatum lokaal opslaan van de push-bewerking op het apparaat en weergeven in de volgende keer dat de app wordt geopend, zelfs als de campagne handmatig wordt beëindigd.
* Meer dan een campagne wordt gestart op hetzelfde moment kan langer duren om te scannen van uw gebruikersgroep (probeer te begint pas met een campagne tegelijk met een maximum van vier, ook doel alleen voor uw actieve gebruikers dat oude gebruikers hoeven te worden gescand).
* Als u de optie 'Negeren doelgroep, push wordt naar gebruikers verzonden via de API' in de sectie "Campagne" van een Reach-campagne gebruiken, de campagne wordt niet automatisch verzenden, moet u handmatig verzenden via de Reach API.
* Als u een aangepaste categorie in Reach in-app-meldingen worden weergegeven, u moet de juiste levenscyclus van een melding te volgen, anders kan de melding niet worden gewist wanneer de gebruiker genegeerd.

