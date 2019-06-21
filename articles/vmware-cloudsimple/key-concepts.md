---
title: Belangrijkste concepten voor het beheren van VMware-oplossing van Azure door CloudSimple
description: Beschrijft de belangrijkste concepten voor het beheren van VMware-oplossing van Azure door CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3eff61408cb190396987ace6dee21182cff4f25c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165193"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>Belangrijkste concepten voor het beheer van Azure VMware-oplossing door CloudSimple

Beheer van Azure VMware-oplossing door CloudSimple vereist een goed begrip van de volgende concepten:

* CloudSimple-service, die wordt weergegeven als Azure VMware-oplossing door CloudSimple - Service
* CloudSimple knooppunt, die wordt weergegeven als Azure VMware oplossing CloudSimple - knooppunt
* CloudSimple privécloud
* Service-netwerken
* CloudSimple virtuele machine, die wordt weergegeven als Azure VMware-oplossing door CloudSimple - virtuele machine

## <a name="cloudsimple-service"></a>CloudSimple service

U kunt met de service CloudSimple maken en beheren van alle resources die zijn gekoppeld aan VMware-oplossingen door CloudSimple vanuit Azure portal. Een Serviceresource maken in elke regio waarin u van plan bent om de service te gebruiken.

Meer informatie over de [CloudSimple service](cloudsimple-service.md).

## <a name="cloudsimple-node"></a>CloudSimple knooppunt

Een knooppunt CloudSimple is een speciale, bare-metal, hypergeconvergeerd reken- en -host waarnaar de VMware ESXi-hypervisor wordt geïmplementeerd. Dit knooppunt wordt vervolgens opgenomen in de VMware vSphere, vCenter, vSAN en NSX platforms. CloudSimple netwerkservices en edge netwerkservices zijn ook ingeschakeld. Elk knooppunt fungeert als een eenheid van reken- en opslagcapaciteit die u inrichten kunt voor het maken van [CloudSimple privéclouds](cloudsimple-private-cloud.md). Richt of reserveren van knooppunten in een regio waar de service CloudSimple beschikbaar is.


Meer informatie over [CloudSimple knooppunten](cloudsimple-node.md).

## <a name="cloudsimple-private-cloud"></a>CloudSimple privécloud

Een privécloud CloudSimple is een geïsoleerde VMware-stack-omgeving beheerd door een vCenter-server in een eigen beheerdomein. De VMware-stack bevat ESXi-hosts, vSphere, vCenter, vSAN en NSX. De stack wordt uitgevoerd op toegewezen knooppunten (toegewezen en geïsoleerd bare-metal hardware) en wordt gebruikt door gebruikers met de systeemeigen VMware-hulpprogramma's met vCenter en NSX Manager. Toegewezen knooppunten worden geïmplementeerd in Azure-locaties en worden beheerd door Azure. Elke privécloud kan worden gesegmenteerd en beveiligd met behulp van netwerkservices zoals VLAN's en subnetten en firewall-tabellen. Verbindingen met uw on-premises omgeving en het Azure-netwerk worden gemaakt door met behulp van veilige, persoonlijke VPN- en Azure ExpressRoute verbindingen.

Meer informatie over [CloudSimple privécloud](cloudsimple-private-cloud.md).

## <a name="service-networking"></a>Service-netwerken

De service CloudSimple biedt een netwerk per regio waar uw CloudSimple-service is geïmplementeerd. Het netwerk is een één TCP-laag-3-adresruimte met routering is standaard ingeschakeld. Alle persoonlijke clouds en subnetten zijn gemaakt in deze regio worden met elkaar communiceren zonder extra configuratie. U maken gedistribueerde poortgroepen op de vCenter-met behulp van de VLAN's. U kunt de volgende functies van network gebruiken om te configureren en beveiligen van uw workload-resources in uw privécloud:

* [VLAN's en subnetten](cloudsimple-vlans-subnets.md)
* [Firewall-tabellen](cloudsimple-firewall-tables.md)
* [VPN-gateways](cloudsimple-vpn-gateways.md)
* [Openbare IP](cloudsimple-public-ip-address.md)
* [Azure-netwerkverbinding](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>CloudSimple virtuele machine

U kunt virtuele VMware-machines met de service CloudSimple beheren vanuit Azure portal. Een of meer clusters of resourcegroepen in uw omgeving vSphere kunnen worden toegewezen aan het abonnement waarin de service is gemaakt.

Meer informatie over:

* [CloudSimple virtuele machines](cloudsimple-virtual-machines.md)
* [Toewijzing van de Azure-abonnement](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
