---
title: " Herstellen van de Azure VM waarschuwingen met Automation-Runbooks | Microsoft Docs"
description: Dit artikel wordt beschreven hoe u waarschuwingen van de virtuele Machine van Azure integreren met Azure Automation-runbooks en automatisch oplossen van problemen
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: csand;magoedte
ms.openlocfilehash: ef18a2de8ce62de945c49b91dc74aca7d7f408b0
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Azure Automation-scenario: waarschuwingen van de virtuele machine in Azure oplossen
Azure Automation en Azure Virtual Machines zijn beschikbaar voor een nieuwe functie waarmee u kunt de virtuele Machine (VM) waarschuwingen configureren om te Automation-runbooks worden uitgevoerd. Deze nieuwe mogelijkheid kunt u automatisch uit te voeren standaard in reactie op waarschuwingen van de virtuele machine, zoals het opnieuw te starten of stoppen van de virtuele machine.

Voorheen tijdens het maken van de waarschuwingsregel VM kon u [opgeven van een Automation-webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) aan een runbook om uit te voeren van het runbook wanneer de waarschuwing is geactiveerd. Maar vereist dit dat u voor het werk van het maken van het runbook, maken van de webhook voor het runbook en klik vervolgens kopiëren en plakken van de webhook tijdens het maken van de waarschuwingsregel. Met deze nieuwe versie is het proces is veel eenvoudiger omdat u een runbook rechtstreeks uit een lijst tijdens het maken van de waarschuwingsregel kiezen kunt en een Automation-account die wordt uitgevoerd van het runbook of kies een account gemakkelijk te maken.

In dit artikel zien we u hoe eenvoudig het is een Azure VM-waarschuwing instellen en configureren van een Automation-runbook moet worden uitgevoerd als de waarschuwing wordt geactiveerd. Voorbeeldscenario's bevatten een virtuele machine opnieuw te starten wanneer het geheugengebruik sommige als gevolg van een toepassing op de virtuele machine met een geheugenlek overschrijdt of een virtuele machine stoppen wanneer de gebruiker CPU-tijd minder dan 1% voor het afgelopen uur is en niet gebruikt wordt. Ook wordt uitgelegd hoe het automatisch maken van een service-principal in uw Automation-account vereenvoudigt het gebruik van runbooks in Azure waarschuwing herbemiddeling.

## <a name="create-an-alert-on-a-vm"></a>Een waarschuwing op een virtuele machine maken
Voer de volgende stappen uit voor het configureren van een waarschuwing als u wilt een runbook starten wanneer de drempel is voldaan.

> [!NOTE]
> Met deze release alleen wordt ondersteund V2 virtuele machines en ondersteuning voor klassieke die virtuele machines wordt binnenkort toegevoegd.  
> 
> 

1. Aanmelden bij de Azure-portal en klikt u op **virtuele Machines**.  
2. Selecteer een van uw virtuele machines.  
3. Op het scherm VM in de **bewaking** sectie, klikt u op **waarschuwing regels**.
4. Op de **waarschuwing regels** deelvenster, klikt u op **waarschuwing toevoegen**.

Hiermee opent u de **een waarschuwingsregel toevoegen** pagina kunt u de voorwaarden voor de waarschuwing configureren en kiezen uit een of alle van de volgende opties: e-mailbericht verzenden naar iemand, gebruikt u een webhook om de waarschuwing naar een ander systeem doorsturen en/of voer een Automation-runbook in het antwoord proberen het probleem oplossen.

## <a name="configure-a-runbook"></a>Een runbook configureren
Selecteer voor het configureren van een runbook worden uitgevoerd wanneer de waarschuwingsdrempel van de VM wordt voldaan, **Automation-Runbook**. In de **runbook configureren** deelvenster kunt u het runbook nu wordt uitgevoerd en het Automation-account voor het uitvoeren van het runbook in.

![Configureer een Automation-runbook en een nieuw Automation-Account maken](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> Voor deze release kunt u kiezen uit drie runbooks waarmee de service – VM starten, stoppen VM of VM verwijderen (verwijderen).  De mogelijkheid om te selecteren van andere runbooks of een van uw eigen runbooks zijn beschikbaar in een toekomstige release.
> 
> 

![Runbooks kiezen](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

Nadat u een van de drie beschikbare runbooks, selecteert de **Automation-account** vervolgkeuzelijst wordt weergegeven en kunt u een automation-account als het runbook wordt uitgevoerd. Runbooks moeten worden uitgevoerd in de context van een [Automation-account](automation-security-overview.md) die zich in uw Azure-abonnement. U kunt een Automation-account dat u al hebt gemaakt, of u kunt een nieuw Automation-account voor u gemaakt.

De runbooks die worden geleverd worden geverifieerd bij Azure met behulp van een service-principal. Als u kiest voor het uitvoeren van het runbook in een van uw bestaande Automation-accounts, wordt automatisch de service principal voor u gemaakt. Als u een nieuw automatiseringsaccount maakt, vervolgens maken automatisch we het account en de service-principal. In beide gevallen worden ook twee activa gemaakt in het Automation-account: de activa van een certificaat met de naam **AzureRunAsCertificate** en de activa van een verbinding met de naam **AzureRunAsConnection**. Gebruik de runbooks **AzureRunAsConnection** te verifiëren bij Azure om de actie voor beheer op basis van de virtuele machine uitvoeren.

> [!NOTE]
> De service-principal in het abonnementsbereik wordt gemaakt en is de rol Inzender toegewezen. Deze rol is vereist om het account moet gemachtigd zijn om uit te voeren van Automation-runbooks voor het beheren van virtuele Azure-machines.  Het maken van een account Automaton en/of de service-principal is een eenmalige gebeurtenis. Zodra ze zijn gemaakt, kunt u dat account kunt gebruiken voor het uitvoeren van runbooks voor andere virtuele machine van Azure-waarschuwingen.
> 
> 

Wanneer u klikt op **OK** de waarschuwing is geconfigureerd en als u de optie voor het maken van een nieuw automatiseringsaccount hebt geselecteerd, samen met de service-principal wordt gemaakt.  Dit kan enkele seconden duren.  

![Runbook wordt geconfigureerd](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

Nadat de configuratie is voltooid, ziet u de naam van het runbook in de **een waarschuwingsregel toevoegen** pagina.

![Runbook is geconfigureerd](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Klik op **OK** in de **een waarschuwingsregel toevoegen** pagina.  De waarschuwingsregel wordt gemaakt en wordt geactiveerd als de virtuele machine actief is.

### <a name="enable-or-disable-a-runbook"></a>In- of uitschakelen van een runbook
Als u een runbook dat is geconfigureerd voor een waarschuwing hebt, kunt u deze uitschakelen zonder te verwijderen van de runbookconfiguratie. Hiermee kunt u de waarschuwing actief houden en testen mogelijk enkele van de regels voor waarschuwingen en later opnieuw inschakelen het runbook.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Een runbook maken dat met een Azure-waarschuwing werkt
Wanneer u een runbook als onderdeel van een Azure waarschuwingsregel kiest, moet het runbook bevatten logica voor het beheren van de gegevens van de waarschuwing die wordt doorgegeven aan deze.  Wanneer een runbook is geconfigureerd in een waarschuwingsregel, is een webhook gemaakt voor het runbook; Deze webhook wordt vervolgens gebruikt voor het runbook starten elke keer dat de waarschuwing wordt geactiveerd.  De werkelijke aanroep naar start het runbook is een HTTP POST-aanvraag naar de webhook-URL. De hoofdtekst van de POST-aanvraag bevat een JSON-indeling object dat nuttige eigenschappen die betrekking hebben op de waarschuwing bevat.  Zoals u hieronder zien kunt, bevat de waarschuwingsgegevens-gegevens, zoals de abonnements-id, resourceGroupName resourceName en resourceType.

### <a name="example-of-alert-data"></a>Voorbeeld van waarschuwingsgegevens
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

Wanneer de service Automation-webhook het HTTP POST-protocol ontvangt haalt gegevens van de waarschuwing en wordt doorgegeven aan het runbook in de WebhookData runbook-invoerparameter.  Hieronder ziet u een voorbeeldrunbook dat laat hoe gebruikt u de parameter WebhookData zien en ophalen van gegevens van de waarschuwing en deze gebruiken voor het beheren van de Azure resource die de waarschuwing heeft geactiveerd.

### <a name="example-runbook"></a>Voorbeeldrunbook
```
#  This runbook restarts an ARM (V2) VM in response to an Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that the alert status is 'Activated' (alert condition went from false to true)
    # and not 'Resolved' (alert condition went from true to false)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get the info needed to identify the VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is the expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate to Azure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart the VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant to be started from an Azure alert only."
}
```

## <a name="summary"></a>Samenvatting
Wanneer u een waarschuwing op een virtuele machine in Azure configureert, hebt u nu de mogelijkheid voor het configureren van eenvoudig een Automation-runbook om de herstelactie automatisch worden uitgevoerd wanneer de waarschuwing wordt geactiveerd. Voor deze release kunt u kiezen uit runbooks te starten, stoppen of verwijderen van een virtuele machine, afhankelijk van uw waarschuwing scenario. Dit is slechts een begin van het inschakelen van scenario's waarin u de acties die automatisch worden uitgevoerd wanneer een waarschuwing wordt geactiveerd (melding, het oplossen van problemen herstel) beheren.

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen

