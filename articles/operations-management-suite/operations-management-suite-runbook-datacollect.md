---
title: Verzamelen van gegevens met een runbook in Azure Automation Log Analytics | Microsoft Docs
description: Stapsgewijze zelfstudie die helpt bij het maken van een runbook in Azure Automation voor het verzamelen van gegevens in de OMS-opslagplaats voor analyse van logboekanalyse.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: 59f674c9c6404da7f5384539189f41a4ba1a939a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a>Gegevens verzamelen in logboekanalyse met een Azure Automation-runbook
U kunt een aanzienlijke hoeveelheid gegevens in logboekanalyse verzamelen uit diverse bronnen, zoals [gegevensbronnen](../log-analytics/log-analytics-data-sources.md) op agents en ook [gegevens verzameld van Azure](../log-analytics/log-analytics-azure-storage.md).  Er zijn een scenario's waarin u wilt verzamelen van gegevens die niet worden geopend via deze standaard bronnen.  In dergelijke gevallen kunt u de [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) om gegevens te schrijven met logboekanalyse vanaf elke client REST-API.  Een veelgebruikte methode voor het uitvoeren van deze gegevensverzameling maakt gebruik van een runbook in Azure Automation.   

Deze zelfstudie wordt door het proces voor het maken en plannen van een runbook in Azure Automation om gegevens te schrijven met logboekanalyse.


## <a name="prerequisites"></a>Vereisten
Dit scenario vereist de volgende bronnen die zijn geconfigureerd in uw Azure-abonnement.  Beide mag een gratis account.

- [Meld u werkruimte](../log-analytics/log-analytics-get-started.md).
- [Azure automation-account](../automation/automation-offering-get-started.md).

## <a name="overview-of-scenario"></a>Overzicht van scenario
Voor deze zelfstudie schrijft u een runbook dat informatie over het automatiseren van taken verzamelt.  Azure Automation-Runbooks worden geïmplementeerd met PowerShell, zodat u beginnen door te schrijven en testen van een script in de Azure Automation-editor.  Zodra u hebt gecontroleerd dat u de vereiste gegevens wilt verzamelen, hebt u die gegevens schrijven naar Log Analytics en controleer of het aangepaste gegevenstype.  Ten slotte, maakt u een planning voor het starten van het runbook met regelmatige tussenpozen.

> [!NOTE]
> U kunt Azure Automation voor het verzenden van informatie over de taak met logboekanalyse zonder dit runbook configureren.  Dit scenario wordt voornamelijk gebruikt ter ondersteuning van de zelfstudie en het wordt aanbevolen dat u de gegevens naar een test-werkruimte verzenden.  


## <a name="1-install-data-collector-api-module"></a>1. Data Collector API-module installeren
Elke [aanvraag van de API van HTTP-Data Collector](../log-analytics/log-analytics-data-collector-api.md#create-a-request) moet op de juiste manier zijn geformatteerd en autorisatie-header bevatten.  U kunt dit doen in uw runbook, maar u kunt de hoeveelheid code vereist met behulp van een module die vereenvoudigt u dit proces verkleinen.  Een module die u kunt gebruiken is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in de PowerShell-galerie.

Gebruik een [module](../automation/automation-integration-modules.md) in een runbook moet worden geïnstalleerd in uw Automation-account.  Elk runbook in hetzelfde account kunt vervolgens de functies gebruiken in de module.  U kunt een nieuwe module installeren door te selecteren **activa** > **Modules** > **toevoegen van een module** in uw Automation-account.  

De PowerShell-galerie hebt u al een snelle mogelijkheid voor het implementeren van een module rechtstreeks op uw automation-account, zodat u deze optie voor deze zelfstudie gebruiken kunt.  

![Module OMSIngestionAPI](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. Ga naar [PowerShell Gallery](https://www.powershellgallery.com/).
2. Zoeken naar **OMSIngestionAPI**.
3. Klik op de **implementeren in Azure Automation** knop.
4. Selecteer uw automation-account en klik op **OK** de module te installeren.


## <a name="2-create-automation-variables"></a>2. Automation-variabelen maken
[Automation-variabelen](..\automation\automation-variables.md) bevatten waarden die kunnen worden gebruikt door alle runbooks in uw Automation-account.  Zij runbooks flexibelere doordat u deze waarden te wijzigen zonder de werkelijke runbook bewerken. Elke aanvraag van de HTTP-gegevens Collector API vereist de ID en sleutel van de OMS-werkruimte en variabele assets zijn ideaal voor het opslaan van deze informatie.  

![Variabelen](media/operations-management-suite-runbook-datacollect/variables.png)

1. Ga in de Azure-portal naar uw Automation-account.
2. Selecteer **variabelen** onder **gedeelde Resources**.
2. Klik op **toevoegen van een variabele** en maken van de twee variabelen in de volgende tabel.

| Eigenschap | De waarde van de werkruimte-ID | De sleutelwaarde werkruimte |
|:--|:--|:--|
| Naam | WorkspaceId | WorkspaceKey |
| Type | Tekenreeks | Tekenreeks |
| Waarde | In de werkruimte-ID van de werkruimte voor logboekanalyse te plakken. | Plakken aan met de primaire of secundaire sleutel van de werkruimte voor logboekanalyse. |
| Versleuteld | Nee | Ja |



## <a name="3-create-runbook"></a>3. Runbook maken

Azure Automation heeft een editor in de portal kunt u bewerken en test uw runbook.  U hebt de mogelijkheid de scripteditor gebruiken om te werken met [PowerShell rechtstreeks](../automation/automation-edit-textual-runbook.md) of [maken van een grafisch runbook](../automation/automation-graphical-authoring-intro.md).  Voor deze zelfstudie werkt u met een PowerShell-script. 

![Runbook bewerken](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. Navigeer naar uw Automation-account.  
2. Klik op **Runbooks** > **een runbook toevoegen** > **een nieuw runbook maken**.
3. Typ voor de runbooknaam **verzamelen-Automation-taken**.  Selecteer het runbooktype **PowerShell**.
4. Klik op **maken** voor het maken van het runbook en start de editor.
5. Kopieer en plak de volgende code in het runbook.  Raadpleeg de opmerkingen in het script voor een uitleg van de code.
    
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
        Add-AzureRmAccount `
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
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert the job data to json
            $body = $jobs | ConvertTo-Json
        
            # Write the body to verbose output so we can inspect it if verbose logging is on for the runbook.
            Write-Verbose $body
        
            # Send the data to Log Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a>4. Runbook-test
Azure Automation omvat een omgeving naar [test uw runbook](../automation/automation-testing-runbook.md) voordat u deze hebt gepubliceerd.  U kunt de gegevens die worden verzameld door het runbook controleren en nagaan of dat deze met logboekanalyse schrijft naar verwachting voordat het wordt gepubliceerd naar productie. 
 
![Runbook-test](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. Klik op **opslaan** om op te slaan van het runbook.
1. Klik op **testvenster** openen van het runbook in de testomgeving.
3. Nadat uw runbook parameters heeft, wordt u gevraagd waarden op te geven voor hen.  Voer de naam van de resourcegroep en de automation-account die uw gaat voor het verzamelen van informatie over de taak uit.
4. Klik op **Start** toe aan het begin van het runbook.
3. Het runbook wordt gestart met de status van **in de wachtrij geplaatst** voordat deze verwijst **met**.  
3. Het runbook uitgebreide uitvoer moet worden weergegeven met de taken die worden verzameld in json-indeling.  Als er geen taken worden weergegeven, klikt u vervolgens er mogelijk is er geen taken in het automation-account in het afgelopen uur gemaakt.  Probeer een runbook wordt gestart in de automation-account en voer de test vervolgens opnieuw uit.
4. Zorg ervoor dat de uitvoer met logboekanalyse fouten wordt niet in de post-opdracht weergegeven.  U hebt een bericht dat lijkt op het volgende.

    ![Post-uitvoer](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a>5. Controleer de records in Log Analytics
Als het runbook in de test is voltooid en u geverifieerd dat de uitvoer is ontvangen, kunt u controleren of de records zijn gemaakt met behulp van een [logboek zoeken in logboekanalyse](../log-analytics/log-analytics-log-searches.md).

![Logboekuitvoer](media/operations-management-suite-runbook-datacollect/log-output.png)

1. Selecteer in de Azure-portal uw werkruimte voor logboekanalyse.
2. Klik op **Meld zoeken**.
3. Typ de volgende opdracht `Type=AutomationJob_CL` en klik op de zoekknop. Houd er rekening mee dat het recordtype _CL die niet is opgegeven in het script bevat.  Dat achtervoegsel wordt automatisch toegevoegd aan het logboektype om aan te geven dat het een type aangepaste logboek is.
4. Hier ziet u een of meer records geretourneerd die aangeeft dat het runbook correct werkt.


## <a name="6-publish-the-runbook"></a>6. Het runbook publiceren
Nadat u hebt geverifieerd dat het runbook correct werkt, moet u publiceert deze zodat u deze in productie kan uitvoeren.  U kunt doorgaan met bewerken en het runbook testen zonder te wijzigen van de gepubliceerde versie.  

![Runbook publiceren](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. Ga terug naar uw automation-account.
2. Klik op **Runbooks** en selecteer **verzamelen-Automation-taken**.
3. Klik op **bewerken** en vervolgens **publiceren**.
4. Klik op **Ja** wanneer u wordt gevraagd om te controleren dat u wilt de volgende eerder gepubliceerde versie overschrijven.

## <a name="7-set-logging-options"></a>7. Opties voor logboekregistratie instellen 
Test, kon u om weer te geven [uitgebreide uitvoer](../automation/automation-runbook-output-and-messages.md#message-streams) omdat u de variabele $VerbosePreference in het script instellen.  Voor productie moet u de logboekregistratie-eigenschappen instellen voor het runbook als u wilt weergeven, uitgebreide uitvoer.  Voor het runbook in deze zelfstudie gebruikt, wordt de json-gegevens worden verzonden met logboekanalyse weergegeven.

![Logboekregistratie en tracering](media/operations-management-suite-runbook-datacollect/logging.png)

1. Selecteer in de eigenschappen voor uw runbook **logboekregistratie en tracering** onder **Runbookinstellingen**.
2. Wijzig de instelling voor **logboekregistratie van uitgebreide records** naar **op**.
3. Klik op **Opslaan**.

## <a name="8-schedule-runbook"></a>8. Runbook plannen
De meeste gevallen een runbook dat bewakingsgegevens verzamelt starten is om te plannen om automatisch uitgevoerd.  U dit doen door het maken van een [schema in Azure Automation](../automation/automation-schedules.md) en aan uw runbook koppelen.

![Runbook plannen](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. Selecteer in de eigenschappen voor uw runbook **planningen** onder **Resources**.
2. Klik op **toevoegen van een planning** > **een planning aan uw runbook koppelen** > **Maak een nieuwe planning**.
5. Typ de volgende waarden voor de planning en klik op **maken**.

| Eigenschap | Waarde |
|:--|:--|
| Naam | AutomationJobs-per uur |
| Wordt gestart | Selecteer die telkens wanneer u ten minste 5 minuten na de huidige tijd. |
| Terugkeerpatroon | Terugkerend |
| Herhaald elke | 1 uur |
| Vervaldatum van de set | Nee |

Zodra de planning is gemaakt, moet u de parameterwaarden die worden gebruikt telkens wanneer die deze planning wordt gestart voor het runbook instellen.

6. Klik op **parameters configureren en instellingen uitvoeren**.
7. Vul in de waarden voor uw **ResourceGroupName** en **AutomationAccountName**.
8. Klik op **OK**. 

## <a name="9-verify-runbook-starts-on-schedule"></a>9. Controleer of u runbook wordt gestart volgens planning
Telkens wanneer een runbook wordt gestart, [wordt een taak gemaakt](../automation/automation-runbook-execution.md) en eventuele uitvoer in het logboek geregistreerd.  Dit zijn in feite hetzelfde taken die het verzamelen van het runbook.  U kunt controleren of het runbook wordt gestart zoals verwacht door het controleren van de taken voor het runbook nadat de begintijd voor de planning is verstreken.

![Taken](media/operations-management-suite-runbook-datacollect/jobs.png)

1. Selecteer in de eigenschappen voor uw runbook **taken** onder **Resources**.
2. U ziet een lijst met taken voor elke keer dat het runbook is gestart.
3. Klik op een van de taken om de details ervan weer te geven.
4. Klik op **alle logboeken** de logboekbestanden weergeven en de uitvoer van het runbook.
5. Schuif naar beneden naar een vermelding zoeken die lijken op de onderstaande afbeelding.<br>![Uitgebreide](media/operations-management-suite-runbook-datacollect/verbose.png)
6. Klik op dit item om de gedetailleerde json-gegevens die is verzonden met logboekanalyse weer te geven.



## <a name="next-steps"></a>Volgende stappen
- Gebruik [ontwerper](../log-analytics/log-analytics-view-designer.md) voor het maken van een weergave om de gegevens die u hebt verzameld naar de opslagplaats voor logboekanalyse weer te geven.
- Uw runbook in het pakket een [beheeroplossing](operations-management-suite-solutions-creating.md) te distribueren naar klanten.
- Meer informatie over [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/).
- Meer informatie over [Azure Automation](https://docs.microsoft.com/azure/automation/).
- Meer informatie over de [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md).