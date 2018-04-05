---
title: Azure TextBox UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Common.TextBox UI-element voor Azure-portal.
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
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: daee95c177ecddfea1cf7d6d162906fcc86938b4
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox UI-element
Een besturingselement dat kan worden gebruikt voor het bewerken van tekst zonder opmaak.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
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
- Als `constraints.required` is ingesteld op **true**, en vervolgens in het tekstvak moet een waarde kan worden gevalideerd bevatten. De standaardwaarde is **false**.
- `constraints.regex` is een reguliere-expressiepatroon van JavaScript. Indien opgegeven, klikt u vervolgens de waarde van het tekstvak moet overeenkomen met het patroon kan worden gevalideerd. De standaardwaarde is **null**.
- `constraints.validationMessage` is een tekenreeks die moet worden weergegeven wanneer de waarde van het tekstvak validatie is mislukt. Als niet wordt opgegeven, wordt het tekstvak ingebouwde validatieberichten gebruikt. De standaardwaarde is **null**.
- Het is mogelijk om op te geven van een waarde voor `constraints.regex` wanneer `constraints.required` is ingesteld op **false**. In dit scenario wordt is een waarde niet vereist voor het tekstvak kan worden gevalideerd. Als er een is opgegeven, moet overeenkomen met de reguliere-expressiepatroon.

## <a name="sample-output"></a>Voorbeelduitvoer

```json
"foobar"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
