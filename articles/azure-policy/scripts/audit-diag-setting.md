---
title: Json-voorbeeld van Azure Policy - Diagnostische instelling voor controleren | Microsoft Docs
description: Met dit json-voorbeeld van een beleid wordt gecontroleerd of er geen diagnostische instellingen zijn ingeschakeld voor de opgegeven resourcetypen.
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 04/27/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 1f87d411e244d10437e3b6f9befbdee13dde14e9
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="audit-diagnostic-setting"></a>Diagnostische instelling voor controleren

Met dit ingebouwde beleid wordt gecontroleerd of er geen diagnostische instellingen zijn ingeschakeld voor de opgegeven resourcetypen. U geeft een matrix van resourcetypen op waarvoor u wilt controleren of er diagnostische instellingen zijn ingeschakeld.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Voorbeeldsjabloon

[!code-json[main](../../../policy-templates/samples/Monitoring/audit-diagnostic-setting/azurepolicy.json "Audit diagnostic setting")]

U kunt deze sjabloon implementeren met behulp van [Azure Portal](#deploy-with-the-portal), met [PowerShell](#deploy-with-powershell) of met de [Azure CLI](#deploy-with-azure-cli). Gebruik de id `7f89b1eb-583c-429a-8828-af049802c1d9` om het ingebouwde beleid op te vragen.

## <a name="parameters"></a>Parameters

Gebruik de volgende syntaxis om de parameterwaarde door te geven:

```json
{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}
```

## <a name="deploy-with-the-portal"></a>Implementeren met portal

Wanneer u een beleid toewijst, selecteert u **Diagnostische instelling voor controleren** uit de beschikbare ingebouwde definities.

## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/7f89b1eb-583c-429a-8828-af049802c1d9

New-AzureRmPolicyAssignment -name "Audit diagnostics" -PolicyDefinition $definition -PolicyParameter '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell-implementatie opschonen

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmPolicyAssignment -Name "Audit diagnostics" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "Audit diagnostics" --policy 7f89b1eb-583c-429a-8828-af049802c1d9 --params '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}'
```

### <a name="clean-up-azure-cli-deployment"></a>Implementatie van Azure CLI opschonen

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az policy assignment delete --name "Audit diagnostics" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- Meer Azure Policy-voorbeeldsjablonen zijn te vinden op [Sjablonen voor Azure Policy](../json-samples.md).
