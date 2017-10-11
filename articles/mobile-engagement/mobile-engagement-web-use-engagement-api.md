---
title: Azure Mobile Engagement Web SDK-API's | Microsoft Docs
description: De nieuwste updates en procedures voor de webservice-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8a87d5ac-d8b7-4a0d-bdee-414dbcc561b2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: 54c22ce6a03e382b1bbde102bccc97deec249b30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Gebruik de API van Azure Mobile Engagement in een webtoepassing
Dit document is een aanvulling op het document dat wordt bepaald hoe naar [Mobile Engagement integreren in een webtoepassing](mobile-engagement-web-integrate-engagement.md). Het biedt gedetailleerde informatie over het gebruik van de Azure Mobile Engagement-API voor het rapporteren van de toepassingsstatistieken van uw.

De Mobile Engagement-API wordt geleverd door de `engagement.agent` object. De Azure Mobile Engagement Web SDK alias is standaard `engagement`. U kunt deze alias van de SDK-configuratie opnieuw definiëren.

## <a name="mobile-engagement-concepts"></a>Mobile Engagement-concepten
De volgende onderdelen verfijnen algemene [Mobile Engagement-concepten](mobile-engagement-concepts.md) voor de webplatform.

### <a name="session-and-activity"></a>`Session` en `Activity`
Als de gebruiker blijft niet-actief is gedurende meer dan een paar seconden tussen twee activiteiten, wordt de volgorde van de gebruiker van activiteiten splitsen in twee verschillende sessies. Deze enkele seconden worden de sessietime-out genoemd.

Als uw webtoepassing het einde van de activiteiten van de gebruiker niet zelfstandig declareren (door het aanroepen van de `engagement.agent.endActivity` functie), verloopt de Mobile Engagement-server automatisch de gebruikerssessie binnen drie minuten nadat de pagina van de toepassing is gesloten. Dit wordt de sessietime-out van server genoemd.

### `Crash`
Geautomatiseerde rapporten over niet-onderschepte JavaScript-uitzonderingen worden standaard niet worden gemaakt. U kunt echter crashes rapporteren handmatig met behulp van de `sendCrash` (Zie de sectie op reporting crashes).

## <a name="reporting-activities"></a>Rapportage
Rapportage van gebruikersactiviteit omvat zodra een gebruiker een nieuwe activiteit en wanneer de gebruiker de huidige activiteit wordt beëindigd.

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit
    engagement.agent.startActivity("MyUserActivity");

U moet aan te roepen `startActivity()` elke gebruikersactiviteit tijd wordt gewijzigd. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

### <a name="user-ends-the-current-activity"></a>Gebruiker eindigt de huidige activiteit
    engagement.agent.endActivity();

U moet aan te roepen `endActivity()` ten minste eenmaal wanneer de gebruiker is voltooid, de laatste activiteit. Dit informeert de Mobile Engagement Web SDK dat de gebruiker die momenteel niet actief is en dat de sessie van de gebruiker moet worden gesloten nadat de sessietime-out is verlopen. Als u aanroept `startActivity()` voordat de sessietime-out is verlopen, wordt de sessie gewoon hervat.

Omdat er geen betrouwbare aanroep is voor wanneer de navigator-venster wordt gesloten, is het vaak moeilijk of onmogelijk om af te vangen het einde van de activiteiten van de gebruiker binnen een omgeving. Daarom wordt de Mobile Engagement-server automatisch verloopt de sessie van de gebruiker binnen drie minuten nadat de pagina van de toepassing is gesloten.

## <a name="reporting-events"></a>Melden van gebeurtenissen
Rapportage over gebeurtenissen behandelt sessiegebeurtenissen en zelfstandige gebeurtenissen.

### <a name="session-events"></a>Sessiegebeurtenissen
Sessiegebeurtenissen worden meestal gebruikt voor het rapporteren van de acties die worden uitgevoerd door een gebruiker tijdens de sessie van de gebruiker.

**Voorbeeld zonder extra gegevens:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Voorbeeld met extra gegevens:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen
In tegenstelling tot sessiegebeurtenissen, kunnen zelfstandige gebeurtenissen plaatsvinden buiten de context van een sessie.

Gebruik voor die ``engagement.agent.sendEvent`` in plaats van ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Rapportage van fouten
Rapportage van fouten bevat informatie over fouten in sessie en zelfstandige.

### <a name="session-errors"></a>Sessie-fouten
Sessie-fouten worden gewoonlijk gebruikt voor het rapporteren van de fouten die een invloed op de gebruiker tijdens de sessie van de gebruiker hebben.

**Voorbeeld zonder extra gegevens:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Voorbeeld met extra gegevens:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Zelfstandige fouten
In tegenstelling tot sessie fouten, kunnen zelfstandige fouten optreden buiten de context van een sessie.

Gebruik voor die `engagement.agent.sendError` in plaats van `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Rapportage van taken
Rapportage van taken dekt rapportage van fouten en gebeurtenissen die zich tijdens een taak voordoen en de rapportage van crashes.

**Voorbeeld:**

Als u bewaken van een AJAX-aanvraag wilt, gebruikt u het volgende:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Rapportage van fouten tijdens een taak
Fouten kunnen zijn gerelateerd aan een actieve taak in plaats van aan de huidige gebruikerssessie.

**Voorbeeld:**

Als u een fout gemeld wilt als een AJAX-aanvraag is mislukt:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Melden van gebeurtenissen gedurende een project
Gebeurtenissen kunnen zijn gerelateerd aan een actieve taak in plaats van aan de huidige gebruikerssessie dank aan de `engagement.agent.sendJobEvent` functie.

Deze functie werkt precies hetzelfde als `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Reporting crashes
Gebruik de `sendCrash` functie voor het rapport handmatig is vastgelopen.

De `crashid` een tekenreeks die welk type crash aangeeft is.
De `crash` -argument is meestal de stacktracering van de crash als een tekenreeks.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Extra parameters
U kunt willekeurige gegevens koppelen aan een gebeurtenis, fout, activiteit of taak.

De gegevens kan niet een JSON-object (maar niet een matrix of primitief type).

**Voorbeeld:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Limieten
Grenzen die op extra parameters van toepassing zijn op het gebied van reguliere expressies voor sleutels, waardetypen en grootte.

#### <a name="keys"></a>Sleutels
Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dit betekent dat sleutels moeten beginnen met ten minste één letter gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="values"></a>Waarden
Waarden zijn beperkt tot tekenreeks, het aantal en Booleaanse typen.

#### <a name="size"></a>Grootte
Extra's zijn beperkt tot 1024 tekens per aanroep (nadat de Mobile Engagement Web SDK coderen van deze in JSON).

## <a name="reporting-application-information"></a>Rapportage-toepassingsinformatie
U kunt handmatig rapporteren voor het bijhouden van gegevens (of andere toepassingsspecifieke gegevens) met behulp van de `sendAppInfo()` functie.

Houd er rekening mee dat deze gegevens stapsgewijs kunnen worden verzonden. Alleen de meest recente waarde voor een specifieke sleutel wordt bewaard voor een specifiek apparaat.

Als gebeurtenis extra's, kunt u een JSON-object om toepassingsinformatie. Houd er rekening mee dat matrices of onderliggende objecten worden behandeld als platte tekenreeksen (met behulp van de JSON-serialisatie).

**Voorbeeld:**

Hier volgt een voorbeeld van code voor het verzenden van de gebruiker geslacht en Geboortedatum:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Limieten
Limieten die van toepassing op toepassingsgegevens zijn op het gebied van reguliere expressies voor sleutels en grootte.

#### <a name="keys"></a>Sleutels
Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dit betekent dat sleutels moeten beginnen met ten minste één letter gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte
Toepassingsinformatie is beperkt tot 1024 tekens per aanroep (nadat de Mobile Engagement Web SDK coderen van deze in JSON).

In het voorgaande voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

    {"birthdate":"1983-12-07","gender":"female"}
