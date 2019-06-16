---
title: Azure TextBox UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Common.TextBox UI-element voor Azure-portal.
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
ms.openlocfilehash: b06e8b49efe8b6de720fa9bb819d4720e4f80fb6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61044528"
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox UI-element
Een besturingselement dat kan worden gebruikt om niet-opgemaakte tekst te bewerken.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Example text box 1",
  "defaultValue": "my text value",
  "toolTip": "Use only allowed characters",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Als `constraints.required` is ingesteld op **waar**, en vervolgens in het tekstvak moet een waarde die moet worden gevalideerd. De standaardwaarde is **false**.
- `constraints.regex` is een reguliere-expressiepatroon van JavaScript. Als u opgeeft, klikt u vervolgens het tekstvak waarde moet overeenkomen met het patroon worden gevalideerd. De standaardwaarde is **null**.
- `constraints.validationMessage` is een tekenreeks die moet worden weergegeven wanneer de waarde van het tekstvak niet kan worden gevalideerd. Als niet is opgegeven, wordt het tekstvak ingebouwde validatieberichten gebruikt. De standaardwaarde is **null**.
- Het is mogelijk om op te geven van een waarde voor `constraints.regex` wanneer `constraints.required` is ingesteld op **false**. In dit scenario is een waarde niet vereist voor het tekstvak worden gevalideerd. Als er een is opgegeven, moet deze overeenkomen met de reguliere-expressiepatroon.

## <a name="sample-output"></a>Voorbeelduitvoer

```json
"my text value"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
