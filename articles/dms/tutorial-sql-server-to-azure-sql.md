---
title: Gebruik van de migratie van Azure databaseservice SQL-Server migreren naar Azure SQL Database | Microsoft Docs
description: Informatie over het migreren van SQL Server on-premises naar Azure SQL Database met behulp van de Service Azure Database migratie.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 06/08/2018
ms.openlocfilehash: f64b2922818eddcab02f7d1c7b8f97671d92589e
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850250"
---
# <a name="migrate-sql-server-to-azure-sql-database-using-dms"></a>SQL-Server migreren naar Azure SQL Database met DMS
U kunt de Azure-Service voor het migreren van Database voor het migreren van de databases van een lokale SQL Server-exemplaar naar [Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/). In deze zelfstudie maakt u de migratie de **Adventureworks2012** database teruggezet naar een lokaal exemplaar van SQL Server 2016 (of hoger) naar een Azure SQL Database met behulp van de Service Azure Database migratie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Evalueer uw lokale database met behulp van de gegevens migratie-assistent.
> * De voorbeeldschema met behulp van de gegevens migratie-assistent migreren.
> * Geen exemplaar maken van de Service Azure Database migratie.
> * Maak een migratieproject met behulp van de Service Azure Database migratie.
> * Voer de migratie uit.
> * De migratie bewaken.
> * Download een rapport voor de migratie.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt voltooid, moet u:

- Download en installeer [SQL Server 2016 of hoger](https://www.microsoft.com/sql-server/sql-server-downloads) (alle versies).
- Inschakelen van het TCP/IP-protocol is standaard uitgeschakeld tijdens de installatie van SQL Server Express door de instructies in het artikel [in- of uitschakelen van een Server netwerkprotocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Geen exemplaar maken van Azure SQL Database-instantie die u doen door de details in het artikel [maken van een Azure SQL database in de Azure portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- Download en installeer de [gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
- Een VNET maken voor de Azure-Service voor het migreren van Database met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Netwerkbeveiligingsgroep regels komen de volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over het Azure VNET NSG wordt verkeer gefilterd [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Open Windows firewall zodat de Service Azure Database migratie voor toegang tot de bron van SQL Server, die standaard TCP-poort 1433.
- Als u meerdere benoemde exemplaren van SQL Server met behulp van dynamische poorten worden uitgevoerd, kunt u desgewenst de SQL Browser-Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de migratie van Azure databaseservice verbinding met een benoemd exemplaar op de bron maken kan -Server.
- Wanneer u een firewallapparaat voor uw databases bron, moet u wellicht toevoegen van firewallregels zodat de Service Azure Database migratie voor toegang tot de bron-databases voor migratie.
- Maken van een serverniveau [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor de Azure SQL Database-server zodat de migratie van Azure databaseservice toegang hebben tot de doeldatabases. Geef het subnetbereik van de VNET die wordt gebruikt voor de Azure-Service voor het migreren van Database.
- Zorg ervoor dat de referenties waarmee verbinding met SQL Server-bronexemplaar [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) machtigingen.
- Zorg ervoor dat de referenties waarmee verbinding met Azure SQL Database doelexemplaar machtiging beheer DATABASE op de doel-Azure SQL-databases.

## <a name="assess-your-on-premises-database"></a>Evalueren van uw lokale-database.
Voordat u gegevens vanuit een lokale SQL Server-exemplaar met Azure SQL Database migreren kunt, moet u de SQL Server-database voor tegen blokkerende problemen die mogelijk niet op de migratie. Met behulp van de migratie-assistent gegevens v3.3 of hoger, volg de stappen in het artikel [uitvoeren van een SQL Server-migratie beoordeling](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) voltooid de on-premises database assessment. Hier volgt een samenvatting van de vereiste stappen:
1.  In de gegevens migratie-assistent, selecteer het pictogram Nieuw (+) en selecteer vervolgens de **Assessment** projecttype.
2.  Geef een projectnaam in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, in de **server doeltype** in het tekstvak, selecteer **Azure SQL Database**, en selecteer vervolgens **maken** om het project te maken.

    Wanneer u van de SQL Server database migreren naar Azure SQL Database beoordelen, kunt u een of beide van de volgende evaluatie rapporttypen kiezen:
    - Databasecompatibiliteit controleren
    - Functiepariteit controleren

    Beide rapporttypen worden standaard geselecteerd.

3.  In de gegevens migratie-assistent op de **opties** Schakel in het scherm **volgende**.
4.  Op de **bronnen selecteren** scherm in de **verbinding maken met een server** in het dialoogvenster details van de verbinding met uw SQL-Server bieden, en selecteer vervolgens **Connect**.
5.  In de **bronnen toevoegen** dialoogvenster, **AdventureWorks2012**, selecteer **toevoegen**, en selecteer vervolgens **Start Assessment**.

    Wanneer de beoordeling voltooid is, worden de resultaten weergegeven zoals in de volgende afbeelding:

    ![Gegevensmigratie beoordelen](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    Voor Azure SQL Database identificeert de beoordelingen functie pariteit problemen en problemen met blokkeringen van migratie.

    - De **pariteit voor SQL Server-functie** categorie biedt een uitgebreide set met aanbevelingen, alternatieve methoden die beschikbaar zijn in Azure, en de beperkende stappen voor het plannen van de inzet in uw migratie-projecten.
    - De **compatibiliteitsproblemen** categorie identificeert gedeeltelijk ondersteunde of niet-ondersteunde functies die overeenkomen met compatibiliteit van software die mogelijk blokkeren migreren lokale SQL Server-databases naar Azure SQL Database. Aanbevelingen worden ook gegeven om u te helpen bij het aanpakken van die problemen.

6.  Bekijk de resultaten van de assessment voor migratie blokkerende problemen en functie pariteit problemen door de specifieke opties te selecteren.

## <a name="migrate-the-sample-schema"></a>Het voorbeeldschema migreren
Nadat u vertrouwd met het beoordelen bent en de gegevens migratie-assistent ervan overtuigd is dat de geselecteerde database levensvatbaar geschikt is voor migratie naar Azure SQL Database gebruiken voor het migreren van het schema met Azure SQL Database.

> [!NOTE]
> Voordat u een migratieproject in Data migratie-assistent maken, zorg er dan voor dat u hebt al een Azure SQL database ingericht zoals vermeld in de vereisten. Voor deze zelfstudie de naam van de Azure SQL Database wordt ervan uitgegaan dat **AdventureWorksAzure**, maar u kunt opgeven welke naam u wenst.

Voor het migreren van de **AdventureWorks2012** schema met Azure SQL Database, de volgende stappen uitvoeren:

1.  Selecteer het pictogram Nieuw (+) in de gegevens migratie-assistent, en klik vervolgens onder **projecttype**, selecteer **migratie**.
2.  Geef een projectnaam in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, en klik vervolgens in de **server doeltype** in het tekstvak, selecteer **Azure SQL Database**.
3.  Onder **migratie bereik**, selecteer **Schema only**.

    De migratie-assistent gegevens interface moet worden weergegeven zoals in de volgende afbeelding na het uitvoeren van de vorige stappen:
    
    ![Gegevens migreren Assistant-Project maken](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

4.  Selecteer **maken** om het project te maken.
5.  Geef in de gegevens migratie-assistent, de details van de bron-verbinding voor uw SQL-Server, selecteer **Connect**, en selecteer vervolgens de **AdventureWorks2012** database.

    ![Details van gegevensbron assistent verbinding migratie](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)

6.  Selecteer **volgende**onder **verbinding maken met de doelserver**, geef de details van de doel-verbinding voor de Azure SQL database, selecteert u **Connect**, en selecteer vervolgens de **AdventureWorksAzure** database die u vooraf is ingericht in Azure SQL-database.

    ![Migratie-assistent doel verbinding Gegevensdetails](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)

7.  Selecteer **volgende** om door te gaan naar de **objecten selecteren** scherm waarop kunt u de schemaobjecten in de **AdventureWorks2012** database die moeten worden geïmplementeerd naar Azure SQL-Database.

    Standaard worden alle objecten geselecteerd.

    ![SQL-Scripts genereren](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)

8.  Selecteer **genereren SQL-script** voor het maken van de SQL-scripts en bekijk vervolgens de scripts op fouten.

    ![Schemascript](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)

9.  Selecteer **implementeren schema** het schema implementeren naar Azure SQL Database en vervolgens nadat het schema is geïmplementeerd, controleert u de doelserver voor eventuele afwijkingen.

    ![Schema implementeren](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>De registerbronprovider is Microsoft.DataMigration
1. Aanmelden bij de Azure portal, selecteer **alle services**, en selecteer vervolgens **abonnementen**.
 
   ![Portal abonnementen weergeven](media\tutorial-sql-server-to-azure-sql\portal-select-subscription1.png)
       
2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.
 
    ![resourceproviders weergeven](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)
    
3.  Zoekcriteria voor de migratie en rechts van **Microsoft.DataMigration**, selecteer **registreren**.
 
    ![Resourceprovider registreren](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Geen exemplaar maken
1.  Selecteer in de Azure-portal + **maken van een resource**, zoeken naar Azure Database migratie-Service en selecteer vervolgens **Azure migratie databaseservice** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)

2.  Op de **Azure migratie databaseservice** Schakel in het scherm **maken**.
 
    ![Azure-Database migratieservice-exemplaar maken](media\tutorial-sql-server-to-azure-sql\dms-create1.png)
  
3.  Op de **migratieservice maken** scherm, Geef een naam voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer een bestaand virtueel netwerk (VNET) of een nieuwe maken.

    Het VNET biedt de Service Azure Database migratie met toegang tot de SQL Server-bron en het doel-Azure SQL Database-exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNET in de Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

5. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en Prijscategorieën de [pagina met prijzen](https://aka.ms/dms-pricing).

    Als u hulp bij het kiezen van de juiste Azure Database migratie-servicelaag nodig hebt, raadpleegt u de aanbevelingen in het boeken [hier](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Migratie van Azure databaseservice exemplaar instellingen configureren](media\tutorial-sql-server-to-azure-sql\dms-settings1.png)

6.  Selecteer **maken** de service te maken.

## <a name="create-a-migration-project"></a>Een migratieproject maken
Nadat de service is gemaakt, zoeken binnen de Azure-portal, opent u het en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **alle services**, zoeken naar Azure Database migratie-Service en selecteer vervolgens **Azure migratie databaseservices**.
 
      ![Alle exemplaren van de Azure-Service voor het migreren van Database vinden](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Op de **Azure migratie databaseservices** scherm gezocht naar de naam van de Azure-Service voor het migreren van Database-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.
 
     ![Zoek uw exemplaar van de Service Azure Database migreren](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Selecteer + **nieuw migratieproject**.
4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **server gegevensbrontype** in het tekstvak, selecteer **SQL Server**, en klik vervolgens in de **doel servertype** in het tekstvak, selecteer **Azure SQL Database**.

    ![Database migratie Service-Project maken](media\tutorial-sql-server-to-azure-sql\dms-create-project1.png)

5.  Selecteer **maken** om het project te maken.

## <a name="specify-source-details"></a>Geef details van gegevensbron
1. Op de **bron details** scherm, geeft u de verbindingsgegevens voor de bron SQL Server-exemplaar.
 
    Zorg ervoor dat een Fully Qualified Domain Name (FQDN) gebruiken voor de bronnaam van de SQL Server-exemplaar. U kunt ook het IP-adres gebruiken voor situaties waarin DNS-naamomzetting niet mogelijk is.

2. Als u een vertrouwd certificaat op de bronserver niet hebt geïnstalleerd, selecteert u de **vertrouwensrelatie servercertificaat** selectievakje.

    Wanneer een vertrouwd certificaat niet is geïnstalleerd, wordt door SQL Server een zelfondertekend certificaat gegenereerd wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die met behulp van een zelfondertekend certificaat encyopted beschikbaar sterke beveiliging niet. Ze zijn vatbaar voor man-in-the-middle-aanvallen. Vertrouw niet op SSL zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Details van gegevensbron](media\tutorial-sql-server-to-azure-sql\dms-source-details1.png)
  
2. Selecteer **opslaan**, en selecteer vervolgens de **AdventureWorks2012** database voor de migratie.

    ![Bron DB selecteren](media\tutorial-sql-server-to-azure-sql\dms-select-source-db1.png)

## <a name="specify-target-details"></a>Geef Doeldetails op
1. Selecteer **opslaan**, en klik vervolgens op de **doel details** scherm, geeft u de verbindingsgegevens voor het doel-Azure SQL Database-Server, die vooraf zijn ingericht Azure SQL Database waarnaar de **AdventureWorks2012** schema met behulp van de gegevens migratie-assistent is geïmplementeerd.

    ![Doel selecteren](media\tutorial-sql-server-to-azure-sql\dms-select-target1.png)

2. Selecteer **opslaan** het project op te slaan.

3. Op de **Projectsamenvatting** scherm bekijken en controleer de gegevens die zijn gekoppeld aan het migratieproject.

    ![DMS-samenvatting](media\tutorial-sql-server-to-azure-sql\dms-summary1.png)

4. Selecteer **Opslaan**.

## <a name="run-the-migration"></a>De migratie uitvoeren
1.  De laatst opgeslagen project selecteert, selecteert u + **nieuwe activiteit**, en selecteer vervolgens **migratie uitvoeren**.

    ![Nieuwe activiteit](media\tutorial-sql-server-to-azure-sql\dms-new-activity1.png)

2.  Wanneer u wordt gevraagd, voer de referenties voor de bron- en doelservers en selecteer vervolgens **opslaan**.

3.  Op de **toewijzen aan de doeldatabases** scherm, het toewijzen van de bron- en de doeldatabase voor migratie.

    Als de doeldatabase dezelfde databasenaam als de brondatabase bevat, wordt in Azure DMS de doeldatabase standaard geselecteerd.

    ![Toewijzen aan doeldatabases](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity1.png)

4. Selecteer **opslaan**op de **tabellen selecteren** scherm, vouw de tabel met en bekijk de lijst met velden betrokken.

    Houd er rekening mee dat de automatische migratie van Azure databaseservice selecteert alle leeg brontabellen die aanwezig zijn op de doel-Azure SQL Database-exemplaar. Als u migreren van tabellen die al gegevens bevatten wilt, moet u expliciet de tabellen op deze blade te selecteren.

    ![Selecteer tabellen](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity1.png)

5.  Selecteer **opslaan**op de **migratie samenvatting** scherm in de **activiteitsnaam** tekst, geeft u een naam voor de migratieactiviteit.

6. Vouw de **validatieoptie** sectie om weer te geven de **validatie optie** scherm, Geef op of de gemigreerde databases voor valideren **Schema vergelijking**, **Gegevensconsistentie**, en **query uitvoeren op juistheid**.
    
    ![De validatieoptie kiezen](media\tutorial-sql-server-to-azure-sql\dms-configuration1.png)

6.  Selecteer **opslaan**, controleert u de samenvatting om ervoor te zorgen dat de details van de bron en doel overeenkomen met wat u eerder hebt opgegeven.

    ![Samenvatting van de migratie](media\tutorial-sql-server-to-azure-sql\dms-run-migration1.png)

7.  Selecteer **migratie uitvoeren**.

    Het venster van de activiteit migratie wordt weergegeven, en de **Status** van de activiteit is **in behandeling**.

    ![Activiteitsstatus](media\tutorial-sql-server-to-azure-sql\dms-activity-status1.png)

## <a name="monitor-the-migration"></a>De migratie bewaken
1. Selecteer op het scherm van de activiteit migratie **vernieuwen** bijwerken van de weergave totdat de **Status** van de migratie wordt weergegeven als **voltooid**.

    ![De Status van de activiteit is voltooid](media\tutorial-sql-server-to-azure-sql\dms-completed-activity1.png)

2. Nadat de migratie is voltooid, selecteert u **Download rapport** ophalen van een rapport met gegevens die zijn gekoppeld aan het migratieproces.

3. Controleer of de doel-databases op de doel-Azure SQL Database-server.

### <a name="additional-resources"></a>Aanvullende resources

 * [SQL-migratie met behulp van Azure Data Migration Service (DMS)](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587) praktische testomgeving.