---
title: Aangepaste rollen voor Azure-resources met behulp van Azure CLI maken | Microsoft Docs
description: Informatie over het maken van aangepaste rollen met op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources met behulp van Azure CLI. Dit omvat het weergeven, maken, bijwerken en verwijderen van aangepaste rollen.
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
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ebced83346a7b130598e4a5f49a72d51ffd18e4f
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56584893"
---
# <a name="create-custom-roles-for-azure-resources-using-azure-cli"></a>Aangepaste rollen maken voor Azure-resources met behulp van Azure CLI

Als de [ingebouwde rollen voor Azure-resources](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. In dit artikel wordt beschreven hoe u maken en beheren van aangepaste rollen met behulp van Azure CLI.

Zie voor een stapsgewijze zelfstudie voor het maken van een aangepaste rol [zelfstudie: Maken van een aangepaste rol voor Azure-resources met behulp van Azure CLI](tutorial-custom-role-cli.md).

## <a name="prerequisites"></a>Vereisten

Voor het maken van aangepaste rollen, hebt u het volgende nodig:

- Machtigingen voor het maken van aangepaste rollen, zoals [Eigenaar](built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator)
- [Azure Cloudshell](../cloud-shell/overview.md) of [Azure CLI](/cli/azure/install-azure-cli)

## <a name="list-custom-roles"></a>Aangepaste rollen opvragen

U kunt aangepaste rollen die beschikbaar voor toewijzing zijn gebruiken [az role definitielijst](/cli/azure/role/definition#az-role-definition-list). De volgende voorbeelden ziet u alle aangepaste rollen in het huidige abonnement.

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "roleType":.roleType}'
```

```azurecli
az role definition list --output json | jq '.[] | if .roleType == "CustomRole" then {"roleName":.roleName, "roleType":.roleType} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}
{
  "roleName": "Virtual Machine Operator",
  "type": "CustomRole"
}

...
```

## <a name="list-a-custom-role-definition"></a>Lijst van een aangepaste roldefinitie

U kunt een aangepaste roldefinitie gebruiken [az role definitielijst](/cli/azure/role/definition#az-role-definition-list). Dit is dezelfde opdracht die u voor een ingebouwde rol gebruiken zou.

```azurecli
az role definition list --name <role_name>
```

De volgende voorbeeld ziet u de *virtuele Machine-Operator* roldefinitie:

```azurecli
az role definition list --name "Virtual Machine Operator"
```

```Output
[
  {
    "assignableScopes": [
      "/subscriptions/11111111-1111-1111-1111-111111111111"
    ],
    "description": "Can monitor and restart virtual machines.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/00000000-0000-0000-0000-000000000000",
    "name": "00000000-0000-0000-0000-000000000000",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/*/read",
          "Microsoft.Network/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action",
          "Microsoft.Authorization/*/read",
          "Microsoft.ResourceHealth/availabilityStatuses/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Insights/diagnosticSettings/*",
          "Microsoft.Support/*"
        ],
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Virtual Machine Operator",
    "roleType": "CustomRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Het volgende voorbeeld worden alleen de acties van de *virtuele Machine-Operator* rol:

```azurecli
az role definition list --name "Virtual Machine Operator" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Storage/*/read",
  "Microsoft.Network/*/read",
  "Microsoft.Compute/*/read",
  "Microsoft.Compute/virtualMachines/start/action",
  "Microsoft.Compute/virtualMachines/restart/action",
  "Microsoft.Authorization/*/read",
  "Microsoft.ResourceHealth/availabilityStatuses/read",
  "Microsoft.Resources/subscriptions/resourceGroups/read",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Insights/diagnosticSettings/*",
  "Microsoft.Support/*"
]
```

## <a name="create-a-custom-role"></a>Een aangepaste rol maken

Gebruik voor het maken van een aangepaste rol [az roldefinitie maken](/cli/azure/role/definition#az-role-definition-create). De roldefinitie mag een JSON-beschrijving of een pad naar een bestand met een JSON-beschrijving.

```azurecli
az role definition create --role-definition <role_definition>
```

Het volgende voorbeeld wordt een aangepaste rol met de naam *virtuele Machine-Operator*. Deze aangepaste rol toegang toegewezen aan alle leesbewerkingen van *Microsoft.Compute*, *Microsoft.Storage*, en *Microsoft.Network* providers en toegewezen toegang tot resources Als u wilt starten, opnieuw starten en controleren van virtuele machines. Deze aangepaste rol kan worden gebruikt in twee abonnementen. Dit voorbeeld wordt een JSON-bestand als invoer.

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
    "Microsoft.ResourceHealth/availabilityStatuses/read",
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

## <a name="update-a-custom-role"></a>Een aangepaste rol bijwerken

Voor het bijwerken van een aangepaste rol voor het eerst gebruiken [az role definitielijst](/cli/azure/role/definition#az-role-definition-list) om op te halen van de roldefinitie. Controleer vervolgens de gewenste wijzigingen aan de roldefinitie van de. Gebruik tot slot [az role definition update](/cli/azure/role/definition#az-role-definition-update) om op te slaan van de definitie van de bijgewerkte rol.

```azurecli
az role definition update --role-definition <role_definition>
```

Het volgende voorbeeld wordt de *Microsoft.Insights/diagnosticSettings/* bewerking naar de *acties* van de *virtuele Machine-Operator* aangepaste rol.

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
    "Microsoft.ResourceHealth/availabilityStatuses/read",
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

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Als u wilt een aangepaste rol verwijderen, gebruikt u [az roldefinitie verwijderen](/cli/azure/role/definition#az-role-definition-delete). Om op te geven van de rol wilt verwijderen, gebruikt u de rolnaam van de of de rol-ID. Gebruiken om te bepalen van de rol-ID, [az role definitielijst](/cli/azure/role/definition#az-role-definition-list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Hiermee verwijdert u het volgende voorbeeld de *virtuele Machine-Operator* aangepaste rol.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Maken van een aangepaste rol voor Azure-resources met behulp van Azure CLI](tutorial-custom-role-cli.md)
- [Aangepaste rollen voor Azure-resources](custom-roles.md)
- [Azure Resource Manager-resourceproviderbewerkingen](resource-provider-operations.md)