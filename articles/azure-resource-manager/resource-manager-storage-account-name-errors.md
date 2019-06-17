---
title: Azure-opslag account naam fouten | Microsoft Docs
description: Beschrijving van de fouten die optreden kunnen bij het opgeven van een storage-accountnaam.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: c3d4d764b1076c8705cfa64d6c0b38e3b8c1a801
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64716387"
---
# <a name="resolve-errors-for-storage-account-names"></a>Los fouten voor de namen van opslagaccounts

Dit artikel wordt beschreven naming fouten die optreden kunnen bij het implementeren van een storage-account.

## <a name="symptom"></a>Symptoom

Als de naam van uw opslagaccount niet-toegestane tekens bevat, ontvangt u een foutbericht wordt weergegeven als:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

U moet een naam voor de resource die uniek is binnen Azure opgeven voor storage-accounts. Als u een unieke naam opgeeft, ontvangt u een foutbericht wordt weergegeven als:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Als u een opslagaccount met dezelfde naam als een bestaand opslagaccount in uw abonnement implementeren, maar een andere locatie bieden, ontvangt u een foutbericht met dat de storage-account bestaat al in een andere locatie. Verwijder het bestaande storage-account of geef dezelfde locatie als het bestaande storage-account.

## <a name="cause"></a>Oorzaak

Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en alleen cijfers en kleine letters gebruiken. De naam moet uniek zijn.

## <a name="solution"></a>Oplossing

Zorg ervoor dat de dat naam van het opslagaccount uniek is. U kunt een unieke naam maken door het samenvoegen van uw naamconventie met het resultaat van de [uniqueString](resource-group-template-functions-string.md#uniquestring) functie.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Zorg ervoor dat de dat naam van uw opslagaccount niet langer zijn dan 24 tekens bestaan. De [uniqueString](resource-group-template-functions-string.md#uniquestring) functie 13 tekens retourneert. Als u samenvoegen van een voorvoegsel of achtervoegsel aan de **uniqueString** leiden, Geef een waarde die is 11 tekens of minder.

```json
"parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    }
}
```

Zorg ervoor dat de dat naam van uw opslagaccount bevat geen geen hoofdletters of speciale tekens.