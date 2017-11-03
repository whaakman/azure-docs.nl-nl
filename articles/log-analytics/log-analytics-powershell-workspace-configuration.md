---
title: PowerShell gebruiken voor het maken en configureren van een Log Analytics-werkruimte | Microsoft Docs
description: Log Analytics maakt gebruik van gegevens van servers in uw on-premises of cloud-infrastructuur. U kunt gegevens van de machine van Azure storage wanneer gegenereerd door Azure diagnostics verzamelen.
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 11/21/2016
ms.author: richrund
ms.openlocfilehash: 6807ab67e3593da82c147669b29bfdae3b6c967c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-log-analytics-using-powershell"></a>Log Analytics beheren met PowerShell
U kunt de [Log Analytics PowerShell-cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) diverse functies uitvoeren in logboekanalyse vanaf een opdrachtregel of als onderdeel van een script.  Voorbeelden van de taken die u met PowerShell uitvoeren kunt zijn:

* Een werkruimte maken
* Toevoegen of verwijderen van een oplossing
* Importeren en exporteren van opgeslagen zoekopdrachten
* Maak een computergroep
* Verzamelen van IIS-logboeken van computers met de Windows-agent die is geïnstalleerd
* Verzamelen van prestatiemeteritems van Linux- en Windows-computers
* Gebeurtenissen verzamelen van syslog op Linux-computers 
* Verzamelen van gebeurtenissen van Windows-gebeurtenislogboeken
* Aangepaste logboeken verzamelen
* Log analytics agent toevoegen aan een virtuele machine van Azure
* Log analytics om gegevens te indexeren verzameld met behulp van Azure diagnostics configureren

Dit artikel bevat twee codevoorbeelden die illustratie van enkele van de functies die u vanuit PowerShell uitvoeren kunt.  U kunt verwijzen naar de [Log Analytics PowerShell-cmdlet-verwijzing](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) voor andere functies.

> [!NOTE]
> Log Analytics is eerder aangeroepen voor operationeel inzicht, dat is waarom dit de naam in de cmdlets gebruikt is.
> 
> 

## <a name="prerequisites"></a>Vereisten
Deze voorbeelden werkt met versie 2.3.0 of hoger van de module AzureRm.OperationalInsights.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Maak en configureer een Log Analytics-werkruimte
Het volgende voorbeeldscript ziet u hoe:

1. Een werkruimte maken
2. Lijst van beschikbare oplossingen
3. Oplossingen toevoegen aan de werkruimte
4. Importeren opgeslagen zoekopdrachten
5. Export opgeslagen zoekopdrachten
6. Maak een computergroep
7. Verzamelen van IIS-logboeken van computers met de Windows-agent die is geïnstalleerd
8. Verzamelen van prestatiemeteritems voor logische schijf van Linux-computers (% gebruikte Inodes Beschikbare Megabytes; Percentage gebruikte ruimte; Schijfoverdrachten per seconde; Schijf lezen per seconde; Schijf schrijven per seconde)
9. Syslog-gebeurtenissen verzamelen van Linux-computers
10. Fout- en waarschuwingsberichten gebeurtenissen verzamelen uit het gebeurtenislogboek van toepassing op Windows-computers
11. Beschikbaar geheugen in megabytes-prestatiemeteritem verzamelen van Windows-computers
12. Een aangepaste logboekgegevens verzamelen 

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

#List enabled solutions
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

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Log Analytics index van Azure diagnostics configureren
Voor de bewaking zonder agents van Azure-resources, moeten de resources Azure diagnostics ingeschakeld en geconfigureerd voor het schrijven naar een werkruimte voor logboekanalyse. Deze methode verzendt gegevens rechtstreeks naar het Log Analytics en vereist geen gegevens naar een opslagaccount worden geschreven. Ondersteunde resources omvatten:

| Resourcetype | Logboeken | Metrische gegevens |
| --- | --- | --- |
| Toepassingsgateways    | Ja | Ja |
| Automation-accounts     | Ja | |
| Batch-accounts          | Ja | Ja |
| Data Lake analytics     | Ja | | 
| Data Lake store         | Ja | |
| Elastische SQL-groep        |     | Ja |
| Event Hub-naamruimte     |     | Ja |
| IoT-Hubs                |     | Ja |
| Key Vault               | Ja | |
| Taakverdelers          | Ja | |
| Logic Apps              | Ja | Ja |
| Netwerkbeveiligingsgroepen | Ja | |
| Redis Cache             |     | Ja |
| Services zoeken         | Ja | Ja |
| Service Bus-naamruimte   |     | Ja |
| SQL (v12)               |     | Ja |
| Web Sites               |     | Ja |
| Webserver-farms        |     | Ja |

Raadpleeg voor de details van de beschikbare metrische gegevens, [ondersteund met een Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

Raadpleeg voor de details van de beschikbare logboeken [ondersteund services en het schema voor diagnostische logboeken](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

U kunt ook de voorgaande cmdlet gebruiken voor het verzamelen van Logboeken van de resources die tot verschillende abonnementen behoren. De cmdlet kan werken voor abonnementen, omdat u de id van zowel de bron voor het maken van Logboeken en de werkruimte die de logboeken worden verzonden naar opgeeft.


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a>Log Analytics index van Azure diagnostics uit de opslag configureren
Voor het verzamelen van logboekgegevens van binnen een actief exemplaar van een klassieke cloudservice of een service fabric-cluster, moet u eerst de gegevens te schrijven naar Azure storage. Log Analytics wordt vervolgens geconfigureerd voor het verzamelen van de logboeken van het opslagaccount. Ondersteunde resources omvatten:

* Klassieke cloudservices (web- en werkrollen rollen)
* Service fabric-clusters

Het volgende voorbeeld wordt getoond hoe:

1. Weergeven van de bestaande opslagaccounts en de locaties die logboekanalyse indexeert gegevens uit
2. Een configuratie lezen van een opslagaccount maken
3. De zojuist gemaakte configuratie bijwerken om gegevens te indexeren van extra locaties
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

U kunt dit script ook gebruiken voor het verzamelen van Logboeken van opslagaccounts met verschillende abonnementen. Het script is kunnen werken voor abonnementen, omdat u de resource-id van de storage-account en een bijbehorende toegangssleutel opgeeft. Wanneer u de toegangssleutel wijzigt, moet u het inzicht opslag als u de nieuwe sleutel wilt bijwerken.


## <a name="next-steps"></a>Volgende stappen
* [Bekijk Log Analytics PowerShell-cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) voor meer informatie over het gebruik van PowerShell voor de configuratie van logboekanalyse.

