---
title: Azure PasswordBox UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Common.PasswordBox UI-element voor Azure-portal.
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
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 944f59da680c3a058a3cd245cca48d903e44ab87
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251964"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft.Common.PasswordBox UI-element
Een besturingselement dat kan worden gebruikt om te bieden en een wachtwoord te bevestigen.

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
- Voor implementatiegegevens van `constraints`, Zie [Microsoft.Common.TextBox](microsoft-common-textbox.md).
- Als `options.hideConfirmation` is ingesteld op **waar**, het tweede tekstvak voor het bevestigen van het wachtwoord van gebruikers wordt verborgen. De standaardwaarde is **false**.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
