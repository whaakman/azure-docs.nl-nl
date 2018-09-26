---
title: Detectie van bedreigingen - Azure SQL Database Managed Instance | Microsoft Docs
description: Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database in een beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
manager: craigg
ms.date: 09/19/2018
ms.openlocfilehash: 7f05bd6c96a8e22b1e039c3edcec491b7c079d54
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162406"
---
# <a name="azure-sql-database-managed-instance-threat-detection"></a>Azure SQL Database Managed Instance detectie van bedreigingen

SQL Threat Detection detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases in een Azure SQL Database Managed Instance aangeeft.

## <a name="overview"></a>Overzicht

Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's naar Managed Instance. Detectie van bedreigingen is nu in Preview-versie van Managed Instance.

Bedreigingsdetectie biedt een nieuwe beveiligingslaag, waarmee klanten een om te detecteren en op mogelijke bedreigingen reageert zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Detectie van bedreigingen is het eenvoudig om potentiële bedreigingen voor het beheerde exemplaar zonder de noodzaak om te worden van een beveiligingsexpert of beheren van geavanceerde bewakingssystemen. Voor een volledige onderzoeksmethode het verdient aanbeveling om in te schakelen Azure beheerd exemplaar Auditing, schrijft databasegebeurtenissen naar een auditlogboek in uw Azure storage-account aanmelden. 

SQL Threat Detection integreert waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/), en elke beveiligde Managed Instance wordt gefactureerd tegen dezelfde prijs als Azure Security Center Standard-laag, op $15/knooppunt/maand, waarbij elke Managed Instance beveiligde Als één knooppunt wordt beschouwd.  

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Detectie van bedreigingen instellen voor uw beheerde exemplaar in de Azure-portal
1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van het beheerde exemplaar u wilt beveiligen. In de **instellingen** weergeeft, schakelt **detectie van bedreigingen**. 
3. In de configuratiepagina van de detectie van bedreigingen 
   - Schakel **ON** detectie van bedreigingen.
   - Configureer de **lijst met e-mailberichten** voor het ontvangen van beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten.
   - Selecteer de **Azure storage-account** waar controlerecords afwijkende bedreigingen zijn opgeslagen. 
4.  Klik op **opslaan** de nieuwe of bijgewerkte threat detection-beleid op te slaan.

   ![Detectie van bedreigingen](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="explore-anomalous-managed-instance-activities-upon-detection-of-a-suspicious-event"></a>Afwijkende Managed Instance-activiteiten tijdens de detectie van een verdachte activiteit verkennen

1. U ontvangt een e-mailmelding bij detectie van afwijkende activiteiten. 

   Het e-mailbericht bevat informatie over de verdachte beveiligingsgebeurtenis, inclusief de aard van de afwijkende activiteiten, databasenaam, de naam van server en de tijd van de gebeurtenis. Bovendien vindt u informatie over mogelijke oorzaken en aanbevolen acties om te onderzoeken en oplossen van de mogelijke bedreiging met het beheerde exemplaar.

   ![Threat detection-e-mailadres.](./media/sql-database-managed-instance-threat-detection/threat-detection-email.png)

2. Klik op de **recente SQL-waarschuwingen weergeven** koppeling in de e-mail om te starten van de Azure portal en de Azure Security Center pagina met waarschuwingen, waarmee u een overzicht van actieve SQL-bedreigingen die zijn gedetecteerd op het beheerde exemplaar database weergeven.

   ![actieve bedreigingen](./media/sql-database-managed-instance-threat-detection/active-threats.png)

3. Klik op een specifieke waarschuwing om aanvullende gegevens en acties voor deze bedreiging te onderzoeken en oplossen van problemen met toekomstige bedreigingen.

   SQL-injectie is een van de veelvoorkomende beveiligingsproblemen voor webtoepassingen op Internet. SQL-injectie is gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers maken gebruik van beveiligingsproblemen van toepassingen om te injecteren in invoervelden van de toepassing, schadelijke SQL-instructies schendingen veroorzaken of wijzigen van gegevens in de database. Voor waarschuwingen van de SQL-injectie bevatten de details van de waarschuwing de kwetsbare SQL-instructie die misbruik wordt gemaakt.

   ![SQL-injectie](./media/sql-database-managed-instance-threat-detection/sql-injection.png)

## <a name="managed-instance-threat-detection-alerts"></a>Beheerde exemplaar Threat Protection-waarschuwingen 

Detectie van bedreigingen voor de Managed Instance detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven en deze kan de volgende waarschuwing is geactiveerd:
- **Kwetsbaarheid voor SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een foutieve SQL-instructie wordt gedetecteerd in een database. Deze waarschuwing kan duiden op mogelijke kwetsbaarheid voor SQL-injectieaanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie:
 - Een fout in de toepassingscode die de foutieve SQL-instructie maakt
 - Toepassingscode of opgeslagen procedures schonen gebruikersinvoer niet op tijdens het construeren van de foutieve SQL-instructie, dit kan worden misbruikt voor SQL-injectie
- **Mogelijke SQL-injectie**: deze waarschuwing wordt geactiveerd wanneer een actieve aanval wordt uitgevoerd tegen een geïdentificeerd beveiligingslek voor SQL-injectie in een toepassing. Dit betekent dat de aanvaller probeert te injecteren schadelijke SQL-instructies met de kwetsbare toepassingscode of opgeslagen procedures.
- **Toegang vanaf ongebruikelijke locatie**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon naar een beheerd exemplaar, waarbij iemand zich heeft aangemeld met het beheerde exemplaar van een ongebruikelijke geografische locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of de onderhoudsbewerking-ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller, enzovoort).
- **Toegang vanaf ongebruikelijk Azure-datacentrum**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot Managed Instance, waarbij iemand zich heeft aangemeld met het beheerde exemplaar van een Azure-datacenter die niet zichtbaar is voor toegang tot dit beheerde Het exemplaar in de recente periode. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI en Azure SQL-Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
- **Toegang vanaf ongebruikelijke**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot Managed Instance-server, waarbij iemand zich heeft aangemeld met het beheerde exemplaar met behulp van een ongebruikelijke klant (SQL-gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (bewerking van het nieuwe toepassingsontwikkelaar onderhoud). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang tot een toepassing die mogelijk schadelijk is**: deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
- **Brute Force SQL-referenties**: deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties is. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een brute force-aanval.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Managed Instance, Zie [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Meer informatie over [beheerd exemplaar controleren](https://go.microsoft.com/fwlink/?linkid=869430) 
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
