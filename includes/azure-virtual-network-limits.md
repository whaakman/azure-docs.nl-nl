---
title: bestand opnemen
description: bestand opnemen
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 02/07/2019
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: fe1993a914ea4d3bd8ab9116748198cbb0c1c43c
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55904997"
---
<a name="virtual-networking-limits-classic"></a>De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via het klassieke implementatiemodel voor elk abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Virtuele netwerken |50 |100 |
| Lokale netwerksites |20 |contact met ondersteuning |
| DNS-servers per virtueel netwerk |20 |20 |
| Privé-IP-adressen per virtueel netwerk |4096 |4096 |
| Gelijktijdige TCP of UDP-stromen per NIC van een virtuele machine of rolinstantie |500.000 |500.000 |
| Netwerkbeveiligingsgroepen (NSG's) |100 |200 |
| NSG-regels per NSG |200 |1000 |
| Door de gebruiker gedefinieerde routetabellen |100 |200 |
| Door de gebruiker gedefinieerde routes per routetabel |100 |400 |
| Openbare IP-adressen (dynamisch) |5 |contact met ondersteuning |
| Gereserveerde openbare IP-adressen |20 |contact met ondersteuning |
| Openbare VIP per implementatie |5 |contact met ondersteuning |
| Privé-VIP (ILB) per implementatie |1 |1 |
| Toegangsbeheerlijsten voor eindpunt (ACL's) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Netwerklimieten - Azure Resource Manager
De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via Azure Resource Manager. De beperkingen gelden per regio en per abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> We alle standaardlimieten om de maximale limieten voor het laatst verhoogd. Als er geen **maximumlimiet** kolom en vervolgens de resource geen instelbare limieten. Als u deze limieten verhogen door de ondersteuning in het verleden hebben gehad en geen bijgewerkte limieten als hieronder ziet, kunt u [opent u een ondersteuningsaanvraag online klant gratis](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Resource | Standaardlimiet | 
| --- | --- |
| Virtuele netwerken |1000 |
| Subnetten per virtueel netwerk |3000 |
| Virtueel netwerk-peerings per Virtueelnetwerk |100 |
| DNS-servers per virtueel netwerk |20 |
| Privé-IP-adressen per virtueel netwerk |65536 |
| Privé-IP-adressen per netwerkinterface |256 |
| Privé-IP-adressen per virtuele machine |256 |
| Gelijktijdige TCP of UDP-stromen per NIC van een virtuele machine of rolinstantie |500.000 |
| Netwerkinterfaces (NIC) |65536 |
| Netwerkbeveiligingsgroepen (NSG's) |5000 |
| NSG-regels per NSG |1000 |
| IP-adressen en -bereiken die zijn opgegeven voor de bron- of doelserver in een beveiligingsgroep |4000 |
| Toepassingsbeveiligingsgroepen |3000 |
| Toepassingsbeveiligingsgroepen per IP-configuratie, per NIC |20 |
| IP-configuraties per toepassingsbeveiligingsgroep |4000 |
| Toepassingsbeveiligingsgroepen die kunnen worden opgegeven in alle beveiligingsregels van een netwerkbeveiligingsgroep |100 |
| Door de gebruiker gedefinieerde routetabellen |200 |
| Door de gebruiker gedefinieerde routes per routetabel |400 |
| Punt-naar-site-basiscertificaten per VPN Gateway |20 |
| Virtueel netwerk op tikt |100 |
| Tik op netwerkinterfaceconfiguraties per virtueel netwerk-TAP |100 |

#### <a name="publicip-address"></a>Limieten voor openbare IP-adres
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Openbare IP-adressen - dynamisch |1000 (standaard) |contact met ondersteuning |
| Openbare IP-adressen - statisch |200 (standaard) |contact met ondersteuning |
| Openbare IP-adressen - statisch |(Standard) 200 |contact met ondersteuning |
| Openbare IP-voorvoegsel grootte (preview) | /28 | /28 |

#### <a name="load-balancer"></a>Load Balancer-limieten
De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via Azure Resource Manager. De beperkingen gelden per regio en per abonnement. Meer informatie over het [uw huidige brongebruik op basis van uw abonnementen weergeven](../articles/networking/check-usage-against-limits.md)

| Resource | Standaardlimiet |
| --- | --- |
| Load balancers | 1000 | 
| Regels per resource, Basic | 250 |
| Regels per resource, Standard | 1500 | 
| Regels per IP-configuratie | 299 |
| Regels per NIC | 500 |
| Frontend-IP-configuraties, Basic | 200 |
| Frontend-IP-configuraties, Standard | 600 |
| Back-endpool, Basic | 100, één Beschikbaarheidsset |
| Back-endpool, Standard | 1000, één VNet |
| Back-endresources per Load Balancer Standard * | 150 |
| HA-poorten, Standard | 1 per interne frontend |

** Tot 150 resources, een combinatie van resources voor zelfstandige virtuele machine, beschikbaarheidssets resources en resources voor virtuele-machineschaalset.

