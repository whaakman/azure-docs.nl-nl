---
title: Azure PowerShell Monitor snel starten-voorbeelden. | Microsoft Docs
description: PowerShell gebruiken voor toegang tot Azure Monitor functies zoals automatisch schalen, waarschuwingen, webhooks en activiteitenlogboeken zoeken.
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: robb
ms.openlocfilehash: 36836a4528c8ba04eee1c5234fd6d4e0f9545913
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure PowerShell Monitor snel starten-voorbeelden
In dit artikel worden steekproef van PowerShell-opdrachten kunt u toegang tot Azure Monitor functies. Monitor voor Azure kunt u automatisch schalen Cloudservices, virtuele Machines en Web-Apps. Ook kunt u meldingen van waarschuwingen verzenden of web-URL's op basis van waarden van de geconfigureerde telemetriegegevens aanroepen.

> [!NOTE]
> Monitor voor Azure is de nieuwe naam voor wat 'Azure Insights' is aangeroepen tot 25 september 2016. Echter, de naamruimten, en daarom de volgende opdrachten nog steeds de het woord 'insights'.
> 
> 

## <a name="set-up-powershell"></a>Instellen van PowerShell
Als u nog niet gedaan hebt, kunt u PowerShell uitvoeren op uw computer instellen. Zie voor meer informatie [installeren en configureren van PowerShell](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>Voorbeelden in dit artikel
De voorbeelden in het artikel ziet hoe u Azure Monitor cmdlets kunt gebruiken. U kunt ook de volledige lijst met Azure Monitor PowerShell-cmdlets op bekijken [Azure Monitor (Insights) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).

## <a name="sign-in-and-use-subscriptions"></a>Aanmelden en abonnementen gebruiken
Eerst aanmelden bij uw Azure-abonnement.

```PowerShell
Login-AzureRmAccount
```

Hier ziet u een teken in het scherm. Eenmaal u uw Account, TenantID, zich aanmelden en standaard abonnements-ID worden weergegeven. Alle Azure-cmdlets werkt in de context van uw standaardabonnement. U hebt toegang tot de volgende opdracht gebruiken om de lijst met abonnementen weer te geven:

```PowerShell
Get-AzureRmSubscription
```

Als u wilt uw werkende context wijzigen naar een ander abonnement, moet u de volgende opdracht gebruiken:

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Activiteitenlogboek voor een abonnement ophalen
Gebruik de `Get-AzureRmLog` cmdlet.  Hier volgen enkele algemene voorbeelden.

Logboekvermeldingen ophalen uit deze tijd/datum om weer te geven:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Logboekvermeldingen tussen een bereik van tijd/datum ophalen:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Logboekvermeldingen ophalen uit een specifieke resourcegroep:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Logboekvermeldingen ophalen van een bepaalde resourceprovider tussen een bereik van tijd/datum:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Alle logboekvermeldingen met een specifieke aanroeper ophalen:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

De volgende opdracht haalt de laatste 1000 gebeurtenissen uit het activiteitenlogboek:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`biedt ondersteuning voor veel andere parameters. Zie de `Get-AzureRmLog` documentatie voor meer informatie.

> [!NOTE]
> `Get-AzureRmLog`biedt alleen 15 dagen van de geschiedenis. Met behulp van de **- MaxEvents** parameter kunt u de laatste N-gebeurtenissen na 15 dagen een query. Gebruik de REST-API of de SDK (C# voorbeeld met de SDK) op toegangsgebeurtenissen die ouder zijn dan 15 dagen. Als u geen **StartTime**, dan is de standaardwaarde **EndTime** min één uur. Als u geen **EndTime**, en vervolgens de standaardwaarde is de huidige tijd. Alle tijden zijn in UTC.
> 
> 

## <a name="retrieve-alerts-history"></a>Ophalen van de geschiedenis van waarschuwingen
Alle waarschuwingen als gebeurtenissen wilt weergeven, kunt u de Azure Resource Manager-logboeken met behulp van de volgende voorbeelden kunt opvragen.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Als u wilt weergeven in de geschiedenis voor een specifieke waarschuwingsregel, kunt u de `Get-AzureRmAlertHistory` cmdlet in de resource-ID van de waarschuwingsregel wordt doorgegeven.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

De `Get-AzureRmAlertHistory` cmdlet ondersteunt verschillende parameters. Meer informatie Zie [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>Ophalen van informatie over de regels voor waarschuwingen
Alle van de volgende opdrachten fungeren op een resourcegroep met de naam 'montest'.

De eigenschappen van de waarschuwingsregel weergeven:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Alle waarschuwingen voor een resourcegroep ophalen:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Alle regels voor waarschuwingen instellen voor een doelresource worden opgehaald. Bijvoorbeeld instellen alle regels voor waarschuwingen op een virtuele machine.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`biedt ondersteuning voor andere parameters. Zie [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) voor meer informatie.

## <a name="create-metric-alerts"></a>Metrische waarschuwingen maken
U kunt de `Add-AlertRule` cmdlet maken, bijwerken of een waarschuwingsregel uitschakelen.

Kunt u e-mail en -webhook eigenschappen met behulp van `New-AzureRmAlertRuleEmail` en `New-AzureRmAlertRuleWebhook`respectievelijk. In de waarschuwingsregel-cmdlet kunt u deze eigenschappen toewijzen als acties voor de **acties** eigenschap van de waarschuwingsregel.

De volgende tabel beschrijft de parameters en waarden die worden gebruikt voor het maken van een waarschuwing met behulp van een metriek.

| Parameter | waarde |
| --- | --- |
| Naam |simpletestdiskwrite |
| Locatie van deze waarschuwingsregel |VS - oost |
| ResourceGroup |montest |
| TargetResourceId |/Subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig |
| MetricName van de waarschuwing die wordt gemaakt |\Disk \PhysicalDisk (_Totaal) per seconde. Zie de `Get-MetricDefinitions` cmdlet over het ophalen van de exacte metrische namen |
| Operator |GreaterThan |
| Drempelwaarde (aantal per seconde in voor deze metrische gegevens) |1 |
| Venstergrootte (indeling: mm: SS) |00:05:00 |
| aggregator (statistiek van de metrische gegevens die in dit geval gemiddeld aantal gebruikt) |Gemiddeld |
| aangepaste e-mailberichten (string array) |'foo@example.com','bar@example.com' |
| e-mail verzenden aan eigenaren, bijdragers en lezers |-SendToServiceOwners |

Een e-actie maken

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Maken van een Webhook-actie

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

De waarschuwingsregel maken op de CPU % metrische gegevens op een klassieke virtuele machine

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

De waarschuwingsregel ophalen

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

De regel de waarschuwing toevoegen-cmdlet ook bijgewerkt als er al een waarschuwingsregel voor de opgegeven eigenschappen bestaat. Als u wilt een waarschuwingsregel uitschakelen, bevatten de parameter **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Een lijst met beschikbare metrische gegevens ophalen voor waarschuwingen
U kunt de `Get-AzureRmMetricDefinition` cmdlet om de lijst met alle metrische gegevens voor een specifieke bron weer te geven.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Het volgende voorbeeld wordt een tabel met de naam van de metriek en de eenheid voor het gegenereerd.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Een volledige lijst met beschikbare opties voor `Get-AzureRmMetricDefinition` is beschikbaar op [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-autoscale-settings"></a>Maken en beheren van instellingen voor automatisch schalen
Een resource (een Web-app, VM, Cloud Service- of virtuele-Machineschaalset) kan slechts één instelling voor automatisch schalen geconfigureerd hebben.
Elke instelling voor automatisch schalen kunt echter meerdere profielen hebben. Bijvoorbeeld, een voor een profiel schalen op basis van prestaties en een tweede voor een profiel op basis van een planning. Elk profiel kan meerdere regels die zijn geconfigureerd op deze hebben. Zie voor meer informatie over automatisch schalen, [hoe een toepassing voor automatisch schalen](../cloud-services/cloud-services-how-to-scale-portal.md).

Hier volgen de stappen om te gebruiken:

1. Regel (s) maken.
2. Profielen die eerder toewijzing van de regels die u hebt gemaakt om de profielen te maken.
3. Optioneel: Maak meldingen voor automatisch schalen door webhook en e-eigenschappen te configureren.
4. Een instelling voor automatisch schalen met een naam voor de doelbron maken via het toewijzen van de profielen en meldingen die u in de vorige stappen hebt gemaakt.

De volgende voorbeelden ziet u hoe u een instelling voor automatisch schalen voor een virtuele-Machineschaalset voor een Windows-besturingssysteem op basis van met behulp van de CPU-gebruik metrische gegevens kunt maken.

Maak eerst een regel moet worden uitgebreid, met een verhoging van het aantal exemplaar.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Vervolgens maakt u een regel, met een aantal exemplaar daling schalen.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Vervolgens maakt u een profiel voor de regels.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Maak een webhook-eigenschap.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Maken van de eigenschap melding voor de instelling voor automatisch schalen, met inbegrip van e-mailadres en de webhook die u eerder hebt gemaakt.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Maak ten slotte de instelling voor automatisch schalen om toe te voegen van het profiel dat u eerder hebt gemaakt. 

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Zie voor meer informatie over het beheren van instellingen voor automatisch schalen [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Geschiedenis van automatisch schalen
Het volgende voorbeeld ziet u hoe u recente automatisch schalen en waarschuwingsgebeurtenissen kunt bekijken. Gebruik de activiteit logboek zoekopdracht om de geschiedenis voor automatisch schalen weer te geven.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

U kunt de `Get-AzureRmAutoScaleHistory` cmdlet voor het ophalen van de geschiedenis voor automatisch schalen.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Zie voor meer informatie [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Details weergeven voor een instelling voor automatisch schalen
U kunt de `Get-Autoscalesetting` cmdlet meer informatie over de instelling voor automatisch schalen op te halen.

Het volgende voorbeeld worden details weergegeven over alle instellingen voor automatisch schalen in de resource-groep 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Het volgende voorbeeld worden details weergegeven over alle instellingen voor automatisch schalen in de resource-groep 'myrg1' en specifiek de instelling voor automatisch schalen met de naam 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Verwijderen van een instelling voor automatisch schalen
U kunt de `Remove-Autoscalesetting` cmdlet verwijderen van een instelling voor automatisch schalen.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Logboek profielen beheert voor activiteitenlogboek
Kunt u een *Meld profiel* en gegevens uit uw activiteitenlogboek exporteren naar een opslagaccount en u kunt bewaren van gegevens voor het configureren. U kunt eventueel ook de gegevens naar uw Event Hub streamen. Deze functie is momenteel in Preview en u kunt slechts één log-profiel per abonnement maken. U kunt de volgende cmdlets met uw huidige abonnement logboek-profielen maken en beheren. U kunt ook een bepaald abonnement. Hoewel PowerShell wordt standaard ingesteld op het huidige abonnement, kunt u altijd wijzigen die met `Set-AzureRmContext`. U kunt configureren activiteitenlogboek van gegevens voor het routeren naar een opslagaccount of Event Hub binnen dat abonnement. Gegevens worden geschreven als blob-bestanden in de JSON-indeling.

### <a name="get-a-log-profile"></a>Een profiel voor een logboek ophalen
Voor het ophalen van de profielen van uw bestaande logboek, gebruiken de `Get-AzureRmLogProfile` cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Een profiel logboek zonder bewaren van gegevens toevoegen
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Een logboek-profiel verwijderen
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Een profiel van het logboek met bewaren van gegevens toevoegen
U kunt opgeven de **- RetentionInDays** eigenschap met het aantal dagen, als een positief geheel getal, waar de gegevens worden bewaard.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Logboek-profiel met bewaren en EventHub toevoegen
Bovendien uw gegevens naar het opslagaccount kunt u deze ook stream naar een Event Hub. In deze preview-versie de opslagconfiguratie van het account is verplicht, maar de configuratie van de Event Hub is optioneel.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Logboeken met diagnostische gegevens configureren
Veel Azure-services bieden extra logboeken en telemetrie die een of meer van de volgende kunt doen: 
 - worden geconfigureerd voor het opslaan van gegevens in uw Azure Storage-account
 - verzonden naar Event Hubs
 - verzonden naar een logboekanalyse OMS-werkruimte. 

De bewerking kan alleen worden uitgevoerd op het niveau van een resource. De storage-account of event hub moet aanwezig zijn in dezelfde regio bevinden als de doelresource waar de diagnostics-instelling is geconfigureerd.

### <a name="get-diagnostic-setting"></a>De diagnostische instelling niet ophalen
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

Schakel diagnostische instelling met bewaren

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Schakel diagnostische instelling met de bewaarperiode voor een specifieke logboek-categorie

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Schakel diagnostische instelling voor Event Hubs

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Schakel diagnostische instelling voor logboekanalyse (OMS)

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId /subscriptions/s1/resourceGroups/insights-integration/providers/providers/microsoft.operationalinsights/workspaces/myWorkspace -Enabled $true

```

Let op: de eigenschap WorkspaceId neemt de *resource-ID* van de werkruimte. Kunt u de bron-ID van de werkruimte voor logboekanalyse met de volgende opdracht:

```PowerShell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId

```

Deze opdrachten kunnen worden gecombineerd om gegevens te verzenden naar meerdere bestemmingen.
