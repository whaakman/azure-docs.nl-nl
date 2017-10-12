---
title: Mobile Engagement-concepten | Microsoft Docs
description: Azure Mobile Engagement-concepten
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8d19abd1-0a6c-4772-9fa5-5e99980ac5da
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 8450651528007b4527366b89a6ad7615169f93c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile Engagement-concepten
Mobile Engagement definieert een aantal algemene concepten voor alle ondersteunde platforms. Dit artikel biedt een korte beschrijving van deze concepten.

Dit artikel is een goede start als u niet bekend bent met Mobile Engagement. Lees ook de documentatie voor het specifieke platform dat u gebruikt, omdat daarin meer details en voorbeelden worden gegeven voor bepaalde concepten, inclusief eventuele beperkingen.

## <a name="devices-and-users"></a>Apparaten en gebruikers
Mobile Engagement identificeert gebruikers door een unieke id voor elk apparaat te genereren. Deze id heet de apparaat-id (of `deviceid`). Deze wordt zodanig gemaakt dat alle toepassingen die worden uitgevoerd op hetzelfde apparaat, dezelfde apparaat-id delen.

Dit betekent dat Mobile Engagement ervan uitgaat dat één apparaat hoort bij één gebruiker, waardoor gebruikers en apparaten equivalente concepten zijn.

## <a name="sessions-and-activities"></a>Sessies en activiteiten
Een sessie is een gebruik van de toepassing door een gebruiker, vanaf het begin van het gebruik tot het einde van het gebruik.

Een activiteit is een gebruik van een bepaald gedeelte van de toepassing door één gebruiker (meestal is dit een scherm, maar het kan van alles zijn dat hoort bij de toepassing).

Een gebruiker kan slechts één activiteit tegelijk uitvoeren.

Een activiteit wordt aangeduid met een naam (maximaal 64 tekens) en kan eventueel extra gegevens insluiten (maximaal 1024 bytes).

Sessies worden automatisch berekend op basis van de volgorde van activiteiten die worden uitgevoerd door gebruikers. Een sessie begint wanneer de gebruiker de eerste activiteit start en stopt wanneer de laatste activiteit is voltooid. Dit betekent dat een sessie niet expliciet hoeft te worden gestart of gestopt. In plaats daarvan worden activiteiten expliciet gestart of gestopt. Als er geen activiteit wordt gerapporteerd, wordt er ook geen sessie gerapporteerd.

## <a name="events"></a>Gebeurtenissen
Gebeurtenissen worden gebruikt voor het rapporteren van directe acties (zoals een knop die wordt ingedrukt of een artikel dat wordt gelezen door gebruikers).

Een gebeurtenis kan zijn gerelateerd aan de huidige sessie of een actieve taak, of kan een zelfstandige gebeurtenis zijn.

Een gebeurtenis wordt aangeduid met een naam (maximaal 64 tekens) en kan eventueel extra gegevens insluiten (maximaal 1024 bytes).

## <a name="error"></a>Fout
Fouten worden gebruikt voor het rapporteren van problemen die correct worden gedetecteerd door de toepassing (zoals onjuist gebruikersacties of foute API-aanroepen).

Een fout kan zijn gerelateerd aan de huidige sessie of een actieve taak, of kan een zelfstandige fout zijn.

Een fout wordt aangeduid met een naam (maximaal 64 tekens) en kan eventueel extra gegevens insluiten (maximaal 1024 bytes).

## <a name="job"></a>Taak
Taken worden gebruikt voor het rapporteren van acties met een duur (zoals de duur van API-aanroepen, de tijd dat advertenties worden weergegeven, de duur van achtergrondtaken of de duur van gebruikersacties).

Een taak is niet gerelateerd aan een sessie omdat een taak kan worden uitgevoerd op de achtergrond, zonder tussenkomst van de gebruiker.

Een taak wordt aangeduid met een naam (maximaal 64 tekens) en kan eventueel extra gegevens insluiten (maximaal 1024 bytes).

## <a name="crash"></a>Crash
Crashes worden automatisch uitgegeven door de Mobile Engagement SDK voor het rapporteren van toepassingsfouten waarbij niet-gedetecteerde problemen ertoe leiden dat de toepassing crasht.

## <a name="application-information"></a>Toepassingsgegevens
Toepassingsgegevens (of app-gegevens) worden gebruikt voor het taggen van gebruikers, dat wil zeggen het koppelen van sommige gegevens aan de gebruikers van een toepassing (dit is vergelijkbaar met cookies op internet, behalve dat app-gegevens worden opgeslagen op de server van het Azure Mobile Engagement-platform).

App-gegevens kunnen worden geregistreerd met de Mobile Engagement SDK-API of met de apparaat-API van het Mobile Engagement-platform.

App-gegevens zijn een sleutel-waardepaar dat is gekoppeld aan een apparaat. De sleutel is de naam van de app-gegevens (maximaal 64 ASCII-letters [a-zA-Z], cijfers [0-9] en onderstrepingstekens [_]). De waarde (maximaal 1024 tekens) kan elke een willekeurige tekenreeks, geheel getal, datum (jjjj-MM-dd) of Booleaanse waarde (waar of onwaar) zijn.

Aan een apparaat kan een willekeurige hoeveelheid app-gegevens worden gekoppeld, binnen de grenzen die worden gesteld door de Mobile Engagement-prijsvoorwaarden. Voor elke sleutel houdt Mobile Engagement alleen de laatste waardegroep bij (geen geschiedenis). Het instellen of wijzigen van de waarden van app-gegevens forceert dat Mobile Engagement de eventuele publiekscriteria die zijn ingesteld voor de app-gegevens opnieuw evalueert. Dit betekent dat app-gegevens kunnen worden gebruikt om realtime pushes te activeren.

## <a name="extra-data"></a>Extra gegevens
Extra gegevens (of extra's) zijn willekeurige gegevens die kunnen worden toegevoegd aan gebeurtenissen, fouten, activiteiten en taken.

Extra's zijn op dezelfde manier gestructureerd als JSON-objecten: ze zijn gemaakt van een boomstructuur van de sleutel-waardeparen. Sleutels zijn beperkt tot 64 ASCII-letters [a-zA-Z], cijfers [0-9] en onderstrepingstekens [_]), en de totale grootte van extra's is beperkt tot 1024 tekens (na codering in JSON door de Mobile Engagement SDK).

De volledige structuur van sleutel-waardeparen wordt opgeslagen als een JSON-object. Toch wordt alleen het eerste niveau van sleutels/waarden opgesplitst om rechtstreeks toegankelijk te zijn voor bepaalde geavanceerde functies zoals segmenten (u kunt bijvoorbeeld eenvoudig een segment definiëren genaamd 'SF-fans' dat bestaat uit alle gebruikers die in de afgelopen maand minstens 10 keer de gebeurtenis met de naam 'content_viewed' hebben verstuurd met de extra sleutel 'content_type ingesteld op de waarde 'sf'). Het is dus raadzaam om alleen extra's te verzenden die bestaan uit eenvoudige lijsten met sleutel-waardeparen met scalaire waarden (bijvoorbeeld tekenreeksen, datums, gehele getallen of Booleaanse waarde).

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van universele Windows-SDK voor Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
* [Overzicht van Windows Phone Silverlight-SDK voor Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
* [iOS-SDK voor Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
* [Android-SDK voor Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)

