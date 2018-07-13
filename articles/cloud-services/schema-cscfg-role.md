---
title: Azure Cloud Services-Rolschema | Microsoft Docs
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: e4fbffc1-98eb-449c-971c-de415e45ab34
caps.latest.revision: 12
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 20f4186426152d2dc9b445981a69881c35587eb6
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005778"
---
# <a name="azure-cloud-services-config-role-schema"></a>Azure Cloud Services-rol Config-Schema

De `Role` element van het configuratiebestand geeft het aantal rolinstanties implementeren voor elke rol in de service, de waarden van alle configuratie-instellingen, en de vingerafdrukken instellen voor alle certificaten die zijn gekoppeld aan een rol.

Zie voor meer informatie over het configuratieschema van de Azure-Service, [configuratieschema voor Cloud-Service (klassiek)](schema-cscfg-file.md). Zie voor meer informatie over de definitie van Schema van de Azure-Service, [Definitieschema voor Cloud-Service (klassiek)](schema-csdef-file.md).

##  <a name="Role"></a> Rol-Element
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
| Instelling   | Optioneel. Hiermee geeft u een naam van de instelling en waarde in een verzameling instellingen voor een rol. Naam van de instelling wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk en de waarde van de instelling wordt gedefinieerd door een tekenreeks op voor de `value` kenmerk.|
| Certificaat | Optioneel. Hiermee geeft u de naam, de vingerafdruk en het algoritme van het certificaat van een service die moet worden gekoppeld aan de rol. Naam van het certificaat wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk. Vingerafdruk van het certificaat wordt gedefinieerd door een reeks hexadecimale cijfers die bevat geen spaties bevatten voor de `thumbprint` kenmerk. De hexadecimale cijfers moet worden weergegeven met cijfers en alfanumerieke tekens in hoofdletters. De certificaat-algoritme wordt gedefinieerd door een tekenreeks op voor de `thumbprintAlgorithm` kenmerk.|

## <a name="see-also"></a>Zie ook
[Schema van de cloud Service (klassiek)-configuratie](schema-cscfg-file.md)