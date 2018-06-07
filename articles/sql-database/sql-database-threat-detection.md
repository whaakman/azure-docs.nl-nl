---
title: Bedreigingendetectie - Azure SQL Database | Microsoft Docs
description: Detectie van dreigingen detecteert afwijkende databaseactiviteiten die wijzen op beveiligingsdreigingen voor de database.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: 09ba4b3b72d5c82dc42199f2f883cedee6609bd2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649540"
---
# <a name="azure-sql-database-threat-detection"></a>Detectie van dreigingen voor Azure SQL Database

Azure SQL Database met detectie van dreigingen detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke probeert te openen of misbruik van databases aangeeft.

Detectie van dreigingen is onderdeel van de [SQL geavanceerde Threat Protection](sql-advanced-threat-protection.md) (ATP)-oplossing, dit een uniform pakket voor geavanceerde mogelijkheden voor SQL-beveiliging is. Detectie van dreigingen worden geopend en beheerd via de centrale SQL ATP-portal.

## <a name="what-is-threat-detection"></a>Wat is de detectie van dreigingen?

Detectie van dreigingen SQL biedt een nieuwe laag van beveiliging, waarmee klanten om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen doordat beveiligingswaarschuwingen op vreemde activiteiten worden gedetecteerd. Gebruikers ontvangen een waarschuwing bij verdachte databaseactiviteiten, mogelijke beveiligingsproblemen en SQL-injectie aanvallen, evenals afwijkende databasetoegang en patronen-query's. Detectie van dreigingen SQL integreert waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/), waaronder details van verdachte activiteit en de aanbevolen actie voor het onderzoeken en risico. Detectie van dreigingen SQL kunt u eenvoudig op mogelijke bedreigingen adres met de database hoeft te worden van een deskundige beveiliging of geavanceerde bewaking beveiligingssystemen beheren. 

Voor een volledige onderzoek-ervaring wordt aanbevolen om in te schakelen [SQL Database Auditing](sql-database-auditing.md), waarnaar schrijft databasegebeurtenissen naar een auditlogboek Meld u bij uw Azure storage-account.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Detectie van dreigingen voor uw database in de Azure portal instellen
1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van de Azure SQL Database-server die u wilt beveiligen. Selecteer in de beveiligingsinstellingen **Advanced Threat Protection**.
3. Op de **Advanced Threat Protection** configuratiepagina:

   - Advanced Threat Protection inschakelen op de server.
   - In **Threat detectie-instellingen**, in de **waarschuwingen verzenden naar** tekst Geef de lijst met e-mailberichten beveiligingswaarschuwingen na detectie van afwijkende databaseactiviteiten ontvangen.
  
   ![Detectie van dreigingen instellen](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Detectie van dreigingen met behulp van PowerShell instellen

Zie voor een scriptvoorbeeld van een, [configureren van controle en detectie van bedreigingen met behulp van PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Verken afwijkende databaseactiviteiten na detectie van een verdachte activiteit

U ontvangt een e-mailmelding na detectie van afwijkende databaseactiviteiten. Het e-mailbericht bevat informatie over de verdachte-gebeurtenis met inbegrip van de aard van de afwijkende activiteiten, databasenaam, servernaam, toepassingsnaam en de tijd van de gebeurtenis. Bovendien wordt het e-mailbericht bevat informatie over mogelijke oorzaken en aanbevolen acties te onderzoeken en potentiële risico dat naar de database.

![Rapport van de afwijkende activiteit](./media/sql-database-threat-detection/anomalous_activity_report.png)
     
1. Klik op de **recente SQL-waarschuwingen weergeven** koppeling in het e-mailbericht naar de Azure-portal te starten en weergeven van de pagina waarschuwingen Azure Security Center, waarin u een overzicht van actieve bedreigingen die zijn gedetecteerd op de SQL-database.

   ![Activty bedreigingen](./media/sql-database-threat-detection/active_threats.png)

2. Klik op een specifieke waarschuwing voor meer informatie en acties voor deze bedreiging onderzoeken en oplossen van problemen met toekomstige bedreigingen.

   SQL-injectie is bijvoorbeeld een van de meest voorkomende Web application beveiligingsproblemen op Internet die wordt gebruikt voor aanvallen gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de toepassing invoervelden, schendingen veroorzaken of wijzigen van gegevens in de database. Details van de waarschuwing zijn voor SQL-injectie waarschuwingen, kwetsbaar SQL-instructie dat misbruik wordt gemaakt.

   ![Specifieke waarschuwing](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Verken dagelijks geconstateerde waarschuwingen voor uw database in de Azure portal

Detectie van SQL Database dreigingen integreert de waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/). Een live SQL threat detectie-tegels binnen de database en SQL ATP blades in de Azure portal houdt de status van actieve bedreigingen.

Klik op **Threat detectie waarschuwing** starten van het Azure Beveiligingscentrum waarschuwingen pagina en krijgt u een overzicht van actieve SQL bedreigingen die zijn gedetecteerd op de database.

   ![Waarschuwing voor Threat](./media/sql-database-threat-detection/threat_detection_alert.png)
   
   ![Threat detectie alert2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="azure-sql-database-threat-detection-alerts"></a>Waarschuwingen van Azure SQL Database-Bedreigingsdetectie 
Detectie van dreigingen voor Azure SQL Database detecteert afwijkende activiteiten die aangeeft ongebruikelijke en potentieel schadelijke probeert te openen of misbruik van databases en de volgende waarschuwingen kan worden geactiveerd:
- **Kwetsbaarheid voor SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een foutieve SQL-instructie wordt gedetecteerd in een database. Deze waarschuwing kan duiden op mogelijke kwetsbaarheid voor SQL-injectieaanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie:
   - Een fout in de toepassingscode die de foutieve SQL-instructie maakt
   - Toepassingscode of opgeslagen procedures schonen gebruikersinvoer niet op tijdens het construeren van de foutieve SQL-instructie, dit kan worden misbruikt voor SQL-injectie
- **Mogelijke SQL-injectie**: deze waarschuwing wordt geactiveerd wanneer een actieve aanval wordt uitgevoerd tegen een geïdentificeerd beveiligingslek voor SQL-injectie in een toepassing. Dit betekent dat de aanvaller schadelijke SQL-instructies probeert te injecteren met de kwetsbare toepassingscode of opgeslagen procedures.
- **Toegang vanaf ongebruikelijke locatie**: deze waarschuwing wordt geactiveerd wanneer er een wijziging is in het toegangspatroon tot de SQL-server, waarbij iemand zich vanuit een ongebruikelijke geografische locatie heeft aangemeld bij de SQL server. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijk Azure-datacentrum**: deze waarschuwing wordt geactiveerd wanneer er een wijziging is in het toegangspatroon tot de SQL-server, waarbij iemand zich heeft aangemeld bij de SQL server vanuit een Azure-datacentrum dat ongebruikelijk was op deze server in de recente periode. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI, Azure SQL Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijke klant**: deze waarschuwing wordt geactiveerd wanneer er een wijziging is in het toegangspatroon tot de SQL-server, waarbij iemand zich vanuit een ongebruikelijke klant (SQL-gebruiker) heeft aangemeld bij de SQL-server. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang tot een toepassing die mogelijk schadelijk is**: deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
- **Brute Force SQL-referenties**: deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties is. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [SQL Advanced Threat Protection](sql-advanced-threat-protection.md). 
* Meer informatie over [Azure SQL Database Auditing](sql-database-auditing.md)
* Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Zie voor meer informatie over prijzen de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)  
