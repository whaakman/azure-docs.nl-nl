---
title: Beheren van op rollen gebaseerde toegangsbeheer (RBAC) met Azure CLI | Microsoft Docs
description: Informatie over het beheren van op rollen gebaseerde toegangsbeheer (RBAC) met de Azure-opdrachtregelinterface lijst met functies en functie acties en rollen toewijzen aan de scopes abonnement en de toepassing.
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: b99264eb69f115db6e334b6aceae6ed897202d56
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Toegangsbeheer op basis van rollen met de Azure-opdrachtregelinterface beheren
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure-CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


U kunt op rollen gebaseerde toegangsbeheer (RBAC) in de Azure portal en Azure Resource Manager-API gebruiken voor het beheren van toegang tot uw abonnement en bronnen op een heel nauwkeurig. Met deze functie kunt u toegang tot Active Directory-gebruikers, groepen of service-principals verlenen door sommige rollen toewijzen aan deze bij een bepaald bereik.

Voordat u de Azure-opdrachtregelinterface (CLI) gebruiken kunt voor het beheren van RBAC, hebt u de volgende vereisten:

* Azure CLI versie 0.8.8 of hoger. Zie voor het installeren van de meest recente versie en deze koppelen aan uw Azure-abonnement, [installeren en configureren van de Azure CLI](../cli-install-nodejs.md).
* Azure Resource Manager in Azure CLI. Ga naar [met de Azure CLI met Resource Manager](../xplat-cli-azure-resource-manager.md) voor meer informatie.

## <a name="list-roles"></a>Lijst met rollen
### <a name="list-all-available-roles"></a>Lijst van alle beschikbare rollen
U kunt alle beschikbare rollen gebruiken:

        azure role list

Het volgende voorbeeld ziet u de lijst met *alle beschikbare rollen*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Azure RBAC opdrachtregel - lijst van de functie van de azure - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Van Lijstacties van een rol
U kunt de acties van een rol gebruiken:

    azure role show "<role name>"

Het volgende voorbeeld ziet u de acties van de *Inzender* en *Virtual Machine Contributor* rollen.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Azure RBAC opdrachtregel - functie van azure weergeven - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>Lijst met toegang
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Roltoewijzingen lijst effectieve op een resourcegroep
U kunt de roltoewijzingen die aanwezig zijn in een resourcegroep gebruiken:

    azure role assignment list --resource-group <resource group name>

Het volgende voorbeeld ziet u de roltoewijzingen in de *pharma-verkoop-projecforcast* groep.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure RBAC opdrachtregel - toewijzing-lijst per groep functie van azure - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Lijst roltoewijzingen voor een gebruiker
U kunt de roltoewijzingen voor een specifieke gebruiker en de toewijzingen die zijn toegewezen aan een gebruiker groepen gebruiken:

    azure role assignment list --signInName <user email>

U ziet ook roltoewijzingen die zijn overgenomen van groepen doordat de opdracht:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Het volgende voorbeeld ziet u de roltoewijzingen die zijn verleend aan de  *sameert@aaddemo.com*  gebruiker. Dit omvat functies die rechtstreeks aan de gebruiker zijn toegewezen en functies die zijn overgenomen van groepen.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure RBAC opdrachtregel - toewijzingslijst azure rol door gebruiker - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Toegang verlenen
Om toegang te verlenen nadat u hebt aangegeven dat de rol die u wilt toewijzen, gebruiken:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Een rol toewijzen aan de groep in het abonnementsbereik
Als u wilt een rol toewijzen aan een groep in het abonnementsbereik, gebruiken:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Het volgende voorbeeld wordt de *lezer* rol *van Christine Koch Team* op de *abonnement* bereik.

![RBAC Azure vanaf de opdrachtregel - azure roltoewijzing maken door de groep - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Een rol toewijzen aan een toepassing op het abonnementsbereik
Als u wilt een rol toewijzen aan een toepassing op het abonnementsbereik, gebruiken:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Het volgende voorbeeld verleent de *Inzender* role in een *Azure AD* toepassing op het geselecteerde abonnement.

 ![RBAC Azure vanaf de opdrachtregel - azure roltoewijzing maken door toepassing](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Een rol toewijzen aan een gebruiker op het groepsbereik resource
Als u wilt een rol toewijzen aan een gebruiker op het groepsbereik resource, gebruiken:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Het volgende voorbeeld verleent de *Virtual Machine Contributor* rol  *samert@aaddemo.com*  gebruiker op de *Pharma-verkoop-ProjectForcast* resource groepsbereik.

![RBAC Azure vanaf de opdrachtregel - azure roltoewijzing maken door gebruiker - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Een rol toewijzen aan een groep in het bereik van de resource
Als u wilt een rol toewijzen aan een groep voor de resource-scope, gebruiken:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Het volgende voorbeeld verleent de *Virtual Machine Contributor* role in een *Azure AD* groep op een *subnet*.

![RBAC Azure vanaf de opdrachtregel - azure roltoewijzing maken door de groep - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Toegang verwijderen
Als u wilt verwijderen een roltoewijzing, gebruiken:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Het volgende voorbeeld verwijdert u de *Virtual Machine Contributor* functietoewijzing uit de  *sammert@aaddemo.com*  gebruiker op de *Pharma-verkoop-ProjectForcast* resource groep.
Het voorbeeld wordt de roltoewijzing vervolgens verwijderd uit een groep op het abonnement.

![Azure RBAC opdrachtregel - azure-functie toewijzing delete - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Een aangepaste beveiligingsrol maken
Gebruik het volgende voor het maken van een aangepaste rol:

    azure role create --inputfile <file path>

Het volgende voorbeeld wordt een aangepaste beveiligingsrol aangeroepen *virtuele Machine Operator*. Deze aangepaste rol verleent toegang tot alle leesbewerkingen van *Microsoft.Compute*, *Microsoft.Storage*, en *Microsoft.Network* resourceproviders en verleent toegang tot Start, opnieuw opstarten en controleren van de virtuele machines. Deze aangepaste rol kan worden gebruikt in twee abonnementen. Dit voorbeeld wordt een JSON-bestand als invoer.

![JSON - aangepaste roldefinitie - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure vanaf de opdrachtregel - azure-functie maken - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Een aangepaste rol wijzigen
Voor het wijzigen van een aangepaste rol voor het eerst gebruiken de `azure role list` opdracht roldefinitie ophalen. Controleer vervolgens de gewenste wijzigingen aan in het definitiebestand voor de rol. Gebruik tot slot `azure role set` de gewijzigde roldefinitie opslaan.

    azure role set --inputfile <file path>

Het volgende voorbeeld wordt de *Microsoft.Insights/diagnosticSettings/* bewerking is de **acties**, en een Azure-abonnement de **AssignableScopes** van de De aangepaste rol virtuele Machine-Operator.

![JSON - wijzigen aangepaste roldefinitie - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Azure RBAC opdrachtregel - azure-functie set - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen
U verwijdert een aangepaste rol door eerst gebruiken de `azure role list` opdracht om te bepalen de **ID** van de rol. Gebruik vervolgens de `azure role delete` opdracht voor het verwijderen van de rol door op te geven de **ID**.

Het volgende voorbeeld verwijdert u de *virtuele Machine Operator* aangepaste rol.

![Azure RBAC opdrachtregel - azure-functie delete - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Lijst met aangepaste rollen
Als de functies die beschikbaar voor toewijzing op een scope zijn wilt weergeven, gebruikt de `azure role list` opdracht.

De volgende opdracht worden alle functies die beschikbaar voor toewijzing in het geselecteerde abonnement zijn.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Azure RBAC opdrachtregel - lijst van de functie van de azure - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

In het volgende voorbeeld wordt de *virtuele Machine Operator* aangepaste rol is niet beschikbaar in de *Production4* abonnement omdat dat abonnement bevindt zich niet in de **AssignableScopes** van de rol.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Azure RBAC opdrachtregel - lijst met azure-functie voor aangepaste rollen - schermafbeelding](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

