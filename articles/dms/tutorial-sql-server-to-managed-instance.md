---
title: Gebruik de Azure-Service voor het migreren van Database SQL-Server migreren naar Azure SQL Database-beheerd instantie | Microsoft Docs
description: Informatie over het migreren van SQL Server on-premises naar Azure SQL Database-beheerde exemplaar met behulp van de Service Azure Database migratie.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: d74a273061912ea2bdcc39301ce9a727b07ade41
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance"></a>SQL-Server migreren naar beheerde Azure SQL Database-instantie
De Service Azure Database migratie kunt u de databases van een lokale SQL Server-instantie migreren naar Azure SQL Database. In deze zelfstudie maakt u de migratie de **Adventureworks2012** database van een lokaal exemplaar van SQL Server naar een Azure SQL Database met behulp van de Service Azure Database migratie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Geen exemplaar maken van de Service Azure Database migratie.
> * Maak een migratieproject met behulp van de Service Azure Database migratie.
> * Voer de migratie uit.
> * De migratie bewaken.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt voltooid, moet u:

- Een VNET maken voor de Azure-Service voor het migreren van Database met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Meer informatie over netwerktopologieën voor Azure SQL DB-beheerde exemplaar migraties met behulp van de migratie van Azure databaseservice](https://aka.ms/dmsnetworkformi).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Netwerkbeveiligingsgroep regels komen de volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over het Azure VNET NSG wordt verkeer gefilterd [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [configureert u de Windows Firewall voor toegang tot de gegevensbron database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Open Windows Firewall zodat de Service Azure Database migratie voor toegang tot de bron van SQL Server.
- Als u van een firewallapparaat voor uw brondatabases gebruikmaakt, moet u mogelijk firewallregels zodat de Service Azure Database migratie voor toegang tot de bron-databases voor migratie, evenals bestanden via het SMB-poort 445 toevoegen.
- Geen exemplaar maken van Azure SQL Database-beheerd instantie aan de hand van de details in het artikel [maken van een Azure SQL Database beheerd-exemplaar in de Azure portal](https://aka.ms/sqldbmi).
- Zorg ervoor dat de aanmeldingen gebruikt voor het verbinding maken met de bron van SQL Server en de doelinstantie van beheerde leden van de serverrol sysadmin.
- Maak een netwerkshare die de migratie van Azure databaseservice gebruiken kunt voor back-up van de brondatabase.
- Zorg ervoor dat het serviceaccount met een SQL Server-exemplaar van bron schrijfrechten heeft op de netwerkshare die u hebt gemaakt.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die de machtiging Volledig beheer op de netwerkshare die u heeft hierboven hebt gemaakt. De Service Azure Database migratie wordt de gebruikersreferenties voor het uploaden van de back-upbestanden naar Azure storage-container voor terugzetbewerking imiteren.
- Maken van een blob-container en de SAS URI ophalen met behulp van de stappen in het artikel [beheren Azure Blob Storage-resources met Opslagverkenner (Preview)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), moet u alle machtigingen (lezen, schrijven, verwijderen, lijst) in het venster beleid selecteren tijdens het maken van de SAS-URI. Hiermee wordt de migratie van Azure databaseservice toegang bieden tot uw opslag account-container voor het uploaden van de back-upbestanden die wordt gebruikt voor het migreren van databases naar Azure SQL Database-beheerde exemplaar

## <a name="register-the-microsoftdatamigration-resource-provider"></a>De registerbronprovider is Microsoft.DataMigration

1.  Aanmelden bij de Azure portal, selecteer **alle services**, en selecteer vervolgens **abonnementen**.
![Portal abonnementen weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-subscription.png)

1.  Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.
![resourceproviders weergeven](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

1.  Zoekcriteria voor de migratie en rechts van **Microsoft.DataMigration**, selecteer **registreren**.
![de registerbronprovider](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Geen exemplaar maken

1.  Selecteer in de Azure-portal **+ maken van een resource**, zoeken naar **Azure migratie databaseservice**, en selecteer vervolgens **Migratieservice voor Azure-Database** in de vervolgkeuzelijst een lijst.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

1.  Op de **Migratieservice voor Azure-Database (preview)** Schakel in het scherm **maken**.

    ![Azure-Database migratieservice-exemplaar maken](media\tutorial-sql-server-to-managed-instance\dms-create.png)

1.  Op de **migratie databaseservice** scherm, Geef een naam voor de service, het abonnement, Resource-groep, een virtueel netwerk en de prijscategorie.

    Zie voor meer informatie over de kosten en Prijscategorieën de [pagina met prijzen](https://aka.ms/dms-pricing). *De Azure-Service voor het migreren van Database is momenteel in preview en u wordt niet in rekening gebracht.*

    **Netwerk:** Selecteer een bestaande of maak een nieuw VNET, waardoor de migratie van Azure databaseservice met toegang tot de SQL Server-bron en doel beheerde exemplaar van Azure SQL Database. [Meer informatie over netwerktopologieën voor Azure SQL DB-beheerde exemplaar migraties met behulp van de migratie van Azure databaseservice](https://aka.ms/dmsnetworkformi).

    Zie voor meer informatie over het maken van het VNET in Azure-portal [een virtueel netwerk maken met meerdere subnetten met de Azure portal](https://aka.ms/DMSVnet).

    ![DMS-Service maken](media\tutorial-sql-server-to-managed-instance\dms-create-service.png)

1.  Selecteer **maken** de service te maken.


## <a name="create-a-migration-project"></a>Een migratieproject maken

Nadat de service is gemaakt, zoeken binnen de Azure-portal en open het.

1.  Selecteer **+ nieuw migratieproject**.

1.  Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, en klik vervolgens in de **doel servertype** in het tekstvak, selecteer **Azure SQL Database-beheerd instantie**.

    ![DMS-Project maken](media\tutorial-sql-server-to-managed-instance\dms-create-project.png)

1.  Selecteer **maken** om het project te maken.

## <a name="specify-source-details"></a>Geef details van gegevensbron

1.  Op de **bron details** scherm, geeft u de verbindingsgegevens voor de bron van SQL Server.

    ![Details van gegevensbron](media\tutorial-sql-server-to-managed-instance\dms-source-details.png)

1.  Selecteer **opslaan**, en selecteer vervolgens de **Adventureworks2012** database voor de migratie.

    ![Selecteer de brondatabases](media\tutorial-sql-server-to-managed-instance\dms-source-database.png)

## <a name="specify-target-details"></a>Geef Doeldetails op

1.  Selecteer **opslaan**, en klik vervolgens op de **doel details** scherm, geeft u de verbindingsgegevens voor het doel, het vooraf is ingericht Azure SQL Database beheerd exemplaar waarnaar is de  **AdventureWorks2012** database worden gemigreerd.

    ![Doel selecteren](media\tutorial-sql-server-to-managed-instance\dms-target-details.png)

1.  Selecteer **Opslaan**.

1.  Op de **Projectsamenvatting** scherm bekijken en controleer de gegevens die zijn gekoppeld aan het migratieproject.

## <a name="run-the-migration"></a>De migratie uitvoeren

1.  De laatst opgeslagen project selecteert, selecteert u **+ nieuwe activiteit**, en selecteer vervolgens **migratie uitvoeren**.

    ![Nieuwe activiteit maken](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity.png)

1.  Wanneer u wordt gevraagd, voer de referenties van de bron- en doelservers en selecteer vervolgens **opslaan**.

1.  Op de **toewijzen aan de doeldatabases** scherm, selecteert u de bron-databases die u wilt migreren.

    ![Selecteer de brondatabases](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases.png)

1.  Selecteer **opslaan**op de **migratie-instellingen configureren** scherm, bieden de volgende details:

    | | |
    |--------|---------|
    |**Back-uplocatie-server** | Het lokale netwerk delen dat de Service Azure Database migratie kan duren voordat de bron databaseback-ups op. Het serviceaccount met een exemplaar van SQL Server bron moet schrijfmachtigingen hebben op deze netwerkshare. |
    |**Gebruikersnaam** | De windows-gebruikersnaam dat de migratie van Azure databaseservice kan imiteren en de back-upbestanden naar Azure storage-container voor restore-bewerking uploaden. |
    |**Wachtwoord** | Wachtwoord voor de bovenstaande gebruiker. |
    |**Opslag SAS-URI** | De SAS-URI die de Azure-Service voor het migreren van Database toegang tot uw storage-account-container die de service wordt de back-up van bestanden te uploaden en dat wordt gebruikt biedt voor het migreren van databases naar Azure SQL Database-beheerde exemplaar.  [Meer informatie over het ophalen van de SAS-URI voor blob-container](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Migratie-instellingen configureren](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings.png)

1.  Selecteer **opslaan**op scherm de samenvatting migratie de **activiteitsnaam** tekst, geeft u een naam voor de migratieactiviteit.

    ![Samenvatting van de migratie](media\tutorial-sql-server-to-managed-instance\dms-migration-summary.png)


## <a name="monitor-the-migration"></a>De migratie bewaken

1.  Selecteer de migratieactiviteit om de status van de activiteit te controleren.

1.  Nadat de migratie is voltooid, controleert u of de doeldatabases op de doel-Azure SQL Database-beheerd instantie.

    ![De migratie bewaken](media\tutorial-sql-server-to-managed-instance\dms-monitor-migration.png)

