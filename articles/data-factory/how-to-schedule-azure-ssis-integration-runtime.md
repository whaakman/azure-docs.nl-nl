---
title: Het plannen van de Azure-SSIS integratieruntime | Microsoft Docs
description: In dit artikel wordt beschreven hoe u starten en stoppen van een Azure SSIS integratieruntime met behulp van Azure Automation en Data Factory plannen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/16/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 7e93e83f7594d30d223d37454e09943beba6d3ce
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976724"
---
# <a name="how-to-start-and-stop-the-azure-ssis-integration-runtime-on-a-schedule"></a>Het starten en stoppen van de Azure-SSIS integratieruntime volgens een schema
In dit artikel wordt beschreven hoe u starten en stoppen van een Azure SSIS integratieruntime (IR) met behulp van Azure Automation en Azure Data Factory plannen. Met een Azure SSIS (SQL Server Integration Services) integratieruntime is (IR) een kosten die gepaard gaan met het. Daarom wilt u doorgaans om uit te voeren van de IR alleen als u wilt uitvoeren van SSIS-pakketten in Azure en de IR stopt wanneer u deze niet nodig. U kunt de Data Factory-gebruikersinterface of Azure PowerShell om te gebruiken [handmatig starten of stoppen van een Azure SSIS-IR](manage-azure-ssis-integration-runtime.md)).

U kunt bijvoorbeeld webactiviteiten met webhooks aan een Azure Automation-PowerShell-runbook maken en koppelen van een activiteit uitvoeren van SSIS-pakket daartussen. De Web-activiteiten kunnen starten en stoppen van uw Azure-SSIS IR just-in-tijd vóór en na uw pakket wordt uitgevoerd. Zie voor meer informatie over het uitvoeren van SSIS-pakket activiteit [uitvoeren van een SSIS-pakket met behulp van de SSIS-activiteit in Azure Data Factory](how-to-invoke-ssis-package-ssis-activity.md).

## <a name="overview-of-the-steps"></a>Overzicht van de stappen

Hier volgen de hoofdstappen in dit artikel beschreven:

1. **Maken en testen van een Azure Automation-runbook.** In deze stap maakt maken u een PowerShell-runbook met het script dat wordt gestart of gestopt van een Azure SSIS-IR. Vervolgens u het runbook testen in scenario's voor zowel starten en stoppen en Bevestig dat IR wordt gestart of gestopt. 
2. **Maak twee schema's voor het runbook.** Voor de eerste planning configureert u het runbook met START als de bewerking. Voor de tweede planning, configureert u het runbook met stoppen als de bewerking. Voor zowel de planning geeft u de frequentie waarmee het runbook wordt uitgevoerd. U wilt bijvoorbeeld de eerste optie om 8 uur wordt uitgevoerd elke dag en de tweede om uit te voeren op 23: 00 uur elke dag plannen. Wanneer de eerste runbook wordt uitgevoerd, start deze de Azure SSIS-IR. Wanneer de tweede runbook wordt uitgevoerd, stopt het Azure-SSIS-IR. 
3. **Maak twee webhooks voor het runbook**, één voor de bewerking starten en de andere voor de bewerking stoppen. U kunt de URL's van deze webhooks gebruiken bij het configureren van webactiviteiten in een Data Factory-pijplijn. 
4. **Maken van een Data Factory-pijplijn**. De pijplijn die u maakt bestaat uit drie activiteiten. De eerste **Web** activiteit roept de eerste webhook voor het starten van de Azure SSIS-IR. De **opgeslagen Procedure** activiteit voert een SQL-script met de SSIS-pakket. De tweede **Web** activiteit stopt het Azure-SSIS-IR. Zie voor meer informatie over het aanroepen van een SSIS-pakket van een Data Factory-pijplijn met behulp van de activiteit opgeslagen Procedure [aanroepen van een SSIS-pakket](how-to-invoke-ssis-package-stored-procedure-activity.md). Vervolgens maakt u een planningstrigger voor het plannen van de pijplijn om uit te voeren met de frequentie die u opgeeft.

## <a name="prerequisites"></a>Vereisten
Als u een Azure SSIS integratieruntime al nog niet hebt ingericht, inrichten met de instructies in de [zelfstudie](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-test-an-azure-automation-runbook"></a>Maken en testen van een Azure Automation-runbook
In deze sectie voert u de volgende stappen uit: 

1. Maak een Azure Automation-account.
2. Maak een PowerShell-runbook in de Azure Automation-account. Het PowerShell-script dat is gekoppeld aan het runbook wordt gestart of gestopt van een Azure SSIS-IR op basis van de opdracht die u voor de parameter bewerking opgeeft. 
3. Het runbook testen in beide starten en stoppen van scenario's om te controleren of deze werkt. 

### <a name="create-an-azure-automation-account"></a>Een Azure Automation-account maken
Als u een Azure Automation-account niet hebt, maakt u een door de instructies in deze stap. Zie voor gedetailleerde stappen [maken van een Azure Automation-account](../automation/automation-quickstart-create-account.md). Als onderdeel van deze stap maakt u een **Azure uitvoeren als** account (een service-principal in uw Azure Active Directory) en toe te voegen aan de **Inzender** rol van uw Azure-abonnement. Zorg ervoor dat het is hetzelfde als het abonnement met de data factory met de Azure SSIS-IR. Azure Automation maakt gebruik van dit account om te verifiëren naar Azure Resource Manager en worden uitgevoerd op uw resources. 

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
3. Selecteer **nieuw** in het menu links, selecteer **bewaking en beheer**, en selecteer **Automation**. 

    ![Nieuw -> bewaking + Management Automation ->](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
2. In de **Automation-Account toevoegen** venster de volgende stappen uit: 

    1. Geef een **naam** voor het automation-account. 
    2. Selecteer de **abonnement** waarvoor de data factory met Azure SSIS-IR. 
    3. Voor **resourcegroep**, selecteer **nieuw** om te maken van een nieuwe resourcegroep of selecteer **gebruik bestaande** om te selecteren van een bestaande resourcegroep. 
    4. Selecteer een **locatie** voor het automation-account. 
    5. Bevestig dat **uitvoeren als-account maken** is ingesteld op **Ja**. Een service-principal is gemaakt in uw Azure Active Directory. Deze wordt toegevoegd aan de **Inzender** rol van uw Azure-abonnement
    6. Selecteer **vastmaken aan dashboard** zodat u ze op het dashboard van de portal bekijken. 
    7. Selecteer **Maken**. 

        ![Nieuw -> bewaking + Management Automation ->](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
3. U ziet de **Implementatiestatus** op het dashboard en in de meldingen. 
    
    ![Automation implementeren](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
4. U ziet de startpagina van het automation-account nadat deze is gemaakt. 

    ![Automation-startpagina](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-data-factory-modules"></a>Data Factory-modules importeren

1. Selecteer **Modules** in de **gedeelde bronnen** sectie in het menu links en controleer of u hebt **AzureRM.Profile** en **AzureRM.DataFactoryV2** in de lijst met modules.

    ![Controleer of de vereiste modules](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image1.png)

2.  Ga naar de PowerShell Gallery voor de [AzureRM.DataFactoryV2 module](https://www.powershellgallery.com/packages/AzureRM.DataFactoryV2/), selecteer **implementeren in Azure Automation**, selecteert u uw Automation-account en selecteer vervolgens **OK**. Ga terug om weer te geven **Modules** in de **gedeelde bronnen** sectie in het menu links en wacht tot u de **STATUS** van de **AzureRM.DataFactoryV2** wijziging van module **beschikbaar**.

    ![Controleer of de Data Factory-module](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image2.png)

3.  Ga naar de PowerShell Gallery voor de [module AzureRM.Profile](https://www.powershellgallery.com/packages/AzureRM.profile/), klikt u op **implementeren in Azure Automation**, selecteert u uw Automation-account en selecteer vervolgens **OK**. Ga terug om weer te geven **Modules** in de **gedeelde bronnen** sectie in het menu links en wacht tot u de **STATUS** van de **AzureRM.Profile**wijziging van module **beschikbaar**.

    ![Controleer of de module profiel](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image3.png)

### <a name="create-a-powershell-runbook"></a>Een PowerShell-runbook maken
De volgende procedure bevat stappen voor het maken van een PowerShell-runbook. Het script dat is gekoppeld aan het runbook een starten/stoppen een Azure SSIS-IR op basis van de opdracht die u opgeeft voor de **bewerking** parameter. In deze sectie biedt niet alle gegevens voor het maken van een runbook. Zie voor meer informatie, [maken van een runbook](../automation/automation-quickstart-create-runbook.md) artikel.

1. Schakel over naar de **Runbooks** tabblad, en selecteer **+ toevoegen van een runbook** via de werkbalk. 

    ![Een runbook-knop toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
2. Selecteer **een nieuw runbook maken**, en voer de volgende stappen uit: 

    1. Voor **naam**, type **StartStopAzureSsisRuntime**.
    2. Voor **runbooktype**, selecteer **PowerShell**.
    3. Selecteer **Maken**.
    
        ![Een runbook-knop toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
3. Kopiëren en plakken in het volgende script aan het runbook-script-venster. Opslaan en vervolgens het runbook publiceren met behulp van de **opslaan** en **publiceren** knoppen op de werkbalk. 

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
        Connect-AzureRmAccount `
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

    1. Voor **GROEPSNAAM voor ACCOUNTRESOURCES**, voer de naam van de resourcegroep met de data factory met de Azure SSIS-IR. 
    2. Voor **naam DATA FACTORY**, voer de naam van de data factory met de Azure SSIS-IR. 
    3. Voor **AZURESSISNAME**, voer de naam van de Azure SSIS-IR. 
    4. Voor **bewerking**, voer **START**. 
    5. Selecteer **OK**.  

        ![Runbook startvenster](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
7. Selecteer in het venster taak **uitvoer** tegel. In het uitvoervenster van de taak wacht tot u het bericht **### voltooid ###** nadat u hebt gezien **### vanaf ###**. Starten van een Azure SSIS-IR duurt ongeveer 20 minuten. Sluit de **taak** venster en teruggaan naar de **Runbook** venster.

    ![Azure SSIS-IR - aan de slag](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
8.  Herhaal de vorige twee stappen, maar met behulp van **stoppen** als de waarde voor de **bewerking**. Het runbook opnieuw starten door het selecteren van de **Start** op de werkbalk. Geef de naam van de resourcegroep, de naam data factory en Azure SSIS-IR-naam. Voor **bewerking**, voer **stoppen**. 

    In het uitvoervenster van de taak wacht tot u bericht **### voltooid ###** nadat u hebt gezien **### stoppen ###**. Stoppen van een Azure SSIS-IR neemt zo lang vanaf de Azure SSIS-IR. Sluit de **taak** venster en teruggaan naar de **Runbook** venster.

## <a name="create-schedules-for-the-runbook-to-startstop-the-azure-ssis-ir"></a>Schema's voor het runbook te starten/stoppen van de Azure SSIS-IR maken
In de vorige sectie hebt u een Azure Automation-runbook dat kan starten of stoppen van een Azure SSIS-IR. gemaakt In deze sectie maakt u twee schema's voor het runbook. Bij het configureren van de eerste planning, opgeeft START u voor de parameter bewerking. Wanneer de tweede planning configureert, geeft u op dezelfde manier stoppen voor de bewerking. Zie voor gedetailleerde stappen voor het maken van planningen [maakt u een planning](../automation/automation-schedules.md#creating-a-schedule).

1. In de **Runbook** venster **planningen**, en selecteer **+ toevoegen van een schema** op de werkbalk. 

    ![Azure SSIS-IR - aan de slag](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
2. In de **planning Runbook** venster de volgende stappen uitvoeren: 

    1. Selecteer **een planning koppelen aan uw runbook**. 
    2. Selecteer **maakt u een nieuwe planning**.
    3. In de **nieuwe planning** venster, type **IR Start dagelijks** voor **naam**. 
    4. In de **sectie begint**, voor de tijd, Geef een tijd enkele minuten na de huidige tijd. 
    5. Voor **terugkeerpatroon**, selecteer **periodiek**. 
    6. In de **herhalen elke** sectie, selecteer **dag**. 
    7. Selecteer **Maken**. 

        ![Planning voor Azure SSIS-IR-start](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
3. Schakel over naar de **Parameters en uitvoerinstellingen** tabblad. Geef de naam van de resourcegroep, de naam data factory en Azure SSIS-IR-naam. Voor **bewerking**, voer **START**. Selecteer **OK**. Selecteer **OK** opnieuw om te zien van de planning voor de **planningen** pagina van het runbook. 

    ![Planning voor het starten van de Azure SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
4. Herhaal de vorige twee stappen voor het maken van een schema met de naam **IR stopt dagelijks**. Deze keer opgeven tijd ten minste 30 minuten na de tijd die u hebt opgegeven voor de **IR Start dagelijks** planning. Voor **bewerking**, geef **stoppen**. 
5. In de **Runbook** venster **taken** in het menu links. Ziet u de opdrachten aangemaakt door de schema's op de opgegeven tijden en hun status. U kunt details weergeven over de taak, zoals de uitvoer is vergelijkbaar met wat u hebt gezien wanneer u het runbook getest. 

    ![Planning voor het starten van de Azure SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
6. Nadat u klaar bent met testen, schakel de planning te wijzigen en selecteer **Nee** voor **ingeschakeld**. Selecteer **planningen** in het menu links, selecteer de **Start IR dagelijks/stoppen IR dagelijks**, en selecteer **geen** voor **ingeschakeld**. 

## <a name="create-webhooks-to-start-and-stop-the-azure-ssis-ir"></a>Webhooks om te starten en stoppen van de Azure SSIS-IR maken
Volg de instructies in [maken van een webhook](../automation/automation-webhooks.md#creating-a-webhook) twee webhooks voor het runbook te maken. Voor de eerste versie, START opgeven als de bewerking en geef voor de tweede waarde stoppen als de bewerking. Sla de URL's voor zowel de webhooks ergens (zoals een tekstbestand of een OneNote-notitieblok). U kunt deze URL's gebruikt bij het configureren van webactiviteiten in de Data Factory-pijplijn. De volgende afbeelding wordt weergegeven een voorbeeld van het maken van een webhook die de Azure SSIS-IR begint:

1. In de **Runbook** venster **Webhooks** in het menu aan de linkerkant en selecteer **+ Webhook toevoegen** op de werkbalk. 

    ![Webhooks -> Webhook toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/add-web-hook-menu.png)
2. In de **Webhook toevoegen** venster **nieuwe webhook maken**, en voer de volgende acties: 

    1. Voor **naam**, voer **StartAzureSsisIR**. 
    2. Bevestig dat **ingeschakeld** is ingesteld op **Ja**. 
    3. Kopieer de **URL** en sla deze ergens. Deze stap is belangrijk. U ziet geen URL van de later opnieuw. 
    4. Selecteer **OK**. 

        ![Nieuw venster van de Webhook](./media/how-to-schedule-azure-ssis-integration-runtime/new-web-hook-window.png)
3. Schakel over naar de **Parameters en uitvoerinstellingen** tabblad. Geef de naam van de resourcegroep, de naam van de data factory en de Azure SSIS-IR-naam. Voor **bewerking**, voer **START**. Klik op **OK**. Klik vervolgens op **Maken**. 

    ![Webhook - parameters en uitvoerinstellingen](./media/how-to-schedule-azure-ssis-integration-runtime/webhook-parameters.png)
4. Herhaal de vorige drie stappen voor het maken van een andere webhook met de naam **StopAzureSsisIR**. Vergeet niet om te kopiëren van de URL. Bij het opgeven van de parameters en uitvoerinstellingen, voer **stoppen** voor **bewerking**. 

U hebt twee URL's, één voor de **StartAzureSsisIR** webhook en de andere voor de **StopAzureSsisIR** webhook. U kunt een HTTP POST-aanvraag verzenden naar deze URL's voor het starten/stoppen van uw Azure-SSIS-IR. 

## <a name="create-and-schedule-a-data-factory-pipeline-that-startsstops-the-ir"></a>Maken en plannen van een Data Factory-pijplijn die de IR starten/stoppen
Deze sectie wordt beschreven hoe u een webactiviteit voor het aanroepen van de webhooks die u in de vorige sectie hebt gemaakt.

De pijplijn die u maakt bestaat uit drie activiteiten. 

1. De eerste **Web** activiteit roept de eerste webhook voor het starten van de Azure SSIS-IR. 
2. De **SSIS-pakket uitvoeren** activiteit of het **opgeslagen Procedure** uitvoeringen van activiteit het SSIS-pakket.
3. De tweede **Web** activiteit roept de webhook als u wilt stoppen van de Azure SSIS-IR. 

Nadat u maken en testen van de pijplijn, kunt u een schematrigger maken en koppelen aan de pijplijn. De schematrigger definieert een schema voor de pijplijn. Stel dat u maakt een trigger die is gepland voor uitvoering elke dag om 23: 00 uur. De trigger voert de pijplijn op 23: 00 uur elke dag. De pijplijn wordt gestart van de Azure SSIS-IR, voert de SSIS-pakket en stopt het Azure-SSIS-IR. 

### <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
2. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Voer op de pagina **Nieuwe gegevensfactory** **MyAzureSsisDataFactory** in als de **naam**. 
      
     ![De pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamMyAzureSsisDataFactory) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
       `Data factory name �MyAzureSsisDataFactory� is not available`
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de lijst zie u alleen locaties die worden ondersteund voor het maken van gegevensfactory’s.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Klik op **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad.

### <a name="create-a-pipeline"></a>Een pijplijn maken

1. In de **aan de slag** weergeeft, schakelt **pijplijn maken**. 

   ![Pagina Aan de slag](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
2. In de **activiteiten** werkset Vouw **algemene**, Sleep de **Web** activiteit naar het ontwerpoppervlak voor pijplijnen. In de **algemene** tabblad van de **eigenschappen** venster, wijzigt u de naam van de activiteit in op **StartIR**.

   ![Eerste webactiviteit - tabblad Algemeen](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-general-tab.png)
3. Schakel over naar de **instellingen** tabblad de **eigenschappen** venster en voer de volgende acties: 

    1. Voor **URL**, plak de URL voor de webhook die wordt gestart van de Azure SSIS-IR. 
    2. Voor **methode**, selecteer **POST**. 
    3. Voor **hoofdtekst**, voer `{"message":"hello world"}`. 
   
        ![Eerste webactiviteit - tabblad instellingen](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-settnigs-tab.png)

4. Slepen en neerzetten van de activiteit uitvoeren van SSIS-pakket of de activiteit opgeslagen Procedure uit de **algemene** sectie van de **activiteiten** werkset te verplaatsen. Stel de naam van de activiteit in op **RunSSISPackage**. 

5. Als u de activiteit uitvoeren van SSIS-pakket selecteert, volg de instructies in [uitvoeren van een SSIS-pakket met behulp van de SSIS-activiteit in Azure Data Factory](how-to-invoke-ssis-package-ssis-activity.md) om uit te voeren van het maken van de activiteit.  Zorg ervoor dat u opgeeft dat een voldoende aantal nieuwe pogingen die regelmatig na afloop van de beschikbaarheid van de Azure-SSIS-IR, omdat het duurt maximaal 30 minuten om te starten. 

    ![Instellingen voor opnieuw proberen](media/how-to-schedule-azure-ssis-integration-runtime/retry-settings.png)

6. Als u de activiteit opgeslagen Procedure selecteert, volg de instructies in [aanroepen van een SSIS-pakket met behulp van opgeslagen procedure-activiteit in Azure Data Factory](how-to-invoke-ssis-package-stored-procedure-activity.md) om uit te voeren van het maken van de activiteit. Zorg ervoor dat u een Transact-SQL-script die wacht tot de beschikbaarheid van de Azure-SSIS-IR, omdat het duurt maximaal 30 minuten om te starten.
    ```sql
    DECLARE @return_value int, @exe_id bigint, @err_msg nvarchar(150)

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

    -- Raise an error for unsuccessful package execution, check package execution status = created (1)/running (2)/canceled (3)/
    -- failed (4)/pending (5)/ended unexpectedly (6)/succeeded (7)/stopping (8)/completed (9) 
    IF (SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id = @exe_id) <> 7 
    BEGIN
        SET @err_msg=N'Your package execution did not succeed for execution ID: '+ CAST(@execution_id as nvarchar(20))
        RAISERROR(@err_msg, 15, 1)
    END
    ```

7. Verbinding maken met de **Web** activiteit naar de **SSIS-pakket uitvoeren** of de **opgeslagen Procedure** activiteit. 

    ![Web- en Stored Procedure-activiteiten verbinden](./media/how-to-schedule-azure-ssis-integration-runtime/connect-web-sproc.png)

8. Slepen en neerzetten een andere **Web** activiteit aan de rechterkant van de **SSIS-pakket uitvoeren** activiteit of het **opgeslagen Procedure** activiteit. Stel de naam van de activiteit in op **StopIR**. 
9. Schakel over naar de **instellingen** tabblad de **eigenschappen** venster en voer de volgende acties: 

    1. Voor **URL**, plak de URL voor de webhook die het Azure-SSIS-IR. stopt 
    2. Voor **methode**, selecteer **POST**. 
    3. Voor **hoofdtekst**, voer `{"message":"hello world"}`.  
10. Verbinding maken met de **SSIS-pakket uitvoeren** activiteit of het **opgeslagen Procedure** activiteit naar de laatste **Web** activiteit.

    ![Volledige pijplijn](./media/how-to-schedule-azure-ssis-integration-runtime/full-pipeline.png)
11. Instellingen voor de pijplijn valideren door te klikken op **valideren** op de werkbalk. Sluit de **Pipeline Validation Report** door te klikken op **>>** knop. 

    ![Pijplijn valideren](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-the-pipeline"></a>De uitvoering van de pijplijn testen

1. Selecteer **testuitvoering** op de werkbalk voor de pijplijn. U ziet de uitvoer in de **uitvoer** venster in het onderste deelvenster. 

    ![Test uitvoeren](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
2. In de **Runbook** pagina van uw Azure Automation-account, kunt u controleren of de taken werden uitgevoerd om te starten en stoppen van de Azure SSIS-IR. 

    ![Runbook-taken](./media/how-to-schedule-azure-ssis-integration-runtime/runbook-jobs.png)
3. Start SQL Server Management Studio. In de **verbinding maken met Server** venster de volgende acties uitvoeren: 

    1. Voor **servernaam**, geef  **&lt;uw Azure SQL database&gt;. database.windows.net**.
    2. Selecteer **opties >>**.
    3. Voor **verbinding maken met database**, selecteer **SSISDB**.
    4. Selecteer **Verbinden**. 
    5. Vouw **Integration Services-catalogussen** -> **SSISDB** -> uw map -> **projecten** -> uw SSIS-project -> **pakketten** . 
    6. Uw SSIS-pakket met de rechtermuisknop en selecteer **rapporten** -> **standaardrapporten** -> **alle uitvoeringen**. 
    7. Controleer of de SSIS-pakket wordt uitgevoerd. 

        ![Controleer of de uitvoering van SSIS-pakket](./media/how-to-schedule-azure-ssis-integration-runtime/verify-ssis-package-run.png)

### <a name="schedule-the-pipeline"></a>De pijplijn plannen 
Nu dat de pijplijn zoals verwacht werkt, kunt u een trigger deze pijplijn wordt uitgevoerd op een opgegeven uitgebracht. Zie voor meer informatie over het koppelen van een planningstrigger met een pijplijn [de pijplijn activeren volgens een schema](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

1. Selecteer op de werkbalk voor de pijplijn, **Trigger**, en selecteer **nieuw/bewerken**. 

    ![Trigger -> Nieuw/bewerken](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)
2. In de **Triggers toevoegen** venster **+ nieuw**.

    ![Triggers toevoegen - nieuwe](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)
3. In de **nieuwe Trigger**, de volgende acties uitvoeren: 

    1. Voor **naam**, Geef een naam voor de trigger. In het volgende voorbeeld **uitvoeren dagelijks** is de naam van de trigger. 
    2. Voor **Type**, selecteer **planning**. 
    3. Voor **begindatum**, selecteer een datum en tijd. 
    4. Voor **terugkeerpatroon**, geef de frequentie die voor de trigger. In het volgende voorbeeld wordt de eenmaal per dag. 
    5. Voor **End**, kunt u de datum en tijd opgeven door het selecteren van de **op datum** optie. 
    6. Selecteer **geactiveerd**. De trigger wordt geactiveerd zodra u de oplossing naar Data Factory publiceren. 
    7. Selecteer **Volgende**.

        ![Trigger -> Nieuw/bewerken](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
4. In de **Parameters voor Pijplijnuitvoeringen** pagina, lees de waarschuwing en selecteer **voltooien**. 
5. De oplossing publiceren naar Data Factory door het selecteren van **Alles publiceren** in het linkerdeelvenster. 

    ![Alles publiceren](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)

### <a name="monitor-the-pipeline-and-trigger-in-the-azure-portal"></a>De pijplijn en trigger in Azure portal controleren

1. Voor het bewaken van de trigger wordt uitgevoerd en pipeline-activiteiten, gebruikt u de **Monitor** tabblad aan de linkerkant. Zie voor gedetailleerde stappen [bewaken van de pijplijn](quickstart-create-data-factory-portal.md#monitor-the-pipeline).

    ![Pijplijnuitvoeringen](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)
2. Als u wilt weergeven die zijn gekoppeld aan een pijplijnuitvoering uitvoeringen van activiteit, selecteert u de eerste koppeling (**uitvoeringen van activiteit weergeven**) in de **acties** kolom. U ziet de uitvoering van de drie activiteiten die zijn gekoppeld aan elke activiteit in de pijplijn (eerste Web-activiteiten, Stored Procedure-activiteit en de tweede webactiviteit). Als u wilt overschakelen terug als u wilt weergeven van de pijplijn wordt uitgevoerd, selecteert u **pijplijnen** koppelen aan de bovenkant.

    ![Uitvoering van activiteiten](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)
3. U kunt ook triggeruitvoeringen weergeven door het selecteren van **activeren uitvoeringen** uit de vervolgkeuzelijst naast de **Pijplijnuitvoeringen** aan de bovenkant. 

    ![Triggeruitvoeringen](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

### <a name="monitor-the-pipeline-and-trigger-with-powershell"></a>Controleren van de pijplijn en trigger met PowerShell

Scripts, zoals de volgende voorbeelden gebruiken om de pijplijn en de trigger te bewaken.

1. Haal de status van een pijplijnuitvoering te starten.

  ```powershell
  Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $myPipelineRun
  ```

2. Lees meer over de trigger.

  ```powershell
  Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name  "myTrigger"
  ```

3. Haal de status van een trigger uitvoeren.

  ```powershell
  Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "myTrigger" -TriggerRunStartedAfter "2018-07-15" -TriggerRunStartedBefore "2018-07-16"
  ```

## <a name="next-steps"></a>Volgende stappen
Zie het volgende blogbericht:
-   [Moderniseer en uitbreiden van uw ETL/ELT-werkstromen met SSIS-activiteiten in ADF pijplijnen](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)

Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met een SSIS-catalogus in Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie)
