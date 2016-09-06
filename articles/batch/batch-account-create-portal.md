<properties
    pageTitle="Azure Batch-account maken | Microsoft Azure"
    description="Informatie over het maken van een Azure Batch-account in Azure Portal voor het uitvoeren van grootschalige parallelle workloads in de cloud"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/26/2016"
    ms.author="marsma"/>

# Een Azure Batch-account in de Azure Portal maken

> [AZURE.SELECTOR]
- [Azure Portal](batch-account-create-portal.md)
- [Batch Management .NET](batch-management-dotnet.md)

Leer hoe u een Azure Batch-account maakt in de [Azure Portal][azure_portal] en waar u belangrijke accounteigenschappen kunt vinden, zoals toegangssleutels en account-URL's. We bespreken ook Batch-prijzen en het koppelen van een Azure Storage-account aan uw Batch-account zodat u [toepassingspakketten](batch-application-packages.md) kunt gebruiken en [taken kunt uitvoeren](batch-task-output.md).

## Batch-account maken

1. Meld u aan bij [Azure Portal][azure_portal].

2. Klik op **Nieuw** > **Virtuele machines** > **Batch-service**.

    ![Batch in de Marketplace][marketplace_portal]

3. De blade **Nieuw Batch-account** wordt weergegeven. Bij de items *a* tot *e* hieronder vindt u voor elk blade-element een beschrijving.

    ![Batch-account maken][account_portal]

    a. **Accountnaam**: Een unieke naam voor uw Batch-account. Deze naam moet uniek zijn in de Azure-regio waarin het account wordt gemaakt (zie *Locatie* hieronder). Deze naam mag alleen kleine letters en cijfers bevatten en moet 3 tot 24 tekens lang zijn.

    b. **Abonnement**: Een abonnement om het Batch-account in te maken. Als u slechts één abonnement hebt, is het standaard geselecteerd.

    c. **Resourcegroep**: Een bestaande resourcegroep voor het nieuwe Batch-account. U kunt ook optioneel een nieuwe resourcegroep maken.

    d. **Locatie**: Een Azure-regio om het Batch-account in te maken. Alleen de regio's die door uw abonnement en resourcegroep worden ondersteund, worden als opties weergegeven.

    e. **Opslagaccount** (optioneel): Een **algemeen** opslagaccount dat u aan het nieuwe Batch-account koppelt. Zie [Gekoppeld Azure Storage-account](#linked-azure-storage-account) hieronder voor meer informatie.

4. Klik op **Maken** om het account te maken.

  In de portal wordt aangegeven dat het account wordt **geïmplementeerd**. Bij voltooiing wordt de melding **Implementaties voltooid** weergegeven in *Meldingen*.

## Eigenschappen van Batch-account weergeven

Zodra het account is gemaakt, kunt u de blade **Batch-account** openen om naar de instellingen en eigenschappen te gaan. U hebt toegang tot alle eigenschappen en accountinstellingen via het linkermenu van de blade Batch-account.

![Blade Batch-account in Azure Portal][account_blade]

* **Batch-account-URL**: toepassingen die u maakt met de [Batch-ontwikkelings-API's](batch-technical-overview.md#batch-development-apis) moeten beschikken over een account-URL voor het beheer van resources het uitvoeren van taken in het account. Een Batch-account-URL heeft de volgende indeling:

    `https://<account_name>.<region>.batch.azure.com`

![Batch-account-URL in de portal][account_url]

* **Toegangssleutels**: uw toepassingen moeten ook beschikken over een toegangssleutel als u met resources in uw Batch-account werkt. Als u de toegangssleutels van uw Batch-account wilt bekijken of genereren, voert u `keys` in in het vak **Zoeken** van het linkermenu op de blade Batch-account en selecteert u vervolgens **Sleutels**.

    ![Batch-accountsleutels in Azure Portal][account_keys]

## Prijzen

Batch-accounts worden alleen aangeboden in een 'Gratis laag'. Dit betekent dat er voor het Batch-account zelf geen kosten in rekening worden gebracht. De onderliggende Azure-rekenresources die door uw Batch-oplossingen worden gebruikt, worden wel in rekening gebracht, evenals de resources die door andere services worden gebruikt wanneer uw workloads worden uitgevoerd. Er worden bijvoorbeeld kosten in rekening gebracht voor de rekenknooppunten in uw groepen en voor de gegevens die u in Azure Storage opslaat als invoer of uitvoer voor uw taken. Als u de functie voor [toepassingspakketten](batch-application-packages.md) van Batch gebruikt, worden de Azure Storage-resources die worden gebruikt om uw toepassingspakketten op te slaan in rekening gebracht. Zie [Prijzen van Batch][batch_pricing] voor meer informatie.

## Gekoppeld Azure Storage-account

Zoals eerder vermeld, kunt u (optioneel) een opslagaccount voor **Algemeen gebruik** koppelen met uw Batch-account. De [toepassingspakketten](batch-application-packages.md)-functie van Batch maakt gebruik van blob-opslag in een gekoppeld opslagaccount voor Algemeen gebruik. Dit geldt ook voor de [Batch-bestandsconventies-.NET](batch-task-output.md)-bibliotheek. Deze optionele functies helpen u bij het implementeren van de toepassingen die uw Batch-taken uitvoeren en het behouden van de gegevens die deze toepassingen produceren.

Batch ondersteunt momenteel *alleen* het opslagaccounttype **Algemeen**, zoals beschreven in stap 5 [Een opslagaccount maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](../storage/storage-create-storage-account.md). Wanneer u een Azure Storage-account aan uw Batch-account koppelt, zorgt u ervoor dat u *alleen* een **algemeen** opslagaccount koppelt.

![Een opslagaccount voor 'Algemeen gebruik' maken][storage_account]

U wordt aangeraden een opslagaccount te maken dat alleen wordt gebruikt door uw Batch-account.

>[AZURE.WARNING] Wees voorzichtig bij het opnieuw genereren van de toegangssleutels van een gekoppeld opslagaccount. Genereer slechts één opslagaccountsleutel opnieuw en klik op **Sleutels synchroniseren** op de blade voor het gekoppelde opslagaccount. Wacht vijf minuten tot de sleutels aan de rekenknooppunten in uw groepen zijn doorgegeven en genereer en synchroniseer de andere sleutel opnieuw, indien nodig. Als u beide sleutels tegelijk opnieuw genereert, kunnen uw rekenknooppunten geen van beide sleutels synchroniseren en zullen zij toegang verliezen tot het opslagaccount.

  ![Sleutels van opslagaccount opnieuw genereren][4]

## Quota en limieten voor Batch-service

Houd er rekening mee dat er bepaalde [quota en limieten](batch-quota-limit.md) van toepassing zijn op Batch-account, net als bij uw Azure-abonnement en andere Azure-services. De huidige quota voor een Batch-account worden in de portal weergegeven bij de **eigenschappen** van het account.

![Batch-accountquota in Azure Portal][quotas]

Houd rekening met deze quota bij het ontwerpen en schalen van de Batch-workloads. Als de groep bijvoorbeeld het doelaantal rekenknooppunten dat u hebt opgegeven niet haalt, hebt u mogelijk de limiet voor het kernquotum van uw Batch-account bereikt.

Houd er ook rekening mee dat u meer dan één Batch-account voor uw Azure-abonnement mag hebben. U kunt in één Batch-account meerdere Batch-workloads uitvoeren of uw workloads verdelen over Batch-accounts in hetzelfde abonnement maar in verschillende Azure-regio's.

Veel van deze quota kunnen worden verhoogd met een gratis verzoek tot productondersteuning. Dit kunt u indienen in de Azure Portal. Zie [Quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor meer informatie over aanvragen voor het verhogen van uw quotum.

## Andere beheeropties voor uw Batch-account

Naast het gebruik van de Azure Portal kunt u ook op de volgende manieren Batch-accounts maken en beheren:

* [Batch-PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Batch Management .NET](batch-management-dotnet.md)

## Volgende stappen

* Zie [Overzicht van Azure Batch-functies](batch-api-basics.md) voor meer informatie over concepten en functies van de Batch-service. In het artikel worden de primaire Batch-resources zoals pools, rekenknooppunten, jobs en taken besproken en vindt u een overzicht van de servicefuncties waarmee grootschalige rekenworkloads kunnen worden uitgevoerd.

* Leer de basisbeginselen van het ontwikkelen van een voor Batch geschikte toepassing met behulp van de [clientbibliotheek Batch .NET](batch-dotnet-get-started.md). Het [inleidende artikel](batch-dotnet-get-started.md) leidt u door een werkende toepassing die gebruikmaakt van de Batch-service voor het uitvoeren van een workload op meerdere rekenknooppunten. U vindt er ook informatie over het gebruik van Azure Storage voor het faseren en ophalen van een workloadbestand.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Sleutels van opslagaccount opnieuw genereren"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png



<!--HONumber=ago16_HO5-->


