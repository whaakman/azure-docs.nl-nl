---
title: Een Batch-account maken met behulp van Azure Portal | Microsoft Docs
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
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6b6c548ca1001587e2b40bbe9ee2fcb298f40d72
ms.openlocfilehash: bdd7f1410906a46c66c1fd33949b30cc669e5a6b
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Een Batch-account maken met behulp van Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

Leer hoe u een Azure Batch-account maakt in [Azure Portal][azure_portal] en waar u belangrijke accounteigenschappen kunt vinden, zoals toegangssleutels en account-URL's. We bespreken ook Batch-prijzen en het koppelen van een Azure Storage-account aan uw Batch-account zodat u [toepassingspakketten](batch-application-packages.md) kunt gebruiken en [taken kunt uitvoeren](batch-task-output.md).

## <a name="create-a-batch-account"></a>Batch-account maken
1. Meld u aan bij [Azure Portal][azure_portal].
2. Klik op **Nieuw** > **Berekenen** > **Batch-service**.
   
    ![Batch in de Marketplace][marketplace_portal]
3. De blade **Nieuw Batch-account** wordt weergegeven. Bij de items *a* tot *e* hieronder vindt u voor elk blade-element een beschrijving.
   
    ![Batch-account maken][account_portal]
   
    a. **Accountnaam**: de naam voor het Batch-account. De naam die u kiest, moet uniek zijn in de Azure-regio waar het nieuwe account wordt gemaakt. (Zie **Locatie** hieronder.) De accountnaam mag alleen kleine letters of cijfers bevatten en moet 3 tot 24 tekens lang zijn.
   
    b. **Abonnement**: het abonnement waarin het Batch-account moet worden gemaakt. Als u slechts één abonnement hebt, is het standaard geselecteerd.
   
    c. **Resourcegroep**: Selecteer een bestaande resourcegroep voor het nieuwe Batch-account. U kunt er ook voor kiezen om een nieuwe resourcegroep te maken.
   
    d. **Locatie**: de Azure-regio om het Batch-account in te maken. Alleen de regio's die door uw abonnement en resourcegroep worden ondersteund, worden als opties weergegeven.
   
    e. **Opslagaccount** (optioneel): een Azure-opslagaccount voor algemeen gebruik dat u koppelt aan het nieuwe Batch-account. Zie [Gekoppeld Azure Storage-account](#linked-azure-storage-account) hieronder voor meer informatie.

4. Klik op **Maken** om het account te maken.
   
   In de portal wordt aangegeven dat het account wordt **geïmplementeerd**. Bij voltooiing wordt de melding **Implementaties voltooid** weergegeven in *Meldingen*.

## <a name="view-batch-account-properties"></a>Eigenschappen van Batch-account weergeven
Zodra het account is gemaakt, kunt u de blade **Batch-account** openen om naar de instellingen en eigenschappen te gaan. U hebt toegang tot alle eigenschappen en accountinstellingen via het linkermenu van de blade Batch-account.

![Blade Batch-account in Azure Portal][account_blade]

* **Batch-account-URL**: wanneer u een toepassing ontwikkelt met de [Batch-API‘s](batch-technical-overview.md#batch-development-apis), hebt u een account-URL nodig voor toegang tot de Batch-resources. Een Batch-account-URL heeft de volgende indeling:
  
    `https://<account_name>.<region>.batch.azure.com`

![Batch-account-URL in de portal][account_url]

* **Toegangssleutels**: u hebt een toegangssleutel voor het account nodig om de toegang tot het Batch-account te verifiëren vanuit de toepassing. Als u de toegangssleutels van uw Batch-account wilt bekijken of genereren, voert u `keys` in in het vak **Zoeken** van het linkermenu op de blade Batch-account en selecteert u vervolgens **Sleutels**.
  
    ![Batch-accountsleutels in Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Gekoppeld Azure Storage-account

Zoals eerder vermeld, kunt u optioneel een Azure-opslagaccount voor algemeen gebruik koppelen aan uw Batch-account. De functie voor [toepassingspakketten](batch-application-packages.md) van Batch maakt gebruik van Azure Blob-opslag. Dit geldt ook voor de [Batch File Conventions .NET](batch-task-output.md)-bibliotheek. Deze optionele functies helpen u bij het implementeren van de toepassingen die met uw Batch-taken worden uitgevoerd, en het behouden van de geproduceerde gegevens.

U wordt aangeraden een nieuw opslagaccount te maken dat alleen wordt gebruikt voor het Batch-account.

![Een opslagaccount voor 'Algemeen gebruik' maken][storage_account]

> [!NOTE] 
> Azure Batch ondersteunt momenteel alleen het opslagaccounttype voor algemeen gebruik. Dit accounttype wordt beschreven in stap 5 [Een opslagaccount maken] (.. / storage/storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](../storage/storage-create-storage-account.md).
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

Houd rekening met deze quota bij het ontwerpen en schalen van de Batch-workloads. Als de groep bijvoorbeeld het doelaantal rekenknooppunten dat u hebt opgegeven niet haalt, hebt u mogelijk de limiet voor het kernquotum van uw Batch-account bereikt.

De quota voor Batch-accounts gelden per regio per abonnement. U kunt dus standaard meer dan één Batch-account hebben, zolang de accounts zich maar in verschillende regio's bevinden. U kunt in één Batch-account meerdere Batch-workloads uitvoeren of uw workloads verdelen over Batch-accounts in hetzelfde abonnement maar in verschillende Azure-regio's.

Daarnaast kunnen veel van deze quota eenvoudig worden verhoogd met een aanvraag voor gratis productondersteuning. Deze aanvraag kunt u indienen in Azure Portal. Zie [Quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor meer informatie over aanvragen voor het verhogen van uw quotum.

## <a name="other-batch-account-management-options"></a>Andere beheeropties voor uw Batch-account
Naast het gebruik van de Azure Portal kunt u ook op de volgende manieren Batch-accounts maken en beheren:

* [Batch-PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Volgende stappen
* Zie [Overzicht van Azure Batch-functies](batch-api-basics.md) voor meer informatie over concepten en functies van de Batch-service. In het artikel worden de primaire Batch-resources zoals pools, rekenknooppunten, jobs en taken besproken en vindt u een overzicht van de servicefuncties waarmee grootschalige rekenworkloads kunnen worden uitgevoerd.
* Leer de basisbeginselen van het ontwikkelen van een voor Batch geschikte toepassing met behulp van de [clientbibliotheek Batch .NET](batch-dotnet-get-started.md). Het [inleidende artikel](batch-dotnet-get-started.md) leidt u door een werkende toepassing die gebruikmaakt van de Batch-service voor het uitvoeren van een workload op meerdere rekenknooppunten. U vindt er ook informatie over het gebruik van Azure Storage voor het faseren en ophalen van een workloadbestand.

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

