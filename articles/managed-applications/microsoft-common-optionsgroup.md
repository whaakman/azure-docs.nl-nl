---
title: Azure beheerde toepassing OptionsGroup UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Common.OptionsGroup UI-element voor beheerde Azure-toepassingen
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
ms.openlocfilehash: 0ba820176e0231437e878922cb508cd3b97d46c3
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft.Common.OptionsGroup UI-element
Een Selectiebesturingselement met een rij van de beschikbare opties. Gebruik van dit element wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "Foo",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Foo",
        "value": "Bar"
      },
      {
        "label": "Baz",
        "value": "Qux"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Het label voor `constraints.allowedValues` is de tekst weergeven voor een item en de waarde is de uitvoerwaarde van het element als geselecteerd.
- Indien opgegeven, de standaardwaarde moet een label aanwezig is in `constraints.allowedValues`. Als niet wordt opgegeven, het eerste item in `constraints.allowedValues` is standaard geselecteerd. De standaardwaarde is **null**.
- `constraints.allowedValues`moet ten minste één item bevatten.
- Dit element biedt geen ondersteuning voor de `constraints.required` eigenschap; een item moet worden geselecteerd om te worden gevalideerd.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"Bar"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
