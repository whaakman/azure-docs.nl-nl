---
title: Azure Resource Manager-ondersteuning voor Load Balancer | Microsoft Docs
description: Met behulp van powershell voor Load Balancer met Azure Resource Manager. Met behulp van sjablonen voor load balancer
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 596ac871067886ee3124c0f21beb35cb3b8fe1ae
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593808"
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Ondersteuning voor Azure Resource Manager gebruiken met Azure Load Balancer



Azure Resource Manager is het gewenste management framework voor services in Azure. Azure Load Balancer kan worden beheerd met behulp van Azure Resource Manager gebaseerde API's en hulpprogramma's.

## <a name="concepts"></a>Concepten

Met Resource Manager, Azure Load Balancer bevat de volgende onderliggende bronnen:

* Front-end-IP-configuratie: een Load balancer kan een of meer front-end IP-adressen, ook bekend als een virtueel IP-adressen (VIP's) bevatten. Deze IP-adressen fungeren als ingang voor het verkeer.
* Back-end-adresgroep: dit zijn IP-adressen die zijn gekoppeld met de virtuele machine netwerk netwerkkaart (NIC) waarop taakverdeling wordt gedistribueerd.
* Taakverdelingsregels: een voor de regeleigenschap wijst een bepaalde frontend-IP en poort op een set back-end IP-adressen en poort. Een enkele load balancer kan meerdere regels voor taakverdeling bevatten. Elke regel is een combinatie van een front-end-IP en poort en back-end IP en poort die is gekoppeld aan virtuele machines.
* Tests: tests kunnen u de status van VM-exemplaren bij te houden. Als een statustest mislukt, wordt het VM-exemplaar automatisch uit rotatie gehaald.
* Inkomende NAT-regels, NAT-regels definiëren van het binnenkomende verkeer via de frontend-IP en gedistribueerd naar het back-end-IP-adres.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Snelstartsjablonen

Met Azure Resource Manager kunt u uw toepassingen inrichten aan de hand van een declaratieve sjabloon. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U gebruikt dezelfde sjabloon om uw toepassing herhaaldelijk te implementeren in elke fase van de levenscyclus van de toepassing.

Sjablonen kunnen definities voor virtuele Machines, virtuele netwerken, Beschikbaarheidssets, netwerkinterfaces (NIC's), Opslagaccounts, Load Balancers, Netwerkbeveiligingsgroepen en openbare IP-adressen bevatten. Met sjablonen, kunt u alles wat die u nodig hebt voor een complexe toepassing maken. Het sjabloonbestand kan worden gecontroleerd in content management-systeem voor versiebeheer en samenwerking.

[Meer informatie over sjablonen](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Meer informatie over netwerkbronnen](../networking/networking-overview.md)

Zie voor Quickstart-sjablonen met behulp van Azure Load Balancer, de [GitHub-opslagplaats](https://github.com/Azure/azure-quickstart-templates) die als host fungeert voor een aantal community is gegenereerd.

Voorbeelden van sjablonen:

* [2 virtuele machines in een Load Balancer en taakverdelingsregels](https://go.microsoft.com/fwlink/?LinkId=544799)
* [2 virtuele machines in een VNET met een interne Load Balancer en Load Balancer-regels](https://go.microsoft.com/fwlink/?LinkId=544800)
* [2 virtuele machines in een Load Balancer en NAT-regels configureren op de LB](https://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Instellen van Azure Load Balancer met een PowerShell of CLI

Aan de slag met Azure Resource Manager-cmdlets, opdrachtregelprogramma's en REST-API 's

* [Azure-netwerken Cmdlets](https://docs.microsoft.com/powershell/module/az.network#networking) kan worden gebruikt voor het maken van een Load Balancer.
* [Over het maken van een load balancer met behulp van Azure Resource Manager](load-balancer-get-started-ilb-arm-ps.md)
* [De Azure CLI gebruiken met Azure Resource Manager](../xplat-cli-azure-resource-manager.md)
* [Load Balancer REST-API 's](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Volgende stappen

U kunt ook [maken van een internetgerichte load balancer](load-balancer-get-started-internet-arm-ps.md) en configureren welk type [distributiemodus](load-balancer-distribution-mode.md) voor een gedrag voor netwerkverkeer van specifieke load balancer.

Meer informatie over het beheren van [actieve TCP-time-outinstellingen voor een load balancer](load-balancer-tcp-idle-timeout.md). Dit is belangrijk wanneer uw toepassing moet verbindingen actief houden voor servers achter een load balancer.
