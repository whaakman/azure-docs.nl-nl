---
title: Diagnostics-extensie configuratie schema versiegeschiedenis van Azure
description: Relevant zijn voor het verzamelen van prestatiemeteritems in Azure Virtual Machines, VM Scale Sets, Service Fabric en Cloud Services.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/20/2018
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: 42ed050606dd242e43903ed980a5670b4aa6f3df
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343170"
---
# <a name="azure-diagnostics-extension-configuration-schema-versions-and-history"></a>Azure Diagnostics-extensie configuration schema-versies en de geschiedenis
Deze pagina indexen Azure Diagnostics-extensie schema versies geleverd als onderdeel van de Microsoft Azure SDK.  

> [!NOTE]
> De Azure Diagnostics-extensie is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken met:
> - Azure Virtual Machines
> - Virtual Machine Scale Sets
> - Service Fabric
> - Cloud Services
> - Netwerkbeveiligingsgroepen
>
> Deze pagina is alleen van toepassing als u een van deze services.

De Azure Diagnostics-extensie wordt gebruikt met andere Microsoft-producten voor diagnostische gegevens, zoals Azure Monitor, Application Insights en Log Analytics. Zie voor meer informatie, [hulpprogramma's van Microsoft Bewakingsoverzicht](../../azure-monitor/overview.md).

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Azure SDK en diagnostische gegevens over versies verzending van grafiek  

|Azure SDK-versie | De versie van de Diagnostics-extensie | Model|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |invoegtoepassing|  
|2.0 - 2.4         |1.0                            |invoegtoepassing|  
|2,5               |1.2                            |Extensie|  
|2.6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1.5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1.8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|
|2.96              |1.11                           |"|


 Azure Diagnostics-versie 1.0 eerst in een invoegtoepassingsmodel, wat betekent dat u de versie van Azure diagnostics mogelijk tijdens de installatie van de Azure SDK, is verzonden met het verzonden.  

 Beginnen met de SDK 2.5 (diagnostics versie 1.2), Azure diagnostics is een fout met een extensiemodel. De hulpprogramma's gebruik maken van nieuwe functies zijn alleen beschikbaar in de nieuwere Azure-SDK's, maar elke met behulp van Azure diagnostics-service de meest recente versie van de verzending rechtstreeks vanuit Azure wilt ophalen. Bijvoorbeeld, Iedereen nog steeds met behulp van SDK 2.5 zou worden het laden van de meest recente versie die wordt weergegeven in de voorgaande tabel, ongeacht als ze de nieuwere functies gebruiken.  

## <a name="schemas-index"></a>Index van de schema 's  
Verschillende versies van Azure diagnostics gebruiken verschillende configuratieschema.

[Configuratieschema van Diagnostics 1.0](diagnostics-extension-schema-1dot0.md)  

[Configuratieschema van Diagnostics 1.2](diagnostics-extension-schema-1dot2.md)  

[Diagnostics 1.3 en hoger configuratieschema](diagnostics-extension-schema-1dot3.md)  

## <a name="version-history"></a>Versiegeschiedenis

### <a name="diagnostics-extension-111"></a>Extensie voor diagnostische gegevens 1.11
Ondersteuning toegevoegd voor de Azure Monitor-sink. Dit is alleen van toepassing op prestatiemeteritems. Hiermee schakelt u prestatiemeteritems die worden verzameld op de virtuele machine, VMSS of service in de cloud naar Azure Monitor als aangepaste metrische gegevens verzenden. Biedt ondersteuning voor de Azure Monitor-sink:
* Bij het ophalen van alle prestatiemeteritems verzonden naar Azure Monitor via de [Azure Monitor metrics API's.](https://docs.microsoft.com/rest/api/monitor/metrics/list)
* Waarschuwingen voor alle prestatiemeteritems verzonden naar Azure Monitor via de nieuwe [ervaring voor waarschuwingen van geïntegreerde](../../azure-monitor/platform/alerts-overview.md) in Azure Monitor
* Wildcard-operator als de dimensie 'Instantie' op uw metrische gegevens in de prestatiemeteritems te behandelen. Bijvoorbeeld als u hebt verzameld de ' logische schijf (\*) / DiskWrites per seconde "teller die u kunt filteren en splitsen op de dimensie 'Instantie' diagram of de waarschuwing op de schijf schrijven per seconde voor elke logische schijf (C:, D:, enz.)

Azure Monitor definiëren als een nieuwe sink in de configuratie van de extensie voor diagnostische gegevens
```json
"SinksConfig": {
    "Sink": [
        {
            "name": "AzureMonitorSink",
            "AzureMonitor": {}
        },
    ]
}
```

```XML
<SinksConfig>  
  <Sink name="AzureMonitorSink">
      <AzureMonitor/>
  </Sink>
</SinksConfig>
```
> [!NOTE]
> Configureren van de Azure Monitor-sink voor klassieke virtuele machines en klassieke Cloudservice vereist meer parameters worden gedefinieerd in de Diagnostics-extensie persoonlijke configuratie.
>
> Meer informatie gaat u naslaginformatie over de [gedetailleerde diagnostische extensie schemadocumentatie.](diagnostics-extension-schema-1dot3.md)

Vervolgens configureert u uw prestatiemeteritems worden doorgestuurd naar de Azure Monitor-Sink.
```json
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "AzureMonitorSink",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT1M",
            "unit": "percent"
        }
    ]
},
```
```XML
<PerformanceCounters scheduledTransferPeriod="PT1M", sinks="AzureMonitorSink">  
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
</PerformanceCounters>
```

### <a name="diagnostics-extension-19"></a>Extensie voor diagnostische gegevens 1.9
Docker-ondersteuning is toegevoegd.


### <a name="diagnostics-extension-181"></a>Extensie voor diagnostische gegevens 1.8.1
Kan een SAS-token in plaats van een opslagaccountsleutel opgeven in de persoonlijke configuratie. Als een SAS-token is opgegeven, wordt de opslagaccountsleutel genegeerd.


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a>Extensie voor diagnostische gegevens 1.8
Toegevoegde opslagtype u moet PublicConfig. StorageType mag *tabel*, *Blob*, *TableAndBlob*. *Tabel* is de standaardinstelling.


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a>Extensie voor diagnostische gegevens 1.7
De mogelijkheid om te routeren naar EventHub toegevoegd.

### <a name="diagnostics-extension-15"></a>1.5-extensie voor diagnostische gegevens
De PUT-element en de mogelijkheid voor het verzenden van diagnostische gegevens toegevoegd [Application Insights](../../application-insights/app-insights-cloudservices.md) waardoor het gemakkelijker wordt om problemen te diagnosticeren op uw toepassing als het niveau van de systeem- en -infrastructuur.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Azure SDK 2.6 en diagnostische gegevens van de extensie 1.3
Cloud Service-projecten in Visual Studio, zijn de volgende wijzigingen aangebracht. (Deze wijzigingen gelden ook voor latere versies van Azure SDK.)

* De lokale emulator biedt nu ondersteuning voor diagnostische gegevens. Deze wijziging betekent dat u kunt verzamelen van diagnostische gegevens en zorg ervoor dat uw toepassing maakt de juiste traceringen terwijl u ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` schakelt de gegevensverzameling van diagnostische gegevens terwijl u uw cloudserviceproject in Visual Studio voert met behulp van de Azure-opslagemulator. Alle diagnostische gegevens worden verzameld in het opslagaccount (opslag).
* De diagnostische gegevens van storage account connection string (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) wordt opnieuw opgeslagen in het serviceconfiguratiebestand (.cscfg). In de Azure SDK 2.5 is het opslagaccount voor diagnostische gegevens in het bestand diagnostics.wadcfgx opgegeven.

Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in de Azure SDK 2.4 en eerder heeft gewerkt en hoe het werkt in de Azure SDK 2.6 en hoger.

* In de Azure SDK 2.4 en eerdere versies, is de verbindingsreeks gebruikt tijdens runtime door de invoegtoepassing diagnostische gegevens om op te halen van de gegevens over het opslagaccount voor het overbrengen van logboeken met diagnostische gegevens.
* In de Azure SDK 2.6 en hoger, wordt in Visual Studio de verbindingsreeks van diagnostische gegevens over het configureren van de extensie voor diagnostische gegevens met de gegevens van het account de juiste opslag tijdens het publiceren gebruikt. De verbindingsreeks kunt u verschillende opslagaccounts voor verschillende configuraties die Visual Studio wordt gebruikt bij het publiceren van definiëren. Omdat de diagnostics-invoegtoepassing (nadat de Azure SDK 2.5) niet langer beschikbaar is, inschakelen niet het cscfg-bestand zelf echter de extensie voor diagnostische gegevens. U moet de extensie afzonderlijk via hulpprogramma's zoals Visual Studio of PowerShell inschakelen.
* Ter vereenvoudiging van het proces van het configureren van de extensie voor diagnostische gegevens met PowerShell, bevat de uitvoer van het pakket van Visual Studio ook de openbare configuratie-XML voor de extensie voor diagnostische gegevens voor elke rol. Visual Studio gebruikt de verbindingsreeks van diagnostische gegevens voor het vullen van de gegevens over het opslagaccount aanwezig zijn in de openbare configuratie. De openbare configuratiebestanden zijn gemaakt in de map-extensies en volgen het patroon PaaSDiagnostics. <RoleName>. PubConfig.xml. Alle implementaties op basis van PowerShell, kunnen dit patroon gebruiken voor elke configuratie worden toegewezen aan een rol.
* De verbindingsreeks in het .cscfg-bestand wordt ook gebruikt door de Azure-portal voor toegang tot de diagnostische gegevens zodat deze kan worden weergegeven de **bewaking** tabblad. De verbindingsreeks is vereist voor de service configureren voor uitgebreide bewaking gegevens weergeven in de portal.

#### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migreren projecten op Azure SDK 2.6 en hoger
Bij het migreren van Azure SDK 2.5 naar Azure SDK 2.6 of later, als u een opslagaccount voor diagnostische gegevens opgegeven in het bestand .wadcfgx had, klikt u vervolgens blijft er. Als u wilt profiteren van de flexibiliteit van het gebruik van verschillende opslagaccounts voor verschillende opslagconfiguraties, hebt u de verbindingsreeks handmatig toevoegen aan uw project. Als u een project waarnaar u migreert van Azure SDK 2.4 of eerder naar de Azure SDK 2.6, blijven klikt u vervolgens de verbindingsreeksen voor diagnostische gegevens behouden. Houd echter rekening met de wijzigingen in hoe verbindingsreeksen worden behandeld in de Azure SDK 2.6 zoals opgegeven in de vorige sectie.

#### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hoe Visual Studio Hiermee bepaalt u het opslagaccount voor diagnostische gegevens
* Als een verbindingsreeks diagnostische gegevens in het .cscfg-bestand is opgegeven, wordt het in Visual Studio gebruikt voor het configureren van de extensie voor diagnostische gegevens bij het publiceren en bij het genereren van de openbare configuratie-xml-bestanden tijdens het verpakken.
* Als er is geen verbindingsreeks diagnostische gegevens in het .cscfg-bestand is opgegeven, klikt u vervolgens terugvalt Visual Studio op het storage-account dat is opgegeven in het bestand .wadcfgx met het configureren van de extensie voor diagnostische gegevens bij het publiceren van en het genereren van de openbare configuratie-xml-bestanden bij het verpakken.
* De verbindingsreeks voor de diagnostische gegevens in het .cscfg-bestand heeft voorrang op de storage-account in het bestand .wadcfgx. Als een verbindingsreeks diagnostische gegevens in het .cscfg-bestand is opgegeven, Visual Studio gebruikt en het opslagaccount in .wadcfgx worden genegeerd.

#### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Wat is het selectievakje 'Bijwerken ontwikkeling storage-verbindingsreeksen...'?
Het selectievakje voor **ontwikkeling storage-verbindingsreeksen bijwerken voor diagnostische gegevens en Caching met Microsoft Azure storage-accountreferenties, bij het publiceren naar Microsoft Azure** biedt u een handige manier om bij te werken van alle ontwikkeling Storage-accountverbinding tekenreeksen met de Azure storage-account dat is opgegeven tijdens het publiceren.

Stel bijvoorbeeld dat u dit selectievakje inschakelt en de verbindingsreeks van diagnostische gegevens geeft `UseDevelopmentStorage=true`. Wanneer u het project naar Azure publiceert, wordt Visual Studio automatisch de diagnostische gegevens-verbindingsreeks bijwerken met het opslagaccount dat u hebt opgegeven in de wizard publiceren. Echter, als een echte storage-account is opgegeven als de verbindingstekenreeks diagnostische gegevens, klikt u vervolgens dat account wordt gebruikt in plaats daarvan.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostische gegevens over functionaliteit verschillen tussen Azure SDK 2.4 en eerder en Azure SDK 2.5 of hoger
Als u uw project van Azure SDK 2.4 naar Azure SDK 2.5 of hoger bijwerkt, u moet voorzien zijn van rekening met de volgende verschillen van de diagnostics-functionaliteit.

* **Configuratie-API's zijn afgeschaft** – programmatische configuratie van diagnostische gegevens is beschikbaar in de Azure SDK 2.4 of eerdere versies, maar is afgeschaft in Azure SDK 2.5 of hoger. Als de configuratie van de diagnostische gegevens op dit moment in de code is gedefinieerd, moet u die instellingen volledig in de gemigreerde-project in de volgorde voor diagnostische gegevens blijven werken opnieuw configureren. Het configuratiebestand van de diagnostische gegevens voor Azure SDK 2.4 is diagnostics.wadcfg en diagnostics.wadcfgx voor Azure SDK 2.5 of hoger.
* **Diagnostische gegevens voor cloudtoepassingen van de service kan alleen worden geconfigureerd op het rolniveau van de, niet op het exemplaarniveau van het.**
* **Telkens wanneer u uw app implementeert, de configuratie van de diagnostische gegevens wordt bijgewerkt** – Hierdoor kunnen problemen met de functiepariteit als u de configuratie van uw diagnostische gegevens vanuit Server Explorer wijzigen en implementeert u uw app opnieuw.
* **In de Azure SDK 2.5 of hoger, crashdumps zijn geconfigureerd in het configuratiebestand van de diagnostische gegevens niet in de code** – als u crashdumps die zijn geconfigureerd in de code hebt, hebt u handmatig de configuratie van code overbrengen naar het configuratiebestand, omdat de crashdumps worden niet overgedragen tijdens de migratie naar Azure SDK 2.6.
