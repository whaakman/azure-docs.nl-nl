---
title: Gebruik van de migratie van Azure databaseservice SQL-Server migreren naar Azure SQL Database | Microsoft Docs
description: Informatie over het migreren van SQL Server on-premises naar Azure SQL met behulp van de Service Azure Database migratie.
services: dms
author: HJToland3
ms.author: jtoland
manager: jhubbard
ms.reviewer: 
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 01/24/2018
ms.openlocfilehash: 8dc8b4db80d5e319fad0b681924ab5a8e5642b2e
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="migrate-sql-server-to-azure-sql-database"></a>SQL-Server migreren naar Azure SQL Database
De Service Azure Database migratie kunt u de databases van een lokale SQL Server-instantie migreren naar Azure SQL Database. In deze zelfstudie maakt u de migratie de **Adventureworks2012** database teruggezet naar een lokaal exemplaar van SQL Server 2016 (of hoger) naar een Azure SQL Database met behulp van de Service Azure Database migratie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Evalueer uw lokale database met behulp van de gegevens migratie-assistent.
> * De voorbeeldschema met behulp van de gegevens migratie-assistent migreren.
> * Geen exemplaar maken van de Service Azure Database migratie.
> * Maak een migratieproject met behulp van de Service Azure Database migratie.
> * Voer de migratie uit.
> * De migratie bewaken.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt voltooid, moet u:

- Download en installeer [SQL Server 2016 of hoger](https://www.microsoft.com/sql-server/sql-server-downloads) (alle versies).
- Inschakelen van het TCP/IP-protocol is standaard uitgeschakeld tijdens de installatie van SQL Server Express door de instructies in het artikel [in- of uitschakelen van een Server netwerkprotocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Geen exemplaar maken van Azure SQL Database-instantie die u doen door de details in het artikel [maken van een Azure SQL database in de Azure portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- Download en installeer de [gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
- Een VNET maken voor de Azure-Service voor het migreren van Database met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Netwerkbeveiligingsgroep regels komen de volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over het Azure VNET NSG wordt verkeer gefilterd [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Open Windows firewall zodat de Service Azure Database migratie voor toegang tot de bron van SQL Server.
- Wanneer u een firewallapparaat voor uw databases bron, moet u wellicht toevoegen van firewallregels zodat de Service Azure Database migratie voor toegang tot de bron-databases voor migratie.
- Maken van een serverniveau [firewallregel](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) voor de Azure SQL Database-server zodat de migratie van Azure databaseservice toegang hebben tot de doeldatabases. Geef het subnetbereik van de VNET die wordt gebruikt voor de Azure-Service voor het migreren van Database.
- Zorg ervoor dat de referenties waarmee verbinding met SQL Server-bronexemplaar [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) machtigingen.
- Zorg ervoor dat de referenties waarmee verbinding met Azure SQL Database doelexemplaar machtiging beheer DATABASE op de doel-Azure SQL-databases.

## <a name="assess-your-on-premises-database"></a>Evalueren van uw lokale-database.
Voordat u gegevens vanuit een lokale SQL Server-exemplaar met Azure SQL Database migreren kunt, moet u de SQL Server-database voor tegen blokkerende problemen die mogelijk niet op de migratie. In het migratie-assistent gegevens v3.3 of hoger, volg de stappen in het artikel [uitvoeren van een SQL Server-migratie beoordeling](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) voltooid de on-premises database assessment. Hier volgt een samenvatting van de vereiste stappen:
1.  In de gegevens migratie-assistent, selecteer het pictogram Nieuw (+) en selecteer vervolgens de **Assessment** projecttype.
2.  Geef een projectnaam in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, en klik vervolgens in de **server doeltype** in het tekstvak, selecteer **Azure SQL Database**.
3.  Selecteer **maken** om het project te maken.

    Wanneer u van de SQL Server database migreren naar Azure SQL Database beoordelen, kunt u een of beide van de volgende evaluatie rapporttypen kiezen:
    - Databasecompatibiliteit controleren
    - Functie pariteit controleren

    Beide rapporttypen worden standaard geselecteerd.
4.  In de gegevens migratie-assistent op de **opties** Schakel in het scherm **volgende**.
5.  Op de **bronnen selecteren** scherm in de **verbinding maken met een server** in het dialoogvenster details van de verbinding met uw SQL-Server bieden, en selecteer vervolgens **Connect**.
6.  In de **bronnen toevoegen** dialoogvenster, **AdventureWorks2012**, selecteer **toevoegen**, en selecteer vervolgens **Start Assessment**.

    Wanneer de beoordeling voltooid is, worden de resultaten weergegeven zoals in de volgende afbeelding:

    ![Gegevensmigratie beoordelen](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    Voor Azure SQL Database identificeert de beoordelingen blokkerende problemen van migratie en problemen met de pariteit van functies.

7.  Bekijk de resultaten van de assessment voor migratie blokkerende problemen en functie pariteit problemen door de specifieke opties te selecteren.
    - De **pariteit voor SQL Server-functie** categorie biedt een uitgebreide set met aanbevelingen, alternatieve methoden die beschikbaar zijn in Azure, en de beperkende stappen voor het plannen van de inzet in uw migratie-projecten.
    - De **compatibiliteitsproblemen** categorie identificeert gedeeltelijk ondersteunde of niet-ondersteunde functies die overeenkomen met compatibiliteit van software die mogelijk blokkeren migreren lokale SQL Server-databases naar Azure SQL Database. Aanbevelingen worden ook gegeven om u te helpen bij het aanpakken van die problemen.


## <a name="migrate-the-sample-schema"></a>Het voorbeeldschema migreren
Nadat u vertrouwd met het beoordelen bent en de gegevens migratie-assistent voldaan aan de geselecteerde database is een goede kandidaat voor migratie naar Azure SQL Database gebruiken voor het migreren van het schema met Azure SQL Database.

> [!NOTE]
> Voordat u een migratieproject in Data migratie-assistent maken, zorg er dan voor dat u hebt al een Azure SQL database ingericht zoals vermeld in de vereisten. Voor deze zelfstudie de naam van de Azure SQL Database wordt ervan uitgegaan dat **AdventureWorksAzure**, maar u kunt deze anders naam desgewenst.

Voor het migreren van de **AdventureWorks2012** schema met Azure SQL Database, de volgende stappen uitvoeren:

1.  Selecteer het pictogram Nieuw (+) in de gegevens migratie-assistent, en klik vervolgens onder **projecttype**, selecteer **migratie**.
3.  Geef een projectnaam in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, en klik vervolgens in de **server doeltype** in het tekstvak, selecteer **Azure SQL Database**.
4.  Onder **migratie bereik**, selecteer **Schema only**.

    De migratie-assistent gegevens interface moet worden weergegeven zoals in de volgende afbeelding na het uitvoeren van de vorige stappen:
    
    ![Gegevens migreren Assistant-Project maken](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

5.  Selecteer **maken** om het project te maken.
6.  Geef in de gegevens migratie-assistent, de details van de bron-verbinding voor uw SQL-Server, selecteer **Connect**, en selecteer vervolgens de **AdventureWorks2012** database.

    ![Details van gegevensbron assistent verbinding migratie](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)
7.  Selecteer **volgende**onder **verbinding maken met de doelserver**, geef de details van de doel-verbinding voor de Azure SQL database, selecteert u **Connect**, en selecteer vervolgens de **AdventureWorksAzure** database die u vooraf is ingericht in Azure SQL-database.

    ![Migratie-assistent doel verbinding Gegevensdetails](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)
8.  Selecteer **volgende** om door te gaan naar de **objecten selecteren** scherm waarop kunt u de schemaobjecten in de **AdventureWorks2012** database die moeten worden geïmplementeerd naar Azure SQL-Database.

    Standaard worden alle objecten geselecteerd.

    ![SQL-Scripts genereren](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)
9.  Selecteer **genereren SQL-script** voor het maken van de SQL-scripts en bekijk vervolgens de scripts op fouten.

    ![Schemascript](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)
10. Selecteer **implementeren schema** het schema implementeren naar Azure SQL Database en vervolgens nadat het schema is geïmplementeerd, controleert u de doelserver voor eventuele afwijkingen.

    ![Schema implementeren](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>De registerbronprovider is Microsoft.DataMigration
1. Aanmelden bij de Azure portal, selecteer **alle services**, en selecteer vervolgens **abonnementen**.
 
   ![Portal abonnementen weergeven](media\tutorial-sql-server-to-azure-sql\portal-select-subscription.png)
       
2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.
 
    ![resourceproviders weergeven](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)    
3.  Zoekcriteria voor de migratie en rechts van **Microsoft.DataMigration**, selecteer **registreren**.
 
    ![Resourceprovider registreren](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    


## <a name="create-an-instance"></a>Geen exemplaar maken
1.  Selecteer in de Azure-portal **+ maken van een resource**, zoeken naar Azure Database migratie-Service en selecteer vervolgens **Azure migratie databaseservice** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)
2.  Op de **Migratieservice voor Azure-Database (preview)** Schakel in het scherm **maken**.
 
    ![Azure-Database migratieservice-exemplaar maken](media\tutorial-sql-server-to-azure-sql\dms-create.png)
  
3.  Op de **migratie databaseservice** scherm, Geef een naam voor de service, het abonnement, een virtueel netwerk en de prijscategorie.

    Zie voor meer informatie over de kosten en Prijscategorieën de [pagina met prijzen](https://aka.ms/dms-pricing).

     ![Migratie van Azure databaseservice exemplaar instellingen configureren](media\tutorial-sql-server-to-azure-sql\dms-settings.png)

4.  Selecteer **maken** de service te maken.

## <a name="create-a-migration-project"></a>Een migratieproject maken
Nadat de service is gemaakt, zoeken binnen de Azure-portal en maak vervolgens een migratieproject.
1. Selecteer in de Azure-portal **alle services**, zoeken naar Azure Database migratie-Service en selecteer vervolgens **Azure migratie databaseservices**.
 
      ![Alle exemplaren van de Azure-Service voor het migreren van Database vinden](media\tutorial-sql-server-to-azure-sql\dms-search.png)
2. Op de **Azure migratie databaseservices** scherm gezocht naar de naam van de Azure-DMS-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.
 
     ![Zoek uw exemplaar van de Service Azure Database migreren](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Selecteer **+ nieuw migratieproject**.
4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, en klik vervolgens in de **doel servertype** in het tekstvak, selecteer **Azure SQL Database**.

    ![Database migratie Service-Project maken](media\tutorial-sql-server-to-azure-sql\dms-create-project.png)

5.  Selecteer **maken** om het project te maken.


## <a name="specify-source-details"></a>Geef details van gegevensbron
1. Op de **bron details** scherm, geeft u de verbindingsgegevens voor de bron van SQL Server.

    ![Bron selecteren](media\tutorial-sql-server-to-azure-sql\dms-select-source.png)

2. Selecteer **opslaan**, en selecteer vervolgens de **AdventureWorks2012** database voor de migratie.

    ![Bron DB selecteren](media\tutorial-sql-server-to-azure-sql\dms-select-source-db.png)

## <a name="specify-target-details"></a>Geef Doeldetails op
1. Selecteer **opslaan**, en klik vervolgens op de **doel details** scherm, geeft u de verbindingsgegevens voor het doel, vooraf is ingericht Azure SQL Database waarnaar is de **AdventureWorks2012**  schema met behulp van de gegevens migratie-assistent is geïmplementeerd.

    ![Doel selecteren](media\tutorial-sql-server-to-azure-sql\dms-select-target.png)

2. Selecteer **opslaan** het project op te slaan.
3. Op de **migratie samenvatting** scherm bekijken en controleer de gegevens die zijn gekoppeld aan het migratieproject.

    ![DMS-samenvatting](media\tutorial-sql-server-to-azure-sql\dms-summary.png)

4. Selecteer **Opslaan**.

## <a name="run-the-migration"></a>De migratie uitvoeren
1.  De laatst opgeslagen project selecteert, selecteert u **+ nieuwe activiteit**, en selecteer vervolgens **gegevensmigratie uitvoeren**.

    ![Nieuwe activiteit](media\tutorial-sql-server-to-azure-sql\dms-new-activity.png)

2.  Wanneer u wordt gevraagd, voer de referenties voor de bron- en doelservers en selecteer vervolgens **opslaan**.
3.  Op de **toewijzen aan de doeldatabases** scherm, het toewijzen van de bron- en de doeldatabase voor migratie.

    Als de doeldatabase dezelfde databasenaam als de brondatabase bevat, wordt in Azure DMS de doeldatabase standaard geselecteerd.

    ![Toewijzen aan de doeldatabases](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity.png)

4. Selecteer **opslaan**op de **tabellen selecteren** scherm, vouw de vermelding voor de tabel en bekijk de lijst met de betreffende velden.

    ![Selecteer tabellen](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity.png)

5.  Selecteer **opslaan**op de **migratie samenvatting** scherm in de **activiteitsnaam** tekst, geeft u een naam voor de migratieactiviteit.

    In dit scherm kunt u ook uitbreiden de **validatie optie** scherm, dat u gebruiken kunt om op te geven voor het valideren van de gemigreerde database voor:
    - Schema
    - Gegevensconsistentie
    - Query juist en prestaties

    ![Kies de validatieoptie](media\tutorial-sql-server-to-azure-sql\dms-configuration.png)

6.  Selecteer **opslaan**, controleert u de samenvatting om ervoor te zorgen dat de details van de bron en doel overeenkomen met wat u eerder hebt opgegeven.

    ![Samenvatting van de migratie](media\tutorial-sql-server-to-azure-sql\dms-run-migration.png)

7.  Selecteer **migratie uitvoeren** moet de migratieactiviteit start en selecteer vervolgens **vernieuwen** de huidige status controleren.

    ![Activiteitsstatus](media\tutorial-sql-server-to-azure-sql\dms-activity-status.png)

## <a name="monitor-the-migration"></a>De migratie bewaken
1. Selecteer de migratieactiviteit om de status van de activiteit te controleren.
2. Controleer of de doel-Azure SQL database nadat de migratie voltooid is.

    ![Voltooid](media\tutorial-sql-server-to-azure-sql\dms-completed-activity.png)
