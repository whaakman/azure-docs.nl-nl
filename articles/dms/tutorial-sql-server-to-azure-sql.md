---
title: 'Zelfstudie: Azure Database Migration Service gebruiken voor het offline migreren van SQL Server naar Azure SQL Database | Microsoft Docs'
description: Informatie over het offline migreren van on-premises SQL Server naar Azure SQL Database met behulp van de Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: 80f2a05c5a770043a8ff1da088be2ad4acb16768
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53716443"
---
# <a name="tutorial-migrate-sql-server-to-azure-sql-database-offline-using-dms"></a>Zelfstudie: SQL Server migreren naar een offline exemplaar van Azure SQL Database met behulp van DMS
U kunt de Azure Database Migration Service gebruiken voor het migreren van de databases van een on-premises SQL Server-exemplaar naar [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/). In deze zelfstudie migreert u de database **Adventureworks2012** hersteld naar een on-premises exemplaar van SQL Server 2016 (of hoger) met een Azure SQL Database met behulp van de Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Uw on-premises database evalueren met behulp van de Data Migration Assistant.
> * Het voorbeeldschema migreren met behulp van de Data Migration Assistant.
> * De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> * Een migratieproject maken met behulp van de Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.
> * Een migratierapport downloaden.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

In dit artikel wordt een offline migratie uit SQL Server naar Azure SQL Database beschreven. Zie [SQL Server online migreren naar Azure SQL Database met behulp van DMS](tutorial-sql-server-azure-sql-online.md) voor informatie over een online migratie.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Download en installeer [SQL Server 2016 of hoger](https://www.microsoft.com/sql-server/sql-server-downloads) (alle edities).
- Schakel het TCP/IP-protocol in, dat standaard is uitgeschakeld tijdens de installatie van SQL Server Express, door de instructies in het artikel [In- of uitschakelen van een Server Network Protocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) te volgen.
- Maak een Azure SQL Database-exemplaar, dit doet u door de details in het artikel [Een Azure SQL-database maken in de Azure-portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) te volgen.
- Download en installeer de [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
- Maak een VNET voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Network Security Group-regels de volgende communicatiepoorten niet blokkeren: 443, 53, 9354, 445, 12000. Zie voor meer informatie over verkeer filteren van Azure VNET NSG het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Open uw Windows-firewall voor toegang voor de Azure Database Migration Service tot de SQL Server-bron, die standaard TCP-poort 1433 gebruikt.
- Als u meerdere benoemde SQL Server-exemplaren met behulp van dynamische poorten uitvoert, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
- Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.
- Maak een [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) op serverniveau voor de Azure SQL Database-server om voor de Azure Database Migration Service toegang tot de doeldatabase toe te staan. Geef het subnetbereik van het VNET op dat wordt gebruikt voor de Azure Database Migration Service.
- Zorg ervoor dat de referenties waarmee verbinding wordt gemaakt met het SQL Server-bronexemplaar [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-machtigingen hebben.
- Zorg ervoor dat de referenties waarmee verbinding wordt gemaakt met het Azure SQL Database-doelexemplaar CONTROL DATABASE-machtiging hebben op de Azure SQL-doeldatabases.

## <a name="assess-your-on-premises-database"></a>Uw on-premises resources evalueren
Voordat u gegevens uit een on-premises SQL Server-exemplaar naar Azure SQL Database kunt migreren, moet u de SQL Server-database controleren op blokkerende problemen die mogelijk de migratie verhinderen. Volg met behulp van de Data Migration Assistant v3.3 of hoger de stappen in het artikel [Uitvoeren van een SQL Server-migratie-evaluatie](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) uit om de on-premises database-evaluatie uit te voeren. Hier volgt een samenvatting van de vereiste stappen:
1.  Selecteer het pictogram Nieuw (+) in de Data Migration Assistant, en selecteer vervolgens het projecttype **Evaluatie**.
2.  Geef een projectnaam op, selecteer in het tekstvak **Bronservertype** **SQL Server**, in het tekstvak **Doelservertype** **Azure SQL Database**, en selecteer vervolgens **Maken** om het project te maken.

    Wanneer u het migreren van de SQL Server-brondatabase naar Azure SQL Database beoordeelt, kunt u een of beide van de volgende typen evaluatierapport kiezen:
    - Databasecompatibiliteit controleren
    - Functiepariteit controleren

    Beide rapporttypen zijn standaard geselecteerd.

3.  Selecteer in de Data Migration Assistant, in het scherm **Opties**, **Volgende**.
4.  Geef in het scherm **Bronnen selecteren**, in het dialoogvenster **Verbinding maken met een server**, de verbindingsdetails met uw SQL Server op, en selecteer vervolgens **Verbinding maken**.
5.  Selecteer in het dialoogvenster **Bronnen toevoegen** **AdventureWorks2012**, selecteer **Toevoegen**, en selecteer vervolgens **Evaluatie starten**.

    Wanneer de evaluatie voltooid is, worden de resultaten weergegeven zoals in de volgende afbeelding:

    ![Gegevensmigratie beoordelen](media/tutorial-sql-server-to-azure-sql/dma-assessments.png)

    Voor Azure SQL Database identificeren de evaluaties problemen met de functiepariteit en blokkerende problemen voor migratie.

    - De categorie **SQL Server-functiepariteit** biedt een uitgebreide set met aanbevelingen, alternatieve methoden die beschikbaar zijn in Azure, en beperkende stappen voor het plannen van de inzet in uw migratieprojecten.
    - De categorie **compatibiliteitsproblemen** identificeert gedeeltelijk ondersteunde of niet-ondersteunde functies die compatibiliteitsproblemen laten zien die mogelijk de migratie van on-premises SQL Server-databases naar Azure SQL Database blokkeren. Aanbevelingen om deze problemen op te lossen worden ook gegeven.

6.  Bekijk de resultaten van de evaluatie voor migratieblokkerende problemen en problemen met de functiepariteit door de specifieke opties te selecteren.

## <a name="migrate-the-sample-schema"></a>Het voorbeeldschema migreren
Nadat u vertrouwd met de evaluatie bent en ervan overtuigd bent dat de geselecteerde database een levensvatbare kandidaat voor migratie naar Azure SQL Database is, kunt u de Data Migration Assistant gebruiken voor het migreren van het schema naar Azure SQL Database.

> [!NOTE]
> Voordat u een migratieproject in Data Migration Assistant maakt, moet u ervoor zorgen dat u al een Azure SQL database hebt ingericht zoals vermeld in de vereisten. Voor deze zelfstudie wordt ervan uitgegaan dat de naam van de Azure SQL Database **AdventureWorksAzure** is, maar u kunt elke naam die u wenst opgeven.

Voer voor het migreren van het schema **AdventureWorks2012** naar Azure SQL Database, de volgende stappen uit:

1.  Selecteer het pictogram Nieuw (+) in de Data Migration Assistant, en selecteer vervolgens onder **Projecttype** **Migratie**.
2.  Geef een projectnaam op, selecteer in het tekstvak **Bronservertype** **SQL Server**en in het tekstvak **Doelservertype** **Azure SQL Database**.
3.  Selecteer onder **Migratiebereik**, selecteer **Alleen schema**.

    Na het uitvoeren van de vorige stappen, moet de Data Migration Assistant-interface worden weergegeven zoals in de volgende afbeelding:
    
    ![Data Migration Assistant-Project maken](media/tutorial-sql-server-to-azure-sql/dma-create-project.png)

4.  Selecteer **Maken** om het project te maken.
5.  Geef in de Data Migration Assistant de details van de bronverbinding voor uw SQL Server op, selecteer **Connect**, en selecteer vervolgens de **AdventureWorks2012** database.

    ![Details Data Migration Assistant Bronverbinding](media/tutorial-sql-server-to-azure-sql/dma-source-connect.png)

6.  Selecteer **Volgende** onder **Verbinding maken met doelserver**, geef de details van de doel-verbinding voor de Azure SQL-database op, selecteer **Verbinden** en selecteer vervolgens de **AdventureWorksAzure**-database die u vooraf heeft ingericht in Azure SQL-database.

    ![Details Data Migration Assistant-doelverbinding](media/tutorial-sql-server-to-azure-sql/dma-target-connect.png)

7.  Selecteer **Volgende** om door te gaan naar het scherm **Objecten selecteren** waarin u de schemaobjecten in de **AdventureWorks2012**-database kunt opgeven, die moeten worden geïmplementeerd naar Azure SQL Database.

    Standaard worden alle objecten geselecteerd.

    ![SQL-scripts genereren](media/tutorial-sql-server-to-azure-sql/dma-assessment-source.png)

8.  Selecteer **SQL-script genereren** voor het maken van de SQL-scripts en controleer vervolgens de scripts op fouten.

    ![Schemascript](media/tutorial-sql-server-to-azure-sql/dma-schema-script.png)

9.  Selecteer **Schema implementeren** om het schema te implementeren naar Azure SQL Database, en nadat het schema is geïmplementeerd, controleer de doelserver op eventuele afwijkingen.

    ![Schema implementeren](media/tutorial-sql-server-to-azure-sql/dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider
1. Meld u aan bij de Azure-portal, selecteer **Alle services**, en selecteer vervolgens **Abonnementen**.
 
   ![Portal-abonnementen weergeven](media/tutorial-sql-server-to-azure-sql/portal-select-subscription1.png)
       
2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.
 
    ![Resourceproviders weergeven](media/tutorial-sql-server-to-azure-sql/portal-select-resource-provider.png)
    
3.  Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.
 
    ![Resourceprovider registreren](media/tutorial-sql-server-to-azure-sql/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Een instantie maken
1.  Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-sql-server-to-azure-sql/portal-marketplace.png)

2.  Selecteer in het scherm **Azure Database Migration Service** **Maken**.
 
    ![Azure Database Migration Service-exemplaar maken](media/tutorial-sql-server-to-azure-sql/dms-create1.png)
  
3.  Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken. 

5. Selecteer een bestaand virtueel netwerk (VNET) of maak een nieuwe.

    Het VNET biedt de Azure Database Migration Service toegang tot de bron-SQL Server en het doel-Azure SQL Database-exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNET in de Azure-portal [Een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

6. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    Als u hulp nodig hebt bij het kiezen van de juiste Azure Database Migration Service-categorie, raadpleegt u de aanbevelingen in de berichten [hier](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-sql-server-to-azure-sql/dms-settings2.png)

7.  Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject
Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.
 
      ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-sql-server-to-azure-sql/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.
 
     ![Zoek uw exemplaar van de Azure Database Migration Service](media/tutorial-sql-server-to-azure-sql/dms-instance-search.png)
 
3. Selecteer + **Nieuw migratieproject**.
4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project in het tekstvak **bronservertype**, selecteer **SQL Server**, selecteer in het tekstvak **doelserver type** **Azure SQL Database**, en kies vervolgens **Type activiteit kiezen**, selecteer **Offline gegevensmigratie**. 

    ![Azure Database Migration Service-project maken](media/tutorial-sql-server-to-azure-sql/dms-create-project2.png)

5.  Selecteer **Maken en uitvoeren van de activiteit** om het project te maken en de migratieactiviteit uit te voeren.

## <a name="specify-source-details"></a>Geef brondetails op
1. Geef in het scherm **Migratiebron** de details van de verbinding op voor het bron-SQL Server-exemplaar.
 
    Zorg ervoor dat u een Fully Qualified Domain Name (FQDN) gebruikt voor de naam van het bron-SQL Server-exemplaar. U kunt ook het IP-adres gebruiken voor situaties waarin DNS-naamomzetting niet mogelijk is.

2. Als u geen vertrouwd certificaat op de bronserver hebt geïnstalleerd, selecteert u het selectievakje **Servercertificaat vertrouwen**.

    Wanneer geen vertrouwd certificaat is geïnstalleerd, genereert SQL Server een zelfondertekend certificaat wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die zijn versleuteld met behulp van een zelfondertekend certificaat bieden geen sterke beveiliging. Ze zijn vatbaar voor man-in-the-middle-aanvallen. U moet niet vertrouwen op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Brondetails](media/tutorial-sql-server-to-azure-sql/dms-source-details2.png)

## <a name="specify-target-details"></a>Doeldetails opgeven
1. Selecteer **Opslaan**, en geef vervolgens in het scherm **Details migratiedoel** de details van de verbinding op voor de doel-Azure SQL Database-Server, dit is de vooraf ingerichte Azure SQL-Database waarnaar het  **AdventureWorks2012**-schema is geïmplementeerd met behulp van de Data Migration Assistant.

    ![Doel selecteren](media/tutorial-sql-server-to-azure-sql/dms-select-target2.png)

2. Selecteer **Opslaan**, en klik vervolgens in het scherm **Toewijzen aan doeldatabases**, wijs de bron- en de doeldatabase voor de migratie toe.

    Als de doeldatabase de naam van de dezelfde database als de bron-database bevat, wordt in de Azure Database Migration Service de doeldatabase standaard geselecteerd.

    ![Toewijzen aan doeldatabases](media/tutorial-sql-server-to-azure-sql/dms-map-targets-activity2.png)

3. Selecteer **Opslaan**in het scherm **Tabellen selecteren**, vouw de tabellenlijst uit en controleer de lijst met betrokken velden.

    Houd er rekening mee dat de Azure Database Migration Service automatisch alle lege brontabellen selecteert die aanwezig zijn op het doel-Azure SQL Database-exemplaar. Als u tabellen wilt terug migreren die al gegevens bevatten, moet u expliciet de tabellen op deze blade selecteren.

    ![Selecteer tabellen](media/tutorial-sql-server-to-azure-sql/dms-configure-setting-activity2.png)

4.  Selecteer **Opslaan**in het scherm **Migratieoverzicht**, geef in het tekstvak **Activiteitennaam** een naam op voor de migratieactiviteit.

5. Vouw de sectie **Validatieoptie** uit om het scherm **Validatieoptie kiezen** weer te geven en geef op of de gemigreerde databases voor **Schemavergelijking**, **Gegevensconsistentie**, en **Query uitvoeren op juistheid**moeten worden gevalideerd.
    
    ![Validatieoptie kiezen](media/tutorial-sql-server-to-azure-sql/dms-configuration2.png)

6.  Selecteer **Opslaan**, controleer de samenvatting om ervoor te zorgen dat de gegevens van de bron en het doel overeenkomen met wat u eerder hebt opgegeven.

    ![Migratieoverzicht](media/tutorial-sql-server-to-azure-sql/dms-run-migration2.png)

## <a name="run-the-migration"></a>De migratie uitvoeren
- Selecteer **Migratie uitvoeren**.

    Het venster van de migratieactiviteit wordt weergegeven, en de **Status** van de activiteit is **In behandeling**.

    ![Activiteitenstatus](media/tutorial-sql-server-to-azure-sql/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Bewaak de migratie
1. Selecteer op het scherm van de activiteitenmigratie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Voltooid** wordt weergegeven.

    ![Activiteitenstatus voltooid](media/tutorial-sql-server-to-azure-sql/dms-completed-activity1.png)

2. Nadat de migratie is voltooid, selecteert u **Rapport downloaden** om een rapport op te halen met gegevens die zijn gekoppeld aan het migratieproces.

3. Controleer de doel-database (s) op de doel-Azure SQL Database-server.

### <a name="additional-resources"></a>Aanvullende resources

 * [SQL-migratie met behulp van Azure Data Migration Service (DMS)](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587) hands-on lab.