---
title: bestand opnemen
description: bestand opnemen
services: networking
author: anavinahar
ms.service: networking
ms.topic: include
ms.date: 06/13/2019
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: bb13ecb2d9014dbf56823734ac28703df9755b4b
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277205"
---
<a name="azure-resource-manager-virtual-networking-limits"></a>Netwerklimieten - Azure Resource Manager de volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via **Azure Resource Manager** per regio per abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> We alle standaardlimieten om de maximale limieten voor het laatst verhoogd. Als er geen kolom maximumlimiet is, is de resource geen instelbare limieten. Als u deze limieten verhogen door de ondersteuning in het verleden had en bijgewerkte limieten in de volgende tabellen ziet [opent u een ondersteuningsaanvraag online klant gratis](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Resource | Standaard-/ maximumlimiet | 
| --- | --- |
| Virtuele netwerken |1000 |
| Subnetten per virtueel netwerk |3,000 |
| Virtueel netwerk-peerings per virtueel netwerk |500 |
| DNS-servers per virtueel netwerk |20 |
| Privé-IP-adressen per virtueel netwerk |65,536 |
| Privé-IP-adressen per netwerkinterface |256 |
| Privé-IP-adressen per virtuele machine |256 |
| Gelijktijdige TCP of UDP-stromen per NIC van een virtuele machine of rolinstantie |500,000 |
| Netwerkinterfacekaarten |65,536 |
| Netwerkbeveiligingsgroepen |5,000 |
| NSG-regels per NSG |1000 |
| IP-adressen en -bereiken die zijn opgegeven voor de bron- of doelserver in een beveiligingsgroep |4,000 |
| Toepassingsbeveiligingsgroepen |3,000 |
| Toepassingsbeveiligingsgroepen per IP-configuratie, per NIC |20 |
| IP-configuraties per toepassingsbeveiligingsgroep |4,000 |
| Toepassingsbeveiligingsgroepen die kunnen worden opgegeven in alle beveiligingsregels van een netwerkbeveiligingsgroep |100 |
| Gebruiker gedefinieerde routetabellen |200 |
| Gebruiker gedefinieerde routes per routetabel |400 |
| Punt-naar-site-basiscertificaten per Azure VPN-Gateway |20 |
| Virtueel netwerk op tikt |100 |
| Tik op netwerkinterfaceconfiguraties per virtueel netwerk-TAP |100 |

#### <a name="publicip-address"></a>Limieten voor openbare IP-adres
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Openbare IP-adressen - dynamisch | 1000 voor Basic. |Neem contact op met ondersteuning. |
| Openbare IP-adressen - statisch | 1000 voor Basic. |Neem contact op met ondersteuning. |
| Openbare IP-adressen - statisch | 200 voor Standard.|Neem contact op met ondersteuning. |
| Openbare IP-voorvoegsel | /28 | Neem contact op met ondersteuning. |

#### <a name="load-balancer"></a>Load balancer-limieten
De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via Azure Resource Manager. De beperkingen gelden per regio en per abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

| Resource | Standaard-/ maximumlimiet |
| --- | --- |
| Load balancers | 1000 | 
| Regels per resource, Basic | 250 |
| Regels per resource, Standard | 1,500 | 
| Regels per IP-configuratie | 299 |
| Regels per NIC | 300 |
| Front-end-IP-configuraties, Basic | 200 |
| Front-end-IP-configuraties, Standard | 600 |
| Back-end-pool, Basic | 100, één beschikbaarheidsset |
| Back-end-pool, Standard | 1000, één virtueel netwerk |
| Back-end-resources per load balancer Standard<sup>1</sup> | 150 |
| Poorten voor hoge beschikbaarheid, Standard | 1 per interne front-end |

<sup>1</sup>de limiet is maximaal 150 bronnen in elke combinatie van resources voor zelfstandige virtuele machine, beschikbaarheidsset resources en resources voor VM-schaalset.

#### <a name="virtual-networking-limits-classic"></a>De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via de **klassieke** implementatiemodel voor elk abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Virtuele netwerken |50 |100 |
| Lokale netwerksites |20 |Neem contact op met ondersteuning. |
| DNS-servers per virtueel netwerk |20 |20 |
| Privé-IP-adressen per virtueel netwerk |4,096 |4,096 |
| Gelijktijdige TCP of UDP-stromen per NIC van een virtuele machine of rolinstantie |500.000 maximaal 1.000.000 voor twee of meer NIC's. |500.000 maximaal 1.000.000 voor twee of meer NIC's. |
| Netwerkbeveiligingsgroepen (nsg's) |200 |200 |
| NSG-regels per NSG |1000 |1000 |
| Gebruiker gedefinieerde routetabellen |200 |200 |
| Gebruiker gedefinieerde routes per routetabel |400 |400 |
| Openbare IP-adressen (dynamisch) |5 |Neem contact op met ondersteuning |
| Gereserveerde openbare IP-adressen |20 |Neem contact op met ondersteuning |
| Openbare VIP per implementatie |5 |Neem contact op met ondersteuning |
| VIP-privénetwerk (interne taakverdeling) per implementatie |1 |1 |
| Eindpunt-toegangsbeheerlijsten (ACL's) |50 |50 |
