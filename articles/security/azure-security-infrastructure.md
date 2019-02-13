---
title: Beveiliging van de Azure-infrastructuur | Microsoft Docs
description: Het artikel wordt beschreven hoe Microsoft werkt voor het beveiligen van onze Azure-datacenters.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: dc9b4db37e811d8bac6df2d532fd3629d98c9650
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104761"
---
# <a name="azure-infrastructure-security"></a>Beveiliging van de Azure-infrastructuur
Microsoft Azure wordt uitgevoerd in datacenters beheerd en worden beheerd door Microsoft. Deze geografisch verspreide datacenters voldoen aan de belangrijkste industrienormen, zoals ISO/IEC 27001: 2013 en NIST SP 800-53, voor beveiliging en betrouwbaarheid. De datacenters worden beheerd, gecontroleerd en beheerd door Microsoft-personeel. Het operationele team heeft jaar ervaring in het leveren van de grootste online services met 24 x 7 bedrijfscontinuïteit te waarborgen.

Deze reeks artikelen bevat informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur. Het adres artikelen:

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

## <a name="shared-responsibility-model"></a>Model met gedeelde verantwoordelijkheid
Het is belangrijk om te begrijpen van de verdeling van de verantwoordelijkheid tussen u en Microsoft. On-premises eigenaar van de hele stack, maar als u naar de cloud verplaatst, bepaalde verantwoordelijkheden overdraagt aan Microsoft. De volgende afbeelding ziet u de verantwoordelijkheidsgebieden, op basis van het type van de implementatie van uw stack (software als een service [SaaS], platform als een service [PaaS], infrastructure-as-a-service [IaaS] en on-premises).

![Afbeelding met verantwoordelijkheden][1]

Altijd bent u verantwoordelijk voor het volgende, ongeacht het type implementatie:

- Gegevens
- Eindpunten
- Account
- Toegangsbeheer

Zorg ervoor dat u bekend bent met de verdeling van de verantwoordelijkheid tussen u en Microsoft in een implementatie SaaS, PaaS en IaaS. Zie voor meer informatie, [gedeelde verantwoordelijkheden voor cloudcomputing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet om te helpen beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-security-infrastructure/responsibility-zones.png
