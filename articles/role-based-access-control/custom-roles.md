---
title: Aangepaste rollen in Azure | Microsoft Docs
description: Informatie over het definiëren van aangepaste rollen met op rollen gebaseerd toegangsbeheer in Azure (RBAC) voor fijnmazig toegangsbeheer van bronnen in Azure.
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
ms.date: 07/17/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7554ef46289600cd15e4675a91f42a2cd735f18
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112658"
---
# <a name="custom-roles-in-azure"></a>Aangepaste rollen in Azure

Als de [ingebouwde rollen](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. Net als de ingebouwde rollen, kunt u aangepaste rollen toewijzen aan gebruikers, groepen en service-principals op abonnement, resourcegroep en resource-bereiken. Aangepaste rollen worden opgeslagen in een tenant Azure Active Directory (Azure AD) en kunnen worden gedeeld met abonnementen. Elke tenant kan maximaal 2000 aangepaste rollen hebben. Aangepaste rollen kunnen worden gemaakt met behulp van Azure PowerShell, Azure CLI of de REST-API.

## <a name="custom-role-example"></a>Voorbeeld van de aangepaste rol

Hieronder ziet u een aangepaste rol voor het controleren en opnieuw opstarten van virtuele machines weergegeven met behulp van Azure PowerShell:

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

![Pictogram van de aangepaste rol](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Stappen voor het maken van een aangepaste rol

1. De machtigingen die u moet bepalen

    Wanneer u een aangepaste rol maakt, moet u weten de resource provider-bewerkingen die beschikbaar zijn voor het definiëren van uw machtigingen. Als u wilt weergeven in de lijst met bewerkingen, kunt u de [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) of [az provider Bewerkingslijst](/cli/azure/provider/operation#az-provider-operation-list) opdrachten.
    Als u de machtigingen voor uw aangepaste rol, u de bewerkingen zijn toevoegen de `Actions` of `NotActions` eigenschappen van de [roldefinitie](role-definitions.md). Hebt u gegevensbewerkingen, u deze toevoegen aan de `DataActions` of `NotDataActions` eigenschappen.

2. De aangepaste rol maken

    U kunt Azure PowerShell of Azure CLI gebruiken om de aangepaste rol te maken. Normaal gesproken u beginnen met een bestaande ingebouwde rol en wijzig vervolgens het voor uw behoeften. Vervolgens u gebruikt de [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) of [az roldefinitie maken](/cli/azure/role/definition#az-role-definition-create) opdrachten voor het maken van de aangepaste rol. Voor het maken van een aangepaste rol, moet u de `Microsoft.Authorization/roleDefinitions/write` machtiging op alle `AssignableScopes`, zoals [eigenaar](built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator).

3. De aangepaste rol testen

    Nadat u uw aangepaste rol hebt, hebt u testen om te controleren of deze werkt zoals verwacht. Als er wijzigingen moeten worden aangebracht, kunt u de aangepaste rol bijwerken.

## <a name="custom-role-properties"></a>Eigenschappen van de aangepaste rol

Een aangepaste rol heeft de volgende eigenschappen.

| Eigenschap | Vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| `Name` | Ja | Reeks | De weergavenaam van de aangepaste rol. Moet uniek zijn voor uw tenant. Kan bevatten letters, cijfers, spaties en speciale tekens. Maximum aantal tekens is 128. |
| `Id` | Ja | Reeks | De unieke ID van de aangepaste rol. Voor Azure PowerShell en Azure CLI, worden deze ID wordt automatisch gegenereerd bij het maken van een nieuwe rol. |
| `IsCustom` | Ja | Reeks | Geeft aan of dit een aangepaste rol. Ingesteld op `true` voor aangepaste rollen. |
| `Description` | Ja | Reeks | De beschrijving van de aangepaste rol. Kan bevatten letters, cijfers, spaties en speciale tekens. Maximum aantal tekens is 1024. |
| `Actions` | Ja | String] | Een matrix met tekenreeksen die Hiermee geeft u de beheerbewerkingen die de rol kan worden uitgevoerd. Zie voor meer informatie, [acties](role-definitions.md#actions). |
| `NotActions` | Nee | String] | Een matrix met tekenreeksen die Hiermee geeft u de beheerbewerkingen die zijn uitgesloten van het toegestane aantal `Actions`. Zie voor meer informatie, [NotActions](role-definitions.md#notactions). |
| `DataActions` | Nee | String] | Een matrix met tekenreeksen die Hiermee geeft u de bewerkingen die de rol kan worden uitgevoerd met uw gegevens binnen dat object. Zie voor meer informatie, [DataActions (Preview)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Nee | String] | Een matrix met tekenreeksen die Hiermee geeft u de bewerkingen voor gegevens die zijn uitgesloten van het toegestane aantal `DataActions`. Zie voor meer informatie, [NotDataActions (Preview)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | Ja | String] | Een matrix met tekenreeksen die de scopes geeft die de aangepaste rol beschikbaar voor toewijzing is. Kan niet worden ingesteld op basis-bereik (`"/"`). Zie voor meer informatie, [AssignableScopes](role-definitions.md#assignablescopes). |

## <a name="assignablescopes-for-custom-roles"></a>assignableScopes voor aangepaste rollen

Net als ingebouwde rollen, de `AssignableScopes` -eigenschap geeft de scopes die de functie beschikbaar voor toewijzing is. U kunt het bereik van de hoofdmap echter niet gebruiken (`"/"`) in uw eigen aangepaste rollen. Als u probeert, krijgt u een foutmelding autorisatie. De `AssignableScopes` eigenschap voor een aangepaste rol bepaalt ook die kunt maken, verwijderen, wijzigen of weergeven van de aangepaste rol.

| Taak | Bewerking | Beschrijving |
| --- | --- | --- |
| Een aangepaste rol maken/verwijderen | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `AssignableScopes` van de aangepaste rol kunt maken (of verwijderen) aangepaste rollen voor gebruik in deze bereiken. Bijvoorbeeld, [eigenaren](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste rol wijzigen | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `AssignableScopes` van de aangepaste rol die aangepaste rollen in deze bereiken kunt wijzigen. Bijvoorbeeld, [eigenaren](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste rol weergeven | `Microsoft.Authorization/ roleDefinition/read` | Gebruikers die deze bewerking met een bereik worden verleend, kunnen de aangepaste rollen die beschikbaar voor toewijzing op dat bereik zijn bekijken. Alle ingebouwde rollen kunnen aangepaste rollen zijn beschikbaar voor toewijzing. |

## <a name="next-steps"></a>Volgende stappen
- [Maken van aangepaste rollen met behulp van Azure PowerShell](custom-roles-powershell.md)
- [Aangepaste rollen maken met Azure CLI](custom-roles-cli.md)
- [Roldefinities begrijpen](role-definitions.md)
