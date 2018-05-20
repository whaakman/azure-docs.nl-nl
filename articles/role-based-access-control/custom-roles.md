---
title: Aangepaste rollen maken voor Azure RBAC | Microsoft Docs
description: Informatie over het definiëren van aangepaste rollen met op rollen gebaseerd toegangsbeheer voor nauwkeurigere identiteitsbeheer in uw Azure-abonnement.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/12/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9e2ea46ea1a6b5bd3f50d4d4c15492c16c5241c0
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="create-custom-roles-in-azure"></a>Aangepaste rollen maken in Azure

Als de [ingebouwde rollen](built-in-roles.md) niet voldoen aan de behoeften van uw specifieke toegang, kunt u uw eigen aangepaste rollen maken. Net als de ingebouwde rollen kunt u aangepaste rollen toewijzen aan gebruikers, groepen en service-principals op abonnement, resourcegroep en resource bereiken. Aangepaste rollen worden opgeslagen in een tenant van Azure Active Directory (Azure AD) en kunnen worden gedeeld door abonnementen. Aangepaste rollen kunnen worden gemaakt met Azure PowerShell, Azure CLI of de REST-API. Dit artikel bevat een voorbeeld van hoe om te beginnen met het maken van aangepaste rollen met behulp van PowerShell en Azure CLI.

## <a name="create-a-custom-role-to-open-support-requests-using-powershell"></a>Een aangepaste beveiligingsrol om te openen met behulp van PowerShell ondersteuningsaanvragen maken

Voor het maken van een aangepaste beveiligingsrol, kunt u beginnen met een ingebouwde rol, bewerken en maak vervolgens een nieuwe rol. In dit voorbeeld de ingebouwde [lezer](built-in-roles.md#reader) rol kan worden aangepast voor het maken van een aangepaste beveiligingsrol met de naam 'lezer ondersteuningstickets toegangsniveau'. Dit kan de gebruiker om alles in het abonnement en ook openen ondersteuningsaanvragen weer te geven.

> [!NOTE]
> De slechts twee ingebouwde rollen waarmee een gebruiker ondersteuningsaanvragen openen zijn [eigenaar](built-in-roles.md#owner) en [Inzender](built-in-roles.md#contributor). Voor een gebruiker om te kunnen openen ondersteuningsaanvragen moet hij worden toegewezen een rol bij het abonnementsbereik, omdat alle ondersteuningsaanvragen zijn gemaakt op basis van een Azure-abonnement.

Gebruik in PowerShell de [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) opdracht exporteren van de [lezer](built-in-roles.md#reader) rol in JSON-indeling.

```azurepowershell
Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json
```

Hieronder vindt u de JSON-uitvoer voor de [lezer](built-in-roles.md#reader) rol. Een typische rol bestaat uit drie gedeelten `Actions`, `NotActions`, en `AssignableScopes`. De `Actions` sectie vindt u de toegestane bewerkingen voor de rol. Uitsluiten van bewerkingen van het `Actions`, u ze toevoegt aan `NotActions`. De effectieve machtigingen wordt berekend door af te trekken de `NotActions` bewerkingen van de `Actions` bewerkingen.

```json
{
    "Name":  "Reader",
    "Id":  "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "IsCustom":  false,
    "Description":  "Lets you view everything, but not make any changes.",
    "Actions":  [
                    "*/read"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Vervolgens moet bewerken u de JSON-uitvoer voor het maken van uw aangepaste rol. In dit geval maken ondersteuning servicetickets, de `Microsoft.Support/*` bewerking moet worden toegevoegd. Elke bewerking wordt beschikbaar gesteld van een resourceprovider. Als u een lijst van de bewerkingen voor een resourceprovider, kunt u de [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) opdracht of Raadpleeg [Azure Resource Manager resource provider operations](resource-provider-operations.md).

Het is verplicht dat de rol bevat de expliciete abonnement-id's waar het wordt gebruikt. De abonnement-id's worden vermeld in `AssignableScopes`, anders u niet mag worden importeren van de rol in uw abonnement.

Tot slot stelt u de `IsCustom` eigenschap `true` om op te geven dat dit een aangepaste beveiligingsrol is.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/subscriptions/11111111-1111-1111-1111-111111111111"
                         ]
}
```

Voor het maken van de nieuwe aangepaste rol die u gebruikt de [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) opdracht en geeft u het bijgewerkte definitiebestand van de JSON-rol.

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\rbacrole2.json"
```

Nadat u [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition), de nieuwe aangepaste rol is beschikbaar in de Azure-portal en kunnen worden toegewezen aan gebruikers.

![Schermafbeelding van de aangepaste rol geïmporteerd in de Azure portal](./media/custom-roles/18.png)

![schermopname van het aangepaste geïmporteerde rol toewijzen aan gebruiker in dezelfde map](./media/custom-roles/19.png)

![schermopname van machtigingen voor aangepaste geïmporteerde beveiligingsrol](./media/custom-roles/20.png)

Gebruikers met deze aangepaste rol kunnen nieuwe ondersteuningsaanvragen maken.

![Schermafbeelding van de aangepaste rol ondersteuningsaanvragen maken](./media/custom-roles/21.png)

Gebruikers met deze aangepaste rol kunnen andere acties worden uitgevoerd, zoals virtuele machines maken of maken van resourcegroepen.

![Schermafbeelding van de aangepaste rol kan niet worden gemaakt van virtuele machines](./media/custom-roles/22.png)

![Schermafbeelding van de aangepaste rol kan niet worden gemaakt van nieuwe RGs](./media/custom-roles/23.png)

## <a name="create-a-custom-role-to-open-support-requests-using-azure-cli"></a>Een aangepaste beveiligingsrol om te openen met behulp van Azure CLI ondersteuningsaanvragen maken

De stappen voor het maken van een aangepaste beveiligingsrol met Azure CLI zijn vergelijkbaar met behulp van PowerShell, behalve dat de JSON-uitvoer verschilt.

Bijvoorbeeld, u kunt beginnen met de ingebouwde [lezer](built-in-roles.md#reader) rol. Voor een lijst met de acties van de [lezer](built-in-roles.md#reader) rol, gebruik de [az rol definitielijst](/cli/azure/role/definition#az_role_definition_list) opdracht.

```azurecli
az role definition list --name "Reader" --output json
```

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "notActions": [],
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Maak een JSON-bestand met de volgende indeling. De `Microsoft.Support/*` bewerking is toegevoegd aan de `Actions` secties zodat deze gebruiker ondersteuningsaanvragen terwijl u moet een lezer kunt openen. U moet de abonnements-ID waarop u deze rol wordt gebruikt in toevoegen de `AssignableScopes` sectie.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes": [
                            "/subscriptions/11111111-1111-1111-1111-111111111111"
                        ]
}
```

Gebruik voor het maken van de nieuwe aangepaste rol die de [az roldefinitie maken](/cli/azure/role/definition#az_role_definition_create) opdracht.

```azurecli
az role definition create --role-definition ~/roles/rbacrole1.json
```

De nieuwe aangepaste rol is nu beschikbaar in de Azure-portal en het proces voor het gebruik van deze rol is hetzelfde als in de vorige sectie met PowerShell.

![Azure portal schermafbeelding van de aangepaste rol die zijn gemaakt met behulp van de CLI 1.0](./media/custom-roles/26.png)


## <a name="see-also"></a>Zie ook
- [Roldefinities begrijpen](role-definitions.md)
- [Toegangsbeheer op basis van rollen met AzurePowerShell beheren](role-assignments-powershell.md)
- [Op rollen gebaseerde toegangsbeheer met Azure CLI beheren](role-assignments-cli.md)
- [Op rollen gebaseerde toegangsbeheer met de REST-API beheren](role-assignments-rest.md)
