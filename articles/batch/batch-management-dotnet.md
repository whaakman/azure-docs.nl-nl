---
title: Batch-account-resources beheren met de clientbibliotheek voor .NET - Azure | Microsoft Docs
description: Maken, verwijderen en wijzigen van de resources van de Azure Batch-account met de Batch Management .NET-bibliotheek.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 65f8eb0752a181eda312515e557bb733c091e2e5
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505379"
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a>Beheren van Batch-accounts en -quota met de Batch Management-clientbibliotheek voor .NET

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

U kunt verlagen onderhoud overhead in uw Azure Batch-toepassingen met behulp van de [Batch Management .NET] [ api_mgmt_net] bibliotheek voor het automatiseren van Batch-account maken, verwijderen, sleutelbeheer en detectie van quotum.

* **Batch-accounts maken en verwijderen** binnen elke regio. Als, als een onafhankelijke softwareleverancier (ISV) bijvoorbeeld, u een service voor uw clients waarin elk een afzonderlijke Batch-account voor de facturering is toegewezen, kunt u mogelijkheden voor het maken en verwijderen van het account toevoegen aan de klantportal van uw.
* **Ophalen en accountsleutels opnieuw genereren** via een programma voor het gebruik van uw Batch-accounts. Hiermee kunt u voldoen aan het beveiligingsbeleid dat periodieke rollover of vervallen van accountsleutels worden afgedwongen. Wanneer u meerdere Batch-accounts in verschillende Azure-regio's hebt, verhoogt automatisering van dit proces rollover de efficiëntie van uw oplossing.
* **Controleer de accountquota** en haal het gokwerk vallen en opstaan uit het bepalen van welke Batch-accounts hebben welke limieten. Het maken van pools of computerknooppunten toe te voegen, kunt u proactief waar aanpassen door uw accountquota te controleren voordat u begint met taken, of wanneer deze compute-resources worden gemaakt. U kunt bepalen welke accounts moeten quota verhoogt voordat het toewijzen van extra resources in deze accounts.
* **Functies van andere Azure-services combineren** voor een volledig functionele beheerervaring--met behulp van Batch Management .NET [Azure Active Directory][aad_about], en de [Azure Resource Manager] [ resman_overview] samen in dezelfde toepassing. Met behulp van deze functies en hun API's, kunt u een moeiteloze verificatie-ervaring, de mogelijkheid om te maken en verwijderen van resourcegroepen en de mogelijkheden die zijn hierboven beschreven voor een oplossing voor end-to-end-beheer bieden.

> [!NOTE]
> In dit artikel richt zich op het programmatisch beheer van uw Batch-accounts, sleutels en quota's, kunt u veel van deze activiteiten uitvoeren met behulp van de [Azure-portal][azure_portal]. Zie voor meer informatie, [maken van een Azure Batch-account met behulp van de Azure-portal](batch-account-create-portal.md) en [quota en limieten voor de Azure Batch-service](batch-quota-limit.md).
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Batch-accounts maken en verwijderen
Zoals gezegd, is een van de belangrijkste functies van de Batch-API maken en verwijderen van de Batch-accounts in een Azure-regio. Om dit te doen, gebruik [BatchManagementClient.Account.CreateAsync] [ net_create] en [DeleteAsync][net_delete], of hun synchrone collega's.

Het volgende codefragment wordt een account gemaakt, verkrijgt de zojuist gemaakte account van de Batch-service en wordt deze verwijderd. In dit fragment en de andere in dit artikel `batchManagementClient` is een volledig geïnitialiseerd exemplaar van [BatchManagementClient][net_mgmt_client].

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
> Toepassingen die gebruikmaken van de Batch Management .NET-bibliotheek en de klasse BatchManagementClient vereisen **servicebeheerder** of **CO-beheerder** toegang tot het abonnement dat eigenaar is van de Batch account moeten worden beheerd. Zie voor meer informatie de [Azure Active Directory](#azure-active-directory) sectie en de [AccountManagement] [ acct_mgmt_sample] codevoorbeeld.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Ophalen en accountsleutels opnieuw genereren
Primaire en secundaire sleutels verkrijgen van een Batch-account binnen uw abonnement met behulp van [ListKeysAsync][net_list_keys]. U kunt deze sleutels opnieuw genereren met behulp van [RegenerateKeyAsync][net_regenerate_keys].

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
> U kunt een werkstroom gestroomlijnde verbinding maken voor uw toepassingen. Eerst moet u een accountsleutel voor het Batch-account dat u beheren wilt met [ListKeysAsync][net_list_keys]. Gebruik vervolgens deze sleutel bij het initialiseren van de Batch .NET-clientbibliotheek [BatchSharedKeyCredentials] [ net_sharedkeycred] klasse, die wordt gebruikt bij het initialiseren van [BatchClient] [ net_batch_client].
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure-abonnement en de Batch-accountquota controleren
Azure-abonnementen en de afzonderlijke Azure-services zoals batchverwerking alle hebben standaardquota die het nummer van bepaalde entiteiten binnen deze beperken. Zie voor de standaardquota voor Azure-abonnementen, [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md). Zie voor de standaardquota van de Batch-service, [quota en limieten voor de Azure Batch-service](batch-quota-limit.md). U kunt deze quota in uw toepassingen controleren met behulp van de Batch Management .NET-bibliotheek. Hiermee kunt u om toewijzing beslissingen te nemen voordat u accounts toevoegen of-resources zoals pools COMPUTE en rekenknooppunten.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Een Azure-abonnement voor Batch-accountquota controleren
Voordat u een Batch-account maakt in een regio, kunt u uw Azure-abonnement om te zien of u kunt toevoegen aan een account in deze regio zijn controleren.

In het onderstaande codefragment, gebruiken we eerst [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] aan een verzameling van alle Batch-accounts die zich binnen een abonnement ophalen. Als we deze verzameling hebt verkregen, bepalen we hoeveel accounts zich in de doelregio. Vervolgens we gebruiken [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] te verkrijgen van de Batch-accountquotum te bepalen hoeveel accounts (indien aanwezig) kunnen worden gemaakt in deze regio.

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

In het bovenstaande codefragment `creds` is een exemplaar van [TokenCloudCredentials][azure_tokencreds]. Zie voor een voorbeeld van dit object wordt gemaakt, de [AccountManagement] [ acct_mgmt_sample] voorbeeldcode op GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Een Batch-account voor compute resourcequota controleren
Voordat u voor de compute-resources in uw Batch-oplossing, kunt u controleren om te controleren of de resources die u wilt toewijzen aan de quota van het account wordt niet overschrijdt. In het onderstaande codefragment wordt de Quotuminformatie voor de Batch-account met de naam afdrukken `mybatchaccount`. In uw eigen toepassing, kunt u deze informatie gebruiken om te bepalen of het account kan worden gebruikt voor het afhandelen van de extra resources moet worden gemaakt.

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
> Er zijn standaardquota voor Azure-abonnementen en services, kunnen veel van deze beperkingen worden gegenereerd door het uitgeven van een aanvraag in de [Azure-portal][azure_portal]. Zie bijvoorbeeld [quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor instructies over het vergroten van uw Batch-accountquota.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Gebruik Azure AD met Batch Management .NET

De Batch Management .NET-bibliotheek is een Azure-resource provider-client en wordt gebruikt in combinatie met [Azure Resource Manager] [ resman_overview] accountresources op programmatische wijze beheren. Azure AD is vereist voor het verifiëren van aanvragen via een Azure-resource provider client, met inbegrip van de Batch Management .NET-bibliotheek en [Azure Resource Manager][resman_overview]. Zie voor meer informatie over het gebruik van Azure AD met de Batch Management .NET-bibliotheek [gebruik Azure Active Directory voor het verifiëren van Batch-oplossingen](batch-aad-auth.md). 

## <a name="sample-project-on-github"></a>Voorbeeldproject op GitHub

Als u wilt zien Batch Management .NET in actie, bekijk de [AccountManagment] [ acct_mgmt_sample] voorbeeldproject op GitHub. De voorbeeldtoepassing AccountManagment ziet u de volgende bewerkingen:

1. Verkrijgen van een beveiligingstoken van Azure AD met behulp van [ADAL][aad_adal]. Als de gebruiker is niet al aangemeld, wordt ze gevraagd om hun Azure-referenties.
2. Met het beveiligingstoken dat is verkregen via Azure AD, maakt u een [SubscriptionClient] [ resman_subclient] op Azure-query voor een lijst met abonnementen die zijn gekoppeld aan het account. De gebruiker kan een abonnement selecteren in de lijst bevat meer dan één abonnement.
3. Referenties die zijn gekoppeld aan het geselecteerde abonnement ophalen.
4. Maak een [ResourceManagementClient] [ resman_client] object met behulp van de referenties.
5. Gebruik een [ResourceManagementClient] [ resman_client] object om een resourcegroep te maken.
6. Gebruik een [BatchManagementClient] [ net_mgmt_client] object verschillende bewerkingen van Batch-account uit te voeren:
   * Maak een Batch-account in de nieuwe resourcegroep.
   * Het nieuwe account ophalen uit de Batch-service.
   * Afdrukken de accountsleutels voor het nieuwe account.
   * Een nieuwe primaire sleutel voor het account opnieuw genereren.
   * De Quotuminformatie voor het account afdrukken.
   * De Quotuminformatie voor het abonnement afdrukken.
   * Alle accounts in het abonnement afdrukken.
   * Zojuist gemaakte account verwijderen.
7. Verwijder de resourcegroep.

Voordat u de zojuist gemaakte Batch-account- en groep verwijdert, kunt u bekijken in de [Azure-portal][azure_portal]:

Als u wilt de voorbeeldtoepassing is uitgevoerd, moet u eerst registreren bij uw Azure AD-tenant in Azure portal en machtigingen verlenen voor de Azure Resource Manager-API. Volg de stappen in [verifiëren Batch Management-oplossingen met Active Directory](batch-aad-auth-management.md).


[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]:../active-directory/develop/authentication-scenarios.md "Verificatiescenario's voor Azure AD"
[aad_integrate]:../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md "Toepassingen integreren met Azure Active Directory"
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
