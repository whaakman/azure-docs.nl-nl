---
title: Azure Cloud Services Def. NetworkTrafficRules Schema | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 351b369f-365e-46c1-82ce-03fc3655cc88
caps.latest.revision: 17
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 8925943b0a5d151d55adedcfe3f01b5a14c63c1b
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821677"
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Azure Cloud Services Definition NetworkTrafficRules Schema
De `NetworkTrafficRules` knooppunt is een optioneel element in het servicedefinitiebestand waarmee wordt aangegeven hoe de functies met elkaar communiceren. Hiermee beperkt u welke rollen hebben toegang tot de interne eindpunten van de specifieke rol. De `NetworkTrafficRules` is geen zelfstandige element; deze wordt gecombineerd met twee of meer rollen in een service-definitiebestand.

De standaardextensie voor het servicedefinitiebestand is .csdef.

> [!NOTE]
>  De `NetworkTrafficRules` -knooppunt is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Definitieschema voor Basic-service voor de regels voor netwerkverkeer
De basisindeling van een service-definitiebestand met verkeer netwerkdefinities is als volgt.

```xml
<ServiceDefinition …>
   <NetworkTrafficRules>
      <OnlyAllowTrafficTo>
         <Destinations>
            <RoleEndpoint endpointName="<name-of-the-endpoint>" roleName="<name-of-the-role-containing-the-endpoint>"/>
         </Destinations>
         <AllowAllTraffic/>
         <WhenSource matches="[AnyRule]">
            <FromRole roleName="<name-of-the-role-to-allow-traffic-from>"/>
         </WhenSource>
      </OnlyAllowTrafficTo>
   </NetworkTrafficRules>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Schema-elementen
De `NetworkTrafficRules` knooppunt van het servicedefinitiebestand bevat deze elementen in detail in de volgende secties in dit onderwerp beschreven:

[NetworkTrafficRules Element](#NetworkTrafficRules)

[OnlyAllowTrafficTo Element](#OnlyAllowTrafficTo)

[Element bestemmingen](#Destinations)

[RoleEndpoint Element](#RoleEndpoint)

AllowAllTraffic Element

[WhenSource-Element](#WhenSource)

[FromRole-Element](#FromRole)

##  <a name="NetworkTrafficRules"></a> NetworkTrafficRules-Element
De `NetworkTrafficRules` element geeft aan welke functies kunnen communiceren met welk eindpunt op een andere rol. Een service mag een `NetworkTrafficRules` definitie.

##  <a name="OnlyAllowTrafficTo"></a> OnlyAllowTrafficTo Element
De `OnlyAllowTrafficTo` element beschrijft een aantal eindpunten van de bestemming en de functies die ermee kunnen communiceren. Kunt u meerdere `OnlyAllowTrafficTo` knooppunten.

##  <a name="Destinations"></a> Element bestemmingen
De `Destinations` -element worden dan kan worden gecommuniceerd met een verzameling van RoleEndpoints beschreven.

##  <a name="RoleEndpoint"></a> RoleEndpoint-Element
De `RoleEndpoint` element een eindpunt op een rol om communicatie met beschrijft. Kunt u meerdere `RoleEndpoint` elementen als er meer dan één eindpunt op de rol.

| Kenmerk      | Type     | Description |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Vereist. De naam van het eindpunt op het verkeer toestaan.|
| `roleName`     | `string` | Vereist. De naam van de Webrol voor de communicatie op.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic Element
De `AllowAllTraffic` -element is een regel waarmee alle rollen om te communiceren met de eindpunten zijn gedefinieerd de `Destinations` knooppunt.

##  <a name="WhenSource"></a> WhenSource-Element
De `WhenSource` element beschrijft een reeks functies dan kan communiceren met de eindpunten zijn gedefinieerd de `Destinations` knooppunt.

| Kenmerk | Type     | Description |
| --------- | -------- | ----------- |
| `matches` | `string` | Vereist. Hiermee geeft u de regel wilt toepassen wanneer communicatie toestaan. De enige geldige waarde is momenteel `AnyRule`.|
  
##  <a name="FromRole"></a> FromRole-Element
De `FromRole` element Hiermee geeft u de rollen die met de eindpunten zijn gedefinieerd communiceren kunnen de `Destinations` knooppunt. Kunt u meerdere `FromRole` elementen als er meer dan één rol die met de eindpunten communiceren kan.

| Kenmerk  | Type     | Description |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Vereist. De naam voor de rol van waaruit u wilt toestaan.|

## <a name="see-also"></a>Zie ook
[(Klassiek) Definitieschema voor cloud Service](schema-csdef-file.md)
