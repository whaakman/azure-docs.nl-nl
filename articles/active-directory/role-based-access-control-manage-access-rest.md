---
title: Op rollen gebaseerde toegangsbeheer met de REST - Azure AD | Microsoft Docs
description: Op rollen gebaseerde toegangsbeheer met de REST-API beheren
services: active-directory
documentationcenter: na
author: andredm7
manager: mtillman
editor: 
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: active-directory
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: andredm
ms.openlocfilehash: 9ec64dc3ce95de9c29331699ad2140e5a3c25673
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-role-based-access-control-with-the-rest-api"></a>Op rollen gebaseerde toegangsbeheer met de REST-API beheren
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure-CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

Op rollen gebaseerde toegangsbeheer (RBAC) in de Azure portal en Azure Resource Manager-API kunt u toegang tot uw abonnement en bronnen op een heel nauwkeurig beheren. Met deze functie kunt u toegang tot Active Directory-gebruikers, groepen of service-principals verlenen door sommige rollen toewijzen aan deze bij een bepaald bereik.

## <a name="list-all-role-assignments"></a>Lijst van alle roltoewijzingen
Geeft een lijst van alle roltoewijzingen aan het opgegeven bereik en subscopes.

Aan de lijst roltoewijzingen, u moet toegang hebben tot `Microsoft.Authorization/roleAssignments/read` bewerking in het bereik. De ingebouwde rollen krijgen toegangsrechten voor deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **ophalen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het bereik waarvoor u wilt de roltoewijzingen lijst. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{api-versie}* met 2015-07-01.
3. Vervang *{filter}* met de voorwaarde die u wilt toepassen om de rol toewijzingslijst te filteren:

   * Roltoewijzingen voor alleen het opgegeven bereik niet met inbegrip van de roltoewijzingen op subscopes lijst:`atScope()`    
   * Roltoewijzingen lijst voor een specifieke gebruiker, groep of toepassing:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
   * Lijst van roltoewijzingen voor een specifieke gebruiker, inclusief overgenomen van groepen |`assignedTo('{objectId of user}')`

### <a name="response"></a>Antwoord
Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Informatie ophalen over een roltoewijzing
Hiermee haalt u informatie over één roltoewijzing opgegeven door de id van de toewijzing van rollen.

Als u informatie over een roltoewijzing, u moet toegang hebben tot `Microsoft.Authorization/roleAssignments/read` bewerking. De ingebouwde rollen krijgen toegangsrechten voor deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **ophalen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het bereik waarvoor u wilt de roltoewijzingen lijst. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{toewijzing-rol-id}* met de id van de GUID van de roltoewijzing.
3. Vervang *{api-versie}* met 2015-07-01.

### <a name="response"></a>Antwoord
Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken
Een roltoewijzing bij het opgegeven bereik voor de opgegeven principal verlenen van de opgegeven rol maken.

Voor het maken van een roltoewijzing, u moet toegang hebben tot `Microsoft.Authorization/roleAssignments/write` bewerking. Van de ingebouwde rollen alleen *eigenaar* en *beheerder voor gebruikerstoegang* krijgen toegang tot deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **plaatsen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het bereik waarmee u wilt maken van de roltoewijzingen. Wanneer u een roltoewijzing op een bovenliggend bereik maakt, nemen alle onderliggende bereiken de dezelfde toewijzing van rollen. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1   
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{toewijzing-rol-id}* met een nieuwe GUID die de GUID-id van de nieuwe roltoewijzing wordt.
3. Vervang *{api-versie}* met 2015-07-01.

Geef de waarden in de volgende notatie voor de hoofdtekst van de aanvraag:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Elementnaam | Vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| roleDefinitionId |Ja |Tekenreeks |De id van de rol. De indeling van de id is:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId |Ja |Tekenreeks |object-id van de Azure AD-principal waaraan de rol is toegewezen (gebruiker, groep of service-principal). |

### <a name="response"></a>Antwoord
Statuscode: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Een roltoewijzing verwijderen
Een roltoewijzing bij het opgegeven bereik verwijderen.

Als u wilt een roltoewijzing hebt verwijderd, u moet toegang hebben tot de `Microsoft.Authorization/roleAssignments/delete` bewerking. Van de ingebouwde rollen alleen *eigenaar* en *beheerder voor gebruikerstoegang* krijgen toegang tot deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **verwijderen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het bereik waarmee u wilt maken van de roltoewijzingen. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{toewijzing-rol-id}* met de roltoewijzings-id GUID.
3. Vervang *{api-versie}* met 2015-07-01.

### <a name="response"></a>Antwoord
Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Lijst van alle rollen
Geeft een lijst van alle functies die beschikbaar voor toewijzing op het opgegeven bereik zijn.

Lijst met rollen, u moet toegang hebben tot `Microsoft.Authorization/roleDefinitions/read` bewerking in het bereik. De ingebouwde rollen krijgen toegangsrechten voor deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **ophalen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het bereik waarvoor u wilt weergeven van de rollen. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{api-versie}* met 2015-07-01.
3. Vervang *{filter}* met de voorwaarde die u wilt toepassen om te filteren op de lijst met rollen:

   * Lijst met beschikbare rollen voor toewijzing bij het opgegeven bereik en een van de onderliggende scopes van:`atScopeAndBelow()`
   * Zoeken naar een rol met de exacte weergavenaam: `roleName%20eq%20'{role-display-name}'`. Gebruik het URL-gecodeerd-formulier van de exacte weergavenaam van de rol. Bijvoorbeeld:`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Antwoord
Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Informatie ophalen over een rol
Hiermee haalt u informatie over een enkele rol die is opgegeven door de rol definitie-id. Zie voor informatie over de weergegeven naam met één functie, [lijst van alle rollen](role-based-access-control-manage-access-rest.md#list-all-roles).

Als u informatie over een rol, u moet toegang hebben tot `Microsoft.Authorization/roleDefinitions/read` bewerking. De ingebouwde rollen krijgen toegangsrechten voor deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **ophalen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het bereik waarvoor u wilt de roltoewijzingen lijst. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{rol-definitie-id}* met de id van de GUID van de functiedefinitie.
3. Vervang *{api-versie}* met 2015-07-01.

### <a name="response"></a>Antwoord
Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Een aangepaste beveiligingsrol maken
Maak een aangepaste beveiligingsrol.

Voor het maken van een aangepaste rol die u moet toegang hebben tot `Microsoft.Authorization/roleDefinitions/write` bewerking op alle de `AssignableScopes`. Van de ingebouwde rollen alleen *eigenaar* en *beheerder voor gebruikerstoegang* krijgen toegang tot deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **plaatsen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het eerste *AssignableScope* van de aangepaste rol. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus.

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{rol-definitie-id}* met een nieuwe GUID die de GUID-id van de nieuwe aangepaste rol wordt.
3. Vervang *{api-versie}* met 2015-07-01.

Geef de waarden in de volgende notatie voor de hoofdtekst van de aanvraag:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elementnaam | Vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| naam |Ja |Tekenreeks |GUID-id van de aangepaste rol. |
| properties.roleName |Ja |Tekenreeks |Weergavenaam van de aangepaste rol. Maximale grootte 128 tekens. |
| Properties.Description |Nee |Tekenreeks |Beschrijving van de aangepaste rol. Maximale grootte 1024 tekens. |
| Properties.type |Ja |Tekenreeks |Ingesteld op 'CustomRole'. |
| Properties.permissions.Actions |Ja |String] |Een matrix van tekenreeksen voor actie geven de bewerkingen die zijn verleend door de aangepaste rol. |
| properties.permissions.notActions |Nee |String] |Een matrix van tekenreeksen voor actie geven de bewerkingen moeten worden uitgesloten van de bewerkingen die zijn verleend door de aangepaste rol. |
| properties.assignableScopes |Ja |String] |Een matrix van bereiken waarin de aangepaste rol kan worden gebruikt. |

### <a name="response"></a>Antwoord
Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Bijwerken van een aangepaste rol
Een aangepaste rol wijzigen.

Voor het wijzigen van een aangepaste rol die u moet toegang hebben tot `Microsoft.Authorization/roleDefinitions/write` bewerking op alle de `AssignableScopes`. Van de ingebouwde rollen alleen *eigenaar* en *beheerder voor gebruikerstoegang* krijgen toegang tot deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **plaatsen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het eerste *AssignableScope* van de aangepaste rol. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{rol-definitie-id}* met de GUID-id van de aangepaste rol.
3. Vervang *{api-versie}* met 2015-07-01.

Geef de waarden in de volgende notatie voor de hoofdtekst van de aanvraag:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elementnaam | Vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| naam |Ja |Tekenreeks |GUID-id van de aangepaste rol. |
| properties.roleName |Ja |Tekenreeks |Weergavenaam van de aangepaste rol die is bijgewerkt. |
| Properties.Description |Nee |Tekenreeks |Beschrijving van de aangepaste rol die is bijgewerkt. |
| Properties.type |Ja |Tekenreeks |Ingesteld op 'CustomRole'. |
| Properties.permissions.Actions |Ja |String] |Een matrix van tekenreeksen voor actie geven de bewerkingen waarvoor de bijgewerkte aangepaste rol die toegang verleent. |
| properties.permissions.notActions |Nee |String] |Een matrix van tekenreeksen voor actie geven de bewerkingen moeten worden uitgesloten van de bewerkingen die de bijgewerkte aangepaste rol toekent. |
| properties.assignableScopes |Ja |String] |Een matrix van bereiken waarin de bijgewerkte aangepaste rol kan worden gebruikt. |

### <a name="response"></a>Antwoord
Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen
Een aangepaste rol verwijderen.

Voor het verwijderen van een aangepaste rol die u moet toegang hebben tot `Microsoft.Authorization/roleDefinitions/delete` bewerking op alle de `AssignableScopes`. Van de ingebouwde rollen alleen *eigenaar* en *beheerder voor gebruikerstoegang* krijgen toegang tot deze bewerking. Zie voor meer informatie over roltoewijzingen en het beheer van toegang voor Azure-resources [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

### <a name="request"></a>Aanvraag
Gebruik de **verwijderen** methode met de volgende URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Binnen de URI moet u de volgende vervangingen voor het aanpassen van uw aanvraag:

1. Vervang *{bereik}* met het bereik waarmee u wilt verwijderen van de functiedefinitie. De volgende voorbeelden laten zien hoe het bereik opgeven voor verschillende niveaus:

   * Abonnement: /subscriptions/ {abonnement-id}  
   * Resourcegroep: /subscriptions/ {abonnement-id} / resourceGroups/myresourcegroup1  
   * Bron: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Vervang *{rol-definitie-id}* met de GUID roldefinitie-id van de aangepaste rol.
3. Vervang *{api-versie}* met 2015-07-01.

### <a name="response"></a>Antwoord
Statuscode: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
