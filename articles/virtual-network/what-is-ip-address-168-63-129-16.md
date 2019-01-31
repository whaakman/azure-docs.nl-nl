---
title: Wat is een IP-adres 168.63.129.16? | Microsoft Docs
description: Meer informatie over IP-adres 168.63.129.16 en hoe het werkt met uw resources.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2019
ms.author: genli
ms.openlocfilehash: 7f0539e7c2f7e5ae8847b35b47d3708c6c5d6a09
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296035"
---
# <a name="what-is-ip-address-1686312916"></a>Wat is een IP-adres 168.63.129.16?

IP-adres 168.63.129.16 is een virtuele openbare IP-adres dat wordt gebruikt om een communicatiekanaal met Azure-platform bronnen mogelijk te maken. Klanten kunnen een adresruimte die voor hun privé virtueel netwerk definiëren in Azure. Daarom moeten de Azure-platform-resources worden aangeboden als een unieke openbare IP-adres. Deze virtuele openbare IP-adres vereenvoudigt het uitvoeren van de volgende zaken:

- Hiermee kunt de VM-Agent om te communiceren met de Azure-platform om aan te geven dat deze zich op een status 'Gereed'.
- Communicatie met de virtuele DNS-server voor naamomzetting gefilterde tot de resources (zoals VM's) die nog geen een aangepaste DNS-server mogelijk maakt. Deze filtering ervoor zorgt dat alleen de hostnamen van de resources en klanten omzetten kunnen.
- Hiermee kunt statuscontroles van de load balancer om de status van virtuele machines in een load balancer-set te bepalen.
- Kunt Guest Agent heartbeat-berichten voor de PaaS-rol.

## <a name="scope-of-ip-address-1686312916"></a>Bereik van IP-adres 168.63.129.16

Virtuele openbare IP-adres 168.63.129.16 wordt gebruikt in alle regio's en alle nationale clouds. Deze speciale openbare IP-adres verandert niet. Het is toegestaan door de regel voor standaard de netwerkbeveiligingsgroep. Het is raadzaam dat u dit IP-adres in een lokale firewall-beleid toestaan. De communicatie tussen deze speciale IP-adres en de resources is veilig, omdat alleen het interne Azure-platform kan een bericht van dit IP-adres van bron. Als dit adres wordt geblokkeerd, kan onverwacht gedrag optreden in een verscheidenheid aan scenario's.

Bovendien kunt u verwachten dat verkeer naar het stromen van de virtuele openbare IP-adres 168.63.129.16 naar het eindpunt dat is geconfigureerd voor een [Load Balancer-statustest](../load-balancer/load-balancer-custom-probe-overview.md). In een netwerkscenario voor niet-virtuele, de statustest afkomstig is uit een privé IP-adres. 

## <a name="next-steps"></a>Volgende stappen

- [Beveiligingsgroepen](security-overview.md)
- [Een netwerkbeveiligingsgroep maken, wijzigen of verwijderen](manage-network-security-group.md)
