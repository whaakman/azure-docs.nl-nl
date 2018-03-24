---
title: Het plannen van Azure SSIS-integratie runtime | Microsoft Docs
description: Dit artikel wordt beschreven hoe u plant starten en stoppen van de runtime van een Azure SSIS-integratie met behulp van Azure Automation en Data Factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: article
ms.date: 01/25/2018
ms.author: douglasl
ms.openlocfilehash: cc9ab244c784cab608a75092b542dea0a6f69f22
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-schedule-starting-and-stopping-of-an-azure-ssis-integration-runtime"></a>Het starten en stoppen van de runtime van een Azure SSIS-integratie plannen 
Met een Azure-SSIS (SQL Server Integration Services)-integratie-runtime heeft (IR) een kosten die gekoppeld. Daarom wilt u de IR alleen uitvoeren als u wilt SSIS-pakketten in Azure uitvoeren en stop de toepassing wanneer u deze niet nodig. U kunt de Data Factory-gebruikersinterface of Azure PowerShell om te gebruiken [handmatig starten of stoppen van een Azure SSIS-IR](manage-azure-ssis-integration-runtime.md)). Dit artikel wordt beschreven hoe u plant starten en stoppen van een Azure-SSIS-integratie runtime (IR) met behulp van Azure Automation en Azure Data Factory. Hier volgen de stappen op hoog niveau beschreven in dit artikel:

1. **Maken en testen van een Azure Automation-runbook.** In deze stap maakt maken u een PowerShell-runbook met het script dat wordt gestart of gestopt een Azure SSIS-IR Vervolgens het runbook testen in scenario's voor zowel starten en stoppen en Bevestig dat IR wordt gestart of gestopt. 
2. **Maak twee schema's voor het runbook.** Voor de eerste planning configureert u het runbook met gestart als de bewerking. Voor de tweede planning, configureert u het runbook met stoppen als de bewerking. Voor zowel de planningen geeft u de frequentie waarmee het runbook wordt uitgevoerd. U wilt bijvoorbeeld plannen van het eerste beheerpunt voor uitvoering op 8 uur, dagelijks en de tweede op 23: 00 uur dagelijks wordt uitgevoerd. Wanneer de eerste runbook wordt uitgevoerd, wordt de Azure SSIS-IR gestart Wanneer het tweede runbook wordt uitgevoerd, stopt de SSIS-IR van Azure 
3. **Maken van twee webhooks voor het runbook**, één voor het opnieuw starten en de andere voor de bewerking uit te stoppen. U kunt de URL's van deze webhooks gebruiken bij het configureren van webactiviteiten in een Data Factory-pijplijn. 
4. **Maken van een Data Factory-pijplijn**. De pijplijn die u maakt, bestaat uit drie activiteiten. De eerste **Web** activiteit roept de eerste webhook voor het starten van de Azure SSIS-IR De **opgeslagen Procedure** activiteit wordt uitgevoerd voor een SQL-script dat wordt uitgevoerd het SSIS-pakket. De tweede **Web** activiteit stopt de SSIS-IR van Azure Zie voor meer informatie over het aanroepen van een SSIS-pakket van een Data Factory-pijplijn met behulp van de activiteit opgeslagen Procedure [aanroepen van een pakket SSIS](how-to-invoke-ssis-package-stored-procedure-activity.md). Vervolgens maakt u een schema-trigger voor het plannen van de pijplijn om uit te voeren met de frequentie die u opgeeft.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [aanroepen SSIS-pakketten met behulp van de activiteit opgeslagen procedure in versie 1](v1/how-to-invoke-ssis-package-stored-procedure-activity.md).

 
## <a name="prerequisites"></a>Vereisten
Als u dit nog niet hebt al een Azure-SSIS-integratie runtime ingericht, ingericht door de instructies te volgen in de [zelfstudie](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-test-an-azure-automation-runbook"></a>Maken en testen van een Azure Automation-runbook
In deze sectie kunt u de volgende stappen uitvoeren: 

1. Maak een Azure Automation-account.
2. Maak een PowerShell-runbook in Azure Automation-account. Het PowerShell-script dat is gekoppeld aan het runbook wordt gestart of gestopt een Azure-SSIS-IR op basis van de opdracht die u voor de parameter voor bewerking opgeeft. 
3. Het runbook testen in beide start en stop de scenario's om te controleren of deze werkt. 

### <a name="create-an-azure-automation-account"></a>Een Azure Automation-account maken
Als u een Azure Automation-account niet hebt, maakt u een door de instructies in deze stap. Zie voor gedetailleerde stappen [maken van een Azure Automation-account](../automation/automation-quickstart-create-account.md). Als onderdeel van deze stap maakt u een **Azure uitvoeren als** account (een service-principal in uw Azure Active Directory) en toe te voegen aan de **Inzender** rol van uw Azure-abonnement. Zorg ervoor dat het is hetzelfde als het abonnement dat u de gegevensfactory met de Azure SSIS-IR bevat Azure Automation gebruikt deze account in Azure Resource Manager verifiëren en uw resources niet bewerken. 

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
3. Selecteer **nieuw** Selecteer op het menu links **bewaking + Management**, en selecteer **Automation**. 

    ![Nieuw -> bewaking en beheer -> Automation](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
2. In de **Automation-Account toevoegen** venster de volgende stappen uit: 

    1. Geef een **naam** voor het automation-account. 
    2. Selecteer de **abonnement** is met de data factory met Azure SSIS IR 
    3. Voor **resourcegroep**, selecteer **nieuw** een nieuwe resourcegroep maken of selecteren **gebruik bestaande** selecteren van een bestaande resourcegroep. 
    4. Selecteer een **locatie** voor het automation-account. 
    5. Bevestig dat **uitvoeren als-account maken** is ingesteld op **Ja**. Een service-principal wordt gemaakt in uw Azure Active Directory. Deze wordt toegevoegd aan de **Inzender** rol van uw Azure-abonnement
    6. Selecteer **vastmaken aan dashboard** zodat deze wordt weergegeven op het dashboard van de portal. 
    7. Selecteer **Maken**. 

        ![Nieuw -> bewaking en beheer -> Automation](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
3. U ziet de **Implementatiestatus** op het dashboard en in de meldingen. 
    
    ![Automation implementeren](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
4. U ziet de startpagina van het automation-account nadat deze is gemaakt. 

    ![Automation-startpagina](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-data-factory-modules"></a>Data Factory-modules importeren

1. Selecteer **Modules** in de **gedeelde bronnen** sectie in het menu links en controleer of u hebt **AzureRM.Profile** en **AzureRM.DataFactoryV2** in de lijst met modules. Als dat niet het geval is, selecteert u **bladeren galerie** op de werkbalk.

    ![Automation-startpagina](./media/how-to-schedule-azure-ssis-integration-runtime/automation-modules.png)
2. In de **bladeren galerie** venster, type **AzureRM.Profile** in het zoekvenster en druk op **ENTER**. Selecteer **AzureRM.Profile** in de lijst. Klik vervolgens op **importeren** op de werkbalk. 

    ![Select AzureRM.Profile](./media/how-to-schedule-azure-ssis-integration-runtime/select-azurerm-profile.png)
1. In de **importeren** Selecteer **ik ga akkoord met het bijwerken van alle modules in Azure** optie en klik op **OK**.  

    ![Import AzureRM.Profile](./media/how-to-schedule-azure-ssis-integration-runtime/import-azurerm-profile.png)
4. Sluit windows terugkeren naar de **Modules** venster. U ziet de status van het importeren in de lijst. Selecteer **Vernieuwen** om de lijst te vernieuwen. Wacht tot u de **STATUS** als **beschikbaar**.

    ![Status importeren](./media/how-to-schedule-azure-ssis-integration-runtime/module-list-with-azurerm-profile.png)
1. Herhaal de stappen voor het importeren van de **AzureRM.DataFactoryV2** module. Bevestig dat de status van deze module is ingesteld op **beschikbaar** voordat u doorgaat. 

    ![Status van de laatste importeren](./media/how-to-schedule-azure-ssis-integration-runtime/module-list-with-azurerm-datafactoryv2.png)

### <a name="create-a-powershell-runbook"></a>Een PowerShell-runbook maken
De volgende procedure bevat stappen voor het maken van een PowerShell-runbook. Het script ofwel die is gekoppeld aan het runbook wordt gestart stopt een Azure-SSIS-IR op basis van de opdracht die u opgeeft voor de **bewerking** parameter. Deze sectie biedt niet alle gegevens voor het maken van een runbook. Zie voor meer informatie [maken van een runbook](../automation/automation-quickstart-create-runbook.md) artikel.

1. Overschakelen naar de **Runbooks** tabblad en selecteer **+ een runbook toevoegen** van de werkbalk. 

    ![Een knop runbook toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
2. Selecteer **een nieuw runbook maken**, en voer de volgende stappen uit: 

    1. Voor **naam**, type **StartStopAzureSsisRuntime**.
    2. Voor **runbooktype**, selecteer **PowerShell**.
    3. Selecteer **Maken**.
    
        ![Een knop runbook toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
3. Het volgende script voor het runbook scriptvenster kopiëren en plakken. Opslaan en vervolgens het runbook publiceren met behulp van de **opslaan** en **publiceren** knoppen op de werkbalk. 

    ```powershell
    Param
    (
          [Parameter (Mandatory= $true)]
          [String] $ResourceGroupName,
    
          [Parameter (Mandatory= $true)]
          [String] $DataFactoryName,
    
          [Parameter (Mandatory= $true)]
          [String] $AzureSSISName,
    
          [Parameter (Mandatory= $true)]
          [String] $Operation
    )
    
    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
    
        "Logging in to Azure..."
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
        if (!$servicePrincipalConnection)
        {
            $ErrorMessage = "Connection $connectionName not found."
            throw $ErrorMessage
        } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
        }
    }
    
    if($Operation -eq "START" -or $operation -eq "start")
    {
        "##### Starting #####"
        Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $AzureSSISName -Force
    }
    elseif($Operation -eq "STOP" -or $operation -eq "stop")
    {
        "##### Stopping #####"
        Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    }  
    "##### Completed #####"    
    ```

    ![PowerShell-runbook bewerken](./media/how-to-schedule-azure-ssis-integration-runtime/edit-powershell-runbook.png)
5. Het runbook testen door te selecteren **Start** op de werkbalk. 

    ![De knop runbook starten](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
6. In de **Runbook starten** venster de volgende stappen uitvoeren: 

    1. Voor **RESOURCEGROEPNAAM**, voer de naam van de resourcegroep met de data factory met de Azure SSIS-IR 
    2. Voor **DATA FACTORY NAME**, voer de naam van de gegevensfactory met de Azure SSIS-IR 
    3. Voor **AZURESSISNAME**, voer de naam van de Azure SSIS-IR 
    4. Voor **bewerking**, voer **START**. 
    5. Selecteer **OK**.  

        ![Starten van runbook-venster](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
7. Selecteer in het venster taak **uitvoer** tegel. Wacht tot u het bericht in het uitvoervenster van de taak **### voltooid ###** nadat er **### starten ###**. Starten van een Azure SSIS-IR duurt ongeveer 20 minuten. Sluit de **taak** venster en terug te gaan naar de **Runbook** venster.

    ![Azure SSIS-IR - gestart](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
8.  Herhaal de vorige twee stappen, maar met behulp van **stoppen** als de waarde voor de **bewerking**. Het runbook opnieuw starten door het selecteren van de **Start** op de werkbalk. Geef de naam van een resourcegroep, naam gegevensfactory en Azure SSIS IR-naam. Voor **bewerking**, voer **stoppen**. 

    Wacht totdat het bericht wordt weergegeven in het uitvoervenster van de taak **### voltooid ###** nadat er **### stoppen ###**. Stoppen van een Azure SSIS-IR vindt niet zo lang vanaf de Azure SSIS-IR Sluit de **taak** venster en terug te gaan naar de **Runbook** venster.

## <a name="create-schedules-for-the-runbook-to-startstop-the-azure-ssis-ir"></a>Schema's voor het runbook te starten en stoppen van de Azure SSIS-IR maken
In de vorige sectie hebt u een Azure Automation-runbook die kan starten of stoppen van een Azure SSIS-IR gemaakt In deze sectie maakt u twee schema's voor het runbook. Bij het configureren van de eerste planning, opgeven START u voor de parameter voor bewerking. Wanneer de tweede planning configureert, geeft u op dezelfde manier stoppen voor de bewerking. Zie voor gedetailleerde stappen voor het maken van planningen [maakt u een planning](../automation/automation-schedules.md#creating-a-schedule).

1. In de **Runbook** Selecteer **planningen**, en selecteer **+ toevoegen van een planning** op de werkbalk. 

    ![Azure SSIS-IR - gestart](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
2. In de **planning Runbook** venster de volgende stappen uitvoeren: 

    1. Selecteer **een planning aan uw runbook koppelen**. 
    2. Selecteer **Maak een nieuwe planning**.
    3. In de **nieuwe planning** venster, type **IR Start dagelijks** voor **naam**. 
    4. In de **sectie begint**, voor de tijd, Geef een tijd enkele minuten na de huidige tijd. 
    5. Voor **terugkeerpatroon**, selecteer **terugkerend**. 
    6. In de **herhalen elke** sectie **dag**. 
    7. Selecteer **Maken**. 

        ![Planning voor Azure SSIS IR starten](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
3. Overschakelen naar de **Parameters en uitvoerinstellingen** tabblad. Geef de naam van een resourcegroep, naam gegevensfactory en Azure SSIS IR-naam. Voor **bewerking**, voer **START**. Selecteer **OK**. Selecteer **OK** opnieuw uit om te zien van de planning op de **planningen** pagina van het runbook. 

    ![Planning voor de Azure SSIS-IR beginnen](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
4. Herhaal de vorige twee stappen voor het maken van een schema met de naam **IR stoppen dagelijks**. Deze tijd Geef tijd ten minste 30 minuten na de tijd dat u hebt opgegeven voor de **IR Start dagelijks** planning. Voor **bewerking**, geef **stoppen**. 
5. In de **Runbook** Selecteer **taken** in het menu links. U ziet de opdrachten aangemaakt door de schema's op de opgegeven tijden en hun status. Meer informatie over de taak zoals de uitvoer die vergelijkbaar met wat u hebt gezien wanneer u het runbook getest, kunt u zien. 

    ![Planning voor de Azure SSIS-IR beginnen](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
6. Nadat u klaar bent met testen, schakel de planning door ze te bewerken en te selecteren **Nee** voor **ingeschakeld**. Selecteer **planningen** selecteren in het menu links de **Start IR dagelijks/stoppen IR dagelijks**, en selecteer **Nee** voor **ingeschakeld**. 

## <a name="create-webhooks-to-start-and-stop-the-azure-ssis-ir"></a>Webhooks starten en stoppen van de Azure SSIS-IR maken
Volg de instructies in [maken van een webhook](../automation/automation-webhooks.md#creating-a-webhook) twee webhooks voor het runbook te maken. Voor de eerste START opgeven als de bewerking en geef voor de tweede stoppen als de bewerking. Sla de URL's voor zowel de webhooks ergens (zoals een tekstbestand of een laptop OneNote). U kunt deze URL's gebruiken bij het configureren van Web-activiteiten in de Data Factory-pijplijn. De volgende afbeelding wordt een voorbeeld van het maken van een webhook die de SSIS-IR Azure begint weergegeven:

1. In de **Runbook** Selecteer **Webhooks** vanuit het menu aan de linkerkant en selecteer **+ Webhook toevoegen** op de werkbalk. 

    ![Webhooks -> Webhook toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/add-web-hook-menu.png)
2. In de **toevoegen Webhook** Selecteer **maken van nieuwe webhook**, en de volgende acties uitvoeren: 

    1. Voor **naam**, voer **StartAzureSsisIR**. 
    2. Bevestig dat **ingeschakeld** is ingesteld op **Ja**. 
    3. Kopieer de **URL** en een locatie opslaan. Deze stap is het belangrijk. U ziet de URL niet later. 
    4. Selecteer **OK**. 

        ![Nieuw venster van de Webhook](./media/how-to-schedule-azure-ssis-integration-runtime/new-web-hook-window.png)
3. Overschakelen naar de **Parameters en uitvoerinstellingen** tabblad. Geef de naam van de resourcegroep, de naam van de gegevensfactory en de naam van de Azure SSIS IR. Voor **bewerking**, voer **START**. Klik op **OK**. Klik vervolgens op **Maken**. 

    ![Webhook - parameters en uitvoerinstellingen](./media/how-to-schedule-azure-ssis-integration-runtime/webhook-parameters.png)
4. Herhaal de vorige drie stappen voor het maken van een andere webhook met de naam **StopAzureSsisIR**. Vergeet niet de URL kopiëren. Voer bij het opgeven van de parameters en uitvoerinstellingen **stoppen** voor **bewerking**. 

U moet beschikken over twee URL's, één voor de **StartAzureSsisIR** webhook en de andere voor de **StopAzureSsisIR** webhook. U kunt een HTTP POST-aanvraag verzenden naar deze URL's naar uw Azure SSIS-IR starten/stoppen 

## <a name="create-and-schedule-a-data-factory-pipeline-that-startsstops-the-ir"></a>Maken en plannen van een Data Factory-pijplijn die de IR starten/stoppen
Deze sectie wordt beschreven hoe u een webactiviteit gebruiken om aan te roepen de webhooks die u in de vorige sectie hebt gemaakt.

De pijplijn die u maakt, bestaat uit drie activiteiten. 

1. De eerste **Web** activiteit roept de eerste webhook voor het starten van de Azure SSIS-IR 
2. De **opgeslagen Procedure** activiteit wordt uitgevoerd voor een SQL-script dat wordt uitgevoerd het SSIS-pakket. De tweede **Web** activiteit stopt de SSIS-IR van Azure Zie voor meer informatie over het aanroepen van een SSIS-pakket van een Data Factory-pijplijn met behulp van de activiteit opgeslagen Procedure [aanroepen van een pakket SSIS](how-to-invoke-ssis-package-stored-procedure-activity.md). 
3. De tweede **Web** activiteit roept de webhook om te stoppen van de Azure SSIS-IR 

Nadat u maken en testen van de pijplijn, kunt u de trigger van een planning maken en koppelen aan de pijplijn. De schema-trigger definieert een planning voor de pijplijn. Stel dat u een trigger die is gepland op 23: 00 uur per dag maken. De trigger wordt de pijplijn op 23 uur, dagelijks uitgevoerd. De pijplijn wordt gestart van de Azure SSIS-IR SSIS-pakket wordt uitgevoerd en stopt het Azure SSIS-IR 

### <a name="create-a-data-factory"></a>Een gegevensfactory maken

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

### <a name="create-a-pipeline"></a>Een pijplijn maken

1. In de **aan de slag** pagina **maken pijplijn**. 

   ![Pagina Aan de slag](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
2. In de **activiteiten** werkset Vouw **algemene**, slepen en neerzetten de **Web** activiteit naar het ontwerpoppervlak pijplijn. In de **algemene** tabblad van de **eigenschappen** venster, wijzig de naam van de activiteit **StartIR**.

   ![Eerste Web-activiteit - tabblad Algemeen](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-general-tab.png)
3. Overschakelen naar de **instellingen** tabblad de **eigenschappen** venster en voer de volgende acties: 

    1. Voor **URL**, plak de URL voor de webhook die de SSIS-IR Azure begint 
    2. Voor **methode**, selecteer **POST**. 
    3. Voor **hoofdtekst**, voer `{"message":"hello world"}`. 
   
        ![Eerste Web-activiteit - tabblad instellingen](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-settnigs-tab.png)
5. De activiteit opgeslagen Procedure van slepen en neerzetten de **algemene** sectie van de **activiteiten** werkset. De naam van de activiteit instellen **RunSSISPackage**. 
6. Overschakelen naar de **-Account voor SQL** tabblad de **eigenschappen** venster. 
7. Voor **gekoppelde service**, klikt u op **+ nieuw**.
8. In de **nieuwe gekoppelde Service** venster de volgende acties uitvoeren: 

    1. Selecteer **Azure SQL Database** voor **Type**.
    2. Selecteer uw Azure SQL-server die als host fungeert voor de **SSISDB** database voor de **servernaam** veld. Het inrichtingsproces Azure SSIS IR maakt een catalogus SSIS (SSISDB-database) in de Azure SQL-server die u opgeeft.
    3. Selecteer **SSISDB** voor **databasenaam**.
    4. Voor **gebruikersnaam**, voer de naam van de gebruiker die toegang tot de database heeft.
    5. Voor **wachtwoord**, voer het wachtwoord van de gebruiker. 
    6. Test de verbinding met de database door te klikken op **verbinding testen** knop.
    7. Opslaan van de gekoppelde service door te klikken op de **opslaan** knop.
9. In de **eigenschappen** venster overschakelen naar de **opgeslagen Procedure** TAB-toets van de **-Account voor SQL** tabblad en voer de volgende stappen uit: 

    1. Voor **opgeslagen procedurenaam**, selecteer **bewerken** optie en voer **sp_executesql**. 
    2. Selecteer **+ nieuw** in de **opgeslagen procedureparameters** sectie. 
    3. Voor **naam** invoeren van de parameter **instructie INSERT**. 
    4. Voor **type** invoeren van de parameter **tekenreeks**. 
    5. Voor **waarde** van de parameter, voert u de volgende SQL-query:

        Geef in de SQL-query de juiste waarden voor de **mapnaam**, **projectnaam**, en **naam_van_pakket** parameters. 

        ```sql
        DECLARE       @return_value int, @exe_id bigint, @err_msg nvarchar(150)

        -- Wait until Azure-SSIS IR is started
        WHILE NOT EXISTS (SELECT * FROM [SSISDB].[catalog].[worker_agents] WHERE IsEnabled = 1 AND LastOnlineTime > DATEADD(MINUTE, -10, SYSDATETIMEOFFSET()))
        BEGIN
            WAITFOR DELAY '00:00:01';
        END

        EXEC @return_value = [SSISDB].[catalog].[create_execution] @folder_name=N'YourFolder',
            @project_name=N'YourProject', @package_name=N'YourPackage',
            @use32bitruntime=0, @runincluster=1, @useanyworker=1,
            @execution_id=@exe_id OUTPUT 

        EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

        EXEC [SSISDB].[catalog].[start_execution] @execution_id = @exe_id, @retry_count = 0

        -- Raise an error for unsuccessful package execution, check package execution status = created (1)/running (2)/canceled (3)/failed (4)/
        -- pending (5)/ended unexpectedly (6)/succeeded (7)/stopping (8)/completed (9) 
        IF (SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id = @exe_id) <> 7 
        BEGIN
            SET @err_msg=N'Your package execution did not succeed for execution ID: '+ CAST(@execution_id as nvarchar(20))
            RAISERROR(@err_msg, 15, 1)
        END

        ```
10. Verbinding maken met de **Web** activiteit naar de **opgeslagen Procedure** activiteit. 

    ![Verbinding maken met de Web- en opgeslagen Procedure activiteiten](./media/how-to-schedule-azure-ssis-integration-runtime/connect-web-sproc.png)

11. Slepen en neerzetten een andere **Web** activiteit aan de rechterkant van de **opgeslagen Procedure** activiteit. De naam van de activiteit instellen **StopIR**. 
12. Overschakelen naar de **instellingen** tabblad de **eigenschappen** venster en voer de volgende acties: 

    1. Voor **URL**, plak de URL voor de webhook waarin de Azure SSIS-IR wordt gestopt 
    2. Voor **methode**, selecteer **POST**. 
    3. Voor **hoofdtekst**, voer `{"message":"hello world"}`.  
4. Verbinding maken met de **opgeslagen Procedure** activiteit naar de laatste **Web** activiteit.

    ![Volledige pijplijn](./media/how-to-schedule-azure-ssis-integration-runtime/full-pipeline.png)
5. De pipeline-instellingen valideren door te klikken op **valideren** op de werkbalk. Sluit de **pijplijn validatierapport** door te klikken op **>>** knop. 

    ![Pijplijn valideren](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-the-pipeline"></a>De uitvoering van de pijplijn testen

1. Selecteer **testuitvoering** op de werkbalk van de pijplijn. U ziet de uitvoer in de **uitvoer** venster in het onderste deelvenster. 

    ![Test Run](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
2. In de **Runbook** pagina van uw Azure Automation-account, kunt u controleren of de taken werden uitgevoerd om te starten en stoppen van de Azure SSIS-IR 

    ![Runbooktaken](./media/how-to-schedule-azure-ssis-integration-runtime/runbook-jobs.png)
3. Start SQL Server Management Studio. In de **verbinding maken met Server** venster de volgende acties uitvoeren: 

    1. Voor **servernaam**, geef  **&lt;uw Azure SQL database&gt;. database.windows.net**.
    2. Selecteer **opties >>**.
    3. Voor **verbinding maken met database**, selecteer **SSISDB**.
    4. Selecteer **Verbinden**. 
    5. Vouw **Integration Services-catalogussen** -> **SSISDB** -> uw map -> **projecten** -> uw SSIS-project -> **pakketten** . 
    6. SSIS-pakket met de rechtermuisknop en selecteer **rapporten** -> **standaardrapporten** -> **alle uitvoeringen**. 
    7. Controleer of het pakket SSIS uitgevoerd. 

        ![Controleer of uitvoeren van SSIS-pakket](./media/how-to-schedule-azure-ssis-integration-runtime/verfiy-ssis-package-run.png)

### <a name="schedule-the-pipeline"></a>De pijplijn plannen 
Nu dat de pijplijn als u verwacht werkt, kunt u een trigger die u kunt deze pipeline uitvoeren met een opgegeven uitgebracht. Zie voor meer informatie over het koppelen van een trigger planning met een pipeline [activeren van de pijplijn volgens een schema](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

1. Selecteer op de werkbalk van de pijplijn **Trigger**, en selecteer **nieuw/bewerken**. 

    ![Trigger -> nieuwe/bewerken](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)
2. In de **toevoegen Triggers** Selecteer **+ nieuw**.

    ![Triggers - nieuwe toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)
3. In de **nieuwe Trigger**, de volgende acties uitvoeren: 

    1. Voor **naam**, Geef een naam voor de trigger. In het volgende voorbeeld **dagelijks uitgevoerd te worden** is de naam van de trigger. 
    2. Voor **Type**, selecteer **planning**. 
    3. Voor **begindatum**, selecteer een begindatum en -tijd. 
    4. Voor **terugkeerpatroon**, geef de frequentie voor de trigger. In het volgende voorbeeld wordt de eenmaal per dag. 
    5. Voor **End**, kunt u de datum en tijd door het selecteren van de **op datum** optie. 
    6. Selecteer **geactiveerd**. De trigger wordt geactiveerd zodra u de oplossing naar de Data Factory publiceren. 
    7. Selecteer **Volgende**.

        ![Trigger -> nieuwe/bewerken](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
4. In de **Trigger uitvoeren Parameters** pagina, lees de waarschuwing en selecteer **voltooien**. 
5. De oplossing naar de Data Factory publiceren selecteren **Alles publiceren** in het linkerdeelvenster. 

    ![Alles publiceren](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)
6. Gebruik voor het controleren van de trigger wordt uitgevoerd en wordt uitgevoerd met pipeline, de **Monitor** tabblad aan de linkerkant. Zie voor gedetailleerde stappen [bewaken van de pijplijn](quickstart-create-data-factory-portal.md#monitor-the-pipeline).

    ![Pijplijnuitvoeringen](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)
7. Als u wilt weergeven van de activiteit wordt uitgevoerd die zijn gekoppeld aan een pijplijn uitvoeren, selecteert u de eerste koppeling (**weergave activiteit wordt uitgevoerd**) in de **acties** kolom. U ziet de drie activiteit wordt uitgevoerd die zijn gekoppeld aan elke activiteit in de pijplijn (eerste Web-activiteit, de activiteit opgeslagen Procedure en de tweede Web-activiteit). Als u wilt overschakelen terug om weer te geven van de pijplijn wordt uitgevoerd, selecteert u **pijplijnen** koppeling aan de bovenkant.

    ![Uitvoering van activiteiten](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)
8. U kunt de trigger wordt uitgevoerd ook weergeven door te selecteren **activeren wordt uitgevoerd** uit de vervolgkeuzelijst die naast de **pijplijn uitgevoerd** aan de bovenkant. 

    ![Triggeruitvoeringen](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met een SSIS-catalogus in Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie)