---
title: En geschiedenis van Azure Diagnostics extensie schema configuratieversies | Microsoft Docs
description: Relevant zijn voor het verzamelen van prestatiemeteritems in Azure Virtual Machines, VM-Schaalsets, Service Fabric en Cloud-Services.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2017
ms.author: robb
ms.openlocfilehash: 119e8a237f24cdc80a1ab8e376f2b308c9eada05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-diagnostics-extention-configuration-schema-versions-and-history"></a>Azure Diagnostics extensie schema configuratieversies en geschiedenis
Deze pagina indexen Azure Diagnostics extensie schema versies geleverd als onderdeel van de Microsoft Azure SDK.  

> [!NOTE]
> De extensie Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken van:
> - Azure Virtual Machines 
> - Schaalsets voor virtuele machines
> - Service Fabric 
> - Cloudservices 
> - Netwerkbeveiligingsgroepen
> 
> Deze pagina is alleen relevant als u een van deze services.

De extensie Azure Diagnostics wordt gebruikt met andere Microsoft-producten voor diagnostische gegevens zoals Azure Monitor, Application Insights en Log Analytics. Zie voor meer informatie [hulpprogramma's van Microsoft Bewakingsoverzicht](monitoring-overview.md).

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Azure SDK en diagnostische gegevens versies back-upfunctie grafiek  

|Azure SDK-versie | Versie van de Diagnostics-uitbreiding | Model|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |invoegtoepassing|  
|2.0 - 2.4         |1.0                            |invoegtoepassing|  
|2.5               |1.2                            |De extensie|  
|2.6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1.5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1.8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|



 Azure Diagnostics-versie 1.0 eerst verzonden met een invoegtoepassingsmodel--wat betekent dat u de versie van Azure diagnostics mogelijk wanneer u de Azure SDK geïnstalleerd, is geleverd aan.  

 Beginnen met de SDK 2.5 (diagnostics versie 1.2), Azure diagnostics is verzonden naar een extensiemodel. De hulpprogramma's voor gebruik maken van nieuwe functies zijn alleen beschikbaar in de nieuwere Azure-SDK's, maar alle services die gebruikmaken van Azure diagnostics zou kunnen worden opgepikt de meest recente versie van de back-ups rechtstreeks uit Azure. Bijvoorbeeld iemand anders die nog steeds SDK 2.5 zou worden bij het laden van de meest recente versie die wordt weergegeven in de vorige tabel ongeacht als ze de nieuwer functies gebruiken.  

## <a name="schemas-index"></a>Index van de schema 's  
Verschillende versies van Azure diagnostics andere configuratie-schema's gebruiken. 

[Diagnostische gegevens 1.0 configuratieschema](azure-diagnostics-schema-1dot0.md)  

[Diagnostische gegevens 1.2 configuratieschema](azure-diagnostics-schema-1dot2.md)  

[Diagnostische gegevens 1.3 en hoger configuratieschema](azure-diagnostics-schema-1dot3-and-later.md)  

## <a name="version-history"></a>Versiegeschiedenis


### <a name="diagnostics-extension-19"></a>Extensie voor diagnostische gegevens 1,9 
Docker-ondersteuning is toegevoegd.


### <a name="diagnostics-extension-181"></a>Extensie voor diagnostische gegevens 1.8.1 
Kan een SAS-token in plaats van de sleutel van een opslagaccount opgeven in de persoonlijke configuratie. Als een SAS-token is opgegeven, wordt de sleutel van het opslagaccount wordt genegeerd.


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
Toegevoegde opslagtype moet PublicConfig. StorageType mag *tabel*, *Blob*, *TableAndBlob*. *Tabel* is de standaardinstelling.


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
De PUT-element en de mogelijkheid om diagnostische gegevens verzenden toegevoegd [Application Insights](../application-insights/app-insights-cloudservices.md) waardoor het gemakkelijker wordt om problemen te diagnosticeren via uw toepassing, evenals het niveau van systeem en infrastructuur.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Azure SDK 2.6 en diagnostische gegevens van de extensie 1.3 
Voor Cloud Service-projecten in Visual Studio, zijn de volgende wijzigingen aangebracht. (Deze wijzigingen ook van toepassing op latere versies van Azure SDK.)

* De lokale emulator ondersteunt nu de diagnostische gegevens. Dit betekent dat u kunt het verzamelen van diagnostische gegevens en zorg ervoor dat uw toepassing wordt gemaakt door de juiste traceringen tijdens het ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` kunt verzamelen van diagnostische gegevens, terwijl u uw cloudserviceproject in Visual Studio uitvoert met behulp van de Azure-opslagemulator. Alle diagnostische gegevens worden verzameld in het opslagaccount (ontwikkeling opslag).
* De verbindingsreeks van diagnostische gegevens storage-account (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) wordt nog een keer opgeslagen in het service-configuratiebestand (.cscfg). In de Azure SDK 2.5 is de opslagaccount voor diagnostische gegevens opgegeven in het bestand diagnostics.wadcfgx.

Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in de Azure SDK 2.4 en eerder heeft gewerkt, en hoe het werkt in de Azure SDK 2.6 en hoger.

* In Azure SDK 2.4 en eerdere versies, is de verbindingsreeks gebruikt als een runtime door de invoegtoepassing diagnostische gegevens ophalen van de accountgegevens voor de opslag voor het overdragen van diagnostische logboeken.
* In Azure SDK 2.6 en hoger, wordt de verbindingsreeks van de diagnostische gegevens door Visual Studio gebruikt voor het configureren van de extensie voor diagnostische gegevens met de juiste opslag gegevens tijdens de publicatie. De verbindingsreeks kunt u verschillende opslagaccounts voor verschillende configuraties die door Visual Studio wordt gebruikt bij het publiceren van definiëren. Omdat de diagnostics-invoegtoepassing (na de Azure SDK 2,5) niet langer beschikbaar is, kan niet het cscfg-bestand zelf echter de extensie voor diagnostische gegevens inschakelen. U moet de extensie afzonderlijk via hulpprogramma's zoals Visual Studio of PowerShell inschakelen.
* De uitvoer van het pakket van Visual Studio bevat ook de openbare configuratie-XML voor de extensie voor diagnostische gegevens voor elke rol voor het vereenvoudigen van de extensie voor diagnostische gegevens configureren met PowerShell. De verbindingsreeks diagnostics Visual Studio gebruikt voor het vullen van de opslag-accountgegevens die aanwezig zijn in de openbare configuratie. De openbare configuratiebestanden worden gemaakt in de map extensies en volg het patroon PaaSDiagnostics. <RoleName>. PubConfig.xml. Alle implementaties op basis van PowerShell kunnen dit patroon gebruiken voor elke configuratie worden toegewezen aan een rol.
* De verbindingsreeks in het .cscfg-bestand wordt ook gebruikt door de Azure-portal om diagnostische gegevens zodat het kan worden weergegeven de **bewaking** tabblad. De verbindingsreeks is nodig voor de service configureren voor het uitgebreide bewakingsgegevens weergeven in de portal.

#### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migreren projecten op Azure SDK 2.6 en hoger
Wanneer u migreert van Azure SDK 2.5 naar Azure SDK 2.6 of hoger, als u een opslagaccount voor diagnostische gegevens opgegeven in het bestand .wadcfgx had, vervolgens blijft er. Als u wilt profiteren van de flexibiliteit van het gebruik van verschillende storage-accounts voor opslagconfiguraties verschillende, hebt u de verbindingsreeks handmatig toevoegen aan uw project. Als u een project waarnaar u migreert van Azure SDK 2.4 of eerder in Azure SDK 2.6, blijven de verbindingsreeksen van diagnostische gegevens behouden. Let echter de wijzigingen in hoe verbindingsreeksen worden behandeld in de Azure SDK 2.6 zoals opgegeven in de vorige sectie.

#### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hoe de opslagaccount voor diagnostische gegevens in Visual Studio wordt bepaald
* Als een verbindingsreeks diagnostische gegevens in het .cscfg-bestand is opgegeven, wordt het in Visual Studio gebruikt voor het configureren van de extensie voor diagnostische gegevens bij het publiceren en bij het genereren van de openbare configuratie-xml-bestanden tijdens pakketten.
* Als geen diagnostische gegevens verbindingsreeks is opgegeven in het .cscfg-bestand, klikt u vervolgens terugvalt Visual Studio op het storage-account opgegeven in het bestand .wadcfgx met de extensie voor diagnostische gegevens bij het publiceren van en het genereren van de openbare configuratie-xml-bestanden configureren Wanneer verpakken.
* De verbindingsreeks voor diagnostische gegevens in het .cscfg-bestand heeft voorrang op de storage-account in het bestand .wadcfgx. Als een verbindingsreeks diagnostics is opgegeven in het .cscfg-bestand, Visual Studio die wordt gebruikt en het opslagaccount in .wadcfgx worden genegeerd.

#### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Wat doet de 'Update ontwikkeling storage-verbindingsreeksen..." selectievakje doen?
Het selectievakje voor **bijwerken van ontwikkeling storage-verbindingsreeksen voor diagnostische gegevens en opslaan in cache met Microsoft Azure storage-accountreferenties bij het publiceren van Microsoft Azure** biedt u een handige manier om bij te werken van alle ontwikkeling opslagaccountverbindingsreeksen met de Azure storage-account dat is opgegeven tijdens de publicatie.

Stel bijvoorbeeld dat u dit selectievakje inschakelt en de verbindingsreeks voor diagnostische gegevens `UseDevelopmentStorage=true`. Wanneer u het project naar Azure publiceert, wordt Visual Studio de verbindingsreeks van diagnostische gegevens automatisch bijgewerkt met de storage-account dat u hebt opgegeven in de wizard publiceren. Echter, als een echte storage-account is opgegeven als de verbindingsreeks van diagnostische gegevens, klikt u vervolgens dat account wordt gebruikt in plaats daarvan.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostische functionaliteit verschillen tussen Azure SDK 2.4 en eerder en Azure SDK 2,5 en hoger
Als u een uw project van Azure SDK 2.4 naar Azure SDK 2.5 of hoger upgrade uitvoert, moet u vergeet de volgende verschillen van de diagnostics-functionaliteit.

* **Configuratie-API's zijn afgeschaft** – programmatische configuratie van diagnostische gegevens is beschikbaar in de Azure SDK 2.4 of eerdere versies, maar is afgeschaft in Azure SDK 2.5 en hoger. Als uw configuratie van diagnostische momenteel in de code is gedefinieerd, moet u die instellingen vanaf het begin in het gemigreerde project in de volgorde voor diagnostische gegevens blijven werken opnieuw configureren. Het configuratiebestand van de diagnostische gegevens voor Azure SDK 2.4 is diagnostics.wadcfg en diagnostics.wadcfgx voor Azure SDK 2.5 of hoger.
* **Diagnostische gegevens voor cloud-servicetoepassingen kan alleen worden geconfigureerd op het niveau van de functie niet op het niveau van het exemplaar.**
* **Telkens wanneer u uw app implementeert, wordt de configuratie van diagnostische bijgewerkt** : dit kan pariteit problemen veroorzaken als u de configuratie van de diagnostische gegevens in Server Explorer te wijzigen en vervolgens uw app te implementeren.
* **In de Azure SDK 2.5 en hoger crashdumps zijn geconfigureerd in het configuratiebestand van de diagnostische gegevens niet in de code** – als u geconfigureerd in de code crashdumps hebt, hebt u de configuratie van code handmatig te zetten naar het configuratiebestand, omdat de crashdumps worden niet tijdens de migratie naar Azure SDK 2.6 overgedragen.

