---
title: Voorbeeld - transparante gegevensversleuteling voor SQL Database controleren
description: Dit voorbeeldbeleid controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/23/2019
ms.author: dacoulte
ms.openlocfilehash: 8bc9fdb6d1008d6d57edaab0bdbf3892584f3be2
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853796"
---
# <a name="audit-sql-database-encryption"></a>Versleuteling van SQL-database controleren

Dit ingebouwd beleid controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Voorbeeldsjabloon

[!code-json[main](../../../../policy-templates/samples/SQL/audit-sql-db-tde-status/azurepolicy.json "Audit TDE for SQL Database")]

U kunt deze sjabloon implementeren met behulp van [Azure Portal](#deploy-with-the-portal), met [PowerShell](#deploy-with-powershell) of met de [Azure CLI](#deploy-with-azure-cli). Gebruik de id `17k78e20-9358-41c9-923c-fb736d382a12` om het ingebouwde beleid op te vragen.

## <a name="deploy-with-the-portal"></a>Implementeren met portal

Wanneer u een beleid toewijst, selecteert u **Controlestatus van transparante gegevensversleuteling** uit de beschikbare ingebouwde definities.

## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/17k78e20-9358-41c9-923c-fb736d382a12

New-AzPolicyAssignment -name "SQL TDE Audit" -PolicyDefinition $definition -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell-implementatie opschonen

Voer de volgende opdracht uit om de beleidstoewijzing te verwijderen.

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name "SQL TDE Audit" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "SQL TDE Audit" --policy 17k78e20-9358-41c9-923c-fb736d382a12
```

### <a name="clean-up-azure-cli-deployment"></a>Implementatie van Azure CLI opschonen

Voer de volgende opdracht uit om de beleidstoewijzing te verwijderen.

```azurecli-interactive
az policy assignment delete --name "SQL TDE Audit" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- Bekijk meer voorbeelden op [Voorbeelden van Azure Policy](index.md)