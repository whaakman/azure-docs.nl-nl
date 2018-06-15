---
title: VPN-gateways met Azure-netwerk-Watcher probleemoplossing bewaken | Microsoft Docs
description: Dit artikel wordt beschreven hoe sporen On-premises-connectiviteit met Azure Automation en netwerk-Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: a102916bb0626f5b110fb134a8a25c902cfaefe7
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31598129"
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>VPN-gateways bij het oplossen van netwerk-Watcher bewaken

Veel inzicht op de netwerkprestaties van uw krijgen is essentieel voor betrouwbare services aanbieden aan klanten. Daarom is het belangrijk om te detecteren onderbreking netwerkomstandigheden snel en corrigerende maatregelen nemen om het risico van de voorwaarde onderbreking te. Azure Automation kunt u voor het implementeren en uitvoeren van een taak op een programmatische wijze via runbooks. Met behulp van Azure Automation maakt een perfecte recept voor het uitvoeren van continue en proactieve netwerkbewaking en waarschuwingen.

## <a name="scenario"></a>Scenario

Het scenario in de volgende afbeelding is een toepassing met meerdere lagen met op premises-verbinding tot stand gebracht via een VPN-Gateway en de tunnel. Zodat de VPN-Gateway actief is en wordt uitgevoerd is essentieel dat u de toepassingsprestaties.

Een runbook met een script om te controleren op de verbindingsstatus van de VPN-tunnel, met behulp van de Resource probleemoplossing-API om te controleren op verbindingsstatus tunnel gemaakt. Als de status niet in orde is, wordt een trigger e-mailbericht verzonden naar beheerders.

![Voorbeeldscenario][scenario]

Dit scenario wordt:

- Maken van een runbook aanroepen de `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet verbindingsstatus oplossen
- Een planning aan het runbook koppelen

## <a name="before-you-begin"></a>Voordat u begint

Voordat u dit scenario begint, hebt u de volgende vereisten:

- Een Azure automation-account in Azure. Zorg ervoor dat het automation-account de meest recente modules zijn en ook de module AzureRM.Network heeft. De module AzureRM.Network is beschikbaar in de Modulegalerie als u wilt toevoegen aan uw automation-account.
- U moet een set referenties configureren in Azure Automation hebben. Meer informatie in [Azure Automation-beveiliging](../automation/automation-security-overview.md)
- Een geldig SMTP-server (Office 365, uw on-premises e-mail of andere) en de referenties die zijn gedefinieerd in Azure Automation
- Een geconfigureerde virtuele netwerkgateway in Azure.
- Een bestaand opslagaccount met een bestaande container voor het opslaan van de logboeken in.

> [!NOTE]
> De infrastructuur beschreven in de vorige afbeelding is ter illustratie en zijn niet gemaakt met de stappen in dit artikel.

### <a name="create-the-runbook"></a>Het runbook te maken

De eerste stap bij het configureren van het voorbeeld is het maken van het runbook. In dit voorbeeld wordt een run as-account. Ga voor meer informatie over run as-accounts naar [Runbooks verifiëren met Azure uitvoeren als-account](../automation/automation-create-runas-account.md)

### <a name="step-1"></a>Stap 1

Navigeer naar de Azure Automation in de [Azure-portal](https://portal.azure.com) en klik op **Runbooks**

![overzicht van Automation-account][1]

### <a name="step-2"></a>Stap 2

Klik op **een runbook toevoegen** het proces voor het maken van het runbook starten.

![runbooks-blade][2]

### <a name="step-3"></a>Stap 3

Onder **snelle invoer**, klikt u op **een nieuw runbook maken** om het runbook te maken.

![toevoegen van een runbookblade][3]

### <a name="step-4"></a>Stap 4

In deze stap we het runbook een naam geven, in het voorbeeld wordt aangeroepen **Get-VPNGatewayStatus**. Het is belangrijk om het runbook een beschrijvende naam geven en aanbevolen hieraan een naam die standaard PowerShell naamgevingsstandaarden volgt. Het runbooktype voor dit voorbeeld is **PowerShell**, de overige opties zijn grafisch, PowerShell workflow, en de grafische PowerShell-werkstroom.

![runbookblade][4]

### <a name="step-5"></a>Stap 5

In deze stap is die het runbook is gemaakt, biedt het volgende codevoorbeeld alle code nodig voor het voorbeeld. De items in de code die bevatten \<waarde\> moeten worden vervangen door de waarden van uw abonnement.

De volgende code gebruiken als Klik **opslaan**

```PowerShell
# Set these variables to the proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<to email address>"
$smtpServer = "<smtp.office365.com>"
$smtpPort = 587
$runAsConnectionName = "<AzureRunAsConnection>"
$subscriptionId = "<subscription id>"
$region = "<Azure region>"
$vpnConnectionName = "<vpn connection name>"
$vpnConnectionResourceGroup = "<resource group name>"
$storageAccountName = "<storage account name>"
$storageAccountResourceGroup = "<resource group name>"
$storageAccountContainer = "<container name>"

# Get credentials for Office 365 account
$cred = Get-AutomationPSCredential -Name $o365AutomationCredential

# Get the connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in to Azure..."
Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context to a specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView the logs at $($storagePath) to learn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -To $toEmail `
        -Subject $subject `
        -Body $body `
        -UseSsl `
        -Port $smtpPort `
        -SmtpServer $smtpServer `
        -From $fromEmail `
        -BodyAsHtml `
        -Credential $cred
    }
else
    {
    Write-Output ("Connection Status is: $($result.code)")
    }
```

### <a name="step-6"></a>Stap 6

Zodra het runbook is opgeslagen, moet een planning worden gekoppeld aan het begin van het runbook automatiseren. Om het te starten, klikt u op **planning**.

![Stap 6][6]

## <a name="link-a-schedule-to-the-runbook"></a>Een planning aan het runbook koppelen

Een nieuw schema moet worden gemaakt. Klik op **een planning aan uw runbook koppelen**.

![Stap 7][7]

### <a name="step-1"></a>Stap 1

Op de **planning** blade, klikt u op **een nieuw schema maken**

![Stap 8][8]

### <a name="step-2"></a>Stap 2

Op de **nieuwe planning** blade Vul de schema-informatie. De waarden die kunnen worden ingesteld, zijn in de volgende lijst:

- **Naam** -de beschrijvende naam van de planning.
- **Beschrijving** -een beschrijving van de planning.
- **Start** -deze waarde is een combinatie van datum, tijd en tijdzone die gezamenlijk de tijd de schema-triggers.
- **Terugkeerpatroon** -deze waarde bepaalt de herhaling schema's.  Geldige waarden zijn **eenmaal** of **terugkerend**.
- **Herhaald elke** -het terugkeerpatroon van de planning in uren, dagen, weken of maanden.
- **Instellen dat verlopen** -bepaalt de waarde als het schema of niet verlopen moet. Kan worden ingesteld op **Ja** of **Nee**. Een geldige datum en tijd moeten worden opgegeven als u Ja kiest.

> [!NOTE]
> Als u een runbookserver vaker dan elk uur uitgevoerd wilt, moeten meerdere planningen worden gemaakt met een ander interval (dat wil zeggen, 15, 30, 45 minuten na het uur)

![Stap 9][9]

### <a name="step-3"></a>Stap 3

Klik op Opslaan om op te slaan van het schema aan het runbook.

![Stap 10][10]

## <a name="next-steps"></a>Volgende stappen

Nu dat u inzicht over het integreren van netwerk-Watcher het oplossen van problemen met Azure Automation hebt, informatie over het activeren van pakket opnamen van VM-waarschuwingen in via [een waarschuwing geactiveerd pakketopname maken met Azure-netwerk-Watcher](network-watcher-alert-triggered-packet-capture.md).

<!-- images -->
[scenario]: ./media/network-watcher-monitor-with-azure-automation/scenario.png
[1]: ./media/network-watcher-monitor-with-azure-automation/figure1.png
[2]: ./media/network-watcher-monitor-with-azure-automation/figure2.png
[3]: ./media/network-watcher-monitor-with-azure-automation/figure3.png
[4]: ./media/network-watcher-monitor-with-azure-automation/figure4.png
[5]: ./media/network-watcher-monitor-with-azure-automation/figure5.png
[6]: ./media/network-watcher-monitor-with-azure-automation/figure6.png
[7]: ./media/network-watcher-monitor-with-azure-automation/figure7.png
[8]: ./media/network-watcher-monitor-with-azure-automation/figure8.png
[9]: ./media/network-watcher-monitor-with-azure-automation/figure9.png
[10]: ./media/network-watcher-monitor-with-azure-automation/figure10.png
