---
title: Het gebruik van toegangsbeheer op basis van rollen in Azure API Management | Microsoft Docs
description: Meer informatie over het gebruik van de ingebouwde rollen en aangepaste rollen maken in Azure API Management
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 21e7406d566be5be73342d530a4ad66b80a1d23f
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990438"
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Het gebruik van toegangsbeheer op basis van rollen in Azure API Management
Met Azure API Management maakt gebruik van op rollen gebaseerd toegangsbeheer (RBAC) om in te schakelen over Geavanceerd toegangsbeheer voor API Management-services en entiteiten (bijvoorbeeld API's en beleid). In dit artikel biedt een overzicht van de ingebouwde en aangepaste rollen in API Management. Zie voor meer informatie over toegangsbeheer in Azure portal, [aan de slag met toegangsbeheer in Azure portal](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="built-in-roles"></a>Ingebouwde rollen
API Management is momenteel heeft drie ingebouwde rollen en twee meer functies in de nabije toekomst worden toegevoegd. Deze rollen kunnen worden toegewezen aan verschillende bereiken, met inbegrip van abonnement, resourcegroep en afzonderlijke API Management-exemplaar. Bijvoorbeeld, als u de rol van 'Azure API Management-Service lezer' aan een gebruiker op het niveau van de resourcegroep toewijzen, heeft de gebruiker leestoegang tot alle exemplaren van API Management in de resourcegroep. 

De volgende tabel bevat korte beschrijvingen van de ingebouwde rollen. U kunt deze rollen toewijzen met behulp van de Azure-portal of andere hulpprogramma's, waaronder Azure [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli), en [REST-API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest). Zie voor meer informatie over het toewijzen van ingebouwde rollen [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

| Rol          | Leestoegang<sup>[1]</sup> | Toegang voor schrijven<sup>[2]</sup> | Service maken, verwijderen, schaal, VPN- en aangepaste domeinconfiguratie | Toegang tot de verouderde publisher-portal | Description
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Inzender voor Azure API Management-Services | ✓ | ✓ | ✓ | ✓ | Supergebruiker. Heeft volledige toegang tot de CRUD API Management-services en entiteiten (bijvoorbeeld API's en beleidsregels). Heeft toegang tot de verouderde publicatieportal. |
| Azure API Management-Service-lezer | ✓ | | || Heeft alleen-lezen toegang tot de API Management-services en entiteiten. |
| Azure API Management-Service-Operator | ✓ | | ✓ | | API Management-services, maar geen entiteiten beheren.|
| Azure API Management Service-Editor<sup>*</sup> | ✓ | ✓ | |  | API Management-entiteiten, maar geen services beheren.|
| Azure API Management-Inhoudsbeheerder<sup>*</sup> | ✓ | | | ✓ | De developer-portal kunt u beheren. Alleen-lezen toegang tot services en entiteiten.|

<sup>[1] leestoegang tot de API Management-services en entiteiten (bijvoorbeeld API's en beleid).</sup>

<sup>[2] schrijftoegang tot de API Management-services en entiteiten met uitzondering van de volgende bewerkingen: exemplaar te maken, verwijderen en schaling; VPN-configuratie. en aangepast domein instellen.</sup>

<sup>\* De rol Service-Editor zijn beschikbaar nadat we alle admin gebruikersinterface van de bestaande publicatieportal naar Azure portal migreren. De inhoudsbeheerder-functie is beschikbaar nadat de publicatieportal is geherstructureerd alleen bevat functionaliteit die betrekking hebben op het beheren van de portal voor ontwikkelaars.</sup>  

## <a name="custom-roles"></a>Aangepaste rollen
Als geen van de ingebouwde rollen aan uw specifieke behoeften voldoen, kunnen aangepaste rollen worden gemaakt voor meer gedetailleerd toegangsbeheer voor API Management-entiteiten. U kunt bijvoorbeeld een aangepaste rol die is alleen-lezen toegang tot een API Management-service, maar heeft alleen toegang voor schrijven naar een specifieke API maken. Zie voor meer informatie over aangepaste rollen, [aangepaste rollen in Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). 

> [!NOTE]
> Als u om te zien van een exemplaar van API Management in Azure portal, een aangepaste rol moet bevatten de ```Microsoft.ApiManagement/service/read``` actie.

Wanneer u een aangepaste rol maakt, is het gemakkelijker om te beginnen met een van de ingebouwde rollen. Bewerk de kenmerken toe te voegen **acties**, **NotActions**, of **AssignableScopes**, en vervolgens de wijzigingen opslaan als een nieuwe rol. Het volgende voorbeeld begint met de rol van 'Azure API Management-Service lezer' en maakt u een aangepaste rol met de naam "Rekenmachine API-Editor." U kunt de aangepaste rol toewijzen aan een bepaalde API. Deze rol heeft als gevolg daarvan kan alleen toegang tot deze API. 

```
$role = Get-AzRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.Actions.Add('Microsoft.ApiManagement/service/apis/*/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzRoleDefinition -Role $role
New-AzRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

De [Azure Resource Manager-resourceproviderbewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftapimanagement) artikel bevat de lijst met machtigingen die kunnen worden verleend op het niveau van de API Management.

## <a name="video"></a>Video


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
>
>

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over toegangsbeheer op basis van rollen in Azure:
  * [Aan de slag met toegangsbeheer in Azure Portal](../role-based-access-control/overview.md)
  * [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md)
  * [Aangepaste rollen in Azure RBAC](../role-based-access-control/custom-roles.md)
  * [Azure Resource Manager-resourceproviderbewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftapimanagement)

