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
ms.date: 05/07/2019
ms.author: genli
ms.openlocfilehash: 78d2392e32465b3091c49032dc5df5f3a5b6061a
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416039"
---
# <a name="what-is-ip-address-1686312916"></a>Wat is een IP-adres 168.63.129.16?

IP-adres 168.63.129.16 is een virtuele openbare IP-adres dat wordt gebruikt om een communicatiekanaal met Azure-platform bronnen mogelijk te maken. Klanten kunnen een adresruimte die voor hun privé virtueel netwerk definiëren in Azure. Daarom moeten de Azure-platform-resources worden aangeboden als een unieke openbare IP-adres. Deze virtuele openbare IP-adres vereenvoudigt het uitvoeren van de volgende zaken:

- Hiermee kunt de VM-Agent om te communiceren met de Azure-platform om aan te geven dat deze zich op een status 'Gereed'.
- Communicatie met de virtuele DNS-server voor naamomzetting gefilterde tot de resources (zoals VM's) die nog geen een aangepaste DNS-server mogelijk maakt. Deze filtering ervoor zorgt dat alleen de hostnamen van de resources en klanten omzetten kunnen.
- Hiermee kunt [statuscontroles van Azure load balancer](../load-balancer/load-balancer-custom-probe-overview.md) om de status van virtuele machines te bepalen.
- Kan de virtuele machine om te verkrijgen van een dynamisch IP-adres van de DHCP-service in Azure.
- Kunt Guest Agent heartbeat-berichten voor de PaaS-rol.

## <a name="scope-of-ip-address-1686312916"></a>Bereik van IP-adres 168.63.129.16

Het openbare IP-adres 168.63.129.16 wordt gebruikt in alle regio's en alle nationale clouds. Deze speciale openbare IP-adres is eigendom van Microsoft en heeft geen invloed op. Het is toegestaan door de regel voor standaard de netwerkbeveiligingsgroep. Het is raadzaam dat u dit IP-adres in een lokale firewall-beleid toestaan. De communicatie tussen deze speciale IP-adres en de resources is veilig, omdat alleen het interne Azure-platform kan een bericht van dit IP-adres van bron. Als dit adres wordt geblokkeerd, kan onverwacht gedrag optreden in een verscheidenheid aan scenario's.

[Azure Load Balancer statuscontroles](../load-balancer/load-balancer-custom-probe-overview.md) afkomstig is van dit IP-adres. Als u dit IP-adres blokkeert, mislukken uw tests.

De statustest afkomstig is uit een privé IP-adres en 168.63.129.16 wordt niet gebruikt in een netwerkscenario met niet-virtuele.

## <a name="next-steps"></a>Volgende stappen

- [Beveiligingsgroepen](security-overview.md)
- [Een netwerkbeveiligingsgroep maken, wijzigen of verwijderen](manage-network-security-group.md)
