---
title: Bewaakt de VPN-gateways bij het oplossen van Azure Network Watcher | Microsoft Docs
description: Dit artikel wordt beschreven hoe problemen met On-premises-connectiviteit met Azure Automation en Network Watcher vaststellen
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
ms.openlocfilehash: 4995d7ae846652c374a289603f29f88f6f56dfef
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485490"
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>Bewaakt de VPN-gateways bij het oplossen van Network Watcher

Verkrijgen van inzicht in de netwerkprestaties van uw is essentieel dat u betrouwbare services aanbieden aan klanten. Het is daarom belangrijk om te snel netwerkomstandigheden storing detecteren en corrigerende maatregelen nemen om te beperken van de voorwaarde van de serviceonderbreking. Azure Automation kunt u implementeren en uitvoeren van een taak op een programmatische manier via runbooks. Met Azure Automation maakt u een perfecte recept voor het uitvoeren van doorlopende en proactieve bewaking en waarschuwingen.

## <a name="scenario"></a>Scenario

Het scenario in de volgende afbeelding is een toepassing met meerdere lagen met on premises-connectiviteit tot stand gebracht met behulp van een VPN-Gateway en -tunnel. Ervoor te zorgen dat de VPN-Gateway actief is en het uitvoeren van is essentieel dat u de prestaties van toepassingen.

Een runbook is gemaakt met een script om de verbindingsstatus van de VPN-tunnel, met behulp van de API voor het oplossen van problemen van Resource om te controleren voor de verbindingsstatus van de tunnel te controleren. Als de status niet in orde is, wordt een e-mailtrigger verzonden naar beheerders.

![Voorbeeldscenario][scenario]

In dit scenario wordt:

- Maken van een runbook aanroepen de `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet om op te lossen verbindingsstatus
- Een planning aan het runbook koppelen

## <a name="before-you-begin"></a>Voordat u begint

Voordat u dit scenario, hebt u de volgende vereisten:

- Een Azure automation-account in Azure. Zorg ervoor dat het automation-account de meest recente modules heeft en ook de AzureRM.Network-module. De AzureRM.Network-module is beschikbaar in de Modulegalerie als u wilt toevoegen aan uw automation-account.
- Een set referenties in Azure Automation configureert, moet u hebben. Meer informatie over [Azure Automation-beveiliging](../automation/automation-security-overview.md)
- Een geldig SMTP-server (Office 365, uw on-premises e-mail of een andere) en referenties die zijn opgegeven in Azure Automation
- Een geconfigureerde virtuele netwerkgateway in Azure.
- Een bestaand opslagaccount met een bestaande container voor het opslaan van de logboeken in.

> [!NOTE]
> De infrastructuur die wordt weergegeven in de vorige afbeelding is ter illustratie wordt en niet worden gemaakt met de stappen in dit artikel.

### <a name="create-the-runbook"></a>Het runbook te maken

De eerste stap bij het configureren van het voorbeeld is het maken van het runbook. Dit voorbeeld wordt een run as-account. Voor meer informatie over run as-accounts, gaat u naar [Runbooks verifiëren met Azure uitvoeren als-account](../automation/automation-create-runas-account.md)

### <a name="step-1"></a>Stap 1

Ga naar Azure Automation in de [Azure-portal](https://portal.azure.com) en klikt u op **Runbooks**

![overzicht van Automation-account][1]

### <a name="step-2"></a>Stap 2

Klik op **een runbook toevoegen** het proces voor het maken van het runbook te starten.

![runbooks-blade][2]

### <a name="step-3"></a>Stap 3

Onder **snelle invoer**, klikt u op **een nieuw runbook maken** om het runbook te maken.

![toevoegen van een runbookblade][3]

### <a name="step-4"></a>Stap 4

In deze stap we het runbook een naam geven, in het voorbeeld deze heet **Get-VPNGatewayStatus**. Het is belangrijk dat u het runbook een beschrijvende naam geven en aanbevolen een naam die standard PowerShell naamgevingsstandaarden volgt wordt gegeven. De runbooktype voor dit voorbeeld is **PowerShell**, de andere opties zijn grafisch, PowerShell-werkstroom, en grafische PowerShell-werkstroom.

![runbookblade][4]

### <a name="step-5"></a>Stap 5

In deze stap die het runbook is gemaakt, biedt het volgende codevoorbeeld alle code die nodig zijn voor het voorbeeld. De items in de code die bevatten \<waarde\> moet worden vervangen door de waarden uit uw abonnement.

Gebruik de volgende code als Klik **opslaan**

```powershell
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

Wanneer het runbook is opgeslagen, moet een planning zijn gekoppeld aan deze voor het automatiseren van het begin van het runbook. Als u wilt het proces starten, klikt u op **planning**.

![Stap 6][6]

## <a name="link-a-schedule-to-the-runbook"></a>Een planning aan het runbook koppelen

Een nieuw schema moet worden gemaakt. Klik op **een planning koppelen aan uw runbook**.

![Stap 7][7]

### <a name="step-1"></a>Stap 1

Op de **planning** blade, klikt u op **een nieuw schema maken**

![Stap 8][8]

### <a name="step-2"></a>Stap 2

Op de **nieuwe planning** blade, vult u de schema-informatie. Er zijn de waarden die kunnen worden ingesteld in de volgende lijst:

- **Naam** -de beschrijvende naam van de planning.
- **Beschrijving** -een beschrijving van de planning.
- **Start** -deze waarde is een combinatie van datum, tijd en tijdzone die gezamenlijk de tijd de schematriggers.
- **Terugkeerpatroon** -deze waarde bepaalt de herhaling van de schema's.  Geldige waarden zijn **eenmaal** of **periodiek**.
- **Herhaald elke** -interval van het terugkeerpatroon van de planning in uren, dagen, weken of maanden.
- **Instellen dat verlopen** -bepaalt de waarde als het schema of niet verlopen moet. Kan worden ingesteld op **Ja** of **Nee**. Een geldige datum en tijd moeten worden opgegeven als Ja is geselecteerd.

> [!NOTE]
> Als u nodig hebt om een runbook vaker dan eenmaal per uur worden uitgevoerd, moeten meerdere schema's worden gemaakt met verschillende intervallen (dat wil zeggen, 15, 30, 45 minuten na het uur)

![Stap 9][9]

### <a name="step-3"></a>Stap 3

Klik op Opslaan om op te slaan van de planning aan het runbook.

![Stap 10][10]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een goed begrip van het integreren van Network Watcher oplossen van problemen met Azure Automation hebt, informatie over het activeren van pakketopnamen op waarschuwingen van de virtuele machine door naar de pagina [een waarschuwing geactiveerd pakketopname maken met Azure Network Watcher](network-watcher-alert-triggered-packet-capture.md).

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
