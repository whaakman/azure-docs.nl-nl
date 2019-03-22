---
title: Herstellen van App Service op Azure Stack | Microsoft Docs
description: Gedetailleerde richtlijnen voor noodherstel van Azure Stack App Service
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: jeffgilb
ms.reviewer: apwestgarth
ms.lastreviewed: 03/21/2019
ms.openlocfilehash: b034259dde817c863d976384b08da17983ed9de7
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340241"
---
# <a name="recovery-of-app-service-on-azure-stack"></a>Herstel van App Service op Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*  

Dit document bevat instructies over acties te ondernemen voor noodherstel van App Service.

De volgende acties moeten worden genomen om de App Service in Azure Stack herstellen vanuit back-up:
1.  De App Service-databases herstellen
2.  De inhoud van het bestand server share herstellen
3.  Herstellen van App Service-functies en services

Als Azure Stack-opslag is gebruikt voor de opslag van de functie-Apps, moet u ook stappen voor het herstellen van de functie-Apps uitvoeren.

## <a name="restore-the-app-service-databases"></a>De App Service-databases herstellen
De App Service SQL Server-databases moeten worden hersteld op een productie gereed SQL Server-exemplaar. 

Na [voorbereiden van de SQL Server-exemplaar](azure-stack-app-service-before-you-get-started.md#prepare-the-sql-server-instance) voor het hosten van de App Service-databases, kunt u deze stappen gebruiken voor het terugzetten van back-up:

1. Meld u aan met de SQL-Server die als host voor de herstelde App Service-databases met beheerdersmachtigingen fungeert.
2. Gebruik de volgende opdrachten in de App Service-databases herstellen vanaf een opdrachtprompt met beheerdersbevoegdheden:
    ```dos
    sqlcmd -U <SQL admin login> -P <SQL admin password> -Q "RESTORE DATABASE appservice_hosting FROM DISK='<full path to backup>' WITH REPLACE"
    sqlcmd -U <SQL admin login> -P <SQL admin password> -Q "RESTORE DATABASE appservice_metering FROM DISK='<full path to backup>' WITH REPLACE"
    ```
3. Controleer of beide databases voor App Service is hersteld en SQL Server Management Studio sluiten.

> [!NOTE]
> Om te herstellen van een storing in failover cluster exemplaar, [deze instructies](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure?view=sql-server-2017). 

## <a name="restore-the-app-service-file-share-content"></a>Herstellen van de share-inhoud van het App Service-bestand
Na [voorbereiden van de bestandsserver](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server) voor het hosten van de App Service-bestandsshare, moet u de share-inhoud van het tenant-bestand herstellen vanuit back-up. U kunt de methode die de bestanden te kopiëren naar de zojuist gemaakte App Service-bestandssharelocatie beschikbaar zijn. In dit voorbeeld wordt uitgevoerd op de bestandsserver gebruikt PowerShell en robocopy verbinding maken met een externe share en kopieer de bestanden naar de share:

```powershell
$source = "<remote backup storage share location>"
$destination = "<local file share location>"
net use $source /user:<account to use to connect to the remote share in the format of domain\username> *
robocopy /E $source $destination
net use $source /delete
```

Na het kopiëren van de inhoud van het bestand delen, moet u machtigingen voor de bestandsshare zelf ook opnieuw instellen. U doet dit door open een administratieve opdrachtprompt op de computer met het bestand en voer de **ReACL.cmd** bestand. De **ReACL.cmd** bestand bevindt zich in de installatiebestanden van App Service in de **BCDR** directory.

## <a name="restore-app-service-roles-and-services"></a>Herstellen van App Service-functies en services
Nadat de App Service-databases en de inhoud van share hebt hersteld, moet u naast PowerShell gebruiken voor het herstellen van de App Service-functies en services. Deze stappen worden hersteld geheimen van de App Service en -configuraties.  

1. Meld u aan bij de App Service-controller **CN0 VM** VM als **roleadmin** met het wachtwoord die u hebt opgegeven tijdens de installatie van App Service. 
    > [!TIP]
    > U moet de netwerkbeveiligingsgroep zodat RDP-verbindingen van de virtuele machine wijzigen. 
2. Kopieer de **SystemSecrets.JSON** bestand lokaal op de virtuele machine van de controller. U moet het pad naar dit bestand op als de `$pathToExportedSecretFile` parameter in de volgende stap. 
3. Gebruik de volgende opdrachten in een PowerShell-consolevenster met verhoogde bevoegdheden om terug te zetten van App Service-functies en services:

    ```powershell
    # Stop App Service services on the primary controller VM
    net stop WebFarmService
    net stop ResourceMetering
    net stop HostingVssService # This service was deprecated in the App Service 1.5 release and is not required after the App Service 1.4 release.

    # Restore App Service secrets. Provide the path to the App Service secrets file copied from backup. For example, C:\temp\SystemSecrets.json.
    # Press ENTER when prompted to reconfigure App Service from backup 

    # If necessary, use -OverrideDatabaseServer <restored server> with Restore-AppServiceStamp when the restored database server has a different address than backed-up deployment.
    # If necessary, use -OverrideContentShare <restored file share path> with Restore-AppServiceStamp when the restored file share has a different path from backed-up deployment.
    Restore-AppServiceStamp -FilePath $pathToExportedSecretFile 

    # Restore App Service roles
    Restore-AppServiceRoles

    # Restart App Service services
    net start WebFarmService
    net start ResourceMetering
    net start HostingVssService  # This service was deprecated in the App Service 1.5 release and is not required after the App Service 1.4 release.

    # After App Service has successfully restarted, and at least one management server is in ready state, synchronize App Service objects to complete the restore
    # Enter Y when prompted to get all sites and again for all ServerFarm entities.
    Get-AppServiceSite | Sync-AppServiceObject
    Get-AppServiceServerFarm | Sync-AppServiceObject
    ```

> [!TIP]
> Het is raadzaam deze PowerShell-sessie wanneer de opdracht is voltooid.

## <a name="restore-function-apps"></a>Herstellen van de functie-Apps 
App Service voor Azure Stack biedt geen ondersteuning voor het herstellen van tenant gebruiker apps of andere gegevens dan inhoud delen. Daarom deze moeten worden back-up gemaakt en hersteld buiten de App Service back-up en herstelbewerkingen. Als Azure Stack-opslag is gebruikt voor de opslag van de functie-Apps, moeten de volgende stappen worden genomen om verlies van gegevens herstellen:

1. Maak een nieuw opslagaccount dat moet worden gebruikt door de functie-App. Deze opslag kan worden Azure Stack-opslag, Azure storage of alle compatibele opslag.
2. Haal de verbindingsreeks voor de opslag.
3. Open de functie-portal, blader naar de functie-app.
4. Blader naar de **platformfuncties** tabblad en klik op **toepassingsinstellingen**.
5. Wijziging **AzureWebJobsDashboard** en **AzureWebJobsStorage** naar de nieuwe verbindingsreeks, klik op **opslaan**.
6. Schakel over naar **overzicht**.
7. De app opnieuw starten. Het kan aantal pogingen om te wissen van alle fouten duren.

## <a name="next-steps"></a>Volgende stappen
[App Service op Azure Stack-overzicht](azure-stack-app-service-overview.md)