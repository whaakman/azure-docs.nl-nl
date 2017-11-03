---
title: Azure Cloud Services-Schema NetworkConfiguration | Microsoft Docs
ms.custom: 
ms.date: 12/07/2016
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: c1b94a9e-46e8-4a18-ac99-343c94b1d4bd
caps.latest.revision: "28"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 2438876e210363e9918e700397d4181990a3983f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-config-networkconfiguration-schema"></a>Azure Cloud Services-Config NetworkConfiguration Schema

De `NetworkConfiguration` element van het configuratiebestand van de service virtueel netwerk en DNS-waarden zijn opgegeven. Deze instellingen zijn optioneel voor cloud-services.

U kunt de volgende bronnen voor meer informatie over virtuele netwerken en de bijbehorende schema's:

- [Cloud Service (klassiek) configuratieschema](schema-cscfg-file.md)
- [Cloud-Service (klassiek) definitie Schema](schema-csdef-file.md)
- [Een virtueel netwerk (klassiek) maken](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)

## <a name="networkconfiguration-element"></a>NetworkConfiguration-Element
Het volgende voorbeeld wordt de `NetworkConfiguration` -element en de onderliggende elementen.

```xml
<ServiceConfiguration>
  <NetworkConfiguration>
    <AccessControls>
      <AccessControl name="aclName1">
        <Rule order="<rule-order>" action="<rule-action>" remoteSubnet="<subnet-address>" description="rule-description"/>
      </AccessControl>
    </AccessControls>
    <EndpointAcls>
      <EndpointAcl role="<role-name>" endpoint="<endpoint-name>" accessControl="<acl-name>"/>
    </EndpointAcls>
    <Dns>
      <DnsServers>
        <DnsServer name="<server-name>" IPAddress="<server-address>" />
      </DnsServers>
    </Dns>
    <VirtualNetworkSite name="<site-name>"/>
    <AddressAssignments>
      <InstanceAddress roleName="<role-name>">
        <Subnets>
          <Subnet name="<subnet-name>"/>
        </Subnets>
      </InstanceAddress>
      <ReservedIPs>
        <ReservedIP name="<reserved-ip-name>"/>
      </ReservedIPs>
    </AddressAssignments>
  </NetworkConfiguration>
</ServiceConfiguration>
```

De volgende tabel beschrijft de onderliggende elementen van de `NetworkConfiguration` element.

| Element       | Beschrijving |
| ------------- | ----------- |
| AccessControl | Optioneel. Hiermee geeft u de regels voor toegang tot eindpunten in een cloudservice. De naam van het besturingselement toegang wordt gedefinieerd door een tekenreeks op voor `name` kenmerk. De `AccessControl` element bevat een of meer `Rule` elementen. Meer dan één `AccessControl` element kan worden gedefinieerd.|
| Regel | Optioneel. Hiermee geeft u de actie die moet worden uitgevoerd voor een opgegeven subnet bereik van IP-adressen. De volgorde van de regel wordt gedefinieerd door een string-waarde voor de `order` kenmerk. Hoe lager nummer op voor de regel hoe hoger de prioriteit. Regels kunnen bijvoorbeeld worden opgegeven met volgnummers van 100, 200 en 300. De regel met het volgnummer van 100 heeft voorrang op de regel die de volgorde van 200.<br /><br /> De actie voor de regel wordt gedefinieerd door een tekenreeks op voor de `action` kenmerk. Mogelijke waarden zijn:<br /><br /> -   `permit`– Geeft aan dat alleen de pakketten uit het opgegeven subnet-bereik met het eindpunt communiceren kunnen.<br />-   `deny`– Geeft aan dat de toegang is geweigerd op de eindpunten in het bereik van het opgegeven subnet.<br /><br /> Het subnet bereik van IP-adressen die worden beïnvloed door de regel zijn gedefinieerd door een tekenreeks op voor de `remoteSubnet` kenmerk. De beschrijving voor de regel wordt gedefinieerd door een tekenreeks op voor de `description` kenmerk.|
| EndpointAcl | Optioneel. Hiermee geeft u de toewijzing van toegangscontroleregels naar een eindpunt. De naam van de rol met het eindpunt wordt gedefinieerd door een tekenreeks op voor de `role` kenmerk. De naam van het eindpunt wordt gedefinieerd door een tekenreeks op voor de `endpoint` kenmerk. De naam van de reeks `AccessControl` regels die moeten worden toegepast op het eindpunt zijn gedefinieerd in een tekenreeks op voor de `accessControl` kenmerk. Meer dan één `EndpointAcl` elementen kunnen worden gedefinieerd.|
| DNS-server | Optioneel. Hiermee geeft u de instellingen voor een DNS-server. U kunt instellingen opgeven voor DNS-servers zonder een virtueel netwerk. De naam van de DNS-server wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk. Het IP-adres van de DNS-server wordt gedefinieerd door een tekenreeks op voor de `IPAddress` kenmerk. Het IP-adres moet een geldig IPv4-adres.|
| VirtualNetworkSite | Optioneel. Hiermee geeft u de naam van de virtuele netwerksite die u wilt uw cloudservice implementeren. Deze instelling wordt niet gemaakt voor een virtuele netwerksite. Hierin wordt verwezen naar een site die is eerder gedefinieerd in het netwerkbestand voor het virtuele netwerk. Een cloudservice kan alleen worden lid van een virtueel netwerk. Als u deze instelling niet opgeeft, wordt de cloudservice niet worden geïmplementeerd met een virtueel netwerk. De naam van de virtuele netwerksite wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk.|
| InstanceAddress | Optioneel. Hiermee geeft u de koppeling van een rol met een subnet of reeks subnetten in het virtuele netwerk. Als u een rolnaam op in een adres exemplaar koppelt, kunt u de subnetten waarnaar deze rol zijn gekoppeld. De `InstanceAddress` bevat een element subnetten. De naam van de rol die is gekoppeld aan het subnet of subnetten wordt gedefinieerd door een tekenreeks op voor de `roleName` kenmerk.|
| Subnet | Optioneel. Hiermee geeft u het subnet dat overeenkomt met de subnetnaam in het configuratiebestand van het netwerk. De naam van het subnet wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk.|
| ReservedIP | Optioneel. Hiermee geeft u het gereserveerde IP-adres dat aan de implementatie gekoppeld worden moet. Gereserveerd IP-adres maken moet u het gereserveerde IP-adres maken. Elke implementatie in een cloudservice kan worden gekoppeld aan een gereserveerd IP-adres. De naam van het gereserveerde IP-adres wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk.|

## <a name="see-also"></a>Zie ook
[Cloud Service (klassiek) configuratieschema](schema-cscfg-file.md)