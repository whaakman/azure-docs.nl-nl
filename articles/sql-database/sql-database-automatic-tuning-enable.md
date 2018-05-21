---
title: Schakel automatische afstemming voor Azure SQL Database | Microsoft Docs
description: U kunt inschakelen automatische afstemming op uw Azure SQL Database eenvoudig.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/01/2018
ms.author: vvasic
ms.openlocfilehash: e4c3a2c1f21bf14bfc75f20dd18cefca68fd2067
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="enable-automatic-tuning"></a>Automatisch instellen inschakelen

Azure SQL-Database is een automatisch beheerd gegevens-service die voortdurend bewaakt uw query's en geeft de actie die u uitvoeren kunt om de prestaties van uw workload te verbeteren. U kunt bekijken van aanbevelingen en handmatig toepassen of Azure SQL Database automatisch toegepast corrigerende maatregelen kunt: dit staat bekend als **automatische afstemmen modus**. Automatische afstemming kan worden ingeschakeld op de server of de database.

## <a name="enable-automatic-tuning-on-server"></a>Automatische afstemming op server inschakelen
Op het niveau van de server kunt u automatische afstemmen configuratie van de 'Azure standaard' overnemen of niet worden overgenomen van de configuratie. Azure standaardwaarden zijn FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

### <a name="azure-portal"></a>Azure Portal
Automatische afstemming op Azure SQL Database inschakelen **server**, navigeer naar de server in Azure-portal en selecteer vervolgens **automatische afstemming** in het menu. Selecteer de automatische afstemmen opties die u wilt inschakelen en selecteer **toepassen**.

![Server](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Houd er rekening mee dat **DROP_INDEX** optie op dit moment is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints en in dergelijke gevallen niet worden ingeschakeld.
>

Opties voor automatische afstemmen op de server worden toegepast op alle databases op de server. Standaard alle databases de configuratie van hun bovenliggende server overnemen, maar dit kan worden genegeerd en afzonderlijk opgegeven voor elke database.

### <a name="rest-api"></a>REST-API
[Klik hier voor meer informatie over het inschakelen van automatische afstemming op het niveau van de server via de REST-API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

## <a name="enable-automatic-tuning-on-an-individual-database"></a>Automatische afstemming op een individuele database inschakelen

De Azure SQL Database kunt u de automatische configuratie afstemmen afzonderlijk op elke database opgeven. Op het databaseniveau kunt u automatische afstemmen configuratie overnemen van bovenliggende server, 'Azure standaard' of niet worden overgenomen van de configuratie. Azure zijn standaard ingeschakeld FORCE_LAST_GOOD_PLAN, CREATE_INDEX ingeschakeld en DROP_INDEX uitgeschakeld.

> [!NOTE]
> Er is een algemene aanbevelingen voor het beheren van de automatische afstemmen configuratie op serverniveau zodat dezelfde configuratie-instellingen kunnen worden toegepast op elke database automatisch. Configureer Automatische afstemming op een individuele database als de database andere die door anderen op dezelfde server.
>

### <a name="azure-portal"></a>Azure Portal

Om in te schakelen automatische afstemming op een **één database**, gaat u naar de database in de Azure portal en vervolgens selecteert **automatische afstemming**. U kunt een individuele database om de instellingen overnemen van de server door de optie configureren of u kunt de configuratie voor een database afzonderlijk opgeven.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Nadat u de juiste configuratie hebt geselecteerd, klikt u op **toepassen**.

Houd er rekening mee dat de optie DROP_INDEX op dit moment is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints en moet niet worden ingeschakeld in deze gevallen.

### <a name="rest-api"></a>REST API
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

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Automatische afstemmen e-mailmeldingen configureren

Zie [automatische afstemming van e-mailmeldingen](sql-database-automatic-tuning-email-notifications.md)

## <a name="next-steps"></a>Volgende stappen
* Lees de [automatische afstemmen artikel](sql-database-automatic-tuning.md) voor meer informatie over automatische afstemming en hoe deze kan u helpen de prestaties worden verbeterd.
* Zie [prestaties aanbevelingen](sql-database-advisor.md) voor een overzicht van Azure SQL Database prestaties aanbevelingen.
* Zie [inzichten voor queryprestaties](sql-database-query-performance.md) voor meer informatie over het weergeven van de prestatie-invloed van de meest gebruikte query's.
