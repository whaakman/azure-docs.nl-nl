---
title: Azure Cloud Services van Definitieschema (.cscfg-bestand) | Microsoft Docs
services: cloud-services
ms.custom: ''
ms.date: 12/07/2016
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 3ddc7fea-3339-4fc0-bdf9-853c32b25f69
caps.latest.revision: 35
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 96df87a0d49296280140e392509c0d735f904957
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007019"
---
# <a name="azure-cloud-services-config-schema-cscfg-file"></a>Azure Cloud Services-Config-Schema (.cscfg-bestand)
Het serviceconfiguratiebestand Hiermee geeft u het aantal rolinstanties implementeren voor elke rol in de service, de waarden van de configuratie-instellingen en de vingerafdrukken instellen voor alle certificaten die zijn gekoppeld aan een rol. Als de service deel uit van een Virtueelnetwerk maakt, kunnen configuratie-informatie voor het netwerk moet worden opgegeven in het serviceconfiguratiebestand, evenals van het configuratiebestand voor virtuele netwerken. De standaardextensie voor het serviceconfiguratiebestand is cscfg-bestand.

Het servicemodel is beschreven door de [Definitieschema voor Cloud-Service (klassiek)](schema-csdef-file.md).

Het hulpprogramma voor het schema van de Azure Diagnostics-configuratiebestand is standaard geïnstalleerd op de `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` directory. Vervang `<version>` met de geïnstalleerde versie van de [Azure SDK](https://azure.microsoft.com/downloads/).

Zie voor meer informatie over het configureren van rollen in een service [wat is het servicemodel Cloud](cloud-services-model-and-package.md).

## <a name="basic-service-configuration-schema"></a>Het Schema van Basic-Service
De basisindeling van het serviceconfiguratiebestand is als volgt.

```xml
<ServiceConfiguration serviceName="<service-name>" osFamily="<osfamily-number>" osVersion="<os-version>" schemaVersion="<schema-version>">

  <Role …>
    …
  </Role>

  <NetworkConfiguration>
    …
  </NetworkConfiguration>

</ServiceConfiguration>
```

## <a name="schema-definitions"></a>Schemadefinities
De volgende onderwerpen bevatten informatie over het schema voor de `ServiceConfiguration` element:

- [Role Schema](schema-cscfg-role.md)
- [NetworkConfiguration Schema](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Service-configuratie Namespace
De XML-naamruimte voor het configuratiebestand van de service is: `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration`.

##  <a name="ServiceConfiguration"></a> ServiceConfiguration-Element
De `ServiceConfiguration` -element is het element op het hoogste niveau van het configuratiebestand van de service.

De volgende tabel beschrijft de kenmerken van de `ServiceConfiguration` element. Alle kenmerken waarden zijn tekenreekstypen.

| Kenmerk | Beschrijving |
| --------- | ----------- |
|serviceName|Vereist. De naam van de cloudservice. De hier opgegeven naam moet overeenkomen met de naam die is opgegeven in het servicedefinitiebestand.|
|besturingssysteemtype|Optioneel. Hiermee geeft u het Gastbesturingssysteem dat wordt uitgevoerd op rolinstanties in de cloudservice. Zie voor meer informatie over ondersteunde Guest OS releases [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md).<br /><br /> Als u geen een `osFamily` waarde en u niet hebt ingesteld de `osVersion` kenmerk naar een specifieke Guest OS-versie, een standaardwaarde van 1 wordt gebruikt.|
|osVersion|Optioneel. Hiermee geeft u de versie van het Gastbesturingssysteem dat wordt uitgevoerd op rolinstanties in de cloudservice. Zie voor meer informatie over de Gast-OS-versies, [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md).<br /><br /> U kunt opgeven dat het Gastbesturingssysteem automatisch moeten worden bijgewerkt naar de meest recente versie. Om dit te doen, stel de waarde van de `osVersion` kenmerk `*`. Als de waarde `*`, de rolinstanties zijn geïmplementeerd met behulp van de meest recente versie van het Gastbesturingssysteem van de opgegeven OS-familie en wordt automatisch bijgewerkt wanneer nieuwe versies van het Gastbesturingssysteem worden vrijgegeven.<br /><br /> Als u wilt een specifieke versie handmatig opgeven, gebruikt u de `Configuration String` uit de tabel in de **toekomst, huidige en overgangs Gast OS-versies** sectie van [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md) .<br /><br /> De standaardwaarde voor de `osVersion` kenmerk is `*`.|
|schemaVersion|Optioneel. Hiermee geeft u de versie van het schema van de Service. De schemaversie kunt Visual Studio om het juiste SDK-hulpprogramma's gebruiken voor schemavalidatie als meer dan één versie van de SDK is geïnstalleerd selecteert side-by-side. Zie voor meer informatie over de compatibiliteit van schema- en versie [Azure Guest OS Releases en SDK Compatibility Matrix](cloud-services-guestos-update-matrix.md)|

Het configuratiebestand van de service moet een bevatten `ServiceConfiguration` element. De `ServiceConfiguration` element omvat mogelijk een willekeurig aantal `Role` elementen en nul of 1 `NetworkConfiguration` elementen.