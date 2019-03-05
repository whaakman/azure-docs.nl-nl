---
title: Verzamelen van Log Analytics-gegevens met een runbook in Azure Automation | Microsoft Docs
description: Stapsgewijze zelfstudie die helpt bij het maken van een runbook in Azure Automation voor het verzamelen van gegevens in de opslagplaats voor analyse door Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: monitoring
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: 5de5191ee616f38404e2423c23f4e8b363240b0e
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308326"
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a>Gegevens verzamelen in Log Analytics met een Azure Automation-runbook

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

U kunt een aanzienlijke hoeveelheid gegevens in Log Analytics verzamelt uit diverse bronnen, zoals [gegevensbronnen](../../azure-monitor/platform/agent-data-sources.md) op agents en ook [gegevens verzameld van Azure](../../azure-monitor/platform/collect-azure-metrics-logs.md). Er zijn een's al waar moet u het verzamelen van gegevens die niet worden geopend via deze standaard bronnen. In dergelijke gevallen kunt u de [HTTP Data Collector API](../../azure-monitor/platform/data-collector-api.md) gegevens schrijven naar Log Analytics vanuit elke client REST-API. Een veelgebruikte methode voor het uitvoeren van deze gegevensverzameling wordt met behulp van een runbook in Azure Automation.

Deze zelfstudie leidt u door het proces voor het maken en plannen van een runbook in Azure Automation om gegevens te schrijven naar Log Analytics.

## <a name="prerequisites"></a>Vereisten
Dit scenario vereist de volgende resources die zijn geconfigureerd in uw Azure-abonnement. Beide is een gratis account.

- [Log Analytics-werkruimte](../../azure-monitor/learn/quick-create-workspace.md).
- [Azure automation-account](../..//automation/automation-quickstart-create-account.md).

## <a name="overview-of-scenario"></a>Overzicht van scenario
Voor deze zelfstudie schrijft u een runbook dat informatie over Automation-taken verzamelt. Azure Automation-Runbooks worden geïmplementeerd met PowerShell, zodat u beginnen met schrijven en testen van een script in de Azure Automation-editor. Als u hebt gecontroleerd dat u de vereiste gegevens verzamelt, zult u schrijven die gegevens naar Log Analytics en controleer of het aangepaste type. Ten slotte maakt u een planning aan het runbook met regelmatige tussenpozen starten.

> [!NOTE]
> U kunt Azure Automation om taakgegevens te verzenden naar Log Analytics zonder dat dit runbook configureren. In dit scenario wordt voornamelijk gebruikt ter ondersteuning van de zelfstudie en het wordt aanbevolen dat u de gegevens naar een test-werkruimte verzenden.

## <a name="1-install-data-collector-api-module"></a>1. Data Collector-API-module installeren
Elke [aanvraag van de API HTTP Data Collector](../../azure-monitor/platform/data-collector-api.md#create-a-request) op de juiste wijze moet worden geformatteerd en een autorisatie-header bevatten. U kunt dit doen in uw runbook, maar u kunt verminderen de hoeveelheid code met behulp van een module die vereenvoudigt u dit proces vereist. Een module die u kunt gebruiken is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in de PowerShell Gallery.

Gebruik een [module](../../automation/automation-integration-modules.md) in een runbook moet worden geïnstalleerd in uw Automation-account.  Een runbook in hetzelfde account kunt vervolgens de functies gebruiken in de module. U kunt een nieuwe module installeren door te selecteren **activa** > **Modules** > **toevoegen van een module** in uw Automation-account.

De PowerShell Gallery kunt u echter een snelle optie voor het implementeren van een module rechtstreeks naar uw automation-account, zodat u deze optie voor deze zelfstudie gebruiken kunt.

![OMSIngestionAPI-module](media/runbook-datacollect/OMSIngestionAPI.png)

1. Ga naar [PowerShell Gallery](https://www.powershellgallery.com/).
2. Zoeken naar **OMSIngestionAPI**.
3. Klik op de **implementeren in Azure Automation** knop.
4. Selecteer uw automation-account en klik op **OK** om de module te installeren.

## <a name="2-create-automation-variables"></a>2. Automation-variabelen maken
[Automation-variabelen](../../automation/automation-variables.md) bevatten waarden die kunnen worden gebruikt door alle runbooks in uw Automation-account. Ze maken runbooks meer flexibiliteit doordat u deze waarden wijzigen zonder de werkelijke runbook bewerken. Elke aanvraag vanuit de API HTTP Data Collector vereist de ID en sleutel van de Log Analytics-werkruimte en variabele assets zijn ideaal voor het opslaan van deze informatie.

![Variabelen](media/runbook-datacollect/variables.png)

1. Navigeer in de Azure-portal naar uw Automation-account.
2. Selecteer **variabelen** onder **gedeelde Resources**.
2. Klik op **toevoegen van een variabele** en maken van de twee variabelen die in de volgende tabel.

| Eigenschap | De waarde van de werkruimte-ID | Werkruimte-sleutelwaarde |
|:--|:--|:--|
| Name | WorkspaceId | WorkspaceKey |
| Type | String | String |
| Value | Plak in de werkruimte-ID van uw Log Analytics-werkruimte. | Plakken aan met de primaire of secundaire sleutel van uw Log Analytics-werkruimte. |
| Versleuteld | Nee | Ja |

## <a name="3-create-runbook"></a>3. Runbook maken

Azure Automation heeft een editor in de portal waar u kunt bewerken en test uw runbook. U hebt de mogelijkheid de script-editor gebruiken om te werken met [PowerShell direct](../../automation/automation-edit-textual-runbook.md) of [maken van een grafisch runbook](../../automation/automation-graphical-authoring-intro.md). Voor deze zelfstudie werkt u met een PowerShell-script.

![Runbook bewerken](media/runbook-datacollect/edit-runbook.png)

1. Navigeer naar uw Automation-account.
2. Klik op **Runbooks** > **toevoegen van een runbook** > **een nieuw runbook maken**.
3. Typ voor de runbooknaam **verzamelen-Automation-taken**. Selecteer voor het runbooktype **PowerShell**.
4. Klik op **maken** het runbook te maken en starten van de editor.
5. Kopieer en plak de volgende code in het runbook. Raadpleeg de opmerkingen in het script voor uitleg van de code.
    ```
    # Get information required for the automation account from parameter values when the runbook is started.
    Param
    (
        [Parameter(Mandatory = $True)]
        [string]$resourceGroupName,
        [Parameter(Mandatory = $True)]
        [string]$automationAccountName
    )
    
    # Authenticate to the Automation account using the Azure connection created when the Automation account was created.
    # Code copied from the runbook AzureAutomationTutorial.
    $connectionName = "AzureRunAsConnection"
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName
    Connect-AzAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    
    # Set the $VerbosePreference variable so that we get verbose output in test environment.
    $VerbosePreference = "Continue"
    
    # Get information required for Log Analytics workspace from Automation variables.
    $customerId = Get-AutomationVariable -Name 'WorkspaceID'
    $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
    
    # Set the name of the record type.
    $logType = "AutomationJob"
    
    # Get the jobs from the past hour.
    $jobs = Get-AzAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
    
    if ($jobs -ne $null) {
        # Convert the job data to json
        $body = $jobs | ConvertTo-Json
    
        # Write the body to verbose output so we can inspect it if verbose logging is on for the runbook.
        Write-Verbose $body
    
        # Send the data to Log Analytics.
        Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
    }
    ```

## <a name="4-test-runbook"></a>4. Runbook-test
Azure Automation bevat een omgeving te [test uw runbook](../../automation/automation-testing-runbook.md) voordat u deze publiceert. U kunt de gegevens die zijn verzameld door het runbook controleren en nagaan of dat er wordt geschreven naar Log Analytics zoals verwacht voordat het wordt gepubliceerd naar productie.

![Runbook-test](media/runbook-datacollect/test-runbook.png)

6. Klik op **opslaan** om op te slaan van het runbook.
1. Klik op **testvenster** openen van het runbook in de testomgeving.
3. Omdat het runbook parameters heeft, wordt u gevraagd waarden op te geven voor hen. Voer de naam van de resourcegroep en het automation-account dat uw gaan voor het verzamelen van informatie over de taak uit.
4. Klik op **Start** aan het begin van het runbook.
3. Het runbook wordt gestart met de status van **in de wachtrij geplaatst** voordat deze gaat u naar **met**.
3. Het runbook moet uitgebreide uitvoer worden weergegeven met de taken die worden verzameld in json-indeling. Als er geen taken worden weergegeven, klikt u vervolgens er mogelijk is er geen taken in het automation-account in het afgelopen uur gemaakt. Probeer een runbook starten in het automation-account en klikt u vervolgens de test opnieuw uitvoeren.
4. Zorg ervoor dat de uitvoer naar Log Analytics eventuele fouten niet wordt weergegeven in de post-opdracht. U hebt een bericht dat lijkt op het volgende.

    ![Post-uitvoer](media/runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a>5. Controleer de records in Log Analytics
Zodra het runbook in de test is voltooid en u gecontroleerd dat de uitvoer is ontvangen, kunt u controleren dat de records zijn gemaakt met behulp van een [zoeken in Logboeken in Log Analytics](../../azure-monitor/log-query/log-query-overview.md).

![De uitvoer van](media/runbook-datacollect/log-output.png)

1. Selecteer uw Log Analytics-werkruimte in de Azure-portal.
2. Klik op **zoeken in logboeken**.
3. Typ de volgende opdracht `Type=AutomationJob_CL` en klikt u op de zoekknop. Houd er rekening mee dat het recordtype _CL die niet is opgegeven in het script bevat. Dit achtervoegsel wordt automatisch toegevoegd aan het logboektype om aan te geven dat het een type aangepaste logboek is.
4. Hier ziet u een of meer records geretourneerd die aangeeft dat het runbook werkt zoals verwacht.

## <a name="6-publish-the-runbook"></a>6. Het runbook publiceren
Nadat u hebt gecontroleerd of het runbook correct werkt, moet u publiceert deze zodat u deze in productie kan uitvoeren. U kunt bewerken en het runbook testen zonder te wijzigen van de gepubliceerde versie.

![Runbook publiceren](media/runbook-datacollect/publish-runbook.png)

1. Ga terug naar uw automation-account.
2. Klik op **Runbooks** en selecteer **verzamelen-Automation-taken**.
3. Klik op **bewerken** en vervolgens **publiceren**.
4. Klik op **Ja** wanneer u wordt gevraagd om te controleren of dat u wilt overschrijven van de eerder gepubliceerde versie.

## <a name="7-set-logging-options"></a>7. Opties voor logboekregistratie instellen
Voor testen, u zou kunnen zijn om weer te geven [uitgebreide uitvoer](../../automation/automation-runbook-output-and-messages.md#message-streams) omdat u de variabele $VerbosePreference in het script instellen. Voor productie moet u de registratie-eigenschappen voor het runbook instellen als u wilt om uitgebreide uitvoer weer te geven. Voor het runbook in deze zelfstudie gebruikt, wordt dit weergegeven de json-gegevens worden verzonden naar Log Analytics.

![Logboekregistratie en tracering](media/runbook-datacollect/logging.png)

1. Selecteer in de eigenschappen voor uw runbook **logboekregistratie en tracering** onder **Runbookinstellingen**.
2. Wijzig de instelling voor **logboekregistratie van uitgebreide records** naar **op**.
3. Klik op **Opslaan**.

## <a name="8-schedule-runbook"></a>8. Runbook plannen
De meest voorkomende manier om een runbook die worden verzameld van bewakingsgegevens te starten is om te plannen dat het automatisch wordt uitgevoerd. U dit doen door het maken van een [planning in Azure Automation](../../automation/automation-schedules.md) en deze te koppelen aan uw runbook.

![Runbook plannen](media/runbook-datacollect/schedule-runbook.png)

1. Selecteer in de eigenschappen voor uw runbook **planningen** onder **Resources**.
2. Klik op **toevoegen van een schema** > **een planning koppelen aan uw runbook** > **maakt u een nieuwe planning**.
5. Typ de volgende waarden voor de planning en klik op **maken**.

| Eigenschap | Value |
|:--|:--|
| Name | AutomationJobs-Hourly |
| Start | Selecteer altijd ten minste 5 minuten na de huidige tijd. |
| Terugkeerpatroon | Terugkerend |
| Herhalen elke | 1 uur |
| Vervaldatum instellen | Nee |

Zodra de planning is gemaakt, moet u instellen de parameterwaarden die telkens wanneer die deze planning het runbook begint worden gebruikt.

6. Klik op **configureren parameters en uitvoerinstellingen**.
7. Vul de waarden voor uw **ResourceGroupName** en **AutomationAccountName**.
8. Klik op **OK**.

## <a name="9-verify-runbook-starts-on-schedule"></a>9. Controleer of u runbook wordt gestart volgens planning
Telkens wanneer een runbook wordt gestart, [wordt een taak gemaakt](../../automation/automation-runbook-execution.md) en eventuele uitvoer in het logboek geregistreerd. Dit zijn in feite dezelfde taken die het runbook worden verzameld. U kunt controleren of dat het runbook wordt gestart zoals verwacht door het controleren van de taken voor het runbook nadat de begintijd voor de planning is verstreken.

![Taken](media/runbook-datacollect/jobs.png)

1. Selecteer in de eigenschappen voor uw runbook **taken** onder **Resources**.
2. U ziet een lijst met taken voor elke keer dat het runbook is gestart.
3. Klik op een van de taken om de details ervan weer te geven.
4. Klik op **alle logboeken** voor het weergeven van de logboeken en uitvoer van het runbook.
5. Ga naar de onderkant vinden een vermelding die vergelijkbaar is met de onderstaande afbeelding.<br>![Uitgebreid](media/runbook-datacollect/verbose.png)
6. Klik op deze vermelding om de gedetailleerde json-gegevens die zijn verzonden naar Log Analytics weer te geven.

## <a name="next-steps"></a>Volgende stappen
- Gebruik [Weergaveontwerper](../../azure-monitor/platform/view-designer.md) een weergave met de gegevens die u hebt verzameld in de Log Analytics-opslagplaats te maken.
- Uw runbook in het pakket een [beheeroplossing](../../azure-monitor/insights/solutions-creating.md) te distribueren naar klanten.
- Meer informatie over [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).
- Meer informatie over [Azure Automation](https://docs.microsoft.com/azure/automation/).
- Meer informatie over de [HTTP Data Collector API](../../azure-monitor/platform/data-collector-api.md).
