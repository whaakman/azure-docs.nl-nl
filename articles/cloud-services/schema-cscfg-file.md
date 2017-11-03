---
title: Azure Cloud Services-definitie Schema (.cscfg-bestand) | Microsoft Docs
ms.custom: 
ms.date: 12/07/2016
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 3ddc7fea-3339-4fc0-bdf9-853c32b25f69
caps.latest.revision: "35"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: bba02688e443d4b5bde89691ca0b74b3597b453a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-config-schema-cscfg-file"></a>Azure Cloud Services-Config-Schema (.cscfg-bestand)
Het configuratiebestand van de service wordt het aantal rolinstanties implementeren voor elke rol in de service, de waarden van een configuratie-instellingen en de vingerafdrukken instellen voor alle certificaten die zijn gekoppeld aan een rol. Als de service deel van een virtueel netwerk uitmaakt, kunnen configuratie-informatie voor het netwerk moet worden opgegeven in het configuratiebestand van de service, evenals in het configuratiebestand voor virtuele netwerken. De standaardextensie voor het configuratiebestand van de service is cscfg-bestand.

Het servicemodel is beschreven door de [Cloudservice (klassiek) definitie Schema](schema-csdef-file.md).

Het schema van Azure Diagnostics configuratiebestand worden standaard geïnstalleerd op de `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` directory. Vervang `<version>` met de geïnstalleerde versie van de [Azure SDK](https://azure.microsoft.com/downloads/).

Zie voor meer informatie over het configureren van rollen in een service [wat is het Cloud-servicemodel](cloud-services-model-and-package.md).

## <a name="basic-service-configuration-schema"></a>Basic-Service configuratieschema
De basisindeling van het configuratiebestand van de service is als volgt.

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
De volgende onderwerpen beschrijven het schema voor de `ServiceConfiguration` element:

- [Role Schema](schema-cscfg-role.md)
- [NetworkConfiguration Schema](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Service Configuration Namespace
De XML-naamruimte voor het service-configuratiebestand is: `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration`.

##  <a name="ServiceConfiguration"></a>Element serviceconfiguration zijn
De `ServiceConfiguration` -element is het element op het hoogste niveau van het configuratiebestand van de service.

De volgende tabel beschrijft de kenmerken van de `ServiceConfiguration` element. Alle kenmerken waarden zijn tekenreekstypen.

| Kenmerk | Beschrijving |
| --------- | ----------- |
|Servicenaam|Vereist. De naam van de cloudservice. De hier opgegeven naam moet overeenkomen met de naam die is opgegeven in het servicedefinitiebestand.|
|besturingssysteemtype|Optioneel. Hiermee geeft u het Gastbesturingssysteem dat op de rolinstanties in de cloudservice wordt uitgevoerd. Zie voor meer informatie over ondersteunde Gast OS releases [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).<br /><br /> Als u geen een `osFamily` waarde en u niet hebt ingesteld de `osVersion` kenmerk naar een specifieke Gast OS-versie, een standaardwaarde van 1 wordt gebruikt.|
|OsVersion|Optioneel. Hiermee geeft u de versie van het Gastbesturingssysteem dat op de rolinstanties in de cloudservice wordt uitgevoerd. Zie voor meer informatie over de versies van het Gastbesturingssysteem [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).<br /><br /> U kunt opgeven dat het Gastbesturingssysteem automatisch moeten worden bijgewerkt naar de nieuwste versie. Om dit te doen, stel de waarde van de `osVersion` kenmerk `*`. Als de waarde `*`, de rolinstanties zijn geïmplementeerd met behulp van de nieuwste versie van het Gastbesturingssysteem voor de opgegeven OS-familie en wordt automatisch bijgewerkt wanneer er nieuwe versies van het Gastbesturingssysteem zijn uitgebracht.<br /><br /> Als u wilt een specifieke versie handmatig opgeven, gebruikt u de `Configuration String` uit de tabel in de **toekomstige, huidige en overgangs Gast OS-versies** sectie van [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md) .<br /><br /> De standaardwaarde voor de `osVersion` kenmerk `*`.|
|schemaVersion|Optioneel. Hiermee geeft u de versie van het schema van de configuratie van de Service. De schemaversie kunt Visual Studio, selecteer de juiste SDK's moet worden gebruikt voor schemavalidatie als meer dan één versie van de SDK is geïnstalleerd naast elkaar. Zie voor meer informatie over de compatibiliteit van het schema en versie [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md)|

Het configuratiebestand van de service moet een bevatten `ServiceConfiguration` element. De `ServiceConfiguration` element omvat mogelijk een willekeurig aantal `Role` elementen en nul of 1 `NetworkConfiguration` elementen.