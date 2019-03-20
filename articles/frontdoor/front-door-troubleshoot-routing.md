---
title: Probleemoplossing - oplossen van problemen met uw Azure-serviceconfiguratie voor voordeur | Microsoft Docs
description: In deze zelfstudie leert u hoe u voor het zelf oplossen van de veelvoorkomende problemen die zich voor uw voordeur voordoen kunnen.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2018
ms.author: sharadag
ms.openlocfilehash: 7a261d65a7bd3eea150dd764c65b94ddd47466b3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58100306"
---
# <a name="troubleshooting-common-routing-issues"></a>Het oplossen van veelvoorkomende problemen met Routering
Dit artikel wordt beschreven hoe u voor het oplossen van de algemene problemen met routering die zich voor uw Azure voordeur-serviceconfiguratie voordoen kunnen. 

## <a name="hostname-not-routing-to-backend-and-returns-400-status-code"></a>Hostnaam niet routering naar back-end en retourneert 400-statuscode


### <a name="symptom"></a>Symptoom
- U kunt een voordeur hebt gemaakt, maar een aanvraag naar de Frontend-host een HTTP-fout 400-statuscode is geretourneerd.

  - U kunt een DNS-server hebt gemaakt van een aangepast domein toewijzen aan de front-end-host u hebt geconfigureerd. Echter, verzenden van een aanvraag naar de hostnaam van het aangepaste domein een statuscode HTTP 400 geretourneerd en wordt niet weergegeven voor het routeren naar de backend(s) die u hebt geconfigureerd.

### <a name="cause"></a>Oorzaak
- Dit probleem kan zich voordoen als u een regel voor het doorsturen voor het aangepaste domein die u hebt toegevoegd als een front-host niet hebt geconfigureerd. Een regel voor doorsturen moet expliciet worden toegevoegd voor die host frontend, zelfs als een is al geconfigureerd voor de frontend-host onder het subdomein voordeur (*. azurefd.net) dat uw aangepaste domein een DNS-toewijzing aan heeft.

### <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
- Een regel voor het doorsturen van het aangepaste domein toevoegen aan de gewenste back-endpool.

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>Aanvraag voor de Frontend-hostnaam retourneert 404-statuscode

### <a name="symptom"></a>Symptoom
- U hebt een voordeur gemaakt en een front-end-host, een back-endpool met ten minste één back-end erin en een regel voor doorsturen die verbinding de frontend-host naar de back-endpool maakt geconfigureerd. Uw inhoud lijkt niet beschikbaar zijn bij het verzenden van een aanvraag naar de geconfigureerde front-end-host, omdat er een HTTP 404-statuscode is geretourneerd.

### <a name="cause"></a>Oorzaak
Er zijn verschillende mogelijke oorzaken voor dit probleem:
 - De back-end is niet een openbare gerichte back-end en is niet zichtbaar voor de voordeur-service.

- De back-end onjuist is geconfigureerd, dat wordt veroorzaakt door de voordeur-service de verkeerde aanvraag te verzenden (dat wil zeggen, uw back-end accepteert alleen HTTP, maar u dit niet hebt uitgeschakeld zodat HTTPS, zodat de voordeur probeert om door te sturen van HTTPS-aanvragen).
- De back-end wordt afgewezen met de host-header die is doorgegeven aan het verzoek om de back-end.
- De configuratie voor de back-end is nog niet volledig geïmplementeerd.

### <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
1. Implementatietijd
    - Zorg ervoor dat u ongeveer 10 minuten voor de configuratie om te worden geïmplementeerd hebt gewacht.

2. Controleer de instellingen van de back-end
   - Navigeer naar de back endadresgroep die de aanvraag moet worden routering naar (afhankelijk van hoe u de regel voor doorsturen geconfigureerd hebt) en controleer de _back-end hosttype_ en back-end-hostnaam juist zijn. Als de back-end een aangepaste host is, zorgt u ervoor dat u hebt deze correct gespeld. 

   - Controleer uw HTTP en HTTPS-poorten. In de meeste gevallen 80 en 443 (respectievelijk) juist zijn en er zijn geen wijzigingen is vereist. Er is echter een kans is dat uw back-end niet op deze manier is geconfigureerd en een andere poort luistert.

     - Controleer de _host-header van back-end_ geconfigureerd voor de back-ends die de front-end-host moet routering naar. In de meeste gevallen deze header moet gelijk zijn aan de _back-end-hostnaam_. Een onjuiste waarde kan echter verschillende HTTP 4xx-statuscodes veroorzaken als de back-end wordt verwacht iets anders dat. Als u het IP-adres van uw back-end-ingang, moet u mogelijk om in te stellen de _host-header van back-end_ op de hostnaam van de back-end.


3. Controleer de regelinstellingen van de routering
     - Navigeer naar de regel voor doorsturen die uit de betreffende Frontend-hostnaam naar een back-endpool routeren moet. Zorg ervoor dat de toegestane protocollen correct zijn geconfigureerd of als dit niet het geval is, zorg ervoor dat het protocol dat voordeur wordt gebruikt als de aanvraag correct is geconfigureerd. De _geaccepteerd protocollen_ bepaalt welke aanvragen voordeur moet accepteren en de _doorsturen protocol_ onder de _Geavanceerd_ tabblad bepaalt welk protocol voordeur moet gebruiken om de aanvraag doorsturen naar de back-end.
          - Een voorbeeld: als de back-end alleen HTTP-aanvragen accepteert de volgende configuraties kunnen worden gebruikt:
               - _Geaccepteerd protocollen_ zijn HTTP en HTTPS. _Doorsturen van protocol_ is HTTP. Overeenkomst aanvraag werkt niet, aangezien HTTPS een toegestane-protocol is en als een aanvraag afkomstig als HTTPS is, voordeur probeert om door te sturen met behulp van HTTPS.

               - _Geaccepteerd protocollen_ zijn HTTP. _Doorsturen van protocol_ is een aanvraag of HTTPS overeenkomen.

   - Klik op de _Geavanceerd_ tabblad aan de bovenkant van het deelvenster voor configuratie van routering regel. _Herschrijven van URL's_ is standaard uitgeschakeld en moet u dit veld alleen gebruiken als u wilt beperken tot het bereik van de back-end gehoste bronnen die u beschikbaar wilt maken. Als uitgeschakeld, stuurt de voordeur hetzelfde Aanvraagpad die wordt ontvangen. Het is mogelijk dat dit veld is onjuist geconfigureerd en voordeur een resource van de back-end die niet beschikbaar vraagt, dus een HTTP 404-statuscode geretourneerd.

