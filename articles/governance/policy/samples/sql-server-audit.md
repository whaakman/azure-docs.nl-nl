---
title: Voorbeeld Azure Policy - auditinstellingsniveau SQL Server controleren
description: Dit voorbeeldbeleid controleert de auditinstellingen van SQL Server.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 258d56f70349c99faf6053de46886b736ffb95cf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949603"
---
# <a name="audit-sql-server-audit-settings"></a>Auditinstellingen SQL Server controleren

Dit ingebouwde beleid controleert SQL Server op basis van of de auditinstellingen zijn ingeschakeld.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Voorbeeldsjabloon

```json
{
  "if": {
    "field": "type",
    "equals": "Microsoft.SQL/servers"
  },
  "then": {
    "effect": "auditIfNotExists",
    "details": {
      "type": "Microsoft.SQL/servers/auditingSettings",
      "name": "default",
      "existenceCondition": {
        "allOf": [
          {
            "field": "Microsoft.Sql/auditingSettings.state",
            "equals": "[parameters('setting')]"
          }
        ]
      }
    }
  }
}
```

U kunt deze sjabloon implementeren met behulp van [Azure Portal](#deploy-with-the-portal), met [PowerShell](#deploy-with-powershell) of met de [Azure CLI](#deploy-with-azure-cli). Gebruik de id `a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9` om het ingebouwde beleid op te vragen.

## <a name="parameters"></a>Parameters

Gebruik de volgende syntaxis om de parameterwaarde door te geven:

```json
{"setting": {"value":"enabled"}}
```

## <a name="deploy-with-the-portal"></a>Implementeren met portal

Wanneer u een beleid toewijst, selecteert u **Niveau auditinstellingen SQL Server controleren** uit de beschikbare ingebouwde definities.

## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9

New-AzureRmPolicyAssignment -name "SQL Audit audit" -PolicyDefinition $definition -PolicyParameter '{"setting": {"value":"enabled"}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell-implementatie opschonen

Voer de volgende opdracht uit om de beleidstoewijzing te verwijderen.

```azurepowershell-interactive
Remove-AzureRmPolicyAssignment -Name "SQL Audit audit" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "SQL Audit audit" --policy a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9 --params '{"setting": {"value":"enabled"}}'
```

### <a name="clean-up-azure-cli-deployment"></a>Implementatie van Azure CLI opschonen

Voer de volgende opdracht uit om de beleidstoewijzing te verwijderen.

```azurecli-interactive
az policy assignment delete --name "SQL Audit audit" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- Bekijk meer voorbeelden op [Voorbeelden van Azure Policy](index.md)