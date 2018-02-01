---
title: Een Azure SSIS-integratieruntime inrichten met behulp van de portal| Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een Azure SSIS-integratieruntime kunt maken met behulp van Azure Portal.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: spelluru
ms.openlocfilehash: 281fe65393086ec6a04dcba5aae868f4fec097ad
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="provision-an-azure-ssis-integration-runtime-by-using-the-data-factory-ui"></a>Een Azure SSIS-integratieruntime inrichten met behulp van de Data factory-UI
Deze zelfstudie bevat de stappen voor het gebruik van Azure Portal om een Azure SSIS-integratieruntime (IR) in te richten in Azure Data Factory. Vervolgens kunt u SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) gebruiken om pakketten van SQL Server Integration Services (SSIS) te implementeren in deze runtime van Azure. Zie [Overzicht van Integration Runtime in Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime) voor algemene informatie over Azure-SSIS IR.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure SSIS-integratieruntime maken en starten

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint. 
- **Azure SQL Database-server**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. De SSISDB (SSIS Catalog database) wordt met Azure Data Factory in deze databaseserver gemaakt. Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. 
    - Controleer of de instelling **Toegang tot Azure-services toestaan** is ingeschakeld voor de databaseserver. Zie [Secure your Azure SQL database](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal) (Azure SQL-database beveiligen) voor meer informatie. Zie [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1) om deze instelling met behulp van PowerShell in te schakelen.
    - Voeg het IP-adres van de clientcomputer (of een reeks IP-adressen dat het IP-adres van de clientcomputer bevat) toe aan de lijst met client-IP-adressen in de instellingen van de firewall voor de databaseserver. Zie [Overzicht van de firewallregels voor Azure SQL Database](../sql-database/sql-database-firewall-configure.md) voor meer informatie.
    - Controleer of uw Azure SQL Database-server geen SSIS-catalogus (SSIDB-database) heeft. Het inrichten van Azure-SSIS-IR ondersteunt niet het gebruik van een bestaande SSIS-catalogus.
 
## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
2. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Voer op de pagina **Nieuwe gegevensfactory** **MyAzureSsisDataFactory** in als de **naam**. 
      
     ![De pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamMyAzureSsisDataFactory) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de lijst zie u alleen locaties die worden ondersteund voor het maken van gegevensfactory’s.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Klik op **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Een Azure SSIS-integratieruntime inrichten

1. Klik op de pagina Aan de slag op de tegel **SSIS-integratieruntime configureren**. 

   ![De tegel SSIS-integratieruntime configureren](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Voer op de pagina **Algemene instellingen** van **Installatie van integratieruntime** de volgende stappen uit: 

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Geef een **naam** op voor de zelf-hostende integratieruntime.
    2. Geef de **locatie** op voor de zelf-hostende integratieruntime. Alleen ondersteunde locaties worden weergegeven.
    3. Selecteer de **grootte van het knooppunt** dat moet worden geconfigureerd met de SSIS-runtime.
    4. Geef het **aantal knooppunten** in het cluster op.
    5. Klik op **Volgende**. 
1. Voer in de **SQL-instellingen** de volgende stappen uit: 

    ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Geef het **Azure-abonnement** op dat de Azure SQL-server bevat. 
    2. Selecteer uw Azure SQL-server als het **eindpunt voor de Catalog-databaseserver**.
    3. Voer de gebruikersnaam van de **beheerder** in.
    4. Voer het **wachtwoord** voor de beheerder in.  
    5. Selecteer de **servicelaag** voor de SSISDB-database. De standaardwaarde is Basic.
    6. Klik op **Volgende**. 
1.  Selecteer op de pagina **Geavanceerde instellingen** een waarde voor het **maximale aantal parallelle uitvoeringen per knooppunt**.   

    ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Deze stap is **optioneel**. Als u een klassiek VNet hebt (virtueel netwerk) waaraan u de integratieruntime wilt koppelen, selecteert u de optie voor het **selecteren van een VNet om uw Azure SSIS-integratieruntime aan te koppelen en om toe te staan dat via Azure-services VNet-machtigingen/-instellingen worden geconfigureerd**. Voer hierna de volgende stappen uit: 

    ![Geavanceerde instellingen met VNet](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Geef het **abonnement** op dat het klassieke VNet bevat. 
    2. Selecteer het **VNet**. <br/>
    4. Selecteer het **Subnet**.<br/> 
1. Klik op **Voltooien** om te beginnen met het maken van de Azure SSIS-integratieruntime. 

    > [!IMPORTANT]
    > - Het duurt ongeveer 20 minuten om dit proces te voltooien
    > - De Data Factory-service maakt verbinding met uw Azure SQL-database om de SSISDB (SSIS Catalog Database) voor te bereiden. Het script configureert ook machtigingen en instellingen voor uw VNet, indien opgegeven, en verbindt de nieuwe instantie van de Azure SSIS Integration Runtime met het VNet.
    > - Wanneer u een exemplaar van de SQL-database inricht voor het hosten van SSISDB, worden ook het Azure-functiepakket voor SSIS en het te distribueren Azure-pakket geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel- en Access-bestanden en met verschillende Azure-gegevensbronnen, naast de gegevensbronnen die worden ondersteund door de ingebouwde onderdelen. Onderdelen van derden voor SSIS kunnen op dit moment niet worden geïnstalleerd (met inbegrip van onderdelen van derden van Microsoft, zoals de Oracle- en Teradata-onderdelen van Attunity en de SAP BI-onderdelen).

7. Ga in het venster **Verbindingen** naar **Integratieruntimes**, indien nodig. Klik op **Vernieuwen** om de status te vernieuwen. 

    ![Status van het maken](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Gebruik de koppelingen onder de kolom **Acties** om de integratieruntime te controleren, te stoppen/starten, te bewerken of te verwijderen. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

    ![Azure SSIS-IR - acties](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Klik op de koppeling **Controleren** onder **Acties**.  

    ![Azure SSIS-IR - details](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Als er een **fout** optreedt die is gerelateerd aan de Azure SSIS-IR, ziet u het aantal fouten op deze pagina en een koppeling naar de details over deze fout. Als de SSIS Catalog al bestaat op de databaseserver, ziet u bijvoorbeeld een foutmelding waarin het bestaan van de SSISDB-database wordt aangegeven.  
11. Klik bovenaan op **Integratieruntimes** om terug te gaan naar de vorige pagina met alle integratieruntimes die zijn gekoppeld aan de gegevensfactory.  

## <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS-integratieruntimes in de portal

1. Ga in de Azure Data Factory-UI naar het tabblad **Bewerken**, klik op **Verbindingen** en ga vervolgens naar het tabblad **Integratieruntimes** om bestaande integratieruntimes in uw gegevensfactory weer te geven. 
    ![Bestaande IR’s weergeven](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Klik op **Nieuw** om een nieuwe Azure SSIS-IR te maken. 

    ![Integratieruntime via menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Als u een Azure SSIS-integratieruntime wilt maken, klikt u op **Nieuw** zoals wordt weergegeven in de afbeelding. 
3. Selecteer in het venster Installatie van integratieruntime de optie voor het **verplaatsen van bestaande SSIS-pakketten voor uitvoering in Azure** en klik vervolgens op **Volgende**.

    ![Geef het type integratieruntime op](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Zie de sectie [Een Azure SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure SSIS-IR. 

 
## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Maak verbinding met de Azure SQL-server waarop de SSIS-catalogus (SSISDB) is opgeslagen. De naam van de Azure SQL-server moet deze indeling hebben: `<servername>.database.windows.net` (voor Azure SQL Database). 

Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met een SSIS-catalogus in Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie) 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd: 

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure SSIS-integratieruntime maken en starten

Ga naar de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
>[Gegevens in de cloud kopiëren](tutorial-copy-data-portal.md)
