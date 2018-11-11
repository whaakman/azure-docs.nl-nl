---
title: Azure Cloud Services van Definitieschema (.csdef-bestand) | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: b7735dbf-8e91-4d1b-89f7-2f17e9302469
caps.latest.revision: 42
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 9cb78362b5c0613d6ed6820bbf8e6d3275ab4787
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250659"
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>De definitie van Schema (.csdef-bestand) voor Azure Cloud Services
Het servicedefinitiebestand definieert het servicemodel voor een toepassing. Het bestand bevat de definities voor de functies die beschikbaar voor een cloudservice zijn, Hiermee geeft u de service-eindpunten en configuratie-instellingen voor de service maakt. Configuratie-instellingswaarden zijn ingesteld in het configuratiebestand van de service, zoals is beschreven in de [configuratieschema voor Cloud-Service (klassiek)](https://msdn.microsoft.com/library/b1ae68cd-cc95-48cb-a4a4-da91dc708a35).

Het hulpprogramma voor het schema van de Azure Diagnostics-configuratiebestand is standaard geïnstalleerd op de `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` directory. Vervang `<version>` met de geïnstalleerde versie van de [Azure SDK](http://www.windowsazure.com/develop/downloads/).

De standaardextensie voor het servicedefinitiebestand is .csdef.

## <a name="basic-service-definition-schema"></a>Definitieschema voor Basic-service
Het servicedefinitiebestand moet één bevatten `ServiceDefinition` element. Definitie van de service moet ten minste één rol bevatten (`WebRole` of `WorkerRole`) element. Het kan maximaal 25 rollen die zijn gedefinieerd in één definitie bevatten en u roltypen kunt combineren. Definitie van de service bevat ook de optionele `NetworkTrafficRules` element dat voorkomt welke functies dat kan communiceren met de opgegeven interne eindpunten. Definitie van de service bevat ook de optionele `LoadBalancerProbes` element waarin de klant gedefinieerd statuscontroles van eindpunten.

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
De volgende onderwerpen wordt beschreven voor het schema:

- [LoadBalancerProbe Schema](schema-csdef-loadbalancerprobe.md)
- [WebRole Schema](schema-csdef-webrole.md)
- [WorkerRole Schema](schema-csdef-workerrole.md)
- [NetworkTrafficRules Schema](schema-csdef-networktrafficrules.md)

##  <a name="ServiceDefinition"></a> ServiceDefinition-Element
De `ServiceDefinition` -element is het element op het hoogste niveau van het servicedefinitiebestand.

De volgende tabel beschrijft de kenmerken van de `ServiceDefinition` element.

| Kenmerk               | Beschrijving |
| ----------------------- | ----------- |
| naam                    |Vereist. De naam van de service. De naam moet uniek zijn binnen het serviceaccount.|
| topologyChangeDiscovery | Optioneel. Hiermee geeft u het type topologie wijzigingsmelding. Mogelijke waarden zijn:<br /><br /> -   `Blast` -De update zo snel mogelijk verzendt naar alle rolinstanties. Als u kiest, moet de rol van de update topologie zonder wordt opnieuw opgestart overweg kan met zijn.<br />-   `UpgradeDomainWalk` – Verzendt de update naar elke rolinstantie in een sequentiële wijze nadat het vorige exemplaar heeft de update is geaccepteerd.|
| schemaVersion           | Optioneel. Hiermee geeft u de versie van de definitie van schema van de service. De schemaversie kunt Visual Studio om het juiste SDK-hulpprogramma's gebruiken voor schemavalidatie als meer dan één versie van de SDK is geïnstalleerd selecteert side-by-side.|
| upgradeDomainCount      | Optioneel. Hiermee geeft u het aantal upgradedomeinen over welke functies in deze service worden toegewezen. Rolinstanties worden toegewezen aan een upgradedomein als de service is geïmplementeerd. Zie voor meer informatie, [bijwerken van een cloud service-rol of de implementatie](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment).<br /><br /> U kunt maximaal 20 upgradedomeinen opgeven. Indien niet opgegeven, is het aantal upgradedomeinen 5.|