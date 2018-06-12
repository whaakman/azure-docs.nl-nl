---
title: Azure OptionsGroup UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Common.OptionsGroup UI-element voor Azure-portal.
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
ms.openlocfilehash: 2e0b448b5ab48e7be3429d3d3b5b898b6bf22115
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261845"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft.Common.OptionsGroup UI-element
Een Selectiebesturingselement met een rij van de beschikbare opties.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
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
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Het label voor `constraints.allowedValues` is de tekst weergeven voor een item en de waarde is de uitvoerwaarde van het element als geselecteerd.
- Indien opgegeven, de standaardwaarde moet een label aanwezig is in `constraints.allowedValues`. Als niet wordt opgegeven, het eerste item in `constraints.allowedValues` is standaard geselecteerd. De standaardwaarde is **null**.
- `constraints.allowedValues` moet ten minste één item bevatten.
- Dit element biedt geen ondersteuning voor de `constraints.required` eigenschap; een item moet worden geselecteerd om te worden gevalideerd.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"two"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
