---
title: Azure Resource Manager ondersteuning voor Load Balancer | Microsoft Docs
description: Power shell gebruiken voor Load Balancer met Azure Resource Manager. Sjablonen gebruiken voor load balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 839b607b7787d51151401737848a46d7b66229dd
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275476"
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Azure Resource Manager ondersteuning gebruiken met Azure Load Balancer



Azure Resource Manager is het voorkeurs beheer raamwerk voor services in Azure. Azure Load Balancer kunnen worden beheerd met behulp van op Azure Resource Manager gebaseerde Api's en hulpprogram ma's.

## <a name="concepts"></a>Concepten

Met Resource Manager bevat Azure Load Balancer de volgende onderliggende resources:

* Front-end-IP-configuratie: een Load Balancer kan een of meer frontend-IP-adressen bevatten, ook wel een virtuele Ip's (Vip's) genoemd. Deze IP-adressen fungeren als ingang voor het verkeer.
* Back-end-adres groep: Dit zijn IP-adressen die zijn gekoppeld aan de netwerk interface kaart (NIC) van de virtuele machine waarop de belasting wordt gedistribueerd.
* Taakverdelings regels: een regel eigenschap wijst een opgegeven frontend-IP en poort combinatie toe aan een set back-end-IP-adressen en poort combinatie. Een enkele load balancer kan meerdere regels voor taakverdeling bevatten. Elke regel is een combi natie van een frontend-IP en-poort en een back-end-IP en poort die is gekoppeld aan Vm's.
* Tests: tests bieden u de mogelijkheid om de status van VM-exemplaren bij te houden. Als een status test mislukt, wordt het VM-exemplaar automatisch uit de rotatie gehaald.
* Binnenkomende NAT-regels: NAT-regels die het binnenkomende verkeer bepalen dat door het frontend-IP-adres wordt doorgeleid en naar het back-end-IP-adres wordt gedistribueerd.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Snelstartsjablonen

Met Azure Resource Manager kunt u uw toepassingen inrichten aan de hand van een declaratieve sjabloon. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U gebruikt dezelfde sjabloon om uw toepassing herhaaldelijk te implementeren in elke fase van de levenscyclus van de toepassing.

Sjablonen kunnen definities bevatten voor Virtual Machines, virtuele netwerken, beschikbaarheids sets, netwerk interfaces (Nic's), opslag accounts, load balancers, netwerk beveiligings groepen en open bare Ip's. Met sjablonen kunt u alles wat u nodig hebt voor een complexe toepassing maken. Het sjabloon bestand kan worden ingecheckt voor het inhouds beheersysteem voor versie beheer en samen werking.

[Meer informatie over sjablonen](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Meer informatie over netwerk bronnen](../networking/networking-overview.md)

Zie voor Quick Start-sjablonen met Azure Load Balancer de [github-opslag plaats](https://github.com/Azure/azure-quickstart-templates) die als host fungeert voor een set door de Community gegenereerde sjablonen.

Voor beelden van sjablonen:

* [2 Vm's in een Load Balancer en taakverdelings regels](https://go.microsoft.com/fwlink/?LinkId=544799)
* [2 Vm's in een VNET met een interne Load Balancer en Load Balancer regels](https://go.microsoft.com/fwlink/?LinkId=544800)
* [2 Vm's in een Load Balancer en NAT-regels configureren op de LB](https://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Azure Load Balancer instellen met een Power shell of CLI

Aan de slag met Azure Resource Manager-cmdlets, opdracht regel Programma's en REST-Api's

* [Azure-netwerk](https://docs.microsoft.com/powershell/module/az.network#networking) -cmdlets kunnen worden gebruikt om een Load Balancer te maken.
* [Een load balancer maken met behulp van Azure Resource Manager](load-balancer-get-started-ilb-arm-ps.md)
* [Azure CLI gebruiken met Azure resource management](../xplat-cli-azure-resource-manager.md)
* [Load Balancer REST-Api's](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Volgende stappen

U kunt ook [aan de slag gaan met het maken van een Internet gerichte Load Balancer](load-balancer-get-started-internet-arm-ps.md) en het type [distributie modus](load-balancer-distribution-mode.md) configureren voor een bepaald Load Balancer gedrag van netwerk verkeer.

Meer informatie over het beheren [van instellingen voor inactieve TCP-time-out voor een Load Balancer](load-balancer-tcp-idle-timeout.md). Dit is belang rijk wanneer uw toepassing verbindingen in stand moet houden voor servers achter een load balancer.
