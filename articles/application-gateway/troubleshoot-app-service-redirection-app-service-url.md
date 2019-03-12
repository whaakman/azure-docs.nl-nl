---
title: Oplossen van problemen met Azure Application Gateway met App Service-omleiding naar de URL van App Service
description: Dit artikel bevat informatie over het oplossen van het probleem omleiding van Azure Application Gateway wordt gebruikt in combinatie met Azure App Service
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: cd15e139b2bcd0046d2cfbd7603809936adf1cfc
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57548129"
---
# <a name="troubleshoot-application-gateway-with-app-service--redirection-to-app-services-url"></a>Problemen met Application Gateway oplossen met App Service – Omleiding naar de URL van App Service

 Informatie over het diagnosticeren en oplossen van problemen omleiden met Application Gateway waar de App Service-URL is ophalen blootgesteld.

## <a name="overview"></a>Overzicht

Wanneer u een openbare App Service in de back-endadresgroep van Application Gateway gerichte configureren en als u een omleiding geconfigureerd in de code van uw toepassing hebt, u die tegenkomen kunt wanneer u toegang Application Gateway tot, wordt u omgeleid door de browser rechtstreeks naar de App. Service-URL.

Dit probleem kan optreden vanwege de volgende belangrijke redenen:

- Hebt u omleiding geconfigureerd op uw App Service. Omleiding is net zo eenvoudig als een afsluitende schuine streep toe te voegen aan de aanvraag.
- Hebt u Azure AD-verificatie die ervoor zorgt de omleiding dat.
- U kunt de switch 'Kies Host naam van back-end-adres' in de HTTP-instellingen van Application Gateway hebt ingeschakeld.
- U hebt geen uw aangepaste domein dat is geregistreerd bij uw App Service.

## <a name="sample-configuration"></a>Voorbeeldconfiguratie

- HTTP Listener: Basic- of multi-site
- Back-end-adresgroep: App Service
- HTTP-instellingen: "Hostnaam Kies uit meer dan back-Endadres' ingeschakeld
- Test: "Hostnaam Kies uit meer dan HTTP-instellingen' ingeschakeld

## <a name="cause"></a>Oorzaak

Een App Service kunnen alleen worden geopend met de geconfigureerde hostnamen in de instellingen voor aangepast domein standaard, is het "example.azurewebsites.net" en als u wilt toegang tot uw App Service met behulp van Application Gateway met een hostnaam is niet geregistreerd in App Service of met Application Gateway de FQDN-naam, die u hebt voor de onderdrukking van de hostnaam in de oorspronkelijke aanvraag aan de hostnaam van de App Service.

Om dit te doen met Application Gateway, wordt de switch 'Kies hostnaam van back-end-adres' gebruiken in de HTTP-instellingen en voor de test om te werken, gebruiken we 'Kies hostnaam van back-end-HTTP-instellingen' in de Testconfiguratie.

![appservice-1](.\media\troubleshoot-app-service-redirection-app-service-url\appservice-1.png)

Hiervan wanneer de App Service een omleiding biedt gebruikt de hostnaam 'example.azurewebsites.net' in de Location-header in plaats van de oorspronkelijke hostnaam, tenzij anders is geconfigureerd. U kunt het voorbeeld van de aanvraag- en reactieheaders hieronder controleren.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://example.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=example.azurewebsites.net

X-Powered-By: ASP.NET
```
In het bovenstaande voorbeeld ziet u dat de antwoord-header statuscode 301 voor omleiding heeft en de location-header van de App Service-hostnaam in plaats van de oorspronkelijke hostnaam 'www.contoso.com'.

## <a name="solution"></a>Oplossing

Dit probleem kan worden opgelost door niet met een omleiding aan de toepassing, maar als dit niet mogelijk is, geven we de host-header die Application Gateway ontvangt met de App Service ook in plaats van een onderdrukking host doen.

Zodra we dat gaan doen, App Service doet de omleiding (indien aanwezig) op de dezelfde, oorspronkelijke host-header is die naar de Application Gateway verwijst en niet een eigen.

Om dit te doen, moet u een aangepast domein en volg de procedure die hieronder worden vermeld.

- Het domein aan de lijst aangepaste domein van de App Service registreren. Hiervoor moet u een CNAME in uw aangepaste domein die verwijst naar de App Service-FQDN hebben. Zie voor meer informatie, [een bestaande aangepaste DNS-naam toewijzen in Azure App Service](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![appservice-2](.\media\troubleshoot-app-service-redirection-app-service-url\appservice-2.png)

- Wanneer dat is gebeurd, wordt uw App Service klaar om te accepteren van de hostnaam 'www.contoso.com'. Wijzig nu de CNAME-vermelding in DNS naar het beheerpunt waarmee deze terug naar de FQDN van de toepassingsgateway. Bijvoorbeeld, "appgw.eastus.cloudapp.azure.com'.

- Zorg ervoor dat uw domein 'www.contoso.com' wordt omgezet naar de FQDN van de toepassingsgateway, wanneer u een DNS-query.

- Stel uw aangepaste test 'Kies hostnaam van back-end-HTTP-instellingen' uitschakelen. Dit kan worden gedaan via de portal Hiervoor schakelt het selectievakje in de test-instellingen en Ga in PowerShell met behulp van de - PickHostNameFromBackendHttpSettings niet in de opdracht Set-AzApplicationGatewayProbeConfig. Voer in het veld hostnaam van de test uw App Service de FQDN-naam 'example.azurewebsites.net' als de test-aanvragen verzonden van Application Gateway wordt dit in de host-header.

  > [!NOTE]
  > Terwijl u de volgende stap uitvoert, zorg ervoor dat uw aangepaste test is niet gekoppeld aan uw back-end-HTTP-instellingen omdat de HTTP-instellingen nog steeds de switch 'Kies hostnaam van back-end-adres' is ingeschakeld op dit moment heeft.

- Uw Application-Gateway HTTP-instellingen om uit te schakelen 'Kies hostnaam van back-end-adres' instellen. Dit kan vanuit de portal worden gedaan door het selectievakje uitschakelt en de PickHostNameFromBackendAddress - Ga in PowerShell met behulp van niet in de opdracht Set-AzApplicationGatewayBackendHttpSettings.

- Koppelen van de aangepaste test terug naar de back-end-HTTP-instellingen en controleer of de back-endstatus als deze in orde is.

- Zodra dit is gebeurd, Application Gateway moet nu de dezelfde hostnaam 'www.contoso.com' doorsturen naar de App-Service en de omleiding wordt uitgevoerd op de dezelfde hostnaam. U kunt het voorbeeld van de aanvraag- en reactieheaders hieronder controleren.
```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
```
## <a name="next-steps"></a>Volgende stappen

Als de voorgaande stappen het probleem niet verhelpen, opent u een [ondersteuningsticket](https://azure.microsoft.com/support/options/).
