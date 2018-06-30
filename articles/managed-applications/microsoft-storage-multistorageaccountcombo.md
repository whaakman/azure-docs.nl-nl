---
title: Azure MultiStorageAccountCombo UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Storage.MultiStorageAccountCombo UI-element voor Azure-portal.
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
ms.openlocfilehash: f5fa81d53e1728e8f566a2a39aed8311828b20c7
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37108702"
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft.Storage.MultiStorageAccountCombo UI-element
Een groep besturingselementen voor het maken van verschillende opslagaccounts met namen die met een algemeen voorvoegsel beginnen.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- De waarde voor `defaultValue.prefix` is samengevoegd met een of meer getallen voor het genereren van de volgorde van de namen van opslagaccounts. Bijvoorbeeld, als `defaultValue.prefix` is **sa** en `count` is **2**, klikt u vervolgens opslagaccountnamen **sa1** en **sa2** worden gegenereerd. Gegenereerde opslagaccountnamen worden gevalideerd voor uniekheid automatisch.
- De namen van opslagaccounts zijn gegenereerd lexicographically op basis van `count`. Bijvoorbeeld, als `count` 10 is, wordt de namen van opslagaccounts eindigen met twee cijfers gehele getallen (01, 02, 03).
- De standaardwaarde voor `defaultValue.prefix` is **null**, en voor `defaultValue.type` is **Premium_LRS**.
- Een type dat niet is opgegeven in `constraints.allowedTypes` is verborgen, en een type dat niet is opgegeven in `constraints.excludedTypes` wordt weergegeven. `constraints.allowedTypes` en `constraints.excludedTypes` zijn beide optioneel, maar niet gelijktijdig worden gebruikt.
- Naast het genereren van de namen van opslagaccounts, `count` wordt gebruikt voor het instellen van de juiste vermenigvuldiger voor het element. Een statische waarde, zoals ondersteunt **2**, of een dynamische waarde van een ander element, zoals `[steps('step1').storageAccountCount]`. De standaardwaarde is **1**.

## <a name="sample-output"></a>Voorbeelduitvoer

```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
