---
title: SQL Server Integration Services-pakketten migreren naar een beheerd exemplaar voor Azure SQL Database | Microsoft Docs
description: Leer hoe u SQL Server Integration Services-pakketten migreren naar een beheerd exemplaar voor Azure SQL Database.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2019
ms.openlocfilehash: 82a047616c199e37bfa22f53e02f3f7b224b47c1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083182"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-database-managed-instance"></a>SQL Server Integration Services-pakketten migreren naar een beheerd exemplaar voor Azure SQL Database
Als u SQL Server Integration Services (SSIS) te gebruiken en wilt migreren van uw SSIS-projecten /-pakketten van de bron SSISDB wordt gehost door de SQL Server naar de bestemming SSISDB wordt gehost door een beheerd exemplaar voor Azure SQL Database, kunt u Azure Database Migration Service.

Als de versie van u gebruik van SSIS ouder dan 2012 is of u niet-SSISDB pakket store typen, voordat de migratie van uw SSIS-projecten /-pakketten, moet u deze converteren met behulp van de Wizard Integration Services-Project conversie, die ook vanuit een SSMS kunnen worden gestart. Zie voor meer informatie het artikel [projecten converteren naar het Projectimplementatiemodel](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure Database Migration Service (DMS) ondersteunt op dit moment geen Azure SQL Database als een doellocatie voor migratie. Voor het implementeren van SSIS-projecten /-pakketten naar Azure SQL Database, Zie het artikel [pakketten van SQL Server Integration Services opnieuw implementeren naar Azure SQL Database](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

In dit artikel leert u het volgende:
> [!div class="checklist"]
>
> * Bron SSIS-projecten /-pakketten te beoordelen.
> * SSIS-projecten /-pakketten migreren naar Azure.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze stappen hebt voltooid, hebt u het volgende nodig:

* Een Azure Virtual Network (VNet) maken voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute ](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Zie voor meer informatie het artikel [netwerktopologieën voor Azure SQL Database managed instance van migraties met Azure Database Migration Service]( https://aka.ms/dmsnetworkformi). Zie voor meer informatie over het maken van een VNet, de [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/), en met name de artikelen met vindt u meer details.
* Om ervoor te zorgen dat uw Netwerkbeveiligingsgroep VNet-regels niet die zijn de volgende poorten voor binnenkomende communicatie naar Azure Database Migration Service blokkeert: 443, 53, 9354, 445, 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Het configureren van uw [Windows Firewall voor toegang tot de bron database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017).
* Uw Windows-Firewall voor de Azure Database Migration Service voor toegang tot de bron-SQL-Server, die standaard TCP-poort 1433 openen.
* Als u meerdere benoemde SQL Server-exemplaren uitvoert met behulp van dynamische poorten, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
* Als u een firewallapparaat gebruikt vóór de brondatabases, moet u mogelijk firewallregels toevoegen om de Azure Database Migration Service toegang te geven tot de brondatabase(s) voor migratie. Hetzelfde geldt voor bestanden via SMB-poort 445.
* Een Azure SQL-Database beheerd exemplaar naar SSISDB-host. Als u maken wilt, volgt u de details in het artikel [maken van een Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* Beheerd exemplaar van SQL Server en het doel zijn om ervoor te zorgen dat de aanmeldingen waarmee verbinding wordt gemaakt de bron leden van de serverrol sysadmin.
* Om te controleren dat SSIS in Azure Data Factory (ADF) met Azure-SSIS Integration Runtime (IR) met de bestemming SSISDB wordt gehost door een Azure SQL-Database wordt ingericht beheerd exemplaar (zoals beschreven in het artikel [maken van de Azure-SSIS integratieruntime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)).

## <a name="assess-source-ssis-projectspackages"></a>Bron SSIS-projecten /-pakketten beoordelen

Tijdens de evaluatie van de bron SSISDB is niet nog geïntegreerd in de Database Migration Assistant (DMA), worden uw SSIS-projecten /-pakketten beoordeeld/gevalideerd als ze zijn geïmplementeerd naar de bestemming die SSISDB wordt gehost op een beheerd exemplaar voor Azure SQL Database.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

    ![Portal-abonnementen weergeven](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Selecteer het abonnement waarin u wilt maken van het exemplaar van Azure Database Migration Service, en selecteer vervolgens **resourceproviders**.

    ![Resourceproviders weergeven](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Een Azure Database Migration Service-exemplaar maken

1. Selecteer in de Azure-portal + **Een resource maken**, zoek naar **Azure Database Migration Service** en selecteer vervolgens **Azure Database Migration Service** in de vervolgkeuzelijst.

     ![Azure Marketplace](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer de locatie waarop u het exemplaar van DMS wilt maken.

5. Selecteer een bestaand VNet of maak een.

    Het VNet biedt Azure Database Migration Service toegang tot de bron-SQL Server- en doel-Azure SQL-Database beheerd exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

    Zie het artikel [Netwerktopologieën voor migraties van Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi) voor aanvullende informatie.

6. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    ![DMS-service starten](media/how-to-migrate-ssis-packages-mi/dms-create-service2.png)

7. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat er een exemplaar van de service is gemaakt, zoekt u het exemplaar in de Azure-portal, opent u het en maakt u vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

    ![Zoek alle exemplaren van de Azure Database Migration Service](media/how-to-migrate-ssis-packages-mi/dms-search.png)

2. Zoek in het scherm **Azure Database Migration Service** naar de naam van het exemplaar dat u hebt gemaakt en selecteer vervolgens het exemplaar.

3. Selecteer + **Nieuw migratieproject**.

4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **bronservertype** tekstvak, selecteer **SQL Server**, in de **doelserver type** tekstvak, selecteer **Azure SQL Database Managed Instance**, en vervolgens voor **type activiteit kiezen**, selecteer **SSIS-pakket migratie**.

   ![DMS-project maken](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Selecteer **Maken** om het project te maken.

## <a name="specify-source-details"></a>Geef brondetails op

1. Geef in het scherm **Brongegevens** de verbindingsgegevens op voor de brondatabase van SQL Server.

2. Als u geen vertrouwd certificaat hebt geïnstalleerd op de server, schakelt u het selectievakje **Servercertificaat vertrouwen** in.

    Wanneer er geen vertrouwd certificaat is geïnstalleerd, genereert SQL Server een zelfondertekend certificaat wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die worden versleuteld met behulp van een zelfondertekend certificaat bieden geen sterke beveiliging. Ze zijn vatbaar voor man-in-the-middle-aanvallen. U moet niet vertrouwen op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Brondetails](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Op de **details migratiedoel** scherm, geef de details van de verbinding voor het doel.

     ![Doeldetails](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. Selecteer **Opslaan**.

## <a name="review-the-migration-summary"></a>Migratieoverzicht controleren

1. Geef in het tekstvak **Naam activiteit** van het scherm **Migratieoverzicht** een naam op voor de migratieactiviteit.

2. Voor de **SSIS-projecten en environment(s) overschijvingsoptie**, opgeven of u wilt overschrijven of bestaande SSIS-projecten en omgevingen genegeerd.

    ![Overzicht van migratieproject](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Bekijk en controleer de gegevens van het migratieproject.

## <a name="run-the-migration"></a>De migratie uitvoeren

* Selecteer **Migratie uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de hulp bij de migratie in de Microsoft [handleiding voor databasemigratie](https://datamigration.microsoft.com/).
