---
title: Voorbeeld - Audit als toepassingen niet zijn geïnstalleerd in virtuele Linux-machines
description: In dit voorbeeld Gast beleidsconfiguratie initiatief en definities controleren als de opgegeven toepassingen niet binnen de virtuele Linux-machines zijn geïnstalleerd.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 03/18/2019
ms.author: dacoulte
ms.openlocfilehash: c939deda9b1468b5ce843d497b81a462938a2554
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805547"
---
# <a name="sample---audit-if-specified-applications-are-not-installed-inside-linux-vms"></a>Voorbeeld - Audit als opgegeven toepassingen zijn niet geïnstalleerd in virtuele Linux-machines

Dit initiatief beleidsconfiguratie Gast voert een controle uit dat de opgegeven toepassing is geïnstalleerd in virtuele Linux-machines. De ID van deze ingebouwde initiatief is `/providers/Microsoft.Authorization/policySetDefinitions/c937dcb4-4398-4b39-8d63-4a6be432252e`.

> [!IMPORTANT]
> Alle configuratie van de Gast-initiatieven worden samengesteld uit **controleren** en **deployIfNotExists** beleidsdefinities. Het toekennen van alleen een van de beleidsdefinities ertoe leiden dat de configuratie van de Gast niet correct te laten werken.

In dit voorbeeld met, kunt u toewijzen:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="components-of-the-initiative"></a>Onderdelen van het initiatief

Dit [Gast configuratie](../concepts/guest-configuration.md) initiatief is afhankelijk van het volgende beleid:

- [audit](#audit-definition) -controleren dat een toepassing in Linux-VM's is geïnstalleerd
  - ID: `/providers/Microsoft.Authorization/policyDefinitions/fee5cb2b-9d9b-410e-afe3-2902d90d0004`
- [deployIfNotExists](#deployIfNotExists-definition) -implementatie van VM-extensie om te controleren dat een toepassing in Linux-VM's is geïnstalleerd
  - ID: `/providers/Microsoft.Authorization/policyDefinitions/4d1c04de-2172-403f-901b-90608c35c721`

### <a name="initiative-definition"></a>Initiatiefdefinitie

Het initiatief is gemaakt door de **controleren** en **deployIfNotExists** definities samen en de [initiatief Parameters](#initiative-parameters). Dit is de JSON van de definitie.

[!code-json[initiative-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/azurepolicyset.json "Initiative definition (JSON)")]

### <a name="initiative-parameters"></a>Initiatiefparameters

|Name |Type ||Description | |---|---||---| |applicationName |String |Application names. Voorbeeld: 'python', 'powershell' of een door komma's gescheiden lijst, zoals 'python, powershell'. Gebruik \* voor jokertekens, zoals ' power\*'. |

Bij het maken van een toewijzing via PowerShell of Azure CLI kunnen de parameterwaarden worden doorgegeven als JSON in een tekenreeks of via een bestand met `-PolicyParameter` (PowerShell) of `--params` (Azure CLI).
PowerShell ondersteunt ook `-PolicyParameterObject`, waarvoor de cmdlet een hashtabel met naam/waardeparen moet ontvangen waarin **Name** de parameternaam is en **Value** is de enkelvoudige waarde of matrix met waarden die tijdens toewijzing wordt doorgegeven.

In dit voorbeeld van de parameter, de installatie van toepassingen _python_ en _powershell_ wordt gecontroleerd.

```json
{
    "applicationName": {
        "value": "python,powershell"
    }
}
```

Alleen de **deployIfNotExists** beleidsdefinitie maakt gebruik van de initiatiefparameters.

### <a name="audit-definition"></a>audit-definitie

De JSON voor het definiëren van de regels van de **controleren** beleidsdefinitie.

[!code-json[audit-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json "audit policy rules (JSON)")]

### <a name="deployifnotexists-definition"></a>deployIfNotExists-definitie

De JSON voor het definiëren van de regels van de **deployIfNotExists** beleidsdefinitie.

[!code-json[deployIfNotExists-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json "deployIfNotExists policy rules (JSON)")]

De **deployIfNotExists** beleidsdefinitie definieert de installatiekopieën van Azure die het beleid is gevalideerd op:

|Uitgever |Aanbieding |SKU |
|-|-|-|
|OpenLogic |CentOS\* |Overal behalve in 6\* |
|RedHat |RHEL |Overal behalve in 6\* |
|RedHat |osa | Alle |
|credativ |Debian | Overal behalve 7\* |
|SUSE |SLES\* |Overal behalve 11\* |
|Canonical| UbuntuServer |Overal behalve 12\* |
|microsoft-dsvm |linux-data-science-vm-ubuntu |Alle |
|microsoft-dsvm |azureml |Alle |
|cloudera |cloudera-centos-os |Overal behalve in 6\* |
|cloudera |cloudera-altus-centos-os |Alle |
|microsoft-ads |linux\* |Alle |
|microsoft-aks |Alle |Alle |
|AzureDatabricks |Alle |Alle |
|qubole-inc |Alle |Alle |
|datastax |Alle |Alle |
|Couchbase |Alle |Alle |
|scalegrid |Alle |Alle |
|Controlepunt |Alle |Alle |
|paloaltonetworks |Alle |Alle |

De **implementatie** gedeelte van de regel geeft de _installedApplication_ parameter met de configuratie van de Gast-agent op de virtuele machine. Deze configuratie kan de agent uit te voeren van de validaties en het rapport naleving back-ups maken via de **controleren** beleidsdefinitie.

## <a name="azure-portal"></a>Azure Portal

Na de **controleren** en **deployIfNotExists** definities in de portal zijn gemaakt, het is raadzaam om te groeperen in een [initiatief](../concepts/definition-structure.md#initiatives) voor toewijzing.

### <a name="create-copy-of-audit-definition"></a>Kopie van de audit-definitie maken

[![De voorbeeld-beleid implementeren naar Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)
[![de voorbeeld-beleid implementeren naar Azure (overheid)](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)

Gebruik deze knoppen voor het implementeren via de portal maakt een kopie van de **controleren** beleidsdefinitie.
Zonder de gekoppelde **deployIfNotExists** beleidsdefinitie, de configuratie van de Gast werkt niet correct.

### <a name="create-copy-of-deployifnotexists-definition"></a>Kopie van deployIfNotExists-definitie maken

[![De voorbeeld-beleid implementeren naar Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)
[![de voorbeeld-beleid implementeren naar Azure (overheid)](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)

Gebruik deze knoppen voor het implementeren via de portal maakt een kopie van de **deployIfNotExists** beleidsdefinitie. Zonder de gekoppelde **controleren** beleidsdefinitie, de configuratie van de Gast werkt niet correct.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Implementeren met Azure PowerShell

#### <a name="copy-and-assign-the-initiative"></a>Kopiëren en het initiatief toewijzen

Deze stappen maakt u een kopie van het initiatief met inbegrip van de ingebouwde beleidsregels voor beide **controleren** en **deployIfNotExists** en het initiatief toewijst aan een resourcegroep.

```azurepowershell-interactive
# Create the policy initiative (Subscription scope)
$initDef = New-AzPolicySetDefinition -Name 'guestconfig-installed-application-linux' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This initiative will both deploy the policy requirements and audit that the specified application is installed inside Linux virtual machines.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.definitions.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the initiative parameter (JSON format)
$initParam = '{ "applicationName": { "value": "python,powershell" } }'

# Create the initiative assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicySetDefinition $initDef -PolicyParameter $initParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Voer de volgende opdrachten uit om de vorige toewijzing en definitie te verwijderen:

```azurepowershell-interactive
# Remove the initiative assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the initiative definition
Remove-AzPolicySetDefinition -Id $initDef
```

#### <a name="copy-and-assign-the-audit-definition"></a>Kopiëren en de definitie van de audit toewijzen

Deze stappen maakt u een kopie van de **controleren** definitie en deze toewijzen aan een resourcegroep. Deze definitie werkt niet goed zonder de gekoppelde **deployIfNotExists** definitie ook wordt toegewezen.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-audit' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This policy audits that the specified application is installed inside Linux virtual machines. This policy should only be used along with its corresponding deploy policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-audit-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicyDefinition $definition
```

Voer de volgende opdrachten uit om de vorige toewijzing en definitie te verwijderen:

```azurepowershell-interactive
# Remove the policy definition
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

#### <a name="copy-and-assign-the-deployifnotexists-definition"></a>Kopiëren en de definitie van de deployIfNotExists toewijzen

Deze stappen maakt u een kopie van de **deployIfNotExists** definitie en deze toewijzen aan een resourcegroep.
Deze definitie werkt niet goed zonder de gekoppelde **controleren** definitie ook wordt toegewezen.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-deployIfNotExists' -DisplayName 'GuestConfig - Deploy VM extension to audit that an application is installed inside Linux VMs' -description 'Include this rule to deploy the VM extension for Microsoft Guest Configuration, the VM extension for Microsoft Azure Managed Service Identity, and the content required to audit that an application is installed inside Linux virtual machines. This policy should only be used along with its corresponding audit policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the definition parameter (JSON format)
$policyParam  = '{ "applicationName": { "value": "python,powershell" } }'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-deployIfNotExists-assignment' -DisplayName 'GuestConfig - Deploy VM extension to audit that Python and PowerShell are installed inside Linux VMs' -Scope $scope.ResourceID -PolicyDefinition $definition -PolicyParameter $policyParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Voer de volgende opdrachten uit om de vorige toewijzing en definitie te verwijderen:

```azurepowershell-interactive
# Remove the policy assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

### <a name="azure-powershell-explanation"></a>Toelichting van Azure PowerShell

De scripts voor implementeren en verwijderen gebruiken de volgende opdrachten. Elke opdracht in de volgende tabel is een koppeling naar opdrachtspecifieke documentatie:

| Opdracht | Opmerkingen |
|---|---|
| [New-AzPolicySetDefinition](/powershell/module/az.resources/New-AzPolicySetDefinition) | Hiermee maakt u een Azure Policy-initiatief. |
| [New-AzPolicyDefinition](/powershell/module/az.resources/New-AzPolicyDefinition) | Hiermee maakt u een definitie van een Azure Policy. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup) | Hiermee vraagt u één resourcegroep op. |
| [New-AzPolicyAssignment](/powershell/module/az.resources/New-AzPolicyAssignment) | Hiermee maakt u een nieuwe Azure Policy-toewijzing voor een initiatief- of -definitie. |
| [New-AzRoleAssignment](/powershell/module/az.resources/New-AzRoleAssignment) | Geeft een bestaande roltoewijzing aan de specifieke principal. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-AzPolicyAssignment) | Hiermee verwijdert u een bestaande Azure Policy-toewijzing. |
| [Remove-AzPolicySetDefinition](/powershell/module/az.resources/Remove-AzPolicySetDefinition) | Hiermee verwijdert u een initiatief. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-AzPolicyDefinition) | Hiermee verwijdert u een definitie. |

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg aanvullende [voorbeelden voor Azure Policy](index.md).
- Meer informatie over [Azure Gast beleidsconfiguratie](../concepts/guest-configuration.md).
- Beoordeling [Azure structuur van beleidsdefinities](../concepts/definition-structure.md).