---
title: Een Batch-account maken in de Azure Portal | Microsoft Docs
description: Informatie over het maken van een Azure Batch-account in Azure Portal voor het uitvoeren van grootschalige parallelle workloads in de cloud
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 11f8c3f37e56e0b5c566c4abdb60697c5279e72a
ms.lasthandoff: 04/06/2017


---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Een Batch-account maken met behulp van Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

Lees hoe u een Azure Batch-account maakt in de [Azure Portal][azure_portal] en kies de accounteigenschappen die aansluiten op uw berekeningsscenario. Lees waar u belangrijke accounteigenschappen, zoals toegangssleutels en account-URL's, kunt vinden. 

Zie [Overzicht van de functies](batch-api-basics.md) voor achtergrondinformatie over Batch-accounts en scenario's.



## <a name="create-a-batch-account"></a>Batch-account maken

Gebruik de portal om een Batch-account te maken in een van de twee *groepstoewijzingsmodi*: de modus **Batch-service** of de nieuwere modus **Gebruikersabonnement**, waarvoor meer configuratie nodig is. Zie [Overzicht van de functies](batch-api-basics.md#account) voor informatie over deze twee modi. Zie ook het [blogbericht](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/) voor functies van de modus Gebruikersabonnement.

## <a name="batch-service-mode"></a>Modus Batch-service



1. Meld u aan bij [Azure Portal][azure_portal].
2. Klik op **Nieuw** > **Berekenen** > **Batch-service**.
   
    ![Batch in de Marketplace][marketplace_portal]
3. De blade **Nieuw Batch-account** wordt weergegeven. Zie de beschrijvingen hieronder van elk blade-element.
   
    ![Batch-account maken][account_portal]
   
    a. **Accountnaam**: de Batch-accountnaam die u kiest, moet uniek zijn in de Azure-regio waarin het account wordt gemaakt (zie **Locatie** hieronder). De accountnaam mag alleen kleine letters of cijfers bevatten en moet 3 tot 24 tekens lang zijn.
   
    b. **Abonnement**: het abonnement waarin het Batch-account moet worden gemaakt. Als u slechts één abonnement hebt, is het standaard geselecteerd.

    c. **Groepstoewijzingsmodus**: selecteer **Batch-service**.
   
    c. **Resourcegroep**: Selecteer een bestaande resourcegroep voor het nieuwe Batch-account. U kunt er ook voor kiezen om een nieuwe resourcegroep te maken.
   
    d. **Locatie**: de Azure-regio om het Batch-account in te maken. Alleen de regio's die door uw abonnement en resourcegroep worden ondersteund, worden als opties weergegeven.
   
    e. **Opslagaccount** (optioneel): een Azure Storage-account voor algemeen gebruik dat u koppelt aan het Batch-account. Dit wordt aanbevolen voor de meeste Batch-accounts. Zie [Gekoppeld Azure Storage-account](#linked-azure-storage-account) verderop in dit artikel voor meer informatie.

4. Klik op **Maken** om het account te maken.
   
   In de portal wordt aangegeven dat de implementatie wordt uitgevoerd. Na voltooiing wordt de melding **Implementaties geslaagd** weergegeven in **Meldingen**.
   
## <a name="user-subscription-mode"></a>Modus Gebruikersabonnement

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Azure Batch toegang geven tot het abonnement (eenmalige bewerking)
Wanneer u uw eerste Batch-account maakt in de modus Gebruikersabonnement, voert u de volgende stappen uit om uw abonnement te registreren met Batch. (Als u dit eerder hebt gedaan, gaat u naar de volgende sectie.)

1. Meld u aan bij [Azure Portal][azure_portal].

2. Klik op **Meer services** > **abonnementen** en klik op het abonnement dat u wilt gebruiken voor het Batch-account. 

3. Klik in de blade **Abonnement** op **Toegangsbeheer (IAM)** > **Toevoegen**.

    ![Toegangsbeheer voor abonnement][subscription_access]

4. Selecteer op de blade **Machtigingen toevoegen** de rol **Bijdrager** en zoek naar **MicrosoftAzureBatch** (zonder spaties). Selecteer **MicrosoftAzureBatch** en klik op **Opslaan**.

    ![Batch-machtigingen toevoegen][add_permission]

### <a name="create-a-key-vault"></a>Een sleutelkluis maken
In de modus Gebruikersabonnement is een Azure-sleutelkluis vereist die behoort tot dezelfde resourcegroep als het Batch-account dat moet worden gemaakt. Zorg ervoor dat de resourcegroep zich bevindt in een regio waarin Batch [beschikbaar](https://azure.microsoft.com/regions/services/) is en die uw abonnement ondersteunt.

1. Klik in de [Azure Portal][azure_portal] op **Nieuw** > **Beveiliging en identiteit** > **Sleutelkluis**. 

2. Typ in de blade **Sleutelkluis maken** een naam voor de sleutelkluis en maak een resourcegroep in de gewenste regio voor uw Batch-account. Laat de overige instellingen op de standaardwaarden staan en klik op **Maken**.

### <a name="create-a-batch-account"></a>Batch-account maken

1. Klik in de [Azure Portal][azure_portal] op **Nieuw** > **Compute** > **Batch-service**.
   
    ![Batch in de Marketplace][marketplace_portal]
3. De blade **Nieuw Batch-account** wordt weergegeven. Zie de beschrijvingen hieronder van elk blade-element.
   
    ![Batch-account maken][account_portal_byos]
   
    a. **Accountnaam**: de Batch-accountnaam die u kiest, moet uniek zijn in de Azure-regio waarin het account wordt gemaakt (zie **Locatie** hieronder). De accountnaam mag alleen kleine letters of cijfers bevatten en moet 3 tot 24 tekens lang zijn.
   
    b. **Abonnement**: als u meerdere abonnementen hebt, selecteert u het abonnement dat u hebt geregistreerd met de Batch-service.

    c. **Groepstoewijzingsmodus**: selecteer **Gebruikersabonnement**.

    d. **Sleutelkluis**: selecteer de sleutelkluis die u hebt gemaakt voor uw Batch-account in de vorige sectie. Maak desgewenst een nieuwe sleutelkluis. Wanneer u de kluis hebt geselecteerd, schakelt u het selectievakje in voor het verlenen van toegang tot de sleutelkluis aan Azure Batch.
   
    c. **Resourcegroep**: selecteer de resourcegroep waarin u de sleutelkluis hebt gemaakt.
   
    d. **Locatie**: de Azure-regio waarin u de sleutelkluis voor het Batch-account hebt gemaakt. 
   
    e. **Opslagaccount** (optioneel): een Azure Storage-account voor algemeen gebruik dat u koppelt aan het Batch-account. Dit wordt aanbevolen voor de meeste Batch-accounts. Zie [Gekoppeld Azure Storage-account](#linked-azure-storage-account) hieronder voor meer informatie.

4. Klik op **Maken** om het account te maken.
   
   In de portal wordt aangegeven dat de implementatie wordt uitgevoerd. Na voltooiing wordt de melding **Implementaties geslaagd** weergegeven in **Meldingen**.



## <a name="view-batch-account-properties"></a>Eigenschappen van Batch-account weergeven
Zodra het account is gemaakt, kunt u de blade **Batch-account** openen om naar de instellingen en eigenschappen te gaan. U hebt toegang tot alle eigenschappen en accountinstellingen via het linkermenu van de blade Batch-account.

![Blade Batch-account in Azure Portal][account_blade]

* **Batch-account-URL**: wanneer u een toepassing ontwikkelt met de [Batch-API‘s](batch-apis-tools.md#batch-development-apis), hebt u een account-URL nodig voor toegang tot de Batch-resources. Een Batch-account-URL heeft de volgende indeling:
  
    `https://<account_name>.<region>.batch.azure.com`

![Batch-account-URL in de portal][account_url]

* **Toegangssleutels** (modus Batch-service): u hebt een toegangssleutel voor het account nodig om de toegang tot het Batch-account te verifiëren vanuit de toepassing. (Deze instelling is niet beschikbaar in de modus Gebruikersabonnement, waarin u Azure Active Directory-verificatie gebruikt.)

    Als u de toegangssleutels van uw Batch-account wilt bekijken of genereren, voert u `keys` in in het vak **Zoeken** van het linkermenu op de blade Batch-account en selecteert u vervolgens **Sleutels**. 
  
    ![Batch-accountsleutels in Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Gekoppeld Azure Storage-account

U kunt optioneel een Azure Storage-account voor algemeen gebruik koppelen aan uw Batch-account. De functie voor [toepassingspakketten](batch-application-packages.md) van Batch maakt gebruik van Azure Blob-opslag. Dit geldt ook voor de [Batch File Conventions .NET](batch-task-output.md)-bibliotheek. Deze optionele functies helpen u bij het implementeren van de toepassingen die met uw Batch-taken worden uitgevoerd, en het behouden van de geproduceerde gegevens.

U wordt aangeraden een nieuw opslagaccount te maken dat alleen wordt gebruikt voor het Batch-account.

![Een opslagaccount voor 'Algemeen gebruik' maken][storage_account]

> [!NOTE] 
> Azure Batch ondersteunt momenteel alleen het opslagaccounttype voor algemeen gebruik. Dit accounttype wordt beschreven in stap 5 [Een opslagaccount maken] (../ storage/storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](../storage/storage-create-storage-account.md).
>
>

> [!WARNING]
> Wees voorzichtig bij het opnieuw genereren van de toegangssleutels van een gekoppeld opslagaccount. Genereer slechts één opslagaccountsleutel opnieuw en klik op **Sleutels synchroniseren** op de blade voor het gekoppelde opslagaccount. Wacht vijf minuten tot de sleutels aan de rekenknooppunten in uw groepen zijn doorgegeven en genereer en synchroniseer de andere sleutel opnieuw, indien nodig. Als u beide sleutels tegelijk opnieuw genereert, kunnen uw rekenknooppunten geen van beide sleutels synchroniseren en zullen zij toegang verliezen tot het opslagaccount.
> 
> 

![Sleutels van opslagaccounts opnieuw genereren][4]

## <a name="batch-service-quotas-and-limits"></a>Quota en limieten voor Batch-service
Houd er rekening mee dat er bepaalde [quota en limieten](batch-quota-limit.md) van toepassing zijn op Batch-account, net als bij uw Azure-abonnement en andere Azure-services. De huidige quota voor een Batch-account worden in de portal weergegeven bij de **eigenschappen** van het account.

![Batch-accountquota in Azure Portal][quotas]



Daarnaast kunnen veel van deze quota eenvoudig worden verhoogd met een aanvraag voor gratis productondersteuning. Deze aanvraag kunt u indienen in Azure Portal. Zie [Quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor meer informatie over aanvragen voor het verhogen van uw quotum.

## <a name="other-batch-account-management-options"></a>Andere beheeropties voor uw Batch-account
Naast het gebruik van de Azure Portal kunt u ook op de volgende manieren Batch-accounts maken en beheren:

* [Batch-PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Volgende stappen
* Zie [Overzicht van Batch-functies](batch-api-basics.md) voor meer informatie over de concepten en functies van de Batch-service. In het artikel worden de primaire Batch-resources zoals pools, rekenknooppunten, jobs en taken besproken en vindt u een overzicht van de servicefuncties waarmee grootschalige rekenworkloads kunnen worden uitgevoerd.
* Lees de basisbeginselen van het ontwikkelen van een voor Batch geschikte toepassing met behulp van de [clientbibliotheek Batch .NET](batch-dotnet-get-started.md) of [Python](batch-python-tutorial.md). Het inleidende artikel leidt u door een werkende toepassing die gebruikmaakt van de Batch-service voor het uitvoeren van een workload op meerdere rekenknooppunten. U vindt er ook informatie over het gebruik van Azure Storage voor het faseren en ophalen van een workloadbestand.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Sleutels van opslagaccounts opnieuw genereren"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
