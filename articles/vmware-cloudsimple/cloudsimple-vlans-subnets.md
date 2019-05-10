---
title: VLAN's en subnetten in VMware-oplossing door CloudSimple - Azure
description: Meer informatie over de VLAN's en subnetten in een privécloud CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e88977cc4d99df176116e6be7d8e06adb6297782
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209582"
---
# <a name="vlans-and-subnets-overview"></a>Overzicht van VLAN's en subnetten

CloudSimple biedt een netwerk per regio waar uw CloudSimple-service is geïmplementeerd.  Het netwerk is een één TCP-laag-3-adresruimte met routering is standaard ingeschakeld.  Alle persoonlijke clouds en subnetten zijn gemaakt in deze regio kunnen met elkaar communiceren zonder extra configuratie.  U kunt gedistribueerde poortgroepen maken op de vCenter-met behulp van de VLAN's.

## <a name="vlans"></a>VLAN 's

VLAN's (Layer 2-netwerk) worden per privécloud gemaakt.  De laag-2-verkeer blijft binnen de grens van een privécloud, zodat u kunt het lokale verkeer binnen de privécloud te isoleren.  Een VLAN gemaakt in de privécloud kan worden gebruikt om gedistribueerde poortgroepen alleen in deze privécloud.  Een VLAN gemaakt in een privécloud wordt automatisch geconfigureerd op de switches die zijn verbonden met de hosts van een privécloud.

## <a name="subnets"></a>Subnetten

U kunt een subnet maken wanneer u een VLAN maakt met het definiëren van de adresruimte van het subnet. Een IP-adres van de adresruimte is toegewezen als de gateway van een subnet. Een enkele privé Layer 3-adresruimte is toegewezen per klant en de regio. U kunt elke gewenste RFC 1918 niet-overlappende adresruimte, met uw on-premises netwerk of de Azure-netwerk configureren in de regio van uw netwerk.

Alle subnetten kunnen standaard, waardoor de configuratie van de overhead voor routering tussen persoonlijke clouds met elkaar communiceren. Oost-west-gegevens voor pc's in dezelfde regio blijft in de dezelfde laag-3-netwerk en brengt via de infrastructuur van het lokale netwerk binnen de regio. Er is geen uitgaande is vereist voor communicatie tussen persoonlijke clouds in een regio. Deze benadering wordt voorkomen dat een WAN/uitgaand verkeer op de prestaties bij het implementeren van verschillende werkbelastingen in verschillende privéclouds.

## <a name="vspherevsan-subnets-cidr-range"></a>vSphere/vSAN subnetten CIDR-bereik

Een Privécloud is gemaakt als een geïsoleerde VMware-stack (ESXi-hosts, vCenter, vSAN en NSX) omgeving die wordt beheerd door een vCenter-server.  De onderdelen worden geïmplementeerd in het netwerk geselecteerd voor **vSphere/vSAN subnetten CIDR**.  Het netwerk CIDR-bereik is onderverdeeld in verschillende subnetten tijdens de implementatie.

Minimale vSphere/vSAN subnetten CIDR-bereik voorvoegsel: **/24** Maximum vSphere/vSAN subnetten CIDR-bereik voorvoegsel:   **/21**

### <a name="vspherevsan-subnets-cidr-range-limits"></a>vSphere/vSAN subnetten CIDR-bereik limieten

Selecteren van vSphere/vSAN subnetten CIDR-bereik grootte, heeft een invloed op de grootte van uw Privécloud.  De onderstaande tabel ziet u het maximum aantal knooppunten die u kan zijn gebaseerd op de grootte van vSphere/vSAN subnetten CIDR.

| Opgegeven vSphere/vSAN subnetten CIDR voorvoegsellengte | Maximumaantal knooppunten |
|---------------------------------------------------|-------------------------|
| /24 | 26 |
| /23 | 58 |
| /22 | 118 |
| /21 | 220 |

### <a name="management-subnets-created-on-a-private-cloud"></a>Management-subnetten die zijn gemaakt op een Privécloud

Volgende management subnetten worden gemaakt wanneer u een Privécloud maken. 

* **Systeembeheer** -VLAN-subnet voor het beheer van de ESXi-hosts en netwerk, DNS-server, vCenter-server.
* **VMotion** -VLAN en het subnetmasker voor ESXi-hosts vMotion netwerk.
* **Virtueel SAN** -VLAN en het subnetmasker voor ESXi-hosts vSAN netwerk.
* **NsxtEdgeUplink1** -VLAN en het subnetmasker voor VLAN uplinks met een extern netwerk.
* **NsxtEdgeUplink2** -VLAN en het subnetmasker voor VLAN uplinks met een extern netwerk.
* **NsxtEdgeTransport** -VLAN- en voor transport zones bepalen het bereik van laag 2-netwerken in NSX-T.
* **NsxtHostTransport** -VLAN- en voor host transport zone.

### <a name="management-network-cidr-range-breakdown"></a>Uitsplitsing van Management netwerk CIDR-bereik

vSphere/vSAN subnetten CIDR-bereik is opgegeven is onderverdeeld in meerdere subnetten.  De onderstaande tabel ziet u een voorbeeld van de indeling voor toegestane voorvoegsels.  Het voorbeeld wordt **192.168.0.0** als de CIDR-bereik.

Voorbeeld:

| Opgegeven vSphere/vSAN subnetten CIDR-voorvoegsel | 192.168.0.0/21 | 192.168.0.0/22 | 192.168.0.0/23 | 192.168.0.0/24 |
|---------------------------------|----------------|----------------|----------------|----------------|
| Systeembeheer | 192.168.0.0/24 | 192.168.0.0/24 | 192.168.0.0/25 | 192.168.0.0/26 |
| vMotion | 192.168.1.0/24 | 192.168.1.0/25 | 192.168.0.128/26 | 192.168.0.64/27 |
| vSAN | 192.168.2.0/24 | 192.168.1.128/25 | 192.168.0.192/26 | 192.168.0.96/27 |
| NSX-T Host Transport | 192.168.4.0/23 | 192.168.2.0/24 | 192.168.1.0/25 | 192.168.0.128/26 |
| NSX-T Edge Transport | 192.168.7.208/28 | 192.168.3.208/28 | 192.168.1.208/28 | 192.168.0.208/28 |
| NSX-T Edge Uplink1 | 192.168.7.224/28 | 192.168.3.224/28 | 192.168.1.224/28 | 192.168.0.224/28 |
| NSX-T Edge uplink2 | 192.168.7.240/28 | 192.168.3.240/28 | 192.168.1.240/28 | 192.168.0.240/28 |

## <a name="next-steps"></a>Volgende stappen

* [VLAN's en subnetten maken en beheren](https://docs.azure.cloudsimple.com/create-vlan-subnet/)