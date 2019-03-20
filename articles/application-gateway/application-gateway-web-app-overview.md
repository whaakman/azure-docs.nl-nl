---
title: Overzicht van multitenant-back-ends, zoals Azure App service, met Azure Application Gateway
description: Op deze pagina wordt de ondersteuning voor back-ends met meerdere tenants in Application Gateway beschreven.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/18/2019
ms.author: victorh
ms.openlocfilehash: 8434340bb7ed95cc36115c05048b2b67682b5796
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224495"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends-such-as-app-service"></a>Application Gateway-ondersteuning voor meerdere tenants back-ends zoals appservice

In multitenant-architectuur ontwerpen in webservers, worden meerdere websites uitgevoerd op hetzelfde exemplaar van de webserver. Hostnamen worden gebruikt om onderscheid maken tussen de verschillende toepassingen die worden gehost. Application Gateway wijzigt standaard niet de binnenkomende HTTP-host-header van de client en stuurt de header ongewijzigd terug naar de back-end. Dit werkt goed voor back-endpoolleden zoals NIC's, virtuele-machineschaalsets, openbare IP-adressen, interne IP-adressen, FQDN-naam en als deze niet zijn gebaseerd op een specifieke host-header of SNI-extensie op te lossen naar het juiste eindpunt. Er zijn echter veel services zoals Azure App service WebApps en Azure API management die nature werken met meerdere tenants en vertrouwen op een specifieke host-header of SNI-extensie om te worden omgezet naar het juiste eindpunt. Normaal gesproken verschilt de DNS-naam van de toepassing die op zijn beurt de DNS-naam die is gekoppeld aan de toepassingsgateway is, van de domeinnaam van de back-endservice. De host-header in de oorspronkelijke aanvraag is ontvangen door de application gateway is daarom niet hetzelfde als de hostnaam van de back-endservice. Als gevolg hiervan, tenzij de host-header in de aanvraag van de application gateway naar de back-end wordt gewijzigd naar de hostnaam van de back endservice, de back-ends voor meerdere tenants kan niet worden voor het omzetten van de aanvraag naar het juiste eindpunt. 

Application gateway biedt een functie waarmee gebruikers voor de onderdrukking van de HTTP host-header in de aanvraag op basis van de hostnaam van de back-end. Deze mogelijkheid biedt ondersteuning voor meerdere tenants back-ends, zoals Azure App service WebApps en API management. Deze mogelijkheid is beschikbaar voor zowel de v1 en v2 standard- en WAF-SKU's. 

![Host-overschrijven](./media/application-gateway-web-app-overview/host-override.png)

> [!NOTE]
> Dit is niet van toepassing op Azure App service-omgeving (ASE) omdat het as-omgeving is een toegewezen resource in tegenstelling tot Azure App service is een multitenant-resource.

## <a name="override-host-header-in-the-request"></a>Host-header in de aanvraag overschrijven

De mogelijkheid om op te geven van een host onderdrukking is gedefinieerd in de [HTTP-instellingen](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) en kan worden toegepast op alle back-end-adrespool tijdens het maken van de regel. De volgende twee manieren om host-headers en SNI-extensie voor back-ends voor meerdere tenants te overschrijven wordt ondersteund:

- De mogelijkheid om in te stellen van de hostnaam op een vaste waarde expliciet is ingevoerd in de HTTP-instellingen. Deze functionaliteit zorgt ervoor dat de host-header wordt overschreven met deze waarde voor al het verkeer naar de back-end-pool waar de bepaalde HTTP-instellingen worden toegepast. Als u end-to-end SSL gebruikt, wordt de overschreven hostnaam gebruikt in de SNI-extensie. Op deze manier kunt scenario's waarin een back-end-pool-farm wordt verwacht dat een host-header die verschilt van de binnenkomende host-header van de klant.

- De mogelijkheid om de hostnaam afgeleid van de IP- of FQDN-naam van de leden van de back-end-pool. HTTP-instellingen bieden ook de mogelijkheid om op te halen dynamisch de hostnaam van een lid van de back-end-pool FQDN als geconfigureerd met de optie voor de hostnaam afgeleid van een afzonderlijke back-end-adrespool lid. Als u end-to-end SSL gebruikt, wordt de hostnaam afgeleid van de FQDN en gebruikt in de SNI-extensie. Op deze manier kunt scenario's waarin een back-end-pool kan twee of meer multitenant PaaS-services zoals Azure web-apps hebben en host-header op elk lid van de aanvraag bevat de hostnaam die is afgeleid van de FQDN. Voor de implementatie van dit scenario gebruiken we een switch in de HTTP-instellingen met de naam [kiezen hostnaam van de back-endadres](https://docs.microsoft.com/azure/application-gateway/configuration-overview#pick-host-name-from-backend-address) die dynamisch de host-header in de oorspronkelijke aanvraag naar de vermeld in de back-endpool wordt overschreven.  Bijvoorbeeld, als uw back-endgroep FQDN-naam 'contoso11.azurewebsites.net' en 'contoso22.azurewebsites.net' bevat, van de oorspronkelijke aanvraag host-header die contoso.com is overschreven contoso11.azurewebsites.net of contoso22.azurewebsites.net Wanneer de aanvraag wordt verzonden naar de juiste back-endserver. 

  ![web-app-scenario](./media/application-gateway-web-app-overview/scenario.png)

Met deze functie kunnen klanten opties opgeven in de HTTP-instellingen en kunnen ze aangepaste tests uitvoeren voor de gewenste configuratie. Deze instelling wordt vervolgens gekoppeld aan een listener en een back-end-adrespool met behulp van een regel.

## <a name="special-considerations"></a>Speciale overwegingen

### <a name="ssl-termination-and-end-to-end-ssl-with-multi-tenant-services"></a>SSL-beëindiging en end-to-end SSL met multitenant-services

Zowel SSL-beëindiging en end-to-end SSL-versleuteling wordt ondersteund met multitenant-services. Voor SSL-beëindiging aan de toepassingsgateway blijft het SSL-certificaat vereist om te worden toegevoegd aan de application gateway-listener. Echter, in het geval van end-to-end SSL, vertrouwde Azure-services zoals Azure App service WebApps hoeven niet in de whitelist aan de back-ends via de application gateway. Er is daarom niet nodig om toe te voegen van eventuele certificaten voor serververificatie. 

![end-to-end SSL](./media/application-gateway-web-app-overview/end-to-end-ssl.png)

U ziet dat in de bovenstaande afbeelding, er is geen vereiste certificaten voor serververificatie toevoegen wanneer appservice is geselecteerd als back-end.

### <a name="health-probe"></a>Test

Het overschrijven van de host-header in de **HTTP-instellingen** alleen van invloed op de aanvraag en de routering. Dit heeft geen invloed op het gedrag van de statustest. Als u wilt dat de end-to-end configuratie werkt, moeten zowel de test als de HTTP-instellingen worden bewerkt op basis van de juiste configuratie. Naast de biedt de mogelijkheid om op te geven van een host-header in de Testconfiguratie, bieden de mogelijkheid worden afgeleid van de host-header van de momenteel geconfigureerde HTTP-instellingen ook ondersteuning voor aangepaste tests. Deze configuratie kan worden opgegeven met behulp van de parameter `PickHostNameFromBackendHttpSettings` in de testconfiguratie.

### <a name="redirection-to-app-services-url-scenario"></a>Omleiding naar de App Service-URL-scenario

Kunnen er scenario's waar de hostnaam in het antwoord van de appservice de browser door eindgebruikers kan direct de *. azurewebsites.net hostnaam in plaats van het domein dat is gekoppeld aan de toepassingsgateway. Dit probleem kan optreden wanneer:

- Hebt u omleiding geconfigureerd op uw App Service. Omleiding is net zo eenvoudig als een afsluitende schuine streep toe te voegen aan de aanvraag.
- Hebt u Azure AD-verificatie die ervoor zorgt de omleiding dat.

Raadpleeg voor het oplossen van dergelijke gevallen [omleiding naar de appservice-URL probleem oplossen](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het instellen van een toepassingsgateway met een multitenant-app, zoals Azure App service-web-app als lid van de back-end-pool, recentst [configureren App Service WebApps met Application Gateway](https://docs.microsoft.com/azure/application-gateway/create-web-app)
