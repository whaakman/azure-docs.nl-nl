---
title: bestand opnemen
description: bestand opnemen
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 3/25/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 368a7cf1d372d79f062affe9b206d070afe5c2f7
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505725"
---
| Resource | Standaardlimiet |
| --- | --- |
| Doorvoer van gegevens |30 Gbps<sup>1</sup> |
|Regels|10.000, regel alle typen gecombineerd.|
|Minimumgrootte AzureFirewallSubnet |/26|
|Poortbereik in netwerk- en toepassingsregels|0-64,000. Werk wordt uitgevoerd op deze beperking te versoepelen.|
|Routetabel|AzureFirewallSubnet heeft standaard een route 0.0.0.0/0 met de waarde NextHopType is ingesteld op **Internet**.<br><br>Als u geforceerde tunneling naar inschakelt on-premises via ExpressRoute of VPN-Gateway, moet u expliciet een 0.0.0.0/0 door de gebruiker gedefinieerde route (UDR) configureren met het instellen van de waarde NextHopType als Internet en koppel deze aan uw AzureFirewallSubnet. Dit heeft voorrang op een mogelijke standaard-gateway BGP-aankondiging terug naar uw on-premises netwerk. Als uw organisatie geforceerde tunnels zijn voor de Firewall van Azure om te leiden van standaard gateway-verkeer via uw on-premises netwerk vereist, moet u contact op met ondersteuning. Kunnen we goedgekeurde uw abonnement om te controleren of de vereiste firewall verbinding met Internet wordt onderhouden.|

<sup>1</sup>als u deze limieten verhogen wilt, neem dan contact op met de ondersteuning van Azure.
