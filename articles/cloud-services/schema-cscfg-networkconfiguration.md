---
title: Azure Cloud Services-Schema Primary | Microsoft Docs
ms.custom: ''
origin.date: 12/07/2016
ms.date: 11/06/2017
ms.prod: azure
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: c1b94a9e-46e8-4a18-ac99-343c94b1d4bd
caps.latest.revision: 28
author: thraka
ms.author: v-yiso
manager: timlt
ms.openlocfilehash: fb833904502c0c42b46201fd46a368de0376277c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130261"
---
# <a name="azure-cloud-services-config-networkconfiguration-schema"></a>Azure Cloud Services Config NetworkConfiguration Schema

De `NetworkConfiguration` element van het serviceconfiguratiebestand Hiermee geeft u waarden voor Virtueelnetwerk en DNS. Deze instellingen zijn optioneel voor cloudservices.

U kunt de volgende bronnen voor meer informatie over virtuele netwerken en de bijbehorende schema's:

- [Schema van de cloud Service (klassiek)-configuratie](schema-cscfg-file.md)
- [(Klassiek) Definitieschema voor cloud Service](schema-csdef-file.md)
- [Een Virtueelnetwerk (klassiek) maken](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)

## <a name="networkconfiguration-element"></a>NetworkConfiguration Element
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

| Element       | Description |
| ------------- | ----------- |
| AccessControl | Optioneel. Hiermee geeft u de regels voor toegang tot eindpunten in een cloudservice. De naam van de access control wordt gedefinieerd door een tekenreeks op voor `name` kenmerk. De `AccessControl` element bevat een of meer `Rule` elementen. Meer dan één `AccessControl` element kan worden gedefinieerd.|
| Regel | Optioneel. Hiermee geeft u de actie die moet worden uitgevoerd voor een opgegeven subnet bereik van IP-adressen. De volgorde van de regel wordt gedefinieerd door een string-waarde voor de `order` kenmerk. Hoe lager nummer op voor de regel hoe hoger de prioriteit. Regels kunnen bijvoorbeeld worden opgegeven met volgnummers van 100, 200 en 300. De regel met het volgnummer van 100 heeft voorrang op de regel die een order van 200.<br /><br /> De actie voor de regel wordt gedefinieerd door een tekenreeks op voor de `action` kenmerk. Mogelijke waarden zijn:<br /><br /> -   `permit` – Geeft aan dat alleen pakketten uit het opgegeven subnet-bereik met het eindpunt communiceren kunnen.<br />-   `deny` : Hiermee geeft u dat de toegang is geweigerd met de eindpunten in het opgegeven subnet-bereik.<br /><br /> Het subnet bereik van IP-adressen die worden beïnvloed door de regel zijn gedefinieerd door een tekenreeks op voor de `remoteSubnet` kenmerk. De beschrijving voor de regel wordt gedefinieerd door een tekenreeks op voor de `description` kenmerk.|
| EndpointAcl | Optioneel. Hiermee geeft u de toewijzing van de regels van de access control naar een eindpunt. De naam van de rol met het eindpunt is gedefinieerd door een tekenreeks voor de `role` kenmerk. De naam van het eindpunt wordt gedefinieerd door een tekenreeks op voor de `endpoint` kenmerk. De naam van de set `AccessControl` regels die moeten worden toegepast op het eindpunt worden gedefinieerd in een tekenreeks op voor de `accessControl` kenmerk. Meer dan één `EndpointAcl` elementen kunnen worden gedefinieerd.|
| DnsServer | Optioneel. Hiermee geeft u de instellingen voor een DNS-server. U kunt instellingen opgeven voor DNS-servers zonder een Virtueelnetwerk. De naam van de DNS-server wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk. Het IP-adres van de DNS-server wordt gedefinieerd door een tekenreeks op voor de `IPAddress` kenmerk. Het IP-adres moet een geldig IPv4-adres.|
| VirtualNetworkSite | Optioneel. Hiermee geeft u de naam van de virtuele netwerksite die u wilt uw service in de cloud implementeren. Deze instelling maakt geen een virtuele netwerksite. Hierin wordt verwezen naar een site die is eerder gedefinieerd in het netwerkbestand voor het Virtueelnetwerk. Een service in de cloud kan alleen worden voor een lid van een Virtueelnetwerk. Als u deze instelling niet opgeeft, wordt de cloudservice niet worden geïmplementeerd op een Virtueelnetwerk. De naam van de virtuele netwerksite wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk.|
| InstanceAddress | Optioneel. Hiermee geeft u de koppeling van een rol aan een subnet of een set van subnetten in het Virtueelnetwerk. Als u de naam van een rol in een adres exemplaar koppelt, kunt u de subnetten die u wilt dat deze rol moet worden gekoppeld. De `InstanceAddress` bevat een element subnetten. De naam van de rol die is gekoppeld aan het subnet of subnetten wordt gedefinieerd door een tekenreeks op voor de `roleName` kenmerk.|
| Subnet | Optioneel. Hiermee geeft u het subnet dat overeenkomt met de naam van het subnet in het configuratiebestand van het netwerk. De naam van het subnet wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk.|
| ReservedIP | Optioneel. Hiermee geeft u het gereserveerde IP-adres dat gekoppeld aan de implementatie worden moet. Gereserveerde IP-adres maken moet u het gereserveerde IP-adres maken. Elke implementatie in een cloudservice kan worden gekoppeld aan een gereserveerde IP-adres. De naam van het gereserveerde IP-adres wordt gedefinieerd door een tekenreeks op voor de `name` kenmerk.|

## <a name="see-also"></a>Zie ook
[Schema van de cloud Service (klassiek)-configuratie](schema-cscfg-file.md)