---
title: Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek klassieke Service in de Cloud opslaan
description: Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek klassieke Service in de Cloud opslaan
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: be27ff3f8dda3209a011c3ad79d1a7a1f1d259fe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986911"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-classic-cloud-service"></a>Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek klassieke Service in de Cloud opslaan

De Azure Monitor [Windows Azure Diagnostics-extensie](azure-diagnostics.md) (WAD), kunt u voor het verzamelen van metrische gegevens en logboeken van de Gast-besturingssysteem (guestOS) uitgevoerd als onderdeel van een virtuele Machine, Cloudservice of Service Fabric-cluster.  De extensie kunt telemetrie verzenden naar verschillende locaties die worden vermeld in de eerder gekoppelde artikel.  

Dit artikel beschrijft het proces voor het verzenden Gast OS-maatstaven voor prestaties voor Azure classic Cloud-Services naar de Azure Monitor metrische store. U kunt beginnen met WAD versie 1.11, metrische gegevens schrijven rechtstreeks naar de Azure Monitor metrics store waar al standaardplatform metrische gegevens worden verzameld. Op deze locatie opslaat, kunt u toegang tot de dezelfde acties die beschikbaar zijn voor platform metrische gegevens.  Acties omvatten bijna realtime waarschuwingen, grafieken, routering, toegang tot van REST-API en nog veel meer.  In het verleden heeft de extensie WAD geschreven naar Azure Storage, maar niet de gegevensopslag voor Azure Monitor.  

De procedure die wordt beschreven in dit artikel werkt alleen voor prestatiemeteritems op Azure Cloud Services. Het werkt niet voor andere aangepaste metrische gegevens. 
   

## <a name="pre-requisites"></a>Vereisten

- U moet een [servicebeheerder of CO-beheerder](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) op uw Azure-abonnement 

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- U moet beschikken over een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) geïnstalleerd, of kunt u [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 


## <a name="provision-cloud-service-and-storage-account"></a>Cloudservice en Storage-Account inrichten 

1. Maken en implementeren van een klassieke Cloudservice (een voorbeeldtoepassing van het klassieke cloud-service en de implementatie vindt [hier](../cloud-services/cloud-services-dotnet-get-started.md). 

2. U kunt een bestaand opslagaccount gebruiken of een nieuw opslagaccount te implementeren. Het is raadzaam als het opslagaccount zich in dezelfde regio bevinden als de klassieke Cloudservice die u zojuist hebt gemaakt. In de Azure-portal, gaat u naar de resourceblade van de Storage-Account en kies de **sleutels**. Noteer de naam en de storage-account toegangssleutel van het opslagaccount, moet u deze in latere stappen.

   ![Opslagaccountsleutels](./media/metrics-store-custom-guestos-classic-cloud-service/storage-keys.png)



## <a name="create-a-service-principal"></a>Een Service-Principal maken 

Een service-principal maken in uw Azure Active Directory-tenant met behulp van de instructies op... / azure/azure-resource-manager/resource-group-create-service-principal-portal. Let op het volgende tijdens dit proces te doorlopen: 
  - U kunt opnemen in een URL voor de aanmeldings-URL.  
  - Nieuwe clientgeheim voor deze app maken  
  - Sla de sleutel en de client-id voor gebruik in latere stappen.  

Geef de app in de vorige stap hebt gemaakt *bewaking metrische gegevens Publisher* machtigingen voor de resource die u wilt verzenden van metrische gegevens tegen. Als u van plan bent de app om te verzenden van aangepaste metrische gegevens op veel resources gebruiken, kunt u deze machtigingen op het niveau van de resource-groep of abonnement verlenen.  

> [!NOTE]
> De Diagnostics-extensie de service-principal gebruikt voor verificatie op basis van Azure Monitor en verzenden van metrische gegevens voor je cloudservice 

## <a name="author-diagnostics-extension-configuration"></a>Configuratie van de auteur van diagnostische gegevens van de extensie 

Bereid uw configuratiebestand WAD diagnostics-extensie. Dit bestand bepaalt welke logboeken en prestatiemeteritems voor de extensie voor diagnostische gegevens voor je cloudservice dient te verzamelen. Hieronder vindt u een voorbeeldbestand voor de configuratie van diagnostische gegevens.  

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

In de sectie van het configuratiebestand waar u lijst van de prestaties van items moeten worden verzameld, toevoegen de Sink-Azure Monitor. Deze vermelding zorgt ervoor dat alle prestatiemeteritems die zijn opgegeven, worden doorgestuurd naar Azure Monitor als metrische gegevens. U kunt prestatiemeteritems volgens uw behoeften toevoegen/verwijderen. 

```XML
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink"> 
 <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
  … 
</PerformanceCounters> 
```
Ten slotte in de persoonlijke configuratie voegt een *Azure Monitor-Account* sectie. Voer in het service-principalclient-ID en het geheim die zijn gemaakt in de vorige stap. 

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

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>De extensie voor diagnostische gegevens in uw Cloud-Service implementeren 

Start PowerShell en meld u aan bij Azure 

```PowerShell
Login-AzureRmAccount 
```

De Details over de details van de Storage-Account hebt gemaakt in een eerdere stap in de variabelen met de volgende opdrachten Store. 

```PowerShell
$storage_account = <name of your storage account from step 3> 
$storage_keys = <storage account key from step 3> 
```
 
Ingesteld op dezelfde manier het bestandspad van diagnostische gegevens naar een variabele met de onderstaande opdracht. 

```PowerShell
$diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>” 
```
 
De extensie voor diagnostische gegevens in uw cloud-service met de diagnostics-bestand met de Azure Monitor-sink die is geconfigureerd met behulp van de onderstaande opdracht implementeren 

```PowerShell
Set-AzureServiceDiagnosticsExtension -ServiceName <classicCloudServiceName> -StorageAccountName $storage_account -StorageAccountKey $storage_keys -DiagnosticsConfigurationPath $diagconfig 
```
 
> [!NOTE] 
> Het is nog steeds verplicht om een Storage-Account als onderdeel van de installatie van de extensie voor diagnostische gegevens. Alle logboeken en/of de prestatiemeteritems die zijn opgegeven in het configuratiebestand van de diagnostische gegevens worden geschreven naar het opgegeven opslagaccount.  

## <a name="plot-metrics-in-the-azure-portal"></a>Tekenen van metrische gegevens in Azure portal 

Navigeer naar de Azure-portal 

 ![Metrische gegevens over Azure-portal](./media/metrics-store-custom-guestos-classic-cloud-service/navigate-metrics.png)

1. Klik in het menu links op Monitor 

1. Klik op de blade Monitor op het tabblad Voorbeeld van metrische gegevens 

1. Selecteer in de vervolgkeuzelijst resource uw klassieke Cloudservice 

1. Selecteer in de vervolgkeuzelijst naamruimten **azure.vm.windows.guest** 

1. In de metrische gegevens vervolgkeuzelijst, selecteer *tussen geheugen\toegewezen Bytes in gebruik* 

U kunt kiezen om de totale hoeveelheid geheugen die wordt gebruikt door een bepaalde rol heeft en elk rolexemplaar met behulp van de dimensie filteren en mogelijkheden splitsen weer te geven. 

 ![Metrische gegevens over Azure-portal](./media/metrics-store-custom-guestos-classic-cloud-service/metrics-graph.png)

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).



