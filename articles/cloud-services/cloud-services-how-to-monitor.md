---
title: Een Azure-Cloud-Service controleren | Microsoft Docs
description: Hierin wordt beschreven wat de bewaking van een Azure Cloud Service omvat en aantal van uw opties zijn.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: ''
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: jeconnoc
ms.openlocfilehash: 725a705f746bbf5e32fcc410ba4153ac29e3fc3d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003850"
---
# <a name="introduction-to-cloud-service-monitoring"></a>Inleiding tot Cloudservice controleren

U kunt de van de belangrijkste metrische prestatiegegevens voor elke cloudservice bewaken. De rol van elke cloud-service verzamelt minimale gegevens: CPU-gebruik, netwerkgebruik en schijfgebruik. Als de service in de cloud heeft de `Microsoft.Azure.Diagnostics` extensie toegepast op een rol die rol kan extra punten van de gegevens worden verzameld. In dit artikel bevat een inleiding tot Azure Diagnostics voor Cloud Services.

Met eenvoudige controle, gegevens van prestatiemeteritems van instanties van de voorbeelden worden gemaakt en die worden verzameld met 3 minuten durende intervallen. Deze eenvoudige bewaking gegevens worden niet opgeslagen in uw storage-account en is zonder extra kosten aan gekoppeld.

Aanvullende metrische gegevens zijn met geavanceerde bewaking voorbeelden worden gemaakt en die worden verzameld met intervallen van vijf minuten, 1 uur en 12 uur. De samengevoegde gegevens worden opgeslagen in een storage-account in tabellen, en na 10 dagen wordt verwijderd. Het opslagaccount dat wordt gebruikt is door de rol; geconfigureerd u kunt verschillende opslagaccounts gebruiken voor verschillende rollen. Dit is geconfigureerd met een verbindingsreeks in de [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) en [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) bestanden.


## <a name="basic-monitoring"></a>Basiscontrole

Zoals vermeld in de inleiding, verzamelt een cloudservice automatisch basic bewakingsgegevens van de virtuele machine van de host. Deze gegevens omvatten CPU-percentage, in/uit-netwerk en schijf lezen/schrijven. De verzamelde gegevens wordt automatisch weergegeven op de pagina overzicht en metrische gegevens van de cloudservice in Azure portal. 

Basiscontrole, hoeft niet een storage-account. 

![tegels voor servicecontrole over de cloud](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Geavanceerde controle

Geavanceerde controle omvat het gebruik van de **Azure Diagnostics** -extensie (en eventueel de Application Insights-SDK) van de rol die u wilt bewaken. De diagnostics-extensie gebruikmaakt van een configuratiebestand (per rol) met de naam **diagnostics.wadcfgx** het configureren van de metrische gegevens bewaakt. De diagnostische Azure-extensie verzamelt en gegevens worden opgeslagen in een Azure Storage-account. Deze instellingen zijn geconfigureerd in de **.wadcfgx**, [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef), en [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) bestanden. Dit betekent dat er extra kosten die gepaard gaan met geavanceerde controle.

Omdat elke rol is gemaakt, wordt de Azure Diagnostics-extensie in Visual Studio toegevoegd aan het. Deze extensie voor diagnostische gegevens kan de volgende soorten informatie verzamelen:

* Aangepaste prestatiemeteritems
* Toepassingslogboeken
* Windows-gebeurtenislogboeken
* De bron van de .NET-gebeurtenis
* IIS-logboeken
* ETW op basis van manifest
* Crashdumps
* Klant-foutenlogboeken

> [!IMPORTANT]
> Hoewel al deze gegevens worden samengevoegd in de storage-account, de portal biedt **niet** bieden een systeemeigen manier om de gegevens van grafiek. Het is raadzaam dat u een andere service, zoals Application Insights in uw toepassing integreren.

## <a name="setup-diagnostics-extension"></a>Setup-extensie voor diagnostische gegevens

Eerste, als u geen een **klassieke** storage-account, [maakt u er een](../storage/common/storage-create-storage-account.md#create-a-storage-account). Zorg ervoor dat het opslagaccount is gemaakt met de **klassieke implementatiemodel** opgegeven.

Vervolgens gaat u naar de **opslagaccount (klassiek)** resource. Selecteer **instellingen** > **toegangssleutels** en kopieer de **primaire verbindingsreeks** waarde. In dat geval moet u deze waarde in voor de cloudservice. 

Er zijn twee configuratiebestanden voor geavanceerde diagnostische gegevens worden ingeschakeld, moet u **ServiceDefinition.csdef** en **ServiceConfiguration.cscfg**.

### <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef

In de **ServiceDefinition.csdef** bestand, Voeg een nieuwe instelling `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` voor elke rol die gebruikmaakt van geavanceerde diagnostische gegevens. Wanneer u een nieuw project maakt, Visual Studio deze waarde toegevoegd aan het bestand. Als deze ontbreekt, kunt u deze nu toevoegen. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Hiermee definieert u een nieuwe instelling die moet worden toegevoegd voor elke **ServiceConfiguration.cscfg** bestand. 

Waarschijnlijk hebt u twee **.cscfg** bestanden, één met de naam **ServiceConfiguration.cloud.cscfg** voor het implementeren op Azure en één met de naam **ServiceConfiguration.local.cscfg** die wordt gebruikt voor lokale implementaties in de geëmuleerde omgeving. Open en wijzig elke **.cscfg** bestand. Voeg een instelling met de naam `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`. Stel de waarde voor de **primaire verbindingsreeks** van het klassieke storage-account. Als u gebruiken van de lokale opslag op uw ontwikkelcomputer wilt, gebruikt u `UseDevelopmentStorage=true`.

```xml
<ServiceConfiguration serviceName="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="WorkerRoleWithSBQueue1">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=mystorage;AccountKey=KWwkdfmskOIS240jnBOeeXVGHT9QgKS4kIQ3wWVKzOYkfjdsjfkjdsaf+sddfwwfw+sdffsdafda/w==" />
      
      <!-- or use the local development machine for storage
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
      -->
```

## <a name="use-application-insights"></a>Application Insights gebruiken

Wanneer u de Cloud-Service van Visual Studio publiceert, krijgt u de optie voor het verzenden van diagnostische gegevens naar Application Insights. U kunt maken van de Application Insights-Azure-resource op dat moment of de gegevens verzenden naar een bestaande Azure-resource. Uw cloudservice kan worden bewaakt door Application Insights voor beschikbaarheid, prestaties, fouten en gebruik. Aangepaste grafieken kunnen worden toegevoegd aan Application Insights, zodat u de gegevens kunt zien die het meest belangrijk is. Gegevens van exemplaar van rol kunnen worden verzameld met behulp van de Application Insights SDK in uw cloud service-project. Zie voor meer informatie over hoe u Application Insights integreert [Application Insights met Cloud Services](../application-insights/app-insights-cloudservices.md).

Houd er rekening mee dat wanneer u Application Insights gebruiken kunt om weer te geven van de prestatiemeteritems (en de andere instellingen) u hebt opgegeven via de Windows Azure Diagnostics-extensie, u alleen een rijkere ervaring krijgen door de integratie van Application Insights SDK in uw Worker en webservice-rollen.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Application Insights met Cloudservices](../application-insights/app-insights-cloudservices.md)
- [Instellen van prestatiemeteritems](diagnostics-performance-counters.md)

