---
title: Schakel automatische afstemming voor Azure SQL Database | Microsoft Docs
description: U kunt inschakelen automatische afstemming op uw Azure SQL Database eenvoudig.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: vvasic
ms.openlocfilehash: d4d3b7f54c7393b57339ea149e8a79f97891dc20
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646028"
---
# <a name="enable-automatic-tuning"></a>Automatisch instellen inschakelen

Azure SQL-Database is een automatisch beheerd gegevens-service die voortdurend bewaakt uw query's en geeft de actie die u uitvoeren kunt om de prestaties van uw workload te verbeteren. U kunt bekijken van aanbevelingen en handmatig toepassen of Azure SQL Database automatisch toegepast corrigerende maatregelen kunt: dit staat bekend als **automatische afstemmen modus**. Automatische afstemming kan worden ingeschakeld op de server of de database.

## <a name="enable-automatic-tuning-on-server"></a>Automatische afstemming op server inschakelen
Op het niveau van de server kunt u automatische afstemmen configuratie van de 'Azure standaard' overnemen of niet worden overgenomen van de configuratie. Azure standaardwaarden zijn FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

### <a name="azure-portal"></a>Azure Portal
Om in te schakelen automatische afstemming op Azure SQL Database logische **server**, navigeer naar de server in Azure-portal en selecteer vervolgens **automatische afstemming** in het menu.

![Server](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Houd er rekening mee dat **DROP_INDEX** optie op dit moment is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints en mag niet worden ingeschakeld in deze gevallen.
>

Selecteer de automatische afstemmen opties die u wilt inschakelen en selecteer **toepassen**.

Opties voor automatische afstemmen op een server worden toegepast op alle databases op deze server. Standaard alle databases configuratie overnemen van hun bovenliggende server, maar dit kan worden genegeerd en afzonderlijk opgegeven voor elke database.

### <a name="rest-api"></a>REST-API
[Klik hier voor meer informatie over het inschakelen van automatische afstemming op het niveau van de server via de REST-API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

## <a name="enable-automatic-tuning-on-an-individual-database"></a>Automatische afstemming op een individuele database inschakelen

De Azure SQL Database kunt u de automatische afstemmen configuratie voor elke database afzonderlijk opgeven. Op het databaseniveau kunt u automatische afstemmen configuratie overnemen van de bovenliggende server, 'Azure standaard' of niet worden overgenomen van de configuratie. Azure standaardwaarden zijn ingesteld voor FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

> [!NOTE]
> De algemene aanbeveling is voor het beheren van de configuratie voor automatische afstemmen op **serverniveau** zodat dezelfde configuratie-instellingen kunnen worden toegepast op elke database automatisch. Configureer Automatische afstemming op een individuele database alleen als u andere instellingen dan anderen hebben dat de database moet instellingen overnemen van dezelfde server.
>

### <a name="azure-portal"></a>Azure Portal

Om in te schakelen automatische afstemming op een **één database**, navigeer naar de database in Azure-portal en selecteer **automatische afstemming**.

Afzonderlijke instellingen voor automatische afstemmen kunnen afzonderlijk worden geconfigureerd voor elke database. U kunt handmatig configureren van een afzonderlijke optie voor automatische afstemmen of opgeven dat een optie overneemt van de instellingen van de server.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Houd er rekening mee dat de optie DROP_INDEX op dit moment is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints en mag niet worden ingeschakeld in deze gevallen.

Nadat u de gewenste configuratie hebt geselecteerd, klikt u op **toepassen**.

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
