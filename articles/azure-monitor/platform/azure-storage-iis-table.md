---
title: Blob storage gebruiken voor IIS en table storage voor gebeurtenissen in Azure Monitor | Microsoft Docs
description: Azure Monitor kunt u de logboeken voor Azure-services die schrijft diagnostische gegevens naar table storage of IIS-logboeken geschreven naar de blobopslag lezen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.openlocfilehash: 35befe7122f493998d0d91c2721e6013e057fed3
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540598"
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-azure-monitor"></a>Azure blob storage gebruiken voor IIS en Azure table storage voor gebeurtenissen met Azure Monitor

Azure Monitor kunt u de logboeken voor de volgende services die schrijft diagnostische gegevens naar table storage of IIS-logboeken geschreven naar de blobopslag lezen:

* Service Fabric-clusters (Preview)
* Virtuele machines
* Web-/ werkrollen

Voordat u Azure Monitor kunt u gegevens verzamelen in Log Analytics-werkruimte voor deze resources, moet Azure diagnostics zijn ingeschakeld.

Als diagnostische gegevens zijn ingeschakeld, kunt u de Azure-portal of PowerShell configureren van de werkruimte om de logboeken te verzamelen.

Azure Diagnostics is een Azure-extensie waarmee u diagnostische gegevens verzamelen uit een werkrol, Webrol of virtuele machine in Azure. De gegevens worden opgeslagen in Azure storage-account en vervolgens kan worden verzameld door Azure Monitor.

Voor Azure Monitor om deze Azure Diagnostics-logboeken te verzamelen, moet de logboeken op de volgende locaties:

| Logboektype | Resourcetype | Locatie |
| --- | --- | --- |
| IIS-logboeken |Virtuele machines <br> Webrollen <br> Werkrollen |wad-iis-logboekbestanden (Blob Storage) |
| Syslog |Virtuele machines |LinuxsyslogVer2v0 (Table Storage) |
| Operationele gebeurtenissen van de service Fabric |Service Fabric-knooppunten |WADServiceFabricSystemEventTable |
| Service Fabric Reliable Actor-gebeurtenissen |Service Fabric-knooppunten |WADServiceFabricReliableActorEventTable |
| Service Fabric Reliable Services-gebeurtenissen |Service Fabric-knooppunten |WADServiceFabricReliableServiceEventTable |
| Windows-gebeurtenislogboeken |Service Fabric-knooppunten <br> Virtuele machines <br> Webrollen <br> Werkrollen |WADWindowsEventLogsTable (Table Storage) |
| Windows ETW-Logboeken |Service Fabric-knooppunten <br> Virtuele machines <br> Webrollen <br> Werkrollen |WADETWEventTable (Table Storage) |

> [!NOTE]
> IIS-logboeken van Azure Websites worden momenteel niet ondersteund.
>
>

Voor virtuele machines, die u hebt de mogelijkheid van het installeren van de [Log Analytics-agent](../../azure-monitor/learn/quick-collect-azurevm.md) met uw virtuele machine om in te schakelen als u meer inzicht te krijgen. Naast het IIS-logboeken en gebeurtenislogboeken analyseren, kunt u aanvullende analyse, waaronder configuratie bijhouden, SQL-evaluatie en update-evaluatie uitvoeren.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Azure diagnostics in een virtuele machine inschakelen voor het logboek voor systeemgebeurtenissen en IIS het foutenlogboek van verzameling
Gebruik de volgende procedure om in te schakelen van Azure diagnostics in een virtuele machine voor de gebeurtenis- en IIS logboekverzameling met behulp van de Microsoft Azure-portal.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Om in te schakelen van Azure diagnostics in een virtuele machine met de Azure-portal
1. Installeer de VM-Agent bij het maken van een virtuele machine. Als de virtuele machine al bestaat, moet u controleren of de VM-Agent al is geïnstalleerd.

   * In Azure portal, gaat u naar de virtuele machine, selecteer **optionele configuratie**, klikt u vervolgens **Diagnostics** en stel **Status** naar **op** .

     De virtuele machine heeft na voltooiing, de Azure Diagnostics-extensie geïnstalleerd en wordt uitgevoerd. Deze uitbreiding is verantwoordelijk voor het verzamelen van diagnostische gegevens.
2. Schakel de bewaking en logboekregistratie in een bestaande virtuele machine configureren. U kunt diagnostische gegevens op het niveau van de virtuele machine inschakelen. Diagnostische gegevens inschakelen en vervolgens logboekregistratie configureren, moet u de volgende stappen uitvoeren:

   1. Selecteer de VM.
   2. Klik op **bewaking**.
   3. Klik op **Diagnostics**.
   4. Stel de **Status** naar **ON**.
   5. Selecteer elke logboekbestanden met diagnostische gegevens die u wenst te verzamelen.
   6. Klik op **OK**.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Azure diagnostics in een Webrol voor IIS-logboek- en gebeurtenisgegevens verzameling inschakelen
Raadpleeg [hoe om te schakelen diagnostische gegevens in een Cloudservice](../../cloud-services/cloud-services-dotnet-diagnostics.md) voor algemene stappen voor het inschakelen van Azure diagnostics. De onderstaande instructies deze informatie gebruiken en aanpassen voor gebruik met Log Analytics.

Met Azure diagnostics is ingeschakeld:

* IIS-logboeken worden standaard opgeslagen met logboekgegevens die zijn overgedragen op basis van de overdracht scheduledTransferPeriod interval.
* Windows-gebeurtenislogboeken worden standaard niet overgedragen.

### <a name="to-enable-diagnostics"></a>Diagnostische gegevens inschakelen
Om in te schakelen van Windows-gebeurtenislogboeken, of te wijzigen van de scheduledTransferPeriod, Azure Diagnostics configureren met de XML-configuratiebestand (diagnostics.wadcfg), zoals wordt weergegeven in [stap 4: Maken van het configuratiebestand van de diagnostische gegevens en de extensie installeren](../../cloud-services/cloud-services-dotnet-diagnostics.md)

Het volgende voorbeeld-configuratiebestand worden IIS-logboeken en alle gebeurtenissen van de toepassings- en systeemlogboeken verzameld:

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

Zorg ervoor dat uw ConfigurationSettings Hiermee geeft u een opslagaccount, zoals in het volgende voorbeeld:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

De **AccountName** en **AccountKey** waarden worden gevonden in de Azure portal in het dashboard van de storage-account onder toegangssleutels beheren. Het protocol voor de verbindingsreeks moet **https**.

Als de bijgewerkte diagnostische configuratie is toegepast in uw cloud-service en diagnostische gegevens naar Azure Storage schrijven, klikt u vervolgens bent u klaar om te configureren van de Log Analytics-werkruimte.

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>De Azure portal gebruiken voor het verzamelen van Logboeken van Azure Storage
De Azure-portal kunt u een Log Analytics-werkruimte configureren in Azure Monitor om de logboeken voor de volgende Azure-services te verzamelen:

* Service Fabric-clusters
* Virtuele machines
* Web-/ werkrollen

In de Azure-portal, gaat u naar uw Log Analytics-werkruimte en de volgende taken uitvoeren:

1. Klik op *logboeken voor opslagaccounts*
2. Klik op de *toevoegen* taak
3. Selecteer het opslagaccount waarin de logboeken met diagnostische gegevens
   * Dit account mag een klassieke storage-account of een Azure Resource Manager-opslagaccount
4. Selecteer het gegevenstype dat u wenst te verzamelen van Logboeken voor
   * De opties zijn IIS-logboeken; Gebeurtenissen. Syslog (Linux); ETW-Logboeken; Service Fabric-gebeurtenissen
5. De waarde voor de bron wordt automatisch ingevuld op basis van het gegevenstype en kan niet worden gewijzigd
6. Klik op OK om de configuratie op te slaan

Herhaal stappen 2 tot en met 6 voor extra opslagaccounts en gegevenstypen die u wilt verzamelen en onderbrengen in de werkruimte.

In ongeveer 30 minuten bent u gegevens uit het opslagaccount in de Log Analytics-werkruimte. U ziet alleen de gegevens die worden geschreven naar opslag nadat de configuratie is toegepast. De werkruimte heeft de vooraf bestaande gegevens niet lezen uit het opslagaccount.

> [!NOTE]
> De portal wordt niet gevalideerd of de bron in het opslagaccount bestaat of als nieuwe gegevens worden geschreven.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Azure diagnostics in een virtuele machine inschakelen voor het logboek voor systeemgebeurtenissen en IIS het foutenlogboek van verzameling met behulp van PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gebruik de stappen in [Azure Monitor configureren om te indexeren van Azure diagnostics](powershell-workspace-configuration.md#configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage) PowerShell gebruiken om te lezen van Azure diagnostics die zijn geschreven naar table storage.

U kunt meer precies een de gebeurtenissen die worden geschreven naar Azure Storage opgeven met behulp van Azure PowerShell.
Zie voor meer informatie, [inschakelen van diagnostische gegevens in Azure Virtual Machines](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

U kunt inschakelen en Azure diagnostics met behulp van de volgende PowerShell-script bijwerken.
U kunt dit script ook gebruiken met een aangepaste configuratie voor logboekregistratie.
Het script voor het instellen van de storage-account, de servicenaam en de naam van virtuele machine wijzigen.
Het script maakt gebruik van cmdlets voor klassieke virtuele machines.

Controleer het volgende voorbeeldscript, kopieert u het zo nodig wijzigen, het voorbeeld opslaan als een PowerShell-scriptbestand en voer het script.

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

    $wad_storage_account_key = (Get-AzStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Volgende stappen
* [Verzamelen van Logboeken en metrische gegevens voor Azure-services](collect-azure-metrics-logs.md) voor ondersteunde Azure-services.
* [Oplossingen inschakelen](../../azure-monitor/insights/solutions.md) om inzicht in de gegevens te bieden.
* [Gebruik van zoekquery's](../../azure-monitor/log-query/log-query-overview.md) om de gegevens te analyseren.
