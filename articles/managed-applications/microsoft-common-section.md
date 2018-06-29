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
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 90ffae3dd8b05041c34d766e464eb68f793f6066
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062975"
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
- `elements` moet ten minste één element hebben en kunnen alle elementtypen behalve `Microsoft.Common.Section`.
- Dit element biedt geen ondersteuning voor de `toolTip` eigenschap.

## <a name="sample-output"></a>Voorbeelduitvoer
Voor toegang tot de uitvoerwaarden van elementen in `elements`, gebruiken de [basics()](create-uidefinition-functions.md#basics) of [steps()](create-uidefinition-functions.md#steps) functies en puntnotatie:

```json
steps('configuration').section1.text1
```

Elementen van het type `Microsoft.Common.Section` hebben geen uitvoerwaarden zelf.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
