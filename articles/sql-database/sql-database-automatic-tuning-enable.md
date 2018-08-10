---
title: Automatisch instellen voor Azure SQL Database inschakelen | Microsoft Docs
description: U kunt automatisch afstemmen op uw Azure SQL Database eenvoudig.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 9ebc3a8cb01d93fc6cec5d208c5a10020413cec2
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631092"
---
# <a name="enable-automatic-tuning"></a>Automatisch instellen inschakelen

Azure SQL Database is een automatisch beheerde service waarmee voortdurend uw query's worden gecontroleerd en geeft de actie die u uitvoeren kunt voor betere prestaties van uw workload. U kunt Bekijk aanbevelingen en handmatig toepassen of laat Azure SQL Database automatisch corrigerende maatregelen treffen: dit staat bekend als **modus voor automatisch afstemmen**.

Automatisch afstemmen kan worden ingeschakeld op de server of het databaseniveau van de via de [Azure-portal](sql-database-automatic-tuning-enable.md#azure-portal), [REST-API](sql-database-automatic-tuning-enable.md#rest-api) aanroepen en [T-SQL](sql-database-automatic-tuning-enable.md#t-sql) opdrachten.

## <a name="enable-automatic-tuning-on-server"></a>Automatisch instellen op server inschakelen
U kunt kiezen om over te nemen van de configuratie voor automatisch afstemmen van de 'Azure standaard' of niet over te nemen van de configuratie op niveau van de server. Standaardinstellingen van Azure zijn FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

### <a name="azure-portal"></a>Azure Portal
Om in te schakelen automatisch afstemmen op logische Azure SQL-Database **server**, navigeer naar de server in Azure portal en selecteer vervolgens **automatisch afstemmen** in het menu.

![Server](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Houd er rekening mee dat **DROP_INDEX** optie op dit moment is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints en mag niet worden ingeschakeld in deze gevallen.
>

Selecteer de opties voor automatisch afstemmen om te schakelen en selecteer de gewenste **toepassen**.

Opties voor automatisch afstemmen op een server worden toegepast op alle databases op deze server. Standaard alle databases configuratie van de bovenliggende server overnemen, maar dit kan worden overschreven en voor elke database afzonderlijk opgegeven.

### <a name="rest-api"></a>REST-API

Meer informatie over het gebruik van REST-API voor het inschakelen van automatisch afstemmen op een server, Zie [Server automatisch afstemmen van SQL-UPDATE als GET HTTP-methoden](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).


## <a name="enable-automatic-tuning-on-an-individual-database"></a>Automatisch instellen op een afzonderlijke database inschakelen

De Azure SQL Database kunt u de configuratie voor automatisch afstemmen voor elke database afzonderlijk opgeven. U kunt kiezen configuratie voor automatisch afstemmen overgenomen van de bovenliggende server, 'Azure standaard' of niet over te nemen van de configuratie op het databaseniveau van de. Standaardinstellingen van Azure zijn ingesteld op FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

> [!NOTE]
> De algemene aanbeveling is voor het beheren van de configuratie voor automatisch afstemmen op **serverniveau** zodat dezelfde configuratie-instellingen kunnen worden toegepast op elke database automatisch. Configureren van automatisch afstemmen op een afzonderlijke database alleen als u andere instellingen dan de andere database neemt instellingen over van dezelfde server.
>

### <a name="azure-portal"></a>Azure Portal

Om in te schakelen automatisch afstemmen op een **individuele database**, gaat u naar de database in Azure portal en selecteer **automatisch afstemmen**.

Afzonderlijke instellingen voor automatisch afstemmen kunnen afzonderlijk worden geconfigureerd voor elke database. U kunt handmatig configureren van een afzonderlijke optie voor automatisch afstemmen of opgeven dat een optie overneemt van de instellingen van de server.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Houd er rekening mee dat de optie DROP_INDEX op dit moment is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints en mag niet worden ingeschakeld in deze gevallen.

Nadat u de gewenste configuratie hebt geselecteerd, klikt u op **toepassen**.

### <a name="rest-api"></a>REST API

Meer informatie over het gebruik van REST-API om in te schakelen automatisch afstemmen voor een individuele database, Zie [Database automatisch afstemmen van SQL-UPDATE als GET HTTP-methoden](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning).

### <a name="t-sql"></a>T-SQL

Zo kunnen de functie automatisch afstemmen voor een individuele database via T-SQL, verbinding maken met de database en voer de volgende query uit:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
Instellen van automatisch afstemmen op AUTO is van toepassing standaard van Azure. Instellen op OVERNEMEN, configuratie voor automatisch afstemmen overgenomen van de bovenliggende server. AANGEPASTE kiezen, moet u het handmatig configureren van automatisch afstemmen.

Verbinding maken met de database voor het configureren van afzonderlijke opties voor automatisch afstemmen via T-SQL, en voer de query uit zoals deze:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
De afzonderlijke afstemmingsoptie instellen op ON, overschrijven de instellingen die van de database overgenomen en de afstemmingsoptie inschakelen. Instellen op uitgeschakeld, worden ook alle instellingen die van de database overgenomen onderdrukken en de afstemmingsoptie uitschakelen. Optie voor automatisch afstemmen, waarvoor de STANDAARDWAARDE is opgegeven, wordt de configuratie van het databaseniveau van de automatisch afstemmen van de instelling overnemen.  

Zoeken naar onze meer maar T-SQL-opties voor het configureren van automatisch afstemmen, Zie [ALTER DATABASE SET Options (Transact-SQL) voor de logische SQL Database-server](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-database-transact-sql-set-options?view=sql-server-2017&tabs=sqldbls#arguments-1).

## <a name="disabled-by-the-system"></a>Uitgeschakeld door het systeem
Alle acties die het duurt voor de database automatisch afstemmen wordt bewaakt en in sommige gevallen kan bepalen dat automatisch afstemmen correct op de database kan niet werken. In dit geval worden, afstemmingsoptie uitgeschakeld door het systeem. In de meeste gevallen is dit gebeurt omdat Query Store is niet ingeschakeld of deze heeft de status alleen-lezen op een specifieke database.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Automatische afstemming e-mailmeldingen configureren

Zie [automatische afstemming van e-mailmeldingen](sql-database-automatic-tuning-email-notifications.md) handleiding.

## <a name="next-steps"></a>Volgende stappen
* Lees de [automatisch afstemmen artikel](sql-database-automatic-tuning.md) voor meer informatie over automatisch afstemmen en hoe u kunnen helpen de prestaties verbeteren.
* Zie [aanbevelingen voor prestaties](sql-database-advisor.md) voor een overzicht van Azure SQL Database-aanbevelingen voor prestaties.
* Zie [Query prestatie-inzichten](sql-database-query-performance.md) voor meer informatie over het weergeven van de invloed op de prestaties van uw belangrijkste query's.
