---
title: Aangepaste rollen voor Azure-resources | Microsoft Docs
description: Informatie over het maken van aangepaste rollen met op rollen gebaseerd toegangsbeheer (RBAC) voor fijnmazig toegangsbeheer van Azure-resources.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6307c57f32700c0c2dd2e5da15b98a2a54dbe9c4
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56339325"
---
# <a name="custom-roles-for-azure-resources"></a>Aangepaste rollen voor Azure-resources

Als de [ingebouwde rollen voor Azure-resources](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. Net als de ingebouwde rollen, kunt u aangepaste rollen toewijzen aan gebruikers, groepen en service-principals op abonnement, resourcegroep en resource-bereiken. Aangepaste rollen worden opgeslagen in een directory van Azure Active Directory (Azure AD) en kunnen worden gedeeld met abonnementen. Elke directory kan maximaal 2000 aangepaste rollen hebben. Aangepaste rollen kunnen worden gemaakt met behulp van Azure PowerShell, Azure CLI of de REST-API.

## <a name="custom-role-example"></a>Voorbeeld van de aangepaste rol

Hieronder ziet u een aangepaste rol eruit zoals weergegeven in de JSON-indeling. Deze aangepaste rol kan worden gebruikt voor het controleren en opnieuw opstarten van virtuele machines.

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "88888888-8888-8888-8888-888888888888",
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
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}",
    "/subscriptions/{subscriptionId3}"
  ]
}
```

Wanneer u een aangepaste rol maakt, wordt deze weergegeven in de Azure-portal met een oranje resource-pictogram.

![Pictogram van de aangepaste rol](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Stappen voor het maken van een aangepaste rol

1. De machtigingen die u moet bepalen

    Wanneer u een aangepaste rol maakt, moet u weten de resource provider-bewerkingen die beschikbaar zijn voor het definiëren van uw machtigingen. Als u wilt weergeven in de lijst met bewerkingen, kunt u de [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) of [az provider Bewerkingslijst](/cli/azure/provider/operation#az-provider-operation-list) opdrachten.
    Als u de machtigingen voor uw aangepaste rol, u de bewerkingen zijn toevoegen de `Actions` of `NotActions` eigenschappen van de [roldefinitie](role-definitions.md). Hebt u gegevensbewerkingen, u deze toevoegen aan de `DataActions` of `NotDataActions` eigenschappen.

2. De aangepaste rol maken

    U kunt Azure PowerShell of Azure CLI gebruiken om de aangepaste rol te maken. Normaal gesproken u beginnen met een bestaande ingebouwde rol en wijzig vervolgens het voor uw behoeften. Vervolgens u gebruikt de [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) of [az roldefinitie maken](/cli/azure/role/definition#az-role-definition-create) opdrachten voor het maken van de aangepaste rol. Voor het maken van een aangepaste rol, moet u de `Microsoft.Authorization/roleDefinitions/write` machtiging op alle `AssignableScopes`, zoals [eigenaar](built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator).

3. De aangepaste rol testen

    Nadat u uw aangepaste rol hebt, hebt u testen om te controleren of deze werkt zoals verwacht. Als u wilt de wijzigingen later aanbrengen, kunt u de aangepaste rol bijwerken.

Zie voor een stapsgewijze zelfstudie voor het maken van een aangepaste rol [zelfstudie: Maken van een aangepaste rol die met behulp van Azure PowerShell](tutorial-custom-role-powershell.md) of [zelfstudie: Maken van een aangepaste rol die met behulp van Azure CLI](tutorial-custom-role-cli.md).

## <a name="custom-role-properties"></a>Eigenschappen van de aangepaste rol

Een aangepaste rol heeft de volgende eigenschappen.

| Eigenschap | Vereist | Type | Description |
| --- | --- | --- | --- |
| `Name` | Ja | String | De weergavenaam van de aangepaste rol. De roldefinitie van een is een resource op abonnementsniveau, kan een roldefinitie kan worden gebruikt in meerdere abonnementen die de dezelfde Azure AD-directory delen. Deze weergavenaam moet uniek zijn in het bereik van de Azure AD-directory. Kan bevatten letters, cijfers, spaties en speciale tekens. Maximum aantal tekens is 128. |
| `Id` | Ja | String | De unieke ID van de aangepaste rol. Voor Azure PowerShell en Azure CLI, worden deze ID wordt automatisch gegenereerd bij het maken van een nieuwe rol. |
| `IsCustom` | Ja | String | Geeft aan of dit een aangepaste rol. Ingesteld op `true` voor aangepaste rollen. |
| `Description` | Ja | String | De beschrijving van de aangepaste rol. Kan bevatten letters, cijfers, spaties en speciale tekens. Maximum aantal tekens is 1024. |
| `Actions` | Ja | String[] | Een matrix met tekenreeksen die Hiermee geeft u de beheerbewerkingen die de rol kan worden uitgevoerd. Zie voor meer informatie, [acties](role-definitions.md#actions). |
| `NotActions` | Nee | String[] | Een matrix met tekenreeksen die Hiermee geeft u de beheerbewerkingen die zijn uitgesloten van het toegestane aantal `Actions`. Zie voor meer informatie, [NotActions](role-definitions.md#notactions). |
| `DataActions` | Nee | String[] | Een matrix met tekenreeksen die Hiermee geeft u de bewerkingen die de rol kan worden uitgevoerd met uw gegevens binnen dat object. Zie voor meer informatie, [DataActions (Preview)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Nee | String[] | Een matrix met tekenreeksen die Hiermee geeft u de bewerkingen voor gegevens die zijn uitgesloten van het toegestane aantal `DataActions`. Zie voor meer informatie, [NotDataActions (Preview)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | Ja | String[] | Een matrix met tekenreeksen die de scopes geeft die de aangepaste rol beschikbaar voor toewijzing is. Op dit moment niet worden ingesteld op het bereik van de hoofdmap (`"/"`) of een beheerbereik van de groep. Zie voor meer informatie, [AssignableScopes](role-definitions.md#assignablescopes) en [organiseren van uw resources met Azure-beheergroepen](../governance/management-groups/index.md#custom-rbac-role-definition-and-assignment). |

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Wie kunt maken, verwijderen, bijwerken of weergeven van een aangepaste rol

Net als ingebouwde rollen, de `AssignableScopes` -eigenschap geeft de scopes die de functie beschikbaar voor toewijzing is. De `AssignableScopes` eigenschap voor een aangepaste rol bepaalt ook die kunt maken, verwijderen, bijwerken of weergeven van de aangepaste rol.

| Taak | Bewerking | Description |
| --- | --- | --- |
| Een aangepaste rol maken/verwijderen | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `AssignableScopes` van de aangepaste rol kunt maken (of verwijderen) aangepaste rollen voor gebruik in deze bereiken. Bijvoorbeeld, [eigenaren](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste rol bijwerken | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `AssignableScopes` van de aangepaste rol die aangepaste rollen in deze bereiken kunt bijwerken. Bijvoorbeeld, [eigenaren](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste rol weergeven | `Microsoft.Authorization/ roleDefinition/read` | Gebruikers die deze bewerking met een bereik worden verleend, kunnen de aangepaste rollen die beschikbaar voor toewijzing op dat bereik zijn bekijken. Alle ingebouwde rollen kunnen aangepaste rollen zijn beschikbaar voor toewijzing. |

## <a name="next-steps"></a>Volgende stappen
- [Aangepaste rollen maken voor Azure-resources met behulp van Azure PowerShell](custom-roles-powershell.md)
- [Aangepaste rollen maken voor Azure-resources met behulp van Azure CLI](custom-roles-cli.md)
- [Informatie over roldefinities voor Azure-resources](role-definitions.md)
- [RBAC voor Azure-resources oplossen](troubleshooting.md)
