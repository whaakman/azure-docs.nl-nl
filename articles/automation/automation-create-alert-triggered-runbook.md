---
title: Reageren op waarschuwingen van Azure met een automation-runbook | Microsoft Docs
description: Informatie over het activeren van een runbook worden uitgevoerd wanneer u Azure waarschuwingen worden gegenereerd.
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 01/11/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 486a3c80c9d6c19ca0a7ba7d4e3cdcde927053b9
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="respond-to-azure-alerts-with-an-automation-runbook"></a>Reageren op waarschuwingen van Azure met een automation-runbook

[Monitor voor Azure](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) biedt basisondersteuning metrische gegevens en logboeken voor de meeste services in Microsoft Azure. Azure automation-runbooks worden aangeroepen met [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) of vanuit de klassieke waarschuwingen voor het automatiseren van taken op basis van de waarschuwingen. In dit artikel leest u hoe het configureren en uitvoeren van een runbook dat is gebaseerd op deze waarschuwingen.

## <a name="alert-types"></a>Waarschuwingstypen

Runbooks zijn ondersteunde bewerkingen op alle drie soorten waarschuwingen. Wanneer een waarschuwing het runbook roept, wordt door de werkelijke aanroep een HTTP POST-aanvraag naar de webhook is. De hoofdtekst van de POST-aanvraag bevat een JSON-indeling object dat nuttige eigenschappen die betrekking hebben op de waarschuwing bevat. De volgende tabel bevat koppelingen naar het schema nettolading voor elk type waarschuwing:

|Waarschuwing  |Beschrijving|De nettolading van Schema  |
|---------|---------|---------|
|[Klassieke metrische waarschuwing](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Een melding ontvangen wanneer een metriek platform niveau voldoet aan een bepaalde voorwaarde (bijvoorbeeld, CPU-percentage op een virtuele machine is groter dan 90 voor de afgelopen vijf minuten).| [De nettolading van schema](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Waarschuwing voor activiteit-logboek](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Een melding ontvangen wanneer een nieuwe gebeurtenis in de Azure Activity Log voldoet aan bepaalde voorwaarden (bijvoorbeeld wanneer een bewerking 'VM verwijderen' in myProductionResourceGroup optreedt of wanneer een nieuwe gebeurtenis servicestatus met 'Active' als de status wordt weergegeven).| [De nettolading van schema](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Bijna realtime metrische waarschuwing](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Een bericht sneller dan metrische waarschuwingen wanneer een of meer platform niveau metrische gegevens aan de opgegeven voorwaarden voldoen (bijvoorbeeld, CPU-percentage op een virtuele machine is groter dan 90 en netwerk In groter is dan 500 MB voor de afgelopen vijf minuten).| [De nettolading van schema](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Omdat de gegevens van elke waarschuwing verschilt, moet elke waarschuwing anders worden afgehandeld. In de volgende sectie leert u hoe een runbook te verwerken van deze verschillende typen waarschuwingen te maken.

## <a name="create-a-runbook-to-handle-alerts"></a>Maken van een runbook voor het afhandelen van waarschuwingen

Voor het gebruik van automatisering met waarschuwingen, moet u een runbook dat logica voor het beheren van de waarschuwing JSON-nettolading die wordt doorgegeven aan het runbook heeft. Het volgende voorbeeldrunbook moet worden aangeroepen vanuit een Azure-waarschuwing. Zoals beschreven in de vorige sectie, heeft elk type Waarschuwingstype in een ander schema. Het script neemt in de webhook-gegevens in de `WebhookData` runbook invoerparameter vanuit een waarschuwing en evalueert het JSON-nettolading om te bepalen welk type waarschuwing is gebruikt. Het volgende voorbeeld zou worden gebruikt op een waarschuwing van een virtuele machine. Deze VM gegevens ophaalt uit de nettolading en die gegevens worden gebruikt om te stoppen van de virtuele machine. De verbinding moet worden ingesteld in de automatisering account waarop het runbook is uitgevoerd.

Maakt gebruik van het runbook de **AzureRunAsConnection** [run as-account](automation-create-runas-account.md) te verifiëren bij Azure om de actie voor beheer op basis van de virtuele machine uitvoeren.

De volgende PowerShell-script kan worden gewijzigd voor gebruik met veel verschillende bronnen.

Het volgende voorbeeld nemen en maken van een runbook aangeroepen **Stop AzureVmInResponsetoVMAlert** met het PowerShell-voorbeeld.

1. Open uw Automation-account.
1. Klik op **Runbooks** onder **PROCESAUTOMATISERING**. De lijst met runbooks wordt weergegeven.
1. Klik op de knop **Een runbook toevoegen** boven aan de lijst. Op de **Runbook toevoegen pagina**, selecteer **snelle invoer**.
1. Voer 'Stop AzureVmInResponsetoVMAlert' voor het runbook **naam**, en selecteer **PowerShell** voor **runbooktype**. Klik op **Create**.
1. Kopieer de volgende PowerShell-voorbeeld in het deelvenster bewerken. Klik op **publiceren** opslaan en publiceren van het runbook.

```powershell-interactive
<#
.SYNOPSIS
  This runbook will stop a resource management VM in response to an Azure alert trigger.

.DESCRIPTION
  This runbook will stop an resource management VM in response to an Azure alert trigger.
  Input is alert data with information needed to identify which VM to stop.
  
  DEPENDENCIES
  - The runbook must be called from an Azure alert via a webhook.
  
  REQUIRED AUTOMATION ASSETS
  - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
  - An Automation certificate asset called "AzureRunAsCertificate".

.PARAMETER WebhookData
   Optional (user does not need to enter anything, but the service will always pass an object)
   This is the data that is sent in the webhook that is triggered from the alert.

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
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Get the info needed to identify the VM (depends on the payload schema)
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
        # Schema not supported
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

        # Do work only if this is a resource management VM
        if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
        {
            # This is the VM
            Write-Verbose "This is a resource management VM." -Verbose

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

            # Stop the VM
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
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
    }
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="create-an-action-group"></a>Een actiegroep maken

Een actiegroep is een verzameling van acties die worden uitgevoerd op basis van een waarschuwing. Runbooks zijn slechts een van de vele acties die beschikbaar met actiegroepen zijn.

1. Selecteer in de portal **Monitor**.

1. Selecteer **actiegroepen** onder **instellingen**.

1. Selecteer **actie-groep toevoegen**, en vul de velden in.

1. Voer een naam in het vak actie groep naam en voer een naam in het vak korte naam. De korte naam wordt gebruikt in plaats van een volledige groep actienaam wanneer meldingen worden verzonden met behulp van deze groep.

1. Het abonnement vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep wordt opgeslagen.
Selecteer de resourcegroep waarin u de actiegroep wordt opgeslagen.

In dit voorbeeld maakt u twee acties, een actie van het runbook en een meldingsactie.

### <a name="runbook-action"></a>Runbook-actie

De volgende stappen maakt een runbook actie binnen de groep in te grijpen.

1. Onder **acties**, Geef een naam op voor de actie.

1. Selecteer **Automation-Runbook** voor de **actietype**.

1. Onder **Details** selecteert, **Details bewerken**

1. Op de **Runbook configureren** pagina **gebruiker** onder **runbookbron**.

1. Kies uw **abonnement** en **Automation-account**, en ten slotte selecteert u het runbook dat u hebt gemaakt in de vorige stap 'Stop AzureVmInResponsetoVMAlert' genoemd.

1. Wanneer u klaar bent, klikt u op **OK**.

### <a name="notification-action"></a>Meldingsactie

De volgende stappen maakt een meldingsactie binnen de groep in te grijpen.

1. Onder **acties**, Geef een naam op voor de actie.

1. Selecteer **e** voor de **actietype**.

1. Onder **Details** selecteert, **Details bewerken**

1. Op de **e** pagina, voert u het e-mailadres om te melden en klikt u op **OK**. Een e-mailadres, evenals het runbook toe te voegen als een actie is handig als u wordt gewaarschuwd wanneer het runbook wordt gestart. Uw actiegroep moet eruitzien als in de volgende afbeelding:

   ![Actie groep pagina toevoegen](./media/automation-create-alert-triggered-runbook/add-action-group.png)

1. Klik op **OK** om het actiegroep te maken.

Met de actiegroep is gemaakt, kunt u [activiteit logboek waarschuwingen](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) of [near-realtime-waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) en gebruik de actiegroep die u hebt gemaakt.

## <a name="classic-alert"></a>Klassieke waarschuwing

Klassieke waarschuwingen zijn gebaseerd op metrische gegevens en kan niet werken met actiegroepen, maar zijn acties runbook op basis van deze. Gebruik de volgende stappen uit voor het maken van een klassieke waarschuwing:

1. Selecteer **+ metrische waarschuwing toevoegen**.

1. Naam van de metrische waarschuwing 'myVMCPUAlert' en geef een korte beschrijving voor de waarschuwing.

1. De voorwaarde voor de metrische waarschuwing als 'Die groter is dan' en de periode instellen als 'op de laatste vijf minuten' de drempelwaarde ingesteld als "10".

1. Onder **maatregelen**, selecteer **een runbook uitvoeren vanuit deze waarschuwing**

1. Op de **Runbook configureren** pagina **gebruiker** voor **runbookbron**. Kies uw automation-account en selecteer de **Stop AzureVmInResponsetoVMAlert** runbook. Klik op **OK**, en klik vervolgens op **OK** om opnieuw op te slaan van de waarschuwingsregel.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het starten van automation-runbooks met webhooks [een runbook starten vanuit een webhook](automation-webhooks.md)
* Zie voor meer informatie over verschillende manieren om een runbook te starten [een Runbook starten](automation-starting-a-runbook.md).
* Zie voor meer informatie over het maken van een activiteit logboek waarschuwing, [logboek waarschuwingen voor de activiteit maken](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)
* Als u wilt zien hoe u een waarschuwing voor near realtime wilt maken, gaat u naar [een waarschuwingsregel maken met de Azure portal](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).