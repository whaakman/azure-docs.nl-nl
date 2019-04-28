---
title: Azure StorageAccountSelector UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Storage.StorageAccountSelector UI-element voor Azure-portal.
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
ms.openlocfilehash: c6d4ef50645902aecd57ceb9fc48b7d99bf22d53
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104853"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Microsoft.Storage.StorageAccountSelector UI element
Een besturingselement voor het selecteren van een nieuwe of bestaande storage-account.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface

Het besturingselement toont de standaardwaarde.

![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

Het besturingselement kan de gebruiker een nieuw opslagaccount maken of een bestaand opslagaccount selecteren.

![Nieuwe Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector-new.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Als u opgeeft, `defaultValue.name` wordt automatisch gevalideerd voor uniekheid. Als naam van het opslagaccount niet uniek is, moet de gebruiker een andere naam geven of een bestaand opslagaccount kiezen.
- De standaardwaarde voor `defaultValue.type` is **Premium_LRS**.
- Elk type niet opgegeven in `constraints.allowedTypes` verborgen is, en elk type niet opgegeven in `constraints.excludedTypes` wordt weergegeven. `constraints.allowedTypes` en `constraints.excludedTypes` zijn beide optioneel, maar kunnen niet gelijktijdig worden gebruikt.
- Als `options.hideExisting` is **waar**, de gebruiker een bestaand opslagaccount niet kiezen. De standaardwaarde is **false**.

## <a name="sample-output"></a>Voorbeelduitvoer

```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
