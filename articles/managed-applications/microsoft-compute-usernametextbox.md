---
title: Azure beheerde toepassing UserNameTextBox UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Compute.UserNameTextBox UI-element voor beheerde Azure-toepassingen
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
ms.openlocfilehash: 6a395915af274750eb57a085ee51b55fdd392615
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft.Compute.UserNameTextBox UI-element
Een tekstvak met ingebouwde validatie voor Windows en Linux-gebruikersnamen. Gebruik van dit element wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md).

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
- Als `constraints.required` is ingesteld op **true**, en vervolgens in het tekstvak moet een waarde kan worden gevalideerd bevatten. De standaardwaarde is **true**.
- `osPlatform`moet worden opgegeven en kan **Windows** of **Linux**.
- `constraints.regex`is een reguliere-expressiepatroon van JavaScript. Indien opgegeven, klikt u vervolgens de waarde van het tekstvak moet overeenkomen met het patroon kan worden gevalideerd. De standaardwaarde is **null**.
- `constraints.validationMessage`is een tekenreeks om weer te geven wanneer de waarde van het tekstvak, de validatie is opgegeven mislukt door `constraints.regex`. Als niet wordt opgegeven, wordt het tekstvak ingebouwde validatieberichten gebruikt. De standaardwaarde is **null**.
- Dit element heeft ingebouwde validatiefouten die is gebaseerd op de opgegeven waarde voor `osPlatform`. De ingebouwde validatie kan worden gebruikt samen met een aangepaste reguliere expressie.
Als er een waarde voor `constraints.regex` is opgegeven, worden de ingebouwde en aangepaste validaties worden geactiveerd.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"tabrezm"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
