---
title: Gast OS metrische gegevens verzenden naar de Azure Monitor-gegevensopslag voor een Windows virtuele Machine (klassiek)
description: Gast OS metrische gegevens verzenden naar de Azure Monitor-gegevensopslag voor een Windows virtuele Machine (klassiek)
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: ''
ms.openlocfilehash: 235eda231dfb0f936bf55c7c8d93a8f709fdf9bc
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954837"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-data-store-for-a-windows-virtual-machine-classic"></a>Gast OS metrische gegevens verzenden naar de Azure Monitor-gegevensopslag voor een Windows virtuele Machine (klassiek)

De Azure Monitor [Windows Azure Diagnostics-extensie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) (WAD), kunt u voor het verzamelen van metrische gegevens en logboeken van de Gast-besturingssysteem (als gast-OS) die wordt uitgevoerd als onderdeel van een virtuele Machine, Cloudservice of Service Fabric-cluster. De extensie kunt telemetrie verzenden naar verschillende locaties die worden vermeld in de eerder gekoppelde artikel.

Dit artikel beschrijft het proces voor het verzenden Gast OS-maatstaven voor prestaties voor een Windows virtuele Machine (klassiek) naar de Azure Monitor metrische store. U kunt beginnen met WAD versie 1.11, metrische gegevens schrijven rechtstreeks naar de Azure Monitor metrics store waar al standaardplatform metrische gegevens worden verzameld. Op deze locatie opslaat, kunt u toegang tot de dezelfde acties die beschikbaar zijn voor platform metrische gegevens.  Acties omvatten bijna realtime waarschuwingen, grafieken, routering, toegang tot van REST-API en nog veel meer.  In het verleden heeft de extensie WAD geschreven naar Azure Storage, maar niet de gegevensopslag voor Azure Monitor. 

De procedure die wordt beschreven in dit artikel werkt alleen in klassieke virtuele Machines met het Windows-besturingssysteem.

## <a name="pre-requisites"></a>Vereisten

- U moet een [servicebeheerder of CO-beheerder](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) op uw Azure-abonnement 

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- U moet beschikken over een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) geïnstalleerd, of kunt u [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Een klassieke virtuele Machine en een Opslagaccount maken

1. Een klassieke virtuele machine maken met de Azure-portal ![klassiek VM maken](./media/metrics-store-custom-guestos-classic-vm/create-classic-vm.png)

1. Wanneer deze virtuele machine maakt, kunt u kiezen om een nieuwe klassieke opslagaccount te maken. We gebruiken dit opslagaccount wordt gebruikt in latere stappen.

1. In de Azure-portal, gaat u naar de resourceblade van de Storage-Account en kies de **sleutels** en noteert u de opslagaccountnaam en opslagaccountsleutel. U moet deze sleutels in latere stappen ![toegangssleutels voor opslag](./media/metrics-store-custom-guestos-classic-vm/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Een service-principal maken

Een service-principal maken in uw Azure Active Directory-tenant met behulp van de instructies op [maken van een service-principal](../active-directory/develop/howto-create-service-principal-portal.md). Let op het volgende tijdens dit proces te doorlopen: 
- Nieuwe clientgeheim voor deze app maken  
- Sla de sleutel en de client-id voor gebruik in latere stappen.

Geef deze app 'Bewaking van metrische gegevens Publisher'-machtigingen voor de resource die u wilt verzenden van metrische gegevens tegen. U kunt een resourcegroep of een volledig abonnement.  

> [!NOTE]
> De extensie voor diagnostische gegevens wordt de service-principal gebruiken om te verifiëren op basis van Azure Monitor en metrische gegevens verzenden voor uw klassieke virtuele machine.

## <a name="author-diagnostics-extension-configuration"></a>Configuratie van de auteur van diagnostische gegevens van de extensie

1. Bereid uw configuratiebestand WAD diagnostics-extensie. Dit bestand bepaalt welke logboeken en prestatiemeteritems voor de extensie voor diagnostische gegevens voor uw klassieke virtuele machine verzamelen moet. Hieronder staat een voorbeeld.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
            <Directories scheduledTransferPeriod="PT1M">
                <IISLogs containerName="wad-iis-logfiles" />
                <FailedRequestLogs containerName="wad-failedrequestlogs" />
            </Directories>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
                <PerformanceCounterConfiguration counterSpecifier="\Processor(*)\% Processor Time" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(*)\Disk Read Bytes/sec" sampleRate="PT15S" />
            </PerformanceCounters>
            <WindowsEventLog scheduledTransferPeriod="PT1M">
                <DataSource name="Application!*[System[(Level=1 or Level=2 or Level=3)]]" />
                <DataSource name="Windows Azure!*[System[(Level=1 or Level=2 or Level=3 or Level=4)]]" />
            </WindowsEventLog>
            <CrashDumps>
                <CrashDumpConfiguration processName="WaIISHost.exe" />
                <CrashDumpConfiguration processName="WaWorkerHost.exe" />
                <CrashDumpConfiguration processName="w3wp.exe" />
            </CrashDumps>
            <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Error" />
            <Metrics resourceId="/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicCompute/virtualMachines/MyClassicVM">
                <MetricAggregation scheduledTransferPeriod="PT1M" />
                <MetricAggregation scheduledTransferPeriod="PT1H" />
            </Metrics>
        </DiagnosticMonitorConfiguration>
        <SinksConfig>
        </SinksConfig>
        </WadCfg>
        <StorageAccount />
    </PublicConfig>
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <StorageAccount name="" endpoint="" />
    </PrivateConfig>
    <IsEnabled>true</IsEnabled>
    </DiagnosticsConfiguration>
    ```
1. Definieer een nieuwe Azure Monitor-sink in de sectie 'SinksConfig' van het bestand met diagnostische gegevens:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide your Classic VM’s Resource ID </ResourceId>
                <Region>Region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. In het gedeelte van het configuratiebestand waar de lijst met prestatiemeteritems moeten worden verzameld wordt vermeld, de prestatiemeteritems te routeren naar de Azure Monitor-Sink 'AzMonSink'.

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. In de persoonlijke configuratie definiëren van het account van Azure Monitor en de gegevens voor de service-principal gebruiken om te verzenden van metrische gegevens toevoegen.

    ```xml
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" endpoint="" />
        <AzureMonitorAccount>
            <ServicePrincipalMeta>
                <PrincipalId>clientId for your service principal</PrincipalId>
                <Secret>client secret of your service principal</Secret>
            </ServicePrincipalMeta>
        </AzureMonitorAccount>
    </PrivateConfig>
    ```

1. Sla dit bestand lokaal op.

## <a name="deploy-diagnostics-extension-to-your-cloud-service"></a>Extensie voor diagnostische gegevens in uw Cloud-Service implementeren

1. Start PowerShell en meld u aan

    ```powershell
    Login-AzureRmAccount
    ```

1. Beginnen met het instellen van de context met uw klassieke virtuele machine

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. Stel de context van de klassieke storage-account dat is gemaakt met de virtuele machine.

    ```powershell
    $StorageContext = New-AzureStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Instellen van het bestandspad van diagnostische gegevens naar een variabele met de onderstaande opdracht.

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  Voorbereiden van de update naar uw klassieke virtuele machine met de diagnostics-bestand met de Azure Monitor-sink geconfigureerd

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  De update voor uw virtuele machine implementeren door het uitvoeren van de onderstaande opdracht

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> Het is nog steeds verplicht om een Storage-Account als onderdeel van de installatie van de extensie voor diagnostische gegevens. Alle logboeken en/of de prestatiemeteritems die zijn opgegeven in het configuratiebestand van de diagnostische gegevens worden geschreven naar het opgegeven opslagaccount.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Tekenen van de metrische gegevens in Azure portal

1.  Navigeer naar de Azure-portal

1.  Klik in het linkermenu op op de Monitor

1.  Klik op de blade Monitor op de **metrische gegevens**
   ![gaat u metrische gegevens](./media/metrics-store-custom-guestos-classic-vm/navigate-metrics.png)

1. Selecteer uw klassieke virtuele machine in de vervolgkeuzelijst-resource

1. Selecteer in de vervolgkeuzelijst-naamruimten **azure.vm.windows.guest**

1. In de metrische gegevens vervolgkeuzelijst, selecteer **tussen geheugen\toegewezen Bytes in gebruik**
   ![metrische gegevens tekenen](./media/metrics-store-custom-guestos-classic-vm/plot-metrics.png)


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).
