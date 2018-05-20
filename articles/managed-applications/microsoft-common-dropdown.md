---
title: Azure DropDown UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Common.DropDown UI-element voor Azure-portal.
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
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: c8966c70fba1cbb17d377223ed5348075ebb7adc
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown UI-element
Een Selectiebesturingselement met een vervolgkeuzelijst.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
  "defaultValue": "my value",
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
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Het label voor `constraints.allowedValues` is de tekst weergeven voor een item en de waarde is de uitvoerwaarde van het element als geselecteerd.
- Indien opgegeven, de standaardwaarde moet een label aanwezig is in `constraints.allowedValues`. Als niet wordt opgegeven, het eerste item in `constraints.allowedValues` is geselecteerd. De standaardwaarde is **null**.
- `constraints.allowedValues` moet ten minste één item bevatten.
- Dit element biedt geen ondersteuning voor de `constraints.required` eigenschap. Om te emuleren, Voeg een item met een label en de waarde van `""` (lege tekenreeks) om te `constraints.allowedValues`.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"Bar"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
