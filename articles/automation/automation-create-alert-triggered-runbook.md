---
title: Gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook
description: Informatie over het activeren van een runbook worden uitgevoerd wanneer een Azure-waarschuwing wordt gegenereerd.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/29/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8a1ae906a72d781f638fb171a409b860ffa6d501
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517704"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook

U kunt [Azure Monitor](../azure-monitor/overview.md?toc=%2fazure%2fautomation%2ftoc.json) om te controleren op basisniveau metrische gegevens en logboeken voor de meeste services in Azure. U kunt Azure Automation-runbooks aanroepen met behulp van [actiegroepen](../azure-monitor/platform/action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) of met behulp van klassieke waarschuwingen voor het automatiseren van taken op basis van waarschuwingen. Dit artikel leest u hoe configureren en uitvoeren van een runbook met behulp van waarschuwingen.

## <a name="alert-types"></a>Waarschuwingstypen

U kunt automation-runbooks met drie typen waarschuwingen:

* Algemene waarschuwingen
* Waarschuwingen voor activiteitenlogboeken
* Bijna realtime metrische waarschuwingen

> [!NOTE]
> Het algemene schema van de waarschuwing standaardiseert vandaag nog de ervaring voor waarschuwingen in Azure. De drie typen waarschuwingen in Azure vandaag (metrische gegevens, logboeken en het activiteitenlogboek) hebben in het verleden had hun eigen e-mailsjablonen, webhook-schema's, enzovoort. Zie voor meer informatie, [algemene waarschuwing schema](../azure-monitor/platform/alerts-common-schema.md)

Wanneer een waarschuwing een runbook aanroept, wordt de werkelijke aanroep een HTTP POST-aanvraag naar de webhook. De hoofdtekst van de POST-aanvraag bevat een JSON-indeling-object dat is nuttige eigenschappen die betrekking op de waarschuwing hebben. De volgende tabel bevat koppelingen naar het schema van de nettolading voor elk type waarschuwing:

|Waarschuwing  |Description|De nettolading van schema  |
|---------|---------|---------|
|[Algemene waarschuwing](../azure-monitor/platform/alerts-common-schema.md?toc=%2fazure%2fautomation%2ftoc.json)|Het algemene waarschuwing schema die de ervaring voor waarschuwingen in Azure vandaag standaardiseert.|[Algemene alert payload schema](../azure-monitor/platform/alerts-common-schema-definitions.md?toc=%2fazure%2fautomation%2ftoc.json#sample-alert-payload)|
|[Waarschuwing voor activiteitenlogboek](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Een melding wordt verzonden wanneer een nieuwe gebeurtenis in het Azure-activiteitenlogboek voldoet aan bepaalde voorwaarden. Bijvoorbeeld, wanneer een `Delete VM` bewerking wordt uitgevoerd **myProductionResourceGroup** of wanneer een nieuwe Azure Service Health-gebeurtenis met een **Active** status wordt weergegeven.| [Activiteit log alert payload schema](../azure-monitor/platform/activity-log-alerts-webhook.md)        |
|[Bijna realtime metrische waarschuwing](../azure-monitor/platform/alerts-metric-near-real-time.md?toc=%2fazure%2fautomation%2ftoc.json)    |Verzendt een melding sneller dan metrische waarschuwingen wanneer een of meer platform-niveau metrische gegevens voldoen aan opgegeven voorwaarden. Bijvoorbeeld, wanneer de waarde voor **CPU-percentage** op een virtuele machine is groter dan **90**, en de waarde voor **netwerk In** groter is dan **500 MB** gedurende de afgelopen 5 minuten.| [Bijna realtime metrische alert payload schema](../azure-monitor/platform/alerts-webhooks.md#payload-schema)          |

Omdat de gegevens die wordt geleverd door elk type waarschuwing anders is, wordt elk type waarschuwing wordt anders afgehandeld. In de volgende sectie leert u hoe een runbook voor het afhandelen van verschillende typen waarschuwingen te maken.

## <a name="create-a-runbook-to-handle-alerts"></a>Een runbook voor het afhandelen van waarschuwingen maken

Voor het gebruik van Automation met waarschuwingen, moet u een runbook dat logica waarmee de waarschuwing JSON-nettolading die wordt doorgegeven aan het runbook wordt beheerd. Het volgende voorbeeld van een runbook moet worden aangeroepen vanuit een Azure-waarschuwing.

Zoals beschreven in de vorige sectie, heeft elk type waarschuwing een ander schema. Het script neemt in de webhookgegevens in de `WebhookData` invoerparameter van runbook vanuit een waarschuwing. Het script evalueert vervolgens de JSON-nettolading om te bepalen welk type waarschuwing is gebruikt.

In dit voorbeeld wordt een waarschuwing van een virtuele machine. Het de VM-gegevens opgehaald uit de nettolading en die gegevens vervolgens worden gebruikt om te stoppen van de virtuele machine. De verbinding moet worden ingesteld in het Automation-account waarop het runbook wordt uitgevoerd. Wanneer u waarschuwingen voor het activeren van runbooks, is het belangrijk om te controleren of de status van de waarschuwing in het runbook dat wordt geactiveerd. Het runbook wordt geactiveerd telkens wanneer de waarschuwing status wordt gewijzigd. Waarschuwingen hebben meerdere statussen, de twee meest voorkomende statussen zijn `Activated` en `Resolved`. Controleer of deze status in uw runbooklogica om ervoor te zorgen dat uw runbook niet meer dan één keer wordt uitgevoerd. Het voorbeeld in dit artikel laat zien hoe om te zoeken naar `Activated` alleen waarschuwingen.

Maakt gebruik van het runbook de **AzureRunAsConnection** [uitvoeren als-account](automation-create-runas-account.md) voor verificatie met Azure de management-actie op basis van de virtuele machine uit te voeren.

In dit voorbeeld gebruiken om te maken van een runbook met de naam **Stop-AzureVmInResponsetoVMAlert**. U kunt het PowerShell-script wijzigen en deze gebruiken met veel verschillende bronnen.

1. Ga naar uw Azure Automation-account.
2. Onder **procesautomatisering**, selecteer **Runbooks**.
3. Selecteer aan de bovenkant van de lijst met runbooks, **+ maken van een runbook**.
4. Op de **Runbook toevoegen** pagina **Stop-AzureVmInResponsetoVMAlert** voor de naam van het runbook. Selecteer voor het runbooktype **PowerShell**. Ten slotte selecteert u **Create**.  
5. Kopieer de volgende PowerShell-voorbeeld in de **bewerken** pagina.

    ```powershell-interactive
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData
    )
    $ErrorActionPreference = "stop"

    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema)
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "azureMonitorCommonAlertSchema") {
            # This is the common Metric Alert schema (released March 2019)
            $Essentials = [object] ($WebhookBody.data).essentials
            # Get the first target only as this script doesn't handle multiple
            $alertTargetIdArray = (($Essentials.alertTargetIds)[0]).Split("/")
            $SubId = ($alertTargetIdArray)[2]
            $ResourceGroupName = ($alertTargetIdArray)[4]
            $ResourceType = ($alertTargetIdArray)[6] + "/" + ($alertTargetIdArray)[7]
            $ResourceName = ($alertTargetIdArray)[-1]
            $status = $Essentials.monitorCondition
        }
        elseif ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # Schema not supported
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if (($status -eq "Activated") -or ($status -eq "Fired"))
        {
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Determine code path depending on the resourceType
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is an Resource Manager VM
                Write-Verbose "This is an Resource Manager VM." -Verbose

                # Authenticate to Azure with service principal and certificate and set subscription
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the Resource Manager VM
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType not supported
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated' or 'Fired' so no action taken
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```

6. Selecteer **publiceren** opslaan en publiceren van het runbook.

## <a name="create-the-alert"></a>De waarschuwing maken

Waarschuwingen gebruiken actiegroepen, oftewel verzamelingen van acties die worden geactiveerd door de waarschuwing. Runbooks zijn slechts een van de vele acties die u met actiegroepen gebruiken kunt.

1. Selecteer in uw Automation-Account **waarschuwingen** onder **bewaking**.
1. Selecteer **+ nieuwe waarschuwingsregel**.
1. Klik op **Selecteer** onder **Resource**. Op de **selecteert u een resource** pagina, selecteer de virtuele machine op waarschuwing afmelden en klikt u op **gedaan**.
1. Klik op **voorwaarde toevoegen** onder **voorwaarde**. Selecteer het signaal dat u gebruiken wilt, bijvoorbeeld **Percentage CPU** en klikt u op **gedaan**.
1. Op de **signaallogica configureren** pagina, Voer uw **drempelwaarde** onder **waarschuwen logische**, en klikt u op **gedaan**.
1. Selecteer onder **Actiegroepen** de optie **Nieuwe maken**.
1. Op de **actiegroep toevoegen** pagina, Geef uw actiegroep een naam en een korte naam.
1. Geef een naam op de actie. Selecteer voor het actietype **Automation-Runbook**.
1. Selecteer **Details bewerken**. Op de **Runbook configureren** pagina onder **runbookbron**, selecteer **gebruiker**.  
1. Selecteer uw **abonnement** en **Automation-account**, en selecteer vervolgens de **Stop-AzureVmInResponsetoVMAlert** runbook.  
1. Selecteer **Ja** voor **inschakelen van het algemene schema van de waarschuwing**.
1. Voor het maken van de actiegroep selecteert **OK**.

    ![Actie groepspagina toevoegen](./media/automation-create-alert-triggered-runbook/add-action-group.png)

    U kunt deze actiegroep in de [waarschuwingen voor activiteitenlogboeken](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) en [bijna in realtime waarschuwingen](../azure-monitor/platform/alerts-overview.md?toc=%2fazure%2fautomation%2ftoc.json) die u maakt.

1. Onder **Waarschuwingsdetails**, een waarschuwingsregel naam en beschrijving toevoegen en klik op **waarschuwingsregel maken**.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het starten van een Automation-runbook met behulp van een webhook [een runbook starten vanuit een webhook](automation-webhooks.md).
* Zie voor meer informatie over de verschillende manieren om een runbook te starten, [een runbook starten](automation-starting-a-runbook.md).
* Zie voor meer informatie over het maken van een waarschuwing voor activiteitenlogboek, [waarschuwingen voor activiteitenlogboek maken](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Zie voor meer informatie over het maken van een waarschuwing voor bijna realtime, [een waarschuwingsregel maken in Azure portal](../azure-monitor/platform/alerts-metric.md?toc=/azure/azure-monitor/toc.json).
