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
ms.openlocfilehash: c6c57390e0a2fba0c79d3198df0f5577eb813f88
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553397"
---
<a name="virtual-networking-limits-classic"></a>De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via het klassieke implementatiemodel voor elk abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Virtuele netwerken |50 |100 |
| Lokale netwerksites |20 |Neem contact op met ondersteuning. |
| DNS-servers per virtueel netwerk |20 |20 |
| Privé-IP-adressen per virtueel netwerk |4,096 |4,096 |
| Gelijktijdige TCP of UDP-stromen per NIC van een virtuele machine of rolinstantie |500.000 maximaal 1.000.000 voor twee of meer NIC's. |500.000 maximaal 1.000.000 voor twee of meer NIC's. |
| Netwerkbeveiligingsgroepen (nsg's) |100 |200 |
| NSG-regels per NSG |200 |1000 |
| Gebruiker gedefinieerde routetabellen |100 |200 |
| Gebruiker gedefinieerde routes per routetabel |100 |400 |
| Openbare IP-adressen (dynamisch) |5 |Neem contact op met ondersteuning. |
| Gereserveerde openbare IP-adressen |20 |Neem contact op met ondersteuning. |
| Openbare VIP per implementatie |5 |Neem contact op met ondersteuning. |
| VIP-privénetwerk (interne taakverdeling) per implementatie |1 |1 |
| Eindpunt-toegangsbeheerlijsten (ACL's) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Netwerklimieten - Azure Resource Manager
De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via Azure Resource Manager. De beperkingen gelden per regio en per abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> We alle standaardlimieten om de maximale limieten voor het laatst verhoogd. Als er geen kolom maximumlimiet is, is de resource geen instelbare limieten. Als u deze limieten verhogen door de ondersteuning in het verleden had en bijgewerkte limieten in de volgende tabellen ziet [opent u een ondersteuningsaanvraag online klant gratis](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Resource | Standaardlimiet | 
| --- | --- |
| Virtuele netwerken |1000 |
| Subnetten per virtueel netwerk |3.000 |
| Virtueel netwerk-peerings per virtueel netwerk |100 |
| DNS-servers per virtueel netwerk |20 |
| Privé-IP-adressen per virtueel netwerk |65,536 |
| Privé-IP-adressen per netwerkinterface |256 |
| Privé-IP-adressen per virtuele machine |256 |
| Gelijktijdige TCP of UDP-stromen per NIC van een virtuele machine of rolinstantie |500,000 |
| Netwerkadapters |65,536 |
| Netwerkbeveiligingsgroepen |5.000 |
| NSG-regels per NSG |1000 |
| IP-adressen en -bereiken die zijn opgegeven voor de bron- of doelserver in een beveiligingsgroep |4,000 |
| Toepassingsbeveiligingsgroepen |3.000 |
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
| Openbare IP-adressen - statisch | 200 voor Basic. |Neem contact op met ondersteuning. |
| Openbare IP-adressen - statisch | 200 voor Standard.|Neem contact op met ondersteuning. |
| Openbare IP-voorvoegsel grootte (preview) | /28 | /28 |

#### <a name="load-balancer"></a>Load balancer-limieten
De volgende beperkingen gelden alleen voor netwerkresources die worden beheerd via Azure Resource Manager. De beperkingen gelden per regio en per abonnement. Meer informatie over het [weergeven van uw huidige brongebruik op basis van uw abonnementen](../articles/networking/check-usage-against-limits.md).

| Resource | Standaardlimiet |
| --- | --- |
| Load balancers | 1000 | 
| Regels per resource, Basic | 250 |
| Regels per resource, Standard | 1,500 | 
| Regels per IP-configuratie | 299 |
| Regels per NIC | 500 |
| Front-end-IP-configuraties, Basic | 200 |
| Front-end-IP-configuraties, Standard | 600 |
| Back-end-pool, Basic | 100, één beschikbaarheidsset |
| Back-end-pool, Standard | 1000, één virtueel netwerk |
| Back-end-resources per load balancer Standard<sup>1</sup> | 150 |
| Poorten voor hoge beschikbaarheid, Standard | 1 per interne front-end |

<sup>1</sup>de limiet is maximaal 150 bronnen in elke combinatie van resources voor zelfstandige virtuele machine, beschikbaarheidsset resources en resources voor VM-schaalset.

