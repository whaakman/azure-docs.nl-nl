---
title: Azure MultiStorageAccountCombo UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Storage.MultiStorageAccountCombo UI-element voor Azure-portal.
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
ms.openlocfilehash: 08b65770414e9ee1cb5e478427fe7654b2bb9a78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60252427"
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft.Storage.MultiStorageAccountCombo UI element
Een groep van besturingselementen voor het maken van verschillende opslagaccounts met namen die met een algemene voorvoegsel beginnen.

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
- De waarde voor `defaultValue.prefix` is samengevoegd met een of meer gehele getallen voor het genereren van de volgorde van de namen van opslagaccounts. Bijvoorbeeld, als `defaultValue.prefix` is **sa** en `count` is **2**, klikt u vervolgens opslagaccountnamen **sa1** en **sa2** worden gegenereerd. Gegenereerde opslagaccountnamen zijn gevalideerd voor uniekheid automatisch.
- De namen van opslagaccounts worden gegenereerd lexicographically op basis van `count`. Bijvoorbeeld, als `count` is 10 en vervolgens de namen van opslagaccounts eindigen met twee cijfers gehele getallen (01, 02, 03).
- De standaardwaarde voor `defaultValue.prefix` is **null**, en voor `defaultValue.type` is **Premium_LRS**.
- Elk type niet opgegeven in `constraints.allowedTypes` verborgen is, en elk type niet opgegeven in `constraints.excludedTypes` wordt weergegeven. `constraints.allowedTypes` en `constraints.excludedTypes` zijn beide optioneel, maar kunnen niet gelijktijdig worden gebruikt.
- Naast het genereren van namen van opslagaccounts, `count` wordt gebruikt voor het instellen van de juiste vermenigvuldiger voor het element. Azureondersteunt een statische waarde, zoals **2**, of een dynamische waarde van een ander element, zoals `[steps('step1').storageAccountCount]`. De standaardwaarde is **1**.

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
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
