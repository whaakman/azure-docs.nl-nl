---
title: Een Batch-account maken in Azure Portal | Microsoft Docs
description: Informatie over het maken van een Azure Batch-account in Azure Portal voor het uitvoeren van grootschalige parallelle workloads in de cloud
services: batch
documentationcenter: 
author: v-dotren
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ebda2f11f93b04a5592d18f8e15c8fc3b560aac3
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
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

> [!NOTE]
> Als u een Batch-account maakt, moet u normaliter de standaardmodus **Batch-service** kiezen, waarbij pools achter de schermen van door Azure beheerde abonnementen worden toegewezen. In de alternatieve modus, **Gebruikersabonnement**, die voor de meeste scenario's niet meer wordt aanbevolen, worden Batch-VM's en andere resources rechtstreeks in uw abonnement gemaakt wanneer er een groep wordt gemaakt. Voor het maken van een Batch-account in de gebruikersabonnementmodus moet u het account ook bij Azure Batch registreren en aan een Azure Key Vault koppelen.

1. Meld u aan bij [Azure Portal][azure_portal].
2. Klik op **Nieuw** en zoek op de Marketplace naar **Batch-service**.

    ![Batch in de Marketplace][marketplace_portal]
3. Selecteer **Batch-service**, klik op **Maken** en voer instellingen bij **Nieuw Batch-account** in. Zie de volgende details.

    ![Batch-account maken][account_portal]

    a. **Accountnaam**: de naam die u kiest, moet uniek zijn in de Azure-regio waarin het account wordt gemaakt (zie **Locatie** hieronder). De accountnaam mag alleen kleine letters of cijfers bevatten en moet 3 tot 24 tekens lang zijn.

    b. **Abonnement**: het abonnement waarin het Batch-account moet worden gemaakt. Als u slechts één abonnement hebt, is het standaard geselecteerd.

    c. **Groeptoewijzingsmodus**: als deze instelling wordt weergegeven, accepteert u de standaardinstelling **Batch-service**.

    c. **Resourcegroep**: Selecteer een bestaande resourcegroep voor het nieuwe Batch-account. U kunt er ook voor kiezen om een nieuwe resourcegroep te maken.

    d. **Locatie**: de Azure-regio om het Batch-account in te maken. Alleen de regio's die door uw abonnement en resourcegroep worden ondersteund, worden als opties weergegeven.

    e. **Opslagaccount** (optioneel): een Azure Storage-account voor algemeen gebruik dat u koppelt aan het Batch-account. Dit wordt aanbevolen voor de meeste Batch-accounts. Zie [Gekoppeld Azure Storage-account](#linked-azure-storage-account) verderop in dit artikel voor informatie.

4. Klik op **Maken** om het account te maken.



## <a name="view-batch-account-properties"></a>Eigenschappen van Batch-account weergeven
Zodra het account is gemaakt, klikt u op het account om naar de instellingen en eigenschappen te gaan. U hebt toegang tot alle eigenschappen en accountinstellingen via het linkermenu.

![Pagina Batch-account in Azure Portal][account_blade]

* **Batch-account-URL**: wanneer u een toepassing ontwikkelt met de [Batch-API‘s](batch-apis-tools.md#azure-accounts-for-batch-development), hebt u een account-URL nodig voor toegang tot de Batch-resources. Een Batch-account-URL heeft de volgende indeling:

    `https://<account_name>.<region>.batch.azure.com`

![Batch-account-URL in de portal][account_url]

* **Toegangssleutels**: u kunt een toegangssleutel voor het account gebruiken om de toegang tot het Batch-account te verifiëren vanuit de toepassing. (Batch ondersteunt ook Azure Active Directory-verificatie.)

    Selecteer **Sleutels** als u de toegangssleutels wilt bekijken of opnieuw wilt genereren.

    ![Batch-accountsleutels in Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Gekoppeld Azure Storage-account

U kunt een Azure Storage-account voor algemeen gebruik koppelen aan uw Batch-account. Dit kan in veel scenario's handig zijn. De functie voor [toepassingspakketten](batch-application-packages.md) van Batch maakt gebruik van Azure Blob-opslag. Dit geldt ook voor de [Batch File Conventions .NET](batch-task-output.md)-bibliotheek. Deze optionele functies helpen u bij het implementeren van de toepassingen die met uw Batch-taken worden uitgevoerd, en het behouden van de geproduceerde gegevens.

U wordt aangeraden een nieuw opslagaccount te maken dat alleen wordt gebruikt voor het Batch-account. Azure Batch ondersteunt momenteel alleen het opslagaccounttype voor algemeen gebruik. Dit accounttype wordt beschreven in stap 5, [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](../storage/common/storage-create-storage-account.md).

![Een opslagaccount voor algemeen gebruik maken][storage_account]

> [!NOTE]
> Wees voorzichtig bij het opnieuw genereren van de toegangssleutels van een gekoppeld opslagaccount. Genereer slechts één opslagaccountsleutel opnieuw en klik op **Sleutels synchroniseren** op de pagina voor het gekoppelde opslagaccount. Wacht vijf minuten tot de sleutels aan de rekenknooppunten in uw groepen zijn doorgegeven en genereer en synchroniseer de andere sleutel opnieuw, indien nodig. Als u beide sleutels tegelijk opnieuw genereert, kunnen uw rekenknooppunten geen van beide sleutels synchroniseren en zullen zij toegang verliezen tot het opslagaccount.
>
>

![Sleutels van opslagaccounts opnieuw genereren][4]

## <a name="additional-configuration-for-user-subscription-mode"></a>Aanvullende configuratie voor de modus Gebruikersabonnement

Als u ervoor kiest een Batch-account te maken in de modus Gebruikersabonnement, moet u de volgende aanvullende stappen uitvoeren voordat u het account gaat maken.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Azure Batch toegang geven tot het abonnement (eenmalige bewerking)
Wanneer u uw eerste Batch-account maakt in de modus Gebruikersabonnement, moet u de volgende stappen uitvoeren om uw abonnement te registreren bij Batch. (Als u dit eerder hebt gedaan, gaat u naar de volgende sectie.)

1. Meld u aan bij [Azure Portal][azure_portal].

2. Klik op **Meer services** > **abonnementen** en klik op het abonnement dat u wilt gebruiken voor het Batch-account.

3. Klik op de pagina **Abonnement** op **Toegangsbeheer (IAM)** > **Toevoegen**.

    ![Toegangsbeheer voor abonnement][subscription_access]

4. Selecteer op de pagina **Machtigingen toevoegen** de rol **Bijdrager** en zoek naar de Batch API. Zoek deze tekenreeksen totdat u de API hebt gevonden:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Nieuwere Azure AD-tenants kunnen deze naam gebruiken.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** is de id voor de Batch-API. 

5. Als u de Batch-API hebt gevonden, selecteert u deze en klikt u op **Opslaan**.

    ![Batch-machtigingen toevoegen][add_permission]

### <a name="create-a-key-vault"></a>Een sleutelkluis maken
In de modus Gebruikersabonnement is een Azure-sleutelkluis vereist die behoort tot dezelfde resourcegroep als het Batch-account dat moet worden gemaakt. Zorg ervoor dat de resourcegroep zich bevindt in een regio waarin Batch [beschikbaar](https://azure.microsoft.com/regions/services/) is en die uw abonnement ondersteunt.

1. Klik in de [Azure Portal][azure_portal] op **Nieuw** > **Beveiliging en identiteit** > **Sleutelkluis**.

2. Typ op de pagina **Sleutelkluis maken** een naam voor de sleutelkluis en maak een resourcegroep in de gewenste regio voor uw Batch-account. Laat de overige instellingen op de standaardwaarden staan en klik op **Maken**.




## <a name="batch-service-quotas-and-limits"></a>Quota en limieten voor Batch-service
Er zijn bepaalde [quota en limieten](batch-quota-limit.md) van toepassing op Batch-account, net als bij uw Azure-abonnement en andere Azure-services. De huidige quota voor een Batch-account worden weergegeven in **Quota**.

![Batch-accountquota in Azure Portal][quotas]



Daarnaast kunnen veel van deze quota worden verhoogd met een aanvraag voor gratis productondersteuning. Deze aanvraag kunt u indienen in Azure Portal. Zie [Quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor meer informatie over aanvragen voor het verhogen van uw quotum.

## <a name="other-batch-account-management-options"></a>Andere beheeropties voor uw Batch-account
Naast het gebruik van Azure Portal kunt u op de volgende manieren Batch-accounts maken en beheren:

* [Batch-PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Volgende stappen
* Zie [Overzicht van Batch-functies](batch-api-basics.md) voor meer informatie over de concepten en functies van de Batch-service. In het artikel worden de primaire Batch-resources zoals pools, rekenknooppunten, jobs en taken besproken en vindt u een overzicht van de servicefuncties voor grootschalige rekenworkloads.
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

