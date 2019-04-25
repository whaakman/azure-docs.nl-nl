---
title: Azure sectie UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Common.Section UI-element voor Azure-portal.
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
ms.openlocfilehash: c89b45dd4d8e6c2964f3d2bcbb6c3cef445c79e6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251888"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI element
Een besturingselement waarin een of meer elementen onder de kop zijn gegroepeerd.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Schema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- `elements` moet ten minste één element hebben en kan alle elementtypen behalve `Microsoft.Common.Section`.
- Dit element biedt geen ondersteuning voor de `toolTip` eigenschap.

## <a name="sample-output"></a>Voorbeelduitvoer
Voor toegang tot de uitvoerwaarden van elementen in `elements`, gebruikt u de [basics()](create-uidefinition-functions.md#basics) of [steps()](create-uidefinition-functions.md#steps) functies en puntnotatie:

```json
steps('configuration').section1.text1
```

Elementen van het type `Microsoft.Common.Section` hebben geen uitvoerwaarden zelf.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
