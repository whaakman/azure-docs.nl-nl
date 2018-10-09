---
title: Azure PublicIpAddressCombo UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Network.PublicIpAddressCombo UI-element voor Azure-portal.
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
ms.openlocfilehash: c3e8c99f6648f0f4927140f3215978566afb9eb8
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868901"
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo UI-element
Een groep van besturingselementen voor het selecteren van een nieuwe of bestaande openbare IP-adres.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Als de gebruiker geen voor de openbare IP-adres selecteert, is het tekstvak label domein verborgen.
- Als de gebruiker een bestaande openbare IP-adres selecteert, is het tekstvak domein label uitgeschakeld. De waarde is het domeinnaamlabel van het geselecteerde IP-adres.
- Het domein naam (bijvoorbeeld westus.cloudapp.azure.com)-achtervoegsel updates automatisch op basis van de geselecteerde locatie.

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false,
    "zone": 3
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Als `constraints.required.domainNameLabel` is ingesteld op **waar**, moet de gebruiker een domeinnaamlabel opgeven bij het maken van een nieuw openbaar IP-adres. Bestaande openbare IP-adressen zonder een label zijn niet beschikbaar voor selectie.
- Als `options.hideNone` is ingesteld op **waar**, klikt u vervolgens de optie te selecteren **geen** voor het openbare IP-adres is verborgen. De standaardwaarde is **false**.
- Als `options.hideDomainNameLabel` is ingesteld op **waar**, en vervolgens in het tekstvak voor domeinnaamlabel is verborgen. De standaardwaarde is **false**.
- Als `options.hideExisting` is ingesteld op true, wordt de gebruiker kan geen tot een bestaande openbare IP-adres kiezen. De standaardwaarde is **false**.
- Voor `zone`, alleen openbare IP-adressen voor de opgegeven zone of zone tegen openbare IP-adressen beschikbaar zijn.

## <a name="sample-output"></a>Voorbeelduitvoer
Als de gebruiker geen openbaar IP-adres selecteert, retourneert het besturingselement in de volgende uitvoer:

```json
{
  "newOrExistingOrNone": "none"
}
```

Als de gebruiker een nieuwe of bestaande IP-adres selecteert, retourneert het besturingselement in de volgende uitvoer:

```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "sku": "Basic",
  "newOrExistingOrNone": "new"
}
```

- Wanneer `options.hideNone` is opgegeven als **waar**, `newOrExistingOrNone` heeft alleen een waarde van **nieuwe** of **bestaande**.
- Wanneer `options.hideDomainNameLabel` is opgegeven als **waar**, `domainNameLabel` is niet gedeclareerd.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
