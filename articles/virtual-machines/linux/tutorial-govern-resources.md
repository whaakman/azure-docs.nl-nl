---
title: Azure virtuele machines met Azure CLI reguleren | Microsoft Docs
description: Zelfstudie - virtuele machines in Azure beheren door toe te passen RBAC, beleidsregels, vergrendelingen en labels met Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: tomfitz
ms.openlocfilehash: 86ac3ec390c7aa9bc24a90ef2ee582f97f8b5407
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="virtual-machine-governance-with-azure-cli"></a>Beheer van de virtuele machine met Azure CLI

[!include[Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, Zie [2.0 voor Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="understand-scope"></a>Bereik begrijpen

[!include[Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

In deze zelfstudie maakt u alle management instellingen toepassen op een resourcegroep zodat u deze instellingen wanneer u klaar bent eenvoudig kunt verwijderen.

We maken die resourcegroep.

```azurecli-interactive
az group create --name myResourceGroup --location "East US"
```

De resourcegroep is momenteel leeg.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

U wilt controleren of gebruikers in uw organisatie hebben het juiste niveau van toegang tot deze bronnen. U niet wilt onbeperkte toegang verlenen aan gebruikers, maar moet u ook om ervoor te zorgen dat ze hun werk te kunnen doen. [Toegangsbeheer op basis van rollen](../../active-directory/role-based-access-control-what-is.md) kunt u beheren welke gebruikers hebben een machtiging voor het voltooien van specifieke acties op een scope.

Als u wilt maken en verwijderen van roltoewijzingen, moeten gebruikers hebben `Microsoft.Authorization/roleAssignments/*` toegang. Deze toegang wordt verleend door middel van de eigenaar of beheerder voor gebruikerstoegang rollen.

Voor het beheren van virtuele machine oplossingen, zijn er drie resourcespecifieke rollen die vaak nodig toegang bieden:

* [Virtual Machine Contributor](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor)
* [Inzender voor netwerken](../../active-directory/role-based-access-built-in-roles.md#network-contributor)
* [Storage-Account Inzender](../../active-directory/role-based-access-built-in-roles.md#storage-account-contributor)

In plaats van de rollen toewijzen aan individuele gebruikers, is het vaak eenvoudiger [een Azure Active Directory-groep maken](../../active-directory/active-directory-groups-create-azure-portal.md) voor gebruikers hoeven vergelijkbare acties te ondernemen. Vervolgens die groep toewijzen aan de juiste rol. Om te vereenvoudigen in dit artikel, moet u een Azure Active Directory-groep zonder leden maken. U kunt deze groep nog steeds toewijzen aan een rol voor een scope. 

Het volgende voorbeeld wordt een Azure Active Directory-groep met de naam *VMDemoContributors* met een e-mail bijnaam van *vmDemoGroup*. De mail-bijnaam fungeert als een alias voor de groep.

```azurecli-interactive
adgroupId=$(az ad group create --display-name VMDemoContributors --mail-nickname vmDemoGroup --query objectId --output tsv)
```

Het duurt even na de opdrachtprompt voor de groep worden doorgegeven in Azure Active Directory als resultaat geven. Gebruik na een wachttijd van 20 of 30 seconden, de [az roltoewijzing maken](/cli/azure/role/assignment#az_role_assignment_create) opdracht voor het toewijzen van de nieuwe Azure Active Directory-groep aan de rol Inzender van de virtuele Machine voor de resourcegroep.  Als u de volgende opdracht uitvoeren voordat deze is doorgegeven, ontvangt u een foutbericht waarin wordt gemeld **Principal <guid> bestaat niet in de map**. Probeer de opdracht opnieuw uit te voeren.

```azurecli-interactive
az role assignment create --assignee-object-id $adgroupId --role "Virtual Machine Contributor" --resource-group myResourceGroup
```

Normaal gesproken u het proces voor herhalen *Network Contributor* en *Storage Account Inzender* om ervoor te zorgen dat gebruikers worden toegewezen aan het geïmplementeerde resources beheren. In dit artikel kunt u deze stappen overslaan.

## <a name="azure-policies"></a>Azure-beleid

[!include[Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Beleid toepassen

Uw abonnement al heeft meerdere beleidsdefinities. Als de beleidsdefinities beschikbaar weergeven, gebruikt de [az definitie beleidslijst](/cli/azure/policy/definition#az_policy_definition_list) opdracht:

```azurecli-interactive
az policy definition list --query "[].[displayName, policyType, name]" --output table
```

Ziet u de bestaande beleidsdefinities. Het type beleid dat is **BuiltIn** of **aangepaste**. Bekijk de definities voor apparaten die worden beschreven van een voorwaarde die u wilt toewijzen. In dit artikel leert toewijzen u beleid die:

* De locaties voor alle resources worden beperkt.
* De SKU's voor virtuele machines beperkt.
* Virtuele machines die geen van beheerde schijven gebruikmaken controleren.

In het volgende voorbeeld moet u drie beleidsdefinities op basis van de weergavenaam ophalen. U gebruikt de [az beleidstoewijzing maken](/cli/azure/policy/assignment#az_policy_assignment_create) opdracht deze definities toewijzen aan de resourcegroep. Voor sommige beleidsregels, kunt u parameterwaarden om op te geven van de toegestane waarden opgeven.

```azurecli-interactive
# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
locationDefinition=$(az policy definition list --query "[?displayName=='Allowed locations'].name | [0]" --output tsv)
skuDefinition=$(az policy definition list --query "[?displayName=='Allowed virtual machine SKUs'].name | [0]" --output tsv)
auditDefinition=$(az policy definition list --query "[?displayName=='Audit VMs that do not use managed disks'].name | [0]" --output tsv)

# Assign policy for allowed locations
az policy assignment create --name "Set permitted locations" \
  --resource-group myResourceGroup \
  --policy $locationDefinition \
  --params '{ 
      "listOfAllowedLocations": {
        "value": [
          "eastus", 
          "eastus2"
        ]
      }
    }'

# Assign policy for allowed SKUs
az policy assignment create --name "Set permitted VM SKUs" \
  --resource-group myResourceGroup \
  --policy $skuDefinition \
  --params '{ 
      "listOfAllowedSKUs": {
        "value": [
          "Standard_DS1_v2", 
          "Standard_E2s_v2"
        ]
      }
    }'

# Assign policy for auditing unmanaged disks
az policy assignment create --name "Audit unmanaged disks" \
  --resource-group myResourceGroup \
  --policy $auditDefinition
```

Het voorgaande voorbeeld wordt ervan uitgegaan dat u al weet dat de parameters voor een beleid. Als u de parameters weergeven moet, gebruikt:

```azurecli-interactive
az policy definition show --name $locationDefinition --query parameters
```

## <a name="deploy-the-virtual-machine"></a>Implementeer de virtuele machine

U kunt functies en het beleid hebt toegewezen, zodat u klaar bent om uw oplossing implementeren. De standaardgrootte is Standard_DS1_v2, namelijk een van de toegestane SKU's. De opdracht maakt u SSH-sleutels als ze niet bestaan op de standaardlocatie.

```azurecli-interactive
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Nadat de implementatie is voltooid, kunt u meer instellingen toepassen op de oplossing.

## <a name="lock-resources"></a>Resources vergrendelen

[Resource vergrendelingen](../../azure-resource-manager/resource-group-lock-resources.md) te voorkomen dat gebruikers in uw organisatie per ongeluk worden kritieke bronnen wijzigen of verwijderen. In tegenstelling tot toegangsbeheer op basis van functie toepassen resource vergrendelingen een beperking voor alle gebruikers en rollen. U kunt de vergrendeling op instellen *CanNotDelete* of *ReadOnly*.

Als u wilt maken of verwijderen van management vergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/locks/*` acties. Van de ingebouwde rollen alleen **eigenaar** en **beheerder voor gebruikerstoegang** deze acties worden verleend.

Als u wilt vergrendelen op de virtuele machine en de netwerkbeveiligingsgroep, gebruiken de [az vergrendeling maken](/cli/azure/lock#az_lock_create) opdracht:

```azurecli-interactive
# Add CanNotDelete lock to the VM
az lock create --name LockVM \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVM \
  --resource-type Microsoft.Compute/virtualMachines

# Add CanNotDelete lock to the network security group
az lock create --name LockNSG \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVMNSG \
  --resource-type Microsoft.Network/networkSecurityGroups
```

Als u wilt testen vergrendelingen zijn opgegeven, probeert de volgende opdracht uit te voeren:

```azurecli-interactive 
az group delete --name myResourceGroup
```

Er is een fout die aangeeft dat de delete-bewerking kan niet worden uitgevoerd vanwege een vergrendeling. De resourcegroep kan alleen worden verwijderd als u de vergrendelingen specifiek verwijdert. Deze stap wordt weergegeven in [resources opschonen](#clean-up-resources).

## <a name="tag-resources"></a>Tag resources

U hebt toegepast [labels](../../azure-resource-manager/resource-group-using-tags.md) voor uw Azure-resources aan een logische manier te organiseren in categorieën. Elke tag bestaat uit een naam en een waarde. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

[!include[Resource Manager governance tags CLI](../../../includes/resource-manager-governance-tags-cli.md)]

Gebruiken om tags toepassen op een virtuele machine, de [az resource tag](/cli/azure/resource#az_resource_tag) opdracht. Eventuele bestaande labels op de bron blijven niet behouden.

```azurecli-interactive
az resource tag -n myVM \
  -g myResourceGroup \
  --tags Dept=IT Environment=Test Project=Documentation \
  --resource-type "Microsoft.Compute/virtualMachines"
```

### <a name="find-resources-by-tag"></a>Resources zoeken op label

Gebruiken om resources te zoeken met een naam en waarde, de [az resourcelijst](/cli/azure/resource#az_resource_list) opdracht:

```azurecli-interactive
az resource list --tag Environment=Test --query [].name
```

U kunt de geretourneerde waarden gebruiken voor beheertaken zoals alle virtuele machines met een tagwaarde wordt gestopt.

```azurecli-interactive
az vm stop --ids $(az resource list --tag Environment=Test --query "[?type=='Microsoft.Compute/virtualMachines'].id" --output tsv)
```

### <a name="view-costs-by-tag-values"></a>Kosten weergeven door labelwaarden

[!include[Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Resources opschonen

De vergrendelde netwerkbeveiligingsgroep kan niet worden verwijderd nadat de vergrendeling wordt verwijderd. Voor het verwijderen van de vergrendeling ophalen van de id's van de vergrendelingen en bieden ze de [az vergrendeling verwijderen](/cli/azure/lock#az_lock_delete) opdracht:

```azurecli-interactive
vmlock=$(az lock show --name LockVM \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Compute/virtualMachines \
  --resource-name myVM --output tsv --query id)
nsglock=$(az lock show --name LockNSG \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Network/networkSecurityGroups \
  --resource-name myVMNSG --output tsv --query id)
az lock delete --ids $vmlock $nsglock
```

U kunt de opdracht [az group delete](/cli/azure/group#az_group_delete) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt. Sluit SSH-sessie met uw virtuele machine af en verwijder vervolgens de resources als volgt:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aangepaste installatiekopie voor een virtuele machine gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Gebruikers toewijzen aan een rol
> * Beleidsregels die standaarden afdwingen
> * Kritieke bronnen met vergrendelingen beveiligen
> * Tag-resources voor facturering en -beheer

Ga naar de volgende zelfstudie voor meer informatie over hoe maximaal beschikbare virtuele machines.

> [!div class="nextstepaction"]
> [Virtuele machines bewaken](tutorial-monitoring.md)

