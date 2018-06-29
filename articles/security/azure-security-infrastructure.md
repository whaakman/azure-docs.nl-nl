---
title: Beveiliging van de Azure-infrastructuur | Microsoft Docs
description: Dit artikel wordt beschreven hoe Microsoft zorgt voor de beveiliging van ons Azure-datacenters.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 313fbc0fea317e8888bf64e7f7817ab0e5c9f049
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102327"
---
# <a name="security-of-azure-infrastructure"></a>Beveiliging van de Azure-infrastructuur
Microsoft Azure wordt uitgevoerd in datacenters beheerd en beheerd door Microsoft. Deze geografisch verspreide datacenters voldoen aan de sleutel industrienormen, zoals ISO/IEC 27001: 2013 en NIST SP 800-53 voor beveiliging en betrouwbaarheid. De datacenters worden beheerd, bewaakt en beheerd door medewerkers van Microsoft. Beheerders heeft jarenlange ervaring in's werelds grootste online services leveren met 24 x 7 continuïteit.

Deze reeks artikelen bevat informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur. Het adres van artikelen:

- [Fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid](azure-infrastructure-availability.md)
- [Onderdelen en grenzen](azure-infrastructure-components.md)
- [Netwerkarchitectuur](azure-infrastructure-network.md)
- [Productienetwerk](azure-production-network.md)
- [SQL Database](azure-infrastructure-sql.md)
- [Bewerkingen](azure-infrastructure-operations.md)
- [Controle](azure-infrastructure-monitoring.md)
- [Integriteit](azure-infrastructure-integrity.md)
- [Gegevensbeveiliging](azure-protection-of-customer-data.md)

## <a name="shared-responsibility-model"></a>Gedeelde verantwoordelijkheid model
Het is belangrijk om te begrijpen van de deling van verantwoordelijkheid tussen u en Microsoft. On-premises eigenaar van de hele stack, maar als u naar de cloud verplaatsen bepaalde verantwoordelijkheden overbrengen naar Microsoft. De volgende verantwoordelijkheid matrix geeft de gebieden van de stack in een software als een service (SaaS), platform als een service (PaaS) en de infrastructuur als een service (IaaS) implementatie waarbij u bent zelf verantwoordelijk voor- en Microsoft is verantwoordelijk voor het.

![Gedeelde verantwoordelijkheid][1]

Taken die altijd door u, ongeacht het type implementatie behouden, zijn:

- Gegevens
- Eindpunten
- Account
- Toegangsbeheer

Zorg ervoor dat u de deling van verantwoordelijkheid tussen u en Microsoft in een SaaS PaaS en IaaS-implementatie begrijpt. Zie [gedeeld verantwoordelijkheden voor Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-security-infrastructure/responsibility-zones.png
