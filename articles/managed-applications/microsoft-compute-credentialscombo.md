---
title: Azure CredentialsCombo UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Compute.CredentialsCombo UI-element voor Azure-portal.
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
ms.date: 09/29/2018
ms.author: tomfitz
ms.openlocfilehash: 0412d55fe60524cde404e6a640723d3259e020e1
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586237"
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo UI-element
Een groep besturingselementen met ingebouwde validatie voor wachtwoorden voor Windows en Linux- en openbare SSH-sleutels.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface

Voor Windows, gebruikers te zien:

![Microsoft.Compute.CredentialsCombo Windows](./media/managed-application-elements/microsoft.compute.credentialscombo-windows.png)

Zie voor Linux met een wachtwoord dat is geselecteerd, gebruikers:

![Microsoft.Compute.CredentialsCombo Linux-wachtwoord](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-password.png)

Zie voor Linux met openbare SSH-sleutel hebt geselecteerd, gebruikers:

![Microsoft.Compute.CredentialsCombo Linux-sleutel](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-key.png)

## <a name="schema"></a>Schema
Voor Windows, gebruikt u het volgende schema:

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
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must contain at least 12 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

Voor **Linux**, gebruikt u het volgende schema:

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
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must contain at least 12 characters, with at least 1 letter and 1 number."
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
- `osPlatform` Er moet worden opgegeven en kan **Windows** of **Linux**.
- Als `constraints.required` is ingesteld op **waar**, en vervolgens het wachtwoord of SSH public key tekstvakken moet waarden die moeten worden gevalideerd. De standaardwaarde is **waar**.
- Als `options.hideConfirmation` is ingesteld op **waar**, en vervolgens het tweede tekstvak voor het bevestigen van het wachtwoord van gebruikers wordt verborgen. De standaardwaarde is **false**.
- Als `options.hidePassword` is ingesteld op **waar**, en vervolgens de optie voor het gebruik van wachtwoordverificatie is verborgen. Het kan worden gebruikt alleen wanneer `osPlatform` is **Linux**. De standaardwaarde is **false**.
- Aanvullende beperkingen met betrekking tot de toegestane wachtwoorden kunnen worden geïmplementeerd met behulp van de `customPasswordRegex` eigenschap. De tekenreeks in `customValidationMessage` wordt weergegeven als een wachtwoord aangepaste validatie is mislukt. De standaardwaarde voor beide eigenschappen is **null**.

## <a name="sample-output"></a>Voorbeelduitvoer
Als `osPlatform` is **Windows**, of `osPlatform` is **Linux** en de gebruiker een wachtwoord in plaats van een openbare SSH-sleutel opgegeven, wordt het besturingselement geeft als resultaat de volgende uitvoer:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rddem0",
}
```

Als `osPlatform` is **Linux** en de gebruiker een openbare SSH-sleutel hebt opgegeven, wordt het besturingselement geeft als resultaat de volgende uitvoer:

```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
