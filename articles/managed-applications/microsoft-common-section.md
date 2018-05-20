---
title: Azure sectie UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Common.Section UI-element voor Azure-portal.
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
ms.openlocfilehash: e6d7d5d7b205d275c72e96df527a354b072a9dd3
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI-element
Een besturingselement dat groepen van een of meer elementen onder de kop.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Schema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- `elements` moet ten minste één element bevatten en mogen alle elementtypen behalve `Microsoft.Common.Section`.
- Dit element biedt geen ondersteuning voor de `toolTip` eigenschap.

## <a name="sample-output"></a>Voorbeelduitvoer
Voor toegang tot de uitvoerwaarden van elementen in `elements`, gebruiken de [basics()](create-uidefinition-functions.md#basics) of [steps()](create-uidefinition-functions.md#steps) functies en puntnotatie:

```json
basics('section1').element1
```

Elementen van het type `Microsoft.Common.Section` hebben geen uitvoerwaarden zelf.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
