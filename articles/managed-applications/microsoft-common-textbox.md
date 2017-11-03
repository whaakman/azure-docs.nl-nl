---
title: Azure Application TextBox-UI beheerd element | Microsoft Docs
description: Beschrijft het Microsoft.Common.TextBox UI-element voor beheerde Azure-toepassingen
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
ms.openlocfilehash: d3fae42ae202fe720761382e1020fa8bd8c62b44
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox UI-element
Een besturingselement dat kan worden gebruikt voor het bewerken van tekst zonder opmaak. Gebruik van dit element wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Halp!",
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
- `constraints.regex`is een reguliere-expressiepatroon van JavaScript. Indien opgegeven, klikt u vervolgens de waarde van het tekstvak moet overeenkomen met het patroon kan worden gevalideerd. De standaardwaarde is **null**.
- `constraints.validationMessage`is een tekenreeks die moet worden weergegeven wanneer de waarde van het tekstvak validatie is mislukt. Als niet wordt opgegeven, wordt het tekstvak ingebouwde validatieberichten gebruikt. De standaardwaarde is **null**.
- Het is mogelijk om op te geven van een waarde voor `constraints.regex` wanneer `constraints.required` is ingesteld op **false**. In dit scenario wordt is een waarde niet vereist voor het tekstvak kan worden gevalideerd. Als er een is opgegeven, moet overeenkomen met de reguliere-expressiepatroon.

## <a name="sample-output"></a>Voorbeelduitvoer

```json
"foobar"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
