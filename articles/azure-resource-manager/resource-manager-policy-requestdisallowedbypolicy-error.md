---
title: Fout bij het bronbeleid Azure RequestDisallowedByPolicy | Microsoft Docs
description: Beschrijft de oorzaak van de fout RequestDisallowedByPolicy.
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
ms.date: 03/09/2018
ms.author: genli
ms.openlocfilehash: 474400d92660b68fd7fef906216b8e37c6e8c94d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Fout bij het bronbeleid Azure RequestDisallowedByPolicy

In dit artikel beschrijft de oorzaak van de fout RequestDisallowedByPolicy, het bevat ook oplossing voor deze fout.

## <a name="symptom"></a>Symptoom

Tijdens de implementatie, verschijnt er een **RequestDisallowedByPolicy** fout waardoor het maken van de resources. Het volgende voorbeeld ziet u de volgende fout:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Problemen oplossen

Voor het ophalen van gegevens over het beleid dat uw implementatie geblokkeerd, gebruik de volgende methoden:

### <a name="powershell"></a>PowerShell

In PowerShell bieden die beleids-id als de `Id` parameter voor het ophalen van informatie over het beleid dat uw implementatie geblokkeerd.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure-CLI

Geef de naam van de beleidsdefinitie in Azure CLI 2.0:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Oplossing

Voor beveiliging of naleving wijst de abonnementbeheerders van uw mogelijk beleidsregels die beperken hoe resources worden geïmplementeerd. Uw abonnement kan bijvoorbeeld een beleid waardoor het niet openbaar IP-adressen, Netwerkbeveiligingsgroepen, door de gebruiker gedefinieerde Routes maken of routetabellen. Het foutbericht in de **symptomen** sectie ziet u de naam van het beleid.
U lost dit probleem, Controleer de bronbeleid en bepalen hoe resources die aan deze beleidsregels voldoen implementeren.

Raadpleeg voor meer informatie de volgende artikelen:

- [Wat is Azure beleid?](../azure-policy/azure-policy-introduction.md)
- [Beleidsregels voor het afdwingen van compatibiliteit maken en beheren](../azure-policy/create-manage-policy.md)
