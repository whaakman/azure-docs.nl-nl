---
title: DMS gebruiken om te migreren naar Azure SQL Database Managed Instance | Microsoft Docs
description: Meer informatie over het migreren van on-premises SQL Server naar Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 07/12/2018
ms.openlocfilehash: c911b096af6662e11afb4c4262b92c239d252c36
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990224"
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>SQL Server migreren naar Azure SQL Database Managed Instance met behulp van DMS
U kunt de Azure Database Migration Service gebruiken voor het migreren van de databases van een on-premises SQL Server-exemplaar naar een [Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance.md). Voor extra methoden waarvoor handmatige moeite, Zie het artikel [SQL Server-exemplaar migratie naar Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-migrate.md).

> [!IMPORTANT]
> Migratieprojecten van SQL Server naar Azure SQL Database Managed Instance zijn in preview en zijn onderworpen aan de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In deze zelfstudie, migreert u de **Adventureworks2012** database uit een on-premises exemplaar van SQL Server naar een Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Maak een instantie van de Azure Database Migration Service.
> * Een migratieproject maken met behulp van de Azure Database Migration Service.
> * De migratie uitvoert.
> * De migratie te bewaken.
> * Een migratierapport downloaden.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie, moet u naar:

- Een VNET maken voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Netwerktopologieën voor migraties van Azure SQL database Managed Instance met behulp van de Azure Database Migration Service meer](https://aka.ms/dmsnetworkformi).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Network Security Group regels Doe het volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over Azure VNET NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows Firewall voor toegang tot de bron database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Open uw Windows-Firewall voor de Azure Database Migration Service voor toegang tot de bron-SQL-Server, die standaard TCP-poort 1433.
- Als u meerdere benoemde SQL Server-exemplaren met behulp van dynamische poorten uitvoert, kunt u desgewenst de SQL Browser-Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding met een benoemd exemplaar op de bron maken kan de server.
- Als u van een firewallapparaat vóór de brondatabases gebruikmaakt, moet u mogelijk om toe te voegen firewallregels om toe te staan de Azure Database Migration Service voor toegang tot de brondatabase (s) voor migratie, evenals de bestanden via SMB-poort 445.
- Een Azure SQL Database Managed Instance maken door het volgen van de details in het artikel [maken van een Azure SQL Database Managed Instance in de Azure portal](https://aka.ms/sqldbmi).
- Zorg ervoor dat de aanmeldingen dat is gebruikt voor het verbinding maken met de bron-SQL Server en de doelinstantie van beheerde leden van de serverrol sysadmin.
- Maak een netwerkshare die de Azure Database Migration Service gebruiken kunt om back-up van de brondatabase.
- Zorg ervoor dat de serviceaccount met de bron SQL Server-exemplaar heeft schrijfbevoegdheid op de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver voor lezen/schrijven toegang tot de dezelfde share heeft.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die het volledige beheer heeft op de netwerkshare die u eerder hebt gemaakt. Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar Azure storage-container voor de herstelbewerking.
- Een blob-container maken en de SAS-URI ophalen met behulp van de stappen in het artikel [beheren Azure Blob Storage-resources met Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), zorg ervoor dat u alle machtigingen (lezen, schrijven, verwijderen, lijst) in het venster terwijl voor beleid selecteren het maken van de SAS-URI. Deze details biedt de Azure Database Migration Service toegang tot de container van het opslagaccount voor het uploaden van de back-upbestanden die wordt gebruikt voor het migreren van databases naar Azure SQL Database Managed Instance

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure portal, selecteer **alle services**, en selecteer vervolgens **abonnementen**.

    ![Portal-abonnementen weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-subscriptions.png)        

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3. Zoekactie voor migratie en klikt u vervolgens aan de rechterkant van **Microsoft.DataMigration**, selecteer **registreren**.

    ![Resourceprovider registreren](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Database Migration Service-exemplaar maken

1. Selecteer in de Azure portal + **een resource maken**, zoeken naar **Azure Database Migration Service**, en selecteer vervolgens **Azure Database Migration Service** in de vervolgkeuzelijst de lijst.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2. Op de **Azure Database Migration Service** scherm, selecteer **maken**.

    ![Azure Database Migration Service-exemplaar maken](media\tutorial-sql-server-to-managed-instance\dms-create1.png)

3. Op de **migratieservice maken** scherm, Geef een naam voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer een bestaand virtueel netwerk (VNET) of maak een.
 
    Het VNET biedt de Azure Database Migration Service toegang tot de bron-SQL Server en het doel-Azure SQL Database Managed Instance.

    Zie het artikel voor meer informatie over het maken van een VNET in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

    Voor aanvullende informatie, Zie het artikel [netwerktopologieën voor migraties van Azure SQL database Managed Instance met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi).

5. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en Prijscategorieën, de [pagina met prijzen](https://aka.ms/dms-pricing).
   
    ![DMS-Service maken](media\tutorial-sql-server-to-managed-instance\dms-create-service1.png)

6.  Selecteer **maken** om de service te maken.

## <a name="create-a-migration-project"></a>Een migratieproject maken

Nadat de service is gemaakt, zoeken in Azure portal, opent u het en maak vervolgens een nieuw migratieproject voor.

1. Selecteer in de Azure portal, **alle services**, zoeken naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration service**.

    ![Zoeken naar alle exemplaren van de Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Op de **Azure Database Migration Service scherm**, zoekt u de naam van het exemplaar dat u hebt gemaakt en selecteer vervolgens het exemplaar.
 
3. Selecteer + **nieuw migratieproject**.

4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **bronservertype** tekstvak, selecteer **SQL Server**, en klik vervolgens in de **doel servertype** tekstvak, selecteer **Azure SQL Database Managed Instance**.

   ![DMS-Project maken](media\tutorial-sql-server-to-managed-instance\dms-create-project1.png)

5. Selecteer **maken** om het project te maken.

## <a name="specify-source-details"></a>Geef details van gegevensbron

1. Op de **details van de gegevensbron** scherm, geef de details van de verbinding voor de bron-SQL Server.

2. Als u een vertrouwd certificaat niet op uw server hebt geïnstalleerd, selecteert u de **servercertificaat vertrouwen** selectievakje.

    Wanneer een vertrouwd certificaat niet is geïnstalleerd, wordt SQL Server een zelfondertekend certificaat gegenereerd wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die zijn versleuteld met behulp van een zelfondertekend certificaat zorgt niet voor sterke beveiliging. Ze zijn vatbaar voor man-in-the-middle-aanvallen. U moet niet vertrouwen op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Details van gegevensbron](media\tutorial-sql-server-to-managed-instance\dms-source-details1.png)

3. Selecteer **Opslaan**.

4. Op de **de brondatabases selecteren** scherm, selecteert u de **Adventureworks2012** database voor migratie.

   ![De brondatabases selecteren](media\tutorial-sql-server-to-managed-instance\dms-source-database1.png)

5. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Geef Doeldetails op

1.  Op de **gericht op details** scherm, geef de details van de verbinding voor het doel is van de vooraf ingerichte Azure SQL Database Managed Instance waarop de **AdventureWorks2012** database moet worden gemigreerd.

    Als u de Azure SQL Database Managed Instance nog niet hebt ingericht, selecteert u **geen** voor een koppeling bij het inrichten van het exemplaar. U kunt nog steeds doorgaan met het maken van het project en vervolgens, wanneer de Azure SQL Database Managed Instance klaar bent is, Ga terug naar dit specifieke project voor het uitvoeren van de migratie.   
 
       ![Doel selecteren](media\tutorial-sql-server-to-managed-instance\dms-target-details1.png)

2.  Selecteer **Opslaan**.

3.  Op de **projectoverzicht** scherm, controleren en verifiëren van de gegevens die zijn gekoppeld aan het migratieproject.
 
    ![Migratieproject samenvatting](media\tutorial-sql-server-to-managed-instance\dms-project-summary1.png)

4.  Selecteer **Opslaan**.   

## <a name="run-the-migration"></a>De migratie uitvoeren

1.  Selecteer de laatst opgeslagen project, selecteer + **nieuwe activiteit**, en selecteer vervolgens **migratie uitvoeren**.

    ![Nieuwe activiteit maken](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity1.png)

2.  Wanneer u hierom wordt gevraagd, voert u de referenties van de bron- en doelservers en selecteer vervolgens **opslaan**.

3.  Op de **de brondatabases selecteren** scherm, selecteert u de brondatabase (s) die u wilt migreren.

    ![De brondatabases selecteren](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases2.png)

4.  Selecteer **opslaan**, en klik vervolgens op de **aanmeldingen selecteren** scherm, selecteert u de aanmeldingen die u wilt migreren.

    De huidige versie ondersteunt alleen migreren SQL-aanmeldingen.

    ![Aanmeldingen selecteren](media\tutorial-sql-server-to-managed-instance\dms-select-logins.png)

5. Selecteer **opslaan**, en klik vervolgens op de **migratie-instellingen configureren** scherm, vindt u de volgende informatie:

    | | |
    |--------|---------|
    |**Netwerklocatieshare** | Het lokale netwerk delen dat de Azure Database Migration Service kan duren voordat de bron databaseback-ups op. Het serviceaccount waarop SQL Server-bronexemplaar wordt uitgevoerd moet schrijfmachtigingen hebben op deze netwerkshare. Geef een FQDN-naam of IP-adressen van de server in de netwerkshare, bijvoorbeeld '\\\servername.domainname.com\backupfolder' of '\\\IP address\backupfolder'.|
    |**Gebruikersnaam** | De windows-gebruikersnaam die de Azure Database Migration Service kan imiteren en de back-bestanden naar Azure storage-container voor de herstelbewerking uploaden. |
    |**Wachtwoord** | Wachtwoord voor de gebruiker. |
    |**Opslagaccountinstellingen** | De SAS-URI die de Azure Database Migration Service toegang tot de container van het opslagaccount waarnaar de service wordt geüpload de back-up maken van bestanden en die wordt gebruikt voor het migreren van databases naar Azure SQL Database Managed Instance. [Informatie over het verkrijgen van de SAS-URI voor de blob-container](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Migratie-instellingen configureren](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings2.png)

5.  Selecteer **opslaan**, en klik vervolgens op de **overzicht Objectmigratie** scherm, in de **activiteitnaam** tekst, geeft u een naam voor de migratieactiviteit.

    ![Overzicht van migratie](media\tutorial-sql-server-to-managed-instance\dms-migration-summary2.png)

6. Vouw de **validatieoptie** sectie om weer te geven de **validatieoptie kiezen** scherm, Geef op of de gemigreerde database voor de juistheid van query valideren, en selecteer vervolgens **opslaan** .  

7. Selecteer **migratie uitvoeren**.

    Het venster van de activiteit migratie wordt weergegeven, en de status van de activiteit **in behandeling**.

## <a name="monitor-the-migration"></a>De migratie controleren

1. Selecteer op het scherm van de activiteit migratie **vernieuwen** naar de weergave bijwerken.
 
   ![Migratieactiviteit wordt uitgevoerd](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-in-progress.png)

2. U kunt de databases en aanmeldingen categorieën voor het controleren van de migratiestatus van de respectieve serverobjecten verder uitbreiden.

   ![Migratieactiviteit wordt uitgevoerd](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-monitor.png)

3. Nadat de migratie is voltooid, selecteert u **rapport downloaden** om op te halen van een rapport met gegevens die zijn gekoppeld aan het migratieproces.
 
4. Controleren of de doeldatabase op de doel-Azure SQL Database Managed Instance-omgeving.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een zelfstudie wordt uitgelegd hoe u een database migreren naar een beheerd exemplaar met de T-SQL terugzetten opdracht u [een back-up herstellen naar een beheerd exemplaar met behulp van de opdracht restore](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Zie voor meer informatie over Managed Instance [wat is een beheerd exemplaar](../sql-database/sql-database-managed-instance.md).
- Zie voor meer informatie over het verbinden van apps naar een beheerd exemplaar [toepassingen verbinden met](../sql-database/sql-database-managed-instance-connect-app.md).
