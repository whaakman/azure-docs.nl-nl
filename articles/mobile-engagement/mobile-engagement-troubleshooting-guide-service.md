---
title: Azure Mobile Engagement Troubleshooting Guide - Service
description: Troubleshooting Guide voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f13fd0540b783120014b3a8d4e41f78808c7fade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a>Gids voor probleemoplossing voor problemen met de Service
Hieronder vindt u mogelijke problemen die optreden bij hoe Azure Mobile Engagement wordt uitgevoerd.

## <a name="service-outages"></a>Serviceonderbrekingen
### <a name="issue"></a>Probleem
* Problemen die worden veroorzaakt door Azure Mobile Engagement serviceonderbrekingen worden weergegeven.

### <a name="causes"></a>Oorzaken
* Problemen die worden veroorzaakt door Azure Mobile Engagement serviceonderbrekingen lijken kunnen zijn veroorzaakt door verschillende andere problemen:
  * Geïsoleerde problemen die oorspronkelijk al aan alle Azure Mobile Engagement weergegeven
  * Bekende problemen veroorzaakt door serverstoringen (die niet altijd wordt weergegeven in de serverstatus):
  * Voor de planning vertragingen, fouten Targeting, problemen Badge-update, statistieken stop verzamelen, werkt niet voor Push kan niet-API's meer werken, nieuwe apps of gebruikers worden gemaakt, DNS-fouten en time-outfouten in de gebruikersinterface, API of Apps op een apparaat.
  * Afhankelijkheid uitval cloud [Azure servicestatus](http://status.azure.com/)
  * Push Notification Services (PNS) afhankelijkheid uitval
  * App Store uitval

1) Als u wilt testen om te zien als het probleem al is kunt u dezelfde functie uit een andere testen

* Azure Mobile Engagement geïntegreerde toepassing
* Testapparaat
* Testversie van het besturingssysteem van het apparaat
* Campagne
* Beheerdersaccount
* Browser (IE, Firefox, Chrome, enz.)
* Computer

2) Als het probleem alleen betrekking heeft op de gebruikersinterface of de API testen:

* Dezelfde functie uit de Azure Mobile Engagement-gebruikersinterface en de Azure Mobile Engagement API testen.

3) Als het probleem met uw mobiele telefoonnetwerk is testen:

* Test tijdens verbinding met Internet via Wi-Fi en via uw mobiele telefoon 3G netwerk is verbonden.
* Bevestig dat uw firewall niet worden geblokkeerd van de Azure Mobile Engagement IP-adressen of -poorten.

4) Als het probleem met uw apparaat is testen:

* Als het apparaat verbinding kunnen maken met Azure Mobile Engagement met een andere geïntegreerde Azure Mobile Engagement-app testen.
* Test die u kunt gebeurtenissen, taken en Crashes van uw telefoon die kan worden weergegeven in de gebruikersinterface van Azure Mobile Engagement genereren. 
* Testen of u pushmeldingen verzenden vanuit de Azure Mobile Engagement-gebruikersinterface op uw apparaat op basis van de apparaat-ID kunt. 

5) Als het probleem met uw App is testen:

* Installeren en testen van de toepassing van een emulator in plaats van vanaf een fysiek apparaat:

6) Om te controleren of het probleem is met upgrades voor het besturingssysteem voor de eindgebruiker apparaten waarvoor een upgrade van de SDK om op te lossen:

* Test uw toepassing op verschillende apparaten met verschillende versies van het besturingssysteem.
* Controleer of u van de meest recente versie van de SDK gebruikmaakt.

## <a name="connectivity-and-incorrect-information-issues"></a>Connectiviteit en onjuiste informatie over problemen
### <a name="issue"></a>Probleem
* Problemen bij het aanmelden bij de Azure Mobile Engagement-gebruikersinterface.
* Verbindingsfouten met de Azure Mobile Engagement API.
* Problemen met het uploaden van App-Info labels via de apparaat-API.
* Problemen bij het downloaden van Logboeken of de geëxporteerde gegevens van Azure Mobile Engagement.
* Onjuiste informatie weergegeven in de gebruikersinterface van Azure Mobile Engagement.
* Onjuiste informatie die wordt weergegeven in Azure Mobile Engagement-Logboeken.

### <a name="causes"></a>Oorzaken
* Bevestig dat uw gebruikersaccount heeft onvoldoende machtigingen om uit te voeren van de taak.
* Bevestig dat het probleem niet geïsoleerd, zodat één computer of uw lokale netwerk is.
* Controleer of het uitval moet worden gemeld dat de service Azure Mobile Engagement geen heeft.
* Bevestig dat uw App-Info label bestanden al deze regels volgen:
  * Gebruik alleen de UTF8-tekenset (de ANSI-tekenset wordt niet ondersteund).
  * Gebruik een komma "," als scheidingsteken (u kunt een service om aan te vragen het CSV-scheidingsteken wijzigen vanaf een door komma's openen "," naar een ander teken, zoals een puntkomma ';').
  * Gebruik voor Boole-waarden alle kleine letters 'true' en 'false'.
  * Gebruik een bestand dat kleiner is dan de maximale bestandsgrootte van 35MB.

