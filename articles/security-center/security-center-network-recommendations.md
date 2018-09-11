---
title: Beveiligen van uw netwerk in Azure Security Center | Microsoft Docs
description: In dit document adressen aanbevelingen in Azure Security Center die u helpen bij uw Azure-netwerk te beveiligen en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 12c00d6dfac6c9c2a377a8c142118ff6fd0af751
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302269"
---
# <a name="protecting-your-network-in-azure-security-center"></a>Beveiligen van uw netwerk in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen die u bij het proces begeleiden van het configureren van de benodigde besturingselementen.  Aanbevelingen zijn van toepassing op Azure-resource-typen: virtuele machines (VM's), netwerk-, SQL en toepassingen.

In dit artikel biedt aanbevelingen die betrekking hebben op uw netwerk.  Netwerkcentrum aanbevelingen om de volgende generation-firewalls, Netwerkbeveiligingsgroepen en regels voor binnenkomend verkeer op configureren.  Gebruik de onderstaande tabel als referentie om te begrijpen van de aanbevelingen voor de beschikbare netwerken en wat elke doet als u deze toepast.

## <a name="available-network-recommendations"></a>Aanbevelingen voor netwerken beschikbaar
| Aanbeveling | Beschrijving |
| --- | --- |
| [Een firewall van de volgende generatie toevoegen](security-center-add-next-generation-firewall.md) |Hiermee wordt aanbevolen dat u een volgende generatie Firewall (NGFW) toevoegen van een Microsoft-partner uw beveiligingen verhogen. |
| [Verkeer alleen via NGFW sturen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Hiermee wordt aanbevolen dat u configureert (NSG) regels voor netwerkbeveiligingsgroepen die binnenkomend verkeer op uw virtuele machine via de NGFW afgedwongen. |
| [Netwerkbeveiligingsgroepen inschakelen op subnetten of virtuele machines](security-center-enable-network-security-groups.md) |Hiermee wordt aanbevolen dat u nsg's op subnetten of virtuele machines inschakelen. |
| [Toegang via een Internetgericht eindpunt beperken](security-center-restrict-access-through-internet-facing-endpoints.md) |Hiermee wordt aanbevolen dat u regels voor binnenkomend verkeer voor nsg's configureren. |

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Protecting your virtual machines in Azure Security Center](security-center-virtual-machine-recommendations.md) (Uw virtuele machines beveiligen in Azure Security Center)
* [Protecting your applications in Azure Security Center](security-center-application-recommendations.md) (Uw toepassingen beveiligen in Azure Security Center)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
