---
title: Fout bij het bronbeleid Azure RequestDisallowedByPolicy | Microsoft Docs
description: Beschrijft de oorzaak van de fout RequestDisallowedByPolicy.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 2e821c0369c6f01a7f09361c1093259429a79fa6
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Fout bij het bronbeleid Azure RequestDisallowedByPolicy

In dit artikel beschrijft de oorzaak van de fout RequestDisallowedByPolicy, het bevat ook oplossing voor deze fout.

## <a name="symptom"></a>Symptoom

Wanneer u een actie wordt uitgevoerd tijdens de implementatie probeert, verschijnt er een **RequestDisallowedByPolicy** fout waardoor de actie voltooid. Het volgende voorbeeld ziet u de volgende fout:

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

### <a name="method-1"></a>Methode 1

In PowerShell bieden die beleids-id als de `Id` parameter voor het ophalen van informatie over het beleid dat uw implementatie geblokkeerd.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Methode 2 

Geef de naam van de beleidsdefinitie in Azure CLI 2.0: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Oplossing

Voor beveiliging of naleving uw IT-afdeling kan resource beleid afdwingen dat verbiedt het openbare IP-adressen, Netwerkbeveiligingsgroepen, door de gebruiker gedefinieerde Routes of routetabellen maken. Het foutbericht in de **symptomen** sectie ziet u een beleid met de naam **regionPolicyDefinition**. Uw beleid mogelijk een andere naam.
U lost dit probleem, werken met uw IT-afdeling om te controleren van de resource-beleidsregels en bepalen hoe u kunt de aangevraagde actie in overeenstemming met deze beleidsregels uitvoeren.

Raadpleeg voor meer informatie de volgende artikelen:

- [Overzicht van de resource-beleid](resource-manager-policy.md)
- [Beleidstoewijzingen weergeven via de portal](resource-manager-policy-portal.md)
