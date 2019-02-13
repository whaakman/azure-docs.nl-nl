---
title: Azure-infrastructuur bewaken
description: Dit artikel worden de bewaking van de Azure-productienetwerk.
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
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 88bc76116392140c533f67402642c68d714227c0
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56108209"
---
# <a name="azure-infrastructure-monitoring"></a>Azure-infrastructuur bewaken   

## <a name="configuration-and-change-management"></a>Configuratie-en wijzigingsbeheer
Azure controleert en configuratie-instellingen en basislijnconfiguraties van hardware, software en netwerkapparaten jaarlijks worden bijgewerkt. Wijzigingen worden ontwikkeld, getest en goedgekeurd voordat de productie-omgeving van een omgeving voor ontwikkeling en/of testen.

De basislijnconfiguraties die vereist voor services op basis van Azure zijn worden gecontroleerd door het team van Azure beveiliging en naleving en service-teams. Een overzicht van service-team maakt deel uit van de tests die wordt weergegeven vóór de implementatie van de productieservice.

## <a name="vulnerability-management"></a>Beveiligingsproblemen
Update van beveiligingsbeheer beschermt tegen bekende beveiligingsproblemen systemen aan. Azure maakt gebruik van geïntegreerde implementatiesystemen beheren de distributie en installatie van de security updates voor Microsoft-software. Azure is ook mogelijk om het tekenen op de resources van de Microsoft Security Response Center (MSRC). Het MSRC identificeert, bewaakt, reageert op en wordt omgezet van beveiligingsincidenten en cloud nacht beveiligingsproblemen, elke dag van het jaar.

## <a name="vulnerability-scanning"></a>Scannen van beveiligingsproblemen
Scannen op beveiligingsproblemen is uitgevoerd op server-besturingssystemen, databases en netwerkapparaten. De scant beveiligingsproblemen worden uitgevoerd op kwartaalbasis minimaal. Azure contracten met onafhankelijke beoordelaars om uit te voeren indringingstests van de grens van Azure. Red team oefeningen ook regelmatig worden uitgevoerd en de resultaten worden gebruikt om verbeteringen in de beveiliging.

## <a name="protective-monitoring"></a>Beschermende controleren
Azure-beveiliging heeft vereisten voor het bewaken van actieve gedefinieerd. Service-teams configureren active controleprogramma's in overeenstemming met deze vereisten. Actieve controleprogramma's omvatten de Microsoft Monitoring Agent (MMA) en System Center Operations Manager. Deze hulpprogramma's zijn geconfigureerd voor waarschuwingen van tijd tot Azure-beveiliging-personeel in situaties die directe actie is vereist.

## <a name="incident-management"></a>incidentbeheer
Microsoft implementeert een security incident management-proces een gecoördineerde reactie op incidenten, in het kader moet een optreden.

Als Microsoft op de hoogte van niet-geautoriseerde toegang tot klantgegevens die zijn opgeslagen op de apparatuur of in de faciliteiten, of deze op de hoogte van niet-geautoriseerde toegang tot dergelijke apparatuur en faciliteiten wordt, wat resulteert in gegevensverlies, openbaarmaking of wijziging van de gegevens van de klant, Microsoft voert de volgende acties uit:

- Onmiddellijk informeert de klant van het beveiligingsincident.
- Snel het beveiligingsincident onderzoekt het probleem en biedt klanten gedetailleerde informatie over het beveiligingsincident.
- Neemt redelijke en vragen stappen voor het de effecten van en het resultaat zijn van het beveiligingsincident schade te minimaliseren.

Een incident management-framework is vastgesteld dat definieert rollen en verantwoordelijkheden worden toegewezen. Het Azure security incident management-team is verantwoordelijk voor het beheren van beveiligingsincidenten, met inbegrip van escalatie en ervoor te zorgen dat de betrokkenheid van gespecialiseerde teams wanneer dat nodig. Azure operations managers zijn die verantwoordelijk is voor het beheren van het onderzoek en de resolutie van beveiliging en privacy incidenten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)
