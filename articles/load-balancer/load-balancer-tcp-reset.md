---
title: Load Balancer TCP opnieuw instellen op niet-actieve | Microsoft Docs
description: Load Balancer met in twee richtingen TCP eerste pakketten op de time-out voor inactiviteit
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: kumud
ms.openlocfilehash: 0b533a48e94db880f23d42decc5c3fb39a27e5ac
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395641"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer met TCP-opnieuw instellen op niet-actieve (openbare Preview)

U kunt [Standard Load Balancer](load-balancer-standard-overview.md) te maken van een beter voorspelbare gedrag van toepassingen voor uw scenario's met in twee richtingen TCP wordt opnieuw ingesteld (TCP RST pakket) voor elke configureerbare time-out voor inactiviteit.  Het standaardgedrag van de Load Balancer is stromen op de achtergrond te verwijderen wanneer de time-out voor inactiviteit van een stroom is bereikt.

![Load Balancer TCP opnieuw instellen](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>Load Balancer met TCP-opnieuw instellen van een time-out voor inactiviteit-functionaliteit is beschikbaar als openbare Preview op dit moment en beschikbaar in een beperkt aantal [regio's](#regions). Deze preview wordt aangeboden zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Bepaalde functies mogelijk niet ondersteund of mogelijk beperkt. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
U kunt dit standaardgedrag en inschakelen verzenden van TCP-wordt opnieuw ingesteld op de time-out voor inactiviteit op inkomende NAT-regels, load balancer-regels, wijzigen en [regels voor uitgaand verkeer](https://aka.ms/lboutboundrules).  Wanneer dit wordt ingeschakeld per regel, Load Balancer wordt verzonden in twee richtingen TCP (TCP RST pakketten) opnieuw instellen naar de client- en eindpunten op het moment van time-out voor inactiviteit voor alle overeenkomende stromen.

Eindpunten TCP RST pakketten ontvangt de bijbehorende socket onmiddellijk afgesloten. Dit biedt een onmiddellijke melding aan de eindpunten die de versie van de verbinding is opgetreden en alle toekomstige communicatie op dezelfde TCP-verbinding mislukken.  Toepassingen kunnen verbindingen verwijderen wanneer de socket wordt gesloten en opnieuw tot stand brengen van verbindingen naar behoefte zonder te wachten op de TCP-verbinding uiteindelijk time-out.

Voor veel scenario's, kan dit de keepalives hoeft te verzenden TCP (of het niveau van toepassing) om te vernieuwen van de time-out voor inactiviteit van een stroom verminderen. 

Als uw niet-actieve duur groter zijn dan die van de toegestaan door de configuratie of uw toepassing een ongewenst gedrag met TCP wordt ingeschakeld bevat, moet u mogelijk nog steeds gebruik van TCP keepalives (of toepassing laag keepalives) voor het bewaken van de liveness van de TCP-verbindingen.  Keepalives kan bovendien ook blijven handig voor als de verbinding via proxy ergens in het pad, met name laag keepalives van toepassing is.  

Zorgvuldig de hele end-to-end scenario om te bepalen of u profiteren van het inschakelen van TCP-wordt opnieuw ingesteld, de time-out voor inactiviteit, aanpassen en indien mogelijk extra stappen vereist om te controleren of de werking van de gewenste toepassing.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Inschakelen van de TCP-opnieuw instellen op de time-out voor inactiviteit

Met API-versie 2018-07-01, kunt u inschakelen verzenden van bidirectionele TCP wordt opnieuw ingesteld op de time-out voor inactiviteit op basis van per regel:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="regions"></a> Beschikbaarheid in regio 's

Deze parameter werkt momenteel in de volgende regio's.  In de regio's hier niet wordt vermeld, heeft de parameter geen effect.

| Regio |
|---|
| Azië - zuidoost |
| Europa - west |
| US - oost 2 |
| Verenigde Staten-Noord |
| US - west |

Deze tabel wordt bijgewerkt als de Preview-versie is uitgebreid naar andere regio's.  

## <a name="limitations"></a>Beperkingen

- Beperkte [beschikbaarheid in regio](#regions).
- Portal kan niet worden gebruikt om te configureren of weergeven van de TCP-opnieuw instellen.  Gebruik in plaats daarvan sjablonen, REST-API, Az CLI 2.0 of PowerShell.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [standaardversie van Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over [regels voor uitgaand verkeer](load-balancer-outbound-rules-overview.md).
