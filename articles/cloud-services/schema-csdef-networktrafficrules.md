---
title: Azure Cloud Services-naam Def. NetworkTrafficRules Schema | Microsoft Docs
ms.custom: 
ms.date: 04/14/2015
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 351b369f-365e-46c1-82ce-03fc3655cc88
caps.latest.revision: "17"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 37ec6c771ad83be680d95e9b5597d8da3cace9ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Azure Cloud Services-definitie NetworkTrafficRules Schema
De `NetworkTrafficRules` knooppunt is een optioneel element in het servicedefinitiebestand waarmee wordt aangegeven hoe de functies met elkaar communiceren. Deze wordt beperkt welke rollen u toegang tot de interne eindpunten van de specifieke rol. De `NetworkTrafficRules` is geen zelfstandige element; deze wordt gecombineerd met twee of meer rollen in een servicedefinitiebestand.

De standaardextensie voor het servicedefinitiebestand is csdef.

> [!NOTE]
>  De `NetworkTrafficRules` knooppunt is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Basic-service definitie schema voor de regels voor netwerkverkeer
De basisindeling van een servicedefinitiebestand met netwerkdefinities verkeer is als volgt.

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
De `NetworkTrafficRules` knooppunt van het servicedefinitiebestand bevat deze elementen, uitgebreid beschreven in de volgende secties in dit onderwerp:

[NetworkTrafficRules Element](#NetworkTrafficRules)

[OnlyAllowTrafficTo Element](#OnlyAllowTrafficTo)

[Bestemmingen Element](#Destinations)

[RoleEndpoint Element](#RoleEndpoint)

[AllowAllTraffic Element](#AllowAllTraffic)

[WhenSource Element](#WhenSource)

[FromRole Element](#FromRole)

##  <a name="NetworkTrafficRules"></a>NetworkTrafficRules Element
De `NetworkTrafficRules` element geeft aan welke rollen kunnen communiceren met welk eindpunt op een andere rol. Een service kan een bevatten `NetworkTrafficRules` definitie.

##  <a name="OnlyAllowTrafficTo"></a>OnlyAllowTrafficTo Element
De `OnlyAllowTrafficTo` -element worden beschreven van een verzameling van bestemming eindpunten en de functies die ermee kunnen communiceren. U kunt meerdere opgeven `OnlyAllowTrafficTo` knooppunten.

##  <a name="Destinations"></a>Bestemmingen Element
De `Destinations` -element worden dan kan worden gecommuniceerd met een verzameling van RoleEndpoints beschreven.

##  <a name="RoleEndpoint"></a>RoleEndpoint Element
De `RoleEndpoint` element een eindpunt op een functie waarmee de communicatie met beschrijft. U kunt meerdere opgeven `RoleEndpoint` elementen als er meer dan één eindpunt op de rol.

| Kenmerk      | Type     | Beschrijving |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Vereist. De naam van het eindpunt waarmee verkeer naar.|
| `roleName`     | `string` | Vereist. De naam van de Webrol in de communicatie met toestaan.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic Element
De `AllowAllTraffic` -element is een regel waarmee alle functies om te communiceren met de eindpunten zijn gedefinieerd de `Destinations` knooppunt.

##  <a name="WhenSource"></a>WhenSource Element
De `WhenSource` element wordt een verzameling functies beschreven, dan kan communiceren met de eindpunten zijn gedefinieerd de `Destinations` knooppunt.

| Kenmerk | Type     | Beschrijving |
| --------- | -------- | ----------- |
| `matches` | `string` | Vereist. Hiermee geeft u de regel wilt toepassen wanneer u communicatie toestaat. De enige geldige waarde is momenteel `AnyRule`.|
  
##  <a name="FromRole"></a>FromRole Element
De `FromRole` element geeft de rollen die met de eindpunten zijn gedefinieerd communiceren kunnen de `Destinations` knooppunt. U kunt meerdere opgeven `FromRole` elementen als er meer dan één rol die met de eindpunten communiceren kan.

| Kenmerk  | Type     | Beschrijving |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Vereist. De naam voor de rol waaruit u wilt toestaan.|

## <a name="see-also"></a>Zie ook
[Cloud-Service (klassiek) definitie Schema](schema-csdef-file.md)