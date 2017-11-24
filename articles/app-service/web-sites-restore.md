---
title: Een app in Azure herstellen
description: Informatie over het herstellen van uw app vanaf een back-up.
services: app-service
documentationcenter: 
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
ms.openlocfilehash: 60b55acaf687dae76e054b428b8d6584d8eb09f8
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="restore-an-app-in-azure"></a>Een app in Azure herstellen
Dit artikel laat zien hoe u een app in [Azure App Service](../app-service/app-service-web-overview.md) die u hebt eerder back-up gemaakt (Zie [Back-up van uw app in Azure](web-sites-backup.md)). U kunt uw app met de gekoppelde databases op aanvraag naar een eerdere status herstellen of een nieuwe app op basis van een back-ups van uw oorspronkelijke app maken. Azure App Service ondersteunt de volgende databases voor back-up en herstel:
- [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [Azure-Database voor MySQL (Preview)](https://azure.microsoft.com/en-us/services/mysql)
- [Azure-Database voor PostgreSQL (Preview)](https://azure.microsoft.com/en-us/services/postgres)
- [MySQL in-app](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Herstellen vanuit back-ups is beschikbaar voor apps die worden uitgevoerd **standaard** en **Premium** laag. Zie voor meer informatie over het schalen van uw app [een app in Azure opschalen](web-sites-scale.md). **Premium** laag kan een groter aantal dagelijkse back-ups worden uitgevoerd dan **standaard** laag.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Een app uit een bestaande back-up herstellen
1. Op de **instellingen** pagina van uw app in de Azure portal, klikt u op **back-ups** om weer te geven de **back-ups** pagina. Klik vervolgens op **herstellen**.
   
    ![Kies nu terugzetten][ChooseRestoreNow]
2. In de **herstellen** pagina, selecteert u eerst de back-bron.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    De **App back-up** optie ziet u alle de bestaande back-ups van de huidige app en u deze eenvoudig kunt selecteren.
    De **opslag** optie kunt u een back-ZIP-bestand van een bestaande Azure-opslagaccount en container in uw abonnement te selecteren.
    Als u probeert te herstellen van een back-up van een andere app, gebruikt u de **opslag** optie.
3. Geef het doel voor het herstellen van de app in **terugzetdoel**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Als u ervoor kiest **overschrijven**, worden alle bestaande gegevens in uw huidige app is gewist en overschreven. Voordat u op **OK**, zorg ervoor dat deze precies wat u wilt doen.
   > 
   > 
   
    U kunt selecteren **bestaande App** om te zetten back-up van de app op een andere app in dezelfde groep resoure. Voordat u deze optie gebruikt, moet hebt u al een andere app gemaakt in de resourcegroep met de databaseconfiguratie naar een gedefinieerd in de app back-up voor het spiegelen. U kunt ook maken een **nieuw** app uw inhoud te herstellen.

4. Klik op **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Downloaden of een back-up van een opslagaccount verwijderen
1. Vanuit het hoofdvenster **Bladeren** pagina van de Azure portal, selecteer **opslagaccounts**. Er wordt een lijst met uw bestaande opslagaccounts weergegeven.
2. Selecteer het opslagaccount waarin de back-up die u wilt downloaden of verwijderen. De pagina voor het opslagaccount wordt weergegeven.
3. Selecteer de container die u wilt dat in de pagina opslag account
   
    ![Weergavecontainers][ViewContainers]
4. Selecteer de back-upbestand dat u wilt downloaden of verwijderen.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Klik op **downloaden** of **verwijderen** afhankelijk van wat u wilt doen.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Een herstelbewerking bewaken
Voor informatie over het slagen of mislukken van de app-herstelbewerking, gaat u naar de **activiteitenlogboek** pagina in de Azure-portal.  
 

Schuif naar beneden om te zoeken naar de gewenste herstelbewerking en klikt u op om deze te selecteren.

De detailpagina geeft de beschikbare informatie met betrekking tot de herstelbewerking opnieuw.

## <a name="automate-with-scripts"></a>Automatiseren met behulp van scripts

U kunt back-beheer met behulp van scripts, automatiseren met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

Raadpleeg voor voorbeelden:

- [Azure CLI-voorbeelden](app-service-cli-samples.md)
- [Azure PowerShell-voorbeelden](app-service-powershell-samples.md)

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
