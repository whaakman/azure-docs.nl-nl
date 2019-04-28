---
title: Azure DropDown UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Common.DropDown UI-element voor Azure-portal.
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
ms.openlocfilehash: e78fa419b067c0bad48229dcfd8d4e986fc16903
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62117298"
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown UI element
Selectiebesturingselement voor een met een vervolgkeuzelijst weergegeven.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen

- Het label voor `constraints.allowedValues` is de tekst weergeven voor een item en de waarde is de uitvoerwaarde van het element als geselecteerd.
- Als u opgeeft, moet de standaardwaarde een label aanwezig zijn in `constraints.allowedValues`. Indien niet opgegeven, het eerste item in `constraints.allowedValues` is geselecteerd. De standaardwaarde is **null**.
- `constraints.allowedValues` beschikken over ten minste één item.
- Om te emuleren een waarde niet vereist, Voeg een item met een label en de waarde van `""` (een lege tekenreeks) op `constraints.allowedValues`.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"two"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
