---
title: Load Balancer TCP opnieuw instellen bij niet-actief in Azure
titlesuffix: Azure Load Balancer
description: Load Balancer met in twee richtingen TCP eerste pakketten op de time-out voor inactiviteit
services: load-balancer
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/29/2019
ms.author: kumud
ms.openlocfilehash: 52524e6291faae8ccc27c0d53e9e38ab63a4c8d5
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58132426"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer met TCP-opnieuw instellen op niet-actieve (openbare Preview)

U kunt [Standard Load Balancer](load-balancer-standard-overview.md) TCP opnieuw ingesteld op niet-actief inschakelen voor een bepaalde regel maakt een beter voorspelbare gedrag van toepassingen voor uw scenario's. Het standaardgedrag van de Load Balancer is stromen op de achtergrond te verwijderen wanneer de time-out voor inactiviteit van een stroom is bereikt.  Inschakelen van deze functie zorgt ervoor dat Load Balancer voor het verzenden van bidirectionele TCP wordt opnieuw ingesteld (TCP RST pakket) op de time-out voor inactiviteit.  Dit zal de toepassingseindpunten van uw te informeren dat de verbinding een time-out is en niet meer bruikbaar is.  Eindpunten kunnen onmiddellijk een nieuwe verbinding maken indien nodig.

![Load Balancer TCP opnieuw instellen](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>Load Balancer met TCP-opnieuw instellen van een time-out voor inactiviteit-functionaliteit is beschikbaar als openbare preview-versie op dit moment. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
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

Beschikbaar in alle regio's.

## <a name="limitations"></a>Beperkingen

- Portal kan niet worden gebruikt om te configureren of weergeven van de TCP-opnieuw instellen.  Gebruik in plaats daarvan sjablonen, REST-API, Az CLI 2.0 of PowerShell.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [standaardversie van Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over [regels voor uitgaand verkeer](load-balancer-outbound-rules-overview.md).
