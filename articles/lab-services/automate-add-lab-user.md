---
title: Automatiseren met het toevoegen van een lab-gebruiker in Azure DevTest Labs | Microsoft Docs
description: Informatie over het automatiseren van een lab-gebruiker toe te voegen aan een lab in Azure DevTest Labs.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2019
ms.author: spelluru
ms.openlocfilehash: 2ad81ae97414abbf3266cc5728febf9abe836151
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65522951"
---
# <a name="automate-adding-a-lab-user-to-a-lab-in-azure-devtest-labs"></a>Een lab-gebruiker toe te voegen aan een lab in Azure DevTest Labs automatiseren
Azure DevTest Labs kunt u snel ontwikkel-en selfservice-omgevingen maken met behulp van Azure portal. Echter, hebt u verschillende teams en meerdere exemplaren van DevTest Labs, het maken van het proces te automatiseren kunt tijd besparen. [Azure Resource Manager-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) kunt u labs, lab-virtuele machines, aangepaste installatiekopieën en formules maken en gebruikers toevoegen in een geautomatiseerde manier worden geactiveerd. In dit artikel is specifiek gericht op het toevoegen van gebruikers met een DevTest Labs-exemplaar.

Als u een gebruiker toevoegen aan een lab, wilt u toevoegen aan de **DevTest Labs-gebruiker** rol voor de testomgeving. In dit artikel laat zien hoe u voor het automatiseren van een gebruiker toe te voegen aan een lab met behulp van een van de volgende manieren:

- Azure Resource Manager-sjablonen
- Azure PowerShell-cmdlets 
- Azure CLI.

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen gebruiken
De volgende voorbeeldsjabloon voor Resource Manager Hiermee geeft u een gebruiker moet worden toegevoegd aan de **DevTest Labs-gebruiker** rol van een lab. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The objectId of the user, group, or service principal for the role."
      }
    },
    "labName": {
      "type": "string",
      "metadata": {
        "description": "The name of the lab instance to be created."
      }
    },
    "roleAssignmentGuid": {
      "type": "string",
      "metadata": {
        "description": "Guid to use as the name for the role assignment."
      }
    }
  },
  "variables": {
    "devTestLabUserRoleId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/111111111-0000-0000-11111111111111111')]",
    "fullDevTestLabUserRoleName": "[concat(parameters('labName'), '/Microsoft.Authorization/', parameters('roleAssignmentGuid'))]",
    "roleScope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.DevTestLab/labs/', parameters('labName'))]"
  },
  "resources": [
    {
      "apiVersion": "2016-05-15",
      "type": "Microsoft.DevTestLab/labs",
      "name": "[parameters('labName')]",
      "location": "[resourceGroup().location]"
    },
    {
      "apiVersion": "2016-07-01",
      "type": "Microsoft.DevTestLab/labs/providers/roleAssignments",
      "name": "[variables('fullDevTestLabUserRoleName')]",
      "properties": {
        "roleDefinitionId": "[variables('devTestLabUserRoleId')]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('roleScope')]"
      },
      "dependsOn": [
        "[resourceId('Microsoft.DevTestLab/labs', parameters('labName'))]"
      ]
    }
  ]
}

```

Als u de rol in dezelfde sjabloon die wordt gemaakt in het lab toewijst, vergeet dan niet om toe te voegen een afhankelijkheid tussen de bron van de toewijzing van rol en het lab. Zie voor meer informatie, [afhankelijkheden definiëren in Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-define-dependencies.md) artikel.

### <a name="role-assignment-resource-information"></a>De toewijzingsgegevens Resource rol
De rol toewijzing resource nodig heeft om op te geven van het type en de naam.

Het eerste wat om te weten is dat het type voor de resource niet `Microsoft.Authorization/roleAssignments` omdat het normaal zou voor een resourcegroep zijn.  In plaats daarvan, het resourcetype heeft het patroon `{provider-namespace}/{resource-type}/providers/roleAssignments`. In dit geval het resourcetype is `Microsoft.DevTestLab/labs/providers/roleAssignments`.

De naam van de roltoewijzing zelf moet globaal uniek zijn.  De naam van de toewijzing maakt gebruik van het patroon `{labName}/Microsoft.Authorization/{newGuid}`. De `newGuid` is de waarde van een parameter voor de sjabloon. Het zorgt ervoor dat de naam van de roltoewijzing uniek is. Er zijn geen functies van sjablonen voor het maken van GUID's, moet u een GUID zelf genereren met behulp van een GUID-generator-hulpprogramma.  

In de sjabloon de naam van de roltoewijzing wordt gedefinieerd door de `fullDevTestLabUserRoleName` variabele. De exacte regel van de sjabloon is:

```json
"fullDevTestLabUserRoleName": "[concat(parameters('labName'), '/Microsoft.Authorization/', parameters('roleAssignmentGuid'))]"
```


### <a name="role-assignment-resource-properties"></a>Resource Roltoewijzingseigenschappen
Een roltoewijzing zelf drie eigenschappen gedefinieerd. Het moet de `roleDefinitionId`, `principalId`, en `scope`.

### <a name="role-definition"></a>Roldefinitie
De roldefinitie-ID is de id van de tekenreeks voor de bestaande roldefinitie. De rol-ID in het formulier is `/subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}`. 

De ID wordt verkregen met behulp van abonnement `subscription().subscriptionId` sjabloonfunctie.  

U moet ophalen van de roldefinitie voor de `DevTest Labs User` ingebouwde rol. Om op te halen van de GUID voor de [DevTest Labs-gebruiker](../role-based-access-control/built-in-roles.md#devtest-labs-user) rol, kunt u de [rol toewijzingen REST-API](/rest/api/authorization/roleassignments) of de [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition?view=azps-1.8.0) cmdlet.

```powershell
$dtlUserRoleDefId = (Get-AzRoleDefinition -Name "DevTest Labs User").Id
```

De rol-ID is gedefinieerd in de sectie met variabelen en met de naam `devTestLabUserRoleId`. In de sjabloon is de rol-ID ingesteld op: 111111111-0000-0000-11111111111111111. 

```json
"devTestLabUserRoleId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/111111111-0000-0000-11111111111111111')]",
```

### <a name="principal-id"></a>Principal-id
Principal-ID is de object-ID van de Active Directory-gebruiker, groep of service-principal die u wilt toevoegen als een lab-gebruiker aan het lab. De sjabloon maakt gebruik van de `ObjectId` als parameter.

U kunt de object-id ophalen met behulp van de [Get-AzureRMADUser](/powershell/module/azurerm.resources/get-azurermaduser?view=azurermps-6.13.0), [Get-AzureRMADGroup of [Get-AzureRMADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.13.0) PowerShell-cmdlets. Deze cmdlets retourneren een enkele of lijsten met Active Directory-objecten die een ID-eigenschap, de object-ID die u nodig hebt. Het volgende voorbeeld ziet u hoe u aan de object-ID van een enkele gebruiker bij een bedrijf.

```powershell
$userObjectId = (Get-AzureRmADUser -UserPrincipalName ‘email@company.com').Id
```

U kunt ook de Azure Active Directory PowerShell-cmdlets die zijn [Get-MsolUser](/powershell/module/msonline/get-msoluser?view=azureadps-1.0), [Get-MsolGroup](/powershell/module/msonline/get-msolgroup?view=azureadps-1.0), en [Get-MsolServicePrincipal](/powershell/module/msonline/get-msolserviceprincipal?view=azureadps-1.0).

### <a name="scope"></a>Scope
Bereik Hiermee geeft u de resource of resourcegroep waarvoor de roltoewijzing moet worden toegepast. Het bereik voor resources, heeft de notatie: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}`. De sjabloon maakt gebruik van de `subscription().subscriptionId` functie in te vullen de `subscription-id` onderdeel en de `resourceGroup().name` sjabloonfunctie in te vullen de `resource-group-name` onderdeel. Met behulp van deze functies, betekent dat de testomgeving waarvoor u een rol toewijst moet aanwezig zijn in het huidige abonnement en dezelfde resourcegroep waarmee de sjabloonimplementatie wordt gemaakt. Het laatste deel `resource-name`, is de naam van het testlab. Deze waarde wordt ontvangen via de sjabloonparameter in dit voorbeeld. 

Het bereik van de gebruikersrol in de sjabloon: 

```json
"roleScope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.DevTestLab/labs/', parameters('labName'))]"
```

### <a name="deploying-the-template"></a>Het sjabloon implementeren
Maak eerst een parameterbestand (bijvoorbeeld: azuredeploy.parameters.json) die wordt doorgegeven waarden voor parameters in het Resource Manager-sjabloon. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "value": "11111111-1111-1111-1111-111111111111"
    },
    "labName": {
      "value": "MyLab"
    },
    "roleAssignmentGuid": {
      "value": "22222222-2222-2222-2222-222222222222"
    }
  }
}
```

Vervolgens gebruikt u de [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment?view=azurermps-6.13.0) PowerShell-cmdlet om de Resource Manager-sjabloon te implementeren. De volgende opdracht wordt een persoon, groep of een service-principal toegewezen aan de rol DevTest Labs-gebruiker voor een testomgeving.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateParameterFile .\azuredeploy.parameters.json -TemplateFile .\azuredeploy.json
```

Het is belangrijk te weten dat de implementatie van de naam en de rol groepstoewijzing GUID moet uniek zijn. Als u probeert te implementeren van de toewijzing van een resource met een niet-unieke GUID, dan u krijgt een `RoleAssignmentUpdateNotPermitted` fout.

Als u van plan bent de sjabloon meerdere keren gebruiken aan verschillende Active Directory-objecten toevoegen aan de rol DevTest Labs-gebruiker voor uw lab, kunt u overwegen dynamische objecten in uw PowerShell-opdracht. Het volgende voorbeeld wordt de [New-Guid](/powershell/module/Microsoft.PowerShell.Utility/New-Guid?view=powershell-5.0) cmdlet om op te geven van de resource-implementatie de naam en de rol groepstoewijzing GUID dynamisch.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateFile .\azuredeploy.json -roleAssignmentGuid "$(New-Guid)" -labName "MyLab" -principalId "11111111-1111-1111-1111-111111111111"
```

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
Zoals beschreven in de inleiding, maakt u een nieuwe Azure roltoewijzing voor een gebruiker toevoegen aan de **DevTest Labs-gebruiker** rol voor de testomgeving. In PowerShell kunt u dit doen met behulp van de [New-AzureRMRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment?view=azurermps-6.13.0) cmdlet. Deze cmdlet heeft veel optionele parameters om toe te staan voor flexibiliteit. De `ObjectId`, `SigninName`, of `ServicePrincipalName` kan worden opgegeven als het object worden machtigingen verleend.  

Hier volgt een voorbeeld van Azure PowerShell-opdracht die een gebruiker toegevoegd aan de rol DevTest Labs-gebruiker in het opgegeven lab.

```powershell
New-AzureRmRoleAssignment -UserPrincipalName <email@company.com> -RoleDefinitionName 'DevTest Labs User' -ResourceName '<Lab Name>' -ResourceGroupName '<Resource Group Name>' -ResourceType 'Microsoft.DevTestLab/labs'
```

Om op te geven van de resource waarmee machtigingen worden verleend kan worden opgegeven met een combinatie van `ResourceName`, `ResourceType`, `ResourceGroup` of door de `scope` parameter. Elke combinatie van parameters wordt gebruikt, voldoende informatie gegeven aan de cmdlet voor het aanduiden van de Active Directory-object (gebruiker, groep of service-principal), het bereik (resourcegroep of resource) en de roldefinitie.

## <a name="use-azure-command-line-interface-cli"></a>Gebruik Azure Command Line Interface (CLI)
In de Azure CLI, een labs-gebruiker toe te voegen aan een lab wordt gedaan via de `az role assignment create` opdracht. Zie voor meer informatie over Azure CLI-cmdlets [beheren van toegang tot Azure-resources met behulp van RBAC en Azure CLI](../role-based-access-control/role-assignments-cli.md).

Het object dat toegang wordt verleend, kan worden opgegeven met de `objectId`, `signInName`, `spn` parameters. De testomgeving waarop het object wordt hun toegang wordt verleend, kan worden geïdentificeerd door de `scope` url of een combinatie van de `resource-name`, `resource-type`, en `resource-group` parameters.

De volgende Azure CLI-voorbeeld ziet u een persoon toevoegen aan de rol DevTest Labs-gebruiker voor het opgegeven Lab.  

```azurecli
az role assignment create --roleName "DevTest Labs User" --signInName <email@company.com> -–resource-name "<Lab Name>" --resource-type “Microsoft.DevTestLab/labs" --resource-group "<Resource Group Name>"
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Maken en beheren van virtuele machines met DevTest Labs met de Azure CLI](devtest-lab-vmcli.md)
- [Een virtuele machine maken met DevTest Labs met behulp van Azure PowerShell](devtest-lab-vm-powershell.md)
- [Opdrachtregelprogramma's gebruiken om te starten en stoppen van virtuele machines van Azure DevTest Labs](use-command-line-start-stop-virtual-machines.md)

