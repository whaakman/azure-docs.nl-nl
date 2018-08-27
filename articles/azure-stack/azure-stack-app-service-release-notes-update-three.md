---
title: App Service in Azure Stack update 3 opmerkingen bij de release | Microsoft Docs
description: Meer informatie over wat er in de update 3 voor App Service in Azure Stack, de bekende problemen en het downloaden van de update.
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
ms.date: 08/20/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: f825a2a343d9b5ad8f9802042b7aca2ba1544dfb
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42917399"
---
# <a name="app-service-on-azure-stack-update-3-release-notes"></a>App Service op de opmerkingen bij de release van de Azure Stack update 3

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en oplossingen in Azure App Service op Azure Stack Update 3 en bekende problemen. Bekende problemen zijn onderverdeeld in problemen direct verband houden met de implementatie, het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]
> De update 1807 toepassen op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack development kit implementeren voordat u Azure App Service 1.3 implementeert.
>
>

## <a name="build-reference"></a>Naslaginformatie over bouwen

De App Service op Azure Stack Update 3 build-nummer is **74.0.13698.31**

### <a name="prerequisites"></a>Vereiste onderdelen

Raadpleeg de [voordat u aan de slag-documentatie](azure-stack-app-service-before-you-get-started.md) voordat u begint met implementeren.

Voordat u de upgrade van Azure App Service in Azure Stack voor 1.3, zorg ervoor dat alle rollen zijn gereed in het Azure App Service-beheer in de Azure Stack-beheerportal

![Status van de functie App Service](media/azure-stack-app-service-release-notes-update-three/image01.png)

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Azure App Service onder Azure Stack Update 3 bevat de volgende verbeteringen en oplossingen:

- Ondersteuning voor het gebruik van SQL Server Always On voor de Resourceprovider van Azure App Service-databases.

- Nieuwe omgeving-parameter toegevoegd aan het script Create-AADIdentityApp helper om te helpen doelitems verschillende regio's met AAD.

- Updates voor **App Service-Tenant, Admin, portals van functies en hulpprogramma's voor Kudu**. Consistent zijn met Azure Stack-Portal SDK-versie.

- Updates voor core-service om de betrouwbaarheid en foutberichten eenvoudiger diagnose van algemene problemen waardoor te verbeteren.

- **Updates voor de volgende App-frameworks en hulpmiddelen**:
  - Toegevoegde ASP.Net Core 2.1.2
  - Toegevoegde NodeJS 10.0.0
  - Zulu OpenJDK 8.30.0.1 toegevoegd
  - Toegevoegde Tomcat 8.5.31 en 9.0.8
  - Toegevoegde PHP-versies:
    - 5.6.36
    - 7.0.30
    - 7.1.17
    - 7.2.5
  - Toegevoegde Wincache 2.0.0.8
  - Bijgewerkte Git voor Windows v 2.17.1.2
  - Bijgewerkte Kudu op 74.10611.3437
  
- **Updates voor het onderliggende besturingssysteem dat van alle rollen**:
  - [Servicing stack update voor Windows Server 2016 voor x64 64-systemen (KB4132216)](https://support.microsoft.com/help/4132216/servicing-stack-update-for-windows-10-1607-may-17-2018)
  - [2018-07 cumulatieve Update voor WindowsServer 2016 voor x64 64-systemen (KB4338822)](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)

### <a name="post-update-steps-optional"></a>Boeken updatestappen (optioneel)

Voor klanten die willen migreren naar een ingesloten database voor bestaande Azure App Service op Azure Stack-implementaties, voert u deze stappen uit nadat de Azure App Service op Azure Stack 1.3 update is voltooid:

> [!IMPORTANT]
> Deze procedure duurt ongeveer 5-10 minuten.  Deze procedure omvat het beëindigen van de bestaande database-aanmeldingssessies.  Uitvaltijd te migreren en valideren van Azure App Service op Azure Stack nadat de migratie plannen
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

1. Converteren van een Database op die gedeeltelijk zijn opgenomen.  Deze stap wordt in rekening gebracht downtime als alle actieve sessies moeten worden verwijderd

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

- Werknemers kunnen geen bestandsserver bereiken wanneer App Service is geïmplementeerd in een bestaand virtueel netwerk en de bestandsserver alleen beschikbaar op het particuliere netwerk is.  Dit is ook genoemd in de Azure App Service op de documentatie over Azure Stack-implementatie.

Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres verbinding maken met de bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen de worker-subnet en de bestandsserver aan te geven. Om dit te doen, gaat u naar de WorkersNsg in de beheerportal en voeg een uitgaande beveiligingsregel met de volgende eigenschappen toe:
 * Bron:
 * Poortbereik van bron: *
 * Bestemming: IP-adressen
 * Doel-IP-adresbereik: bereik van IP-adressen voor uw bestandsserver
 * Poortbereik van doel: 445
 * Protocol: TCP
 * Actie: toestaan
 * Prioriteit: 700
 * Naam: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure App Service in Azure Stack

Raadpleeg de documentatie in de [opmerkingen bij de Release van Azure Stack 1807](azure-stack-update-1807.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure App Service, [Azure App Service op Azure Stack-overzicht](azure-stack-app-service-overview.md).
- Zie voor meer informatie over het voorbereiden van het App Service implementeren in Azure Stack [voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md).
