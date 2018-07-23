---
title: -Azure-productiebewerkingen en beheer
description: Dit artikel bevat een algemene beschrijving van het beheer en het gebruik van de Azure-productienetwerk.
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
ms.openlocfilehash: 21ae81f1d8423a9d05208ec6d8c4f31d909d2f9f
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173156"
---
# <a name="azure-production-operations-and-management"></a>-Azure-productiebewerkingen en beheer    
Het beheer en de werking van de Azure-productienetwerk is een gecoördineerde inspanningen tussen de operations-teams van Azure en Azure SQL Database. De teams gebruiken verschillende systeem- en bewaking van toepassingsprestaties-hulpprogramma's in de omgeving. En ze de juiste hulpprogramma's gebruiken voor het bewaken van netwerkapparaten, servers, services en processen van toepassingen.

De operations-teams implementeren om ervoor te zorgen de veilige uitvoering van de services die worden uitgevoerd in de Azure-omgeving, meerdere niveaus van controle, logboekregistratie en rapportage, met inbegrip van de volgende acties:

- Primair, de Microsoft Monitoring Agent (MMA) worden verzameld van bewakingsgegevens en diagnostische logboekgegevens op tal van plaatsen, met inbegrip van de infrastructuurcontroller (FC) en het root-besturingssysteem (OS) en schrijft deze logboekbestanden. De agent pushes uiteindelijk een ontsloten subset van de informatie in een vooraf geconfigureerde Azure storage-account. Bovendien zijn de zelfstandige bewaking en diagnostische service leest verschillende bewakingsgegevens en diagnostische logboekgegevens, en bevat een overzicht van de gegevens. De bewakingsgegevens en diagnostische-service schrijft de informatie in een geïntegreerde logboek. Azure maakt gebruik van de op maat gemaakte Azure-beveiliging bewaakt, dit is een uitbreiding van de Azure monitoring system. Onderdelen die observeren, analyseren en rapporteren over beveiliging relevante gebeurtenissen van verschillende punten in het platform heeft.

- De Azure SQL Database Windows Fabric-platform biedt beheer, implementatie, ontwikkeling en operationele toezicht services voor Azure SQL Database. Het platform biedt een gedistribueerde implementatie met meerdere stappen services, statuscontrole, automatische reparaties en naleving van service-versie. Het biedt de volgende services:

   - Service modelleringsmogelijkheden met hoogwaardige-ontwikkelomgeving (datacenter-clusters zijn duur en schaarse).
   - Implementatie met één klik en upgrade-werkstromen voor service-bootstrap en onderhoud.
   - De gezondheid van rapportage via herstel op geautomatiseerde werkstromen zelfherstel inschakelen.
   - Realtimebewaking, waarschuwingen en foutopsporing van Microsoft faciliteiten hebben op de knooppunten van een gedistribueerd systeem.
   - Gecentraliseerde verzameling van operationele gegevens en metrische gegevens voor gedistribueerde hoofdmap ervoor zorgen dat analyse en service inzicht.
   - Operationele verschillende hulpprogramma's voor implementatie, wijzigingsbeheer en toezicht.
   - De Azure SQL Database Windows Fabric-platform en watchdog scripts continu worden uitgevoerd en in realtime te controleren.

Als er mogelijke afwijkingen optreden, wordt het proces voor reacties op incidenten gevolgd door het team van Azure incident sorteren geactiveerd. De juiste Azure-ondersteuningspersoneel krijgt een melding om te reageren op het incident. Problemen bijhouden en de oplossing worden beschreven en beheerd in een gecentraliseerde ticketsysteem. Systeem uptime metrische gegevens zijn beschikbaar onder de non-disclosure agreement (NDA) en op aanvraag.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Bedrijfsnetwerk en multi-factor Authentication toegang tot productie
De gebruikersgroep bedrijfsnetwerk bevat Azure-ondersteuningspersoneel. Het bedrijfsnetwerk biedt ondersteuning voor interne zakelijke functies en omvat toegang tot interne toepassingen die worden gebruikt voor ondersteuning van Azure-klant. Het bedrijfsnetwerk is logisch en fysiek gescheiden van de Azure-productienetwerk. Azure medewerkers toegang hebben tot het bedrijfsnetwerk met behulp van Azure voor werkstations en laptops. Azure Active Directory (Azure AD)-account, met inbegrip van een gebruikersnaam en wachtwoord voor toegang tot zakelijke netwerkbronnen, moeten alle gebruikers hebben. Toegang tot het bedrijfsnetwerk maakt gebruik van Azure AD-accounts die zijn verleend aan alle Microsoft-personeel, aannemers en leveranciers en beheerd door Microsoft Information Technology. Unieke gebruikers-id onderscheid tussen personeel op basis van hun dienstverbandstatus bij Microsoft.

Toegang tot interne Azure-toepassingen wordt geregeld via verificatie op Active Directory Federation Services (AD FS). AD FS is een service die wordt gehost door Microsoft Information Technology waarmee de verificatie van gebruikers via het toepassen van een beveiligde token en gebruiker claims bedrijfsnetwerk bevinden. AD FS kan interne Azure-toepassingen om te verifiëren van gebruikers aan de hand van de Microsoft zakelijke active directory-domein. Voor toegang tot het productienetwerk uit de omgeving van het bedrijfsnetwerk bevinden, moeten gebruikers worden geverifieerd met behulp van meervoudige verificatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)
