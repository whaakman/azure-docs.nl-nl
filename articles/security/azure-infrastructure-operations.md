---
title: Azure productie-uitvoering en beheer
description: In dit artikel biedt een algemene beschrijving van het beheer en de werking van de Azure-productienetwerk.
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
ms.openlocfilehash: dc389f5f5c155555deb860f041b15b0ea49ee416
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102331"
---
# <a name="azure-production-operations-and-management"></a>Azure productie-uitvoering en beheer    
Beheer en de werking van het Azure-productienetwerk is een gecoördineerde inspanning tussen de operations-teams van Azure en Azure SQL Database. Verschillende systeem- en hulpprogramma's voor prestatiecontrole worden gebruikt in de omgeving. Netwerkapparaten, servers, services en processen van toepassingen worden bewaakt met de juiste hulpprogramma's.

Meerdere niveaus van bewaking, logboekregistratie en rapportage zijn geïmplementeerd om beveiligde uitvoering van de services die worden uitgevoerd in de Microsoft Azure-omgeving, inclusief de volgende acties:

- Microsoft Azure Monitoring Agent (MA) is voornamelijk om controle- en diagnostische logboekgegevens moeten worden verzameld uit veel plaatsen, met inbegrip van de FC en de hoofdmap OS en schrijft deze logboekbestanden. Uiteindelijk duwt deze een ontsloten subset van de informatie in een vooraf geconfigureerde Azure Storage-Account. De controle en diagnostische Service (MDS) is bovendien een zelfstandige service die verschillende bewakings- en diagnostische logboekgegevens leest en bevat een overzicht van de gegevens. MDS schrijft de informatie in een geïntegreerde logboek. Azure maakt gebruik van de op maat gemaakte Azure Security Monitoring (ASM), dit is een uitbreiding van het Azure-bewakingssysteem. Onderdelen die u ziet, analyseren en rapporteren over beveiliging relevante gebeurtenissen van verschillende punten in het platform heeft.
- Microsoft Azure SQL Database WinFabric-platform biedt management, implementatie, ontwikkeling en operationele toezicht services voor Microsoft Azure SQL Database. Gedistribueerde implementatie met meerdere stappen services, statuscontrole, automatische reparaties en naleving voor service-versie biedt. Bevat de volgende services:

   - Service modeling mogelijkheden met hoogwaardige ontwikkelomgeving (datacenter clusters zijn goedkoper en weinig).
   - Implementatie met één muisklik en upgrade werkstromen voor service bootstrap en onderhoud.
   - De statusrapportage met geautomatiseerde reparatie werkstromen zelfreparerende inschakelen.
   - Real-time bewaking, waarschuwingen en foutopsporing van opslagruimten op de knooppunten van een gedistribueerde systeem.
   - Gecentraliseerde verzameling van operationele gegevens en metrische gegevens voor gedistribueerde hoofdmap analyse- en inzicht veroorzaken.
   - Operationele tooling voor implementatie, wijzigingsbeheer en toezicht.
   - Microsoft Azure SQL Database WinFabric platform en watchdog scripts continu uitvoeren en bewaken in realtime.

Als een afwijkingen optreedt, wordt het Incident Response-proces gevolgd door het team van Azure Incident selectie is geactiveerd. De juiste Azure ondersteuningspersoneel om te reageren op het incident is gewaarschuwd. Bijhouden en de oplossing worden beschreven en beheerd in een gecentraliseerde ticketsysteem gebruikt. Systeem bedrijfstijd metrische gegevens zijn beschikbaar onder de openbaarmaking van Non-overeenkomst (geheimhoudingsverklaring) en op verzoek.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Bedrijfsnetwerk en multifactor Access naar productie
De gebruikersgroep bedrijfsnetwerk omvat ondersteuningspersoneel van Microsoft Azure. Het bedrijfsnetwerk biedt ondersteuning voor interne zakelijke functies en toegang tot interne toepassingen die worden gebruikt voor Azure klantondersteuning bevat. Het bedrijfsnetwerk is logisch en fysiek gescheiden van het Azure-productienetwerk. Microsoft Azure medewerkers toegang tot het bedrijfsnetwerk met behulp van Microsoft Azure-werkstations en laptops. Een Active Directory (AD)-account, inclusief een gebruikersnaam en wachtwoord voor toegang tot zakelijke netwerkbronnen, moeten alle gebruikers hebben. Toegang tot het bedrijfsnetwerk maakt gebruik van AD-accounts die zijn verleend aan alle medewerkers van Microsoft, aannemers, leveranciers en beheerd door MSIT. Unieke gebruikers-id's onderscheiden op basis van hun werkstatus bij Microsoft personeel.

Toegang tot interne Azure-toepassingen wordt gecontroleerd door middel van verificatie met Active Directory Federation Services (ADFS). AD FS is een service die wordt gehost door MSIT waarmee de verificatie van gebruikers via het toepassen van een beveiligde token en de gebruiker van de claimprovider CorpNet. AD FS kan interne Microsoft Azure-toepassingen om te verifiëren van gebruikers op basis van het Microsoft zakelijke AD-domein. Voor toegang tot het productienetwerk CorpNet-omgeving, moet de gebruiker verifiëren met multi-factor authentication.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)
