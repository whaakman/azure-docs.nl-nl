---
title: Schakel automatische afstemming voor Azure SQL Database | Microsoft Docs
description: U kunt inschakelen automatische afstemming op uw Azure SQL Database eenvoudig.
services: sql-database
documentationcenter: 
author: veljko-msft
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/19/2016
ms.author: veljko-msft
ms.openlocfilehash: bf8e0203112a42132a80e234964747c550fea284
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="enable-automatic-tuning"></a>Automatisch instellen inschakelen

Azure SQL-Database is een automatisch beheerd gegevens-service die voortdurend bewaakt uw query's en geeft de actie die u uitvoeren kunt om de prestaties van uw workload te verbeteren. U kunt bekijken van aanbevelingen en handmatig toepassen of Azure SQL Database automatisch toegepast corrigerende maatregelen kunt: dit staat bekend als **automatische afstemmen modus**. Automatische afstemming kan worden ingeschakeld op de server of de database.

## <a name="enable-automatic-tuning-on-server"></a>Automatische afstemming op server inschakelen
Op het niveau van de server kunt u automatische afstemmen configuratie van de 'Azure standaard' overnemen of niet worden overgenomen van de configuratie. Azure zijn standaard ingeschakeld FORCE_LAST_GOOD_PLAN, CREATE_INDEX ingeschakeld en DROP_INDEX uitgeschakeld.

### <a name="portal"></a>Portal
Om automatische afstemming op Azure SQL Database-server, gaat u naar de server in Azure-portal en selecteer vervolgens **automatische afstemming** in het menu. Selecteer de automatische afstemmen opties die u wilt inschakelen en selecteer **toepassen**:

![Server](./media/sql-database-automatic-tuning-enable/server.png)

Opties voor automatische afstemmen op de server worden toegepast op alle databases op de server. Standaard alle databases de configuratie van hun bovenliggende server overnemen, maar dit kan worden genegeerd en afzonderlijk opgegeven voor elke database.

### <a name="rest-api"></a>REST API
[Klik hier voor meer informatie over het inschakelen van automatische afstemming op het niveau van de server via de REST-API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

## <a name="enable-automatic-tuning-on-database"></a>Schakel automatische afstemming op database

De Azure SQL Database kunt u de automatische configuratie afstemmen afzonderlijk op elke database opgeven. Op het databaseniveau kunt u automatische afstemmen configuratie overnemen van bovenliggende server, 'Azure standaard' of niet worden overgenomen van de configuratie. Azure zijn standaard ingeschakeld FORCE_LAST_GOOD_PLAN, CREATE_INDEX ingeschakeld en DROP_INDEX uitgeschakeld.

> [!NOTE]
> Er is een algemene aanbevelingen voor het beheren van de automatische afstemmen configuratie op serverniveau zodat dezelfde configuratie-instellingen kunnen worden toegepast op elke database automatisch. Configureer Automatische afstemming op een individuele database als de database andere die door anderen op dezelfde server.
>

### <a name="portal"></a>Portal

Navigeer naar de database in de Azure portal om automatische afstemming op een individuele database, en vervolgens selecteert **automatische afstemming**. U kunt een individuele database om de instellingen overnemen van de server door de optie configureren of u kunt de configuratie voor een database afzonderlijk opgeven.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Nadat u de juiste configuratie hebt geselecteerd, klikt u op **toepassen**.

### <a name="rest-api"></a>Rest-API
[Klik hier voor meer informatie over het inschakelen van automatische afstemming op een individuele database via de REST-API](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning)

### <a name="t-sql"></a>T-SQL

Verbinding maken met de database om automatische afstemming op een individuele database via T-SQL, en voer de volgende query uit:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
Instellen van automatische afstemming op AUTO toepassing Azure standaardwaarden. Instellen op OVERNEMEN, worden automatische afstemmen configuratie overgenomen van de bovenliggende server. AANGEPASTE kiezen, moet u handmatig configureren van automatische afstemming.

Configureren van afzonderlijke opties voor automatische afstemmen via T-SQL, verbinding maken met de database en de query zoals deze wordt uitgevoerd:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
De optie voor het afstemmen van afzonderlijke instellen op ON, overschrijven de instellingen die database overgenomen en schakelt u de optie afstemmen. Instellen op OFF, worden ook alle instellingen die database overgenomen onderdrukken en schakel de optie afstemmen. Automatische afstemmen optie waarvoor de STANDAARDWAARDE is opgegeven, worden overgenomen door de configuratie van automatische afstemming van de instelling databaseniveau.  

## <a name="disabled-by-the-system"></a>Uitgeschakeld door het systeem
Automatische afstemming wordt bewaakt door alle acties die nodig is voor de database en in sommige gevallen kan bepalen dat automatische afstemming is niet goed voor de database geschikt. In dit geval wordt afstemmingsoptie uitgeschakeld door het systeem. In de meeste gevallen is dit gebeurt omdat de Query Store is niet ingeschakeld of alleen-lezen status op een specifieke database.

## <a name="next-steps"></a>Volgende stappen
* Lees de [automatische afstemmen artikel](sql-database-automatic-tuning.md) voor meer informatie over automatische afstemming en hoe deze kan u helpen de prestaties worden verbeterd.
* Zie [prestaties aanbevelingen](sql-database-advisor.md) voor een overzicht van Azure SQL Database prestaties aanbevelingen.
* Zie [inzichten voor queryprestaties](sql-database-query-performance.md) voor meer informatie over het weergeven van de prestatie-invloed van de meest gebruikte query's.
