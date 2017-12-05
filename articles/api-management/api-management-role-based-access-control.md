---
title: Het gebruik van toegangsbeheer op basis van rollen in Azure API Management | Microsoft Docs
description: Informatie over het gebruik van de ingebouwde rollen en aangepaste rollen maken in Azure API Management
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: c775780a39c4d423c62bf88f55d35675c70442c7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Het gebruik van toegangsbeheer op basis van rollen in Azure API Management
Azure API Management is gebaseerd op gebaseerd toegangsbeheer (RBAC) om in te schakelen Geavanceerd toegangsbeheer voor API Management-services en entiteiten (bijvoorbeeld, API's en beleidsregels). In dit artikel biedt een overzicht van de ingebouwde en aangepaste rollen in API Management. Zie voor meer informatie over toegangsbeheer in Azure portal [aan de slag met toegangsbeheer in Azure portal](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

## <a name="built-in-roles"></a>Ingebouwde rollen
API Management biedt drie ingebouwde rollen momenteel en twee meer functies in de nabije toekomst wordt toegevoegd. Deze rollen kunnen worden toegewezen aan verschillende bereiken, met inbegrip van abonnement, resourcegroep en een afzonderlijk exemplaar van API Management. Bijvoorbeeld, als u de functie 'Azure API Management-Service lezer' aan een gebruiker op het niveau van de resourcegroep toewijst, heeft de gebruiker leestoegang tot alle exemplaren van API Management in de resourcegroep. 

De volgende tabel bevat een korte beschrijving van de ingebouwde rollen. U kunt deze rollen toewijzen met behulp van de Azure-portal of andere hulpprogramma's, waaronder Azure [PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [Azure CLI](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli), en [REST-API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Zie voor meer informatie over het toewijzen van ingebouwde rollen [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

| Rol          | Leestoegang<sup>[1]</sup> | Schrijftoegang<sup>[2]</sup> | Service maken, verwijderen, VPN- en aangepaste domeinconfiguratie schalen | Toegang tot de uitgever van de oude portal | Beschrijving
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Azure API Management-Service Inzender | ✓ | ✓ | ✓ | ✓ | SUPER-gebruiker. Heeft volledige CRUD-toegang tot API Management-services en entiteiten (bijvoorbeeld, API's en beleidsregels). Heeft toegang tot de verouderde publicatieportal. |
| Azure API Management-Service lezer | ✓ | | || Heeft alleen-lezen toegang tot API Management-services en entiteiten. |
| Azure API Management-Service-Operator | ✓ | | ✓ | | API Management-services, maar geen entiteiten beheren.|
| Azure API Management Service-Editor<sup>*</sup> | ✓ | ✓ | |  | API Management-entiteiten, maar niet services beheren.|
| Azure API Management-Inhoudsbeheerder<sup>*</sup> | ✓ | | | ✓ | Kan de portal voor ontwikkelaars beheren. Alleen-lezen toegang tot services en entiteiten.|

<sup>[1] leestoegang tot de API Management-services en entiteiten (bijvoorbeeld, API's en beleidsregels).</sup>

<sup>[2] schrijven toegang tot API Management-services en entiteiten met uitzondering van de volgende bewerkingen: maken, verwijderen en schalen;-exemplaar VPN-configuratie. en instellingen van aangepast domein.</sup>

<sup>\*De functie Service Editor zijn beschikbaar nadat we alle admin gebruikersinterface van de bestaande publicatieportal naar de Azure-portal migreren. De inhoudsbeheerder-functie is beschikbaar nadat de publicatieportal bevindt, is geherstructureerd alleen bevat functionaliteit die betrekking hebben op het beheren van de portal voor ontwikkelaars.</sup>  

## <a name="custom-roles"></a>Aangepaste rollen
Als geen van de ingebouwde rollen aan uw specifieke behoeften, kunnen aangepaste rollen om meer gedetailleerd toegangsbeheer voor API Management-entiteiten worden gemaakt. U kunt bijvoorbeeld een aangepaste rol die alleen-lezen toegang heeft tot een API Management-service, maar alleen schrijftoegang heeft tot één specifieke API maken. Zie voor meer informatie over aangepaste rollen, [aangepaste rollen in Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles). 

Wanneer u een aangepaste rol maakt, is het eenvoudiger om te beginnen met een van de ingebouwde rollen. Bewerk de kenmerken toevoegen **acties**, **NotActions**, of **AssignableScopes**, en vervolgens de wijzigingen opslaan als een nieuwe rol. Het volgende voorbeeld begint met de functie 'Azure API Management-Service lezer' en maakt u een aangepaste rol met de naam ' Rekenmachine API Editor '. U kunt de aangepaste rol toewijzen aan een specifieke API. Deze rol heeft als gevolg daarvan kan alleen toegang tot deze API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

De [Azure Resource Manager resource provider operations](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement) artikel bevat de lijst met machtigingen die kunnen worden verleend op het niveau van API Management.

## <a name="video"></a>Video


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
>
>

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over toegangsbeheer op basis van rollen in Azure, de volgende artikelen:
  * [Aan de slag met toegangsbeheer in Azure Portal](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Aangepaste rollen in Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles)
  * [Azure Resource Manager resource provider-bewerkingen](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement)

