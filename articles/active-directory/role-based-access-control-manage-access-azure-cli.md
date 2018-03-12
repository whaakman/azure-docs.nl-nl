---
title: Beheren van op rollen gebaseerde toegangsbeheer (RBAC) met Azure CLI | Microsoft Docs
description: Informatie over het beheren van op rollen gebaseerde toegangsbeheer (RBAC) met de Azure-opdrachtregelinterface lijst met functies en functie acties en rollen toewijzen aan de scopes abonnement en de toepassing.
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 6c9df11e528601d94cb72a8e3ef0868dc7781e12
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Toegangsbeheer op basis van rollen met de Azure-opdrachtregelinterface beheren

> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure-CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


Met op rollen gebaseerde toegangsbeheer (RBAC), definiëren u toegang voor gebruikers, groepen en service-principals door het toewijzen van rollen bij een bepaald bereik. Dit artikel wordt beschreven hoe u voor het beheren van toegang met behulp van de Azure-opdrachtregelinterface (CLI).

## <a name="prerequisites"></a>Vereisten

Als u de Azure CLI wilt RBAC beheren, hebt u de volgende vereisten:

* [Azure CLI 2.0](/cli/azure). U kunt deze in uw browser gebruiken met [Azure Cloud Shell](../cloud-shell/overview.md), of [installeren](/cli/azure/install-azure-cli) op macOS, Linux of Windows en uitvoeren vanaf de opdrachtregel.

## <a name="list-roles"></a>Lijst met rollen

### <a name="list-role-definitions"></a>Roldefinities lijst

U kunt alle beschikbare roldefinities gebruiken [az rol definitielijst](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list
```

Het volgende voorbeeld ziet u de naam en beschrijving van alle beschikbare roldefinities:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

Het volgende voorbeeld worden alle van de ingebouwde functiedefinities:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description, "type":.properties.type}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "type": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role-definition"></a>Van Lijstacties van een roldefinitie

U kunt de acties van een roldefinitie gebruiken [az rol definitielijst](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list --name <role_name>
```

De volgende voorbeeld ziet u de *Inzender* roldefinitie:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "properties": {
      "additionalProperties": {
        "createdBy": null,
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedBy": null,
        "updatedOn": "2016-12-14T02:04:45.1393855Z"
      },
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything except access to resources.",
      "permissions": [
        {
          "actions": [
            "*"
          ],
          "notActions": [
            "Microsoft.Authorization/*/Delete",
            "Microsoft.Authorization/*/Write",
            "Microsoft.Authorization/elevateAccess/Action"
          ]
        }
      ],
      "roleName": "Contributor",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

De volgende voorbeeld ziet u de *acties* en *notActions* van de *Inzender* rol:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.properties.permissions[0].actions, "notActions":.properties.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

Het volgende voorbeeld worden de acties van de *Virtual Machine Contributor* rol:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .properties.permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>Lijst met toegang

### <a name="list-role-assignments-for-a-user"></a>Lijst roltoewijzingen voor een gebruiker

U kunt de roltoewijzingen voor een specifieke gebruiker gebruiken [az rol toewijzingslijst](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --assignee <assignee>
```

Standaard worden alleen toewijzingen binnen het bereik van abonnement wordt weergegeven. Als u wilt weergeven van de toewijzingen van ingedeeld per resource of groep gebruiken `--all`.

Het volgende voorbeeld worden de roltoewijzingen die zijn toegewezen rechtstreeks naar de  *patlong@contoso.com*  gebruiker:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.properties.principalName, "roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Lijst roltoewijzingen voor een resourcegroep

U kunt de roltoewijzingen die beschikbaar zijn voor een resourcegroep gebruiken [az rol toewijzingslijst](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Het volgende voorbeeld worden de roltoewijzing voor de *pharma-verkoop-projectforecast* resourcegroep:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}

...
```

## <a name="assign-access"></a>Toegang toewijzen

### <a name="assign-a-role-to-a-user"></a>Een rol toewijzen aan een gebruiker

Gebruiken om een rol toewijzen aan een gebruiker op het groepsbereik resource, [az roltoewijzing maken](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

Het volgende voorbeeld wordt de *Virtual Machine Contributor* rol  *patlong@contoso.com*  gebruiker op de *pharma-verkoop-projectforecast* groepsbereik resource:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="assign-a-role-to-a-group"></a>Een rol toewijzen aan een groep

Als u wilt een rol toewijzen aan een groep, gebruik [az roltoewijzing maken](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Het volgende voorbeeld wordt de *lezer* rol de *Anne Mack Team* groep met ID 22222222-2222-2222-2222-222222222222 bij het abonnementsbereik. Als u de ID van de groep, kunt u [az ad groepslijst](/cli/azure/ad/group#az_ad_group_list) of [az ad-groep weergeven](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

Het volgende voorbeeld wordt de *Virtual Machine Contributor* rol de *Anne Mack Team* groep met ID 22222222-2222-2222-2222-222222222222 op een scope resource voor een virtueel netwerk met de naam *pharma-verkoop-project-network*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="assign-a-role-to-an-application"></a>Een rol toewijzen aan een toepassing

Als u wilt een rol toewijzen aan een toepassing, gebruik [az roltoewijzing maken](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Het volgende voorbeeld wordt de *Virtual Machine Contributor* role in een toepassing met object-ID 44444444-4444-4444-4444-444444444444 op de *pharma-verkoop-projectforecast* resourcegroep de scope. Als u de object-ID van de toepassing, kunt u [az ad applijst](/cli/azure/ad/app#az_ad_app_list) of [az ad app weergeven](/cli/azure/ad/app#az_ad_app_show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Toegang verwijderen

### <a name="remove-a-role-assignment"></a>Een roltoewijzing verwijderen

U kunt een roltoewijzing verwijderen [az roltoewijzing verwijderen](/cli/azure/role/assignment#az_role_assignment_delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

Het volgende voorbeeld verwijdert u de *Virtual Machine Contributor* functietoewijzing uit de  *patlong@contoso.com*  gebruiker op de *pharma-verkoop-projectforecast* resource groep:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

Het volgende voorbeeld verwijdert u de *lezer* functie uit de *Anne Mack Team* groep met ID 22222222-2222-2222-2222-222222222222 bij het abonnementsbereik. Als u de ID van de groep, kunt u [az ad groepslijst](/cli/azure/ad/group#az_ad_group_list) of [az ad-groep weergeven](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="custom-roles"></a>Aangepaste rollen

### <a name="list-custom-roles"></a>Lijst met aangepaste rollen

U kunt de functies die beschikbaar voor toewijzing op een scope zijn gebruiken [az rol definitielijst](/cli/azure/role/definition#az_role_definition_list).

Beide van de volgende voorbeelden tonen de aangepaste rollen in het huidige abonnement:

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.properties.roleName, "type":.properties.type}'
```

```azurecli
az role definition list --output json | jq '.[] | if .properties.type == "CustomRole" then {"roleName":.properties.roleName, "type":.properties.type} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Operator Role",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}

...
```

### <a name="create-a-custom-role"></a>Een aangepaste beveiligingsrol maken

Gebruik voor het maken van een aangepaste beveiligingsrol [az roldefinitie maken](/cli/azure/role/definition#az_role_definition_create). De roldefinitie mag een JSON-beschrijving of een pad naar een bestand met een JSON-beschrijving.

```azurecli
az role definition create --role-definition <role_definition>
```

Het volgende voorbeeld wordt een aangepaste beveiligingsrol met de naam *virtuele Machine Operator*. Deze aangepaste rol toegang toegewezen aan alle leesbewerkingen van *Microsoft.Compute*, *Microsoft.Storage*, en *Microsoft.Network* providers en wordt toegewezen toegang tot bedrijfsbronnen Als u wilt starten, start en virtuele machines bewaken. Deze aangepaste rol kan worden gebruikt in twee abonnementen. Dit voorbeeld wordt een JSON-bestand als invoer.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

### <a name="update-a-custom-role"></a>Bijwerken van een aangepaste rol

Voor het bijwerken van een aangepaste rol voor het eerst gebruiken [az rol definitielijst](/cli/azure/role/definition#az_role_definition_list) voor het ophalen van de functiedefinitie. Controleer vervolgens de gewenste wijzigingen aan de functiedefinitie. Gebruik tot slot [az rol definitie-update](/cli/azure/role/definition#az_role_definition_update) om op te slaan van de bijgewerkte roldefinitie.

```azurecli
az role definition update --role-definition <role_definition>
```

Het volgende voorbeeld wordt de *Microsoft.Insights/diagnosticSettings/* bewerking is de *acties* van de *virtuele Machine Operator* aangepaste rol.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

### <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Gebruik voor het verwijderen van een aangepaste beveiligingsrol [az roldefinitie verwijderen](/cli/azure/role/definition#az_role_definition_delete). Om op te geven van de rol wilt verwijderen, gebruikt u de rolnaam van de of de rol-ID. Gebruiken om te bepalen van de rol-ID, [az rol definitielijst](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Het volgende voorbeeld wordt de *virtuele Machine Operator* aangepaste rol:

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

