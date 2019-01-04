---
title: Back-up van app - Azure App Service
description: Informatie over het maken van back-ups van uw apps in Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 1491068248d20a917e28db5a75dee3d4b3753f5c
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731057"
---
# <a name="back-up-your-app-in-azure"></a>Back-up maken van uw app in Azure
De functie back-up en herstel in [Azure App Service](overview.md) kunt u eenvoudig back-ups maken, handmatig of volgens een schema. U kunt de app op een momentopname van een eerdere status herstellen door overschrijft de bestaande app of het herstellen naar een andere app. 

Zie voor meer informatie over het terugzetten van een app vanuit back-up [herstellen van een app in Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Wat wordt een back-up
App Service kunt back-up van de volgende informatie op een Azure storage-account en een container die u hebt geconfigureerd dat uw app te gebruiken. 

* App-configuratie
* Bestandsinhoud
* Database verbonden met uw app

De volgende databaseoplossingen worden ondersteund door back-up: 
   - [SQL Database](https://azure.microsoft.com/services/sql-database/)
   - [Azure Database for MySQL](https://azure.microsoft.com/services/mysql)
   - [Azure Database for PostgreSQL](https://azure.microsoft.com/services/postgresql)
   - [MySQL in-app](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Elke back-up is een complete offline kopie van uw app, niet een incrementele update.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Vereisten en beperkingen
* De functie back-up en terugzetten vereist het App Service-plan in de **Standard** laag of **Premium** laag. Zie voor meer informatie over het schalen van uw App Service-plan het gebruik van een hogere laag [opschalen van een app in Azure](web-sites-scale.md).  
  **Premium** laag kunt u een groter aantal dagelijks back-ups dan **Standard** laag.
* U moet een Azure storage-account en een container in hetzelfde abonnement als de app die u back wilt-up. Zie voor meer informatie over Azure storage-accounts, het [koppelingen](#moreaboutstorage) aan het einde van dit artikel.
* Back-ups mag maximaal 10 GB-app en database-inhoud. Als de back-upgrootte deze limiet overschrijdt, krijgt u een fout.
* Back-ups van SSL ingeschakeld Azure Database voor MySQL wordt niet ondersteund. Als een back-up is geconfigureerd, wordt er mislukte back-ups.
* Back-ups van SSL ingeschakeld Azure Database voor PostgreSQL wordt niet ondersteund. Als een back-up is geconfigureerd, wordt er mislukte back-ups.
* In-app MySQL-databases worden automatisch back-ups zonder configuratie. Als u handmatig de instellingen voor in-app-MySQL-databases, zoals het toevoegen van tekenreeksen voor databaseverbindingen, werkt de back-ups mogelijk niet correct.
* Storage-account met behulp van een firewall worden ingeschakeld als de bestemming voor uw back-ups wordt niet ondersteund. Als een back-up is geconfigureerd, wordt er mislukte back-ups.


<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>Een handmatige back-up maken
1. In de [Azure-portal](https://portal.azure.com), gaat u naar de pagina van uw app, selecteer **back-ups**. De **back-ups** pagina wordt weergegeven.
   
    ![Pagina van de back-ups][ChooseBackupsPage]
   
   > [!NOTE]
   > Als u het volgende bericht ziet, klikt u erop om te upgraden van uw App Service-plan voordat u verder kunt gaan met back-ups.
   > Zie voor meer informatie, [opschalen van een app in Azure](web-sites-scale.md).  
   > ![Opslagaccount kiezen](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. In de **back-up** pagina, klikt u op **configureren**
![klikt u op configureren](./media/web-sites-backup/ClickConfigure1.png)
3. In de **back-upconfiguratie** pagina, klikt u op **opslag: Niet geconfigureerd** het configureren van een storage-account.
   
    ![Opslagaccount kiezen][ChooseStorageAccount]
4. Kies uw back-upbestemming door het selecteren van een **Opslagaccount** en **Container**. Het opslagaccount moet behoren tot hetzelfde abonnement als de app die u wilt back-up. Als u wilt, kunt u een nieuw opslagaccount of een nieuwe container maken in de respectievelijke pagina's. Wanneer u klaar bent, klikt u op **Selecteer**.
   
    ![Opslagaccount kiezen](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. In de **back-upconfiguratie** pagina die nog steeds geopend blijft, kunt u configureren **Backup Database**, vervolgens selecteert u de databases die u wilt opnemen in de back-ups (SQL-database of MySQL) en klik vervolgens op **OK**.  
   
    ![Opslagaccount kiezen](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Voor een database in deze lijst wordt weergegeven, de verbindingsreeks moet aanwezig zijn in de **verbindingsreeksen** sectie van de **toepassingsinstellingen** pagina voor uw app. 
   >
   > In-app MySQL-databases worden automatisch back-ups zonder configuratie. Als u handmatig de instellingen voor in-app-MySQL-databases, zoals het toevoegen van tekenreeksen voor databaseverbindingen, werkt de back-ups mogelijk niet correct.
   > 
   > 
6. In de **back-upconfiguratie** pagina, klikt u op **opslaan**.    
7. In de **back-ups** pagina, klikt u op **back-up**.
   
    ![Knop BackUpNow][BackUpNow]
   
    U ziet een bericht wordt uitgevoerd tijdens de back-upproces.

Nadat het opslagaccount en container is geconfigureerd, kunt u een handmatige back-up starten op elk gewenst moment.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Automatische back-ups configureren
1. In de **back-upconfiguratie** pagina, stelt u **geplande back-up** naar **op**. 
   
    ![Opslagaccount kiezen](./media/web-sites-backup/05ScheduleBackup1.png)
2. Back-upschema opties worden weergegeven, instellen **geplande back-** naar **op**, vervolgens de back-upschema naar wens configureren en klik op **OK**.
   
    ![Automatische back-ups inschakelen][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Gedeeltelijke back-ups configureren
Soms wilt u geen back-up van alles in uw app. Enkele voorbeelden:

* U [wekelijkse back-ups instellen](#configure-automated-backups) van uw app met statische inhoud die nooit wordt gewijzigd, zoals oude blogberichten of installatiekopieën.
* Uw app heeft meer dan 10 GB aan inhoud (dat wil zeggen de maximale hoeveelheid die u kunt back-up op een tijdstip).
* U wilt niet dat back-up van de logboekbestanden.

Gedeeltelijke back-ups kunnen u precies welke u back wilt-up van bestanden.

### <a name="exclude-files-from-your-backup"></a>Bestanden uitsluiten van de back-up
Stel dat u hebt een app met logboekbestanden en statische afbeeldingen die één keer back-up zijn en niet meer wijzigen. In dergelijke gevallen kunt u de mappen en bestanden uitsluiten van wordt opgeslagen in uw toekomstige back-ups. Als u wilt uitsluiten van bestanden en mappen van uw back-ups, maakt u een `_backup.filter` -bestand in de `D:\home\site\wwwroot` map van uw app. Geef de lijst met bestanden en mappen die u wilt uitsluiten in dit bestand. 

Er is een eenvoudige manier om toegang tot uw bestanden met Kudu. Klik op **geavanceerde hulpprogramma's -> Go** instellen voor uw web-app voor toegang tot Kudu.

![Kudu met portal][kudu-portal]

Identificeer de mappen die u wilt uitsluiten van uw back-ups.  Bijvoorbeeld, wilt u de geselecteerde map en bestanden filteren.

![De afbeeldingenmap][ImagesFolder]

Maak een bestand met de naam `_backup.filter` en plaatst u de voorgaande lijst in het bestand, maar Verwijder `D:\home`. Lijst met één map of het bestand per regel. Daarom moet de inhoud van het bestand:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Uploaden `_backup.filter` van het bestand in de `D:\home\site\wwwroot\` map van uw site met [ftp](deploy-ftp.md) of een andere methode. Als u wilt, kunt u het bestand rechtstreeks met Kudu `DebugConsole` en voeg er de inhoud.

Back-ups uitvoeren de dezelfde manier als normaal, [handmatig](#create-a-manual-backup) of [automatisch](#configure-automated-backups). Nu, bestanden en mappen die zijn opgegeven in `_backup.filter` is uitgesloten van de toekomstige back-ups volgens de planning of handmatig wordt gestart. 

> [!NOTE]
> U een gedeeltelijke back-ups van uw site herstellen de dezelfde manier [een regelmatige back-up herstellen](web-sites-restore.md). Het herstelproces doet de juiste dingen.
> 
> Wanneer een volledige back-up wordt hersteld, wordt alle inhoud op de site vervangen door de wat in de back-up is. Als een bestand op de site, maar niet in de back-up is wordt het verwijderd. Maar wanneer een gedeeltelijke back-up wordt hersteld, alle inhoud die zich in een van de niet-toegestane mappen of een niet-toegestane bestand is ongewijzigd.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Hoe back-ups worden opgeslagen
Nadat u een of meer back-ups voor uw app gemaakt hebt, de back-ups worden weergegeven op de **Containers** pagina van uw opslagaccount en uw app. In de storage-account, wordt elke back-up bestaat uit een`.zip` -bestand met de back-upgegevens en een `.xml` -bestand met een manifest van de `.zip` bestandsinhoud. U kunt pakken en deze bestanden bladeren als u toegang krijgen tot uw back-ups wilt zonder daadwerkelijk uitvoeren van een app herstellen.

De databaseback-up voor de app wordt opgeslagen in de hoofdmap van het ZIP-bestand. Voor een SQL-database is een BACPAC-bestand (zonder bestandsextensie) en kunnen worden geïmporteerd. Zie voor het maken van een SQL-database op basis van de export BACPAC [een BACPAC-bestand voor het maken van een nieuwe gebruiker-Database importeren](https://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Het wijzigen van een van de bestanden in uw **websitebackups** container kan leiden tot de back-up worden ongeldig en daarom niet herstelbaar.
> 
> 

## <a name="automate-with-scripts"></a>Automatiseren met scripts

U kunt back-upbeheer met scripts automatiseren met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

Zie voor voorbeelden:

- [Azure CLI-voorbeelden](samples-cli.md)
- [Voorbeelden van Azure PowerShell](samples-powershell.md)

<a name="nextsteps"></a>

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het terugzetten van een app vanuit een back-up [herstellen van een app in Azure](web-sites-restore.md). 


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG

