---
title: Een Azure Cloudservice bewaken | Microsoft Docs
description: Hierin wordt beschreven wat een Azure Cloud Service bewaking omvat en het aantal van uw opties zijn.
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: adegeo
ms.openlocfilehash: c63a49c65f2d8261caa534308477888c752a89da
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="introduction-to-cloud-service-monitoring"></a>Inleiding tot Cloudservice controleren

U kunt de belangrijkste prestatiemetrieken voor elke cloudservice bewaken. Elke functie cloud service verzamelt minimale gegevens: CPU-gebruik, netwerkgebruik en het gebruik van de schijf. Als de cloudservice heeft de `Microsoft.Azure.Diagnostics` extensie toegepast op een rol, die rol extra punten van de gegevens kunt verzamelen. Dit artikel bevat een inleiding tot Azure Diagnostics voor Cloud-Services.

Met eenvoudige bewaking prestatiemeteritemgegevens van rolinstanties door actieve en verzameld om de 3 minuten. Deze eenvoudige bewakingsgegevens worden niet opgeslagen in uw opslagaccount en is geen extra kosten aan gekoppeld.

Met geavanceerde bewaking, aanvullende gegevens door actieve en verzameld met een interval van 5 minuten, 1 uur en 12 uur. De verzamelde gegevens worden opgeslagen in een opslagaccount in tabellen, en na 10 dagen wordt verwijderd. Het opslagaccount gebruikt is geconfigureerd door de rol; u kunt verschillende storage-accounts gebruiken voor andere rollen. Dit is geconfigureerd met een verbindingsreeks in de [csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) en [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) bestanden.


## <a name="basic-monitoring"></a>Basiscontrole

Zoals vermeld in de introductie, verzamelt een cloudservice automatisch basic bewakingsgegevens van de virtuele-hostmachine. Deze gegevens omvatten CPU-percentage, in/out-netwerk en schijf lezen/schrijven. De verzamelde gegevens van de bewaking wordt automatisch weergegeven op de pagina's overzicht en metrische gegevens van de cloudservice in de Azure portal. 

Basic bewaking is niet vereist voor een opslagaccount. 

![Basic cloudservice tegels bewaking](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Geavanceerde controle

Geavanceerde controle omvat het gebruik van de extensie Azure Diagnostics (en desgewenst de Application Insights-SDK) op de rol die u wilt bewaken. De extensie voor diagnostische gegevens maakt gebruik van een configuratiebestand (per rol) met de naam **diagnostics.wadcfgx** voor het configureren van de diagnostische gegevens metrische gegevens bewaakt. De gegevens de diagnostische Azure-extensie verzamelt wordt opgeslagen in een Azure Storage-account is geconfigureerd in de **.wadcfgx** en in de [csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) en [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) bestanden. Dit betekent dat er een extra kosten in verband met geavanceerde bewaking.

Omdat elke rol is gemaakt, wordt de extensie Azure Diagnostics in Visual Studio toegevoegd aan het. Deze extensie kunt de volgende soorten informatie verzamelen:

* Aangepaste prestatiemeteritems
* Toepassingslogboeken
* Windows-gebeurtenislogboeken
* De gebeurtenisbron .NET
* IIS-logboeken
* ETW op basis van het manifest
* Crashdumps
* Klant-foutenlogboeken

Hoewel al deze gegevens worden samengevoegd in de storage-account, is de portal biedt geen een systeemeigen manier om de grafiekgegevens. U kunt een andere service, zoals Application Insights, afstemmen en de gegevens worden weergegeven.

### <a name="use-application-insights"></a>Application Insights gebruiken

Wanneer u de Cloudservice vanuit Visual Studio publiceert, krijgt u de optie voor de diagnostische gegevens verzenden naar Application Insights. U kunt de Application Insights-resource op dat moment gemaakt of de gegevens verzenden naar een bestaande resource. Uw cloudservice kan worden bewaakt met Application Insights voor beschikbaarheid, prestaties, fouten en informatie over het gebruik. Aangepaste grafieken kunnen worden toegevoegd aan de Application Insights zodat u de gegevens ziet die belangrijk is de meest voor u. Rol van de gegevens kunnen worden verzameld met behulp van de Application Insights-SDK in uw cloudserviceproject. Zie voor meer informatie over het integreren van Application Insights [Application Insights met Cloudservices](../application-insights/app-insights-cloudservices.md).

Opmerking terwijl u Application Insights kunt gebruiken om weer te geven van de prestatiemeteritems (en de andere instellingen) u hebt opgegeven via de Windows Azure Diagnostics-extensie, u krijgt alleen een rijkere ervaring door de Application Insights-SDK integreren in uw rollen worker en webservice.


## <a name="add-advanced-monitoring"></a>Geavanceerde bewaking toevoegen

Eerste, als u hebt een **klassieke** opslagaccount [maken van een](../storage/common/storage-create-storage-account.md#create-a-storage-account). Zorg ervoor dat het opslagaccount is gemaakt met de **klassieke implementatiemodel** opgegeven.

Vervolgens gaat u naar de **Storage-account (klassiek)** resource. Selecteer **instellingen** > **toegangssleutels** en kopieer de **primaire verbindingsreeks** waarde. In dat geval moet u deze waarde in voor de cloudservice. 

Er zijn twee configuratiebestanden voor geavanceerde diagnostische gegevens worden inschakelen, moet u **ServiceDefinition.csdef** en **ServiceConfiguration.cscfg**. Waarschijnlijk hebt u twee **.cscfg** bestanden, één met de naam **ServiceConfiguration.cloud.cscfg** voor het implementeren op Azure en één met de naam **ServiceConfiguration.local.cscfg** die wordt gebruikt voor lokale foutopsporing implementaties. Wijzigen van beide.

In de **ServiceDefinition.csdef** bestand, het toevoegen van een nieuwe instelling `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` voor elke rol die gebruikmaakt van geavanceerde diagnostische gegevens. Wanneer u een nieuw project maakt, Visual Studio deze waarde toegevoegd aan het bestand. Als het bestand ontbreekt, kunt u deze nu toevoegen. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Hiermee definieert u een nieuwe instelling die moet worden toegevoegd voor elke **ServiceConfiguration.cscfg** bestand. Open en wijzig elke **.cscfg** bestand. Voeg een instelling met de naam `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`. De waarde ingesteld op de **primaire verbindingsreeks** van het klassieke opslagaccount of naar `UseDevelopmentStorage=true`, als u wilt gebruiken van de lokale opslag op uw ontwikkelcomputer.

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

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Application Insights met Cloudservices.](../application-insights/app-insights-cloudservices.md)

