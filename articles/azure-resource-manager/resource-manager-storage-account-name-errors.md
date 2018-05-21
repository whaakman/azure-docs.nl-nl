---
title: Azure storage account name fouten | Microsoft Docs
description: Beschrijft de fouten die optreden kunnen bij het opgeven van de naam van een opslagaccount.
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
ms.openlocfilehash: da7147439bba668c6c614c19d91a22ee78275c69
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="resolve-errors-for-storage-account-names"></a>Los de fouten voor de namen van opslagaccounts

Dit artikel wordt beschreven fouten optreden kunnen bij het implementeren van een opslagaccount in namen.

## <a name="symptom"></a>Symptoom

Als de naam van uw opslag niet-toegestane tekens bevat, foutbericht er een zoals:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

U moet een naam voor de resource die uniek is voor alle Azure opgeven voor de storage-accounts. Als u niet een unieke naam opgeeft, wordt een fout als volgt:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Als u een opslagaccount met dezelfde naam als een bestaand opslagaccount in uw abonnement implementeren, maar een andere locatie opgeven, ontvangt u een foutbericht retourneren over dat de storage-account bestaat al in een andere locatie. Verwijder de bestaande opslagaccount of geef dezelfde locatie als het bestaande opslagaccount.

## <a name="cause"></a>Oorzaak

Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en alleen cijfers en kleine letters gebruiken. De naam moet uniek zijn.

## <a name="solution"></a>Oplossing

Zorg ervoor dat de opslagaccountnaam die uniek is. U kunt een unieke naam maken door het samenvoegen van de naamconventie met het resultaat van de [uniqueString](resource-group-template-functions-string.md#uniquestring) functie.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Zorg ervoor dat de dat naam van het opslagaccount niet meer dan 24 tekens bestaan. De [uniqueString](resource-group-template-functions-string.md#uniquestring) functie retourneert 13 tekens. Als u een voorvoegsel samenvoegen of postfix naar de **uniqueString** leiden, Geef een waarde 11 tekens of minder.

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

Zorg ervoor dat de dat naam van uw opslagaccount omvat geen geen hoofdletters of speciale tekens.