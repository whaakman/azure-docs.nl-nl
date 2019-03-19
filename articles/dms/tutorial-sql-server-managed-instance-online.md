---
title: 'Zelfstudie: Azure Database Migration Service gebruiken om uit te voeren van een online migratie van SQL Server naar een beheerd exemplaar voor Azure SQL Database | Microsoft Docs'
description: Leer hoe u een online migratie van SQL Server on-premises uitvoeren naar een beheerd exemplaar voor Azure SQL Database met behulp van de Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 76a2d04aece6eb935d3c716a4ce8654c27f077d2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58177996"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-database-managed-instance-online-using-dms"></a>Zelfstudie: SQL Server migreren naar een beheerd exemplaar voor Azure SQL Database online met behulp van DMS

U kunt de Azure Database Migration Service gebruiken om met minimale downtime databases te migreren van een on-premises SQL Server-exemplaar naar een [beheerd Azure SQL Database-exemplaar](../sql-database/sql-database-managed-instance.md). In het artikel [Een SQL Server-exemplaar migreren naar een beheerd Azure SQL Database-exemplaar](../sql-database/sql-database-managed-instance-migrate.md) vindt u aanvullende methoden, waarvoor enkele handmatige stappen nodig kunnen zijn.

In deze zelfstudie migreert u de database **Adventureworks2012** van een on-premises exemplaar van SQL Server met minimale downtime naar een beheerd Azure SQL Database-exemplaar. Dit doet u met behulp van de Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> - De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> - Een migratieproject maken en de onlinemigratie starten met behulp van de Azure Database Migration Service.
> - De migratie controleren.
> - Cutover uitvoeren voor de migratie als u klaar bent.

> [!NOTE]
> Als u Azure Database Migration Service gebruikt om een onlinemigratie uit te voeren, is het vereist dat u een exemplaar maakt op basis van de prijscategorie Premium.
> [!IMPORTANT]
> Voor een optimale migratie-ervaring raadt Microsoft u aan een exemplaar van de Azure Database Migration Service te maken in dezelfde Azure-regio als de doeldatabase. Het verplaatsen van gegevens naar regio's of geografieën kan het migratieproces vertragen en fouten veroorzaken.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Dit artikel beschrijft een online migratie van SQL Server naar een beheerd exemplaar voor Azure SQL Database. Zie voor een offlinemigratie [beheerd exemplaar van SQL Server migreren naar een Azure SQL-Database offline met behulp van DMS](tutorial-sql-server-to-managed-instance.md).

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Maak een Azure Virtual Network (VNET) voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Lees hier meer over netwerktopologieën voor migraties van beheerde Azure SQL Database-exemplaren met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi).

    > [!NOTE]
    > Tijdens de installatie van de VNET, als u ExpressRoute gebruikt met het naar Microsoft-netwerkpeering, voeg de volgende service [eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aan het subnet waarin de service worden ingericht:
    > - Doel-database-eindpunt (bijvoorbeeld SQL-eindpunt, Cosmos DB-eindpunt, enzovoort)
    > - Opslageindpunt
    > - Service bus-eindpunt
    >
    > Deze configuratie is nodig omdat de Azure Database Migration Service beschikt niet over de verbinding met internet.

- Zorg ervoor dat uw Netwerkbeveiligingsgroep VNET-regels niet die zijn de volgende blokkeert communicatiepoorten 443, 53, 9354, 445, 12000. Zie voor meer informatie over verkeer filteren van Azure VNET NSG het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows-firewall voor toegang tot de engine van de brondatabase](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van SQL Server. Standaard verloopt dit via TCP-poort 1433.
- Als u meerdere benoemde SQL Server-exemplaren uitvoert met behulp van dynamische poorten, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
- Als u een firewallapparaat gebruikt vóór de brondatabases, moet u mogelijk firewallregels toevoegen om de Azure Database Migration Service toegang te geven tot de brondatabase(s) voor migratie. Hetzelfde geldt voor bestanden via SMB-poort 445.
- Maak een beheerd Azure SQL Database-exemplaar aan de hand van de instructies in het artikel [Een beheerd exemplaar voor Azure SQL Database maken in de Azure-portal](https://aka.ms/sqldbmi).
- Zorg ervoor dat de aanmeldingen gebruikt voor het verbinding maken met de bron-SQL Server en het doel beheerde exemplaar zijn leden van de serverrol sysadmin.
- Richt een SMB-netwerkshare in die volledige back-upbestanden bevat van uw database en latere back-logbestanden van transacties die Azure Database Migration Service voor migratie van de database kan gebruiken.
- Zorg ervoor dat het serviceaccount van het bronexemplaar van SQL Server schrijfbevoegdheid heeft voor de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver lees-/schrijftoegang heeft tot de share.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die volledig beheer heeft over de netwerkshare die u eerder hebt gemaakt. De Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar een Azure-opslagcontainer voor herstelbewerkingen.
- Maak een Azure Active Directory-toepassings-id die de toepassings-id-sleutel genereert die de DMS-service kan gebruiken genereert om verbinding te maken met het beheerde doelexemplaar voor Azure SQL Database en Azure Storage Container. Zie het artikel [Portal gebruiken voor het maken van een Azure Active Directory-toepassing en een -service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) voor meer informatie.
- Maak of noteer een Azure Storage-account in de **Standard-prestatielaag** waarmee de DMS-service de back-upbestanden van de database kan uploaden en gebruiken voor het migreren van databases.  Zorg ervoor dat u het Azure Storage-account maakt in de regio van de DMS-service.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

    ![Portal-abonnementen weergeven](media/tutorial-sql-server-to-managed-instance-online/portal-select-subscriptions.png)        
2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-sql-server-to-managed-instance-online/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-sql-server-to-managed-instance-online/portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Een Azure Database Migration Service-exemplaar maken

1. Selecteer in de Azure-portal + **Een resource maken**, zoek naar **Azure Database Migration Service** en selecteer vervolgens **Azure Database Migration Service** in de vervolgkeuzelijst.

     ![Azure Marketplace](media/tutorial-sql-server-to-managed-instance-online/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-sql-server-to-managed-instance-online/dms-create1.png)

3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer de locatie waarop u het exemplaar van DMS wilt maken.

5. Selecteer een bestaand virtueel netwerk (VNET) of maak een netwerk.

    Het VNET biedt de Azure Database Migration Service toegang tot de brondatabase van SQL Server en de doeldatabase van het beheerde Azure SQL Database-exemplaar.

    Zie het artikel [Een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet) voor meer informatie over het maken van een VNET in de Azure-portal.

    Zie het artikel [Netwerktopologieën voor migraties van een beheerd Azure SQL Database-exemplaar met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi) voor aanvullende informatie.

6. Selecteer een SKU in de prijscategorie Premium.

    > [!NOTE]
    > Onlinemigraties worden alleen ondersteund bij gebruik van deze categorie. 

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    ![DMS-service starten](media/tutorial-sql-server-to-managed-instance-online/dms-create-service3.png)

7. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat er een exemplaar van de service is gemaakt, zoekt u het exemplaar in de Azure-portal, opent u het en maakt u vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

    ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance-online/dms-search.png)

2. Zoek in het scherm **Azure Database Migration Service** naar de naam van het exemplaar dat u hebt gemaakt en selecteer vervolgens het exemplaar.

3. Selecteer + **Nieuw migratieproject**.

4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project, in het tekstvak **Bronservertype**, selecteer **SQL Server**, in het tekstvak **Type doelserver**, selecteer **Azure SQL Database Managed Instance** en selecteer vervolgens **Onlinegegevensmigratie** voor **Type activiteit kiezen**.

   ![DMS-project maken](media/tutorial-sql-server-to-managed-instance-online/dms-create-project3.png)

5. Selecteer **Maken en uitvoeren van de activiteit** om het project te maken.

## <a name="specify-source-details"></a>Geef brondetails op

1. Geef in het scherm **Brongegevens** de verbindingsgegevens op voor de brondatabase van SQL Server.

2. Als u geen vertrouwd certificaat hebt geïnstalleerd op de server, schakelt u het selectievakje **Servercertificaat vertrouwen** in.

    Wanneer er geen vertrouwd certificaat is geïnstalleerd, genereert SQL Server een zelfondertekend certificaat wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die worden versleuteld met behulp van een zelfondertekend certificaat bieden geen sterke beveiliging. Ze zijn vatbaar voor man-in-the-middle-aanvallen. U moet niet vertrouwen op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Brondetails](media/tutorial-sql-server-to-managed-instance-online/dms-source-details2.png)

3. Selecteer **Opslaan**.

4. Selecteer in het scherm **Brondatabases selecteren** de database **Adventureworks2012** voor migratie.

   ![Brondatabases selecteren](media/tutorial-sql-server-to-managed-instance-online/dms-source-database1.png)

    > [!IMPORTANT]
    > Als u SSIS (SQL Server Integration Services) gebruikt, biedt DMS momenteel geen ondersteuning voor het migreren van de catalogusdatabase voor uw SSIS-projecten/pakketten (SSISDB) van SQL Server naar een beheerd Azure SQL Database-exemplaar. U kunt SSIS echter in Azure Data Factory (ADF) inrichten en uw SSIS-projecten/-pakketten opnieuw implementeren naar de bestemmings-SSISDB die wordt gehost door het beheerde Azure SQL Database-exemplaar. Zie het artikel [Pakketten van SQL Server Integration Services migreren naar Azure](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages) voor meer informatie over het migreren van SSIS-pakketten.

5. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Geef op het scherm **Details migratiedoel** waarden op voor **Toepassings-id** en **Sleutel** die het DMS-exemplaar kan gebruiken om verbinding te maken met het doelexemplaar van een beheerd Azure SQL Database-exemplaar en het Azure Storage-account.

    Zie het artikel [Portal gebruiken voor het maken van een Azure Active Directory-toepassing en een -service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) voor meer informatie.

2. Selecteer het **abonnement** met het doelexemplaar van Azure SQL Database Managed Instance en selecteer vervolgens het doelexemplaar.

    Als u het beheerde Azure SQL Database-exemplaar nog niet hebt ingericht, selecteert u de [koppeling](https://aka.ms/SQLDBMI) voor instructies voor het inrichten van het exemplaar. Wanneer het beheerde Azure SQL Database-exemplaar klaar is, gaat u terug naar dit specifieke project om de migratie uit te voeren.

3. Geef waarden op voor **SQL-gebruiker** en **Wachtwoord** om verbinding te maken met het beheerde Azure SQL Database-exemplaar.

    ![Doel selecteren](media/tutorial-sql-server-to-managed-instance-online/dms-target-details3.png)

4. Selecteer **Opslaan**.

## <a name="select-source-databases"></a>Brondatabases selecteren

1. Selecteer in het scherm **Brondatabases selecteren** de brondatabase die u wilt migreren.

    ![Brondatabases selecteren](media/tutorial-sql-server-to-managed-instance-online/dms-select-source-databases2.png)

2. Selecteer **Opslaan**.

## <a name="configure-migration-settings"></a>Migratie-instellingen configureren

1. Geef in het scherm **Migratie-instellingen configureren** de volgende instellingen op:

    | | |
    |--------|---------|
    |**SMB-netwerksharelocatie** | De lokale SMB-netwerkshare die de volledige back-upbestanden bevat van uw database en latere back-logbestanden van transacties die Azure Database Migration Service kan gebruiken voor migratie. Het serviceaccount waarmee het SQL Server-bronexemplaar wordt uitgevoerd, moet lees-\schrijfbevoegdheid op deze netwerkshare hebben. Geef een FQDN-naam of IP-adressen op van de server in de netwerkshare, bijvoorbeeld '\\\servernaam.domeinnaam.com\back-upmap' of '\\\IP-adres\back-upmap'.|
    |**Gebruikersnaam** | Zorg ervoor dat de Windows-gebruiker volledig beheer heeft over de netwerkshare die u hierboven hebt opgegeven. De Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar een Azure-opslagcontainer voor herstelbewerkingen. |
    |**Wachtwoord** | Het wachtwoord voor de gebruiker. |
    |**Abonnement van het Azure Storage-account** | Selecteer het abonnement met het Azure Storage-account. |
    |**Azure Storage-account** | Selecteer het Azure Storage-account waarnaar DMS de back-upbestanden kan uploaden vanuit de SMB-netwerkshare en vervolgens kan gebruiken voor migratie van de database.  Het is raadzaam om het Storage-account te selecteren in dezelfde regio als de DMS-service voor optimale prestaties bij het uploaden van de bestanden. |

    ![Migratie-instellingen configureren](media/tutorial-sql-server-to-managed-instance-online/dms-configure-migration-settings4.png)

2. Selecteer **Opslaan**.

## <a name="review-the-migration-summary"></a>Migratieoverzicht controleren

1. Geef in het tekstvak **Naam activiteit** van het scherm **Migratieoverzicht** een naam op voor de migratieactiviteit.

2. Bekijk en controleer de gegevens van het migratieproject.

    ![Overzicht van migratieproject](media/tutorial-sql-server-to-managed-instance-online/dms-project-summary3.png)

## <a name="run-and-monitor-the-migration"></a>De migratie uitvoeren en controleren

1. Selecteer **Migratie uitvoeren**.

2. Selecteer op het scherm met migratieactiviteit **Vernieuwen** om de weergave bij te werken.

   ![Migratieactiviteit wordt uitgevoerd](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration2.png)

    U kunt de databases en aanmeldingscategorieën verder uitvouwen om de migratiestatus van de respectieve serverobjecten te controleren.

   ![Migratieactiviteit wordt uitgevoerd](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration-extend2.png)

## <a name="performing-migration-cutover"></a>Migratie-cutover uitvoeren

Nadat de volledige databaseback-up is hersteld op het doelexemplaar van Azure SQL Database Managed Instance, is de database beschikbaar voor het uitvoeren van een migratie-cutover.

1. Wanneer u klaar bent om de onlinedatabasemigratie te voltooien, selecteert u **Cutover starten**.

2. Stop al het binnenkomende verkeer naar brondatabases.

3. Neem het bestand [tail-log backup], maak het back-upbestand beschikbaar in de SMB-netwerkshare en wacht totdat deze laatste transactielogboekback-up is hersteld.

    U ziet dat op dat moment **Wijzigingen in behandeling** wordt ingesteld op 0.

4. Selecteer **Bevestigen** en selecteer vervolgens **Toepassen**.

    ![Voltooien van cutover voorbereiden](media/tutorial-sql-server-to-managed-instance-online/dms-complete-cutover.png)

5. Wanneer de migratie de status **Voltooid** heeft, verbindt u uw toepassingen met het nieuwe doelexemplaar van Azure SQL Database Managed Instance.

    ![Cutover voltooit](media/tutorial-sql-server-to-managed-instance-online/dms-cutover-complete.png)

## <a name="next-steps"></a>Volgende stappen

- Zie [Een back-up van de database herstellen voor een beheerd Azure SQL Database-exemplaar](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md) voor een zelfstudie waarin wordt uitgelegd hoe u een database migreert naar een beheerd exemplaar met de opdracht T-SQL RESTORE.
- Zie [Wat is een beheerd exemplaar?](../sql-database/sql-database-managed-instance.md) voor meer informatie over beheerde exemplaren.
- Zie [Verbinding maken met toepassingen](../sql-database/sql-database-managed-instance-connect-app.md) voor meer informatie over het verbinden van apps met een beheerd exemplaar.
