---
title: Bedreigingendetectie - Azure SQL-Database beheerd exemplaar | Microsoft Docs
description: Met detectie van bedreigingen worden afwijkende databaseactiviteiten gedetecteerd die kunnen duiden op beveiligingsdreigingen voor de database.
services: sql-database
author: rmatchoro
manager: cguyer
ms.service: sql-database
ms.custom: security, managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: f8c08bc16d622516958b8bd182179d07edfa4891
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="azure-sql-database-managed-instance-threat-detection"></a>Azure SQL-Database beheerd Bedreigingsdetectie exemplaar

Detectie van dreigingen SQL detecteert afwijkende activiteiten die aangeeft ongebruikelijke en potentieel schadelijke probeert te krijgen of misbruik van databases in een Azure SQL Database beheerd exemplaar (preview).

## <a name="overview"></a>Overzicht

Detectie van dreigingen detecteert afwijkende databaseactiviteiten beveiligingsdreigingen voor beheerde exemplaar waarmee wordt aangegeven. Detectie van dreigingen is nu in preview voor beheerde exemplaar.

Detectie van dreigingen biedt een nieuwe laag van beveiliging, waarmee klanten om te detecteren en op mogelijke bedreigingen reageert als deze door te geven beveiligingswaarschuwingen over afwijkende databaseactiviteiten problemen optreden. Detectie van dreigingen kunt u eenvoudig op mogelijke bedreigingen adres met het exemplaar worden beheerd zonder de noodzaak een deskundige beveiliging of systemen bewaking van de geavanceerde beveiliging te beheren. Voor een volledige onderzoek-ervaring verdient het inschakelen van Azure beheerd exemplaar controle, schrijft de databasegebeurtenissen naar een auditlogboek in uw Azure storage-account worden geregistreerd. 

Detectie van dreigingen SQL integreert waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/), en elke beveiligde beheerd instantie wordt gefactureerd op dezelfde prijs als Azure Security Center Standard-laag op $15/knooppunt/maand, waarbij elk exemplaar beheerd beveiligd worden geteld als één knooppunt.  

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Detectie van dreigingen instellen voor uw beheerde exemplaar in de Azure portal
1. Starten van de Azure portal op [https://portal.azure.com](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van het beheerde exemplaar u wilt beveiligen. In de **instellingen** pagina **Bedreigingsdetectie**. 
3. In de configuratiepagina van de detectie van dreigingen 
   - Schakel **ON** Bedreigingendetectie.
   - Configureer de **lijst met e-mailberichten** beveiligingswaarschuwingen na detectie van afwijkende databaseactiviteiten ontvangen.
   - Selecteer de **Azure storage-account** waarin afwijkende threat controlerecords worden opgeslagen. 
4.  Klik op **opslaan** de nieuwe of bijgewerkte threat detectie-beleid op te slaan.

   ![Detectie van dreigingen](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="explore-anomalous-managed-instance-activities-upon-detection-of-a-suspicious-event"></a>Verken vreemde activiteiten worden beheerd exemplaar na de detectie van een verdachte activiteit

1. U ontvangt een e-mailmelding na detectie van afwijkende databaseactiviteiten. 

   Het e-mailbericht bevat informatie over de verdachte-gebeurtenis met inbegrip van de aard van de afwijkende activiteiten, databasenaam, de servernaam van de en de tijd van de gebeurtenis. Bovendien bevat informatie over mogelijke oorzaken en aanbevolen acties te onderzoeken en potentiële risico dat met het exemplaar worden beheerd.

   ![Threat e-detectie](./media/sql-database-managed-instance-threat-detection/threat-detection-email.png)

2. Klik op de **recente SQL-waarschuwingen weergeven** koppeling in het e-mailbericht naar de Azure-portal te starten en de pagina waarschuwingen Azure Security Center, waarin u een overzicht van actieve SQL bedreigingen die zijn gedetecteerd op de beheerde exemplaar in de database weer te geven.

   ![actieve bedreigingen](./media/sql-database-managed-instance-threat-detection/active-threats.png)

3. Klik op een specifieke waarschuwing voor meer informatie en acties voor deze bedreiging onderzoeken en oplossen van problemen met toekomstige bedreigingen.

   SQL-injectie is bijvoorbeeld een van de algemene Web application beveiligingsproblemen op het Internet. SQL-injectie wordt gebruikt voor aanvallen gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de toepassing invoervelden, schendingen veroorzaken of wijzigen van gegevens in de database. Details van de waarschuwing zijn voor SQL-injectie waarschuwingen, kwetsbaar SQL-instructie dat misbruik wordt gemaakt.

   ![SQL-injectie](./media/sql-database-managed-instance-threat-detection/sql-injection.png)

## <a name="managed-instance-threat-detection-alerts"></a>Beheerde exemplaar van de detectie van dreigingen waarschuwingen 

Detectie van dreigingen voor beheerde exemplaar detecteert afwijkende activiteiten die aangeeft ongebruikelijke en potentieel schadelijke probeert te openen of misbruik van databases en de volgende waarschuwingen kan worden geactiveerd:
- **Kwetsbaarheid voor SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een foutieve SQL-instructie wordt gedetecteerd in een database. Deze waarschuwing kan duiden op mogelijke kwetsbaarheid voor SQL-injectieaanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie:
 - Een fout in de toepassingscode die de foutieve SQL-instructie maakt
 - Toepassingscode of opgeslagen procedures schonen gebruikersinvoer niet op tijdens het construeren van de foutieve SQL-instructie, dit kan worden misbruikt voor SQL-injectie
- **Mogelijke SQL-injectie**: deze waarschuwing wordt geactiveerd wanneer een actieve aanval wordt uitgevoerd tegen een geïdentificeerd beveiligingslek voor SQL-injectie in een toepassing. Betekent dit dat de aanvaller probeert te injecteren schadelijke SQL-instructies met kwetsbaar toepassingscode of opgeslagen procedures.
- **Toegang vanaf ongebruikelijke locatie**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot een exemplaar beheerd wanneer iemand zich heeft aangemeld met het exemplaar beheerd vanaf een ongebruikelijke geografische locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of ontwikkelaarshandleiding onderhoudsbewerking). De waarschuwing detecteert in andere gevallen is een schadelijke actie (voormalig werknemer, externe aanvaller, enzovoort).
- **Toegang vanaf ongebruikelijke Azure-Datacenter**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot beheerde exemplaar wanneer iemand zich heeft aangemeld met het beheerde-exemplaar van een Azure-datacenter die niet zichtbaar is voor toegang tot deze beheerde Exemplaar tijdens de recente periode. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI en Azure SQL-Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
- **Toegang vanaf onbekende principal**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon exemplaar beheerde server, waar iemand zich heeft aangemeld met het exemplaar worden beheerd met behulp van een ongebruikelijke principal (SQL-gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (bewerking van het nieuwe toepassingsontwikkelaar onderhoud). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
- **Toegang tot een toepassing die mogelijk schadelijk is**: deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
- **Brute Force SQL-referenties**: deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties is. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. De waarschuwing detecteert in andere gevallen is een beveiligingsaanval.

## <a name="next-steps"></a>Volgende stappen

- Lees over beheerde exemplaar [wat is er een exemplaar beheerd](sql-database-managed-instance.md)
- Meer informatie over [beheerd exemplaar controle](https://go.microsoft.com/fwlink/?linkid=869430) 
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
