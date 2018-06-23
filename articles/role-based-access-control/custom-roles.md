---
title: Aangepaste rollen in Azure | Microsoft Docs
description: Informatie over het definiëren van aangepaste rollen met Azure op rollen gebaseerde toegangsbeheer (RBAC) voor Geavanceerd toegangsbeheer van bronnen in Azure.
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
ms.date: 06/12/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 074c305cb15bc1fb25dfa5cfc52dcce53b661a7e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321176"
---
# <a name="custom-roles-in-azure"></a>Aangepaste rollen in Azure

Als de [ingebouwde rollen](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. Net als de ingebouwde rollen kunt u aangepaste rollen toewijzen aan gebruikers, groepen en service-principals op abonnement, resourcegroep en resource bereiken. Aangepaste rollen worden opgeslagen in een tenant van Azure Active Directory (Azure AD) en kunnen worden gedeeld door abonnementen. Elke tenant kan maximaal 2000 aangepaste rollen hebben. Aangepaste rollen kunnen worden gemaakt met Azure PowerShell, Azure CLI of de REST-API.

## <a name="custom-role-example"></a>Voorbeeld van de aangepaste rol

Hieronder ziet u een aangepaste rol voor het controleren en opnieuw starten van virtuele machines met Azure PowerShell weergegeven:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
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
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

Nadat u een aangepaste rol maakt, wordt deze weergegeven in de Azure-portal met een oranje resource-pictogram.

![Aangepaste rol-pictogram](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Stappen voor het maken van een aangepaste rol

1. De machtigingen die u moet bepalen

    Wanneer u een aangepaste rol maakt, moet u weten van de resource provider-bewerkingen die beschikbaar zijn voor het definiëren van uw machtigingen zijn. Als u wilt weergeven in de lijst met bewerkingen, kunt u de [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) of [az provider Bewerkingslijst](/cli/azure/provider/operation#az-provider-operation-list) opdrachten.
    Geef de machtigingen voor uw aangepaste rol door u de bewerkingen zijn toevoegen de `actions` of `notActions` eigenschappen van de [roldefinitie](role-definitions.md). Als u gegevensbewerkingen hebt, u deze toevoegen aan de `dataActions` of `notDataActions` eigenschappen.

2. De aangepaste rol maken

    U kunt Azure PowerShell of Azure CLI gebruiken om de aangepaste rol te maken. Normaal gesproken u beginnen met een bestaande ingebouwde rol en vervolgens te wijzigen voor uw behoeften. U gebruikt de [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) of [az roldefinitie maken](/cli/azure/role/definition#az-role-definition-create) opdrachten voor het maken van de aangepaste rol. Voor het maken van een aangepaste beveiligingsrol, hebt u de `Microsoft.Authorization/roleDefinitions/write` machtiging op alle `assignableScopes`, zoals [eigenaar](built-in-roles.md#owner) of [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator).

3. De aangepaste rol testen

    Nadat u uw aangepaste rol hebt, hebt u testen om te controleren of deze werkt zoals verwacht. Als u aanpassingen aan moeten worden gemaakt, kunt u de aangepaste rol bijwerken.

## <a name="custom-role-properties"></a>Eigenschappen van de aangepaste rol

Een aangepaste rol heeft de volgende eigenschappen.

| Eigenschap | Vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| `Name` | Ja | Reeks | De weergavenaam van de aangepaste rol. Moet uniek zijn voor uw tenant. Kan bevatten letters, cijfers, spaties en speciale tekens. Maximum aantal tekens is 128. |
| `Id` | Ja | Reeks | De unieke ID van de aangepaste rol. Voor Azure PowerShell en Azure CLI, worden deze ID wordt automatisch gegenereerd wanneer u een nieuwe rol maakt. |
| `IsCustom` | Ja | Reeks | Hiermee wordt aangegeven of dit een aangepaste beveiligingsrol is. Ingesteld op `true` voor aangepaste functies. |
| `Description` | Ja | Reeks | De beschrijving van de aangepaste rol. Kan bevatten letters, cijfers, spaties en speciale tekens. Maximum aantal tekens is 1024. |
| `Actions` | Ja | String] | Een matrix van tekenreeksen waarmee de beheerbewerkingen die de rol kan worden uitgevoerd. Zie voor meer informatie [acties](role-definitions.md#actions). |
| `NotActions` | Nee | String] | Een matrix van tekenreeksen waarmee de beheerbewerkingen die zijn uitgesloten van de toegestane `actions`. Zie voor meer informatie [notActions](role-definitions.md#notactions). |
| `DataActions` | Nee | String] | Een matrix met tekenreeksen die Hiermee worden de gegevensbewerkingen waarmee de rol moet worden uitgevoerd op uw gegevens binnen dit object. Zie voor meer informatie [dataActions (Preview)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Nee | String] | Een matrix met tekenreeksen die Hiermee worden de gegevensbewerkingen die zijn uitgesloten van de toegestane `dataActions`. Zie voor meer informatie [notDataActions (Preview)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | Ja | String] | Een matrix met tekenreeksen die aangeeft dat de aangepaste rol die beschikbaar voor toewijzing is scopes. Kan niet worden ingesteld op basis-bereik (`"/"`). Zie voor meer informatie [assignableScopes](role-definitions.md#assignablescopes). |

## <a name="assignablescopes-for-custom-roles"></a>assignableScopes voor aangepaste functies

Net als bij ingebouwde rollen de `assignableScopes` eigenschap geeft u de bereiken dat de rol beschikbaar voor toewijzing is. U kunt het bereik van de hoofdmap echter niet gebruiken (`"/"`) in uw eigen aangepaste rollen. Als u probeert, ontvangt u een Autorisatiefout. De `assignableScopes` ook de eigenschap voor een aangepaste beveiligingsrol bepaalt wie kunt maken, verwijderen, wijzigen of de aangepaste rol weergeven.

| Taak | Bewerking | Beschrijving |
| --- | --- | --- |
| Een aangepaste rol maken/verwijderen | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `assignableScopes` van de aangepaste rol kunt maken (of verwijderen) aangepaste rollen voor gebruik in deze bereiken. Bijvoorbeeld: [eigenaars](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste rol wijzigen | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `assignableScopes` van de aangepaste rol aangepaste rollen in deze bereiken kunt wijzigen. Bijvoorbeeld: [eigenaars](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste beveiligingsrol weergeven | `Microsoft.Authorization/ roleDefinition/read` | Gebruikers die deze bewerking op een scope worden verleend, kunnen de aangepaste rollen die beschikbaar voor toewijzing op dat bereik zijn bekijken. Alle ingebouwde rollen toestaan aangepaste rollen beschikbaar voor toewijzing. |

## <a name="next-steps"></a>Volgende stappen
- [Maken van aangepaste rollen met Azure PowerShell](custom-roles-powershell.md)
- [Maken van aangepaste rollen met Azure CLI](custom-roles-cli.md)
- [Roldefinities begrijpen](role-definitions.md)
