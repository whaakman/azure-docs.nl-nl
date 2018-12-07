---
title: PowerShell gebruiken om te maken en configureren van een Log Analytics-werkruimte | Microsoft Docs
description: Log Analytics maakt gebruik van gegevens van servers in uw on-premises of cloud-infrastructuur. U kunt gegevens van de machine verzamelen uit Azure storage wanneer die worden gegenereerd door Azure diagnostics.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: conceptual
ms.date: 11/21/2016
ms.author: richrund
ms.component: ''
ms.openlocfilehash: 6122c1ef14d8ed1d93f682fdb3dbaaaaf30efabc
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015021"
---
# <a name="manage-log-analytics-using-powershell"></a>Log Analytics beheren met PowerShell
U kunt de [Log Analytics PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) verschillende functies in Log Analytics uitvoeren vanaf een opdrachtregel of als onderdeel van een script.  Voorbeelden van de taken die u met PowerShell uitvoeren kunt zijn:

* Een werkruimte maken
* Toevoegen of verwijderen van een oplossing
* Importeren en exporteren van opgeslagen zoekopdrachten
* Een computergroep maken
* Verzamelen van IIS-logboeken van computers met de Windows-agent is geïnstalleerd
* Verzamelen van prestatiemeteritems van Linux en Windows-computers
* Gebeurtenissen verzamelen van syslog op Linux-computers 
* Gebeurtenissen verzamelen van Windows-gebeurtenislogboeken
* Aangepaste logboeken verzamelen
* De log analytics-agent toevoegen aan een virtuele machine van Azure
* Log analytics om gegevens te indexeren die zijn verzameld met behulp van Azure diagnostics configureren

In dit artikel bevat twee codevoorbeelden van die illustratie van enkele van de functies die u vanuit PowerShell uitvoeren kunt.  U kunt verwijzen naar de [Log Analytics PowerShell-cmdlet-verwijzing](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) voor andere functies.

> [!NOTE]
> Log Analytics werd voorheen Operational Insights, die daarom is het de naam die wordt gebruikt in de cmdlets genoemd.
> 
> 

## <a name="prerequisites"></a>Vereisten
Deze voorbeelden werken met versie 2.3.0 of hoger van de module AzureRm.OperationalInsights.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Maken en configureren van een Log Analytics-werkruimte
Het volgende voorbeeldscript laat zien hoe u:

1. Een werkruimte maken
2. Lijst van de beschikbare oplossingen
3. Oplossingen toevoegen aan de werkruimte
4. Importeren opgeslagen zoekopdrachten
5. Export opgeslagen zoekopdrachten
6. Een computergroep maken
7. Verzamelen van IIS-logboeken van computers met de Windows-agent is geïnstalleerd
8. Verzamelen van prestatiemeteritems voor logische schijf van Linux-computers (% gebruikte Inodes Beschikbare Megabytes; Percentage gebruikte ruimte; Schijfoverdrachten per seconde; Schijf lezen per seconde; Schijf schrijven per seconde)
9. Syslog-gebeurtenissen verzamelen van Linux-computers
10. Fout- en waarschuwingsberichten gebeurtenissen verzamelen van het logboek voor toepassingsgebeurtenissen van Windows-computers
11. Beschikbaar geheugen in megabytes-prestatiemeteritem verzamelen van Windows-computers
12. Een aangepast logboek verzamelen 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group based on a query
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Create a computer group based on names (up to 5000)
$computerGroup = """servername1.contoso.com"",""servername2.contoso.com"",""servername3.contoso.com"",""servername4.contoso.com"""
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Named Servers" -DisplayName "Named Servers" -Category "My Saved Searches" -Query $computerGroup -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Log Analytics om te indexeren van Azure diagnostics configureren
Voor bewaking zonder agent van Azure-resources, moeten de resources hebben van Azure diagnostics ingeschakeld en geconfigureerd voor het schrijven naar Log Analytics-werkruimte. Deze methode verzendt die gegevens rechtstreeks naar Log Analytics en vereist geen gegevens worden geschreven naar een opslagaccount. Ondersteunde resources zijn onder andere:

| Resourcetype | Logboeken | Metrische gegevens |
| --- | --- | --- |
| Toepassingsgateways    | Ja | Ja |
| Automation-accounts     | Ja | |
| Batch-accounts          | Ja | Ja |
| Data Lake analytics     | Ja | | 
| Data Lake store         | Ja | |
| Elastische SQL-groep        |     | Ja |
| Event hub-naamruimte     |     | Ja |
| IoT Hubs                |     | Ja |
| Key Vault               | Ja | |
| Load balancers          | Ja | |
| Logic Apps              | Ja | Ja |
| Netwerkbeveiligingsgroepen | Ja | |
| Azure Cache voor Redis             |     | Ja |
| Services zoeken         | Ja | Ja |
| Service Bus-naamruimte   |     | Ja |
| SQL (v12)               |     | Ja |
| Web Sites               |     | Ja |
| Web Server-farms        |     | Ja |

Raadpleeg voor de details van de beschikbare metrische gegevens, [ondersteunde metrische gegevens met Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

Raadpleeg voor de details van de beschikbare logboeken, [ondersteunde services en -schema voor diagnostische logboeken](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

U kunt ook de voorgaande cmdlet gebruiken om Logboeken te verzamelen uit resources die zich in verschillende abonnementen. De cmdlet kan werken voor abonnementen, omdat u de id van zowel de bron die het maken van Logboeken en de werkruimte die de logboeken zijn verzonden naar opgeeft.


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a>Log Analytics om te indexeren van Azure diagnostics uit de opslag configureren
Voor het verzamelen van logboekgegevens van binnen een actief exemplaar van een klassieke cloudservice of een service fabric-cluster, moet u eerst de gegevens schrijven naar Azure storage. Log Analytics wordt dan geconfigureerd voor het verzamelen van de logboeken van het storage-account. Ondersteunde resources zijn onder andere:

* Klassieke cloudservices (web- en werkrollen rollen)
* Service fabric-clusters

Het volgende voorbeeld wordt getoond hoe u:

1. De bestaande opslagaccounts en de locaties die Log Analytics worden gegevens van index
2. Een configuratie lezen van een storage-account maken
3. Werk de zojuist gemaakte configuratie om gegevens te indexeren vanaf meer locaties
4. De zojuist gemaakte configuratie verwijderen

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

U kunt dit script ook gebruiken voor het verzamelen van Logboeken van de storage-accounts in verschillende abonnementen. Het script kan werken voor abonnementen, omdat u de resource-id van de storage-account en een bijbehorende toegangssleutel opgeeft. Wanneer u de toegangssleutel wijzigt, moet u het opslaginzicht als u de nieuwe sleutel wilt bijwerken.


## <a name="next-steps"></a>Volgende stappen
* [Controleer de Log Analytics PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) voor meer informatie over het gebruik van PowerShell voor de configuratie van Log Analytics.

