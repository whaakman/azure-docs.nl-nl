---
title: Programmatisch Azure Enterprise-mailabonnementen maken | Microsoft Docs
description: Informatie over het extra Azure Enterprise- of Enterprise ontwikkelen en testen om abonnementen te maken via een programma.
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
ms.date: 4/30/2018
ms.author: jlian
ms.openlocfilehash: 308202addbca447ee0dab7a55a1ad2b3b6600a10
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34603318"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Programmatisch maken abonnementen Azure Enterprise (preview)

Als een Azure-klant op [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), u kunt EA (MS-AZR - 0017 P) en abonnementen EA ontwikkelen en testen (MS-AZR - 0148 P) via een programma maken. Als u een andere gebruiker of service-principal de machtiging voor het maken van abonnementen in rekening gebracht bij uw account geven, ze geven [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md) toegang tot je account inschrijving. 

> [!IMPORTANT]
> De Azure-abonnementen gemaakt via deze API is afhankelijk van de overeenkomst waaronder u Microsoft Azure-services van Microsoft of een geautoriseerde reseller hebt verkregen. Zie voor meer informatie, [juridische informatie van Microsoft Azure](https://azure.microsoft.com/support/legal/).

U wordt in dit artikel:

> [!div class="checklist"]
> * Informatie over het maken van abonnementen programmatisch met behulp van Azure Resource Manager
> * Begrijpen hoe gebruikmaken van RBAC voor het delen van de mogelijkheid te maken van abonnementen in rekening gebracht bij uw account EA

Zie ook de [.NET voorbeeldcode op GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).

## <a name="ask-your-ea-enrollment-admin-to-add-you-as-account-owner"></a>Vraag uw beheerder van de inschrijving EA u als eigenaar van Account toevoegen

Om te beginnen, vraag uw beheerder inschrijving [u toevoegen als de eigenaar van een Account in de portal EA](https://ea.azure.com/helpdocs/addNewAccount) (aanmelden vereist). Volg de instructies in de e-mail uitnodiging dat u ontvangt een eerste abonnement handmatig maken.

> [!IMPORTANT]
> U moet bevestigen eigendom van accounts en maak handmatig een initiële EA abonnement voordat u doorgaat met de volgende stap. Alleen het account toe te voegen aan de inschrijving is niet voldoende.

## <a name="find-accounts-you-have-access-to"></a>U toegang tot hebt accounts vinden

Nadat u hebt toegevoegd aan een Azure-EA inschrijving als de eigenaar van een Account, Azure maakt gebruik van de relatie Accountregistratie om te bepalen waar de abonnement-kosten in rekening. Voor het maken van abonnementen eerst weten welke accounts inschrijving u toegang tot hebt. Als u momenteel een EA Account de eigenaar bent en u probeert te gebruiken van deze API, controleert de Azure voor de volgende omstandigheden:

- Uw account is toegevoegd aan een EA-inschrijving
- U hebt een of meer EA of EA Dev/Test-abonnementen, wat betekent dat u hebt doorlopen handmatig registreren ten minste eenmaal
- U bent aangemeld bij de accounteigenaar *basismap*, dit is de map die abonnementen zijn gemaakt in standaard

Als de bovenstaande drie voorwaarden wordt voldaan, een `enrollmentAccount` resource wordt geretourneerd en u kunt beginnen met het maken van abonnementen onder dat account. Alle abonnementen die zijn gemaakt onder het account worden gefactureerd op met de EA-inschrijving die het account is.

# <a name="resttabrest"></a>[REST](#tab/rest)

Aanvraag voor het weergeven van alle accounts voor inschrijving:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure reageert met een lijst met alle inschrijving-accounts die u toegang tot hebt:

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

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Gebruik de [opdracht Get-AzureRmEnrollmentAccount](/powershell/module/azurerm.billing/get-azurermenrollmentaccount) voor een lijst met alle inschrijving accounts die u hebt toegang tot.

```azurepowershell-interactive
Get-AzureRmEnrollmentAccount
```

Azure reageert met een lijst met de Object-id's en e-mailadressen van accounts.

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Gebruik de [az facturering inschrijving account lijst](https://aka.ms/EASubCreationPublicPreviewCLI) opdracht om een lijst van alle inschrijving accounts u toegang tot hebt.

```azurecli-interactive 
az billing enrollment-account list
```
Azure reageert met een lijst met de Object-id's en e-mailadressen van accounts.

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
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

---

Gebruik de `principalName` eigenschap om het account dat u wilt dat de abonnementen aan moet worden gefactureerd te identificeren. Gebruik de `id` als de `enrollmentAccount` waarde waarmee u het abonnement worden gemaakt in de volgende stap.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Abonnementen met een specifieke inschrijving-account maken 

Het volgende voorbeeld wordt een aanvraag voor het maken van abonnement met de naam *Dev Team abonnement* en abonnement aanbieding is *MS-AZR - 0017P* (reguliere EA). Het account van de inschrijving is `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (aanduidingswaarde, dit is een GUID), is het account van de inschrijving voor SignUpEngineering@contoso.com. Deze voegt eventueel ook twee gebruikers als eigenaars van RBAC voor het abonnement.

# <a name="resttabrest"></a>[REST](#tab/rest)

Gebruik de `id` van de `enrollmentAccount` in het pad van de aanvraag-abonnement maken.

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

{
  "displayName": "Dev Team Subscription",
  "offerType": "MS-AZR-0017P",
  "owners": [
    {
      "objectId": "<userObjectId>"
    },
    {
      "objectId": "<servicePrincipalObjectId>"
    }
  ]
}
```

| Elementnaam  | Vereist | Type   | Beschrijving                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Nee      | Reeks | De weergavenaam van het abonnement. Als niet wordt opgegeven, wordt deze ingesteld op de naam van de aanbieding, zoals 'Microsoft Azure Enterprise'.                                 |
| `offerType`   | Ja      | Reeks | De aanbieding van het abonnement. De twee opties voor EA zijn [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (gebruik voor productie) en [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (ontwikkelen en testen, moet [ingeschakeld via de portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Nee       | Reeks | De Object-ID van elke gebruiker die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt.  |

In het antwoord u terughalen een `subscriptionOperation` object voor bewaking. Wanneer het maken van het abonnement is voltooid, de `subscriptionOperation` object zou retourneren een `subscriptionLink` -object met de abonnement-ID.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Voor het gebruik van deze preview-module installeren door te voeren `Install-Module AzureRM.Subscription -AllowPrerelease` eerste. Om ervoor te zorgen `-AllowPrerelease` werkt, installeert u een recente versie van PowerShellGet van [PowerShellGet Get-Module](/powershell/gallery/installing-psget).

Gebruik de [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview) samen met `enrollmentAccount` object-ID als de `EnrollmentAccountObjectId` -parameter voor een nieuw abonnement maken. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Elementnaam  | Vereist | Type   | Beschrijving                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Nee      | Reeks | De weergavenaam van het abonnement. Als niet wordt opgegeven, wordt deze ingesteld op de naam van de aanbieding, zoals 'Microsoft Azure Enterprise'.                                 |
| `OfferType`   | Ja      | Reeks | De aanbieding van het abonnement. De twee opties voor EA zijn [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (gebruik voor productie) en [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (ontwikkelen en testen, moet [ingeschakeld via de portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Ja       | Reeks | De Object-ID van het account van de inschrijving of het abonnement wordt gemaakt onder en gefactureerd op. Dit is een GUID-waarde die u via `Get-AzureRmEnrollmentAccount`. |
| `OwnerObjectId`      | Nee       | Reeks | De Object-ID van elke gebruiker die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt.  |
| `OwnerSignInName`    | Nee       | Reeks | Het e-mailadres van elke gebruiker die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `OwnerObjectId`.|
| `OwnerApplicationId` | Nee       | Reeks | De toepassings-ID van een service-principal die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `OwnerObjectId`.| 

Zie voor een volledige lijst met alle parameters [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Voor het gebruik van deze preview-uitbreiding installeren door te voeren `az extension add --name subscription` eerste.

Gebruik de [az-account maken](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) samen met `enrollmentAccount` object-ID als de `enrollment-account-object-id` -parameter voor een nieuw abonnement maken.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Elementnaam  | Vereist | Type   | Beschrijving                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Nee      | Reeks | De weergavenaam van het abonnement. Als niet wordt opgegeven, wordt deze ingesteld op de naam van de aanbieding, zoals 'Microsoft Azure Enterprise'.                                 |
| `offer-type`   | Ja      | Reeks | De aanbieding van het abonnement. De twee opties voor EA zijn [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (gebruik voor productie) en [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (ontwikkelen en testen, moet [ingeschakeld via de portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Ja       | Reeks | De Object-ID van het account van de inschrijving of het abonnement wordt gemaakt onder en gefactureerd op. Dit is een GUID-waarde die u via `az billing enrollment-account list`. |
| `owner-object-id`      | Nee       | Reeks | De Object-ID van elke gebruiker die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt.  |
| `owner-upn`    | Nee       | Reeks | Het e-mailadres van elke gebruiker die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `owner-object-id`.|
| `owner-spn` | Nee       | Reeks | De toepassings-ID van een service-principal die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `owner-object-id`.| 

Zie voor een volledige lijst met alle parameters [az-account maken](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Toegang tot een registratie-account met RBAC delegeren

Naar een andere gebruiker of service-principal bieden de mogelijkheid voor het maken van abonnementen op basis van een specifiek account [Geef ze een eigenaar RBAC-rol op het bereik van de account van de inschrijving](../active-directory/role-based-access-control-manage-access-rest.md). Het volgende voorbeeld krijgt een gebruiker in de tenant met `principalId` van `<userObjectId>` (voor SignUpEngineering@contoso.com) een rol van eigenaar van de registratie-account. 

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

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Beperkingen van Azure Enterprise-abonnement maken API

- Alleen Azure Enterprise abonnementen kunnen worden gemaakt met behulp van deze API.
- Er is een limiet van 50 abonnementen per account. Daarna worden abonnementen alleen gemaakt met behulp van de Center-Account.
- Er moet ten minste één EA of EA ontwikkelen en testen abonnementen onder de account, wat betekent dat de dat eigenaar van het Account ten minste eenmaal is geworden via handmatig registreren.
- Gebruikers die niet van Account-eigenaars, maar zijn toegevoegd aan een account met inschrijving via RBAC, kunnen abonnementen met Center-Account niet maken.
- U kunt de tenant voor het abonnement worden gemaakt niet selecteren. Het abonnement wordt altijd gemaakt in de oorspronkelijke tenant van de accounteigenaar. Het abonnement om naar te verplaatsen van een andere tenant, Zie [tenant voor abonnement wijzigen](..\active-directory\active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een voorbeeld over het maken van abonnementen met .NET [voorbeeldcode op GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Zie voor meer informatie over Azure Resource Manager en de API's, [overzicht van Azure Resource Manager](resource-group-overview.md).
* Zie voor meer informatie over het beheren van grote aantallen abonnementen op basis van beheergroepen, [ordenen van uw resources met Azure-beheergroepen](management-groups-overview.md)
* Zie voor een uitgebreide aanbevolen procedurerichtlijn voor grote organisaties op abonnement governance [Azure enterprise scaffold - prescriptieve abonnement governance](/azure/architecture/cloud-adoption-guide/subscription-governance)
