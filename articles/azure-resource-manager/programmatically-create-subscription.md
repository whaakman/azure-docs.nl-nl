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
ms.date: 06/05/2018
ms.author: jlian
ms.openlocfilehash: df66ba1ec2c855a24731387210b0127892f5796f
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234781"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Programmatisch maken abonnementen Azure Enterprise (preview)

Als een Azure-klant op [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), u kunt EA (MS-AZR - 0017 P) en abonnementen EA ontwikkelen en testen (MS-AZR - 0148 P) via een programma maken. In dit artikel leert u het maken van abonnementen programmatisch met behulp van Azure Resource Manager.

Wanneer u een Azure-abonnement van deze API maakt, wordt dat abonnement beheerst door de overeenkomst waaronder u Microsoft Azure-services van Microsoft of een geautoriseerde wederverkoper hebt aangeschaft. Zie voor meer informatie, [juridische informatie van Microsoft Azure](https://azure.microsoft.com/support/legal/).

## <a name="prerequisites"></a>Vereisten

* Uw account moet de eigenaar van een Account in een Azure-EA-inschrijving. Als dit niet het geval is, de systeembeheerder inschrijving [u toevoegen als de eigenaar van een Account met de EA portal](https://ea.azure.com/helpdocs/addNewAccount) (aanmelden vereist). Volg de instructies in de e-mail uitnodiging dat u ontvangt een eerste abonnement handmatig maken. Eigendom van accounts bevestigen en maak handmatig een initiële EA abonnement voordat u doorgaat met de volgende stap. Het account voor de inschrijving is zojuist toe te voegen niet voldoende.

* Als u gebruiken van een service-principal wilt voor het maken van het abonnement EA, moet u [de mogelijkheid om abonnementen te maken voor deze service-principal verlenen](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>U toegang tot hebt accounts vinden

Nadat u hebt toegevoegd aan een Azure-EA inschrijving als de eigenaar van een Account, Azure maakt gebruik van de relatie Accountregistratie om te bepalen waar de abonnement-kosten in rekening. Alle abonnementen die zijn gemaakt onder het account worden gefactureerd op met de EA-inschrijving die het account is. U moet doorgeven voor het maken van abonnementen in waarden over de inschrijving-account en de principals gebruiker eigenaar van het abonnement. 

Als u wilt de volgende opdrachten uitvoeren, moet u zijn aangemeld tot de accounteigenaar *basismap*, dit is de map die abonnementen zijn gemaakt in standaard.

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

Het volgende voorbeeld wordt een aanvraag voor het maken van abonnement met de naam *Dev Team abonnement* en abonnement aanbieding is *MS-AZR - 0017P* (reguliere EA). Het account van de inschrijving is `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (aanduidingswaarde, deze waarde is een GUID zijn), dit is het account van de inschrijving voor SignUpEngineering@contoso.com. Deze voegt eventueel ook twee gebruikers als eigenaars van RBAC voor het abonnement.

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
| `EnrollmentAccountObjectId`      | Ja       | Reeks | De Object-ID van het account van de inschrijving of het abonnement wordt gemaakt onder en gefactureerd op. Deze waarde is een GUID die u via `Get-AzureRmEnrollmentAccount`. |
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
| `enrollment-account-object-id`      | Ja       | Reeks | De Object-ID van het account van de inschrijving of het abonnement wordt gemaakt onder en gefactureerd op. Deze waarde is een GUID die u via `az billing enrollment-account list`. |
| `owner-object-id`      | Nee       | Reeks | De Object-ID van elke gebruiker die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt.  |
| `owner-upn`    | Nee       | Reeks | Het e-mailadres van elke gebruiker die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `owner-object-id`.|
| `owner-spn` | Nee       | Reeks | De toepassings-ID van een service-principal die u toevoegen als een eigenaar RBAC voor het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `owner-object-id`.| 

Zie voor een volledige lijst met alle parameters [az-account maken](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Beperkingen van Azure Enterprise-abonnement maken API

- Alleen Azure Enterprise abonnementen kunnen worden gemaakt met behulp van deze API.
- Er is een limiet van 50 abonnementen per account. Daarna worden abonnementen alleen gemaakt met behulp van de Center-Account.
- Er moet ten minste één EA of EA ontwikkelen en testen abonnementen onder de account, wat betekent dat de dat eigenaar van het Account ten minste eenmaal is geworden via handmatig registreren.
- Gebruikers die niet van Account-eigenaars, maar zijn toegevoegd aan een account met inschrijving via RBAC, kunnen abonnementen met Center-Account niet maken.
- U kunt de tenant voor het abonnement worden gemaakt niet selecteren. Het abonnement wordt altijd gemaakt in de oorspronkelijke tenant van de accounteigenaar. Het abonnement om naar te verplaatsen van een andere tenant, Zie [tenant voor abonnement wijzigen](..\active-directory\active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een voorbeeld over het maken van abonnementen met .NET [voorbeeldcode op GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Nu dat u een abonnement hebt gemaakt, kunt u de mogelijkheid verlenen aan andere gebruikers en de service-principals. Zie voor meer informatie [toegang verlenen tot het maken van abonnementen Azure Enterprise (preview)](grant-access-to-create-subscription.md).
* Zie voor meer informatie over het beheren van grote aantallen abonnementen op basis van beheergroepen, [ordenen van uw resources met Azure-beheergroepen](management-groups-overview.md)
