---
title: Een waarschuwing voor het activeren van een Azure Automation-runbook gebruiken
description: Informatie over het activeren van een runbook worden uitgevoerd wanneer u een Azure-waarschuwing wordt gegenereerd.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e791cddb07d3204f807dbeff98b7fc69419ae23c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34194033"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Een waarschuwing voor het activeren van een Azure Automation-runbook gebruiken

U kunt [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) basisondersteuning metrische gegevens en logboeken voor de meeste services in Azure bewaken. U kunt Azure Automation-runbooks aanroepen met behulp van [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) of met behulp van klassieke waarschuwingen voor het automatiseren van taken op basis van waarschuwingen. In dit artikel leest u hoe configureren en uitvoeren van een runbook met behulp van waarschuwingen.

## <a name="alert-types"></a>Waarschuwingstypen

U kunt automation-runbooks met drie Waarschuwingstypen gebruiken:
* Klassieke metrische waarschuwingen
* Waarschuwingen voor activiteitenlogboeken
* Bijna realtime metrische waarschuwingen

Wanneer een waarschuwing een runbook aanroept, wordt door de werkelijke aanroep een HTTP POST-aanvraag naar de webhook is. De hoofdtekst van de POST-aanvraag bevat een JSON-indeling object dat nuttig eigenschappen die betrekking op de waarschuwing hebben heeft. De volgende tabel bevat koppelingen naar het schema nettolading voor elk type waarschuwing:

|Waarschuwing  |Beschrijving|De nettolading van schema  |
|---------|---------|---------|
|[Klassieke metrische waarschuwing](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Een melding verzendt wanneer een metriek platform niveau voldoet aan een bepaalde voorwaarde. Bijvoorbeeld, wanneer de waarde voor **CPU-percentage** op een virtuele machine is groter dan **90** voor de afgelopen vijf minuten.| [Schema voor klasse metrische waarschuwing nettolading](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Waarschuwing voor activiteit-logboek](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Een melding verzendt wanneer een nieuwe gebeurtenis in het Azure activiteitenlogboek voldoet aan bepaalde voorwaarden. Bijvoorbeeld, wanneer een `Delete VM` bewerking plaatsvindt in **myProductionResourceGroup** of wanneer een nieuwe Azure-servicestatus-gebeurtenis met een **Active** status wordt weergegeven.| [Activiteit logboek waarschuwing nettolading schema](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Bijna realtime metrische waarschuwing](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Stuurt een melding sneller dan metrische waarschuwingen wanneer een of meer waarden voor platform-niveau voldoen aan de opgegeven voorwaarden. Bijvoorbeeld, wanneer de waarde voor **CPU-percentage** op een virtuele machine is groter dan **90**, en de waarde voor **netwerk In** is groter dan **500 MB** voor de laatste 5 minuten.| [Bijna realtime metrische waarschuwing nettolading schema](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Omdat de gegevens die wordt geleverd door elk type waarschuwing verschilt, wordt elk type waarschuwing wordt anders afgehandeld. In de volgende sectie leert u hoe een runbook voor het afhandelen van verschillende typen waarschuwingen te maken.

## <a name="create-a-runbook-to-handle-alerts"></a>Maken van een runbook voor het afhandelen van waarschuwingen

Voor het gebruik van automatisering met waarschuwingen, moet u een runbook dat heeft logica waarmee de waarschuwing JSON-nettolading die wordt doorgegeven aan het runbook wordt beheerd. Het volgende voorbeeldrunbook moet worden aangeroepen vanuit een Azure-waarschuwing. 

Zoals beschreven in de vorige sectie, heeft elk type waarschuwing in een ander schema. Het script neemt in de webhook-gegevens in de `WebhookData` invoerparameter van runbook vanuit een waarschuwing. Het script evalueert vervolgens de JSON-nettolading om te bepalen welk type waarschuwing is gebruikt. 

In dit voorbeeld wordt een waarschuwing van een virtuele machine. Het VM-gegevens ophaalt uit de nettolading en die gegevens vervolgens worden gebruikt om te stoppen van de virtuele machine. De verbinding moet worden ingesteld in het Automation-account waarop het runbook wordt uitgevoerd.

Maakt gebruik van het runbook de **AzureRunAsConnection** [Run As-account](automation-create-runas-account.md) te verifiëren bij Azure management-actie tegen de virtuele machine.

In dit voorbeeld gebruiken om te maken van een runbook aangeroepen **Stop AzureVmInResponsetoVMAlert**. U kunt de PowerShell-script wijzigen en deze gebruiken met veel verschillende bronnen.

1. Ga naar uw Azure Automation-account.
1. Onder **PROCESAUTOMATISERING**, selecteer **Runbooks**.
1. Selecteer aan de bovenkant van de lijst met runbooks **een runbook toevoegen**. 
1. Selecteer op de pagina **Runbook toevoegen** de optie **Snel maken**.
1. Voer voor de runbooknaam **Stop AzureVmInResponsetoVMAlert**. Selecteer het runbooktype **PowerShell**. Ten slotte selecteert u **Create**.  
1. Kopieer de volgende PowerShell-voorbeeld in de **bewerken** deelvenster. 

    ```powershell-interactive
    <#
    .SYNOPSIS
    This runbook stops a resource management VM in response to an Azure alert trigger.

    .DESCRIPTION
    This runbook stops a resource management VM in response to an Azure alert trigger.
    The input is alert data that has the information required to identify which VM to stop.
    
    DEPENDENCIES
    - The runbook must be called from an Azure alert via a webhook.
    
    REQUIRED AUTOMATION ASSETS
    - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
    - An Automation certificate asset called "AzureRunAsCertificate".

    .PARAMETER WebhookData
    Optional. (The user doesn't need to enter anything, but the service always passes an object.)
    This is the data that's sent in the webhook that's triggered from the alert.

    .NOTES
    AUTHOR: Azure Automation Team
    LASTEDIT: 2017-11-22
    #>

    [OutputType("PSAzureOperationResponse")]

    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData
    )

    $ErrorActionPreference = "stop"

    if ($WebhookData)
    {
        # Get the data object from WebhookData.
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema).
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # The schema isn't supported.
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if ($status -eq "Activated")
        {
            $ResourceType = $AlertContext.resourceType
            $ResourceGroupName = $AlertContext.resourceGroupName
            $SubId = $AlertContext.subscriptionId
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Use this only if this is a resource management VM.
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is the VM.
                Write-Verbose "This is a resource management VM." -Verbose

                # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the VM.
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType isn't supported.
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated', so no action taken.
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```
1. Selecteer **publiceren** opslaan en publiceren van het runbook.

## <a name="create-an-action-group"></a>Een actiegroep maken

Een actiegroep is een verzameling van acties die worden geactiveerd door een waarschuwing. Runbooks zijn slechts een van de vele acties die u met groepen in te grijpen gebruiken kunt.

1. Selecteer in de Azure-portal **Monitor** > **instellingen** > **actiegroepen**.
1. Selecteer **actie-groep toevoegen**, en voer de vereiste gegevens op:  
    1. In de **actie groepsnaam** Voer een naam.
    1. In de **afkorting** Voer een naam. De korte naam wordt gebruikt in plaats van een volledige groep actienaam wanneer meldingen worden verzonden met behulp van deze groep in te grijpen.
    1. De **abonnement** vak wordt automatisch ingevuld met uw huidige abonnement. Dit is het abonnement waarin de actiegroep wordt opgeslagen.
    1. Selecteer de resourcegroep waarin u de actiegroep wordt opgeslagen.

In dit voorbeeld maakt u twee acties: een actie runbook en een melding.

### <a name="runbook-action"></a>Runbook-actie

Maakt een actie van het runbook in de actiegroep:

1. Onder **acties**, voor **ACTIENAAM**, voer een naam voor de actie. Voor **ACTIETYPE**, selecteer **Automation-Runbook**.
1. Onder **DETAILS**, selecteer **Details bewerken**.  
1. Op de **Runbook configureren** pagina onder **runbookbron**, selecteer **gebruiker**.  
1. Selecteer uw **abonnement** en **Automation-account**, en selecteer vervolgens de **Stop AzureVmInResponsetoVMAlert** runbook.  
1. Wanneer u klaar bent, selecteert u **OK**.

### <a name="notification-action"></a>Meldingsactie

Een meldingsactie maken in de actiegroep:

1. Onder **acties**, voor **ACTIENAAM**, voer een naam voor de actie. Voor **ACTIETYPE**, selecteer **e**.  
1. Onder **DETAILS** selecteert, **Details bewerken**.  
1. Op de **e** pagina, voert u het e-mailadres als u wilt gebruiken voor meldingen en selecteer vervolgens **OK**. Het is nuttig een e-mailadres naast het runbook als een actie toe te voegen. U wordt gewaarschuwd wanneer het runbook wordt gestart.  

    Uw actiegroep moet eruitzien als in de volgende afbeelding:

   ![Actie groep pagina toevoegen](./media/automation-create-alert-triggered-runbook/add-action-group.png)
1. Selecteer de als actiegroep wilt maken, **OK**.

U kunt deze actiegroep in de [activiteit logboek waarschuwingen](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) en [near-realtime-waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) die u maakt.

## <a name="classic-alert"></a>Klassieke waarschuwing

Klassieke waarschuwingen zijn gebaseerd op metrische gegevens en actiegroepen niet gebruiken. U kunt echter een runbook-actie op basis van een klassieke waarschuwing instellen. 

Waarschuwing voor een klassieke maken:

1. Selecteer **Waarschuwing voor metrische gegevens toevoegen**.
1. Naam van de metrische waarschuwing **myVMCPUAlert**. Geef een korte beschrijving voor de waarschuwing.
1. Selecteer voor de metrische meldingsvoorwaarde **groter is dan**. Voor de **drempelwaarde** waarde, selecteer **10**. Voor de **periode** waarde, selecteer **in de afgelopen vijf minuten**.
1. Onder **maatregelen**, selecteer **een runbook uitvoeren vanuit deze waarschuwing**.
1. Op de **Runbook configureren** pagina voor **runbookbron**, selecteer **gebruiker**. Kies uw automation-account en selecteer vervolgens de **Stop AzureVmInResponsetoVMAlert** runbook. Selecteer **OK**.
1. Selecteer voor het opslaan van de waarschuwingsregel **OK**.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het starten van een Automation-runbook met behulp van een webhook [een runbook starten vanuit een webhook](automation-webhooks.md).
* Zie voor meer informatie over de verschillende manieren om een runbook te starten, [een runbook starten](automation-starting-a-runbook.md).
* Zie voor meer informatie over het maken van een activiteit logboek waarschuwing, [activiteit logboek waarschuwingen maken](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Zie voor meer informatie over het maken van een waarschuwing voor near realtime, [geen waarschuwingsregel maken in de Azure portal](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).
