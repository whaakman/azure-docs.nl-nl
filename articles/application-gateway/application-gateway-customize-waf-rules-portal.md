---
title: Regels voor web application firewall in Azure Application Gateway - Azure-portal aanpassen | Microsoft Docs
description: In dit artikel bevat informatie over het aanpassen van web application firewall-regels in Application Gateway met Azure portal.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: victorh
ms.openlocfilehash: 30df26dc3a9697d3435779f91c32b2d99a747b88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990464"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Regels voor web application firewall via de Azure-portal aanpassen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure-CLI](application-gateway-customize-waf-rules-cli.md)

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Deze beveiligingen worden geleverd door de Open Web Application Security Project (OWASP) Core regel ingesteld (CRS). Sommige regels kunnen fout-positieven en echte verkeer blokkeert. Om deze reden biedt Application Gateway de mogelijkheid om aan te passen regelgroepen en -regels. Zie voor meer informatie over de specifieke regelgroepen en regels [lijst met web application firewall CRS-regelgroepen en -regels](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Als uw application gateway niet voor de WAF-laag gebruikt wordt, wordt de optie voor het upgraden van de application gateway naar de WAF-laag wordt weergegeven in het rechter deelvenster. 

![WAF inschakelen][fig1]

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

Wanneer uw bent uitschakelen regels, kunt u een volledige regelgroep of uitschakelen specifieke regels onder een of meer regelgroepen. 

**Regelgroepen of specifieke regels uit te schakelen**

   1. Zoek de regels of regelgroepen die u wilt uitschakelen.
   2. Schakel de selectievakjes voor de regels die u wilt uitschakelen. 
   2. Selecteer **Opslaan**. 

![Wijzigingen opslaan][3]

## <a name="next-steps"></a>Volgende stappen

Nadat u uw uitgeschakelde regels hebt geconfigureerd, leert u hoe u om uw WAF-logboeken weer te geven. Zie voor meer informatie, [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
