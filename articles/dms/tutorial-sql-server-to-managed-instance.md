---
title: Migreren naar Azure SQL Database Managed Instance met behulp van DMS | Microsoft Docs
description: Leer hoe u migreert van on-premises SQL Server naar Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service.
services: dms
author: edmacauley
ms.author: jtoland
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: dbf71b1fcc15743f4670c4072921f1a167a90e97
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887299"
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-offline-using-dms"></a>SQL Server offline migreren naar Azure SQL Database Managed Instance met behulp van DMS
U kunt de Azure Database Migration Service gebruiken om databases te migreren van een on-premises SQL Server-exemplaar naar een [beheerd exemplaar voor Azure SQL Database](../sql-database/sql-database-managed-instance.md). In het artikel [SQL Server-exemplaar migreren naar Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-migrate.md) vindt u aanvullende methoden, waarvoor enkele handmatige stappen nodig kunnen zijn.

> [!IMPORTANT]
> Migratieprojecten van SQL Server naar Azure SQL Database Managed Instance zijn in preview en zijn onderhevig aan de [aanvullende gebruiksvoorwaarden voor Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In deze zelfstudie migreert u de database **Adventureworks2012** van een on-premises exemplaar van SQL Server naar een beheerd exemplaar voor Azure SQL Database. Dit doet u met behulp van de Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> * Een migratieproject maken met behulp van de Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.
> * Een migratierapport downloaden.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Maak een VNET voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Lees hier meer over netwerktopologieën voor migraties van Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Network Security Group-regels de volgende communicatiepoorten niet blokkeren: 443, 53, 9354, 445, 12000. Zie voor meer informatie over verkeer filteren van Azure VNET NSG het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows-firewall voor toegang tot de engine van de brondatabase](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van SQL Server. Standaard verloopt dit via TCP-poort 1433.
- Als u meerdere benoemde SQL Server-exemplaren uitvoert met behulp van dynamische poorten, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
- Als u een firewallapparaat gebruikt vóór de brondatabases, moet u mogelijk firewallregels toevoegen om de Azure Database Migration Service toegang te geven tot de brondatabase(s) voor migratie. Hetzelfde geldt voor bestanden via SMB-poort 445.
- Maak een beheerd exemplaar voor Azure SQL Database aan de hand van de instructies in het artikel [Een beheerd exemplaar voor Azure SQL Database maken in Azure Portal](https://aka.ms/sqldbmi).
- Zorg ervoor dat de aanmeldingsreferenties die worden gebruikt voor verbinding met het bronexemplaar van SQL Server en het doelexemplaar van Managed Instance lid zijn van de serverrol sysadmin.
- Maak een netwerkshare die de Azure Database Migration Service kan gebruiken om een back-up te maken van de brondatabase.
- Zorg ervoor dat het serviceaccount van het bronexemplaar van SQL Server schrijfbevoegdheid heeft voor de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver lees-/schrijftoegang heeft tot de share.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die volledig beheer heeft over de netwerkshare die u eerder hebt gemaakt. De Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar een Azure-opslagcontainer voor herstelbewerkingen.
- Maak een blob-container en haal de SAS-URI van container op met behulp van de stappen in het artikel [Azure Blob Storage-resources beheren met Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Vergeet niet om alle machtigingen (Lezen, Schrijven, Verwijderen, Lijst) te selecteren in het beleidsvenster terwijl u de SAS-URI maakt. De Azure Database Migration Service heeft zo toegang tot de container van het opslagaccount voor het uploaden van de back-upbestanden die worden gebruikt voor het migreren van databases naar Azure SQL Database Managed Instance.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

    ![Portal-abonnementen weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-subscriptions.png)        

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Een Azure Database Migration Service-exemplaar maken

1. Selecteer in de Azure-portal + **Een resource maken**, zoek naar **Azure Database Migration Service** en selecteer vervolgens **Azure Database Migration Service** in de vervolgkeuzelijst.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media\tutorial-sql-server-to-managed-instance\dms-create1.png)

3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4.  Selecteer de locatie waarop u het exemplaar van DMS wilt maken.

5. Selecteer een bestaand virtueel netwerk (VNET) of maak een netwerk.
 
    Het VNET biedt de Azure Database Migration Service toegang tot de brondatabase van SQL Server en de doeldatabase van Azure SQL Database Managed Instance.

    Zie het artikel [Een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet) voor meer informatie over het maken van een VNET in de Azure-portal.

    Zie het artikel [Netwerktopologieën voor migraties van Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi) voor aanvullende informatie.

6. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).
   
    ![DMS-service starten](media\tutorial-sql-server-to-managed-instance\dms-create-service2.png)

7.  Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat er een exemplaar van de service is gemaakt, zoekt u het exemplaar in de Azure-portal, opent u het en maakt u vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

    ![Zoek alle exemplaren van de Azure Database Migration Service](media\tutorial-sql-server-to-managed-instance\dms-search.png)

2. Zoek in het scherm **Azure Database Migration Service** naar de naam van het exemplaar dat u hebt gemaakt en selecteer vervolgens het exemplaar.
 
3. Selecteer + **Nieuw migratieproject**.

4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project in het tekstvak **Bronservertype**, selecteer **SQL Server**, selecteer in het tekstvak **Type doelserver** **Azure SQL Database Managed Instance** en selecteer vervolgens **Offlinegegevensmigratie** voor **Type activiteit kiezen**.

   ![DMS-project maken](media\tutorial-sql-server-to-managed-instance\dms-create-project2.png)

5. Selecteer **Maken** om het project te maken.

## <a name="specify-source-details"></a>Geef brondetails op

1. Geef in het scherm **Brongegevens** de verbindingsgegevens op voor de brondatabase van SQL Server.

2. Als u geen vertrouwd certificaat hebt geïnstalleerd op de server, schakelt u het selectievakje **Servercertificaat vertrouwen** in.

    Wanneer er geen vertrouwd certificaat is geïnstalleerd, genereert SQL Server een zelfondertekend certificaat wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die worden versleuteld met behulp van een zelfondertekend certificaat bieden geen sterke beveiliging. Ze zijn vatbaar voor man-in-the-middle-aanvallen. U moet niet vertrouwen op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Brondetails](media\tutorial-sql-server-to-managed-instance\dms-source-details1.png)

3. Selecteer **Opslaan**.

4. Selecteer in het scherm **Brondatabases selecteren** de database **Adventureworks2012** voor migratie.

   ![Brondatabases selecteren](media\tutorial-sql-server-to-managed-instance\dms-source-database1.png)

5. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Doeldetails opgeven

1.  Geef in het scherm **Details migratiedoel** de verbindingsgegevens op voor het doel, dat bestaat uit het vooraf ingerichte beheerde exemplaar voor Azure SQL-Database waarnaar u de database **AdventureWorks2012** migreert.

    Als u het beheerde exemplaar voor Azure SQL Database nog niet hebt ingericht, selecteert u **Nee** voor een koppeling met instructies voor het inrichten van het exemplaar. U kunt gewoon verdergaan met het maken van het project en wanneer het beheerde exemplaar voor Azure SQL Database klaar is, gaat u terug naar dit specifieke project om de migratie uit te voeren.   
 
       ![Doel selecteren](media\tutorial-sql-server-to-managed-instance\dms-target-details2.png)

2.  Selecteer **Opslaan**.

## <a name="select-source-databases"></a>Brondatabases selecteren

1. Selecteer in het scherm **Brondatabases selecteren** de brondatabase die u wilt migreren.

    ![Brondatabases selecteren](media\tutorial-sql-server-to-managed-instance\select-source-databases.png)

2. Selecteer **Opslaan**.

## <a name="select-logins"></a>Aanmeldingen selecteren
 
1. Selecteer in het scherm **Aanmeldingen selecteren** de aanmeldingen die u wilt migreren.

    >[!NOTE]
    >Deze versie biedt alleen ondersteuning voor het migreren van de SQL-aanmeldingen.

    ![Aanmeldingen selecteren](media\tutorial-sql-server-to-managed-instance\select-logins.png)

2. Selecteer **Opslaan**.
 
## <a name="configure-migration-settings"></a>Migratie-instellingen configureren
 
1. Geef in het scherm **Migratie-instellingen configureren** de volgende instellingen op:

    | | |
    |--------|---------|
    |**De back-upoptie voor de bron kiezen** | Kies de optie **Ik lever de recentste back-upbestanden** wanneer u al beschikt over een volledige set back-upbestanden die DMS kan gebruiken voor migratie van de database. Kies de optie **Ik laat Azure Database Migration Service back-upbestanden maken** als u wilt dat DMS de volledige back up van de brondatabase gebruikt voor migratie. |
    |**De netwerksharelocatie waarheen back-ups van databases kunnen worden verplaatst door Azure Database Migration Service** | De lokale SMB-netwerkshare waarop de Azure Database Migration Service back-ups van de brondatabase kan opslaan. Het serviceaccount waarmee het SQL Server-bronexemplaar wordt uitgevoerd, moet schrijfbevoegdheid op deze netwerkshare hebben. Geef een FQDN-naam of IP-adressen op van de server in de netwerkshare, bijvoorbeeld '\\\servernaam.domeinnaam.com\back-upmap' of '\\\IP-adres\back-upmap'.|
    |**Gebruikersnaam** | Zorg ervoor dat de Windows-gebruiker volledig beheer heeft over de netwerkshare die u hierboven hebt opgegeven. De Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar een Azure-opslagcontainer voor herstelbewerkingen. Als TDE-compatibele databases zijn geselecteerd voor migratie, moet de bovenstaande Windows-gebruiker het ingebouwde administratoraccount zijn en moet [Gebruikersaccountbeheer](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/user-account-control-overview) zijn uitgeschakeld om Azure Database Migration Service in staat te stellen de certificaatbestanden te uploaden en verwijderen. |
    |**Wachtwoord** | Het wachtwoord voor de gebruiker. |
    |**Opslagaccountinstellingen** | De SAS-URI die Azure Database Migration Service toegang biedt tot de container van het opslagaccount waarnaar de service de back-upbestanden uploadt en die wordt gebruikt voor het migreren van databases naar Azure SQL Database Managed Instance. [Lees hier meer over het verkrijgen van de SAS-URI voor de blob-container](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    |**TDE-instellingen** | Als u de brondatabases migreert met TDE (Transparent Data Encryption) ingeschakeld, moet u schrijfrechten hebben voor het beheerde doelexemplaar voor Azure SQL DB.  Selecteer in de vervolgkeuzelijst het abonnement waarin het beheerde exemplaar voor Azure SQL DB is ingericht.  Selecteer het beheerde doelexemplaar voor Azure SQL DB in de vervolgkeuzelijst. |
    
    ![Migratie-instellingen configureren](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings3.png)

2. Selecteer **Opslaan**.
 
## <a name="review-the-migration-summary"></a>Migratieoverzicht controleren

1. Geef in het tekstvak **Naam activiteit** van het scherm **Migratieoverzicht** een naam op voor de migratieactiviteit.

2. Vouw de sectie **Validatieoptie** uit om het scherm **Validatieoptie kiezen** weer te geven en geef op of de gemigreerde databases moeten worden gevalideerd op juistheid van de query's. Selecteer vervolgens **Opslaan**.

3. Bekijk en controleer de gegevens van het migratieproject.
 
    ![Overzicht van migratieproject](media\tutorial-sql-server-to-managed-instance\dms-project-summary2.png)

4.  Selecteer **Opslaan**.   

## <a name="run-the-migration"></a>De migratie uitvoeren

- Selecteer **Migratie uitvoeren**.

  Het venster van de migratieactiviteit wordt weergegeven en de Status van de activiteit is **In behandeling**.

## <a name="monitor-the-migration"></a>Bewaak de migratie

1. Selecteer in het scherm van de migratieactiviteit **Vernieuwen** om de weergave bij te werken.
 
   ![Migratieactiviteit wordt uitgevoerd](media\tutorial-sql-server-to-managed-instance\dms-monitor-migration1.png)

    U kunt de databases en aanmeldingscategorieën verder uitvouwen om de migratiestatus van de respectieve serverobjecten te controleren.

   ![Migratieactiviteit wordt uitgevoerd](media\tutorial-sql-server-to-managed-instance\dms-monitor-migration-extend.png)

2. Nadat de migratie is voltooid, selecteert u **Rapport downloaden** om een rapport op te halen met gegevens die zijn gekoppeld aan het migratieproces.
 
3. Controleer of de doeldatabase bestaat in de omgeving van de doeldatabase van Azure SQL Database Managed Instance.

## <a name="next-steps"></a>Volgende stappen

- Zie [Een back-up van de database herstellen voor een beheerd exemplaar voor Azure SQL Database](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md) voor een zelfstudie waarin wordt uitgelegd hoe u een database migreert naar een beheerd exemplaar met de opdracht T-SQL RESTORE.
- Zie [Wat is een beheerd exemplaar (preview)?](../sql-database/sql-database-managed-instance.md) voor meer informatie over beheerde exemplaren.
- Zie [Verbinding maken tussen uw toepassing en het beheerde exemplaar van Azure SQL Database](../sql-database/sql-database-managed-instance-connect-app.md) voor meer informatie over het verbinden van apps met een beheerd exemplaar.