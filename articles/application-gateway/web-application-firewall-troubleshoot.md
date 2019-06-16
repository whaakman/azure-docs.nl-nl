---
title: De Web Application Firewall voor Azure Application Gateway oplossen
description: Dit artikel bevat informatie over probleemoplossing voor Web Application Firewall (WAF) voor Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 6/11/2019
ms.author: ant
ms.topic: conceptual
ms.openlocfilehash: b81ffc5769fa5521094734da5d00aab77aa7cd35
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082441"
---
# <a name="troubleshoot-web-application-firewall-waf-for-azure-application-gateway"></a>Web Application Firewall (WAF) voor Azure Application Gateway oplossen

Er zijn enkele dingen die u doen kunt als aanvragen die moeten worden verwerkt via uw Web Application Firewall (WAF) worden geblokkeerd.

Eerst, zorg ervoor dat u hebt gelezen de [WAF overzicht](waf-overview.md) en de [configuratie van de WAF](application-gateway-waf-configuration.md) documenten. Controleer ook of u hebt ingeschakeld [bewaking met WAF](application-gateway-diagnostics.md) deze artikelen wordt uitgelegd hoe de WAF werkt, hoe de WAF-regel instellen voor werk, en hoe u toegang tot logboeken van WAF.

## <a name="understanding-waf-logs"></a>Understanding WAF-Logboeken

Het doel van de WAF-Logboeken is om elke aanvraag die is afgestemd of wordt geblokkeerd door de WAF weer te geven. Het is een grootboek van alle geëvalueerde aanvragen die zijn afgestemd of geblokkeerd. Als u merkt dat een aanvraag die deze (een fout-positief) mag niet worden geblokkeerd door de WAF, kunt u een paar dingen doen. Eerst, verfijnen en zoek de specifieke aanvraag. Bekijk de logboeken naar de specifieke URI, timestamp of transactie-ID van de aanvraag vinden. Wanneer u de bijbehorende logboekvermeldingen hebt gevonden, kunt u beginnen om te reageren op de fout-positieven.

Stel dat u hebt een legitieme verkeer met de tekenreeks die *1 = 1* die u wilt uw WAF passeren. Als u de aanvraag probeert, de WAF blokkeert verkeer dat bevat uw *1 = 1* tekenreeks in een veld of de parameter. Dit is een tekenreeks die vaak zijn gekoppeld aan een SQL-injectieaanvallen. U kunt zoeken in de logboeken en de tijdstempel van de aanvraag en de regels die geblokkeerd/overeenkomen met bekijkt.

In het volgende voorbeeld ziet u dat vier regels worden geactiveerd in dezelfde aanvraag (met behulp van het veld TransactionId). Het eerste item zegt overeenkomende omdat de gebruiker een numerieke/IP-adres gebruikt-URL voor de aanvraag, waardoor de anomaliedetectie-score met drie omdat het een waarschuwing. De volgende regel die overeenkomen met is 942130, dat is degene die u zoekt. U ziet de *1 = 1* in de `details.data` veld. Dit verhoogt bovendien de anomaliedetectie-score met drie opnieuw, omdat het ook een waarschuwing. In het algemeen, elke regel waarvoor de actie **overeenkomend** verhoogt de anomalie beoordelen en op dit moment de anomaliedetectie-score zes zou zijn. Zie voor meer informatie, [Anomaliedetectie scoring modus](waf-overview.md#anomaly-scoring-mode).

De laatste twee logboekvermeldingen weergeven in dat de aanvraag is geblokkeerd omdat de anomaliedetectie-score hoog genoeg is. Deze vermeldingen hebben een andere actie dan de andere twee. Ze geven ze daadwerkelijk *geblokkeerd* de aanvraag. Deze regels zijn verplicht en kunnen niet worden uitgeschakeld. Ze al dan niet mogen worden beschouwd als regels, maar meer als basisinfrastructuur van de WAF-inhoud.

```json
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "920350", 
        "message": "Host header is a numeric IP address", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ", 
            "data": "40.90.218.160", 
            "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"", 
            "line": "791" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "942130", 
        "message": "SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ", 
            "data": "Matched Data: 1=1 found within ARGS:text1: 1=1", 
            "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"", 
            "line": "554" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ", 
            "data": "", 
            "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"", 
            "line": "57" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ", 
            "data": "", 
            "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"", 
            "line": "73" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    }
}
```

## <a name="fixing-false-positives"></a>Fout-positieven oplossen

Met deze informatie en de kennis die de regel 942130 is degene die overeenkomen met de *1 = 1* tekenreeks, kunt u een paar dingen om te voorkomen dat dit uw verkeer blokkeert doen:

- Een lijst met uitsluitingen gebruiken

   Zie [configuratie van de WAF](application-gateway-waf-configuration.md#waf-exclusion-lists) voor meer informatie over uitsluiting geeft een lijst.
- De regel uitschakelen.

### <a name="using-an-exclusion-list"></a>Met behulp van een uitsluitingslijst

Als u een gefundeerde beslissing nemen over het verwerken van een onwaar positief, is het belangrijk om vertrouwd te raken met de technologieën die uw toepassing gebruikt. Stel dat er een SQL-server niet in uw-technologiestack en krijgt u fout-positieven met betrekking tot deze regels. Uitschakelen van deze regels verzwakken niet per se voor de beveiliging van uw.

Een voordeel van het gebruik van een uitsluitingslijst is dat alleen een specifiek deel van een aanvraag wordt uitgeschakeld. Dit betekent echter dat een specifieke uitsluiting van toepassing op alle verkeer dat via uw WAF is, omdat deze een algemene instelling. Bijvoorbeeld, dit kan leiden tot een probleem als *1 = 1* is een geldige aanvraag in de hoofdtekst voor een bepaalde app, maar niet voor andere gebruikers. Een ander voordeel is dat u tussen de hoofdtekst, kopteksten en cookies kiezen kunt moeten worden uitgesloten als een bepaalde voorwaarde wordt voldaan, in plaats van met uitzondering van de hele aanvraag.

Er zijn soms gevallen waarbij specifieke parameters in de WAF op een manier die mogelijk niet intuïtief ophalen doorgegeven. Er is bijvoorbeeld een token dat wordt doorgegeven bij het verifiëren met Azure Active Directory. Dit token *__RequestVerificationToken*, meestal ophalen doorgegeven als een Cookie aanvragen. Echter, in sommige gevallen waarbij cookies zijn uitgeschakeld, dit token wordt ook doorgegeven als een aanvraagkenmerk of 'func'. Als dit gebeurt, moet u ervoor te zorgen dat *__RequestVerificationToken* wordt toegevoegd aan de uitsluitingslijst als een **aanvraag kenmerknaam** ook.

![Uitzonderingen](media/waf-tshoot/exclusion-list.png)

In dit voorbeeld dat u wilt uitsluiten de **aanvraag kenmerknaam** die gelijk is aan *Tekst1*. Dit is duidelijk omdat u de naam van het kenmerk in de firewall-Logboeken kunt zien: **gegevens: Overeenkomende gegevens: 1 = 1 binnen ARGS:text1 gevonden: 1=1**. Het kenmerk is **Tekst1**. U vindt hier ook de naam van dit kenmerk enkele andere manieren waarop, [aanvraag zoeken kenmerknamen](#finding-request-attribute-names).

![WAF-uitsluitingslijsten](media/waf-tshoot/waf-config.png)

### <a name="disabling-rules"></a>Uitschakelen van regels

Een andere manier om op te halen over een onwaar positief is om uit te schakelen van de regel die overeenkomen met de invoer van de WAF-thought schadelijke is. Nadat u de WAF-Logboeken hebt geparseerd en de regel omlaag 942130 smaller, kunt u deze uitschakelen in Azure portal. Zie [regels voor web application firewall via de Azure-portal aanpassen](application-gateway-customize-waf-rules-portal.md).

Een voordeel van het uitschakelen van een regel is als u al het verkeer dat een bepaalde voorwaarde die normaal gesproken worden geblokkeerd bevat geldige verkeer is weet, u die regel voor de hele WAF uitschakelen kunt. Echter, als dit alleen geldig verkeer in een specifieke use case is, u opent u een beveiligingslek in de door het uitschakelen van die regel voor de hele WAF omdat deze een algemene instelling.

Als u gebruiken van Azure PowerShell wilt, raadpleegt u [regels voor web application firewall via PowerShell aanpassen](application-gateway-customize-waf-rules-powershell.md). Als u wilt het gebruik van Azure CLI, Zie [regels via de Azure CLI voor web application firewall aanpassen](application-gateway-customize-waf-rules-cli.md).

![WAF-regels](media/waf-tshoot/waf-rules.png)

## <a name="finding-request-attribute-names"></a>Zoeken naar namen van aanvraag

Met behulp van [Fiddler](https://www.telerik.com/fiddler), u afzonderlijke aanvragen te inspecteren en bepalen welke specifieke velden van een webpagina's die worden genoemd. Dit kan helpen om uit te sluiten van bepaalde velden van een controle met behulp van uitsluiting geeft een lijst.

In dit voorbeeld kunt u zien dat het veld waarin de *1 = 1* tekenreeks is ingevoerd heet **Tekst1**.

![Fiddler](media/waf-tshoot/fiddler-1.png)

Dit is een veld dat u kunt uitsluiten. Zie voor meer informatie over uitsluitingslijsten [Web application firewall-aanvraaglimieten grootte en uitsluitingslijsten](application-gateway-waf-configuration.md#waf-exclusion-lists). U kunt de evaluatie in dit geval uitsluiten door het configureren van de volgende uitzondering:

![Uitsluiting van WAF](media/waf-tshoot/waf-exclusion-02.png)

U kunt ook de firewall-Logboeken om de gegevens om te zien wat u nodig hebt om toe te voegen aan de uitsluitingslijst onderzoeken. Zie voor het inschakelen van logboekregistratie [Back-endstatus, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).

Raadpleeg het logboek met firewall en het bestand PT1H.json weergeven voor het uur dat de aanvraag die u wilt controleren heeft plaatsgevonden.

In dit voorbeeld ziet u dat u hebt vier regels met de dezelfde TransactionID en dat ze allemaal op hetzelfde moment exacte is opgetreden:

```json
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "920350",
-           "message": "Host header is a numeric IP address",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ",
-               "data": "40.90.218.160",
-               "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"",
-               "line": "791"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "942130",
-           "message": "SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ",
-               "data": "Matched Data: 1=1 found within ARGS:text1: 1=1",
-               "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"",
-               "line": "554"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ",
-               "data": "",
-               "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"",
-               "line": "57"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ",
-               "data": "",
-               "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"",
-               "line": "73"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
```

Met uw kennis van hoe u de regel CRS werk instellen en dat het ruleset CRS 3.0 werkt met een score systeem afwijking (Zie [Web application firewall voor Azure Application Gateway](waf-overview.md)) u weet dat onder twee regels met de  **actie: Geblokkeerd** eigenschap blokkeren op basis van de totale anomaliedetectie-score. De regels om zich te richten op zijn de twee bovenste.

Het eerste item wordt geregistreerd omdat de gebruiker een numerieke IP-adres gebruikt om te navigeren naar de Application Gateway, die in dit geval kan worden genegeerd.

De tweede een (regel 942130) is het interessant. U kunt zien in de details van dat deze overeenkomen met een patroon (1 = 1) en het veld met de naam **Tekst1**. Volg dezelfde voorgaande stappen om uit te sluiten de **kenmerk Aanvraagnaam** die **gelijk is aan** **1 = 1**.

## <a name="finding-request-header-names"></a>De naam van de header aanvraag zoeken

Fiddler is een handig hulpmiddel opnieuw aanvraagheader om namen te vinden. In de volgende schermafbeelding ziet u de headers voor deze GET-aanvraag, waaronder *Content-Type*, *gebruikersagent*, enzovoort.

![Fiddler](media/waf-tshoot/fiddler-2.png)

Een andere manier om weer te geven van de aanvraag- en reactieheaders is om te zoeken binnen de hulpprogramma's voor ontwikkelaars van Chrome. Kunt u op F12 te drukken of klik met de rechtermuisknop -> **inspecteren** -> **hulpprogramma's voor ontwikkelaars**, en selecteer de **netwerk** tabblad. Laden van webpagina's, en klik op de aanvraag die u wilt controleren.

![Chrome F12](media/waf-overview/chrome-f12.png)

## <a name="finding-request-cookie-names"></a>Namen van cookie aanvraag zoeken

Als de aanvraag cookies bevat, de **Cookies** tabblad kan worden geselecteerd om ze te bekijken in Fiddler.

## <a name="restrict-global-parameters-to-eliminate-false-positives"></a>Algemene parameters voor het Elimineer fout-positieven te beperken

- Aanvraag hoofdtekst controle uitschakelen

   Door in te stellen **inspecteren aanvraagtekst** uitgeschakeld, wordt de aanvraag-instanties van al het verkeer niet worden geëvalueerd door uw WAF. Dit kan zijn handig als u weet dat de aanvraag-instanties zijn niet schadelijke aan uw toepassing.

   Deze optie uitschakelt, wordt de hoofdtekst van de aanvraag niet gecontroleerd. De kop- en cookies blijven domeinen, tenzij afzonderlijke bestanden met behulp van de functionaliteit van de lijst met uitsluitingen worden uitgesloten.

- Maximale bestandsgrootte

   Door het beperken van de grootte van het voor uw WAF, beperkt u de mogelijkheid van een aanval plaatsvindt naar uw webservers. Door grote bestanden worden geüpload, verhoogt het risico van uw back-end die wordt overspoeld. Beperken van de bestandsgrootte naar een normale use-case voor uw toepassing is gewoon een manier om aanvallen te voorkomen.

   > [!NOTE]
   > Als u weet dat uw app nooit een bestand is geüpload moet boven een bepaalde grootte, kunt u die beperken door in te stellen een limiet.

## <a name="next-steps"></a>Volgende stappen

Zie [web application firewall configureren in Application Gateway](tutorial-restrict-web-traffic-powershell.md).
