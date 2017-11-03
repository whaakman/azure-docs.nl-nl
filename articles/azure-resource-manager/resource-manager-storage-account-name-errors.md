---
title: Azure storage account name fouten | Microsoft Docs
description: Beschrijft de fouten die optreden kunnen bij het opgeven van de naam van een opslagaccount.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: dc045827fbd38054a334ff22eb30e0db6a31bac8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

### <a name="solution-1"></a>Oplossing 1

Zorg ervoor dat de opslagaccountnaam die uniek is. U kunt een unieke naam maken door het samenvoegen van de naamconventie met het resultaat van de [uniqueString](resource-group-template-functions-string.md#uniquestring) functie.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

### <a name="solution-2"></a>Oplossing 2

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

### <a name="solution-3"></a>Oplossing 3

Zorg ervoor dat de dat naam van uw opslagaccount omvat geen geen hoofdletters of speciale tekens.