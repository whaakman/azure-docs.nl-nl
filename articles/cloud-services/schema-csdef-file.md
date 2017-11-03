---
title: Azure Cloud Services-definitie Schema (csdef-bestand) | Microsoft Docs
ms.custom: 
ms.date: 04/14/2015
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: b7735dbf-8e91-4d1b-89f7-2f17e9302469
caps.latest.revision: "42"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: b833fdc06e4193c1b478028733c336feb6d8b9ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure Cloud Services-Schema voor de definitie (csdef-bestand)
Het servicedefinitiebestand definieert het servicemodel voor een toepassing. Het bestand bevat de definities voor de functies die beschikbaar voor een cloudservice zijn, geeft de service-eindpunten en stelt configuratie-instellingen voor de service. Configuratie-instellingswaarden worden ingesteld in het configuratiebestand van de service, zoals beschreven door de [Cloudservice (klassiek) configuratieschema](http://msdn.microsoft.com/library/b1ae68cd-cc95-48cb-a4a4-da91dc708a35).

Het schema van Azure Diagnostics configuratiebestand worden standaard geïnstalleerd op de `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` directory. Vervang `<version>` met de geïnstalleerde versie van de [Azure SDK](http://www.windowsazure.com/develop/downloads/).

De standaardextensie voor het servicedefinitiebestand is csdef.

## <a name="basic-service-definition-schema"></a>Basic-service definitie schema
Het servicedefinitiebestand moet één bevatten `ServiceDefinition` element. De servicedefinitie moet ten minste één rol bevatten (`WebRole` of `WorkerRole`) element. Kan maximaal 25 rollen gedefinieerd in één definitie bevatten en kunt u roltypen elkaar. De servicedefinitie bevat ook de optionele `NetworkTrafficRules` element waarmee de beperkt welke rollen kan communiceren met de opgegeven interne eindpunten. De servicedefinitie bevat ook de optionele `LoadBalancerProbes` element waarin klant statuscontroles van eindpunten gedefinieerd.

De basisindeling van het servicedefinitiebestand is als volgt.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Schemadefinities
Het schema voor de volgende onderwerpen wordt beschreven:

- [LoadBalancerProbe Schema](schema-csdef-loadbalancerprobe.md)
- [WebRole Schema](schema-csdef-webrole.md)
- [WorkerRole Schema](schema-csdef-workerrole.md)
- [NetworkTrafficRules Schema](schema-csdef-networktrafficrules.md)

##  <a name="ServiceDefinition"></a>ServiceDefinition Element
De `ServiceDefinition` -element is het element op het hoogste niveau van het servicedefinitiebestand.

De volgende tabel beschrijft de kenmerken van de `ServiceDefinition` element.

| Kenmerk               | Beschrijving |
| ----------------------- | ----------- |
| naam                    |Vereist. De naam van de service. De naam moet uniek zijn binnen het serviceaccount.|
| topologyChangeDiscovery | Optioneel. Hiermee geeft u het type topologie wijzigingsmelding. Mogelijke waarden zijn:<br /><br /> -   `Blast`-De update zo snel mogelijk verzendt naar alle rolinstanties. Als u de optie kiest, is de rol moet kunnen verwerken van de update topologie zonder wordt opnieuw opgestart.<br />-   `UpgradeDomainWalk`– Verzendt de update naar elke rolinstantie in de juiste volgorde nadat het vorige exemplaar heeft de update is geaccepteerd.|
| schemaVersion           | Optioneel. Hiermee geeft u de versie van het schema van de definitie van service. De schemaversie kunt Visual Studio, selecteer de juiste SDK's moet worden gebruikt voor schemavalidatie als meer dan één versie van de SDK is geïnstalleerd naast elkaar.|
| upgradeDomainCount      | Optioneel. Hiermee geeft u het aantal upgradedomeinen in die functies in deze service worden toegewezen. Rolinstanties zijn toegewezen aan een upgradedomein wanneer de service wordt geïmplementeerd. Zie voor meer informatie [bijwerken van een cloud service-rol of de implementatie](cloud-services-how-to-manage-portal.md#how-to-update-a-cloud-service-role-or-deployment).<br /><br /> U kunt maximaal 20 upgradedomeinen opgeven. Als niet wordt opgegeven, is het aantal upgradedomeinen 5.|