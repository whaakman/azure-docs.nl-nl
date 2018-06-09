---
title: Toegang verlenen tot Azure Enterprise abonnementen maken | Microsoft Docs
description: Ontdek hoe u een gebruiker of service-principal bieden de mogelijkheid programmatisch Azure Enterprise om abonnementen te maken.
services: azure-resource-manager
author: jlian
manager: jlian
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: jlian
ms.openlocfilehash: 4c5d505f431ef684b73adc04629464883d336a5b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238264"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Toegang verlenen tot het maken van abonnementen Azure Enterprise (preview)

Als een Azure-klant op [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), geeft u een andere gebruiker of service principal machtiging voor het maken van abonnementen die bij uw account in rekening gebracht. In dit artikel leert u het gebruik van [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md) voor het delen van de mogelijkheid om abonnementen en het controleren van abonnement aangemaakte te maken.

Zie het maken van een abonnement [programmatisch maken abonnementen Azure Enterprise (preview)](programmatically-create-subscription.md).

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Toegang tot een registratie-account met RBAC delegeren

Naar een andere gebruiker of service-principal bieden de mogelijkheid voor het maken van abonnementen op basis van een specifiek account [Geef ze een eigenaar RBAC-rol op het bereik van de account van de inschrijving](../active-directory/role-based-access-control-manage-access-rest.md). Het volgende voorbeeld krijgt een gebruiker in de tenant met `principalId` van `<userObjectId>` (voor SignUpEngineering@contoso.com) een rol van eigenaar van de registratie-account. Het account van de registratie-ID en de principal-ID vindt [programmatisch maken abonnementen Azure Enterprise (preview)](programmatically-create-subscription.md).

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
Wanneer de rol van eigenaar is toegewezen aan het bereik van de account inschrijving Azure reageert met informatie van de roltoewijzing:

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

Gebruik de [New AzureRmRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) naar een andere gebruiker eigenaar toegang geven tot uw account voor de inschrijving.

```azurepowershell-interactive
New-AzureRmRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Gebruik de [az roltoewijzing maken](../active-directory/role-based-access-control-manage-access-azure-cli.md) naar een andere gebruiker eigenaar toegang geven tot uw account voor de inschrijving.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

----

Zodra een gebruiker een eigenaar RBAC voor uw registratie-account wordt, kunnen ze abonnementen onder deze via een programma maken. Een abonnement dat is gemaakt door een gemachtigde gebruiker heeft nog steeds de eigenaar van het oorspronkelijke Account als beheerder van de Service, maar heeft ook een gebruiker de gedelegeerde als eigenaar van een standaard. 

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Audit die heeft gemaakt met behulp van activiteitenlogboeken abonnementen

Als u wilt bijhouden van de abonnementen die zijn gemaakt via deze API, gebruik de [Tenant activiteit Log-API](/rest/api/monitor/tenantactivitylogs). Het is momenteel niet mogelijk gebruik van PowerShell, CLI of Azure-portal voor het bijhouden van abonnement maken.

1. Als de tenantbeheerder van een van Azure AD-tenant [toegangsrechten uitbreiden](../active-directory/role-based-access-control-tenant-admin-access.md) vervolgens een lezersrol toewijzen aan de gebruiker controle over het bereik `/providers/microsoft.insights/eventtypes/management`.
1. Aanroepen als de controle gebruiker, de [Tenant activiteit Log-API](/rest/api/monitor/tenantactivitylogs) voor een overzicht van activiteiten voor abonnement maken. Voorbeeld:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> [!NOTE]
> Eenvoudig deze API aanroepen vanaf de opdrachtregel, probeer [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Volgende stappen

* Nu dat de gebruiker of service-principal gemachtigd om een abonnement te maken is, kunt u dat de identiteit van [programmatisch Azure Enterprise-mailabonnementen maken](programmatically-create-subscription.md).
* Zie voor een voorbeeld over het maken van abonnementen met .NET [voorbeeldcode op GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Zie voor meer informatie over Azure Resource Manager en de API's, [overzicht van Azure Resource Manager](resource-group-overview.md).
* Zie voor meer informatie over het beheren van grote aantallen abonnementen op basis van beheergroepen, [ordenen van uw resources met Azure-beheergroepen](management-groups-overview.md)
* Zie voor een uitgebreide aanbevolen procedurerichtlijn voor grote organisaties op abonnement governance [Azure enterprise scaffold - prescriptieve abonnement governance](/azure/architecture/cloud-adoption-guide/subscription-governance)
