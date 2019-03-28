---
title: Inleiding tot web application firewall voor Azure Application Gateway
description: In dit artikel bevat een overzicht van de web application firewall (WAF) voor Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: amsriva
ms.topic: conceptual
ms.openlocfilehash: 830513a03bd65ca14cb0938ae599a676f1bb3bca
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518181"
---
# <a name="web-application-firewall-for-azure-application-gateway"></a>Web application firewall voor Azure Application Gateway

Azure Application Gateway biedt een web application firewall (WAF) die gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. Webtoepassingen worden steeds vaker het doel van aanvallen die gebruikmaken van bekende beveiligingsproblemen. SQL-injectie en cross-site scripting zijn een van de meest voorkomende aanvallen.

Zo wordt voorkomen dat dergelijke aanvallen in toepassingscode uitdagingen met zich mee. Het kan tevens veel onderhoud, patching en controle op meerdere lagen van de Toepassingstopologie vereisen. Een gecentraliseerde web application firewall helpt maakt het beveiligingsbeheer veel eenvoudiger. Een WAF biedt beheerders ook beter zekerheid dat er beveiliging tegen bedreigingen en beveiligingsrisico's.

Een WAF-oplossing kan reageren op een snellere beveiligingsrisico centraal patches voor een bekend beveiligingsprobleem, in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar fire-prikbord ingeschakeld Toepassingsgateways.

De Application Gateway WAF is gebaseerd op [Core-regel instellen (CRS)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 of 2.2.9 van van de Open Web Application Security Project (OWASP). De WAF wordt automatisch bijgewerkt om op te nemen van bescherming tegen nieuwe beveiligingsproblemen, zonder aanvullende configuratie nodig.

![Application Gateway WAF diagram](./media/waf-overview/WAF1.png)

Application Gateway fungeert als application delivery controller (ADC). Deze optie biedt Secure Sockets Layer (SSL)-beëindiging, sessieaffiniteit op basis van een cookie, round robin-taakverdeling, routering op basis van inhoud van, mogelijkheid voor het hosten van meerdere websites en beveiligingsvoorzieningen.

Application Gateway security verbeteringen zijn onder andere SSL-beleidsbeheer en ondersteuning voor end-to-end SSL. Beveiliging van toepassingen is verder versterkt doordat WAF-integratie in Application Gateway. De combinatie beschermt uw webtoepassingen tegen veelvoorkomende beveiligingsproblemen. En biedt een eenvoudig te configureren, centrale locatie voor het beheren van.

## <a name="benefits"></a>Voordelen

Deze sectie beschrijft de belangrijkste voordelen van Application Gateway en de WAF bieden.

### <a name="protection"></a>Beveiliging

* Bescherm uw webtoepassingen tegen internetkwetsbaarheden en aanvallen zonder aanpassingen aan back-end-code.

* Beveiligen van meerdere webtoepassingen op hetzelfde moment. Een exemplaar van Application Gateway kan hosten van maximaal 20 websites die worden beveiligd door een firewall voor webtoepassingen.

### <a name="monitoring"></a>Bewaking

* Aanvallen tegen uw webtoepassingen bewaken met behulp van een real-time logboek van WAF. Het logboek is geïntegreerd met [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) WAF waarschuwingen volgen en eenvoudig controleren trends.

* De Application Gateway WAF is geïntegreerd met Azure Security Center. Security Center biedt een centraal overzicht van de beveiligingsstatus van al uw Azure-resources.

### <a name="customization"></a>Aanpassing

* U kunt aanpassen WAF-regels en regelgroepen om te stemmen op de toepassingsvereisten van uw en fout-positieven te elimineren.

## <a name="features"></a>Functies

- SQL-injection protection.
- Cross-site scripting beveiliging.
- Bescherming tegen andere veelvoorkomende aanvallen via Internet, zoals opdracht injectie, HTTP-aanvraag smokkelen, HTTP-antwoorden en van externe bestanden maakt.
- Beveiliging tegen schendingen van de HTTP-protocol.
- Beveiliging tegen afwijkingen van HTTP-protocol, zoals ontbrekende gebruikersagent host en accept-headers.
- Beveiliging tegen bots, crawlers en scanners.
- Detectie van veelvoorkomende onjuiste configuraties van toepassingen (bijvoorbeeld Apache en IIS).
- Configureerbare aanvraaggrootte beperkt met kleine en bovengrens.
- Uitsluitingslijsten kunnen u bepaalde kenmerken van een evaluatie WAF weglaten. Een veelvoorkomend voorbeeld is ingevoegd Active Directory-tokens die worden gebruikt voor verificatie of wachtwoordvelden.

### <a name="core-rule-sets"></a>Core Rule Sets

Application Gateway ondersteunt twee regelsets, CRS 3.0 en CRS 2.2.9. Deze regels worden uw webtoepassingen beveiligd tegen schadelijke activiteiten.

De Application Gateway WAF is vooraf geconfigureerd met CRS 3.0 standaard. Maar u kunt kiezen in plaats daarvan CRS 2.2.9 gebruiken. Voordeel van CRS 3.0 minder fout-positieven vergeleken met CRS 2.2.9. U kunt ook [regels aan uw eigen wensen aanpassen](application-gateway-customize-waf-rules-portal.md).

De WAF beschermt tegen de volgende beveiligingsproblemen op het web:

- SQL-injection attacks
- Aanvallen via cross-site scripting
- Andere veelvoorkomende aanvallen, zoals de opdracht-injectie, HTTP-aanvragen ' smokkelen ', HTTP-antwoord splitsen en externe insluiting
- Schendingen van de HTTP-protocol
- HTTP-protocol afwijkingen, zoals ontbrekende gebruikersagent host en accept-headers
- Bots, crawlers en scanners
- Veelvoorkomende onjuiste configuraties van toepassingen (bijvoorbeeld Apache en IIS)

#### <a name="owasp-crs-30"></a>OWASP CRS 3.0

CRS 3.0 bevat 13 regelgroepen, zoals wordt weergegeven in de volgende tabel. Elke groep bevat meerdere regels, die kunnen worden uitgeschakeld.

|Regelgroep|Description|
|---|---|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Lockdown-methoden (PUT, PATCH)|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**|Bescherming tegen poort- en omgevingsscanners|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Bescherming tegen protocol- en coderingsproblemen|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Bescherming tegen header-injectie, smokkelen-aanvragen en -antwoorden|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Bescherming tegen aanvallen van bestanden en paden|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Bescherming tegen aanvallen van remote file inclusion (RFI).|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Opnieuw beveiligen van aanvallen van externe code worden uitgevoerd|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Beschermen tegen aanvallen via PHP-injectie|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Bescherming tegen aanvallen via cross-site scripting|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Beschermen tegen aanvallen via SQL-injectie|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Bescherming tegen sessiefixatie aanvallen|

#### <a name="owasp-crs-229"></a>OWASP CRS 2.2.9

CRS 2.2.9 bevat 10 regelgroepen, zoals wordt weergegeven in de volgende tabel. Elke groep bevat meerdere regels, die kunnen worden uitgeschakeld.

|Regelgroep|Description|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Beveiligen tegen protocolschendingen (zoals ongeldige tekens of een GET met een aanvraagtekst)|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Bescherming tegen onjuiste header-informatie|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Beveiligen tegen argumenten of bestanden die limieten overschrijden|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Bescherming tegen beperkte methoden, headers en bestandstypen|
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Bescherming tegen webcrawlers en scanners|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Bescherming tegen algemene aanvallen (zoals sessiefixatie, remote insluiting en PHP-injectie)|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Beschermen tegen aanvallen via SQL-injectie|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Bescherming tegen aanvallen via cross-site scripting|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Bescherming tegen aanvallen van de pad-traversal|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Beveiligen tegen Trojaanse paarden|

### <a name="waf-modes"></a>WAF-modi

De Application Gateway WAF kan worden geconfigureerd om te worden uitgevoerd in de volgende twee modi:

* **Detectiemodus**: Bewaakt en alle waarschuwingen van threat-Logboeken. U Diagnostische logboekregistratie inschakelen voor Application Gateway in de **Diagnostics** sectie. U moet ook ervoor zorgen dat het WAF-logboek is geselecteerd en ingeschakeld. De Web application firewall blokkeert niet inkomende aanvragen wanneer deze wordt uitgevoerd in de modus voor detectie.
* **Preventiemodus**: Blokken indringers en aanvallen die de regels detecteren. De aanvaller krijgt een uitzondering '403 niet-geautoriseerde toegang' en de verbinding is verbroken. Preventiemodus registreert dergelijke aanvallen in de WAF-Logboeken.

### <a name="anomaly-scoring-mode"></a>Anomaliedetectie score modus
 
OWASP heeft twee modi voor het besluit om verkeer te blokkeren: Traditionele en Scoring-Anomaliedetectie-modus.

In de traditionele modus, wordt beschouwd als verkeer dat overeenkomt met elke regel onafhankelijk van eventuele andere overeenkomt met de regel. In deze modus is eenvoudig te begrijpen. Maar het ontbreken van informatie over het aantal regels overeenkomen met een specifieke aanvraag is een beperking. Dus is Anomaliedetectie Scoring-modus geïntroduceerd. Dit is de standaardinstelling voor OWASP 3. *x*.

Verkeer dat overeenkomt met elke regel is niet in de modus voor Anomaliedetectie scoren onmiddellijk geblokkeerd wanneer de firewall preventiemodus wordt. Regels voor hebben een bepaalde prioriteit: *Kritieke*, *fout*, *waarschuwing*, of *kennisgeving*. Deze ernst is van invloed op een numerieke waarde voor de aanvraag, de Anomaliedetectie-Score wordt genoemd. Als bijvoorbeeld een *waarschuwing* regel overeen bijdraagt 3 van de score. Een *kritieke* regel overeen bijdraagt 5.

Er is een drempelwaarde van 5 voor de Score van afwijkingen verkeer blokkeert. Dus een enkel *Kritiek* overeenkomst van de regel is voldoende voor de Application Gateway WAF een aanvraag, zelfs in preventiemodus blokkeren. Maar één *waarschuwing* regel overeen verhoogt alleen de anomalie Score door 3, die niet voldoende zelf om het verkeer te blokkeren.

> [!NOTE]
> Het bericht dat wordt geregistreerd wanneer een WAF-regel komt overeen met verkeer bevat de actiewaarde 'Geblokkeerd'. Maar het verkeer is eigenlijk alleen geblokkeerd voor een Score van afwijkingen van 5 of hoger.  

### <a name="waf-monitoring"></a>Bewaking met WAF

Het bewaken van de status van uw toepassingsgateway is belangrijk. De status van uw WAF en de toepassingen die ermee worden beveiligd in de gaten wordt ondersteund door integratie met Azure Security Center, Azure Monitor en Azure Monitor geregistreerd.

![Diagram van diagnostische gegevens van Application Gateway WAF](./media/waf-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Application Gateway-logboeken zijn geïntegreerd met [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md). Hiermee kunt u voor het bijhouden van diagnostische informatie, inclusief WAF waarschuwingen en Logboeken. U kunt toegang krijgen tot deze mogelijkheid op de **Diagnostics** tabblad in de Application Gateway-resource in de portal of rechtstreeks via Azure Monitor. Zie voor meer informatie over het inschakelen van Logboeken, [Application Gateway diagnostics](application-gateway-diagnostics.md).

#### <a name="azure-security-center"></a>Azure Security Center

[Security Center](../security-center/security-center-intro.md) helpt voorkomen, detecteren en direct reageren op bedreigingen. Het biedt verbeterde zichtbaarheid en controle over de beveiliging van uw Azure-resources. Application Gateway is [geïntegreerd met Security Center](application-gateway-integration-security-center.md). Security Center scant uw omgeving voor het detecteren van niet-beveiligde webtoepassingen. Deze kunt Application Gateway WAF om deze kwetsbare resources te beschermen aanbevelen. U maakt de firewalls rechtstreeks vanuit Security Center. Deze WAF-exemplaren zijn geïntegreerd met Security Center. Ze verzenden voor het melden van waarschuwingen en statusinformatie naar Security Center.

![Het overzichtsvenster voor Security Center](./media/waf-overview/figure1.png)

#### <a name="logging"></a>Logboekregistratie

Application Gateway WAF biedt gedetailleerde rapporten voor elke bedreiging die worden gedetecteerd. Logboekregistratie is geïntegreerd met Azure Diagnostics-Logboeken. Waarschuwingen worden vastgelegd in de JSON-indeling. Deze logboeken kunnen worden geïntegreerd met [logboeken van Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).

![Application Gateway diagnostische logboeken van windows](./media/waf-overview/waf2.png)

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

De Application Gateway WAF bevindt zich onder een nieuwe een SKU. Deze SKU is beschikbaar in het inrichtingsmodel van Azure Resource Manager alleen, niet in het klassieke implementatiemodel. Bovendien leverbaar de WAF-SKU alleen in middelgrote en grote toepassingsgateway-exemplaargrootten. De limieten voor Application Gateway zijn ook van toepassing op de WAF-SKU.

Prijzen zijn gebaseerd op een uurtarief voor gateway-exemplaar en een gegevensverwerking in rekening gebracht. [Prijzen van Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/) voor de WAF-SKU wijkt af van standaard SKU-kosten in rekening gebracht. Kosten voor gegevensverwerking zijn hetzelfde. Er zijn geen kosten per regel of regelgroep toevoegen. U kunt meerdere webtoepassingen achter dezelfde firewall voor de webtoepassingen beveiligen. Niet in rekening gebracht voor de ondersteuning van meerdere toepassingen.

## <a name="next-steps"></a>Volgende stappen

Zie [web application firewall configureren in Application Gateway](tutorial-restrict-web-traffic-powershell.md).
