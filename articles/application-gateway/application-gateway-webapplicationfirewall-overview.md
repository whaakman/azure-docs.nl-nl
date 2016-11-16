---
title: Inleiding tot Web Application Firewall (WAF) voor Application Gateway | Microsoft Docs
description: 'Deze pagina bevat een overzicht van Web Application Firewall (WAF) voor Application Gateway '
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 04b362bc-6653-4765-86f6-55ee8ec2a0ff
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/10/2016
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 69dd0b2d33c93edfa3073ce297d9a3ff948a037e


---
# <a name="application-gateway-web-application-firewall-preview"></a>Application Gateway Web Application Firewall (preview)
Web Application Firewall (WAF) is een functie van Azure Application Gateway die beveiliging biedt voor webtoepassingen die gebruikmaken van Application Gateway voor standaard ADC-functies (Application Delivery Control). Web Application Firewall doet dit door deze te beschermen tegen het grootste deel van de algemene internetbeveiligingsproblemen uit de OWASP top 10. Webtoepassingen zijn in toenemende mate het doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen. Veelvoorkomende aanvallen zijn hierbij onder andere aanvallen met SQL-injecties en aanvallen via scripting op meerdere sites. Het kan een hele uitdaging zijn om dergelijke aanvallen in toepassingscode te voorkomen en dit kan tevens veel onderhoud, patching en controle vereisen op meerdere lagen van de toepassingstopologie. Een gecentraliseerde Web Application Firewall ter bescherming tegen aanvallen via internet maakt het beveiligingsbeheer veel eenvoudiger en biedt de toepassing meer veiligheid tegen de bedreigingen van indringers. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/WAF1.png)

Application Gateway werkt als een controller voor de levering van toepassingen en biedt SSL-beëindiging, sessieaffiniteit op basis van cookies, round robin-taakverdeling, routering op basis van inhoud en de mogelijkheid om meerdere websites en beveiligingsvoorzieningen te hosten. Beveiligingsvoorzieningen die door Application Gateway worden geboden, zijn onder andere SSL-beleidsbeheer en ondersteuning voor end-to-end SSL. De mogelijkheden voor toepassingsbeveiliging van onze service worden nog vergroot door de introductie van WAF (Web Application Firewall) dat direct is geïntegreerd in het ADC-aanbod. Hiermee kunt u eenvoudig een centrale locatie configureren zodat u uw webtoepassingen kunt beheren en beveiligen tegen veelvoorkomende internetkwetsbaarheden.

Als u WAF configureert in Application Gateway, biedt u dat de volgende voordelen:

* U beveiligt de webtoepassing tegen internetkwetsbaarheden en aanvallen zonder dat u de code voor de back-ends hoeft aan te passen.
* U beveiligt gelijktijdig meerdere webtoepassingen achter de toepassingsgateway. Application Gateway ondersteunt het hosten van maximaal 20 websites achter één gateway, die alle kunnen worden beveiligd tegen aanvallen via internet.
* U controleert de webtoepassing tegen aanvallen met behulp van een realtime rapport dat wordt gegenereerd door de WAF-logboeken in Application Gateway.
* Voor bepaald nalevingsbeheer moeten alle eindpunten die zijn verbonden met internet worden beveiligd door een WAF-oplossing. Wanneer u Application Gateway met de WAF-functionaliteit gebruikt, kunt u aan deze nalevingsvereisten voldoen.

## <a name="overview"></a>Overzicht
Application Gateway WAF wordt aangeboden in een nieuwe voorraadeenheid (WAF-voorraadeenheid) en is vooraf geconfigureerd met ModSecurity en OWASP Core Rule Set waarmee een basisbeveiliging wordt geboden tegen de tien meest voorkomende internetkwetsbaarheden volgens OWASP.

* Beveiliging tegen SQL-injecties
* Beveiliging tegen scripting op meerdere sites
* Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten
* Beveiliging tegen schendingen van het HTTP-protocol
* Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken
* HTTP DoS-beveiligingen, zoals het voorkomen van HTTP-overspoelingen en DoS-aanvallen met trage HTTP-aanvragen
* Beveiliging tegen bots, crawlers en scanners
* Detectie van veelvoorkomende onjuiste configuraties van toepassingen (Apache, IIS, enzovoort)

## <a name="waf-modes"></a>WAF-modi
In Application Gateway WAF kunnen de volgende twee modi worden geconfigureerd:

* **Detectiemodus**: bij deze configuratie worden door Application Gateway WAF alle bedreigingswaarschuwingen gecontroleerd en in een logboekbestand vastgelegd. U moet hiervoor diagnoses voor logboekregistraties voor Application Gateway inschakelen in deze sectie Diagnostische gegevens. U moet er ook voor zorgen dat het WAF-logboek is geselecteerd en ingeschakeld.
* **Preventiemodus**: bij deze configuratie worden indringers en aanvallen die worden gedetecteerd door de van toepassing zijnde regels actief door Application Gateway geblokkeerd. De aanvaller krijgt een 403-foutmelding voor onbevoegde toegang en de verbinding wordt verbroken. De preventiemodus blijft dergelijke aanvallen registreren in de WAF-logboeken.

## <a name="application-gateway-waf-reports"></a>Application Gateway WAF-rapporten
Application Gateway WAF biedt gedetailleerde rapporten voor elke bedreiging die wordt gedetecteerd. De logboekregistratie is geïntegreerd met Azure Diagnostics-logboeken en -waarschuwingen die worden vastgelegd in de JSON-indeling.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/waf2.png)

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
    "operationName": "ApplicationGatewayFirewall",
    "time": "2016-09-20T00:40:04.9138513Z",
    "category": "ApplicationGatewayFirewallLog",
    "properties":     {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIp":"108.41.16.164",
        "clientPort":1815,
        "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
        "ruleId":"OWASP_973336",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "action":"Logged",
        "site":"Global",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
}
```

## <a name="application-gateway-waf-sku-pricing"></a>Prijzen voor de Application Gateway WAF-voorraadeenheid
Tijdens de preview zijn er geen extra kosten verbonden aan het gebruik van Application Gateway WAF. U betaalt gewoon nog steeds de kosten voor de bestaande basisvoorraadeenheid. De kosten voor de WAF-voorraadeenheid zullen worden meegedeeld wanneer deze algemeen beschikbaar is. Voor klanten die ervoor hebben gekozen om Application Gateway te implementeren in de WAF-voorraadeenheid wordt de prijs voor de WAF-voorraadeenheid pas in rekening gebracht na de aankondiging dat deze algemeen beschikbaar is.

## <a name="next-steps"></a>Volgende stappen
Nadat u meer te weten bent gekomen over de mogelijkheden van WAF, kunt u naar [How to configure Web Application Firewall on Application Gateway (Web Application Firewall configureren in Application Gateway)](application-gateway-web-application-firewall-portal.md) gaan.




<!--HONumber=Nov16_HO2-->


