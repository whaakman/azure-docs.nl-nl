---
title: Web application firewall-aanvraaglimieten grootte en uitsluitingslijsten in Azure Application Gateway - Azure portal
description: In dit artikel bevat informatie over web application firewall-aanvraaglimieten grootte en uitsluiting geeft een lijst van de configuratie in Application Gateway met Azure portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.workload: infrastructure-services
ms.date: 11/6/2018
ms.author: victorh
ms.openlocfilehash: 4e57181b62a6d9070c0b2e4de5008e47b62c56bf
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54301896"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Web application firewall-aanvraaglimieten grootte en uitsluitingslijsten

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Dit artikel wordt beschreven WAF grootte aanvraaglimieten en uitsluiting geeft een lijst van de configuratie.

## <a name="waf-request-size-limits"></a>Maximale grootte van WAF-aanvraag

![Maximale grootte aanvragen](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

Web Application Firewall kunnen gebruikers grootte aanvraaglimieten in kleine en bovengrens configureren. De volgende configuraties van de grootte van de twee limieten zijn beschikbaar:

- Het veld voor maximale aanvraag hoofdtekst van de grootte is opgegeven in kB's en besturingselementen algemene aanvraag grootte, met uitzondering van elk bestand wordt geüpload. Dit veld kan variëren van 1 KB minimum aan de maximumwaarde 128 KB. De standaardwaarde voor de aanvraag hoofdtekst van de grootte is 128 KB.
- Het veld met bestand uploaden limiet is opgegeven in MB en deze verklaring regelt de maximum toegestane bestandsgrootte voor uploaden. Dit veld kan een minimumwaarde van 1 MB en maximaal 500 MB voor grote SKU exemplaren hebben terwijl gemiddeld SKU een maximum van 100 MB heeft. De standaardwaarde voor bestand uploadlimiet is 100 MB.

WAF biedt ook een knop kunnen worden geconfigureerd om in te schakelen van de controle van de hoofdtekst van de aanvraag voor in- of uitschakelen. De aanvraag hoofdtekst van de controle is standaard ingeschakeld. Als de aanvraag hoofdtekst van de controle is uitgeschakeld, WAF niet de inhoud van de hoofdtekst van de HTTP-bericht worden geëvalueerd. In dergelijke gevallen blijft WAF WAF-regels op headers, cookies en URI afgedwongen. Als de aanvraag hoofdtekst van de controle is uitgeschakeld, veld voor maximale aanvraag hoofdtekst van de grootte is niet van toepassing en kan niet worden ingesteld. Uitschakelen van de aanvraag hoofdtekst controle mogelijk voor berichten die groter zijn dan 128 KB worden verzonden naar WAF, maar de berichttekst wordt niet gecontroleerd op beveiligingsproblemen.

## <a name="waf-exclusion-lists"></a>WAF-uitsluitingslijsten

![waf-exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

WAF-uitsluitingslijsten toestaan dat gebruikers bepaalde kenmerken van een evaluatie WAF weglaten. Een veelvoorkomend voorbeeld is de dat Active Directory-tokens die worden gebruikt voor verificatie of wachtwoordvelden ingevoegd. Deze kenmerken zijn gevoelig zijn voor de speciale tekens bevatten die een fout-positief vanaf de WAF-regels kunnen activeren. Wanneer een kenmerk is toegevoegd aan de uitsluitingslijst WAF, is niet het in aanmerking genomen door alle geconfigureerde en actieve WAF-regel. Uitsluitingslijsten zijn globaal binnen het bereik.

De volgende kenmerken kunnen worden toegevoegd aan een lijst met uitgesloten:

* Aanvraagheaders
* Cookies van de aanvraag
* Aanvraagtekst

   * Meerdelige formuliergegevens
   * XML
   * JSON

U kunt opgeven van een exacte aanvraagheader body cookie of query-tekenreeksovereenkomst kenmerk of optioneel gedeeltelijke overeenkomsten kunt opgeven.

Hier volgen de ondersteunde overeenkomst criteria operators:

- **Is gelijk aan**:  Deze operator wordt gebruikt voor een exacte overeenkomst. Een voorbeeld: voor het selecteren van een koptekst met de naam **bearerToken**, gebruikt u de operator is gelijk aan met de selector instellen als **bearerToken**.
- **Begint met**: Deze operator komt overeen met alle velden die met de selectie van de opgegeven waarde beginnen.
- **Eindigt met**:  Deze operator komt overeen met alle velden op aanvraag die met de selectie van de opgegeven waarde eindigen.
- **Bevat**: Deze operator komt overeen met alle velden op aanvraag die de opgegeven selector waarde bevatten.

In alle gevallen wordt onderscheid en reguliere expressies zijn niet toegestaan als het selectoren.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw WAF-instellingen configureert, leert u hoe u om uw WAF-logboeken weer te geven. Zie voor meer informatie, [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).
