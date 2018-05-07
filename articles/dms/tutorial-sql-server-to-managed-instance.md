---
title: DMS gebruiken om te migreren naar Azure SQL Database-beheerd instantie | Microsoft Docs
description: Informatie over het migreren van SQL Server on-premises naar Azure SQL Database-beheerde exemplaar met behulp van de Service Azure Database migratie.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/07/2018
ms.openlocfilehash: bb7cc17c36809975e26c8da8beda004a0b0cfd9e
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>SQL-Server migreren naar Azure SQL Database beheerd exemplaar met gebruikmaking van DMS
U kunt de Azure-Service voor het migreren van Database voor het migreren van de databases van een lokale SQL Server-exemplaar naar een [Azure SQL Database-beheerd instantie](../sql-database/sql-database-managed-instance.md). Voor extra methoden waarvoor handmatige moeite, Zie het artikel [SQL Server-exemplaar migratie naar Azure SQL Database-beheerd instantie](../sql-database/sql-database-managed-instance-migrate.md).

> [!IMPORTANT]
> Migratie-projecten uit SQL Server naar Azure SQL Database-beheerde exemplaar zijn Preview-versie en zijn onderworpen aan de [aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In deze zelfstudie maakt u de migratie de **Adventureworks2012** -database van een lokaal exemplaar van SQL Server naar een Azure SQL Database beheerd-exemplaar met behulp van de Service Azure Database migratie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Geen exemplaar maken van de Service Azure Database migratie.
> * Maak een migratieproject met behulp van de Service Azure Database migratie.
> * Voer de migratie uit.
> * De migratie bewaken.
> * Download een rapport voor de migratie.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt voltooid, moet u:

- Een VNET maken voor de Azure-Service voor het migreren van Database met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Meer informatie over netwerktopologieën voor Azure SQL DB-beheerde exemplaar migraties met behulp van de migratie van Azure databaseservice](https://aka.ms/dmsnetworkformi).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Netwerkbeveiligingsgroep regels komen de volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over het Azure VNET NSG wordt verkeer gefilterd [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows Firewall voor toegang tot de gegevensbron database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Open Windows Firewall zodat de Service Azure Database migratie voor toegang tot de bron van SQL Server, die standaard TCP-poort 1433.
- Als u meerdere benoemde exemplaren van SQL Server met behulp van dynamische poorten worden uitgevoerd, kunt u desgewenst de SQL Browser-Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de migratie van Azure databaseservice verbinding met een benoemd exemplaar op de bron maken kan -Server.
- Als u van een firewallapparaat voor uw brondatabases gebruikmaakt, moet u mogelijk firewallregels zodat de Service Azure Database migratie voor toegang tot de bron-databases voor migratie, evenals bestanden via het SMB-poort 445 toevoegen.
- Maken van een Azure SQL Database beheerd-instantie aan de hand van de details in het artikel [maken van een Azure SQL Database beheerd-exemplaar in de Azure portal](https://aka.ms/sqldbmi).
- Zorg ervoor dat de aanmeldingen gebruikt voor het verbinding maken met de bron van SQL Server en de doelinstantie van beheerde leden van de serverrol sysadmin.
- Maak een netwerkshare die de migratie van Azure databaseservice gebruiken kunt voor back-up van de brondatabase.
- Zorg ervoor dat het serviceaccount dat de bron met SQL Server-exemplaar heeft schrijfrechten op de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver lees-/ schrijftoegang tot dezelfde share heeft.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die de machtiging Volledig beheer heeft op de netwerkshare die u eerder hebt gemaakt. De migratie van Azure databaseservice imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar Azure storage-container voor restore-bewerking.
- Maken van een blob-container en de SAS URI ophalen met behulp van de stappen in het artikel [beheren Azure Blob Storage-resources met Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), moet u alle machtigingen (lezen, schrijven, verwijderen, lijst) in het venster beleid tijdens selecteren maken van de SAS-URI. Dit is de Azure migratie databaseservice met toegang tot uw storage-account-container voor het uploaden van de back-upbestanden gebruikt voor het migreren van databases naar Azure SQL Database beheerd exemplaar

## <a name="register-the-microsoftdatamigration-resource-provider"></a>De registerbronprovider is Microsoft.DataMigration

1. Aanmelden bij de Azure portal, selecteer **alle services**, en selecteer vervolgens **abonnementen**.

    ![Portal abonnementen weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-subscriptions.png)        

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![resourceproviders weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3. Zoekcriteria voor de migratie en rechts van **Microsoft.DataMigration**, selecteer **registreren**.

    ![Resourceprovider registreren](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Azure-Database migratieservice-exemplaar maken

1. Selecteer in de Azure-portal + **maken van een resource**, zoeken naar **Azure migratie databaseservice**, en selecteer vervolgens **Migratieservice voor Azure-Database** in de vervolgkeuzelijst een lijst.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2. Op de **Azure migratie databaseservice** Schakel in het scherm **maken**.

    ![Azure-Database migratieservice-exemplaar maken](media\tutorial-sql-server-to-managed-instance\dms-create1.png)

3. Op de **migratieservice maken** scherm, Geef een naam voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer een bestaand virtueel netwerk (VNET) of een maken.
 
    Het VNET biedt de Service Azure Database migratie met toegang tot de SQL Server-bron en doel beheerde exemplaar van Azure SQL Database.

    Raadpleeg het artikel voor meer informatie over het maken van een VNET in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

    Zie voor aanvullende details over het artikel [netwerk topologieën voor Azure SQL DB-beheerde exemplaar migraties met behulp van de migratie van Azure databaseservice](https://aka.ms/dmsnetworkformi).

5. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en Prijscategorieën de [pagina met prijzen](https://aka.ms/dms-pricing).
   
    ![DMS-Service maken](media\tutorial-sql-server-to-managed-instance\dms-create-service1.png)

6.  Selecteer **maken** de service te maken.

## <a name="create-a-migration-project"></a>Een migratieproject maken

Nadat de service is gemaakt, zoeken binnen de Azure-portal, opent u het en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **alle services**, zoeken naar Azure Database migratie-Service en selecteer vervolgens **Azure migratie databaseservices**.

    ![Alle exemplaren van de Azure-Service voor het migreren van Database vinden](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Op de **Azure migratie databaseservice scherm**, zoekt u de naam van het exemplaar gemaakt en selecteer vervolgens het exemplaar.
 
3. Selecteer + **nieuw migratieproject**.

4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, en klik vervolgens in de **doel servertype** in het tekstvak, selecteer **Azure SQL Database-beheerd instantie**.

   ![DMS-Project maken](media\tutorial-sql-server-to-managed-instance\dms-create-project1.png)

5. Selecteer **maken** om het project te maken.

## <a name="specify-source-details"></a>Geef details van gegevensbron

1. Op de **bron details** scherm, geeft u de verbindingsgegevens voor de bron van SQL Server.

2. Als u een vertrouwd certificaat niet op uw server hebt geïnstalleerd, selecteert u de **vertrouwensrelatie servercertificaat** selectievakje.

    Wanneer een vertrouwd certificaat niet is geïnstalleerd, wordt door SQL Server een zelfondertekend certificaat gegenereerd wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die met behulp van een zelfondertekend certificaat encyopted beschikbaar sterke beveiliging niet. Ze zijn vatbaar voor man-in-the-middle-aanvallen. Vertrouw niet op SSL zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Details van gegevensbron](media\tutorial-sql-server-to-managed-instance\dms-source-details1.png)

3. Selecteer **Opslaan**.

4. Op de **brondatabases selecteren** scherm, selecteert u de **Adventureworks2012** database voor de migratie.

   ![Selecteer de brondatabases](media\tutorial-sql-server-to-managed-instance\dms-source-database1.png)

5. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Geef Doeldetails op

1.  Op de **doel details** scherm, geeft u de verbindingsgegevens voor het doel, het vooraf is ingericht Azure SQL Database beheerd exemplaar waarnaar is de **AdventureWorks2012** database gemigreerd.

    Als u de Azure SQL Database beheerd exemplaar nog niet hebt ingericht, selecteert u **Nee** voor een koppeling voor hulp bij het inrichten van het exemplaar. U kunt nog steeds verdergaan met het maken van het project en vervolgens, wanneer het Azure SQL Database beheerd-exemplaar gereed is, terug naar dit specifieke project uit te voeren van de migratie.   
 
       ![Doel selecteren](media\tutorial-sql-server-to-managed-instance\dms-target-details1.png)

2.  Selecteer **Opslaan**.

3.  Op de **Projectsamenvatting** scherm bekijken en controleer de gegevens die zijn gekoppeld aan het migratieproject.
 
    ![Overzicht migratieproject](media\tutorial-sql-server-to-managed-instance\dms-project-summary1.png)

4.  Selecteer **Opslaan**.   

## <a name="run-the-migration"></a>De migratie uitvoeren

1.  De laatst opgeslagen project selecteert, selecteert u + **nieuwe activiteit**, en selecteer vervolgens **migratie uitvoeren**.

    ![Nieuwe activiteit maken](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity1.png)

2.  Wanneer u wordt gevraagd, voer de referenties van de bron- en doelservers en selecteer vervolgens **opslaan**.

3.  Op de **brondatabases selecteren** scherm, selecteert u de database die u wilt migreren.

    ![Selecteer de brondatabases](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases1.png)

4.  Selecteer **opslaan**, en klik vervolgens op de **migratie-instellingen configureren** scherm, bieden de volgende details:

    | | |
    |--------|---------|
    |**Locatie netwerkshare** | Het lokale netwerk delen dat de Service Azure Database migratie kan duren voordat de bron databaseback-ups op. Het serviceaccount met een exemplaar van SQL Server bron moet schrijfmachtigingen hebben op deze netwerkshare. Geef een FQDN-naam of IP-adressen van de server in de netwerkshare, bijvoorbeeld '\\\servername.domainname.com\backupfolder' of '\\\IP address\backupfolder'.|
    |**Gebruikersnaam** | De windows-gebruikersnaam dat de migratie van Azure databaseservice kan imiteren en de back-upbestanden naar Azure storage-container voor restore-bewerking uploaden. |
    |**Wachtwoord** | Wachtwoord voor de gebruiker. |
    |**Instellingen voor de opslag** | De SAS-URI waarmee de Service Azure Database migratie met toegang tot uw storage-account-container waarnaar de service de back-up van bestanden en die uploadt wordt gebruikt voor het migreren van databases naar Azure SQL Database-beheerde exemplaar. [Meer informatie over het ophalen van de SAS-URI voor blob-container](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Migratie-instellingen configureren](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings1.png)

5.  Selecteer **opslaan**, en klik vervolgens op de **migratie samenvatting** scherm in de **activiteitsnaam** tekst, geeft u een naam voor de migratieactiviteit.

6. Vouw de **validatieoptie** sectie om weer te geven de **validatie optie** scherm, Geef op of de gemigreerde database query juist valideren, en selecteer vervolgens **opslaan** .  

    ![Samenvatting van de migratie](media\tutorial-sql-server-to-managed-instance\dms-migration-summary1.png)

7. Selecteer **migratie uitvoeren**.

    Het venster van de activiteit migratie wordt weergegeven en de status van de activiteit **in behandeling**.

   ![Migratieactiviteiten in behandeling](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-pending.png)

## <a name="monitor-the-migration"></a>De migratie bewaken

1. Selecteer op het scherm van de activiteit migratie **vernieuwen** om de weergave bijwerken totdat u ziet dat de status van de migratie wordt weergegeven als **voltooid**.
 
   ![De migratieactiviteit is voltooid](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-finished.png)

2. Nadat de migratie is voltooid, selecteert u **Download rapport** ophalen van een rapport met gegevens die zijn gekoppeld aan het migratieproces.
 
3. Controleren of de doeldatabase op de Azure SQL Database-beheerd instantie doelomgeving.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een zelfstudie waarin u een database migreren naar een beheerde-exemplaar met de opdracht RESTORE T-SQL, [een back-up herstellen naar een exemplaar beheerd met behulp van de opdracht restore](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Zie voor meer informatie over beheerde exemplaar [wat is er een exemplaar beheerd](../sql-database/sql-database-managed-instance.md).
- Zie voor meer informatie over het aansluiten van apps op een beheerde exemplaar [verbinding maken met toepassingen](../sql-database/sql-database-managed-instance-connect-app.md).
