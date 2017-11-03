---
title: Azure Mobile Engagement Troubleshooting Guide - API 's
description: Troubleshooting Guide voor Azure Mobile Engagement - API 's
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3efc8a52-2b74-4917-b887-815ae8277474
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: piyushjo
ms.openlocfilehash: a7ae0a83046f2d67b790f672dcd3ae261987357a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-api-issues"></a>Gids voor probleemoplossing voor problemen die API
Hieronder vindt u mogelijke problemen die optreden bij de wisselwerking tussen beheerders met Azure Mobile Engagement via de API's.

## <a name="syntax-issues"></a>Syntaxis problemen
### <a name="issue"></a>Probleem
* Syntaxisfouten met behulp van de API (of onverwacht gedrag).

### <a name="causes"></a>Oorzaken
* Problemen met de syntaxis:
  * Controleer of de syntaxis van de specifieke API die u gebruikt om te bevestigen dat de optie beschikbaar is.
  * Een veelvoorkomend probleem met het gebruik van de API is gehaald de Reach API en de Push-API (de meeste taken moeten worden uitgevoerd met de Reach API in plaats van de Push-API). 
  * Een andere veel voorkomende problemen met de SDK-integratie en het gebruik van de API is gehaald de SDK-sleutel en de API-sleutel.
  * Scripts die verbinding met de API's maken nodig om gegevens te verzenden ten minste elke 10 minuten of de verbinding wordt time-out (met name voor Monitor API scripts luisteren naar gegevens). Om te voorkomen dat time-outs, hebt u uw script een ping XMPP elke 10 minuten aan de sessie keepalive met de server verzenden.

### <a name="see-also"></a>Zie ook
* [API-documentatie][Link 4]
* [XMPP Protocol Info](http://xmpp.org/extensions/xep-0199.html)

## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Kan de API gebruiken om uit te voeren dezelfde actie beschikbaar is in de gebruikersinterface van Azure Mobile Engagement
### <a name="issue"></a>Probleem
* Een actie die van de Azure Mobile Engagement-gebruikersinterface werkt werkt van de gerelateerde Azure Mobile Engagement API niet.

### <a name="causes"></a>Oorzaken
* Bevestigen dat u dezelfde actie vanaf de Azure Mobile Engagement-gebruikersinterface uitvoeren kunt, ziet u dat u deze functie van Azure Mobile Engagement correct hebt geïntegreerd met de SDK.

### <a name="see-also"></a>Zie ook
* [UI-documentatie][Link 1]

## <a name="error-messages"></a>Foutberichten
### <a name="issue"></a>Probleem
* Foutcodes met behulp van de API weergegeven tijdens runtime, of in Logboeken.

### <a name="causes"></a>Oorzaken
* Dit is een samengestelde lijst met algemene API status codes getallen voor verwijzing en voorlopige probleemoplossing:
  
        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Zie ook
* [API-documentatie - voor gedetailleerde fouten op elke specifieke API][Link 4]

## <a name="silent-failures"></a>Achtergrond fouten
### <a name="issue"></a>Probleem
* API-actie is mislukt met er is geen foutbericht weergegeven tijdens runtime, of in Logboeken.

### <a name="causes"></a>Oorzaken
* Aantal items wordt in de Azure Mobile Engagement-gebruikersinterface wordt uitgeschakeld als ze zijn niet correct geïntegreerd maar achtergrond mislukken van de API, dus moet u dezelfde functionaliteit van de gebruikersinterface om te zien als werkt testen.
* Azure Mobile Engagement en veel geavanceerde functies van Azure Mobile Engagement u probeert te gebruiken, moeten afzonderlijk integreren in uw app met de SDK als afzonderlijke stappen voordat u ze kunt gebruiken.

### <a name="see-also"></a>Zie ook
* [Troubleshooting Guide - SDK][Link 25]

<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

