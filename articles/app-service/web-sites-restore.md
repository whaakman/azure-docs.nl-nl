---
title: App - Azure App Service herstellen
description: Leer hoe u uw app terugzetten vanuit een back-up.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 603a6bab75c4a5da4425bcc57d2cc5ccda728636
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274116"
---
# <a name="restore-an-app-in-azure"></a>Een app in Azure herstellen
Dit artikel leest u hoe u een app in herstellen [Azure App Service](../app-service/app-service-web-overview.md) die u eerder hebt back-up (Zie [maakt u een Back-up van uw app in Azure](web-sites-backup.md)). U kunt uw app met de daaraan gekoppelde databases op verzoek herstellen naar een vorige versie, of een nieuwe app maken op basis van back-ups van uw oorspronkelijke app. Azure App Service ondersteunt de volgende databases voor back-up en herstel:
- [SQL Database](https://azure.microsoft.com/services/sql-database/)
- [Azure Database for MySQL](https://azure.microsoft.com/services/mysql)
- [Azure Database for PostgreSQL](https://azure.microsoft.com/services/postgresql)
- [MySQL in-app](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Terugzetten vanaf een back-ups is beschikbaar voor apps die worden uitgevoerd in **Standard** en **Premium** laag. Zie voor meer informatie over het omhoog schalen van uw app [opschalen van een app in Azure](web-sites-scale.md). **Premium** laag kunt u een groter aantal dagelijkse back-ups moeten worden uitgevoerd dan **Standard** laag.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Een app herstellen vanuit een bestaande back-up
1. Op de **instellingen** pagina van uw app in Azure portal, klikt u op **back-ups** om weer te geven de **back-ups** pagina. Klik vervolgens op **herstellen**.
   
    ![Klik op nu herstellen][ChooseRestoreNow]
2. In de **herstellen** pagina, selecteert u eerst de back-bron.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    De **back-up App** optie ziet u alle bestaande back-ups van de huidige app en kunt u een eenvoudig selecteren.
    De **opslag** optie kunt u een back-ZIP-bestand van een bestaande Azure-opslagaccount en container in uw abonnement selecteren.
    Als u probeert te herstellen van een back-up van een andere app, gebruikt u de **opslag** optie.
3. Geef vervolgens de bestemming voor het herstellen van de app in **hersteldoel**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Als u ervoor kiest **overschrijven**, worden alle bestaande gegevens in uw huidige app worden gewist en overschreven. Voordat u klikt op **OK**, zorg ervoor dat het is precies wat u wilt doen.
   > 
   > 
   
   > [!WARNING]
   > Als de App-Service gegevens naar de database schrijft terwijl u deze wilt herstellen, kan dit problemen zoals schending van de primaire sleutel en verlies van gegevens leiden. Het wordt aangeraden eerst de App-Service stoppen voordat u begint met het herstellen van de database.
   > 
   > 
   
    U kunt selecteren **bestaande App** om te herstellen van back-up van de app naar een andere app in dezelfde resourcegroep bevinden. Voordat u deze optie gebruikt, moet hebt u al een andere app gemaakt in de resourcegroep met de databaseconfiguratie naar een gedefinieerd in de back-up van de app voor het spiegelen. U kunt ook maken een **nieuw** app naar uw inhoud te herstellen.

4. Klik op **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Downloaden of een back-up van een storage-account verwijderen
1. In het hoofdvenster **Bladeren** pagina van de Azure portal, selecteer **opslagaccounts**. Er wordt een lijst met uw bestaande opslagaccounts weergegeven.
2. Selecteer het opslagaccount waarin de back-up die u wilt downloaden of verwijderen. De pagina voor het opslagaccount dat wordt weergegeven.
3. Selecteer in de pagina van het opslagaccount, de container die u wilt
   
    ![Weergavecontainers][ViewContainers]
4. Selecteer de back-upbestand dat u wilt downloaden of te verwijderen.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Klik op **downloaden** of **verwijderen** , afhankelijk van wat u wilt doen.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Monitor een herstelbewerking uit
Voor informatie over het slagen of mislukken van de app-herstelbewerking, gaat u naar de **activiteitenlogboek** pagina in de Azure portal.  
 

Schuif omlaag naar de gewenste bewerking voor het herstellen en klik op om deze te selecteren.

De pagina met details worden de beschikbare informatie met betrekking tot de herstelbewerking opnieuw weergegeven.

## <a name="automate-with-scripts"></a>Automatiseren met scripts

U kunt back-upbeheer met scripts automatiseren met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

Zie voor voorbeelden:

- [Azure CLI-voorbeelden](app-service-cli-samples.md)
- [Voorbeelden van Azure PowerShell](app-service-powershell-samples.md)

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
