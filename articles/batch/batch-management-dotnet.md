---
title: Batch-account-resources beheren met de clientbibliotheek voor .NET - Azure | Microsoft Docs
description: Maken, verwijderen en wijzigen van resources van Azure Batch-account met de Batch Management .NET-bibliotheek.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eafde9258222a2ab09ade2e366f9cc595a303dec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a>Batch-accounts en quota's met de clientbibliotheek Batch Management beheren voor .NET

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

U kunt verlagen onderhoud overhead in uw Azure Batch-toepassingen met behulp van de [Batch Management .NET] [ api_mgmt_net] bibliotheek voor het automatiseren van Batch-account maken, verwijderen, sleutelbeheer en quota voor detectie.

* **Batch-accounts maken en verwijderen** binnen elke regio. Als een onafhankelijke softwareleverancier (ISV) bijvoorbeeld, u als een service voor uw clients waarin elk een afzonderlijke Batch-account is toegewezen voor facturatie bieden, kunt u de mogelijkheden voor het maken en verwijderen van het account naar de portal voor uw klant kunt toevoegen.
* **Ophalen en opnieuw genereren van sleutels** programmatisch voor uw Batch-accounts. Hierdoor kunt u voldoen aan het beveiligingsbeleid dat periodieke rollover of verstrijken van de sleutels worden afgedwongen. Wanneer u meerdere Batch-accounts in verschillende Azure-regio's hebt, verhoogt de automatisering van dit proces voor de overschakeling van uw oplossing efficiëntie.
* **Controleer de accountquota** en vindt meteen vallen en opstaan buiten te bepalen welke Batch-accounts hebt welke grenzen. Maken van groepen of computerknooppunten toe te voegen, kunt u proactief waar aanpassen door uw account quota's controleren voordat u taken start, of wanneer deze compute resources worden gemaakt. U kunt bepalen welke accounts vereisen quotum verhoogt voordat de aanvullende resources in deze accounts toewijzen.
* **Functies van andere Azure-services combineren** voor een complete beheerervaring--met behulp van Batch Management .NET [Azure Active Directory][aad_about], en de [Azure Resource Manager] [ resman_overview] samen in dezelfde toepassing. U kunt een frictionless verificatie-ervaring, de mogelijkheid om te maken en verwijderen van resourcegroepen en de mogelijkheden die worden hier niet beschreven voor een oplossing voor end-to-end opgeven met behulp van deze functies en hun API's.

> [!NOTE]
> Dit artikel is gericht op het programmatisch beheer van uw Batch-accounts, sleutels en quota's, kunt u veel van deze activiteiten uitvoeren met behulp van de [Azure-portal][azure_portal]. Zie voor meer informatie [een Azure Batch-account maken met de Azure-portal](batch-account-create-portal.md) en [quota en limieten voor de Azure Batch-service](batch-quota-limit.md).
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Batch-accounts maken en verwijderen
Zoals vermeld, is een van de belangrijkste functies van de Batch-API Management maken en verwijderen van de Batch-accounts in een Azure-regio. Gebruiken om dit te doen [BatchManagementClient.Account.CreateAsync] [ net_create] en [DeleteAsync][net_delete], of hun synchrone collega's.

Het volgende codefragment maakt u een account, verkrijgt de nieuwe account van de Batch-service en wordt deze verwijderd. In dit fragment en anderen in dit artikel `batchManagementClient` is een volledig geïnitialiseerd exemplaar van [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> Toepassingen die gebruikmaken van de Batch Management .NET-bibliotheek en de klasse BatchManagementClient vereisen **servicebeheerder** of **CO-beheerder** toegang tot het abonnement dat eigenaar is van het Batch-account om te worden beheerd. Zie voor meer informatie de [Azure Active Directory](#azure-active-directory) sectie en de [AccountManagement] [ acct_mgmt_sample] codevoorbeeld.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Ophalen en opnieuw genereren van sleutels
Primaire en secundaire sleutels verkrijgen van een Batch-account in uw abonnement via [ListKeysAsync][net_list_keys]. U kunt deze sleutels opnieuw genereren met behulp van [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> U kunt een werkstroom gestroomlijnde verbinding maken voor uw toepassingen. Eerst moet u een toegangssleutel voor het Batch-account dat u beheren wilt met [ListKeysAsync][net_list_keys]. Vervolgens gebruikt u deze sleutel bij het initialiseren van de Batch .NET-bibliotheek [BatchSharedKeyCredentials] [ net_sharedkeycred] klasse, die wordt gebruikt bij het initialiseren van [BatchClient][net_batch_client].
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Controleer de Azure-abonnement en quota's Batch-account
Azure-abonnementen en de afzonderlijke Azure-services zoals alle Batch-standaardinstelling quota's hebt die het aantal van bepaalde diensten binnen deze beperken. Zie voor de standaardquota voor Azure-abonnementen, [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md). Zie voor de standaardquota van de Batch-service [quota en limieten voor de Azure Batch-service](batch-quota-limit.md). U kunt deze quota in uw toepassingen controleren met behulp van de Batch Management .NET-bibliotheek. Hiermee kunt u om toewijzing beslissingen te nemen voordat u accounts toevoegt of-bronnen zoals pools COMPUTE en rekenknooppunten.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Een Azure-abonnement voor Batch-account quota's controleren
Voordat u een Batch-account in een regio maakt, kunt u uw Azure-abonnement om te zien of u kunt een account toevoegen in deze regio zijn controleren.

In het onderstaande codefragment gebruiken we eerst [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] ophalen van een verzameling van alle Batch-accounts die binnen een abonnement. Als we deze verzameling hebt verkregen, kunt u het bepalen hoeveel accounts zich in de doelregio. Vervolgens we gebruiken [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] verkrijgen van de Batch-accountquotum en bepalen hoeveel accounts (indien aanwezig) kunnen worden gemaakt in deze regio.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

In het bovenstaande codefragment `creds` is een exemplaar van [TokenCloudCredentials][azure_tokencreds]. Zie voor een voorbeeld van het maken van dit object, de [AccountManagement] [ acct_mgmt_sample] codevoorbeeld op GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Een Batch-account voor compute resource quota's controleren
Voordat u voor de rekenresources in uw Batch-oplossing, kunt u controleren om te controleren of de bronnen die u wilt toewijzen van het account quota won't overschrijden. In het onderstaande codefragment we de quota-informatie voor het Batch-account met de naam afdrukken `mybatchaccount`. In uw eigen toepassing kunt u deze informatie gebruiken om te bepalen of het account kan worden gebruikt voor het verwerken van de aanvullende bronnen worden gemaakt.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> Hoewel er standaardquota voor Azure-abonnementen en services, veel van deze limieten kunnen worden gegenereerd door het uitgeven van een aanvraag in de [Azure-portal][azure_portal]. Zie bijvoorbeeld [quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor instructies over het verhogen van de quota van uw Batch-account.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Azure AD gebruikt met Batch Management .NET

De Batch Management .NET-bibliotheek is een Azure-resource provider-client en wordt gebruikt in combinatie met [Azure Resource Manager] [ resman_overview] account om resources te beheren via een programma. Azure AD is vereist voor de verificatie via een Azure-resource provider-client, met inbegrip van de Batch Management .NET-bibliotheek en aanvragen [Azure Resource Manager][resman_overview]. Zie voor meer informatie over het gebruik van Azure AD met de Batch Management .NET-bibliotheek [gebruik Azure Active Directory voor het verifiëren van Batch-oplossingen](batch-aad-auth.md). 

## <a name="sample-project-on-github"></a>Voorbeeld van project op GitHub

Bekijk Batch Management .NET in actie vindt de [AccountManagment] [ acct_mgmt_sample] voorbeeldproject op GitHub. De voorbeeldtoepassing AccountManagment ziet u de volgende bewerkingen:

1. Verkrijgen van een beveiligingstoken uit Azure AD via [ADAL][aad_adal]. Als de gebruiker niet is al aangemeld, wordt ze gevraagd om hun Azure referenties.
2. Met het beveiligingstoken dat is verkregen van Azure AD, maken een [SubscriptionClient] [ resman_subclient] op Azure-query voor een lijst met abonnementen die zijn gekoppeld aan het account. De gebruiker kan een abonnement uit de lijst selecteren als het bevat meer dan één abonnement.
3. Referenties die zijn gekoppeld aan het geselecteerde abonnement ophalen.
4. Maak een [ResourceManagementClient] [ resman_client] object met de referenties.
5. Gebruik een [ResourceManagementClient] [ resman_client] object om een resourcegroep te maken.
6. Gebruik een [BatchManagementClient] [ net_mgmt_client] object meerdere Batch-account bewerkingen uit te voeren:
   * Maak een Batch-account in de nieuwe resourcegroep.
   * De nieuwe account ophalen van de Batch-service.
   * De account-sleutels voor het nieuwe account afdrukken.
   * Een nieuwe primaire sleutel voor het account opnieuw genereren.
   * De quota-informatie voor het account afdrukken.
   * De quota-informatie voor het abonnement afdrukken.
   * Alle accounts binnen het abonnement afdrukken.
   * Gemaakte account verwijderen.
7. De resourcegroep verwijderen.

Voordat u de nieuwe Batch-account en resource groep verwijdert, u kunt ze weergeven in de [Azure-portal][azure_portal]:

Als u wilt de voorbeeldtoepassing is uitgevoerd, moet u eerst registreren met uw Azure AD-tenant in de Azure portal en machtigingen verlenen voor de Azure Resource Manager-API. Volg de stappen in [oplossingen voor het beheer van Batch verifiëren met Active Directory](batch-aad-auth-management.md).


[aad_about]: ../active-directory/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Verificatie-scenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Toepassingen integreren met Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
