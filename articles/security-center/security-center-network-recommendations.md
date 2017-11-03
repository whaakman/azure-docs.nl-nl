---
title: Beveiligen van uw netwerk in Azure Security Center | Microsoft Docs
description: Dit document gaat in Azure Security Center aanbevelingen die u helpen beveiligen van uw Azure-netwerk en blijven in overeenstemming met het beveiligingsbeleid.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 00b715507a7c3a4d784b800e7bf0c700f6ea6ff1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a>Beveiligen van uw netwerk in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer het Beveiligingscentrum identificeert mogelijke beveiligingsproblemen, maakt deze aanbevelingen die u bij het proces begeleiden van het configureren van benodigde besturingselementen.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VM's), netwerken, SQL en toepassingen.

In dit artikel biedt de aanbevelingen die betrekking hebben op uw netwerk.  Netwerkcentrum aanbevelingen om de volgende generatie firewalls, Netwerkbeveiligingsgroepen en regels voor binnenkomend verkeer op configureren.  Gebruik de onderstaande tabel als referentie om te begrijpen van de aanbevelingen beschikbaar netwerk en elk biedt als u deze toe te passen.

## <a name="available-network-recommendations"></a>Aanbevelingen voor de beschikbare netwerken
| Aanbeveling | Beschrijving |
| --- | --- |
| [Een firewall van de volgende generatie toevoegen](security-center-add-next-generation-firewall.md) |Raadt aan dat u de volgende generatie Firewall (NGFW) uit een Microsoft-partner toevoegt te verhogen uw beveiligingsinstellingen. |
| [Verkeer alleen via NGFW sturen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Raadt aan dat u configureert (NSG) netwerkbeveiligingsgroepen die binnenkomend verkeer naar uw virtuele machine via uw NGFW forceren. |
| [Netwerkbeveiligingsgroepen op subnetten of virtuele machines inschakelen](security-center-enable-network-security-groups.md) |Raadt het nsg's op subnetten of VM's in te schakelen. |
| [Toegang tot en met een Internetgericht eindpunt beperken](security-center-restrict-access-through-internet-facing-endpoints.md) |Raadt aan dat u regels voor binnenkomend verkeer voor het nsg's configureren. |

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over de aanbevelingen die betrekking hebben op andere Azure-resource-typen:

* [Beveiligen van uw virtuele machines in Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Beveiligen van uw toepassingen in Azure Security Center](security-center-application-recommendations.md)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
