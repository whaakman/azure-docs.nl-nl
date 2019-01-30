---
title: Detectie van bedreigingen - Azure SQL Database | Microsoft Docs
description: Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de Azure SQL-database.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/29/2019
ms.openlocfilehash: 2f52cbaf5d50eca13a87ff8e745b0f8baefbe23e
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210366"
---
# <a name="azure-sql-database-threat-detection"></a>Azure SQL Database Threat Detection

Azure SQL-Bedreigingsdetectie voor [Azure SQL Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven.

Detectie van bedreigingen maakt deel uit van de [SQL geavanceerde gegevensbeveiliging](sql-advanced-threat-protection.md) (AD) aanbieding waarmee een uniforme-voor geavanceerde mogelijkheden voor de beveiliging van SQL pakket. Detectie van bedreigingen kan worden geopend en worden beheerd via de centrale SQL AD-portal.

> [!NOTE]
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.


## <a name="what-is-threat-detection"></a>Wat is de detectie van bedreigingen

SQL-Bedreigingsdetectie biedt een nieuwe beveiligingslaag, waarmee klanten een om te detecteren en op mogelijke bedreigingen reageert zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers ontvangen een waarschuwing bij verdachte databaseactiviteiten, potentiële kwetsbaarheden, en SQL-injectie, evenals de toegang tot de afwijkende database-aanvallen en patronen-query's. SQL Threat Detection integreert waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/), waaronder de details van de verdachte activiteit en de aanbevolen actie voor het onderzoeken en tegenhouden. SQL-Bedreigingsdetectie is het eenvoudig om potentiële bedreigingen voor de database zonder de noodzaak om te worden van een beveiligingsexpert of bewakingssystemen geavanceerde beveiliging te beheren.

Voor een volledige onderzoeksmethode, is het raadzaam om in te schakelen [SQL Database Auditing](sql-database-auditing.md), waarnaar databasegebeurtenissen naar een auditlogboek in uw Azure storage-account aanmelden worden geschreven.  

## <a name="azure-sql-database-threat-detection-alerts"></a>Azure SQL Database Threat Detection-waarschuwingen

Detectie van bedreigingen voor Azure SQL Database detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven en deze kan de volgende waarschuwing is geactiveerd:

- **Vulnerability to SQL Injection**: Deze waarschuwing wordt geactiveerd wanneer een toepassing een foutieve SQL-instructie in de database genereert. Deze waarschuwing kan duiden op een mogelijk beveiligingsprobleem met SQL-injectieaanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie:

  - Een fout in de toepassingscode die de foutieve SQL-instructie maakt
  - Toepassingscode of opgeslagen procedures schonen gebruikersinvoer niet op tijdens het construeren van de foutieve SQL-instructie, dit kan worden misbruikt voor SQL-injectie
- **Mogelijke SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een actieve aanval wordt uitgevoerd tegen een geïdentificeerde toepassing beveiligingslek voor SQL-injectie. Dit betekent dat de aanvaller schadelijke SQL-instructies probeert te injecteren met de kwetsbare toepassingscode of opgeslagen procedures.
- **Toegang vanaf ongebruikelijke locatie**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand heeft aangemeld bij de SQL server vanuit een ongebruikelijke geografische locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijk Azure-datacentrum**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand heeft aangemeld bij de SQL server vanuit een ongebruikelijke Azure-datacenter die tijdens de recente periode is gedetecteerd op deze server. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI, Azure SQL Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijke**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand zich heeft aangemeld met de SQL-server met behulp van een ongebruikelijke klant (SQL-gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang vanaf een mogelijk schadelijke toepassing**: Deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
- **Brute force SQL-referenties**: Deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Afwijkende databaseactiviteiten tijdens de detectie van een verdachte activiteit verkennen

U ontvangt een e-mailmelding bij detectie van afwijkende activiteiten. Het e-mailbericht bevat informatie over de verdachte beveiligingsgebeurtenis, inclusief de aard van de afwijkende activiteiten, databasenaam, servernaam, de naam van toepassing en de tijd van de gebeurtenis. Bovendien het e-mailbericht bevat informatie over mogelijke oorzaken en aanbevolen acties om te onderzoeken en oplossen van de mogelijke bedreiging met de database.

![Rapport van de afwijkende activiteit](./media/sql-database-threat-detection/anomalous_activity_report.png)

1. Klik op de **recente SQL-waarschuwingen weergeven** koppeling in de e-mail om te starten van de Azure-portal en weergeven van de Azure Security Center pagina met waarschuwingen, waarmee u een overzicht van actieve bedreigingen die zijn gedetecteerd op de SQL-database.

   ![Activiteit bedreigingen](./media/sql-database-threat-detection/active_threats.png)

2. Klik op een specifieke waarschuwing om aanvullende gegevens en acties voor deze bedreiging te onderzoeken en oplossen van problemen met toekomstige bedreigingen.

   SQL-injectie is een van de meest voorkomende beveiligingsproblemen voor webtoepassingen op Internet die wordt gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers maken gebruik van beveiligingsproblemen van toepassingen om te injecteren in invoervelden van de toepassing, schadelijke SQL-instructies schendingen veroorzaken of wijzigen van gegevens in de database. Voor waarschuwingen van de SQL-injectie bevatten de details van de waarschuwing de kwetsbare SQL-instructie die misbruik wordt gemaakt.

   ![Specifieke waarschuwing](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Meldingen voor geconstateerde bedreigingen voor uw database in Azure portal verkennen

SQL Database Threat Detection integreert de waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/). Live SQL threat detection-tegels in de database en SQL-ADVERTENTIES blades in Azure portal de status van actieve bedreigingen bijhouden.

Klik op **Threat detection waarschuwing** naar de Azure Security Center start waarschuwingen pagina en krijg een overzicht van actieve SQL-bedreigingen gedetecteerd op de database of het datawarehouse.

   ![Threat detection waarschuwing](./media/sql-database-threat-detection/threat_detection_alert.png)

   ![Threat detection alert2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [detectie van bedreigingen in één Database](sql-database-threat-detection.md).
- Meer informatie over [detectie van bedreigingen in het beheerde exemplaar](sql-database-managed-instance-threat-detection.md).
- Meer informatie over [SQL Advanced gegevensbeveiliging](sql-advanced-threat-protection.md).
- Meer informatie over [Azure SQL Database Auditing](sql-database-auditing.md)
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Zie voor meer informatie over prijzen voor de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)  
