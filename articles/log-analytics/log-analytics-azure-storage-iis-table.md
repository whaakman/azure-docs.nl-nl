---
title: Blob storage gebruiken voor de opslag van IIS en tabel gebeurtenissen in de Azure Log Analytics | Microsoft Docs
description: Log Analytics kunt u de logboeken voor Azure-services die diagnostische gegevens naar table storage schrijven of IIS-logboeken geschreven naar de blob storage lezen.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 459ef90ca1d76bada6565bfefd7b4bd1086197d5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a>Azure blob storage gebruiken voor IIS en Azure-tabelopslag voor gebeurtenissen met Log Analytics

Log Analytics kunt lezen de logboeken voor de volgende services die schrijft diagnostische gegevens naar table storage of de IIS-logboeken geschreven naar de blob storage:

* Service Fabric-clusters (Preview)
* Virtuele machines
* Web/werkrollen

Voordat u Log Analytics kunt verzamelen van gegevens voor deze bronnen, kan Azure diagnostics moet zijn ingeschakeld.

Zodra de diagnostische gegevens zijn ingeschakeld, kunt u de Azure-portal of PowerShell configureren logboekanalyse voor het verzamelen van de logboeken.

Azure Diagnostics is een Azure-uitbreiding waarmee u kunt het verzamelen van diagnostische gegevens van een werkrol, Webrol of virtuele machine in Azure wordt uitgevoerd. De gegevens wordt opgeslagen in Azure storage-account en vervolgens kan worden verzameld door logboekanalyse.

Voor logboekanalyse voor het verzamelen van deze logboeken Azure Diagnostics, moet de logboeken op de volgende locaties:

| Logboektype | Resourcetype | Locatie |
| --- | --- | --- |
| IIS-logboeken |Virtuele machines <br> Web-rollen <br> Werkrollen |af-iis-logboekbestanden (Blob-opslag) |
| Syslog |Virtuele machines |LinuxsyslogVer2v0 (tabel opslag) |
| Operationele gebeurtenissen van de service Fabric |Service Fabric-knooppunten |WADServiceFabricSystemEventTable |
| Gebeurtenissen van de service Fabric betrouwbare Actor |Service Fabric-knooppunten |WADServiceFabricReliableActorEventTable |
| Gebeurtenissen van betrouwbare Service voor service Fabric |Service Fabric-knooppunten |WADServiceFabricReliableServiceEventTable |
| Windows-gebeurtenislogboeken |Service Fabric-knooppunten <br> Virtuele machines <br> Web-rollen <br> Werkrollen |WADWindowsEventLogsTable (Table Storage) |
| Logboeken van Windows (ETW) |Service Fabric-knooppunten <br> Virtuele machines <br> Web-rollen <br> Werkrollen |WADETWEventTable (Table Storage) |

> [!NOTE]
> IIS-logboeken van Azure Websites worden momenteel niet ondersteund.
>
>

Voor virtuele machines, hebt u de optie van het installeren van de [logboekanalyse agent](log-analytics-azure-vm-extension.md) naar uw virtuele machine inschakelen als u meer inzicht te krijgen. Naast IIS-logboeken en gebeurtenislogboeken analyseren, kunt u aanvullende analyse, waaronder configuratie bijhouden, SQL-evaluatie en update-evaluatie uitvoeren.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Inschakelen van Azure diagnostics in een virtuele machine voor het logboek met systeemgebeurtenissen en IIS Meld verzameling
Gebruik de volgende procedure om in te schakelen van Azure diagnostics in een virtuele machine voor het logboek met systeemgebeurtenissen en IIS-logboekverzameling met de Microsoft Azure-portal.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Inschakelen van Azure diagnostics in een virtuele machine met de Azure-portal
1. Installeer de VM-Agent bij het maken van een virtuele machine. Als de virtuele machine al bestaat, controleert u of de VM-Agent is al geïnstalleerd.

   * Navigeer in de Azure-portal naar de virtuele machine en selecteer **optionele configuratie**, vervolgens **Diagnostics** en stel **Status** naar **op**.

     De virtuele machine heeft na voltooiing, de extensie Azure Diagnostics geïnstalleerd en uitgevoerd. Deze uitbreiding is verantwoordelijk voor het verzamelen van diagnostische gegevens.
2. Controle inschakelen en configureren van logboekregistratie in een bestaande virtuele machine. U kunt diagnostische gegevens op het niveau van de VM inschakelen. Diagnostische gegevens inschakelen en vervolgens logboekregistratie configureren, moet u de volgende stappen uitvoeren:

   1. Selecteer de virtuele machine.
   2. Klik op **bewaking**.
   3. Klik op **Diagnostics**.
   4. Stel de **Status** naar **ON**.
   5. Selecteer elk logboek diagnostische gegevens die u wilt verzamelen.
   6. Klik op **OK**.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Inschakelen van Azure diagnostics in een Webrol voor IIS-logboek en gebeurtenis-verzameling
Raadpleeg [hoe naar inschakelen diagnostische gegevens in een Cloudservice](../cloud-services/cloud-services-dotnet-diagnostics.md) voor algemene stappen over het inschakelen van Azure diagnostics. De onderstaande instructies deze gegevens gebruiken en aanpassen voor gebruik met logboekanalyse.

Met Azure diagnostics ingeschakeld:

* IIS-logboeken worden standaard opgeslagen met logboekgegevens overgedragen met het interval van de overdracht scheduledTransferPeriod.
* Windows-gebeurtenislogboeken worden standaard niet worden overgedragen.

### <a name="to-enable-diagnostics"></a>Diagnostische gegevens inschakelen
Windows-gebeurtenislogboeken inschakelen of wijzigen van de scheduledTransferPeriod, configureert u diagnostische Azure-gegevens met behulp van het XML-configuratiebestand (diagnostics.wadcfg), zoals wordt weergegeven in [stap 4: maken van uw configuratiebestand diagnostische gegevens en de uitbreiding te installeren](../cloud-services/cloud-services-dotnet-diagnostics.md)

Het volgende voorbeeld-configuratiebestand verzamelt IIS-logboeken en alle gebeurtenissen van de toepassings- en systeemlogboeken:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Zorg ervoor dat uw ConfigurationSettings geeft u een opslagaccount, zoals in het volgende voorbeeld:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

De **AccountName** en **AccountKey** waarden zijn gevonden in de Azure-portal in het dashboard van de storage-account onder toegangssleutels beheren. Het protocol voor de verbindingsreeks moet **https**.

Zodra de bijgewerkte diagnostische configuratie wordt toegepast op de cloudservice en het wegschrijven van diagnostische gegevens naar Azure Storage, vervolgens bent u klaar voor het configureren van logboekanalyse.

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>De Azure portal gebruiken voor het verzamelen van Logboeken van Azure Storage
U kunt de Azure-portal voor het configureren van logboekanalyse voor het verzamelen van de logboeken voor de volgende Azure-services:

* Service Fabric-clusters
* Virtuele machines
* Web/werkrollen

Navigeer naar de werkruimte voor logboekanalyse in de Azure-portal en de volgende taken uitvoeren:

1. Klik op *opslagaccounts Logboeken*
2. Klik op de *toevoegen* taak
3. Selecteer het opslagaccount waarin de logboeken met diagnostische gegevens
   * Dit account kan worden een klassieke storage-account of een Azure Resource Manager-storage-account
4. Selecteer het gegevenstype dat u wenst te verzamelen van Logboeken voor
   * De opties zijn IIS-logboeken; Gebeurtenissen; Syslog (Linux); ETW-Logboeken; Service Fabric-gebeurtenissen
5. De waarde voor de gegevensbron wordt automatisch ingevuld op basis van het gegevenstype en kan niet worden gewijzigd
6. Klik op OK om de configuratie op te slaan

Herhaal stappen 2 tot en met 6 voor gegevenstypen die u wilt dat logboekanalyse voor het verzamelen en extra opslagaccounts.

In ongeveer 30 minuten bent u kunt gegevens van het opslagaccount in logboekanalyse zien. U ziet alleen de gegevens die worden geschreven naar de opslag nadat de configuratie is toegepast. Log Analytics biedt de bestaande gegevens niet lezen uit het opslagaccount.

> [!NOTE]
> De portal wordt niet gevalideerd of de bron in het opslagaccount bestaat of als er nieuwe gegevens worden geschreven.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Inschakelen van Azure diagnostics in een virtuele machine voor het logboek met systeemgebeurtenissen en IIS Meld verzameling met behulp van PowerShell
Gebruik de stappen in [logboekanalyse configureren om te indexeren van Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) PowerShell gebruiken om te lezen van Azure diagnostics die zijn geschreven naar table storage.

Met Azure PowerShell kunt u preciezer opgeven de gebeurtenissen die zijn geschreven naar Azure Storage.
Zie voor meer informatie [Diagnostics inschakelen in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).

U kunt inschakelen en bijwerken van Azure diagnostics met de volgende PowerShell-script.
U kunt dit script ook gebruiken met een aangepaste configuratie voor logboekregistratie.
Het script voor het instellen van de storage-account, de servicenaam en de naam van virtuele machine wijzigen.
Het script maakt gebruik van cmdlets voor klassieke virtuele machines.

Bekijk het volgende voorbeeldscript, kopieert u deze zo nodig wijzigen, de steekproef opslaan als een PowerShell-scriptbestand en voer het script.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Volgende stappen
* [Verzamelen van Logboeken en metrische gegevens voor Azure-services](log-analytics-azure-storage.md) voor ondersteunde Azure-services.
* [Inschakelen van oplossingen](log-analytics-add-solutions.md) te bieden inzicht in de gegevens.
* [Gebruik zoekquery's](log-analytics-log-searches.md) om de gegevens te analyseren.
