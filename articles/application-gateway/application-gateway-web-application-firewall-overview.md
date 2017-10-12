---
title: Inleiding tot Web Application Firewall (WAF) voor Azure Application Gateway | Microsoft Docs
description: Deze pagina bevat een overzicht van Web Application Firewall (WAF) voor Application Gateway
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
ms.date: 05/03/2017
ms.author: amsriva
ms.openlocfilehash: 50863b60f2843f033bdb07f4564d937cd6f0b7be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="web-application-firewall-waf"></a>Web Application Firewall (WAF)

Web Application Firewall (WAF) is een functie van Application Gateway die gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. 

Web Application Firewall is gebaseerd op regels uit de [Core Rule Set 3.0 of 2.2.9 van OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project). Webtoepassingen zijn in toenemende mate het doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen. Veelvoorkomende aanvallen zijn hierbij onder andere aanvallen met SQL-injecties en aanvallen via scripting op meerdere sites. Het kan een hele uitdaging zijn om dergelijke aanvallen in toepassingscode te voorkomen en dit kan tevens veel onderhoud, patching en controle vereisen op meerdere lagen van de toepassingstopologie. Een gecentraliseerde firewall voor webtoepassingen maakt het beveiligingsbeheer veel eenvoudiger en biedt toepassingsbeheerders meer veiligheid tegen bedreigingen of aanvallen. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

![imageURLroute](./media/application-gateway-web-application-firewall-overview/WAF1.png)

Application Gateway werkt als een controller voor de levering van toepassingen en biedt SSL-beëindiging, sessieaffiniteit op basis van cookies, round robin-taakverdeling, routering op basis van inhoud en de mogelijkheid om meerdere websites en beveiligingsvoorzieningen te hosten. Beveiligingsvoorzieningen die door Application Gateway worden geboden, zijn onder andere SSL-beleidsbeheer en ondersteuning voor end-to-end SSL. De beveiliging van toepassingen wordt nu verder versterkt doordat WAF (Web Application Firewall) rechtstreeks is geïntegreerd in de aangeboden ADC's. Hiermee kunt u eenvoudig een centrale locatie configureren zodat u uw webtoepassingen kunt beheren en beveiligen tegen veelvoorkomende internetkwetsbaarheden.

## <a name="benefits"></a>Voordelen

Dit zijn de belangrijkste voordelen van Application Gateway en Web Application Firewall:

### <a name="protection"></a>Beveiliging

* U beveiligt de webtoepassing tegen internetkwetsbaarheden en aanvallen zonder dat u de code voor de back-ends hoeft aan te passen.

* U beveiligt gelijktijdig meerdere webtoepassingen achter de toepassingsgateway. Application Gateway ondersteunt het hosten van maximaal 20 websites achter één gateway, die met WAF allemaal kunnen worden beveiligd tegen aanvallen via internet.

### <a name="monitoring"></a>Bewaking

* U controleert op aanvallen tegen de webtoepassing door een real-time logboek van WAF te raadplegen. Dit logboek is geïntegreerd met [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) om waarschuwingen en logboeken van WAF bij te houden en gemakkelijk trends te ontdekken.

* WAF zal binnenkort worden geïntegreerd met Azure Security Center. Azure Security Center biedt een centraal overzicht van de beveiligingsstatus van al uw Azure-resources.

### <a name="customization"></a>Aanpassing

* U kunt regels en regelgroepen van WAF aanpassen om deze te laten voldoen aan de toepassingsvereisten en om fout-positieven te elimineren.

## <a name="features"></a>Functies

Web Application Firewall is standaard geconfigureerd met CRS 3.0, maar u kunt ook versie 2.2.9 gebruiken. Het voordeel van CRS 3.0 is dat er minder fout-positieven worden geregistreerd dan met 2.2.9. U hebt de mogelijkheid om [regels aan uw behoeften aan te passen](application-gateway-customize-waf-rules-portal.md). Hieronder ziet u enkele voorbeelden van veelvoorkomende beveiligingsproblemen waartegen Web Application Firewall bescherming biedt:

* Beveiliging tegen SQL-injecties
* Beveiliging tegen scripting op meerdere sites
* Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten
* Beveiliging tegen schendingen van het HTTP-protocol
* Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken
* Beveiliging tegen bots, crawlers en scanners
* Detectie van veelvoorkomende onjuiste configuraties van toepassingen (Apache, IIS, enzovoort)

Raadpleeg de volgende [Core Rule Sets](#core-rule-sets) voor een gedetailleerde lijst van regels en waartegen ze beveiliging bieden.

### <a name="core-rule-sets"></a>Core Rule Sets

Application Gateway ondersteunt twee regelsets, CRS 3.0 en CRS 2.2.9. Deze Core Rule Sets zijn verzamelingen regels die uw webtoepassingen beschermen tegen schadelijke activiteiten.

#### <a name="owasp30"></a>OWASP_3.0

De Core Rule Set 3.0 bestaat uit de 13 regelgroepen uit de onderstaande tabel. Elk van deze regelgroepen bevat meerdere regels, die desgewenst kunnen worden uitgeschakeld.

|Regelgroep|Beschrijving|
|---|---|
|**[REQUEST-910-IP-REPUTATION](application-gateway-crs-rulegroups-rules.md#crs910)**|Bevat regels om te beveiligen tegen bekende spammers of schadelijke activiteiten.|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Bevat regels om te beveiligen tegen lockdown-methoden (PUT, PATCH< ..)|
|**[REQUEST-912-DOS-PROTECTION](application-gateway-crs-rulegroups-rules.md#crs912)**| Bevat regels om te beveiligen tegen DoS-aanvallen (Denial of Service).|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**| Bevat regels om te beveiligen tegen poort- en omgevingsscanners.|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Bevat regels om te beveiligen tegen protocol- en coderingsproblemen.|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Bevat regels om te beveiligen tegen header-injectie, 'request smuggling' en 'response splitting'.|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Bevat regels om te beveiligen tegen aanvallen van bestanden en paden.|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Bevat regels om te beveiligen tegen Remote File Inclusion (RFI).|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Bevat regels om te beveiligen tegen Remote Code Execution.|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Bevat regels om te beveiligen tegen aanvallen via PHP-injectie.|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Bevat regels om te beveiligen tegen het uitvoeren van scripts op meerdere sites.|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Bevat regels om te beveiligen tegen aanvallen via SQL-injectie.|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Bevat regels om te beveiligen tegen aanvallen via sessiefixatie.|

#### <a name="owasp229"></a>OWASP_2.2.9

De Core Rule Set 2.2.9 bestaat uit de 10 regelgroepen uit de onderstaande tabel. Elk van deze regelgroepen bevat meerdere regels, die desgewenst kunnen worden uitgeschakeld.

|Regelgroep|Beschrijving|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Bevat regels om te beveiligen tegen protocolschendingen (ongeldige tekens, GET met een aanvraagtekst, enzovoort).|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Bevat regels om te beveiligen tegen onjuiste header-informatie.|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Bevat regels om te beveiligen tegen argumenten of bestanden die limieten overschrijden.|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Bevat regels om te beveiligen tegen beperkte methoden, headers en bestandstypen. |
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Bevat regels om te beveiligen tegen webcrawlers en scanners.|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Bevat regels om te beveiligen tegen algemene-aanvallen (sessiefixatie, Remote File Inclusion, PHP injectie, enzovoort).|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Bevat regels om te beveiligen tegen aanvallen via SQL-injectie.|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Bevat regels om te beveiligen tegen het uitvoeren van scripts op meerdere sites.|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Bevat een regel om te beveiligen tegen aanvallen via pad-traversal.|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Bevat regels om te beveiligen tegen Trojaanse paarden.|

### <a name="waf-modes"></a>WAF-modi

In Application Gateway WAF kunnen de volgende twee modi worden geconfigureerd:

* **Detectiemodus**: bij deze configuratie worden door Application Gateway WAF alle bedreigingswaarschuwingen gecontroleerd en in een logboekbestand vastgelegd. Diagnostische logboekregistratie voor Application Gateway kunt u inschakelen via de sectie **Diagnostische gegevens**. U moet er ook voor zorgen dat het WAF-logboek is geselecteerd en ingeschakeld. In de detectiemodus worden binnenkomende verzoeken niet geblokkeerd door Web Application Firewall.
* **Preventiemodus**: bij deze configuratie worden indringers en aanvallen die worden gedetecteerd door de van toepassing zijnde regels actief door Application Gateway geblokkeerd. De aanvaller krijgt een 403-foutmelding voor onbevoegde toegang en de verbinding wordt verbroken. De preventiemodus blijft dergelijke aanvallen registreren in de WAF-logboeken.

### <a name="application-gateway-waf-reports"></a>Bewaking met WAF

Het bewaken van de status van uw toepassingsgateway is belangrijk. De status van Web Application Firewall zelf, en van de toepassingen die ermee worden beveiligd, kan worden gecontroleerd via logboekregistratie en integratie met Azure Monitor, Azure Security Center (binnenkort) en Log Analytics.

![diagnostische gegevens](./media/application-gateway-web-application-firewall-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Elke toepassingsgateway is geïntegreerd met [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).  Dit maak het mogelijk om diagnostische gegevens bij te houden, met inbegrip van meldingen en logboeken van WAF.  Deze functionaliteit wordt aangeboden binnen de resource Application Gateway, op het tabblad **Diagnostische gegevens** van de portal, of rechtstreeks via de service Azure Monitor. Zie [Application Gateway diagnostics](application-gateway-diagnostics.md) (Diagnostische gegevens Application Gateway) voor meer informatie over het inschakelen van diagnostische logboeken voor toepassingsgateways.

#### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](../security-center/security-center-intro.md) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Toepassingsgateway nu [geïntegreerd in Azure Security Center](application-gateway-integration-security-center.md). Azure Security Center scant uw omgeving op niet-beveiligde webtoepassingen. Application Gateway WAF kan worden aanbevolen om deze kwetsbare resources te beschermen. U kunt Application Gateway WAF rechtstreeks maken vanuit Azure Security Center.  Deze WAF-exemplaren zijn geïntegreerd met Azure Security Center en sturen waarschuwingen en statusinformatie terug naar Azure Security Center voor rapportage.

![afbeelding 1](./media/application-gateway-web-application-firewall-overview/figure1.png)

#### <a name="logging"></a>Logboekregistratie

Application Gateway WAF biedt gedetailleerde rapporten voor elke bedreiging die wordt gedetecteerd. Logboekregistratie is geïntegreerd met de logboeken van Azure Diagnostics en waarschuwingen worden vastgelegd in de JSON-indeling. Deze logboeken kunnen worden geïntegreerd met [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).

![imageURLroute](./media/application-gateway-web-application-firewall-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Prijzen voor de Application Gateway WAF-voorraadeenheid

Web Application Firewall is beschikbaar onder een nieuwe WAF-SKU. Deze SKU is alleen beschikbaar in het inrichtingsmodel van Azure Resource Manager en niet in het klassieke implementatiemodel. Bovendien is de WAF-SKU alleen leverbaar in de middelgrote en grote varianten van de toepassingsgateway. Alle limieten voor de toepassingsgateway zijn ook van toepassing op de WAF-SKU. De prijzen variëren naargelang de kosten per gateway-uur en de kosten voor gegevensverwerking. De prijzen voor gateway-uren voor de WAF-SKU verschillen van de kosten voor Basic-SKU's. Zie [Prijzen van Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/) voor meer informatie. De kosten voor gegevensverwerking zijn wel hetzelfde. Er worden geen kosten per regel of regelgroep in rekening gebracht. U kunt meerdere webtoepassingen achter dezelfde firewall voor webtoepassingen beveiligen en er zijn geen extra kosten verbonden aan de ondersteuning van meerdere toepassingen. 

De facturering voor WAF start vanaf 5 mei 2017. Tot die tijd worden de gateways van de WAF-SKU volgens de standaardtarieven gefactureerd.

## <a name="next-steps"></a>Volgende stappen

U weet nu in grote lijnen wat de mogelijkheden van WAF zijn. Ga naar [How to configure Web Application Firewall on Application Gateway (Web Application Firewall configureren in Application Gateway)](application-gateway-web-application-firewall-portal.md) voor informatie over de configuratie van WAF.

