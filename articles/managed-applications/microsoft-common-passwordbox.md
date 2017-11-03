---
title: Azure beheerde toepassing PasswordBox UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Common.PasswordBox UI-element voor beheerde Azure-toepassingen
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 6d9f2b7cf56375d3a609cff20e928307c13bf2b8
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft.Common.PasswordBox UI-element
Een besturingselement dat kan worden gebruikt om te bieden en bevestig een wachtwoord. Gebruik van dit element wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Dit element biedt geen ondersteuning voor de `defaultValue` eigenschap.
- Implementatiedetails van de voor `constraints`, Zie [Microsoft.Common.TextBox](microsoft-common-textbox.md).
- Als `options.hideConfirmation` is ingesteld op **true**, het tweede tekstvak voor het bevestigen van het wachtwoord van de gebruiker wordt verborgen. De standaardwaarde is **false**.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
