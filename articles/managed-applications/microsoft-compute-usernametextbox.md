---
title: Azure UserNameTextBox UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Compute.UserNameTextBox UI-element voor Azure-portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 88ab13329a719ba1e1b8a7b5fba2f7a2d381eca2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251348"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft.Compute.UserNameTextBox UI-element
Besturingselement voor een tekstvak met ingebouwde validatie voor Windows en Linux-gebruikersnamen.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Als `constraints.required` is ingesteld op **waar**, en vervolgens in het tekstvak moet een waarde die moet worden gevalideerd. De standaardwaarde is **waar**.
- `osPlatform` Er moet worden opgegeven en kan **Windows** of **Linux**.
- `constraints.regex` is een reguliere-expressiepatroon van JavaScript. Als u opgeeft, klikt u vervolgens het tekstvak waarde moet overeenkomen met het patroon worden gevalideerd. De standaardwaarde is **null**.
- `constraints.validationMessage` is een tekenreeks om weer te geven wanneer de waarde van het tekstvak, mislukt de validatie is opgegeven door `constraints.regex`. Als niet is opgegeven, wordt het tekstvak ingebouwde validatieberichten gebruikt. De standaardwaarde is **null**.
- Dit element heeft ingebouwde valideren die is gebaseerd op de opgegeven waarde voor `osPlatform`. De ingebouwde validatie kan worden gebruikt samen met een aangepaste reguliere expressie. Als er een waarde voor `constraints.regex` is opgegeven, worden de ingebouwde en aangepaste validaties worden geactiveerd.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"Example name"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
