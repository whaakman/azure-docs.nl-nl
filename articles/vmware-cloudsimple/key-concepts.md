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
ms.openlocfilehash: 0d890553ee145ca6aafed5a34d158c6a34d9af36
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358166"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>Belangrijkste concepten voor het beheer van Azure VMware-oplossing door CloudSimple

Azure VMware-oplossing door CloudSimple beheren, moet inzicht in de volgende concepten.

* CloudSimple Service (weergegeven als Azure VMware-oplossing door CloudSimple - Service)
* CloudSimple-knooppunt (weergegeven als Azure VMware-oplossing door CloudSimple - knooppunt)
* CloudSimple Private Cloud
* Service Network
* CloudSimple virtuele Machine (weergegeven als Azure VMware-oplossing door CloudSimple - virtuele Machine)

## <a name="cloudsimple-service"></a>CloudSimple service

De service CloudSimple kunt u maken en beheren van alle resources die zijn gekoppeld aan VMware-oplossingen door CloudSimple vanuit Azure portal. Een Serviceresource maken in elke regio waarin u van plan bent om de Service te gebruiken. 

Meer informatie over de [CloudSimple Service](cloudsimple-service.md)

## <a name="cloudsimple-node"></a>CloudSimple knooppunt

Een knooppunt CloudSimple is een speciale, bare metal hypergeconvergeerd reken- en host waarnaar de VMware ESXi-hypervisor wordt geïmplementeerd. Dit knooppunt wordt vervolgens opgenomen in de VMware vSphere, vCenter, vSAN en NSX platforms. CloudSimple netwerkservices en edge netwerkservices zijn ook ingeschakeld. Elk knooppunt fungeert als een eenheid van reken- en opslagcapaciteit die u kopen kunt voor het maken van [CloudSimple Privéclouds](cloudsimple-private-cloud.md). Aanschaf of het reserveren van knooppunten in een regio waar de service CloudSimple beschikbaar is.


Meer informatie over [CloudSimple knooppunten](cloudsimple-node.md)

## <a name="cloudsimple-private-cloud"></a>CloudSimple Private Cloud

Een CloudSimple Private Cloud is een geïsoleerde VMware-stack-omgeving beheerd door een vCenter-server in een eigen beheerdomein. VMware-stack bevat ESXi-hosts, vSphere, vCenter, vSAN en NSX.  De stack wordt uitgevoerd op toegewezen knooppunten (toegewezen en geïsoleerd bare metal hardware) en wordt gebruikt door gebruikers met de systeemeigen VMware-hulpprogramma's met vCenter en NSX Manager. Toegewezen knooppunten worden geïmplementeerd in Azure-locaties en worden beheerd door Azure. Elke Privécloud kan worden gesegmenteerd en beveiligd met behulp van netwerkservices, zoals VLAN's / subnetten en firewall-tabellen.  Verbindingen met uw on-premises omgeving en het Azure-netwerk worden gemaakt met behulp van veilige, persoonlijke VPN, en Azure ExpressRoute-verbindingen.

Meer informatie over [CloudSimple Private Cloud](cloudsimple-private-cloud.md)

## <a name="service-networking"></a>Service-netwerken

De service CloudSimple biedt een netwerk per regio waar uw CloudSimple-service is geïmplementeerd. Het netwerk is een één TCP-laag-3-adresruimte met routering is standaard ingeschakeld. Alle persoonlijke Clouds en subnetten zijn gemaakt in deze regio worden met elkaar communiceren zonder extra configuratie. U gedistribueerde poortgroepen gemaakt op de vCenter-met behulp van de VLAN's.  U kunt de volgende functies van network gebruiken om te configureren en beveiligen van uw workload-resources in uw Privécloud.

* [VLANs/subnets](cloudsimple-vlans-subnets.md)
* [Firewall-tabellen](cloudsimple-firewall-tables.md)
* [VPN-gateways](cloudsimple-vpn-gateways.md)
* [Openbare IP](cloudsimple-public-ip-address.md)
* [Azure-netwerkverbinding](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>CloudSimple virtuele machine

De service CloudSimple kunt u voor het beheren van virtuele VMware-machines in Azure portal. Een of meer clusters of resourcegroepen in uw omgeving vSphere kunnen worden toegewezen aan het abonnement waarin de service is gemaakt.

Meer informatie over:

* [CloudSimple virtuele machines](cloudsimple-virtual-machines.md)
* [Toewijzing van de Azure-abonnement](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
