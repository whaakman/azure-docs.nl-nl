---
title: Gastbesturingssysteem metrische gegevens verzenden naar de Azure Monitor-gegevensopslag voor een virtuele Windows-machine (klassiek)
description: Gastbesturingssysteem metrische gegevens verzenden naar de Azure Monitor-gegevensopslag voor een virtuele Windows-machine (klassiek)
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: ''
ms.openlocfilehash: 06b3d97f4b2b7867f09a8c4e5fe974615e9b0c70
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093417"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-data-store-for-a-windows-virtual-machine-classic"></a>Gastbesturingssysteem metrische gegevens verzenden naar de Azure Monitor-gegevensopslag voor een virtuele Windows-machine (klassiek)

De Azure Monitor [Diagnostics-extensie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) (ook wel 'WAD' of 'Diagnostische gegevens') kunt u voor het verzamelen van metrische gegevens en logboeken van het gastbesturingssysteem (Gastbesturingssysteem) uitgevoerd als onderdeel van een virtuele machine, een cloudservice of een Service Fabric cluster. De extensie kunt telemetrie wordt verzonden naar [veel verschillende locaties.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

In dit artikel beschrijft het proces voor het verzenden van Guest OS maatstaven voor prestaties voor een Windows virtuele machine (klassiek) naar de Azure Monitor metrische store. U kunt beginnen met diagnostische gegevens over versie 1.11, metrische gegevens schrijven rechtstreeks naar de Azure-Monitor metrische gegevens opslaan, waar al standaardplatform metrische gegevens worden verzameld. 

Op deze locatie opslaat, kunt u toegang tot dezelfde acties zoals u dat wel voor platform metrische gegevens doet. Acties omvatten bijna realtime waarschuwingen, grafieken, routering, toegang vanaf een REST-API, en meer. In het verleden heeft de extensie voor diagnostische gegevens geschreven naar Azure Storage, maar niet aan de gegevensopslag van Azure Monitor. 

Het proces dat wordt beschreven in dit artikel werkt alleen op klassieke virtuele machines die het Windows-besturingssysteem worden uitgevoerd.

## <a name="prerequisites"></a>Vereisten

- U moet een [servicebeheerder of CO-beheerder](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) op uw Azure-abonnement. 

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services#portal). 

- U moet beschikken over een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) of [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) geïnstalleerd.

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Een klassieke virtuele machine en een opslagaccount maken

1. Een klassieke virtuele machine maken met behulp van de Azure-portal.
   ![Klassiek-virtuele machine maken](./media/metrics-store-custom-guestos-classic-vm/create-classic-vm.png)

1. Wanneer u deze virtuele machine maakt, kiest u de optie voor het maken van een nieuwe klassieke storage-account. We gebruiken dit opslagaccount wordt gebruikt in latere stappen.

1. In de Azure-portal, gaat u naar de **Storage-accounts** resource-blade. Selecteer **sleutels**, en noteer de opslagaccountnaam en opslagaccountsleutel. U hebt deze informatie in latere stappen nodig.
   ![Toegangssleutels voor opslag](./media/metrics-store-custom-guestos-classic-vm/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Een service-principal maken

Een service-principal maken in uw Azure Active Directory-tenant met behulp van de instructies op de [maken van een service-principal](../azure-resource-manager/resource-group-create-service-principal-portal.md). Let op het volgende tijdens dit proces te doorlopen: 
- Maak nieuwe clientgeheim voor deze app.
- Sla de sleutel en de client-ID voor gebruik in latere stappen.

Geef deze app 'Bewaking van metrische gegevens Publisher'-machtigingen voor de resource die u wilt verzenden van metrische gegevens tegen. U kunt een resourcegroep of een hele abonnement gebruiken.  

> [!NOTE]
> De Diagnostics-extensie gebruikt de service-principal voor verificatie op basis van Azure Monitor en metrische gegevens verzenden voor uw klassieke virtuele machine.

## <a name="author-diagnostics-extension-configuration"></a>Configuratie van de auteur van diagnostische gegevens van de extensie

1. Bereid het configuratiebestand van de Diagnostics-extensie. Dit bestand bepaalt welke logboeken en prestatiemeteritems voor de extensie voor diagnostische gegevens voor uw klassieke virtuele machine verzamelen moet. Hieronder volgt een voorbeeld:

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
1. In de sectie 'SinksConfig' van het bestand met diagnostische gegevens, definieert u een nieuwe Azure Monitor-sink als volgt:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide the resource ID of your classic VM </ResourceId>
                <Region>The region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. In het gedeelte van het configuratiebestand waar de lijst met prestatiemeteritems moeten worden verzameld wordt vermeld, de prestatiemeteritems te routeren naar de Azure Monitor-sink 'AzMonSink'.

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. In de persoonlijke configuratie definieert u het account van Azure Monitor. Voeg vervolgens de gegevens voor de service-principal gebruiken om te verzenden van metrische gegevens toe.

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

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>De extensie voor diagnostische gegevens in uw cloud-service implementeren

1. Start PowerShell en meld u aan.

    ```powershell
    Login-AzureRmAccount
    ```

1. Beginnen met het instellen van de context voor uw klassieke virtuele machine.

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. Stel de context van de klassieke storage-account dat is gemaakt met de virtuele machine.

    ```powershell
    $StorageContext = New-AzureStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Het bestandspad van diagnostische gegevens naar een variabele instellen met behulp van de volgende opdracht uit:

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  De update voor uw klassieke virtuele machine voorbereiden door de diagnostics-bestand met de Azure Monitor-sink geconfigureerd.

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  De update voor uw virtuele machine implementeren door het uitvoeren van de volgende opdracht uit:

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> Het is nog steeds verplicht om een storage-account als onderdeel van de installatie van de extensie voor diagnostische gegevens. Alle logboeken of prestatiemeteritems die zijn opgegeven in het configuratiebestand van de diagnostische gegevens worden geschreven naar het opgegeven opslagaccount.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Tekenen van de metrische gegevens in Azure portal

1.  Ga naar Azure Portal. 

1.  Selecteer in het menu links **Monitor.**

1.  Op de **Monitor** Selecteer **metrische gegevens**.

    ![Navigeer metrische gegevens](./media/metrics-store-custom-guestos-classic-vm/navigate-metrics.png)

1. Selecteer in de vervolgkeuzelijst resources uw klassieke virtuele machine.

1. Selecteer in de vervolgkeuzelijst naamruimten **azure.vm.windows.guest**.

1. Selecteer in de vervolgkeuzelijst metrische gegevens **tussen geheugen\toegewezen Bytes in gebruik**.
   ![Metrische gegevens tekenen](./media/metrics-store-custom-guestos-classic-vm/plot-metrics.png)


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).
