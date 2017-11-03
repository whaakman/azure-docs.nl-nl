---
title: Azure beheerde toepassing CredentialsCombo UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Compute.CredentialsCombo UI-element voor beheerde Azure-toepassingen
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
ms.openlocfilehash: d8faa36aca762bc8d787d5750fcf7efdbaf986ea
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo UI-element
Een groep besturingselementen met ingebouwde validatie voor wachtwoorden voor Windows en Linux- en openbare SSH-sleutels. Gebruik van dit element wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a>Schema
Als `osPlatform` is **Windows**, wordt het volgende schema gebruikt:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

Als `osPlatform` is **Linux**, wordt het volgende schema gebruikt:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- `osPlatform`moet worden opgegeven en kan **Windows** of **Linux**.
- Als `constraints.required` is ingesteld op **true**, en vervolgens het wachtwoord of SSH tekstvakken met openbare sleutels moeten bevatten waarden kan worden gevalideerd. De standaardwaarde is **true**.
- Als `options.hideConfirmation` is ingesteld op **true**, en vervolgens het tweede tekstvak voor het bevestigen van het wachtwoord van de gebruiker wordt verborgen. De standaardwaarde is **false**.
- Als `options.hidePassword` is ingesteld op **true**, en vervolgens de optie wachtwoordverificatie te gebruiken, wordt verborgen. Het kan worden gebruikt alleen wanneer `osPlatform` is **Linux**. De standaardwaarde is **false**.
- Aanvullende beperkingen voor de toegestane wachtwoorden kunnen worden geïmplementeerd met behulp van de `customPasswordRegex` eigenschap. De tekenreeks in `customValidationMessage` wordt weergegeven wanneer u een wachtwoord aangepaste validatie is mislukt. De standaardwaarde voor beide eigenschappen is **null**.

## <a name="sample-output"></a>Voorbeelduitvoer
Als `osPlatform` is **Windows**, of de gebruiker een wachtwoord in plaats van een openbare SSH-sleutel opgegeven, wordt de volgende uitvoer wordt verwacht:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

Als de gebruiker een openbare SSH-sleutel hebt opgegeven, wordt de volgende uitvoer verwacht:
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
