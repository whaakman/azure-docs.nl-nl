---
title: Gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook
description: Informatie over het activeren van een runbook worden uitgevoerd wanneer een Azure-waarschuwing wordt gegenereerd.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 32fdafc01f90b687f6fb7bcd147710e0122338ad
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53278165"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook

U kunt [Azure Monitor](../azure-monitor/overview.md?toc=%2fazure%2fautomation%2ftoc.json) om te controleren op basisniveau metrische gegevens en logboeken voor de meeste services in Azure. U kunt Azure Automation-runbooks aanroepen met behulp van [actiegroepen](../azure-monitor/platform/action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) of met behulp van klassieke waarschuwingen voor het automatiseren van taken op basis van waarschuwingen. Dit artikel leest u hoe configureren en uitvoeren van een runbook met behulp van waarschuwingen.

## <a name="alert-types"></a>Waarschuwingstypen

U kunt automation-runbooks met drie typen waarschuwingen:
* Klassieke metrische waarschuwingen
* Waarschuwingen voor activiteitenlogboeken
* Bijna realtime metrische waarschuwingen

Wanneer een waarschuwing een runbook aanroept, wordt de werkelijke aanroep een HTTP POST-aanvraag naar de webhook. De hoofdtekst van de POST-aanvraag bevat een JSON-indeling-object dat is nuttige eigenschappen die betrekking op de waarschuwing hebben. De volgende tabel bevat koppelingen naar het schema van de nettolading voor elk type waarschuwing:

|Waarschuwing  |Description|De nettolading van schema  |
|---------|---------|---------|
|[Klassieke metrische waarschuwing](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Verzendt een melding wanneer elke meetwaarde platform-niveau voldoet aan een bepaalde voorwaarde. Bijvoorbeeld, wanneer de waarde voor **CPU-percentage** op een virtuele machine is groter dan **90** voor de afgelopen vijf minuten.| [Metrische alert payload-klasseschema](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Waarschuwing voor activiteitenlogboek](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Een melding wordt verzonden wanneer een nieuwe gebeurtenis in het Azure-activiteitenlogboek voldoet aan bepaalde voorwaarden. Bijvoorbeeld, wanneer een `Delete VM` bewerking wordt uitgevoerd **myProductionResourceGroup** of wanneer een nieuwe Azure Service Health-gebeurtenis met een **Active** status wordt weergegeven.| [Activiteit log alert payload schema](../azure-monitor/platform/activity-log-alerts-webhook.md)        |
|[Bijna realtime metrische waarschuwing](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Verzendt een melding sneller dan metrische waarschuwingen wanneer een of meer platform-niveau metrische gegevens voldoen aan opgegeven voorwaarden. Bijvoorbeeld, wanneer de waarde voor **CPU-percentage** op een virtuele machine is groter dan **90**, en de waarde voor **netwerk In** groter is dan **500 MB** gedurende de afgelopen 5 minuten.| [Bijna realtime metrische alert payload schema](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Omdat de gegevens die wordt geleverd door elk type waarschuwing anders is, wordt elk type waarschuwing wordt anders afgehandeld. In de volgende sectie leert u hoe een runbook voor het afhandelen van verschillende typen waarschuwingen te maken.

## <a name="create-a-runbook-to-handle-alerts"></a>Een runbook voor het afhandelen van waarschuwingen maken

Voor het gebruik van Automation met waarschuwingen, moet u een runbook dat logica waarmee de waarschuwing JSON-nettolading die wordt doorgegeven aan het runbook wordt beheerd. Het volgende voorbeeld van een runbook moet worden aangeroepen vanuit een Azure-waarschuwing.

Zoals beschreven in de vorige sectie, heeft elk type waarschuwing een ander schema. Het script neemt in de webhookgegevens in de `WebhookData` invoerparameter van runbook vanuit een waarschuwing. Het script evalueert vervolgens de JSON-nettolading om te bepalen welk type waarschuwing is gebruikt.

In dit voorbeeld wordt een waarschuwing van een virtuele machine. Het de VM-gegevens opgehaald uit de nettolading en die gegevens vervolgens worden gebruikt om te stoppen van de virtuele machine. De verbinding moet worden ingesteld in het Automation-account waarop het runbook wordt uitgevoerd. Wanneer u waarschuwingen voor het activeren van runbooks, is het belangrijk om te controleren of de status van de waarschuwing in het runbook dat wordt geactiveerd. Het runbook wordt geactiveerd telkens wanneer de waarschuwing status wordt gewijzigd. Waarschuwingen hebben meerdere statussen, de twee meest voorkomende statussen zijn `Activated` en `Resolved`. Controleer of deze status in uw runbooklogica om ervoor te zorgen dat uw runbook niet meer dan één keer wordt uitgevoerd. Het voorbeeld in dit artikel laat zien hoe om te zoeken naar `Activated` alleen waarschuwingen.

Maakt gebruik van het runbook de **AzureRunAsConnection** [uitvoeren als-account](automation-create-runas-account.md) voor verificatie met Azure de management-actie op basis van de virtuele machine uit te voeren.

In dit voorbeeld gebruiken om te maken van een runbook met de naam **Stop-AzureVmInResponsetoVMAlert**. U kunt het PowerShell-script wijzigen en deze gebruiken met veel verschillende bronnen.

1. Ga naar uw Azure Automation-account.
1. Onder **PROCESAUTOMATISERING**, selecteer **Runbooks**.
1. Selecteer aan de bovenkant van de lijst met runbooks, **een runbook toevoegen**. 
1. Selecteer op de pagina **Runbook toevoegen** de optie **Snel maken**.
1. Voer voor de runbooknaam **Stop-AzureVmInResponsetoVMAlert**. Selecteer voor het runbooktype **PowerShell**. Ten slotte selecteert u **Create**.  
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

Een actiegroep is een verzameling van acties die worden geactiveerd door een waarschuwing. Runbooks zijn slechts een van de vele acties die u met actiegroepen gebruiken kunt.

1. Selecteer in de Azure portal, **Monitor** > **instellingen** > **actiegroepen**.
1. Selecteer **actiegroep toevoegen**, en voer vervolgens de vereiste gegevens:  
    1. In de **naam van de actiegroep** vak, voer een naam in.
    1. In de **afkorting** vak, voer een naam in. De korte naam wordt gebruikt in plaats van een volledige groep actienaam wanneer meldingen worden verzonden met behulp van deze actiegroep.
    1. De **abonnement** vak wordt automatisch ingevuld met uw huidige abonnement. Dit is het abonnement waarin de actiegroep is opgeslagen.
    1. Selecteer de resourcegroep waarin de actiegroep is opgeslagen.

In dit voorbeeld maakt u twee acties: een actie runbook en een melding.

### <a name="runbook-action"></a>Runbook-actie

Een runbook-actie in de actiegroep maken:

1. Onder **acties**, voor **ACTIENAAM**, voer een naam voor de actie. Voor **ACTIETYPE**, selecteer **Automation-Runbook**.
1. Onder **DETAILS**, selecteer **Details bewerken**.  
1. Op de **Runbook configureren** pagina onder **runbookbron**, selecteer **gebruiker**.  
1. Selecteer uw **abonnement** en **Automation-account**, en selecteer vervolgens de **Stop-AzureVmInResponsetoVMAlert** runbook.  
1. Wanneer u klaar bent, selecteert u **OK**.

### <a name="notification-action"></a>Meldingsactie

Een meldingsactie in de actiegroep maken:

1. Onder **acties**, voor **ACTIENAAM**, voer een naam voor de actie. Voor **ACTIETYPE**, selecteer **e**.  
1. Onder **DETAILS** selecteert, **Details bewerken**.  
1. Op de **e** pagina, typ de e-mailadres gebruiken voor meldingen en selecteer vervolgens **OK**. Een e-mailadres naast het runbook als een actie toe te voegen is handig. U wordt gewaarschuwd wanneer het runbook wordt gestart.  

    De actiegroep moet er uitzien zoals in de volgende afbeelding:

   ![Actie groepspagina toevoegen](./media/automation-create-alert-triggered-runbook/add-action-group.png)
1. Voor het maken van de actiegroep selecteert **OK**.

U kunt deze actiegroep in de [waarschuwingen voor activiteitenlogboeken](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) en [bijna in realtime waarschuwingen](../monitoring-and-diagnostics/monitoring-overview-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) die u maakt.

## <a name="classic-alert"></a>Klassieke waarschuwing

Klassieke waarschuwingen zijn gebaseerd op de metrische gegevens en gebruik geen actiegroepen. U kunt echter een runbook-actie op basis van een klassieke waarschuwing instellen. 

Een klassieke waarschuwing maken:

1. Selecteer **Waarschuwing voor metrische gegevens toevoegen**.
1. Naam van de waarschuwing voor metrische gegevens **myVMCPUAlert**. Geef een korte beschrijving voor de waarschuwing.
1. Selecteer voor de waarschuwing metrische **groter is dan**. Voor de **drempelwaarde** waarde, selecteer **10**. Voor de **periode** waarde, selecteer **in de afgelopen vijf minuten**.
1. Onder **actie ondernemen**, selecteer **een runbook uitvoeren vanuit deze waarschuwing**.
1. Op de **Runbook configureren** pagina voor **runbookbron**, selecteer **gebruiker**. Kies uw automation-account en selecteer vervolgens de **Stop-AzureVmInResponsetoVMAlert** runbook. Selecteer **OK**.
1. Als u wilt de waarschuwingsregel opslaan, selecteert u **OK**.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het starten van een Automation-runbook met behulp van een webhook [een runbook starten vanuit een webhook](automation-webhooks.md).
* Zie voor meer informatie over de verschillende manieren om een runbook te starten, [een runbook starten](automation-starting-a-runbook.md).
* Zie voor meer informatie over het maken van een waarschuwing voor activiteitenlogboek, [waarschuwingen voor activiteitenlogboek maken](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Zie voor meer informatie over het maken van een waarschuwing voor bijna realtime, [een waarschuwingsregel maken in Azure portal](../azure-monitor/platform/alerts-metric.md?toc=/azure/azure-monitor/toc.json).
