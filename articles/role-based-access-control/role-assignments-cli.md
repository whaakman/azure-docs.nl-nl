---
title: Beheer de toegang tot Azure-resources met behulp van RBAC en Azure CLI | Microsoft Docs
description: Informatie over het beheren van toegang tot Azure-resources voor gebruikers, groepen en toepassingen die gebruikmaken van op rollen gebaseerd toegangsbeheer (RBAC) en Azure CLI. U vindt hier instructies voor het weergeven van toegang, het verlenen van toegang en het intrekken van toegang.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 751f582e2cfc39b62194ec55efa5cd8580c001e3
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341709"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-cli"></a>Beheer de toegang tot Azure-resources met behulp van RBAC en Azure CLI

[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) is de manier waarop dat u de toegang tot Azure-resources beheren. Dit artikel wordt beschreven hoe u de toegang voor gebruikers, groepen en toepassingen die gebruikmaken van RBAC en Azure CLI beheren.

## <a name="prerequisites"></a>Vereisten

Voor het beheren van toegang, hebt u een van de volgende nodig:

* [Bash in Azure Cloudshell](/azure/cloud-shell/overview)
* [Azure-CLI](/cli/azure)

## <a name="list-roles"></a>Rollen opvragen

U kunt alle beschikbare roldefinities gebruiken [az role definitielijst](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list
```

Het volgende voorbeeld worden de naam en beschrijving van alle beschikbare roldefinities:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
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

Het volgende voorbeeld worden alle van de ingebouwde roldefinities:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role"></a>Lijst met acties van een rol

U kunt de acties van een roldefinitie gebruiken [az role definitielijst](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list --name <role_name>
```

De volgende voorbeeld ziet u de *Inzender* roldefinitie:

```azurecli
az role definition list --name "Contributor"
```

```Output
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

De volgende voorbeeld ziet u de *acties* en *notActions* van de *Inzender* rol:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
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

Het volgende voorbeeld worden de acties van de *Inzender voor virtuele machines* rol:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
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

## <a name="list-access"></a>Toegang opvragen

In RBAC, de toegang van de lijst, u lijst maken met de roltoewijzingen.

### <a name="list-role-assignments-for-a-user"></a>Roltoewijzingen voor een gebruiker weergeven

U kunt de roltoewijzingen voor een specifieke gebruiker gebruiken [lijst van zonetoewijzingen rol az](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --assignee <assignee>
```

Standaard wordt alleen toewijzingen binnen het bereik van abonnement worden weergegeven. Als u wilt weergeven toewijzingen ingedeeld per resource of groep, gebruikt u `--all`.

Het volgende voorbeeld worden de roltoewijzingen die zijn toegewezen rechtstreeks naar de *patlong@contoso.com* gebruiker:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
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

### <a name="list-role-assignments-for-a-resource-group"></a>Roltoewijzingen voor een resourcegroep opvragen

U kunt de toewijzing van functies die beschikbaar zijn voor een resourcegroep gebruiken [lijst van zonetoewijzingen rol az](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Het volgende voorbeeld worden de roltoewijzingen voor de *pharma-verkoop-projectforecast* resourcegroep:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.roleDefinitionName, "scope":.scope}'
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

## <a name="grant-access"></a>Toegang verlenen

In RBAC verleent u toegang door een roltoewijzing te maken.

### <a name="create-a-role-assignment-for-a-user"></a>Een roltoewijzing voor een gebruiker maken

Gebruik voor het maken van een roltoewijzing voor een gebruiker op de resource-groepsbereik [az roltoewijzing maken](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

In het volgende voorbeeld wordt de *Inzender voor virtuele machines* rol *patlong@contoso.com* gebruiker op de *pharma-verkoop-projectforecast* groepsbereik van de resource:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="create-a-role-assignment-for-a-group"></a>Een roltoewijzing voor een groep maken

Gebruik voor het maken van een roltoewijzing voor een groep [az roltoewijzing maken](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

In het volgende voorbeeld wordt de *lezer* rol die u wilt de *Anne Mack Team* groep met ID 22222222-2222-2222-2222-222222222222 op het abonnementsbereik. Als u de ID van de groep, kunt u [az ad group list](/cli/azure/ad/group#az-ad-group-list) of [az ad group show](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

In het volgende voorbeeld wordt de *Inzender voor virtuele machines* rol die u wilt de *Anne Mack Team* groep met ID 22222222-2222-2222-2222-222222222222 met een resource-bereik voor een virtueel netwerk met de naam *pharma-verkoop-project-netwerk*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application"></a>Een roltoewijzing voor een toepassing maken

Gebruik voor het maken van een rol voor een toepassing [az roltoewijzing maken](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

In het volgende voorbeeld wordt de *Inzender voor virtuele machines* rol aan een toepassing met object-ID 44444444-4444-4444-4444-444444444444 op de *pharma-verkoop-projectforecast* resourcegroep de scope. Als u de object-ID van de toepassing, kunt u [az ad app-lijst](/cli/azure/ad/app#az-ad-app-list) of [az ad app show](/cli/azure/ad/app#az-ad-app-show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Toegang intrekken

In RBAC, als u wilt verwijderen van toegang, verwijdert u een roltoewijzing met behulp van [az-roltoewijzing verwijderen](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

Het volgende voorbeeld verwijdert u de *Inzender voor virtuele machines* roltoewijzing van de *patlong@contoso.com* gebruiker op de *pharma-verkoop-projectforecast* resource groep:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

Het volgende voorbeeld verwijdert u de *lezer* rol van de *Anne Mack Team* groep met ID 22222222-2222-2222-2222-222222222222 op het abonnementsbereik. Als u de ID van de groep, kunt u [az ad group list](/cli/azure/ad/group#az-ad-group-list) of [az ad group show](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Maken van een aangepaste rol voor Azure-resources met behulp van Azure CLI](tutorial-custom-role-cli.md)
- [De Azure CLI gebruiken voor het beheren van Azure-resources en resourcegroepen](../azure-resource-manager/cli-azure-resource-manager.md)