---
title: Azure Cloud Services-rol Schema | Microsoft Docs
ms.custom: 
ms.date: 12/07/2016
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: e4fbffc1-98eb-449c-971c-de415e45ab34
caps.latest.revision: "12"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: c25e7d139c7b7fd7c5da6bde8cfb9050eec8a88e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-config-role-schema"></a>Azure Cloud Services-Schema van de rol van de configuratie

De `Role` element in het configuratiebestand geeft het aantal rolinstanties implementeren voor elke rol in de service, de waarden van configuratie-instellingen en de vingerafdrukken instellen voor alle certificaten die zijn gekoppeld aan een rol.

Zie voor meer informatie over het configuratieschema van Azure Service [Cloudservice (klassiek) configuratieschema](schema-cscfg-file.md). Zie voor meer informatie over het Schema van Azure Service definitie [Cloudservice (klassiek) definitie Schema](schema-csdef-file.md).

##  <a name="Role"></a>Element Role
Het volgende voorbeeld wordt de `Role` -element en de onderliggende elementen.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

De volgende tabel beschrijft de kenmerken voor de `Role` element.

| Kenmerk | Beschrijving |
| --------- | ----------- |
| naam   | Vereist. Hiermee geeft u de naam van de rol. De naam moet overeenkomen met de naam opgegeven voor de rol in het servicedefinitiebestand.|
| vmName | Optioneel. Hiermee geeft u de DNS-naam voor een virtuele Machine. De naam moet uit 10 tekens of minder.|

De volgende tabel beschrijft de onderliggende elementen van de `Role` element.

| Element | Beschrijving |
| ------- | ----------- |
| Exemplaren | Vereist. Hiermee geeft u het aantal exemplaren moeten worden geïmplementeerd voor de rol. Het aantal exemplaren wordt gedefinieerd door een geheel getal voor de `count` kenmerk.|
| Instelling   | Optioneel. Hiermee geeft u een naam van de instelling en waarde in een verzameling instellingen voor een rol. Naam van de instelling wordt gedefinieerd door een tekenreeks op voor de `name` -kenmerk en de waarde van de instelling wordt gedefinieerd door een tekenreeks op voor de `value` kenmerk.|
| Certificaat | Optioneel. Hiermee geeft u de naam, de vingerafdruk en het algoritme van een certificaat dat moet worden gekoppeld aan de rol. Naam van het certificaat wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk. Vingerafdruk van het certificaat wordt gedefinieerd door een reeks hexadecimale cijfers zonder spaties voor de `thumbprint` kenmerk. De hexadecimale cijfers moet worden weergegeven met cijfers en alfanumerieke tekens in hoofdletters. De algoritme van het certificaat wordt gedefinieerd door een tekenreeks op voor de `thumbprintAlgorithm` kenmerk.|

## <a name="see-also"></a>Zie ook
[Cloud Service (klassiek) configuratieschema](schema-cscfg-file.md)