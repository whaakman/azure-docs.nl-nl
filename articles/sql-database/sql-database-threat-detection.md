---
title: Bedreigingendetectie - Azure SQL Database | Microsoft Docs
description: Met detectie van bedreigingen worden afwijkende databaseactiviteiten gedetecteerd die kunnen duiden op beveiligingsdreigingen voor de database.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 04/01/2018
ms.author: ronmat
ms.openlocfilehash: c4a94ab9c7e0dab9e8c25e54fdd0a30b28b7a8a3
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="sql-database-threat-detection"></a>Detectie van dreigingen van SQL Database

Detectie van dreigingen SQL detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke probeert te openen of misbruik van databases aangeeft.

## <a name="overview"></a>Overzicht

Detectie van dreigingen SQL biedt een nieuwe laag van beveiliging, waarmee klanten om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen doordat beveiligingswaarschuwingen op vreemde activiteiten worden gedetecteerd.  Gebruikers ontvangen een melding van de op verdachte databaseactiviteiten, mogelijke beveiligingsproblemen en SQL-injectieaanvallen, evenals database afwijkende toegangspatronen. Detectie van dreigingen SQL waarschuwingen Geef details op van de verdachte activiteit en actie voor het onderzoeken en het risico dat het beste. Gebruikers kunnen de verdachte gebeurtenissen met verkennen [SQL Database Auditing](sql-database-auditing.md) om te bepalen of ze het gevolg zijn van een poging om te openen, inbreuk of misbruik van gegevens in de database. Met bedreigingsdetectie is het eenvoudig om potentiële bedreigingen voor de database weg te nemen zonder de hulp van een beveiligingsexpert of het moeten beheren van geavanceerde bewakingssystemen.

SQL-injectie is bijvoorbeeld een van de algemene Web application beveiligingsproblemen op het Internet worden gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de toepassing invoervelden, schendingen veroorzaken of wijzigen van gegevens in de database.

Detectie van dreigingen SQL integreert waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/), en elke beveiligde SQL Database-server wordt gefactureerd op dezelfde prijs als Azure Security Center Standard-laag op $15/knooppunt/maand, waar elke SQL-Database hebt beveiligd Server worden geteld als één knooppunt.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Detectie van dreigingen voor uw database in de Azure portal instellen
1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van de SQL-Database die u wilt bewaken. Selecteer in de pagina instellingen **controle en detectie van dreigingen**. 
    ![Navigatiedeelvenster][1]
3. In de **controle en detectie van dreigingen** configuratiepagina inschakelen **ON** controle, waarin de threat detectie-instellingen worden weergegeven.
  
    ![Navigatievenster][2]
4. Schakel **ON** Bedreigingendetectie.
5. De lijst met e-mailberichten voor het ontvangen van beveiligingsberichten na detectie van afwijkende databaseactiviteiten configureren.
6. Klik op **opslaan** in de **controle en detectie van bedreigingen** pagina om de nieuwe of bijgewerkte controle en threat detectie-instellingen opslaan.
       
    ![Navigatievenster][3]

## <a name="set-up-threat-detection-using-powershell"></a>Detectie van dreigingen met behulp van PowerShell instellen

Zie voor een scriptvoorbeeld van een, [configureren van controle en detectie van bedreigingen met behulp van PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Verken afwijkende databaseactiviteiten na detectie van een verdachte activiteit
1. U ontvangt een e-mailmelding na detectie van afwijkende databaseactiviteiten. <br/>
   Het e-mailbericht bevat informatie over de verdachte-gebeurtenis met inbegrip van de aard van de afwijkende activiteiten, databasenaam, servernaam, toepassingsnaam en de tijd van de gebeurtenis. Bovendien wordt het e-mailbericht bevat informatie over mogelijke oorzaken en aanbevolen acties te onderzoeken en potentiële risico dat naar de database.<br/>
     
    ![Navigatievenster][4]
2. De e-mailmelding bevat een directe koppeling naar het SQL-logboek. Op deze koppeling te klikken, start de Azure-portal en Hiermee opent u de SQL-controlerecords rond de tijd van de verdachte activiteit. Klik op een controlerecord voor meer informatie over de verdachte databaseactiviteiten, waardoor het gemakkelijker vinden van de SQL-instructies die zijn uitgevoerd (die toegankelijk zijn, wat ze hebben gedaan en wanneer) en bepalen of de gebeurtenis legitieme of schadelijke is (bijvoorbeeld een toepassing kwetsbaarheid voor SQL-injectie misbruik wordt gemaakt, iemand geschonden gevoelige gegevens, enzovoort).<br/>
   ![Navigatiedeelvenster][5]


## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Verken dagelijks geconstateerde waarschuwingen voor uw database in de Azure portal

Detectie van SQL Database dreigingen integreert de waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/). Een live SQL security-tegel in de database page in de Azure portal houdt de status van actieve bedreigingen. 

   ![Navigatievenster][6]
   
1. Beveiligingstegel te klikken op de SQL wordt gestart van de pagina Azure Security Center-waarschuwingen en biedt een overzicht van actieve SQL bedreigingen die zijn gedetecteerd op de database. 

  ![Navigatievenster][7]

2. Te klikken op een specifieke waarschuwing biedt aanvullende informatie en acties voor deze bedreiging onderzoeken en oplossen van problemen met toekomstige bedreigingen.

  ![Navigatievenster][8]


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over detectie van dreigingen, gaat u naar de [Azure-blog](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/) 
* Meer informatie over [Azure SQL Database Auditing](sql-database-auditing.md)
* Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Zie voor meer informatie over prijzen de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)  
* Zie voor een voorbeeld van de PowerShell-script, [controle en detectie van bedreigingen met behulp van PowerShell configureren](scripts/sql-database-auditing-and-threat-detection-powershell.md)



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


