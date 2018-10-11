---
title: Advanced Threat Protection - Azure Database voor PostgreSQL | Microsoft Docs
description: Advanced Threat Protection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database.
services: postgresql
author: bolzmj
manager: kfile
ms.service: postgresql
ms.topic: article
ms.date: 09/20/2018
ms.author: mbolz
ms.openlocfilehash: 392947919813753dc57422957029e961a037a616
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49066935"
---
# <a name="azure-database-for-postgresql-advanced-threat-protection"></a>Azure Database for PostgreSQL Advanced Threat Protection

Advanced Threat Protection for Azure Database for PostgreSQL detecteert vreemde activiteiten die duiden op ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot databases of om deze aan te vallen.

Threat Protection maakt deel uit van de aanbieding geavanceerde Threat Protection (ATP), dit een geïntegreerde-pakket voor geavanceerde beveiligingsmogelijkheden is. Advanced Threat Protection kan worden geopend en beheerd de [Azure-portal](https://portal.azure.com) en is momenteel in preview.

> [!NOTE]
> De functie Advanced Threat Protection is **niet** beschikbaar in de volgende Azure government en onafhankelijke cloud-regio's: VS (overheid)-Texas, VS (overheid)-Arizona, VS (overheid)-Iowa, VS (overheid) Virginia, VS DoD Oost, VS DoD-centraal, Duitsland-centraal, Duitsland Noord, China-Oost, China-Oost 2. Ga naar [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/) voor algemene beschikbaarheid.
>

## <a name="what-is-advanced-threat-protection"></a>Wat is Advanced Threat Protection?

Advanced Threat Protection voor Azure Database for PostgreSQL biedt een nieuwe beveiligingslaag, waarmee klanten een om te detecteren en op mogelijke bedreigingen reageert zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers ontvangen een waarschuwing bij verdachte databaseactiviteiten, en mogelijke beveiligingsproblemen, evenals afwijkende toegang en query's patronen. Advanced Threat Protection voor Azure Database voor PostgreSQL waarschuwingen met integreert [Azure Security Center](https://azure.microsoft.com/services/security-center/), die details van verdachte activiteiten bevat en wordt aanbevolen actie voor het onderzoeken en tegenhouden. Advanced Threat Protection voor Azure Database for PostgreSQL is het eenvoudig om potentiële bedreigingen voor de database zonder de noodzaak om te worden van een beveiligingsexpert of bewakingssystemen geavanceerde beveiliging te beheren. 

![Advanced Threat Protection Concept](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Geavanceerde waarschuwingen van Threat Protection 
Advanced Threat Protection voor Azure Database for PostgreSQL detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven en deze kan de volgende waarschuwing is geactiveerd:
- **Toegang vanaf ongebruikelijke locatie**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de Azure Database for PostgreSQL-server, waarbij iemand zich heeft aangemeld met de Azure Database for PostgreSQL-server uit een ongebruikelijke geografische de locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijk Azure-datacentrum**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de Azure Database for PostgreSQL-server, waarbij iemand heeft aangemeld bij de server vanaf een ongebruikelijk Azure-datacenter dat is gedetecteerd op deze de server tijdens de recente periode. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI, Azure Database voor PostgreSQL Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijke**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de Azure Database for PostgreSQL-server, waarbij iemand zich heeft aangemeld bij de server met een ongebruikelijke klant (Azure Database for PostgreSQL de gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang tot een toepassing die mogelijk schadelijk is**: deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
- **Brute force Azure Database voor PostgreSQL-referenties**: deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Zie voor meer informatie over prijzen voor de [Azure Database for PostgreSQL-prijzen-pagina](https://azure.microsoft.com/pricing/details/postgresql/) 
* Configureer [Azure Database for PostgreSQL Advanced Threat Protection](howto-database-threat-protection-portal.md) met behulp van de Azure portal  
