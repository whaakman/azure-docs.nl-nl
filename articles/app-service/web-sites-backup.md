---
title: Back-up maken van uw app in Azure
description: Informatie over het maken van back-ups van uw apps in Azure App Service.
services: app-service
documentationcenter: 
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
ms.openlocfilehash: 435370a8758d439a5fcce2e04efd11b4aaaf0357
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="back-up-your-app-in-azure"></a>Back-up maken van uw app in Azure
De functie back-up en herstel in [Azure App Service](app-service-web-overview.md) kunt u eenvoudig back-ups van app handmatig of volgens een planning aan te maken. U kunt de app naar een momentopname van een eerdere status herstellen door het overschrijven van de bestaande app of het herstellen naar een andere app. 

Zie voor informatie over het herstellen van een app van back-up, [herstellen van een app in Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Wat wordt een back-up
App Service kunt back-up van de volgende informatie naar een Azure-opslagaccount en container die u hebt geconfigureerd dat uw app te gebruiken. 

* App-configuratie
* Bestandsinhoud
* Database verbonden met uw app

De volgende databaseoplossingen worden met de functie back-ups ondersteund: 
   - [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
   - [Azure-Database voor MySQL (Preview)](https://azure.microsoft.com/en-us/services/mysql)
   - [Azure-Database voor PostgreSQL (Preview)](https://azure.microsoft.com/en-us/services/postgres)
   - [MySQL in-app](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Elke back-up is een volledige offline kopie van uw app, niet een incrementele update.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Vereisten en beperkingen
* De functie back-up en terugzetten vereist het App Service-abonnement in de **standaard** laag of **Premium** laag. Zie voor meer informatie over het schalen van uw App Service-abonnement te gebruiken van een hogere laag [een app in Azure opschalen](web-sites-scale.md).  
  **Premium** laag kan een groter aantal dagelijks back-ups dan **standaard** laag.
* U moet een Azure-opslagaccount en container in hetzelfde abonnement als de app die u back wilt-up. Zie voor meer informatie over Azure storage-accounts, het [koppelingen](#moreaboutstorage) aan het einde van dit artikel.
* Back-ups mag maximaal 10 GB-app en database-inhoud. Als de back-upgrootte deze limiet overschrijdt, krijgt u een fout opgetreden.

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>Een handmatige back-up maken
1. In de [Azure-portal](https://portal.azure.com), gaat u naar de pagina van uw app, selecteer **back-ups**. De **back-ups** pagina wordt weergegeven.
   
    ![Back-ups pagina][ChooseBackupsPage]
   
   > [!NOTE]
   > Als u het volgende bericht ziet, klikt u op om uw App Service-abonnement upgraden voordat u kunt doorgaan met de back-ups.
   > Zie voor meer informatie [een app in Azure opschalen](web-sites-scale.md).  
   > ![Opslagaccount kiezen](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. In de **back-up** pagina, klikt u op **configureren**
![klikt u op configureren](./media/web-sites-backup/ClickConfigure1.png)
3. In de **back-upconfiguratie** pagina, klikt u op **opslag: niet geconfigureerd** voor het configureren van een opslagaccount.
   
    ![Opslagaccount kiezen][ChooseStorageAccount]
4. Kies uw back-upbestemming door te selecteren een **Opslagaccount** en **Container**. Het opslagaccount moet behoren tot hetzelfde abonnement als de app die u back wilt-up. Als u wenst, kunt u een nieuw opslagaccount of een nieuwe container maken in de desbetreffende pagina's. Wanneer u bent klaar, klikt u op **Selecteer**.
   
    ![Opslagaccount kiezen](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. In de **back-upconfiguratie** pagina die nog steeds open blijft, kunt u configureren **Backup Database**, vervolgens selecteert u de databases die u wilt opnemen in de back-ups (SQL-database of MySQL) en klik vervolgens op **OK**.  
   
    ![Opslagaccount kiezen](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Voor een database worden weergegeven in deze lijst, de verbindingsreeks moet aanwezig zijn in de **verbindingsreeksen** sectie van de **toepassingsinstellingen** pagina voor uw app.
   > 
   > 
6. In de **back-upconfiguratie** pagina, klikt u op **opslaan**.    
7. In de **back-ups** pagina, klikt u op **back-up**.
   
    ![Knop BackUpNow][BackUpNow]
   
    U ziet een bericht uitgevoerd tijdens de back-upproces.

Zodra het opslagaccount en container is geconfigureerd, kunt u een handmatige back-up op elk gewenst moment kunt starten.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Automatische back-ups configureren
1. In de **back-upconfiguratie** pagina **geplande back-up** naar **op**. 
   
    ![Opslagaccount kiezen](./media/web-sites-backup/05ScheduleBackup1.png)
2. Back-upschema opties wordt weergegeven, stel **geplande back-** naar **op**, configureer de gewenste back-upschema en klikt u op **OK**.
   
    ![Automatische back-ups inschakelen][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Gedeeltelijke back-ups configureren
Soms wilt u geen back-up alles op uw app. Enkele voorbeelden:

* U [wekelijkse back-ups instellen](web-sites-backup.md#configure-automated-backups) van uw app met statische inhoud, die nooit wordt gewijzigd, zoals oude blogberichten of installatiekopieën.
* Uw app heeft meer dan 10 GB aan inhoud (dat wil zeggen de maximale hoeveelheid die u kunt back-up op een tijdstip).
* U wilt niet dat back-up van de logboekbestanden.

Gedeeltelijke back-ups kunnen dat u precies welke bestanden die u back wilt-up kiezen.

### <a name="exclude-files-from-your-backup"></a>Bestanden uitsluiten van uw back-up
Stel dat u hebt een app met logboekbestanden en statische afbeeldingen die zijn back-up eenmaal en gaat niet wijzigen. In dergelijke gevallen kunt u de mappen en bestanden uitsluiten van wordt opgeslagen in uw toekomstige back-ups. Als u wilt uitsluiten van bestanden en mappen van uw back-ups, maakt een `_backup.filter` bestand de `D:\home\site\wwwroot` map van uw app. Geef de lijst van bestanden en mappen die u wilt uitsluiten in dit bestand. 

Een eenvoudige manier om toegang tot uw bestanden is het gebruik van Kudu. Klik op **geavanceerde hulpprogramma's -> Ga** instellen voor uw web-app voor toegang tot Kudu.

![Kudu met portal][kudu-portal]

Identificeer de mappen die u wilt uitsluiten van uw back-ups.  Bijvoorbeeld, wilt u filteren van de geselecteerde map en bestanden.

![Map installatiekopieën][ImagesFolder]

Maken van een bestand met de naam `_backup.filter` en de voorgaande lijst plaatsen in het bestand, maar Verwijder `D:\home`. Lijst van een directory of bestand per regel. De inhoud van het bestand moet dus:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Uploaden `_backup.filter` van het bestand in de `D:\home\site\wwwroot\` map van uw site met [ftp](app-service-deploy-ftp.md) of een andere methode. Als u wenst, kunt u het bestand rechtstreeks met Kudu `DebugConsole` en voeg de inhoud bevat.

Back-ups uitvoeren dezelfde manier als u normaal doet, [handmatig](#create-a-manual-backup) of [automatisch](#configure-automated-backups). Nu, worden bestanden en mappen die zijn opgegeven in `_backup.filter` is uitgesloten van de toekomstige back-ups gepland of handmatig is gestart. 

> [!NOTE]
> U herstelt gedeeltelijke back-ups van uw site dezelfde manier als u doet [een regelmatige back-up terugzetten](web-sites-restore.md). Het herstelproces komt goede.
> 
> Wanneer een volledige back-up wordt hersteld, wordt alle inhoud op de site vervangen door de gewenste bevindt zich in de back-up. Als een bestand op de site, maar niet in de back-up is, wordt deze verwijderd. Maar wanneer een gedeeltelijke back-up wordt hersteld, inhoud die zich in een van de zwarte lijst mappen of een zwarte lijst bestand is ongewijzigd.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Hoe de back-ups worden opgeslagen
Nadat u een of meer back-ups voor uw app gemaakt hebt, de back-ups worden weergegeven op de **Containers** pagina van uw storage-account en uw app. In het opslagaccount wordt elke back-up bestaat uit een`.zip` -bestand met de back-upgegevens en een `.xml` -bestand met een manifest van de `.zip` bestand inhoud. U kunt uitpakken en deze bestanden bladeren als u toegang krijgen tot uw back-ups wilt zonder het daadwerkelijk uitvoeren van een toepassing herstellen.

De databaseback-up voor de app wordt opgeslagen in de hoofdmap van het ZIP-bestand. Voor een SQL-database is een Bacpac-bestand (zonder extensie) en kunnen worden geïmporteerd. Zie het maken van een SQL-database op basis van de export Bacpac- [een Bacpac-bestand voor het maken van een nieuwe gebruiker-Database importeren](http://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Wijzigen van de bestanden in uw **websitebackups** container kan leiden tot de back-up worden ongeldig en worden daarom niet-terug te zetten.
> 
> 

## <a name="automate-with-scripts"></a>Automatiseren met behulp van scripts

U kunt back-beheer met behulp van scripts, automatiseren met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

Raadpleeg voor voorbeelden:

- [Azure CLI-voorbeelden](app-service-cli-samples.md)
- [Azure PowerShell-voorbeelden](app-service-powershell-samples.md)

<a name="nextsteps"></a>

## <a name="next-steps"></a>Volgende stappen
Zie voor informatie over het herstellen van een app van een back-up, [herstellen van een app in Azure](web-sites-restore.md). 


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

