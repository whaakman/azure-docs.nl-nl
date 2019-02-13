---
title: Toegang verlenen tot de Azure Enterprise-abonnementen maken | Microsoft Docs
description: Leer hoe u een gebruiker of service-principal bieden de mogelijkheid Azure Enterprise-abonnementen via een programma te maken.
services: azure-resource-manager
author: adpick
manager: adpick
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: adpick
ms.openlocfilehash: 7a2397328f715dbf63246e8d4aaa789b5986b3b4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112560"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Toegang verlenen tot het maken van Azure Enterprise-abonnementen (preview)

Als klant van Azure op [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), krijgt u een andere gebruiker of service-principal machtiging voor het maken van abonnementen in rekening gebracht op uw account. In dit artikel leert u hoe u [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) voor het delen van de mogelijkheid om te maken van abonnementen en het controleren van bewerkingen voor het abonnement maken. U moet de rol van eigenaar hebben op het account dat u wilt delen.

Zie voor het maken van een abonnement, [Azure Enterprise-abonnementen (preview) via een programma maken](programmatically-create-subscription.md).

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Toegang tot een inschrijvingsaccount met RBAC delegeren

Naar een andere gebruiker of service-principal bieden de mogelijkheid om te maken van abonnementen op basis van een specifiek account [zodat ze een rol RBAC-eigenaar in het bereik van het inschrijvingsaccount](../active-directory/role-based-access-control-manage-access-rest.md). Het volgende voorbeeld geeft u een gebruiker in de tenant met `principalId` van `<userObjectId>` (voor SignUpEngineering@contoso.com) een rol van eigenaar van het inschrijvingsaccount. De voor apparaatregistratie-account-ID en de principal-ID, Zie [Azure Enterprise-abonnementen (preview) via een programma maken](programmatically-create-subscription.md).

# <a name="resttabrest"></a>[REST](#tab/rest)

```json
PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>"
  }
}
```

Wanneer de rol van eigenaar is bij de registratie-accountbereik wordt toegewezen, wordt Azure reageert met informatie van de toewijzing van rollen:

```json
{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>",
    "scope": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "createdOn": "2018-03-05T08:36:26.4014813Z",
    "updatedOn": "2018-03-05T08:36:26.4014813Z",
    "createdBy": "<assignerObjectId>",
    "updatedBy": "<assignerObjectId>"
  },
  "id": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "<roleAssignmentGuid>"
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Gebruik de [New-AzRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) een andere gebruiker eigenaar toegang geven tot het inschrijvingsaccount.

```azurepowershell-interactive
New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Gebruik de [az roltoewijzing maken](../active-directory/role-based-access-control-manage-access-azure-cli.md) een andere gebruiker eigenaar toegang geven tot het inschrijvingsaccount.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

----

Wanneer een gebruiker, een RBAC-eigenaar voor uw inschrijvingsaccount wordt, kunnen ze abonnementen onder het via een programma maken. Een abonnement dat is gemaakt door een gemachtigde gebruiker heeft nog steeds de eigenaar van het oorspronkelijke Account als servicebeheerder, maar heeft ook de gemachtigde gebruiker als eigenaar van een standaard. 

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Controle die heeft gemaakt met behulp van activiteitenlogboeken abonnementen

Voor het volgen van de abonnementen die zijn gemaakt via deze API, gebruikt u de [Tenant activiteit Log API](/rest/api/monitor/tenantactivitylogs). Het is momenteel niet mogelijk met gebruik van PowerShell, CLI of Azure-portal voor het bijhouden van het abonnement.

1. [Verhoog het toegangsniveau](../active-directory/role-based-access-control-tenant-admin-access.md) als tenantbeheerder van de Azure AD-tenant en wijs vervolgens de rol van Lezer toe aan de controlerende gebruiker voor het bereik `/providers/microsoft.insights/eventtypes/management`.
1. Als de controle gebruiker aanroepen de [Tenant activiteit Log-API](/rest/api/monitor/tenantactivitylogs) abonnement maken van activiteiten. Voorbeeld:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> [!NOTE]
> Probeer [ARMClient](https://github.com/projectkudu/ARMClient) om deze API eenvoudig aan te roepen vanaf de opdrachtregel.

## <a name="next-steps"></a>Volgende stappen

* Nu dat de gebruiker of service-principal gemachtigd om een abonnement te maken is, kunt u dat de identiteit van [programmatisch Azure Enterprise-abonnementen maken](programmatically-create-subscription.md).
* Zie voor een voorbeeld over het maken van abonnementen met .NET, [voorbeeldcode op GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Zie voor meer informatie over Azure Resource Manager en de API's, [overzicht van Azure Resource Manager](resource-group-overview.md).
* Zie voor meer informatie over het beheren van grote aantallen abonnementen die gebruikmaken van beheergroepen, [organiseren van uw resources met Azure-beheergroepen](management-groups-overview.md)
* Zie voor een uitgebreide richtlijnen voor best practices voor grote organisaties op abonnementsgovernance [Azure enterprise-platform - prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance)
