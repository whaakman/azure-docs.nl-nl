---
title: Aangepaste rollen maken voor Azure RBAC | Microsoft Docs
description: "Informatie over het definiëren van aangepaste rollen met op rollen gebaseerd toegangsbeheer voor nauwkeurigere identiteitsbeheer in uw Azure-abonnement."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/11/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53c8060413f5625273360d9bf23cf27b3f56fb32
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a>Aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer
Maak een aangepaste rol in gebaseerd toegangsbeheer (RBAC) als geen van de ingebouwde rollen aan de behoeften van uw specifieke toegang. Aangepaste rollen kunnen worden gemaakt met [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure-opdrachtregelinterface](role-based-access-control-manage-access-azure-cli.md) (CLI) en de [REST-API](role-based-access-control-manage-access-rest.md). Net als de ingebouwde rollen kunt u aangepaste rollen toewijzen aan gebruikers, groepen en toepassingen bij het abonnement, resourcegroep en resource bereiken. Aangepaste rollen worden opgeslagen in een Azure AD-tenant en kunnen worden gedeeld door abonnementen.

Elke tenant kan maximaal 2000 aangepaste rollen maken. 

Het volgende voorbeeld ziet u een aangepaste rol voor het controleren en opnieuw starten van virtuele machines:

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
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
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Acties
De **acties** eigenschap van een aangepaste rol geeft u de Azure-bewerkingen waarvoor de rol toegang verleent. Er is een verzameling van bewerking tekenreeksen waarmee beveiligbare bewerkingen van de Azure-resourceproviders. Voer de bewerking tekenreeksen de indeling van `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Bewerking tekenreeksen met jokertekens (\*) toegang verlenen tot alle bewerkingen die overeenkomen met de tekenreeks voor de bewerking. Bijvoorbeeld:

* `*/read`verleent toegang tot leesbewerkingen voor alle resourcetypen van alle Azure-resourceproviders.
* `Microsoft.Compute/*`verleent toegang tot alle bewerkingen voor alle resourcetypen in de Microsoft.Compute-resourceprovider.
* `Microsoft.Network/*/read`verleent toegang tot leesbewerkingen voor alle resourcetypen in de Microsoft.Network-resourceprovider van Azure.
* `Microsoft.Compute/virtualMachines/*`verleent toegang tot alle bewerkingen van virtuele machines en het onderliggende resourcetypen.
* `Microsoft.Web/sites/restart/Action`verleent toegang tot websites starten.

Gebruik `Get-AzureRmProviderOperation` (in PowerShell) of `azure provider operations show` (in de Azure CLI) aan de bewerkingen na opvragen van de Azure-resourceproviders. U kunt deze opdrachten om te controleren of een tekenreeks bewerking geldig is en uit te breiden jokertekens bewerking tekenreeksen.

```powershell
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Schermafbeelding van de PowerShell - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```azurecli
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI schermafbeelding - azure-bewerkingen weergeven voor provider "Microsoft.Compute/virtualMachines/ \* /Action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Gebruik de **NotActions** eigenschap als een reeks bewerkingen die u wilt toestaan eenvoudiger wordt gedefinieerd door het met uitzondering van beperkte bewerkingen. De toegang wordt verleend door een aangepaste rol die wordt berekend door af te trekken de **NotActions** bewerkingen van de **acties** bewerkingen.

> [!NOTE]
> Als een gebruiker een rol heeft die worden uitgesloten van een bewerking in toegewezen **NotActions**, en een tweede rol die toegang tot dezelfde bewerking verleent, de gebruiker is toegestaan voor deze bewerking is toegewezen. **NotActions** is niet een weigeren regel – het is gewoon een handige manier voor het maken van een set toegestane bewerkingen wanneer specifieke bewerkingen moeten worden uitgesloten.
>
>

## <a name="assignablescopes"></a>AssignableScopes
De **AssignableScopes** eigenschap van de aangepaste rol geeft u de bereiken (abonnementen, resourcegroepen of resources) waarin de aangepaste rol die beschikbaar voor toewijzing is. U kunt de aangepaste rol die beschikbaar zijn voor toewijzing te maken in alleen de abonnementen of resourcegroepen waarvoor dit en niet door elkaar op de gebruikerservaring op voor de rest van de abonnementen of resourcegroepen.

Voorbeelden van geldige toewijsbare bereiken zijn:

* "/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e', '/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624' - maakt de rol beschikbaar voor toewijzing in twee abonnementen.
* '/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e' - maakt de rol beschikbaar voor toewijzing in één abonnement.
* '/ abonnementen/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/netwerk' - maakt de rol beschikbaar voor toewijzing alleen in de resourcegroep netwerk.

> [!NOTE]
> Moet u ten minste één abonnement, resourcegroep of resource-ID.
>
>

## <a name="custom-roles-access-control"></a>Aangepaste rollen toegangsbeheer
De **AssignableScopes** ook de eigenschap van de aangepaste rol bepaalt wie kunt weergeven, wijzigen en verwijderen van de rol.

* Wie kan een aangepaste beveiligingsrol maken?
    Eigenaars (en beheerders van de gebruiker toegang) van de abonnementen, resourcegroepen en resources kunnen aangepaste rollen maken voor gebruik in deze bereiken.
    De gebruiker de rol maken moet uitvoeren `Microsoft.Authorization/roleDefinition/write` bewerking op alle de **AssignableScopes** van de rol.
* Wie kan een aangepaste rol wijzigen?
    Eigenaars (en beheerders van de gebruiker toegang) van de abonnementen, resourcegroepen en resources aangepaste rollen in deze bereiken kunnen wijzigen. Gebruikers moeten kunnen worden uitgevoerd de `Microsoft.Authorization/roleDefinition/write` bewerking op alle de **AssignableScopes** van een aangepaste beveiligingsrol.
* Wie kan aangepaste rollen bekijken?
    Alle ingebouwde rollen in Azure RBAC kunnen bekijken van functies die beschikbaar voor toewijzing zijn. Gebruikers die kunnen worden uitgevoerd de `Microsoft.Authorization/roleDefinition/read` bewerking met een bereik de RBAC-rollen die beschikbaar voor toewijzing op dat bereik zijn kunt weergeven.

## <a name="see-also"></a>Zie ook
* [Op rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
* Zie voor een lijst van beschikbare bewerkingen [Resourceprovider van Azure Resource Manager operations](role-based-access-control-resource-provider-operations.md).
* Meer informatie over het beheren van toegang met:
  * [PowerShell](role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
  * [REST API](role-based-access-control-manage-access-rest.md)
* [Ingebouwde rollen](role-based-access-built-in-roles.md): meer informatie over de functies die standaard in RBAC.
