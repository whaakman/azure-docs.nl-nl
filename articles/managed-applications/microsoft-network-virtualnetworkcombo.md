---
title: Azure VirtualNetworkCombo UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Network.VirtualNetworkCombo UI-element voor Azure-portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 2c2553d9ffb1dfbe032385fb77e234a8b96cb239
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110062"
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft.Network.VirtualNetworkCombo UI-element
Een groep besturingselementen voor het selecteren van een nieuw of bestaand virtueel netwerk.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
Wanneer de gebruiker kiest een nieuw virtueel netwerk, kan de gebruiker de naam van elk subnet en adresvoorvoegsel aanpassen. Subnetten configureren is optioneel.

![Nieuwe Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-new.png)

Wanneer de gebruiker kiest een bestaand virtueel netwerk, moet de gebruiker elk subnet dat is vereist voor de implementatiesjabloon toewijzen aan een bestaand subnet. Subnetten configureren is in dit geval vereist.

![Bestaande Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-existing.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Als opgegeven, voorvoegsel van de grootte van de eerste niet-overlappende adres `defaultValue.addressPrefixSize` automatisch op basis van de bestaande virtuele netwerken in het abonnement van de gebruiker wordt bepaald.
- De standaardwaarde voor `defaultValue.name` en `defaultValue.addressPrefixSize` is **null**.
- `constraints.minAddressPrefixSize` moet worden opgegeven. Een bestaande virtuele netwerken met een adresruimte die kleiner is dan de opgegeven waarde zijn niet beschikbaar voor selectie.
- `subnets` Er moet worden opgegeven en `constraints.minAddressPrefixSize` voor elk subnet moet worden opgegeven.
- Wanneer u een nieuw virtueel netwerk maakt, adresvoorvoegsel voor elk subnet wordt automatisch berekend op het virtuele netwerk adresvoorvoegsel en de respectieve `addressPrefixSize`.
- Wanneer u een bestaande virtuele netwerk, kleiner is dan de respectieve subnetten `constraints.minAddressPrefixSize` zijn niet beschikbaar voor selectie. Bovendien, indien opgegeven, subnetten waarvoor geen ten minste `minAddressCount` beschikbare adressen zijn niet beschikbaar voor selectie. De standaardwaarde is **0**. Om ervoor te zorgen dat de beschikbare adressen aaneengesloten zijn, geef **true** voor `requireContiguousAddresses`. De standaardwaarde is **true**.
- Maken van subnetten in een bestaand virtueel netwerk wordt niet ondersteund.
- Als `options.hideExisting` is **true**, de gebruiker een bestaand virtueel netwerk niet kiezen. De standaardwaarde is **false**.

## <a name="sample-output"></a>Voorbeelduitvoer

```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
