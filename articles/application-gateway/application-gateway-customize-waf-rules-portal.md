---
title: Regels voor web application firewall in Azure Application Gateway - Azure-portal aanpassen
description: In dit artikel bevat informatie over het aanpassen van web application firewall-regels in Application Gateway met Azure portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: victorh
ms.openlocfilehash: b18c9666e58925746a3b61740db6fb5118c2010b
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56733713"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Regels voor web application firewall via de Azure-portal aanpassen

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Deze beveiligingen worden geleverd door de Open Web Application Security Project (OWASP) Core regel ingesteld (CRS). Sommige regels kunnen fout-positieven en echte verkeer blokkeert. Om deze reden biedt Application Gateway de mogelijkheid om aan te passen regelgroepen en -regels. Zie voor meer informatie over de specifieke regelgroepen en regels [lijst met web application firewall CRS-regelgroepen en -regels](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Als uw application gateway niet voor de WAF-laag gebruikt wordt, wordt de optie voor het upgraden van de application gateway naar de WAF-laag wordt weergegeven in het rechter deelvenster. 

![Enable WAF][fig1]

## <a name="view-rule-groups-and-rules"></a>Weergave-regelgroepen en -regels

**Regelgroepen en regels weergeven**
   1. Blader naar de application gateway, en selecteer vervolgens **Web application firewall**.  
   2. Selecteer **geavanceerde regelconfiguratie**.  
   In deze weergave bevat een tabel op de pagina van alle regelgroepen voorzien van de gekozen regelset. Alle selectievakjes in van de regel zijn geselecteerd.

![Uitgeschakelde regels configureren][1]

## <a name="search-for-rules-to-disable"></a>Zoeken naar regels om uit te schakelen

De **Web application firewall-instellingen** blade biedt de mogelijkheid voor het filteren van de regels via een tekstuele zoekopdracht. Het resultaat geeft alleen de regelgroepen en -regels die de tekst die u hebt gezocht bevatten.

![Regels zoeken][2]

## <a name="disable-rule-groups-and-rules"></a>Regels en regelgroepen uitschakelen

Wanneer u regels uitschakelen wilt, kunt u een volledige regelgroep of specifieke regels onder een of meer regelgroepen uitschakelen. 

**Regelgroepen of specifieke regels uit te schakelen**

   1. Zoek de regels of regelgroepen die u wilt uitschakelen.
   2. Schakel de selectievakjes voor de regels die u wilt uitschakelen. 
   2. Selecteer **Opslaan**. 

![Wijzigingen opslaan][3]

## <a name="mandatory-rules"></a>Verplichte regels

De volgende lijst bevat de voorwaarden die ertoe leiden dat het WAF moet worden geblokkeerd dat de aanvraag in de Preventiemodus (in de detectiemodus die ze zijn aangemeld als uitzonderingen). Deze kunnen niet worden geconfigureerd of uitgeschakeld:

* Fout bij parseren van de hoofdtekst van de aanvraag resulteert in de aanvraag wordt geblokkeerd, tenzij de hoofdtekst van de controle is uitgeschakeld (XML, JSON, formuliergegevens)
* Gegevenslengte aanvraag-instantie (met geen bestanden) is groter dan de geconfigureerde limiet
* De aanvraag hoofdtekst (met inbegrip van bestanden) is groter dan de limiet
* Er is een interne fout opgetreden in de WAF-engine

CRS 3.x specifieke:

* Inkomende anomaliedetectie score overschreden drempelwaarde

## <a name="next-steps"></a>Volgende stappen

Nadat u uw uitgeschakelde regels hebt geconfigureerd, leert u hoe u om uw WAF-logboeken weer te geven. Zie voor meer informatie, [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
