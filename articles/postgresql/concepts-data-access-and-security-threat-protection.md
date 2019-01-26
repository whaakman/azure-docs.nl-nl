---
title: Advanced Threat Protection - Azure Database for PostgreSQL
description: Advanced Threat Protection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: b01238b0ebfe07b605824a75301165e477bd05e9
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910709"
---
# <a name="azure-database-for-postgresql-advanced-threat-protection"></a>Azure Database for PostgreSQL Advanced Threat Protection

Advanced Threat Protection for Azure Database for PostgreSQL detecteert vreemde activiteiten die duiden op ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot databases of om deze aan te vallen.

Threat Protection maakt deel uit van de aanbieding geavanceerde Threat Protection (ATP), dit een geïntegreerde-pakket voor geavanceerde beveiligingsmogelijkheden is. Advanced Threat Protection kan worden geopend en beheerd de [Azure-portal](https://portal.azure.com) en is momenteel in preview.

> [!NOTE]
> De functie Advanced Threat Protection is **niet** beschikbaar in de volgende Azure government en onafhankelijke cloud-regio's: VS (overheid)-Texas, VS (overheid)-Arizona, VS (overheid)-Iowa, VS (overheid)-Virginia, US DoD-Oost, VS DoD-centraal, Duitsland-centraal, Noord Duitsland, China-Oost, China-Oost 2. Ga naar [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/) voor algemene beschikbaarheid.
>

> [!NOTE]
> Deze functie is beschikbaar in alle regio's van Azure waarop Azure Database for PostgreSQL voor algemeen gebruik en geoptimaliseerd voor geheugen-servers wordt geïmplementeerd.

## <a name="what-is-advanced-threat-protection"></a>Wat is Advanced Threat Protection?

Advanced Threat Protection voor Azure Database for PostgreSQL biedt een nieuwe beveiligingslaag, waarmee klanten een om te detecteren en op mogelijke bedreigingen reageert zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers ontvangen een waarschuwing bij verdachte databaseactiviteiten, en mogelijke beveiligingsproblemen, evenals afwijkende toegang en query's patronen. Advanced Threat Protection voor Azure Database voor PostgreSQL waarschuwingen met integreert [Azure Security Center](https://azure.microsoft.com/services/security-center/), die details van verdachte activiteiten bevat en wordt aanbevolen actie voor het onderzoeken en tegenhouden. Advanced Threat Protection voor Azure Database for PostgreSQL is het eenvoudig om potentiële bedreigingen voor de database zonder de noodzaak om te worden van een beveiligingsexpert of bewakingssystemen geavanceerde beveiliging te beheren. 

![Advanced Threat Protection Concept](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Geavanceerde waarschuwingen van Threat Protection 
Advanced Threat Protection voor Azure Database for PostgreSQL detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven en deze kan de volgende waarschuwing is geactiveerd:
- **Toegang vanaf ongebruikelijke locatie**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de Azure Database for PostgreSQL-server, waarbij iemand zich heeft aangemeld met de Azure Database for PostgreSQL-server vanaf een ongebruikelijke geografische locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijk Azure-datacentrum**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de Azure Database for PostgreSQL-server, waarbij iemand heeft aangemeld bij de server vanaf een ongebruikelijk Azure-datacenter die tijdens de recente periode is gedetecteerd op deze server. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI, Azure Database voor PostgreSQL Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijke**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de Azure Database for PostgreSQL-server, waarbij iemand zich heeft aangemeld bij de server met een ongebruikelijke klant (Azure Database voor PostgreSQL-gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang vanaf een mogelijk schadelijke toepassing**: Deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
- **Brute force Azure Database voor PostgreSQL-referenties**: Deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Zie voor meer informatie over prijzen voor de [Azure Database for PostgreSQL-prijzen-pagina](https://azure.microsoft.com/pricing/details/postgresql/) 
* Configureer [Azure Database for PostgreSQL Advanced Threat Protection](howto-database-threat-protection-portal.md) met behulp van de Azure portal  
