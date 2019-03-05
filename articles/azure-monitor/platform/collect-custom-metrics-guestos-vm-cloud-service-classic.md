---
title: Verzenden van Guest OS metrische gegevens in de Azure Monitor-metriek opslaan klassieke Cloud Services
description: Verzenden van Guest OS metrische gegevens in de Azure Monitor-metriek opslaan van Cloud Services
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: dba1f66be91d8fa8e151a2771bad70b721af02dc
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57313069"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-classic-cloud-services"></a>Verzenden van Guest OS metrische gegevens in de Azure Monitor-metriek opslaan klassieke Cloud Services 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Met de Azure Monitor [Diagnostics-extensie](diagnostics-extension-overview.md), kunt u metrische gegevens en logboeken van het gastbesturingssysteem (Gastbesturingssysteem) uitgevoerd als onderdeel van een virtuele machine, een cloudservice of een Service Fabric-cluster verzamelen. De extensie kunt telemetrie wordt verzonden naar [veel verschillende locaties.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

In dit artikel beschrijft het proces voor het verzenden van Guest OS metrische gegevens voor prestaties voor Azure Cloud Services van klassieke naar het archief van Azure Monitor-metrische gegevens. U kunt beginnen met diagnostische gegevens over versie 1.11, metrische gegevens schrijven rechtstreeks naar de Azure-Monitor metrische gegevens opslaan, waar al standaardplatform metrische gegevens worden verzameld. 

Op deze locatie opslaat, kunt u toegang tot de dezelfde acties die u voor platform metrische gegevens kunt. Acties omvatten bijna realtime waarschuwingen, grafieken, routering, toegang vanaf een REST-API, en meer.  In het verleden heeft de extensie voor diagnostische gegevens geschreven naar Azure Storage, maar niet aan de gegevensopslag van Azure Monitor.  

Het proces dat wordt beschreven in dit artikel geldt alleen voor prestatiemeteritems in Azure Cloud Services. Het werkt niet voor andere aangepaste metrische gegevens. 

## <a name="prerequisites"></a>Vereisten

- U moet een [servicebeheerder of CO-beheerder](~/articles/billing/billing-add-change-azure-subscription-administrator.md) op uw Azure-abonnement. 

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services). 

- U moet beschikken over een [Azure PowerShell](/powershell/azure) of [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) geïnstalleerd.

## <a name="provision-a-cloud-service-and-storage-account"></a>Inrichten van een cloud service- en storage-account 

1. Maken en implementeren van een klassieke cloudservice. Een voorbeeld van klassieke Cloud Services-toepassing en de implementatie kunnen worden gevonden op [aan de slag met Azure Cloud Services en ASP.NET](../../cloud-services/cloud-services-dotnet-get-started.md). 

2. U kunt een bestaand opslagaccount gebruiken of een nieuw opslagaccount te implementeren. Het is raadzaam als het opslagaccount zich in dezelfde regio als de klassieke cloudservice die u hebt gemaakt. In de Azure-portal, gaat u naar de **opslagaccounts** resourceblade, en selecteer vervolgens **sleutels**. Let op de naam van het opslagaccount en de opslagaccountsleutel. U moet deze informatie in latere stappen.

   ![Opslagaccountsleutels](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/storage-keys.png)

## <a name="create-a-service-principal"></a>Een service-principal maken 

Een service-principal maken in uw Azure Active Directory-tenant met behulp van de instructies op de [portal gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Let op het volgende terwijl u door dit proces gaat: 

- U kunt opnemen in een URL voor de URL van de aanmelding.  
- Maak nieuwe clientgeheim voor deze app.  
- Sla de sleutel en de client-ID voor gebruik in latere stappen.  

Geef de app in de vorige stap hebt gemaakt *bewaking metrische gegevens Publisher* machtigingen voor de resource die u wilt verzenden van metrische gegevens tegen. Als u van plan bent de app om te verzenden van aangepaste metrische gegevens op veel resources gebruiken, kunt u deze machtigingen op het niveau van de resource-groep of abonnement verlenen.  

> [!NOTE]
> De extensie voor diagnostische gegevens maakt gebruik van de service-principal voor verificatie op basis van Azure Monitor en metrische gegevens voor uw cloud-service verzenden.

## <a name="author-diagnostics-extension-configuration"></a>Configuratie van de auteur van diagnostische gegevens van de extensie 

Bereid het configuratiebestand van de Diagnostics-extensie. Dit bestand bepaalt welke logboeken en prestatiemeteritems voor de extensie voor diagnostische gegevens voor je cloudservice dient te verzamelen. Hieronder vindt u een voorbeeldbestand voor de configuratie van diagnostische gegevens:  

```XML
<?xml version="1.0" encoding="utf-8"?> 
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
    <WadCfg> 
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096"> 
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" /> 
        <Directories scheduledTransferPeriod="PT1M"> 
          <IISLogs containerName="wad-iis-logfiles" /> 
          <FailedRequestLogs containerName="wad-failedrequestlogs" /> 
        </Directories> 
        <PerformanceCounters scheduledTransferPeriod="PT1M"> 
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Page Faults/sec" sampleRate="PT15S" /> 
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

Definieer een nieuwe Azure Monitor-sink in de sectie 'SinksConfig' van het bestand met diagnostische gegevens: 

```XML
  <SinksConfig> 
    <Sink name="AzMonSink"> 
    <AzureMonitor> 
      <ResourceId>-Provide ClassicCloudService’s Resource ID-</ResourceId> 
      <Region>-Azure Region your Cloud Service is deployed in-</Region> 
    </AzureMonitor> 
    </Sink> 
  </SinksConfig> 
```

In het gedeelte van het configuratiebestand waar u de prestatiemeteritems voor het verzamelen van weergeven, toevoegen de Azure Monitor-sink. Deze vermelding zorgt ervoor dat alle prestatiemeteritems die u hebt opgegeven, worden gerouteerd naar Azure Monitor als metrische gegevens. U kunt toevoegen of verwijderen van prestatiemeteritems op basis van uw behoeften. 

```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
```

Ten slotte in de persoonlijke configuratie voegt een *Azure Monitor-Account* sectie. Voer de service-principalclient-ID en geheim dat u eerder hebt gemaakt. 

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <StorageAccount name="" endpoint="" /> 
    <AzureMonitorAccount> 
      <ServicePrincipalMeta> 
        <PrincipalId>clientId from step 3</PrincipalId> 
        <Secret>client secret from step 3</Secret> 
      </ServicePrincipalMeta> 
    </AzureMonitorAccount> 
</PrivateConfig> 
```

Sla dit bestand lokaal op diagnostische gegevens.  

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>De extensie voor diagnostische gegevens in uw cloud-service implementeren 

Start PowerShell en meld u aan bij Azure. 

```PowerShell
Login-AzAccount 
```

Gebruik de volgende opdrachten voor het opslaan van de details van het opslagaccount dat u eerder hebt gemaakt. 

```PowerShell
$storage_account = <name of your storage account from step 3> 
$storage_keys = <storage account key from step 3> 
```

Het bestandspad van diagnostische gegevens aan een variabele op dezelfde manier ingesteld met behulp van de volgende opdracht uit:

```PowerShell
$diagconfig = “<path of the Diagnostics configuration file with the Azure Monitor sink configured>” 
```

De extensie voor diagnostische gegevens implementeren in uw cloud-service met de diagnostics-bestand met de Azure Monitor-sink die is geconfigureerd met behulp van de volgende opdracht uit:  

```PowerShell
Set-AzureServiceDiagnosticsExtension -ServiceName <classicCloudServiceName> -StorageAccountName $storage_account -StorageAccountKey $storage_keys -DiagnosticsConfigurationPath $diagconfig 
```

> [!NOTE] 
> Het is nog steeds verplicht om een storage-account als onderdeel van de installatie van de extensie voor diagnostische gegevens. Alle logboeken of prestatiemeteritems die zijn opgegeven in het configuratiebestand van de diagnostische gegevens worden geschreven naar het opgegeven opslagaccount.  

## <a name="plot-metrics-in-the-azure-portal"></a>Tekenen van metrische gegevens in Azure portal 

1. Ga naar Azure Portal. 

   ![Metrische gegevens over Azure-portal](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/navigate-metrics.png)

2. Selecteer in het menu links **Monitor.**

3. Op de **Monitor** blade, selecteer de **metrische gegevens Preview** tabblad.

4. Selecteer in de vervolgkeuzelijst resources uw klassieke cloudservice.

5. Selecteer in de vervolgkeuzelijst naamruimten **azure.vm.windows.guest**. 

6. Selecteer in de vervolgkeuzelijst metrische gegevens **tussen geheugen\toegewezen Bytes in gebruik**. 

U de dimensie, te filteren en splitsen mogelijkheden gebruiken om de totale hoeveelheid geheugen die wordt gebruikt door een specifieke rol of de rolinstantie weer te geven. 

 ![Metrische gegevens over Azure-portal](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/metrics-graph.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).

