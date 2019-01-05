---
title: Azure-SSIS Integration Runtime plannen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u het starten en stoppen van Azure-SSIS Integration Runtime plannen met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 12/27/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 5920ec5ec8e864b5bdb986544a3cdc259e7344da
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54053633"
---
# <a name="how-to-start-and-stop-azure-ssis-integration-runtime-on-a-schedule"></a>Het starten en stoppen van Azure-SSIS Integration Runtime volgens een schema
In dit artikel wordt beschreven hoe u het starten en stoppen van de Azure-SSIS Integration Runtime (IR) plannen met behulp van Azure Data Factory (ADF). Azure-SSIS IR is ADF compute resource toegewezen voor het uitvoeren van pakketten van SQL Server Integration Services (SSIS). Uitvoeren van Azure-SSIS IR is een kosten die gepaard gaan met het. Daarom wilt u doorgaans om uit te voeren van uw IR alleen als u wilt uitvoeren van SSIS-pakketten in Azure en uw IR stopt wanneer u hebt u het niet meer nodig. U kunt ADF gebruikersinterface (UI) / app of Azure PowerShell met [handmatig starten of stoppen van de IR](manage-azure-ssis-integration-runtime.md)).

U kunt ook webactiviteiten maken in ADF pijplijnen starten/stoppen uw IR volgens schema, bijvoorbeeld gestart in de ochtend voordat u uw dagelijkse ETL-workloads uitvoeren en stoppen van het in de middag nadat ze zijn voltooid.  U kunt ook een activiteit SSIS-pakket uitvoeren tussen twee webactiviteiten waarmee starten en stoppen van de IR, zodat uw IR wordt starten/stoppen van on-demand, just-in-tijd voor/na de uitvoering van uw pakket gekoppeld. Zie voor meer informatie over het uitvoeren van SSIS-pakket activiteit [een SSIS-pakket met behulp van de activiteit uitvoeren van SSIS-pakket in ADF pijplijn uitvoeren](how-to-invoke-ssis-package-ssis-activity.md) artikel.

## <a name="prerequisites"></a>Vereisten
Als u uw Azure-SSIS IR niet al ingericht hebt, inrichten met de instructies in de [zelfstudie](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-schedule-adf-pipelines-that-start-and-or-stop-azure-ssis-ir"></a>Maken en plannen van de ADF-pijplijnen die beginnen en of Azure-SSIS IR stoppen
Deze sectie leest u hoe u webactiviteiten in ADF pijplijnen op uw Azure-SSIS IR volgens schema starten/stoppen of starten & voorkomen dat deze op aanvraag. U wordt begeleid voor het maken van drie pijplijnen: 

1. De eerste pijplijn bevat een webactiviteit die uw Azure-SSIS-IR. begint 
2. De tweede pijplijn bevat een webactiviteit waarmee wordt gestopt van uw Azure-SSIS-IR.
3. De derde pijplijn bevat een SSIS-pakket uitvoeren-activiteit in een keten tussen de twee webactiviteiten waarmee uw Azure-SSIS-IR. starten/stoppen 

Nadat u maken en testen van de pijplijnen, kunt u een schematrigger maken en deze koppelen aan een pijplijn. De schematrigger definieert een schema voor het uitvoeren van de bijbehorende pijplijn. 

Bijvoorbeeld, kunt u twee triggers maken, het eerste item is gepland voor uitvoering elke dag om 06: 00 en die zijn gekoppeld aan de eerste pijplijn, terwijl de tweede waarde is gepland voor uitvoering elke dag om 18: 00 uur en die zijn gekoppeld aan de tweede pijplijn.  Op deze manier hebt u een periode tussen 6 uur aan 6 uur, dagelijks wanneer uw IR wordt uitgevoerd, gereed voor het uitvoeren van uw dagelijkse ETL-workloads.  

Als u een derde trigger die is gepland voor uitvoering elke dag om middernacht maakt en de derde pijplijn zijn gekoppeld, die pijplijn wordt uitgevoerd om middernacht elke dag vanaf uw IR vlak voor de uitvoering van het pakket vervolgens uw pakket wordt uitgevoerd en onmiddellijk uw IR stopt direct na de uitvoering van pakket, dus uw IR wordt niet uitgevoerd tevergeefs.

### <a name="create-your-adf"></a>Uw ADF maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).    
2. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
   
3. In de **nieuwe data factory** pagina **MyAzureSsisDataFactory** voor **naam**. 
      
   ![De pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   De naam van de ADF moet globaal uniek zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de ADF (bijvoorbeeld Uwnaammyazuressisdatafactory) en probeert u het opnieuw. Zie [Data Factory - naamgevingsregels](naming-rules.md) artikel voor meer informatie over naamgevingsregels voor ADF-artefacten.
  
   `Data factory name �MyAzureSsisDataFactory� is not available`
      
4. Selecteer uw Azure **abonnement** onder die u wilt maken van uw ADF. 
5. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
   - Selecteer **nieuw**, en voer de naam van uw nieuwe resourcegroep.   
         
   Zie voor meer informatie over resourcegroepen, [resourcegroepen gebruiken voor het beheren van uw Azure-resources](../azure-resource-manager/resource-group-overview.md) artikel.
   
6. Voor **versie**, selecteer **V2** .
7. Voor **locatie**, selecteer een van de locaties die wordt ondersteund voor het maken van ADF uit de vervolgkeuzelijst.
8. Selecteer **Vastmaken aan dashboard**.     
9. Klik op **Create**.
10. In Azure-dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**. 

   ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
   
11. Wanneer het aanmaken voltooid is, ziet u de pagina met ADF zoals hieronder wordt weergegeven.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
   
12. Klik op **Author & Monitor** ADF gebruikersinterface/app starten in een afzonderlijk tabblad.

### <a name="create-your-pipelines"></a>Uw pijplijnen maken

1. In **aan de slag** weergeeft, schakelt **pijplijn maken**. 

   ![Pagina Aan de slag](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
   
2. In **activiteiten** werkset Vouw **algemene** in het menu en slepen en neerzetten een **Web** activiteit naar het ontwerpoppervlak voor pijplijnen. In **algemene** tabblad van het eigenschappenvenster van activiteit, wijzig de activiteitsnaam in **startMyIR**. Schakel over naar **instellingen** tabblad en de volgende acties uitvoeren.

    1. Voor **URL**, voer de volgende URL voor de REST-API die Azure-SSIS-IR, begint vervangen `{subscriptionId}`, `{resourceGroupName}`, `{factoryName}`, en `{integrationRuntimeName}` met de werkelijke waarden voor uw IR: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}/start?api-version=2018-06-01`.
    
    U kunt ook u kunt ook kopiëren en plakken van de resource-ID van uw IR van de pagina op ADF gebruikersinterface/app op het volgende deel van de bovenstaande URL vervangen: `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}`.
    
   ![ADF-SSIS IR-Resource-ID](./media/how-to-schedule-azure-ssis-integration-runtime/adf-ssis-ir-resource-id.png)
  
    2. Voor **methode**, selecteer **POST**. 
    3. Voor **hoofdtekst**, voer `{"message":"Start my IR"}`. 
    4. Voor **verificatie**, selecteer **MSI** voor het gebruik van de beheerde identiteit voor uw ADF, Zie [Azure Data Factory-service-identiteit](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) artikel voor meer informatie.
    5. Voor **Resource**, voer `https://management.azure.com/`. 
    
   ![ADF Web activiteitenplanning SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/adf-web-activity-schedule-ssis-ir.png)
  
3. Klonen van de eerste pijplijn voor het maken van een tweede waarde, de activiteitsnaam van de te wijzigen **stopMyIR** en vervangt de volgende eigenschappen.

    1. Voor **URL**, voer de volgende URL voor de REST-API die Azure-SSIS-IR stopt vervangen `{subscriptionId}`, `{resourceGroupName}`, `{factoryName}`, en `{integrationRuntimeName}` met de werkelijke waarden voor uw IR: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}/stop?api-version=2018-06-01`.
  
    2. Voor **hoofdtekst**, voer `{"message":"Stop my IR"}`. 

4. Maken van een derde pijplijn, slepen en neerzetten een **SSIS-pakket uitvoeren** activiteit van **activiteiten** werkset naar de pijplijnontwerper ontwerpoppervlak voor pijplijnen en configureert de instructies in [ Aanroepen van een SSIS-pakket met behulp van de activiteit uitvoeren van SSIS-pakket in ADF](how-to-invoke-ssis-package-ssis-activity.md) artikel.  U kunt ook kunt u een **opgeslagen Procedure** activiteit in plaats daarvan en configureer deze volgens de instructies in [aanroepen van een SSIS-pakket met behulp van Stored Procedure-activiteit in ADF](how-to-invoke-ssis-package-stored-procedure-activity.md) artikel.  Vervolgens koppelt u de activiteit SSIS Package/opgeslagen Procedure uitvoeren tussen twee webactiviteiten waarmee uw IR, die vergelijkbaar is met deze Web-activiteiten in de eerste per seconde pijplijnen starten/stoppen.

   ![ADF-webactiviteit On-Demand SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/adf-web-activity-on-demand-ssis-ir.png)

5. De beheerde identiteit toewijzen voor uw ADF een **Inzender** rol zelf en dus webactiviteiten in de pijplijnen REST-API voor het starten/stoppen ingericht in het Azure-SSIS-IR. kunnen aanroepen.  Klik op de pagina ADF in Azure portal op **toegangsbeheer (IAM)**, klikt u op **+ roltoewijzing toevoegen**, en klik vervolgens op **roltoewijzing toevoegen** blade de volgende acties uitvoeren.

    1. Voor **rol**, selecteer **Inzender**. 
    2. Voor **toegang toewijzen aan**, selecteer **Azure AD-gebruiker, groep of service-principal**. 
    3. Voor **Selecteer**, zoek de naam van uw ADF en selecteert u deze. 
    4. Klik op **Opslaan**.
    
   ![ADF beheerde identiteit roltoewijzing](./media/how-to-schedule-azure-ssis-integration-runtime/adf-managed-identity-role-assignment.png)

6. Valideren van de ADF en alle instellingen pijplijn door te klikken op **Controleer of alle / valideren** op de werkbalk factory-pijplijn. Sluiten **Factory/Pipeline Validation Output** door te klikken op **>>** knop.  

   ![Pijplijn valideren](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-your-pipelines"></a>Test uitvoeren van uw pijplijnen

1. Selecteer **testuitvoering** op de werkbalk voor elke pijplijn en Zie **uitvoer** venster in het onderste deelvenster. 

   ![Test uitvoeren](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
    
2. Als u wilt de derde pijplijn testen, start u SQL Server Management Studio (SSMS). In **verbinding maken met Server** venster de volgende acties uitvoeren. 

    1. Voor **servernaam**, voer  **&lt;de naam van uw Azure SQL Database-server&gt;. database.windows.net**.
    2. Selecteer **opties >>**.
    3. Voor **verbinding maken met database**, selecteer **SSISDB**.
    4. Selecteer **Verbinden**. 
    5. Vouw **Integration Services-catalogussen** -> **SSISDB** -> uw map -> **projecten** -> uw SSIS-project -> **pakketten** . 
    6. Met de rechtermuisknop op het opgegeven SSIS-pakket wilt uitvoeren en selecteer **rapporten** -> **standaardrapporten** -> **alle uitvoeringen**. 
    7. Controleer of deze wordt uitgevoerd. 

   ![Controleer of de uitvoering van SSIS-pakket](./media/how-to-schedule-azure-ssis-integration-runtime/verify-ssis-package-run.png)

### <a name="schedule-your-pipelines"></a>Uw pijplijnen plannen

Nu uw pijplijnen werken als u verwacht, kunt u triggers om ze uit te voeren op de opgegeven cadences maken. Zie voor meer informatie over het koppelen van triggers met pijplijnen [de pijplijn activeren volgens een schema](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule) artikel.

1. Selecteer op de werkbalk pijplijn **Trigger** en selecteer **nieuw/bewerken**. 

   ![Trigger -> Nieuw/bewerken](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)

2. In **Triggers toevoegen** venster **+ nieuw**.

   ![Triggers toevoegen - nieuwe](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)

3. In **nieuwe Trigger** in het deelvenster de volgende acties uitvoeren: 

    1. Voor **naam**, voer een naam voor de trigger. In het volgende voorbeeld **uitvoeren dagelijks** is de naam van de trigger. 
    2. Voor **Type**, selecteer **planning**. 
    3. Voor **Start datum (UTC)**, voert u een datum en tijd in UTC. 
    4. Voor **terugkeerpatroon**, voer een uitgebracht voor de trigger. In het volgende voorbeeld is het **dagelijkse** zodra. 
    5. Voor **End**, selecteer **geen End** of voer een datum en tijd na het selecteren van **op datum**. 
    6. Selecteer **geactiveerd** activeren van de trigger onmiddellijk nadat u de hele ADF-instellingen hebt gepubliceerd. 
    7. Selecteer **Volgende**.

   ![Trigger -> Nieuw/bewerken](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
    
4. In **Parameters voor Pijplijnuitvoeringen** pagina, Controleer een waarschuwing en selecteer **voltooien**. 
5. Publiceren van de hele ADF-instellingen hiervoor **Alles publiceren** in de werkbalk van de factory. 

   ![Alles publiceren](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)

### <a name="monitor-your-pipelines-and-triggers-in-azure-portal"></a>Pijplijnen en triggers in Azure portal controleren

1. Gebruiken voor het bewaken van de trigger wordt uitgevoerd en pipeline-activiteiten, **Monitor** tabblad aan de linkerkant van de gebruikersinterface van de ADF /-app. Zie voor gedetailleerde stappen [bewaken van de pijplijn](quickstart-create-data-factory-portal.md#monitor-the-pipeline) artikel.

   ![Pijplijnuitvoeringen](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)

2. Als u wilt weergeven die zijn gekoppeld aan een pijplijnuitvoering uitvoeringen van activiteit, selecteert u de eerste koppeling (**uitvoeringen van activiteit weergeven**) in **acties** kolom. De derde pipeline, ziet u drie activiteit wordt uitgevoerd, één voor elke gekoppelde activiteit in de pijplijn (activiteit Web om te beginnen uw IR, Stored Procedure-activiteit uitvoeren van het pakket en Web-activiteit als u wilt stoppen van de IR). Om weer te geven van de pijplijn opnieuw wordt uitgevoerd, selecteert u **pijplijnen** koppelen aan de bovenkant.

   ![Uitvoering van activiteiten](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)

3. Als u de trigger wordt uitgevoerd, selecteert u **Triggeruitvoeringen** uit de vervolgkeuzelijst onder **Pijplijnuitvoeringen** aan de bovenkant. 

   ![Triggeruitvoeringen](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

### <a name="monitor-your-pipelines-and-triggers-with-powershell"></a>Pijplijnen en triggers met PowerShell controleren

Scripts, zoals de volgende voorbeelden gebruiken om uw pijplijnen en triggers te bewaken.

1. Haal de status van een pijplijnuitvoering te starten.

  ```powershell
  Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $myPipelineRun
  ```

2. Lees meer over een trigger.

  ```powershell
  Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name  "myTrigger"
  ```

3. Haal de status van een trigger uitvoeren.

  ```powershell
  Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "myTrigger" -TriggerRunStartedAfter "2018-07-15" -TriggerRunStartedBefore "2018-07-16"
  ```

## <a name="create-and-schedule-azure-automation-runbook-that-startsstops-azure-ssis-ir"></a>Maken en plannen van Azure Automation-runbook dat Azure-SSIS IR starten/stoppen

In deze sectie leert u om te maken van Azure Automation-runbook dat wordt uitgevoerd van PowerShell-script uit uw Azure-SSIS IR volgens een schema starten/stoppen.  Dit is handig als u uitvoeren van extra scripts wilt voor/na uw IR voor pre-naverwerking starten/stoppen.

### <a name="create-your-azure-automation-account"></a>Uw Azure Automation-account maken

Als u een Azure Automation-account niet al hebt, maakt u een door de instructies in deze stap. Zie voor gedetailleerde stappen [maken van een Azure Automation-account](../automation/automation-quickstart-create-account.md) artikel. Als onderdeel van deze stap maakt u een **Azure uitvoeren als** account (een service-principal in uw Azure Active Directory) en wijs deze toe een **Inzender** rol in uw Azure-abonnement. Zorg ervoor dat deze het hetzelfde abonnement met uw ADF met Azure SSIS-IR. Azure Automation wordt dit account gebruiken om te verifiëren naar Azure Resource Manager en worden uitgevoerd op uw resources. 

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. ADF-gebruikersinterface/app wordt op dit moment alleen ondersteund in Microsoft Edge en Google Chrome webbrowsers.
2. Meld u aan bij de [Azure-portal](https://portal.azure.com/).    
3. Selecteer **nieuw** in het menu links, selecteer **bewaking en beheer**, en selecteer **Automation**. 

   ![Nieuw -> bewaking + Management Automation ->](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
    
2. In **Automation-Account toevoegen** in het deelvenster de volgende acties uitvoeren.

    1. Voor **naam**, voer een naam in voor uw Azure Automation-account. 
    2. Voor **abonnement**, selecteer het abonnement waarvoor de ADF met Azure-SSIS IR. 
    3. Voor **resourcegroep**, selecteer **nieuw** om een nieuwe resourcegroep te maken of **gebruik bestaande** aan een bestaande set selecteren. 
    4. Voor **locatie**, selecteer een locatie voor uw Azure Automation-account. 
    5. Controleer of **Azure uitvoeren als-account maken** als **Ja**. Een service-principal in uw Azure Active Directory gemaakt en toegewezen een **Inzender** rol in uw Azure-abonnement.
    6. Selecteer **vastmaken aan dashboard** permanent weergeven in Azure-dashboard. 
    7. Selecteer **Maken**. 

   ![Nieuw -> bewaking + Management Automation ->](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
   
3. Hier ziet u de status van de implementatie van uw Azure Automation-account in Azure-dashboard en meldingen. 
    
   ![Automation implementeren](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
    
4. U ziet de startpagina van uw Azure Automation-account nadat deze is gemaakt. 

   ![Automation-startpagina](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-adf-modules"></a>ADF-modules importeren

1. Selecteer **Modules** in **gedeelde bronnen** sectie in het menu links en controleer of u hebt **AzureRM.DataFactoryV2**  +   **AzureRM.Profile** in de lijst met modules.

   ![Controleer of de vereiste modules](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image1.png)

2.  Als u nog geen **AzureRM.DataFactoryV2**, gaat u naar de PowerShell Gallery voor [AzureRM.DataFactoryV2 module](https://www.powershellgallery.com/packages/AzureRM.DataFactoryV2/), selecteer **implementeren in Azure Automation**, selecteer uw Azure Automation-account en selecteer vervolgens **OK**. Ga terug om weer te geven **Modules** in **gedeelde bronnen** sectie in het menu links en wacht tot u **STATUS** van **AzureRM.DataFactoryV2** module gewijzigd in **beschikbaar**.

    ![Controleer of de Data Factory-module](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image2.png)

3.  Als u nog geen **AzureRM.Profile**, gaat u naar de PowerShell Gallery voor [module AzureRM.Profile](https://www.powershellgallery.com/packages/AzureRM.profile/), selecteer **implementeren in Azure Automation**, selecteer uw Azure Automation account en selecteer vervolgens **OK**. Ga terug om weer te geven **Modules** in **gedeelde bronnen** sectie in het menu links en wacht tot u **STATUS** van de **AzureRM.Profile** module gewijzigd in **beschikbaar**.

    ![Controleer of de module profiel](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image3.png)

### <a name="create-your-powershell-runbook"></a>De PowerShell-runbook maken

De volgende sectie bevat stappen voor het maken van een PowerShell-runbook. Het script dat is gekoppeld aan uw runbook ofwel gestart/reageert op basis van de opdracht die u opgeeft voor Azure-SSIS-IR **bewerking** parameter. In deze sectie biedt niet de volledige details voor het maken van een runbook. Zie voor meer informatie, [maken van een runbook](../automation/automation-quickstart-create-runbook.md) artikel.

1. Schakel over naar **Runbooks** tabblad en selecteer **+ toevoegen van een runbook** via de werkbalk. 

   ![Een runbook-knop toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
   
2. Selecteer **een nieuw runbook maken** en voer de volgende acties: 

    1. Voor **naam**, voer **StartStopAzureSsisRuntime**.
    2. Voor **runbooktype**, selecteer **PowerShell**.
    3. Selecteer **Maken**.
    
   ![Een runbook-knop toevoegen](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
   
3. Kopieer en plak het volgende PowerShell-script aan uw runbook-script-venster. Opslaan en vervolgens uw runbook publiceren met behulp van **opslaan** en **publiceren** knoppen op de werkbalk. 

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
    
4. Test uw runbook door te selecteren **Start** op de werkbalk. 

   ![De knop runbook starten](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
    
5. In **Runbook starten** in het deelvenster de volgende ations doen: 

    1. Voor **GROEPSNAAM voor ACCOUNTRESOURCES**, voer de naam van resourcegroep waarvoor de ADF met Azure-SSIS IR. 
    2. Voor **naam DATA FACTORY**, voer de naam van de ADF met Azure-SSIS IR. 
    3. Voor **AZURESSISNAME**, voer de naam van de Azure-SSIS IR. 
    4. Voor **bewerking**, voer **START**. 
    5. Selecteer **OK**.  

   ![Runbook startvenster](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
   
6. Selecteer in het venster taak **uitvoer** tegel. Wachttijd voor een bericht in het uitvoervenster **### voltooid ###** nadat u hebt gezien **### vanaf ###**. Vanaf Azure-SSIS IR duurt ongeveer 20 minuten. Sluiten **taak** venster aan en ontvang back-ups maken op **Runbook** venster.

   ![Azure SSIS-IR - aan de slag](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
    
7. Herhaal de vorige twee stappen met behulp van **stoppen** als de waarde voor **bewerking**. Start uw runbook opnieuw door te selecteren **Start** op de werkbalk. Voer uw resourcegroep, de ADF en de Azure-SSIS IR namen. Voor **bewerking**, voer **stoppen**. Wachttijd voor een bericht in het uitvoervenster **### voltooid ###** nadat u hebt gezien **### stoppen ###**. Azure-SSIS IR stopt neemt zo lang gestart. Sluiten **taak** venster aan en ontvang back-ups maken op **Runbook** venster.

## <a name="create-schedules-for-your-runbook-to-startstop-azure-ssis-ir"></a>Schema's voor uw runbook starten/stoppen Azure-SSIS IR maken

In de vorige sectie hebt hebt u uw Azure Automation-runbook dat kan starten of stoppen van de Azure-SSIS IR gemaakt In deze sectie maakt u twee schema's voor uw runbook. Wanneer u de eerste planning configureert, geeft u **START** voor **bewerking**. Op dezelfde manier bij het configureren van de tweede waarde u **stoppen** voor **bewerking**. Zie voor gedetailleerde stappen voor het maken van planningen, [maakt u een planning](../automation/automation-schedules.md#creating-a-schedule) artikel.

1. In **Runbook** venster **planningen**, en selecteer **+ toevoegen van een schema** op de werkbalk. 

   ![Azure SSIS-IR - aan de slag](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
   
2. In **planning Runbook** in het deelvenster de volgende acties uitvoeren: 

    1. Selecteer **een planning koppelen aan uw runbook**. 
    2. Selecteer **maakt u een nieuwe planning**.
    3. In **nieuwe planning** deelvenster Voer **IR Start dagelijks** voor **naam**. 
    4. Voor **begint**, voer een tijd die een paar minuten na de huidige tijd. 
    5. Voor **terugkeerpatroon**, selecteer **periodiek**. 
    6. Voor **herhalen elke**, voer **1** en selecteer **dag**. 
    7. Selecteer **Maken**. 

   ![Planning voor Azure SSIS-IR-start](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
    
3. Schakel over naar **Parameters en uitvoerinstellingen** tabblad. Geef de resourcegroep, de ADF en de Azure-SSIS IR namen. Voor **bewerking**, voer **START** en selecteer **OK**. Selecteer **OK** opnieuw om te zien van de planning op **planningen** pagina van uw runbook. 

   ![Planning voor het starten van de Azure SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
    
4. Herhaal de vorige twee stappen voor het maken van een schema met de naam **IR stopt dagelijks**. Voer een tijd die is ten minste 30 minuten na de tijd dat u hebt opgegeven voor **IR Start dagelijks** planning. Voor **bewerking**, voer **stoppen** en selecteer **OK**. Selecteer **OK** opnieuw om te zien van de planning op **planningen** pagina van uw runbook. 

5. In **Runbook** venster **taken** in het menu links. Ziet u de opdrachten aangemaakt door de schema's op de opgegeven tijden en hun status. U kunt de taakdetails, zoals de uitvoer ziet, vergelijkbaar met wat u hebt gezien nadat u uw runbook getest. 

   ![Planning voor het starten van de Azure SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
    
6. Nadat u klaar bent met testen, planningen uitschakelen door deze te bewerken. Selecteer **planningen** selecteren in het menu links **Start IR dagelijks/stoppen IR dagelijks**, en selecteer **Nee** voor **ingeschakeld**. 

## <a name="next-steps"></a>Volgende stappen
Zie het volgende blogbericht:
-   [Moderniseer en uitbreiden van uw ETL/ELT-werkstromen met SSIS-activiteiten in ADF pijplijnen](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)

Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met een SSIS-catalogus in Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie)
