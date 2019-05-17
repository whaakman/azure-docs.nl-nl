---
title: Web application firewall-aanvraaglimieten grootte en uitsluitingslijsten in Azure Application Gateway - Azure portal
description: In dit artikel bevat informatie over web application firewall-aanvraaglimieten grootte en uitsluiting geeft een lijst van de configuratie in Application Gateway met Azure portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 5/15/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 5ddcdeca41e2f21fa27db25f7e0721c7ef87e491
ms.sourcegitcommit: 3675daec6c6efa3f2d2bf65279e36ca06ecefb41
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65620287"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Web application firewall-aanvraaglimieten grootte en uitsluitingslijsten

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Dit artikel wordt beschreven WAF grootte aanvraaglimieten en uitsluiting geeft een lijst van de configuratie.

## <a name="waf-request-size-limits"></a>Maximale grootte van WAF-aanvraag

![Maximale grootte aanvragen](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

Web Application Firewall kunt u grootte aanvraaglimieten in kleine en bovengrens configureren. De volgende configuraties van de grootte van de twee limieten zijn beschikbaar:

- Het veld voor maximale aanvraag hoofdtekst van de grootte is opgegeven in kilobytes en besturingselementen algemene aanvraag grootte, met uitzondering van elk bestand wordt geüpload. Dit veld kan variëren van 1 KB minimum aan de maximumwaarde 128 KB. De standaardwaarde voor de aanvraag hoofdtekst van de grootte is 128 KB.
- Het veld met bestand uploaden limiet is opgegeven in MB en deze verklaring regelt de maximum toegestane bestandsgrootte voor uploaden. Dit veld kan een minimumwaarde van 1 MB en maximaal 500 MB voor grote SKU exemplaren hebben terwijl gemiddeld SKU een maximum van 100 MB heeft. De standaardwaarde voor bestand uploadlimiet is 100 MB.

WAF biedt ook een knop kunnen worden geconfigureerd om in te schakelen van de controle van de hoofdtekst van de aanvraag voor in- of uitschakelen. De aanvraag hoofdtekst van de controle is standaard ingeschakeld. Als de aanvraag hoofdtekst van de controle is uitgeschakeld, evalueren niet de inhoud van de hoofdtekst van de HTTP-bericht van WAF. In dergelijke gevallen blijft WAF WAF-regels op headers, cookies en URI afgedwongen. Als de aanvraag hoofdtekst van de controle is uitgeschakeld, veld voor maximale aanvraag hoofdtekst van de grootte is niet van toepassing en kan niet worden ingesteld. Uitschakelen van de aanvraag hoofdtekst controle mogelijk voor berichten die groter zijn dan 128 KB worden verzonden naar WAF, maar de berichttekst wordt niet gecontroleerd op beveiligingsproblemen.

## <a name="waf-exclusion-lists"></a>WAF-uitsluitingslijsten

![waf-exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

WAF-uitsluitingslijsten kunnen u bepaalde kenmerken van een evaluatie WAF weglaten. Een veelvoorkomend voorbeeld is de dat Active Directory-tokens die worden gebruikt voor verificatie of wachtwoordvelden ingevoegd. Deze kenmerken zijn gevoelig zijn voor de speciale tekens bevatten die een fout-positief vanaf de WAF-regels kunnen activeren. Wanneer een kenmerk is toegevoegd aan de uitsluitingslijst WAF, niet wordt dit beschouwd als door geen enkele regel van de WAF geconfigureerd en actief. Uitsluitingslijsten zijn globaal binnen het bereik.

De volgende kenmerken kunnen worden toegevoegd aan een lijst met uitgesloten:

* Aanvraagheaders
* Cookies van de aanvraag
* Naam van de aanvraag-kenmerk (argumenty)

   * Meerdelige formuliergegevens
   * XML
   * JSON
   * Argumenten voor URL-query

U kunt een exacte aanvraagheader, hoofdtekst, cookie opgeven of kenmerk tekenreeksovereenkomst query.  Of u kunt optioneel gedeeltelijke overeenkomsten opgeven. De uitsluiting is altijd op een headerveld nooit op de waarde ervan. Uitsluitingsregels zijn van toepassing binnen het bereik en van toepassing op alle pagina's en alle regels.

Hier volgen de ondersteunde overeenkomst criteria operators:

- **Is gelijk aan**:  Deze operator wordt gebruikt voor een exacte overeenkomst. Een voorbeeld: voor het selecteren van een koptekst met de naam **bearerToken**, gebruikt u de operator is gelijk aan met de selector instellen als **bearerToken**.
- **Begint met**: Deze operator komt overeen met alle velden die met de selectie van de opgegeven waarde beginnen.
- **Eindigt met**:  Deze operator komt overeen met alle velden op aanvraag die met de selectie van de opgegeven waarde eindigen.
- **Bevat**: Deze operator komt overeen met alle velden op aanvraag die de opgegeven selector waarde bevatten.
- **Gelijk aan willekeurig**: Deze operator komt overeen met alle velden van de aanvraag. * is de waarde selector.

In alle gevallen wordt onderscheid en reguliere expressies zijn niet toegestaan als het selectoren.

### <a name="examples"></a>Voorbeelden

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De volgende voorbeelden ziet het gebruik van uitsluitingen.

### <a name="example-1"></a>Voorbeeld 1

In dit voorbeeld dat u wilt uitsluiten van de gebruikersagent-header. De aanvraagheader gebruikersagent bevat een kenmerkende tekenreeks waarmee de netwerk-protocol peers voor het identificeren van het toepassingstype, besturingssysteem, softwareleverancier of softwareversie van de aanvragende gebruikersagent. Zie voor meer informatie, [gebruikersagent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).

Er mag een willekeurig aantal redenen om uit te schakelen voor het evalueren van deze header. Er zijn mogelijk een tekenreeks is die de WAF wordt weergegeven en wordt ervan uitgegaan dat het schadelijk is. Bijvoorbeeld, in de klassieke SQL aanval "x = x ' in een tekenreeks. In sommige gevallen kan kan dit legitieme verkeer zijn. Dus als u wilt uitsluiten van deze header van de evaluatieversie van WAF.

De volgende Azure PowerShell-cmdlet niet van toepassing op de kop van de gebruikersagent van de evaluatieversie:

```azurepowershell
$exclusion1 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestHeaderNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "User-Agent"
```

### <a name="example-2"></a>Voorbeeld 2

In dit voorbeeld omvat niet de waarde in de *gebruiker* parameter die in de aanvraag wordt doorgegeven via de URL. Stel bijvoorbeeld dat het is gebruikelijk in uw omgeving voor het veld met de gebruikers bevatten van een tekenreeks is die de WAF-als schadelijke inhoud weergaven zodat deze worden geblokkeerd.  U kunt de parameter van de gebruiker in dit geval uitsluiten zodat de WAF alles in het veld niet evalueren.

De volgende Azure PowerShell-cmdlet niet van toepassing op de parameter van de gebruiker van de evaluatieversie:

```azurepowershell
$exclusion2 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestArgNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "user"
```
Dus als de URL **http://www.contoso.com/?user=fdafdasfda** wordt doorgegeven aan de WAF de tekenreeks niet beoordeeld **fdafdasfda**.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw WAF-instellingen configureert, leert u hoe u om uw WAF-logboeken weer te geven. Zie voor meer informatie, [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).
