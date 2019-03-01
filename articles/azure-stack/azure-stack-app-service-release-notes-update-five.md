---
title: App Service in Azure Stack update 5 opmerkingen bij de release | Microsoft Docs
description: Meer informatie over wat er in update 5 voor App Service in Azure Stack, de bekende problemen en het downloaden van de update.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: 0a0eb9586e78442947138831dd774298906aaf9c
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56993081"
---
# <a name="app-service-on-azure-stack-update-5-release-notes"></a>App Service op de opmerkingen bij de release van de Azure Stack update 5

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en oplossingen in Azure App Service op Azure Stack Update 5 en bekende problemen. Bekende problemen zijn onderverdeeld in problemen direct verband houden met de implementatie, het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]
> De update 1901 toepassen op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack development kit implementeren voordat u Azure App Service 1.5 implementeert.


## <a name="build-reference"></a>Naslaginformatie over bouwen

De App Service op Azure Stack Update 5 build-nummer is **80.0.2.15**

### <a name="prerequisites"></a>Vereisten

Raadpleeg de [voordat u aan de slag-documentatie](azure-stack-app-service-before-you-get-started.md) voordat u begint met implementeren.

Voordat u de upgrade van Azure App Service in Azure Stack voor 1.5:

- Zorg ervoor dat alle rollen zijn gereed in het Azure App Service-beheer in de Azure Stack-beheerportal

- Back-up van de App-Service en Master Databases:
  - AppService_Hosting;
  - AppService_Metering;
  - Model

- Back-up van de bestandsshare voor Tenant-App-inhoud

- Extern publiceren de **Custom Script Extension** versie **1.9.1** vanuit de Marketplace

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Azure App Service onder Azure Stack Update 5 bevat de volgende verbeteringen en oplossingen:

- Updates voor **App Service-Tenant, Admin, portals van functies en hulpprogramma's voor Kudu**. Consistent zijn met Azure Stack-Portal SDK-versie.

- Updates voor **Kudu-hulpprogramma's** voor het oplossen van problemen met de vormgeving en functionaliteit voor klanten die **verbroken** Azure Stack. 

- Updates voor core-service om de betrouwbaarheid en foutberichten eenvoudiger diagnose van algemene problemen waardoor te verbeteren.

- **Updates voor de volgende App-frameworks en hulpmiddelen**:
  - Toegevoegde NodeJS 10.14.1
  - Toegevoegde NPM 6.4.1
  - Bijgewerkte Kudu op 79.20129.3767
  
- **Updates voor het onderliggende besturingssysteem dat van alle rollen**:
  - [2019-02 Cumulatieve Update voor WindowsServer 2016 voor x64 64-systemen (KB4487006)](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006)

### <a name="post-deployment-steps"></a>Stappen na de implementatie

> [!IMPORTANT]  
> Als u hebt opgegeven dat de RP-App Service met een SQL Always On-exemplaar moet u [de appservice_hosting en appservice_metering databases toevoegen aan een beschikbaarheidsgroep](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) en synchroniseren van de databases om te voorkomen dat verlies van de service in de gebeurtenis van een databasefailover.

### <a name="post-update-steps"></a>Stappen na het bijwerken

Voor klanten die willen migreren naar een ingesloten database voor bestaande Azure App Service op Azure Stack-implementaties, voert u deze stappen uit nadat de Azure App Service op Azure Stack 1.5-update is voltooid:

> [!IMPORTANT]
> De migratieprocedure duurt ongeveer 5-10 minuten.  De procedure omvat het beëindigen van de bestaande database-aanmeldingssessies.  Plan voor uitval migreren en valideren van Azure App Service op Azure Stack nadat de migratie.  Deze stappen zijn niet vereist als u deze stappen hebt voltooid na het bijwerken van Azure App service onder Azure Stack 1.3.
>
>

1. Voeg [AppService-databases (appservice_hosting en appservice_metering) aan een beschikbaarheidsgroep toevoegen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)

1. Database inschakelen die zijn opgenomen
    ```sql

        sp_configure 'contained database authentication', 1;
        GO
        RECONFIGURE;
            GO
    ```

1. Omzetten in een Database die gedeeltelijk zijn opgenomen, de conversie wordt in rekening gebracht downtime als alle actieve sessies moeten worden verwijderd

    ```sql
        /******** [appservice_metering] Migration Start********/
            USE [master];

            -- kill all active sessions
            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_metering')

            EXEC(@kill);

            USE [master]  
            GO  
            ALTER DATABASE [appservice_metering] SET CONTAINMENT = PARTIAL  
            GO  

        /********[appservice_metering] Migration End********/

        /********[appservice_hosting] Migration Start********/

            -- kill all active sessions
            USE [master];

            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_hosting')

            EXEC(@kill);

            -- Convert database to contained
            USE [master]  
            GO  
            ALTER DATABASE [appservice_hosting] SET CONTAINMENT = PARTIAL  
            GO  

            /********[appservice_hosting] Migration End********/
    '''

1. Migrate Logins to Contained Database Users

    ```sql
        IF EXISTS(SELECT * FROM sys.databases WHERE Name=DB_NAME() AND containment = 1)
        BEGIN
        DECLARE @username sysname ;  
        DECLARE user_cursor CURSOR  
        FOR
            SELECT dp.name
            FROM sys.database_principals AS dp  
            JOIN sys.server_principals AS sp
                ON dp.sid = sp.sid  
                WHERE dp.authentication_type = 1 AND dp.name NOT IN ('dbo','sys','guest','INFORMATION_SCHEMA');
            OPEN user_cursor  
            FETCH NEXT FROM user_cursor INTO @username  
                WHILE @@FETCH_STATUS = 0  
                BEGIN  
                    EXECUTE sp_migrate_user_to_contained
                    @username = @username,  
                    @rename = N'copy_login_name',  
                    @disablelogin = N'do_not_disable_login';  
                FETCH NEXT FROM user_cursor INTO @username  
            END  
            CLOSE user_cursor ;  
            DEALLOCATE user_cursor ;
            END
        GO
    ```

Valideren

1. Controleer of SQL Server containment ingeschakeld heeft

    ```sql
        sp_configure  @configname='contained database authentication'
    ```

1. Bestaande ingesloten gedrag controleren
    ```sql
        SELECT containment FROM sys.databases WHERE NAME LIKE (SELECT DB_NAME())
    ```

### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

- Werknemers kunnen geen bestandsserver bereiken wanneer App Service is geïmplementeerd in een bestaand virtueel netwerk en de bestandsserver alleen beschikbaar in het particuliere netwerk, is zoals beschreven in de Azure App Service op de documentatie over Azure Stack-implementatie.

Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres verbinding maken met de bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen de worker-subnet en de bestandsserver aan te geven. Ga naar de WorkersNsg in de beheerportal en voeg een uitgaande beveiligingsregel met de volgende eigenschappen:
 * Bron: Alle
 * Poortbereik van bron: *
 * Bestemming: IP-adressen
 * Doel-IP-adresbereik: Bereik van IP-adressen voor uw bestandsserver
 * Poortbereik van doel: 445
 * Protocol: TCP
 * Actie: Toestaan
 * Prioriteit: 700
 * Naam: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure App Service in Azure Stack

Raadpleeg de documentatie in de [opmerkingen bij de Release van Azure Stack 1809](azure-stack-update-1809.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure App Service, [Azure App Service op Azure Stack-overzicht](azure-stack-app-service-overview.md).
- Zie voor meer informatie over het voorbereiden van het App Service implementeren in Azure Stack [voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md).
