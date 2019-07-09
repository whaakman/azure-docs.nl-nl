---
title: 'Zelfstudie: een gebruiker toegang tot Azure-resources met behulp van RBAC en Resource Manager-sjabloon | Microsoft Docs'
description: Leer hoe u gebruikers toegang geven tot Azure-resources met behulp van op rollen gebaseerd toegangsbeheer (RBAC) met behulp van Azure Resource Manager-sjabloon.
services: role-based-access-control,azure-resource-manager
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/15/2019
ms.author: rolyon
ms.openlocfilehash: edb20221862e6439b3bc574995f4037cbc95f8f9
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67668885"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-resource-manager-template"></a>Zelfstudie: Een gebruiker toegang tot Azure-resources met behulp van RBAC en Resource Manager-sjabloon

[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot Azure-resources beheert. In deze zelfstudie maakt u een resourcegroep maken en gebruikers toegang geven voor het maken en beheren van virtuele machines in de resourcegroep. Deze zelfstudie richt zich op het proces voor het implementeren van een Resource Manager-sjabloon om de toegang te verlenen. Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen [Resource Manager-documentatie](/azure/azure-resource-manager/) en de [sjabloonverwijzing](/azure/templates/microsoft.authorization/allversions
).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Toegang voor een gebruiker toewijzen op het niveau van een resourcegroep
> * De implementatie valideren
> * Opruimen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u wilt toevoegen en verwijderen van roltoewijzingen, moet u het volgende hebben:

* `Microsoft.Authorization/roleAssignments/write` en `Microsoft.Authorization/roleAssignments/delete` machtigingen, zoals [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) of [eigenaar](built-in-roles.md#owner)

## <a name="grant-access"></a>Toegang verlenen

De sjabloon die wordt gebruikt in deze Quick Start is afkomstig van [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-rbac-builtinrole-resourcegroup/). Meer Azure autorisatie gerelateerde sjablonen vindt [hier](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization).

Als u wilt implementeren de sjabloon, selecteert u **uitproberen** Azure Cloud shell openen en plak het volgende PowerShell-script in de shell-venster. Als u de code, met de rechtermuisknop op de shell-venster en selecteer vervolgens **plakken**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$emailAddress = Read-Host -Prompt "Enter your email address that is associated with your Azure subscription (used to find the principal ID)"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

$resourceGroupName = "${projectName}rg"
$roleAssignmentName = New-Guid
$principalId = (Get-AzAdUser -Mail $emailAddress).id
$roleDefinitionId = (Get-AzRoleDefinition -name "Virtual Machine Contributor").id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-rbac-builtinrole-resourcegroup/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -roleAssignmentName $roleAssignmentName -roleDefinitionID $roleDefinitionId -principalId $principalId
```

## <a name="validate-the-deployment"></a>De implementatie valideren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Open de resourcegroep in de laatste procedure hebt gemaakt. De standaardnaam is de naam van het project met **rg** toegevoegd.
1. Selecteer **Toegangsbeheer (IAM)** in het menu links.
1. Selecteer **roltoewijzingen**. 
1. In **naam**, voer het e-mailadres dat u hebt opgegeven in de laatste procedure. U ziet de gebruiker met het e-mailadres heeft de **Inzender voor virtuele machines** rol.

## <a name="clean-up"></a>Opruimen

Als u wilt verwijderen van de resourcegroep in de laatste procedure hebt gemaakt, selecteert u **uitproberen** Azure Cloud shell openen en plak het volgende PowerShell-script in de shell-venster.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a same project name you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Toegang tot Azure-resources verlenen aan een gebruiker met behulp van RBAC en Azure PowerShell](tutorial-role-assignments-user-powershell.md)