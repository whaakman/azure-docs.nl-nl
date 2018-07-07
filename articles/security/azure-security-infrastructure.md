---
title: Beveiliging van de Azure-infrastructuur | Microsoft Docs
description: Het artikel wordt beschreven hoe Microsoft zorgt voor beveiliging van onze Azure-datacenters.
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
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 397bd1f904b676a6ba020ec78fb1cad05c460be1
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903853"
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
Het is belangrijk om te begrijpen van de verdeling van de verantwoordelijkheid tussen u en Microsoft. On-premises eigenaar van de hele stack, maar wanneer u naar de cloud bepaalde verantwoordelijkheden overbrengen naar Microsoft. Verantwoordelijkheid in de volgende matrix bevat de gebieden van de stack in een software als een service (SaaS), platform as a service (PaaS) en de infrastructuur als een dienst (IaaS) implementatie van apps waarmee u bent verantwoordelijk voor en Microsoft is verantwoordelijk voor het.

![Gedeelde verantwoordelijkheid][1]

Taken die altijd door u, ongeacht het type van de implementatie behouden, zijn:

- Gegevens
- Eindpunten
- Account
- Toegangsbeheer

Zorg ervoor dat u bekend bent met de verdeling van de verantwoordelijkheid tussen u en Microsoft in een implementatie SaaS, PaaS en IaaS. Zie [gedeelde verantwoordelijkheden voor Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Microsoft Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van klantgegevens in Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-security-infrastructure/responsibility-zones.png
