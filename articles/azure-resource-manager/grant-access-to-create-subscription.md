---
title: Toegang verlenen tot de Azure Enterprise-abonnementen maken | Microsoft Docs
description: Leer hoe u een gebruiker of service-principal bieden de mogelijkheid Azure Enterprise-abonnementen via een programma te maken.
services: azure-resource-manager
author: jureid
manager: jureid
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/09/2019
ms.author: jureid
ms.openlocfilehash: 742658e36da956c46bd932b59903e68786c65b93
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794568"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Toegang verlenen tot het maken van Azure Enterprise-abonnementen (preview)

Als klant van Azure op [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), krijgt u een andere gebruiker of service-principal machtiging voor het maken van abonnementen in rekening gebracht op uw account. In dit artikel leert u hoe u [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) voor het delen van de mogelijkheid om te maken van abonnementen en het controleren van bewerkingen voor het abonnement maken. U moet de rol van eigenaar hebben op het account dat u wilt delen.

Zie voor het maken van een abonnement, [Azure Enterprise-abonnementen (preview) via een programma maken](programmatically-create-subscription.md).

## <a name="grant-subscription-creation-access-to-a-user-or-group"></a>Abonnement maken toegang verlenen aan een gebruiker of groep

Voor het maken van abonnementen voor een inschrijvingsaccount voor, moeten gebruikers hebben de [rol RBAC-eigenaar](../role-based-access-control/built-in-roles.md#owner) op dat account. U kunt verlenen van een gebruiker of een groep gebruikers de rol RBAC-eigenaar van een inschrijvingsaccount voor door de volgende stappen:

### <a name="1-get-the-object-id-of-the-enrollment-account-you-want-to-grant-access-to"></a>1. Object-ID van de apparaatregistratie-account dat u wilt toegang verlenen tot ophalen

Als u wilt verlenen aan anderen de rol RBAC-eigenaar voor een apparaatregistratie-account, moet u een eigenaar van het Account of een RBAC-eigenaar van het account zijn.

### <a name="resttabrest"></a>[REST](#tab/rest)

De aanvraag om alle inschrijvingsaccounts die u toegang tot hebt weer te geven:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure reageert met een lijst met alle inschrijvingsaccounts u toegang tot hebt:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

Gebruik de `principalName` eigenschap voor het identificeren van het account dat u wilt RBAC-eigenaar toegang te verlenen. Kopieer de `name` van dat account. Bijvoorbeeld, als u wilt RBAC-eigenaar toegang verlenen tot de SignUpEngineering@contoso.com voor apparaatregistratie-account, die u wilt kopiëren ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Dit is de object-ID van het inschrijvingsaccount. Plak deze waarde ergens zodat u deze in de volgende stap als gebruiken kunt `enrollmentAccountObjectId`.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Open [Azure Cloud Shell](https://shell.azure.com/) en selecteer PowerShell.

Gebruik de [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet om alle inschrijvingsaccounts u toegang tot hebt weer te geven.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure reageert met een lijst met inschrijvingsaccounts u toegang tot hebt:

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```

Gebruik de `principalName` eigenschap aangeven welk account u wilt RBAC-eigenaar toegang te verlenen. Kopieer de `ObjectId` van dat account. Bijvoorbeeld, als u wilt RBAC-eigenaar toegang verlenen tot de SignUpEngineering@contoso.com voor apparaatregistratie-account, die u wilt kopiëren ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Plak deze object-ID ergens zodat u deze in de volgende stap als gebruiken kunt de `enrollmentAccountObjectId`.

### <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Gebruik de [az facturering inschrijvingsaccount lijst](https://aka.ms/EASubCreationPublicPreviewCLI) opdracht om een lijst van alle inschrijvingsaccounts u toegang tot hebt.

```azurecli-interactive 
az billing enrollment-account list
```

Azure reageert met een lijst met inschrijvingsaccounts u toegang tot hebt:

```json
[
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "SignUpEngineering@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  },
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "BillingPlatformTeam@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  }
]

```

Gebruik de `principalName` eigenschap voor het identificeren van het account dat u wilt RBAC-eigenaar toegang te verlenen. Kopieer de `name` van dat account. Bijvoorbeeld, als u wilt RBAC-eigenaar toegang verlenen tot de SignUpEngineering@contoso.com voor apparaatregistratie-account, die u wilt kopiëren ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Dit is de object-ID van het inschrijvingsaccount. Plak deze waarde ergens zodat u deze in de volgende stap als gebruiken kunt `enrollmentAccountObjectId`.

<a id="userObjectId"></a>

### <a name="2-get-object-id-of-the-user-or-group-you-want-to-give-the-rbac-owner-role-to"></a>2. Object-ID van de gebruiker of groep die u wilt toekennen aan de rol RBAC-eigenaar aan ophalen

1. Zoeken in de Azure-portal op **Azure Active Directory**.
1. Als u wilt dat gebruikers toegang geven, klikt u op **gebruikers** in het menu aan de linkerkant. Als u toegang verlenen aan een groep wilt, klikt u op **groepen**.
1. Selecteer de gebruiker of groep die u wilt toekennen aan de rol RBAC-eigenaar op.
1. Als u een gebruiker hebt geselecteerd, vindt u de object-ID op de pagina profileren. Als u een groep hebt geselecteerd, worden de object-ID in de pagina overzicht. Kopieer de **ObjectID** door te klikken op het pictogram aan de rechterkant van het tekstvak in. Plak deze ergens zodat u deze in de volgende stap als gebruiken kunt `userObjectId`.

### <a name="3-grant-the-user-or-group-the-rbac-owner-role-on-the-enrollment-account"></a>3. Verleen de gebruiker of groep van de rol RBAC-eigenaar voor de apparaatregistratie-account

Met behulp van de waarden die u hebt verzameld in de eerste twee stappen, Verleen de gebruiker of groep van de rol RBAC-eigenaar in het inschrijvingsaccount.

### <a name="resttabrest-2"></a>[REST](#tab/rest-2)

Voer de volgende opdracht vervangt ```<enrollmentAccountObjectId>``` met de `name` u in de eerste stap hebt gekopieerd (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Vervang ```<userObjectId>``` met de object-ID die u hebt gekopieerd uit de tweede stap.

```json
PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

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

### <a name="powershelltabazure-powershell-2"></a>[PowerShell](#tab/azure-powershell-2)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voer de volgende [New-AzRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) opdracht, vervangt ```<enrollmentAccountObjectId>``` met de `ObjectId` verzameld in de eerste stap (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Vervang ```<userObjectId>``` met de object-ID in de tweede stap verzameld.

```azurepowershell-interactive
New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
```

### <a name="azure-clitabazure-cli-2"></a>[Azure-CLI](#tab/azure-cli-2)

Voer de volgende [az roltoewijzing maken](../active-directory/role-based-access-control-manage-access-azure-cli.md) opdracht, vervangt ```<enrollmentAccountObjectId>``` met de `name` u in de eerste stap hebt gekopieerd (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Vervang ```<userObjectId>``` met de object-ID in de tweede stap verzameld.

```azurecli-interactive
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
```

----

Wanneer een gebruiker, een RBAC-eigenaar voor uw inschrijvingsaccount wordt, kunnen ze [abonnementen via een programma maken](programmatically-create-subscription.md) daaronder. Een abonnement dat is gemaakt door een gemachtigde gebruiker heeft nog steeds de eigenaar van het oorspronkelijke Account als servicebeheerder, maar heeft ook de gemachtigde gebruiker als de eigenaar van een RBAC standaard.

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Controle die heeft gemaakt met behulp van activiteitenlogboeken abonnementen

Voor het volgen van de abonnementen die zijn gemaakt via deze API, gebruikt u de [Tenant activiteit Log API](/rest/api/monitor/tenantactivitylogs). Het is momenteel niet mogelijk met gebruik van PowerShell, CLI of Azure-portal voor het bijhouden van het abonnement.

1. [Verhoog het toegangsniveau](../active-directory/role-based-access-control-tenant-admin-access.md) als tenantbeheerder van de Azure AD-tenant en wijs vervolgens de rol van Lezer toe aan de controlerende gebruiker voor het bereik `/providers/microsoft.insights/eventtypes/management`.
1. Als de controle gebruiker aanroepen de [Tenant activiteit Log-API](/rest/api/monitor/tenantactivitylogs) abonnement maken van activiteiten. Voorbeeld:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> Probeer [ARMClient](https://github.com/projectkudu/ARMClient) om deze API eenvoudig aan te roepen vanaf de opdrachtregel.

## <a name="next-steps"></a>Volgende stappen

* Nu dat de gebruiker of service-principal gemachtigd om een abonnement te maken is, kunt u dat de identiteit van [programmatisch Azure Enterprise-abonnementen maken](programmatically-create-subscription.md).
* Zie voor een voorbeeld over het maken van abonnementen met .NET, [voorbeeldcode op GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Zie voor meer informatie over Azure Resource Manager en de API's, [overzicht van Azure Resource Manager](resource-group-overview.md).
* Zie voor meer informatie over het beheren van grote aantallen abonnementen die gebruikmaken van beheergroepen, [organiseren van uw resources met Azure-beheergroepen](management-groups-overview.md)
* Zie voor een uitgebreide richtlijnen voor best practices voor grote organisaties op abonnementsgovernance [Azure enterprise-platform - prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance)
