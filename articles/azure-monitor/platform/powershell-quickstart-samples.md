---
title: Voorbeelden om snel aan de slag met Azure Monitor PowerShell
description: PowerShell gebruiken voor toegang tot Azure Monitor-functies zoals automatisch schalen, waarschuwingen, webhooks en activiteitenlogboeken te zoeken.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 2/14/2018
ms.author: robb
ms.subservice: ''
ms.openlocfilehash: d3bd4d80f167a496c9b980891f31d30bc86a0cb2
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54473756"
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Voorbeelden om snel aan de slag met Azure Monitor PowerShell
In dit artikel ziet u PowerShell-opdrachten kunt u toegang tot de functies van Azure Monitor een steekproef.

> [!NOTE]
> Azure Monitor is de nieuwe naam voor wat er is met de naam 'Azure Insights' tot 25 september 2016. Echter, de naamruimten en dus de volgende opdrachten nog steeds het woord 'insights'.
> 
> 

## <a name="set-up-powershell"></a>PowerShell instellen
Als u niet hebt gedaan, kunt u instellen van PowerShell om uit te voeren op uw computer. Zie voor meer informatie, [installeren en configureren van PowerShell](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>Voorbeelden in dit artikel
De voorbeelden in het artikel ziet u hoe u Azure Monitor-cmdlets kunt gebruiken. U kunt ook de volledige lijst met Azure Monitor PowerShell-cmdlets op bekijken [Cmdlets van Azure Monitor (inzichten)](https://docs.microsoft.com/powershell/module/azurerm.insights).

## <a name="sign-in-and-use-subscriptions"></a>Meld u aan en abonnementen gebruiken
Eerst, meld u aan bij uw Azure-abonnement.

```PowerShell
Connect-AzureRmAccount
```

Hier ziet u een aanmeldingsscherm. Nadat u uw Account, TenantID, zich aanmelden en standaard abonnements-ID worden weergegeven. Alle Azure-cmdlets werken in de context van uw standaardabonnement. U hebt toegang tot de volgende opdracht gebruiken om de lijst met abonnementen weer te geven:

```PowerShell
Get-AzureRmSubscription
```

Als u wilt uw context werken naar een ander abonnement wijzigen, gebruik de volgende opdracht:

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Activiteitenlogboek voor een abonnement ophalen
Gebruik de `Get-AzureRmLog` cmdlet.  Hier volgen enkele algemene voorbeelden.

Logboekvermeldingen ophalen uit deze tijd/datum om weer te geven:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Logboekvermeldingen tussen een datum-/ bereik ophalen:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Logboekvermeldingen ophalen uit een specifieke resourcegroep:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Logboekvermeldingen ophalen uit een specifieke resourceprovider tussen een datum-/ bereik:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Haal alle logboekvermeldingen met een specifieke aanroeper:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

De volgende opdracht haalt de laatste 1000 gebeurtenissen uit het activiteitenlogboek:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog` biedt ondersteuning voor veel andere parameters. Zie de `Get-AzureRmLog` verwijzing voor meer informatie.

> [!NOTE]
> `Get-AzureRmLog` biedt alleen 15 dagen. Met behulp van de **- MaxEvents** parameter kunt u een query uitvoeren op de laatste N-gebeurtenissen langer dan 15 dagen. Voor toegang tot gebeurtenissen die ouder zijn dan 15 dagen, moet u de REST-API of SDK (voorbeeld van C# met behulp van de SDK) gebruiken. Als u geen **StartTime**, dan is de standaardwaarde **EndTime** min één uur. Als u geen **EndTime**, en vervolgens de standaardwaarde is de huidige tijd. Alle tijden zijn in UTC.
> 
> 

## <a name="retrieve-alerts-history"></a>Geschiedenis van waarschuwingen ophalen
Als u wilt weergeven van alle waarschuwingsgebeurtenissen, kunt u een query de logboeken van de Azure Resource Manager met behulp van de volgende voorbeelden.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Als u de geschiedenis voor een specifieke waarschuwingsregel, kunt u de `Get-AzureRmAlertHistory` cmdlet, doorgeven in de resource-ID van de waarschuwingsregel.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

De `Get-AzureRmAlertHistory` cmdlet biedt ondersteuning voor verschillende parameters. Meer informatie, Zie [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>Ophalen van informatie over de regels voor waarschuwingen
Alle van de volgende opdrachten fungeren in een resourcegroep met de naam 'montest'.

Alle eigenschappen van de waarschuwingsregel weergeven:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Alle waarschuwingen voor een resourcegroep ophalen:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Alle regels voor waarschuwingen instellen voor een doelresource worden opgehaald. Bijvoorbeeld, instellen alle waarschuwingsregels op een virtuele machine.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule` biedt ondersteuning voor andere parameters. Zie [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) voor meer informatie.

## <a name="create-metric-alerts"></a>Metrische waarschuwingen maken
U kunt de `Add-AlertRule` cmdlet voor het maken, bijwerken of een waarschuwingsregel uitschakelen.

U kunt maken e-mail en webhook-eigenschappen met behulp van `New-AzureRmAlertRuleEmail` en `New-AzureRmAlertRuleWebhook`, respectievelijk. In de waarschuwingsregel-cmdlet kunt u deze eigenschappen toewijzen als acties aan de **acties** eigenschap van de waarschuwingsregel.

De volgende tabel beschrijft de parameters en waarden die worden gebruikt voor het maken van een waarschuwing met behulp van een metrische waarde.

| Parameter | waarde |
| --- | --- |
| Name |simpletestdiskwrite |
| Locatie van deze waarschuwingsregel |US - oost |
| ResourceGroup |montest |
| TargetResourceId |/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
| MetricName van de waarschuwing die is gemaakt |\Disk \PhysicalDisk (_Totaal) per seconde. Zie de `Get-MetricDefinitions` cmdlet over het ophalen van de exacte metrische namen |
| operator |GreaterThan |
| De waarde voor drempel (aantal per seconde in voor deze metrische gegevens) |1 |
| Venstergrootte (uu: mm: indeling) |00:05:00 |
| aggregator (statistiek op van de metrische gegevens, die in dit geval maakt gebruik van gemiddeld aantal) |Gemiddeld |
| aangepaste e-mailberichten (string-matrix) |'foo@example.com','bar@example.com' |
| e-mail sturen naar eigenaars, medewerkers en lezers |-SendToServiceOwners |

Maken van een e-mailactie

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

De webhookactie van een maken

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

De waarschuwingsregel op de CPU % metrische gegevens op een klassieke virtuele machine maken

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

De waarschuwingsregel ophalen

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

De waarschuwing toevoegen-cmdlet werkt ook de regel als er al een waarschuwingsregel voor de opgegeven eigenschappen bestaat. Als u wilt een waarschuwingsregel uitschakelen, de parameter opnemen **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Een lijst met beschikbare metrische gegevens ophalen voor waarschuwingen
U kunt de `Get-AzureRmMetricDefinition` cmdlet om de lijst met alle metrische gegevens voor een specifieke resource weer te geven.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Het volgende voorbeeld wordt een tabel met de naam van de metriek en de eenheid voor het gegenereerd.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Een volledige lijst met beschikbare opties voor `Get-AzureRmMetricDefinition` is beschikbaar op [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-activity-log-alerts"></a>Waarschuwingen voor activiteitenlogboek maken en beheren
U kunt de `Set-AzureRmActivityLogAlert` cmdlet om in te stellen van een waarschuwing voor activiteitenlogboek. Een waarschuwing voor activiteitenlogboek, moet u eerst uw voorwaarden definiëren als een woordenlijst met de voorwaarden en maken van een waarschuwing die gebruikmaakt van deze voorwaarden.

```PowerShell

$condition1 = New-AzureRmActivityLogAlertCondition -Field 'category' -Equal 'Administrative'
$condition2 = New-AzureRmActivityLogAlertCondition -Field 'operationName' -Equal 'Microsoft.Compute/virtualMachines/write'
$additionalWebhookProperties = New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"
$additionalWebhookProperties.Add('customProperty', 'someValue')
$actionGrp1 = New-AzureRmActionGroup -ActionGroupId '/subscriptions/<subid>/providers/Microsoft.Insights/actiongr1' -WebhookProperty $additionalWebhookProperties
Set-AzureRmActivityLogAlert -Location 'Global' -Name 'alert on VM create' -ResourceGroupName 'myResourceGroup' -Scope '/subscriptions/<subid>' -Action $actionGrp1 -Condition $condition1, $condition2

```

De aanvullende webhookeigenschappen zijn optioneel. U kunt teruggaan de inhoud van een activiteit Log waarschuwing met `Get-AzureRmActivityLogAlert`.

## <a name="create-and-manage-autoscale-settings"></a>Maken en beheren van instellingen voor automatisch schalen
Een resource (zoals een Web-app, VM, Cloudservice of door virtuele-Machineschaalset) kan slechts één functie voor automatisch schalen instelling die is geconfigureerd voor deze hebben.
Elke instelling voor automatisch schalen kan echter meerdere profielen hebben. Bijvoorbeeld, een voor een schaalprofiel op basis van prestaties en een tweede voor een profiel op basis van een planning. Elk profiel kan meerdere regels geconfigureerd voor hebben. Zie voor meer informatie over automatisch schalen, [automatisch schalen van een toepassing](../../cloud-services/cloud-services-how-to-scale-portal.md).

Hier volgen de stappen om te gebruiken:

1. Maak (s).
2. Maak profielen eerder de regels die u hebt gemaakt voor toewijzing aan de profielen.
3. Optioneel: Meldingen voor automatisch schalen maken door webhook en het e-eigenschappen te configureren.
4. Maak een instelling voor automatisch schalen met een naam voor de doelbron door het toewijzen van de profielen en meldingen die u in de vorige stappen hebt gemaakt.

De volgende voorbeelden ziet u hoe u een instelling voor automatisch schalen voor een virtuele-Machineschaalset opgehaald voor een Windows-besturingssysteem op basis van met behulp van de CPU-gebruik metrische gegevens kunt maken.

Maak eerst een regel voor uitschalen, met een toename in exemplaar.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Maak vervolgens een regel voor inschalen, met een aantal exemplaar verkleinen.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Maak vervolgens een profiel voor de regels.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

De eigenschap van een webhook maken.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Maak de eigenschap melding voor de instelling voor automatisch schalen, met inbegrip van e-mailadres en de webhook die u eerder hebt gemaakt.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Maak ten slotte de instelling voor automatisch schalen om toe te voegen van het profiel dat u eerder hebt gemaakt. 

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Zie voor meer informatie over het beheren van instellingen voor automatisch schalen [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Historie van automatisch schalen
Het volgende voorbeeld ziet u hoe u recente schalings- en waarschuwingsgebeurtenissen kunt bekijken. Gebruik de activiteit zoeken in Logboeken om de geschiedenis voor automatisch schalen weer te geven.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

U kunt de `Get-AzureRmAutoScaleHistory` cmdlet voor het ophalen van de geschiedenis voor automatisch schalen.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Zie voor meer informatie, [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Bekijk de details voor een instelling voor automatisch schalen
U kunt de `Get-Autoscalesetting` cmdlet voor het ophalen van meer informatie over de instelling voor automatisch schalen.

Het volgende voorbeeld ziet details over alle instellingen voor automatisch schalen in de resource group 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Het volgende voorbeeld ziet details over alle instellingen voor automatisch schalen in de resource group 'myrg1' en specifiek de instelling voor automatisch schalen met de naam 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Een instelling voor automatisch schalen verwijderen
U kunt de `Remove-Autoscalesetting` cmdlet voor het verwijderen van een instelling voor automatisch schalen.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Logboekprofielen voor het activiteitenlogboek beheren
U kunt maken een *logboekprofiel* en gegevens uit uw activiteitenlogboek exporteren naar een opslagaccount en u kunt het bewaren van gegevens voor het configureren. U kunt eventueel ook de gegevens streamen naar uw Event Hub. Deze functie is momenteel in Preview en u kunt slechts één logboekprofiel per abonnement maken. U kunt de volgende cmdlets gebruiken met uw huidige abonnement naar het logboekprofielen maken en beheren. U kunt ook een bepaald abonnement. Hoewel PowerShell standaard ingesteld op het huidige abonnement, kunt u altijd wijzigen dat door `Set-AzureRmContext`. Activiteitenlogboek het routeren van gegevens naar een opslagaccount of Event Hub in dat abonnement, kunt u configureren. Gegevens worden geschreven als blob-bestanden in de JSON-indeling.

### <a name="get-a-log-profile"></a>Ophalen van een logboekprofiel
Om op te halen van uw bestaande logboekprofielen, gebruikt u de `Get-AzureRmLogProfile` cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Toevoegen van een logboekprofiel zonder het bewaren van gegevens
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Een logboekprofiel verwijderen
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Een logboekprofiel met het bewaren van gegevens toevoegen
U kunt opgeven de **- RetentionInDays** eigenschap met het aantal dagen, als een positief geheel getal, waarbij de gegevens worden bewaard.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Logboekprofiel met retentie en het Event hub toevoegen
Naast de routering van uw gegevens naar storage-account, kunt u deze ook streamen naar een Event Hub. In deze preview-versie de configuratie van het storage-account is verplicht, maar de configuratie van de Event Hub is optioneel.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Logboeken met diagnostische gegevens configureren
Veel Azure-services bieden extra logboeken en telemetrie die een of meer van de volgende kunt doen: 
 - worden geconfigureerd voor het opslaan van gegevens in uw Azure Storage-account
 - verzonden naar Event Hubs
 - verzonden naar Log Analytics-werkruimte. 

De bewerking kan alleen worden uitgevoerd op het resourceniveau van een. De storage-account of event hub moet worden gebruikt in dezelfde regio als de doelresource waar de diagnostische instelling is geconfigureerd.

### <a name="get-diagnostic-setting"></a>Diagnostische instelling ophalen
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnostische instelling uitschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Schakel diagnostische instelling zonder bewaren

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Diagnostische instelling met een bewaarperiode van inschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Schakel diagnostische instelling met een bewaarperiode voor een specifieke logboekcategorie

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Schakel diagnostische instelling voor Event Hubs

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Schakel diagnostische instelling voor Log Analytics

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId /subscriptions/s1/resourceGroups/insights-integration/providers/providers/microsoft.operationalinsights/workspaces/myWorkspace -Enabled $true

```

Houd er rekening mee dat de werkruimte-id voor deze eigenschap is de *resource-ID* van de werkruimte. U vindt de resource-ID van uw Log Analytics-werkruimte met de volgende opdracht:

```PowerShell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId

```

Deze opdrachten kunnen worden gecombineerd om gegevens te verzenden naar meerdere bestemmingen.

