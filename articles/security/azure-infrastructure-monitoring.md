---
title: Bewaking van Azure-infrastructuur
description: Dit artikel wordt de bewaking van de Azure-productienetwerk.
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
ms.openlocfilehash: 17e7183ff56725462dc43cba21db418a86d86b51
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102340"
---
# <a name="monitoring-of-azure-infrastructure"></a>Bewaking van Azure-infrastructuur   

## <a name="configuration-and-change-management"></a>Configuratie-en wijzigingsbeheer
Microsoft Azure controleert en configuratie-instellingen en basislijnconfiguraties van hardware, software en netwerkapparaten jaarlijks bijgewerkt. Wijzigingen zijn ontwikkeld, getest en goedgekeurd voordat het invoeren van de productie-omgeving van een omgeving voor ontwikkeling en/of de test.

De basislijnconfiguraties vereist voor op basis van een Azure-services wordt gecontroleerd door het team van Azure-beveiliging en naleving en serviceteams. Revisie van een serviceteam is onderdeel van de testen voorafgaand aan de implementatie van de productieservice.

## <a name="vulnerability-management"></a>Beveiligingslek management
Beveiligingsbeheer update beschermt systemen van bekende beveiligingsproblemen. Azure maakt gebruik van geïntegreerde implementatie systemen voor het beheren van de distributie en het installeren van beveiligingsupdates voor Microsoft-software. Azure kan ook beschikken over de middelen van de Microsoft Security Response Center (MSRC). MSRC identificeert, bewaakt reageert op en wordt omgezet beveiligingsincidenten en cloud servicepakket beveiligingsproblemen, elke dag van het jaar.

## <a name="vulnerability-scanning"></a>Beveiligingslek scannen
Beveiligingslek scannen wordt uitgevoerd op server-besturingssystemen, databases en netwerkapparaten. De scans op zwakke plekken worden per kwartaal minimaal uitgevoerd. Microsoft Azure-opdrachten met onafhankelijke beoordelaars binnendringen testen van de Microsoft Azure-grens. Rood Team oefeningen ook regelmatig worden uitgevoerd en resultaten gebruikt om de verbeterde beveiliging.

## <a name="protective-monitoring"></a>Beschermende bewaking
Microsoft Azure-beveiliging is vereisten voor het bewaken van actieve gedefinieerd. Service-teams configureren active controleprogramma's in overeenstemming met deze vereisten. Actieve controleprogramma's omvatten de Monitoring Agent (MA) en System Center Operations Manager. Deze hulpprogramma's zijn geconfigureerd voor het bieden van waarschuwingen van de tijd aan medewerkers van Microsoft Azure-beveiliging in situaties die directe actie is vereist.

## <a name="incident-management"></a>Incidentbeheer
Microsoft implementeert een beveiligingsproces incidentbeheer vergemakkelijkt een gecoördineerde reactie op incidenten, moet een optreden.

Microsoft neemt het volgende als u Microsoft bewust van toegang door onbevoegden tot klantgegevens die zijn opgeslagen op de apparatuur of in de opslagruimten of niet-geautoriseerde toegang tot dergelijke apparatuur of faciliteiten leidt tot gegevensverlies, vrijgeven of wijziging van klantgegevens, acties:

- Zo spoedig mogelijk informeert de klant van het beveiligingsincident
- Zo spoedig mogelijk onderzoekt het veiligheidsincident en de klant biedt gedetailleerde informatie over het veiligheidsincident
- Onderneemt redelijke en vragen stappen om de effecten en een schade ten gevolge van het veiligheidsincident te minimaliseren.

Een framework incidentbeheer is ingesteld met gedefinieerd-rollen en verantwoordelijkheden toegewezen. Het team van Windows Azure Security Incident Management (WASIM) is verantwoordelijk voor het beheer van beveiligingsincidenten, inclusief escalatie en de betrokkenheid van gespecialiseerde teams indien nodig. Azure-Managers die zijn verantwoordelijk voor onderzoek en omzetten van beveiliging en privacy incidenten overzien.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)
