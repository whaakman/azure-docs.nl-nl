---
title: SSIS-pakketten implementeren in Azure | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u SSIS-pakketten implementeert in een Azure SSIS Integration Runtime van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: hero-article
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: cc0c26d83794cfb0b398e668ae89e268901df345
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>Pakketten van SQL Server Integration Services implementeren in Azure
Deze zelfstudie bevat de stappen voor het gebruik van Azure Portal om een Azure SSIS-integratieruntime (IR) in te richten in Azure Data Factory. Vervolgens kunt u SQL Server Data Tools of SQL Server Management Studio gebruiken om pakketten van SQL Server Integration Services (SSIS) te implementeren in deze runtime van Azure. Zie [Overzicht van integratieruntime in Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime) voor algemene informatie over een Azure-SSIS IR.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure-SSIS-integratieruntime inrichten.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u de [documentatie voor versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint. 
- **Azure SQL Database-server**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Azure Data Factory maakt de SSIS-catalogus (SSISDB-database) op deze databaseserver. Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de integratieruntime uitvoeringslogboeken wegschrijven naar de SSISDB-database zonder dat hierbij Azure-regio's worden overschreden. 
- Controleer of de instelling **Toegang tot Azure-services toestaan** is ingeschakeld voor de databaseserver. Zie [Secure your Azure SQL database](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal) (Azure SQL-database beveiligen) voor meer informatie. Zie [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1) om deze instelling met behulp van PowerShell in te schakelen.
- Voeg het IP-adres van de clientcomputer (of een reeks IP-adressen dat het IP-adres van de clientcomputer bevat) toe aan de lijst met client-IP-adressen in de instellingen van de firewall voor de databaseserver. Zie [Overzicht van de firewallregels voor Azure SQL Database](../sql-database/sql-database-firewall-configure.md) voor meer informatie.
- Controleer of uw Azure SQL Database-server geen SSIS-catalogus (SSISDB-database) heeft. Het inrichten van een Azure-SSIS-IR biedt geen ondersteuning voor het gebruik van een bestaande SSIS-catalogus.

> [!NOTE]
> - U kunt een gegevensfactory van versie 2 maken in de volgende regio's: VS - oost, VS - oost 2, Zuidoost-Azië en West-Europa. 
> - U kunt een Azure-SSIS-IR maken in de volgende regio's: VS - oost, VS - oost 2, VS - midden, VS - west 2, Noord-Europa, West-Europa, UK - zuid en Australië - oost. 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
3. Selecteer **Nieuw** in het menu links, selecteer **Gegevens en analyses** en selecteer vervolgens **Data Factory**. 
   
   ![Selectie van Data Factory in het deelvenster Nieuw](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Voer op de pagina **Nieuwe gegevensfactory** **MyAzureSsisDataFactory** in voor **Name**. 
      
   ![Pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet *wereldwijd uniek* zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld in **&lt;uwnaam&gt;MyAzureSsisDataFactory**) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor naamgevingsregels voor Data Factory-artefacten.
  
   `Data factory name “MyAzureSsisDataFactory” is not available`
4. Selecteer voor **Abonnement** het Azure-abonnement waarin u de gegevensfactory wilt maken. 
5. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de lijst. 
   - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
   Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
6. Selecteer **V2 (Preview)** voor **Versie**.
7. Selecteer voor **Locatie** een locatie voor de data factory. De lijst bevat alleen locaties die worden ondersteund voor het maken van gegevensfactory’s.
8. Selecteer **Vastmaken aan dashboard**.     
9. Selecteer **Maken**.
10. Op het dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**: 

   ![Tegel Data Factory implementeren](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
11. Zodra de bewerking is voltooid, ziet u de pagina **Data Factory**.
   
   ![Startpagina voor de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
12. Selecteer **Maken en controleren** om de gebruikersinterface van Data Factory te openen op een nieuw tabblad. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Een Azure-SSIS-integratieruntime inrichten

1. Selecteer op de pagina **Aan de slag** de tegel **SSIS-integratieruntime configureren**. 

   ![De tegel SSIS-integratieruntime configureren](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Voer op de pagina **Algemene instellingen** van **Installatie van integratieruntime** de volgende stappen uit: 

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Geef bij **Naam** een naam op voor de integratieruntime.

   b. Geef bij **Locatie** de locatie op voor de integratieruntime. Alleen ondersteunde locaties worden weergegeven.

   c. Selecteer bij **Knooppuntgrootte** de grootte van het knooppunt dat moet worden geconfigureerd met de SSIS-runtime.

   d. Geef bij **Knooppuntnummer** het aantal knooppunten in het cluster op.
   
   e. Selecteer **Volgende**. 
3. Voer op de pagina **SQL-instellingen** de volgende stappen uit: 

   ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Geef bij **Abonnement** het Azure-abonnement op dat de Azure-databaseserver bevat.

   b. Selecteer bij **Eindpunt voor de Catalog-databaseserver** uw Azure-databaseserver.

   c. Voer bij **Gebruikersnaam van beheerder** de gebruikersnaam van de beheerder in.

   d. Voer bij **Beheerderswachtwoord** het wachtwoord voor de beheerder in.

   e. Selecteer bij **Serverlaag catalogusdatabase** de servicelaag voor de SSISDB-database. De standaardwaarde is **Basic**.

   f. Selecteer **Volgende**. 
4. Selecteer op de pagina **Geavanceerde instellingen** een waarde voor het **maximale aantal parallelle uitvoeringen per knooppunt**.   

   ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Deze stap is *optioneel*. Als u een klassiek netwerk hebt waaraan u de integratieruntime wilt koppelen (gemaakt via het klassieke implementatiemodel of via Azure Resource Manager), selecteert u de optie voor het**selecteren van een VNet om uw Azure-SSIS-integratieruntime aan te koppelen en om toe te staan dat via Azure-services VNet-machtigingen/-instellingen** worden geconfigureerd. Voer hierna de volgende stappen uit: 

   ![Geavanceerde instellingen met een virtueel netwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

   a. Geef bij **Abonnement** het Azure-abonnement op dat de Azure-databaseserver bevat.

   b. Selecteer bij **VNet-naam** de naam van het virtuele netwerk.

   c. Selecteer bij **Subnetnaam** de naam van het subnet in het virtuele netwerk. 
6. Selecteer **Voltooien** om te beginnen met het maken van de Azure-SSIS-integratieruntime. 

   > [!IMPORTANT]
   > Dit proces duurt ongeveer 20 minuten.
   >
   > De Data Factory-service maakt verbinding met uw Azure SQL-database om de SSIS-catalogus (SSISDB-database) voor te bereiden. Het script configureert ook machtigingen en instellingen voor het virtuele netwerk, als die zijn opgegeven. Daarnaast wordt het nieuwe exemplaar van de Azure-SSIS-integratieruntime gekoppeld aan het virtuele netwerk.
   > 
   > Wanneer u een exemplaar van een Azure-SSI-IR inricht, worden ook het Azure Feature Pack voor SSIS en de Access Redistributable geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel- en Access-bestanden en met verschillende Azure-gegevensbronnen, naast de gegevensbronnen die worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren. Zie [Custom setup for the Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Aangepaste instelling voor de Azure-SSIS-integratieruntime) voor meer informatie.

7. Ga op het tabblad **Verbindingen** indien nodig naar **Integratieruntimes**. Selecteer **Vernieuwen** om de status te vernieuwen. 

   ![De status vernieuwen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Gebruik de koppelingen in de kolom **Acties** om de integratieruntime te stoppen/starten, te bewerken of te verwijderen. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

   ![Koppelingen in de kolom 'Acties'](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        

## <a name="create-an-azure-ssis-integration-runtime"></a>Een Azure SSIS Integration Runtime maken

1. Ga in de gebruikersinterface van Azure Data Factory naar het tabblad **Bewerken**, selecteer **Verbindingen** en ga vervolgens naar het tabblad **Integratieruntimes** om bestaande integratieruntimes in uw gegevensfactory weer te geven. 
   ![Selecties voor het weergeven van bestaande IR's](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
2. Selecteer **Nieuw** om een Azure-SSIS-IR te maken. 

   ![Integratieruntime via menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
3. Selecteer in het venster **Installatie van integratieruntime** de**optie voor het verplaatsen van bestaande SSIS-pakketten voor uitvoering in Azure** en selecteer vervolgens **Volgende**.

   ![Geef het type integratieruntime op](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Zie de sectie [Een Azure-SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure-SSIS-IR. 

 
## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools of SQL Server Management Studio om uw SSIS-pakketten te implementeren in Azure. Maak verbinding met de Azure-databaseserver waarop de SSIS-catalogus (SSISDB-database) wordt gehost. De naam van de Azure-databaseserver heeft de notatie `<servername>.database.windows.net` (voor Azure SQL Database). 

Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure)   
- [Connect to the SSISDB Catalog database on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met de SSISDB-catalogusdatabase in Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie) 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd: 

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure-SSIS-integratieruntime inrichten.

Ga verder met de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
> [Gegevens kopiëren in de cloud](tutorial-copy-data-portal.md)
