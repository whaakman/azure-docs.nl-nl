---
title: Problemen met de Azure-toepassing-gateway oplossen met App Service-omleiding naar de URL van App Service
description: Dit artikel bevat informatie over het oplossen van het probleem met de omleiding wanneer Azure-toepassing gateway wordt gebruikt met Azure App Service
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 07/19/2019
ms.author: absha
ms.openlocfilehash: 7baadfb549b19bb12757c82d723578202b5cf8ad
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347879"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>Problemen met App Service oplossen in Application Gateway

Meer informatie over het vaststellen en oplossen van problemen die optreden wanneer app server wordt gebruikt als back-end met Application Gateway

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u de volgende problemen oplost:

> [!div class="checklist"]
> * De URL van de App Service wordt weer gegeven in de browser wanneer er een omleiding is
> * ARRAffinity cookie domein van App Service is ingesteld op App Service hostname (example.azurewebsites.net) in plaats van de oorspronkelijke host

Wanneer een back-end-toepassing een omleidings reactie verzendt, wilt u mogelijk de client omleiden naar een andere URL dan die opgegeven door de back-end-toepassing. Stel dat u dit wilt doen wanneer een app service wordt gehost achter een toepassings gateway en dat de client een omleiding moet doen naar het relatieve pad. (Bijvoorbeeld een omleiding van contoso.azurewebsites.net/path1 naar contoso.azurewebsites.net/path2.) Wanneer de app-service een omleidings reactie verzendt, gebruikt deze dezelfde hostnaam in de locatie header van de reactie als die in de aanvraag die wordt ontvangen van de toepassings gateway. De client gaat de aanvraag dus rechtstreeks naar contoso.azurewebsites.net/path2 in plaats van de Application Gateway (contoso.com/path2). Het overs laan van de toepassings gateway is niet gewenst.

Dit probleem kan de volgende oorzaken hebben:

- U hebt de omleiding geconfigureerd op uw App Service. Omleiding kan net zo eenvoudig zijn als het toevoegen van een afsluitende schuine streep aan de aanvraag.
- U hebt Azure AD-verificatie die de omleiding veroorzaakt.

Wanneer u App Services achter Application Gateway gebruikt, wordt de domein naam die is gekoppeld aan de Application Gateway (example.com), anders dan de domein naam van de App Service (zeg example.azurewebsites.net), omdat u ziet dat de domein waarde voor het ARRAffinity-cookie dat door de App Service is ingesteld, bevat de domein naam ' example.azurewebsites.net '. Dit is niet gewenst. De oorspronkelijke hostnaam (example.com) moet de waarde voor de domein naam in de cookie zijn.

## <a name="sample-configuration"></a>Voorbeeld configuratie

- HTTP-listener: Basis of meerdere locaties
- Back-end-adres groep: App Service
- HTTP-instellingen: ' Hostnaam kiezen van back-end-adres ' ingeschakeld
- Meet De hostnaam van de HTTP-instellingen kiezen is ingeschakeld

## <a name="cause"></a>Oorzaak

Omdat App Service een multi tenant-service is, gebruikt de host-header in de aanvraag om de aanvraag naar het juiste eind punt te sturen. De standaard domein naam van de app-Services, *. azurewebsites.net (zeg contoso.azurewebsites.net) is echter anders dan de domein naam van de toepassings gateway (zeg contoso.com). Omdat de oorspronkelijke aanvraag van de client de domein naam van de toepassings gateway (contoso.com) als de hostnaam heeft, moet u de toepassings gateway configureren om de hostnaam in de oorspronkelijke aanvraag te wijzigen in de hostnaam van de app-Services wanneer de aanvraag wordt doorgestuurd naar de app service-back-end.  U kunt dit doen met behulp van de schakel optie hostname van back-end kiezen in de configuratie van de HTTP-instellingen van de Application Gateway en de switch hostname van back-end-HTTP-instellingen kiezen in de status test configuratie.



![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Als gevolg hiervan wordt de onderdrukkings naam ' contoso.azurewebsites.net ' in de koptekst van de locatie in plaats van de oorspronkelijke hostnaam ' contoso.com ' gebruikt, tenzij anders is geconfigureerd, wanneer de App Service een omleiding doet. U kunt de onderstaande voorbeeld aanvraag en antwoord headers controleren.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://contoso.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=contoso.azurewebsites.net

X-Powered-By: ASP.NET
```
In het bovenstaande voor beeld ziet u dat de antwoord header de status code 301 voor omleiding heeft en de locatie header de App Service hostnaam heeft in plaats van de oorspronkelijke hostnaam "www.contoso.com".

## <a name="solution-rewrite-the-location-header"></a>Oplossen De locatie header opnieuw schrijven

U moet de hostnaam in de locatie header instellen op de domein naam van de toepassings gateway. U doet dit door een [regel](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) voor herschrijven te maken met een voor waarde die evalueert of de locatie header in het antwoord azurewebsites.net bevat en een actie uitvoert om de locatie header te herschrijven naar de hostnaam van de toepassings gateway.  Zie de instructies voor [het herschrijven van de locatie header](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#modify-a-redirection-url).

> [!NOTE]
> De ondersteuning voor het opnieuw schrijven van HTTP-headers is alleen beschikbaar voor de [Standard_V2-en WAF_v2-SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) van de Application Gateway. Als u v1 SKU gebruikt, raden we u ten zeerste aan om te [migreren van v1 naar v2](https://docs.microsoft.com/azure/application-gateway/migrate-v1-v2) , zodat u herschrijf bewerkingen en andere [geavanceerde mogelijkheden](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#feature-comparison-between-v1-sku-and-v2-sku) kunt gebruiken die beschikbaar zijn met v2 SKU.

## <a name="alternate-solution-use-app-services-custom-domain-instead-of-default-domain-name"></a>Alternatieve oplossing: Het aangepaste domein van app service gebruiken in plaats van de standaard domein naam

Als u v1 SKU gebruikt, kunt u de locatie header niet herschrijven omdat deze mogelijkheid alleen beschikbaar is voor v2 SKU. Daarom moet u, om het omleidings probleem op te lossen, dezelfde hostnaam door geven die Application Gateway aan de App Service wordt ontvangen, in plaats van een host te overschrijven.

Als u dat doet, wordt de omleiding (indien van toepassing) op dezelfde oorspronkelijke host-header, die verwijst naar Application Gateway en niet zijn eigen, door App Service.

Hiervoor moet u eigenaar zijn van een aangepast domein en het onderstaande proces volgen.

- Registreer het domein in de lijst met aangepaste domeinen van de App Service. Hiervoor moet u een CNAME in uw aangepaste domein hebben dat verwijst naar de FQDN van App Service. Zie [een bestaande aangepaste DNS-naam toewijzen aan Azure app service](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain)voor meer informatie.

![appservice-2](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Als dat is gebeurd, is uw App Service klaar om de hostnaam ' www.contoso.com ' te accepteren. Wijzig nu de CNAME-vermelding in DNS zodat deze terug naar de FQDN van Application Gateway wijst. Bijvoorbeeld ' appgw.eastus.cloudapp.azure.com '.

- Zorg ervoor dat uw domein ' www.contoso.com ' wordt omgezet in de FQDN van Application Gateway wanneer u een DNS-query uitvoert.

- Stel de aangepaste test in om de optie hostname van back-end-HTTP-instellingen kiezen uit te scha kelen. U kunt dit doen via de portal door het selectie vakje in de test instellingen en Power shell uit te scha kelen door niet de schakel optie-PickHostNameFromBackendHttpSettings in de opdracht set-AzApplicationGatewayProbeConfig te gebruiken. Voer in het veld hostnaam van de test de FQDN-example.azurewebsites.net van uw App Service in als de test aanvragen die vanuit Application Gateway worden verzonden, worden uitgevoerd in de hostheader.

  > [!NOTE]
  > Zorg er tijdens de volgende stap voor dat uw aangepaste test niet is gekoppeld aan uw back-end-HTTP-instellingen, omdat uw HTTP-instellingen nog steeds de schakel optie hostname van back-end kiezen op dit punt hebben ingeschakeld.

- Stel de HTTP-instellingen van uw Application Gateway in om ' hostnaam kiezen van het back-upadres ' uit te scha kelen. U kunt dit doen via de portal door het selectie vakje en in Power shell uit te scha kelen door niet de schakel optie-PickHostNameFromBackendAddress in de opdracht set-AzApplicationGatewayBackendHttpSettings te gebruiken.

- Koppel de aangepaste test terug naar de back-end-HTTP-instellingen en controleer de status van de back-end als deze in orde is.

- Wanneer dit is gebeurd, moet Application Gateway nu dezelfde hostnaam ' www.contoso.com ' door sturen naar de App Service en de omleiding vindt plaats op dezelfde hostnaam. U kunt de onderstaande voorbeeld aanvraag en antwoord headers controleren.

Als u de hierboven vermelde stappen wilt implementeren met behulp van Power shell voor een bestaande installatie, volgt u het voor beeld Power shell-script hieronder. Houd er rekening mee dat de PickHostname-switches in de test-en HTTP-instellingen configuratie niet zijn gebruikt.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
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

Als de voor gaande stappen het probleem niet oplossen, opent u een ondersteunings [ticket](https://azure.microsoft.com/support/options/).
