---
title: Via een programma maken Azure Enterprise-abonnementen | Microsoft Docs
description: Meer informatie over het maken van aanvullende Azure Enterprise- en Enterprise Dev/Test-abonnementen via een programma.
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
ms.openlocfilehash: 90823eded03f298dd912735fb0170fd8002328f3
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715887"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Via een programma maken Azure Enterprise-abonnementen (preview)

Als klant van Azure op [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), u kunt EA (MS-AZR - 0017 P) en -abonnementen voor EA-ontwikkelen en testen (MS-AZR - 0148 P) via een programma maken. In dit artikel leert u hoe u programmatisch met behulp van Azure Resource Manager-abonnementen maken.

Wanneer u een Azure-abonnement van deze API maakt, wordt dat aan het abonnement valt onder de overeenkomst waaronder u Microsoft Azure-services van Microsoft of een geautoriseerde leverancier hebt verkregen. Zie voor meer informatie, [juridische informatie voor Microsoft Azure](https://azure.microsoft.com/support/legal/).

## <a name="prerequisites"></a>Vereisten

U moet een rol eigenaar of bijdrager hebben op het Inschrijvingsaccount die u wilt maken van abonnementen onder. Er zijn twee manieren om op te halen van deze rollen:

* Kan de beheerder van uw inschrijving [moet u de eigenaar van een Account](https://ea.azure.com/helpdocs/addNewAccount) (aanmelding vereist) waardoor u een eigenaar van het Inschrijvingsaccount. Volg de instructies in de uitnodiging per e-mail ontvangt die u voor het handmatig maken van een eerste abonnement. Eigendom van accounts bevestigen en maak handmatig een initiële EA-abonnement voordat u doorgaat met de volgende stap. Het account bij de inschrijving is alleen toe te voegen niet voldoende.

* Kan de eigenaar van een bestaand van het Inschrijvingsaccount [verlenen u toegang tot](grant-access-to-create-subscription.md). Op dezelfde manier als u gebruiken van een service-principal wilt te maken van de EA-abonnement, moet u [de mogelijkheid om abonnementen te maken voor deze service-principal verlenen](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>U toegang tot hebt accounts zoeken

Nadat u bent toegevoegd aan een Azure EA-inschrijving als de eigenaar van een Account, gebruikt Azure de Accountregistratie relatie om te bepalen waar u de kosten voor het abonnement in rekening. Alle abonnementen die zijn gemaakt onder het account worden in rekening gebracht tegen de EA-inschrijving die het account zich in. Voor het maken van abonnementen, moet u doorgeven van waarden over de voor apparaatregistratie-account en de gebruikersprincipals eigenaar van het abonnement. 

Als u wilt de volgende opdrachten uitvoeren, u moet zijn aangemeld op de accounteigenaar *basismap*, dit is de map die abonnementen zijn gemaakt standaard.

# <a name="resttabrest"></a>[REST](#tab/rest)

De aanvraag om alle inschrijvingsaccounts weer te geven:

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

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Gebruik de [opdracht Get-AzureRmEnrollmentAccount](/powershell/module/azurerm.billing/get-azurermenrollmentaccount) om alle inschrijvingsaccounts weer te geven die u hebt toegang tot.

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

Gebruik de [az facturering inschrijvingsaccount lijst](https://aka.ms/EASubCreationPublicPreviewCLI) opdracht om een lijst van alle inschrijvingsaccounts u toegang tot hebt.

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

Gebruik de `principalName` eigenschap aan het account dat u wilt dat abonnementen in rekening gebracht om te identificeren. Gebruik de `id` als de `enrollmentAccount` waarde die u gebruiken voor het maken van het abonnement in de volgende stap.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Abonnementen onder een bepaalde inschrijvingsaccount maken 

Het volgende voorbeeld wordt een aanvraag voor het maken van abonnement met de naam *Dev Team abonnement* en abonnement *MS-AZR - 0017P* (normale EA). Het inschrijvingsaccount is `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (aanduidingswaarde, deze waarde is een GUID), is het inschrijvingsaccount voor SignUpEngineering@contoso.com. Deze voegt eventueel ook twee gebruikers als RBAC-eigenaren voor het abonnement.

# <a name="resttabrest"></a>[REST](#tab/rest)

Gebruik de `id` van de `enrollmentAccount` in het pad van de aanvraag voor het abonnement te maken.

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

| De naam van element  | Vereist | Type   | Beschrijving                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Nee      | Reeks | De weergavenaam van het abonnement. Indien niet opgegeven, ingesteld op de naam van de aanbieding, zoals "Microsoft Azure Enterprise."                                 |
| `offerType`   | Ja      | Reeks | De aanbieding van het abonnement. De twee opties voor EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (gebruik in productieomgevingen) en [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (ontwikkelen/testen, moet [ingeschakeld met behulp van de EA-portal](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Nee       | Reeks | Object-ID van elke gebruiker die u toevoegen als een RBAC-eigenaar van het abonnement wilt wanneer deze wordt gemaakt.  |

In het antwoord je weer toegang krijgen een `subscriptionOperation` object voor de bewaking. Wanneer het abonnement is gemaakt, de `subscriptionOperation` object retourneerde een `subscriptionLink` -object met de abonnements-ID.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Voor het gebruik van deze preview-module installeren door te voeren `Install-Module AzureRM.Subscription -AllowPrerelease` eerste. Om ervoor te zorgen `-AllowPrerelease` werkt, installeert u een recente versie van PowerShellGet van [PowerShellGet-Module ophalen](/powershell/gallery/installing-psget).

Gebruik de [New-AzureRmSubscription](/powershell/module/azurerm.subscription) samen met `enrollmentAccount` object-ID als de `EnrollmentAccountObjectId` parameter voor een nieuw abonnement maken. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| De naam van element  | Vereist | Type   | Beschrijving                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Nee      | Reeks | De weergavenaam van het abonnement. Indien niet opgegeven, ingesteld op de naam van de aanbieding, zoals "Microsoft Azure Enterprise."                                 |
| `OfferType`   | Ja      | Reeks | De aanbieding van het abonnement. De twee opties voor EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (gebruik in productieomgevingen) en [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (ontwikkelen/testen, moet [ingeschakeld met behulp van de EA-portal](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Ja       | Reeks | Object-ID van de voor apparaatregistratie-account dat het abonnement is gemaakt op basis van en kosten in rekening gebracht aan. Deze waarde is een GUID die u via `Get-AzureRmEnrollmentAccount`. |
| `OwnerObjectId`      | Nee       | Reeks | Object-ID van elke gebruiker die u toevoegen als een RBAC-eigenaar van het abonnement wilt wanneer deze wordt gemaakt.  |
| `OwnerSignInName`    | Nee       | Reeks | Het e-mailadres van elke gebruiker die u toevoegen als een RBAC-eigenaar van het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `OwnerObjectId`.|
| `OwnerApplicationId` | Nee       | Reeks | De toepassings-ID van een service-principal die u toevoegen als een RBAC-eigenaar van het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `OwnerObjectId`. Wanneer u deze parameter, de service-principal moet hebben [leestoegang tot de map](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Zie voor een volledige lijst met alle parameters [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Voor het gebruik van deze preview-extensie installeren door te voeren `az extension add --name subscription` eerste.

Gebruik de [az account maken](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) samen met `enrollmentAccount` object-ID als de `enrollment-account-object-id` parameter voor een nieuw abonnement maken.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| De naam van element  | Vereist | Type   | Beschrijving                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Nee      | Reeks | De weergavenaam van het abonnement. Indien niet opgegeven, ingesteld op de naam van de aanbieding, zoals "Microsoft Azure Enterprise."                                 |
| `offer-type`   | Ja      | Reeks | De aanbieding van het abonnement. De twee opties voor EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (gebruik in productieomgevingen) en [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (ontwikkelen/testen, moet [ingeschakeld met behulp van de EA-portal](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Ja       | Reeks | Object-ID van de voor apparaatregistratie-account dat het abonnement is gemaakt op basis van en kosten in rekening gebracht aan. Deze waarde is een GUID die u via `az billing enrollment-account list`. |
| `owner-object-id`      | Nee       | Reeks | Object-ID van elke gebruiker die u toevoegen als een RBAC-eigenaar van het abonnement wilt wanneer deze wordt gemaakt.  |
| `owner-upn`    | Nee       | Reeks | Het e-mailadres van elke gebruiker die u toevoegen als een RBAC-eigenaar van het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `owner-object-id`.|
| `owner-spn` | Nee       | Reeks | De toepassings-ID van een service-principal die u toevoegen als een RBAC-eigenaar van het abonnement wilt wanneer deze wordt gemaakt. U kunt deze parameter in plaats van `owner-object-id`. Wanneer u deze parameter, de service-principal moet hebben [leestoegang tot de map](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Zie voor een volledige lijst met alle parameters [az account maken](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Beperkingen van het abonnement voor Azure Enterprise API

- Alleen Azure Enterprise-abonnementen kunnen worden gemaakt met behulp van deze API.
- Er is een limiet van 50 abonnementen per account. Hierna kunnen alleen abonnementen worden gemaakt met behulp van Account Center.
- Er moet ten minste één EA of EA Dev/Test-abonnementen onder het account, wat betekent dat de dat eigenaar van het Account is geworden via handmatige Meld u aan ten minste één keer.
- Gebruikers die niet zijn eigenaars, maar zijn toegevoegd aan een inschrijvingsaccount voor via RBAC, geen abonnementen met behulp van de Center-Account kunt maken.
- U kunt de tenant voor het abonnement moet worden gemaakt niet selecteren. Het abonnement wordt altijd gemaakt in de starttenant van de eigenaar van het Account. Zie voor het verplaatsen van het abonnement naar een andere tenant, [abonnement-tenant wijzigen](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een voorbeeld over het maken van abonnementen met .NET, [voorbeeldcode op GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Nu dat u een abonnement hebt gemaakt, kunt u deze mogelijkheid kunt verlenen aan andere gebruikers- en service-principals. Zie voor meer informatie, [toegang verlenen tot de Azure Enterprise-abonnementen (preview) maken](grant-access-to-create-subscription.md).
* Zie voor meer informatie over het beheren van grote aantallen abonnementen die gebruikmaken van beheergroepen, [organiseren van uw resources met Azure-beheergroepen](management-groups-overview.md)
