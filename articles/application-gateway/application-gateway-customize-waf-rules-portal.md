---
title: Web application firewall-regels in Azure Application Gateway - Azure-portal aanpassen | Microsoft Docs
description: In dit artikel bevat informatie over het aanpassen van web application firewall-regels in Application Gateway met de Azure-portal.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: davidmu
ms.openlocfilehash: 406e491aa54806b3534ef0f500d6aea03434e2c2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Web application firewall-regels via de Azure portal aanpassen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Deze beveiligingen worden geleverd door de toepassing van Open Web Project beveiliging (OWASP) Core regel ingesteld (CRS). Sommige regels kunnen valse positieven veroorzaken en echte verkeer blokkeert. Daarom biedt Application Gateway de mogelijkheid om aan te passen regelgroepen en regels. Zie voor meer informatie over de specifieke regelgroepen en regels [lijst van groepen met web application firewall CRS regels en voorschriften](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Als uw toepassingsgateway niet van de laag WAF gebruikmaakt, wordt de optie voor het upgraden van de toepassingsgateway aan de laag WAF in het rechterdeelvenster weergegeven. 

![WAF inschakelen][fig1]

## <a name="view-rule-groups-and-rules"></a>Groepen van de regel weergeven en regels

**Regelgroepen en regels weergeven**
   1. Blader naar de toepassingsgateway en selecteer vervolgens **Web application firewall**.  
   2. Selecteer **geavanceerde regelconfiguratie**.  
   Deze weergave bevat een tabel op de pagina van alle regelgroepen opgegeven met de gekozen regelset. Alle selectievakjes voor de regel zijn geselecteerd.

![Uitgeschakelde regels configureren][1]

## <a name="search-for-rules-to-disable"></a>Zoeken naar regels uitschakelen

De **Web application firewall-instellingen** blade biedt de mogelijkheid voor het filteren van de regels via een tekst te zoeken. Het resultaat geeft alleen de regelgroepen en regels die de tekst die u zoekt.

![Zoeken naar regels][2]

## <a name="disable-rule-groups-and-rules"></a>Regelgroepen en regels uitschakelen

Wanneer uw bent regels is uitgeschakeld, kunt u een volledige regelgroep of uitschakelen specifieke regels onder een of meer regelgroepen. 

**Regelgroepen of specifieke regels uit te schakelen**

   1. Zoeken naar de regels of regelgroepen die u wilt uitschakelen.
   2. Schakel de selectievakjes voor de regels die u wilt uitschakelen. 
   2. Selecteer **Opslaan**. 

![Wijzigingen opslaan][3]

## <a name="next-steps"></a>Volgende stappen

Nadat u uw uitgeschakelde regels hebt geconfigureerd, kunt u informatie over het weergeven van uw WAF Logboeken. Zie voor meer informatie [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
