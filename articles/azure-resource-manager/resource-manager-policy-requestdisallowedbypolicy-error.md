---
title: RequestDisallowedByPolicy-fout opgetreden bij het Azure-resource-beleid | Microsoft Docs
description: Beschrijving van de oorzaak van de RequestDisallowedByPolicy-fout.
services: azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 8eea14703a7a4ed6fad56dc0bed981b84266e2db
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112611"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>RequestDisallowedByPolicy-fout opgetreden bij het Azure-resource-beleid

In dit artikel beschrijft de oorzaak van de RequestDisallowedByPolicy-fout, ook biedt deze oplossing voor deze fout.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="symptom"></a>Symptoom

Tijdens de implementatie, ontvangt u mogelijk een **RequestDisallowedByPolicy** fout waarmee wordt voorkomen dat u het maken van de resources. Het volgende voorbeeld ziet u de volgende fout:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Problemen oplossen

Als u wilt ophalen van informatie over het beleid dat uw implementatie geblokkeerd, gebruik de volgende uitvoer van de methoden:

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In PowerShell, geef deze beleids-id als de `Id` parameter om op te halen van meer informatie over het beleid dat uw implementatie geblokkeerd.

```PowerShell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure-CLI

Geef de naam van de beleidsdefinitie in de Azure CLI:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Oplossing

Voor beveiliging of naleving wijst de abonnementbeheerders van uw mogelijk beleidsregels die beperken hoe resources worden geïmplementeerd. Uw abonnement kan bijvoorbeeld een beleid waarmee wordt voorkomen dat het openbare IP-adressen, Network Security Groups, door de gebruiker gedefinieerde Routes maken of routetabellen. Het foutbericht in de **symptomen** sectie ziet u de naam van het beleid.
U lost dit probleem, Controleer de resource-beleidsregels en te bepalen hoe resources die aan de beleidsregels voldoen te implementeren.

Raadpleeg voor meer informatie de volgende artikelen:

- [Wat is Azure Policy?](../azure-policy/azure-policy-introduction.md)
- [Maken en beheren van beleidsregels voor het afdwingen van naleving](../azure-policy/create-manage-policy.md)
