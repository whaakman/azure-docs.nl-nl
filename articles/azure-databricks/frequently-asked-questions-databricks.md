---
title: 'Azure Databricks: Veelgestelde vragen en Help-informatie'
description: Krijg antwoorden op veelgestelde vragen en oplossen van problemen met informatie over Azure Databricks.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 3bcc511ec6ad8a246c2b1b3a33eb59043a45830e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138358"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Veelgestelde vragen over Azure Databricks

Dit artikel worden de meestgestelde vragen die u mogelijk met betrekking tot Azure Databricks. Het bevat ook enkele veelvoorkomende problemen die tijdens het gebruik van Databricks mogelijk hebt. Zie voor meer informatie, [wat is Azure Databricks](what-is-azure-databricks.md). 

## <a name="can-i-use-azure-key-vault-to-store-keyssecrets-to-be-used-in-azure-databricks"></a>Kan ik Azure Key Vault gebruiken voor het opslaan van sleutels/geheimen moet worden gebruikt in Azure Databricks?
Ja. U kunt Azure Key Vault gebruiken voor het opslaan van sleutels/geheimen voor gebruik met Azure Databricks. Zie voor meer informatie, [Azure Key Vault-back-scopes](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#akv-ss).


## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Kan ik virtuele Azure-netwerken met Databricks gebruiken?
Ja. Met Azure Databricks kunt u een Azure Virtual Network (VNET). Zie voor meer informatie, [Azure Databricks implementeren in uw Azure Virtual Network](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html).

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Hoe krijg ik toegang tot Azure Data Lake Store vanuit een notitieblok? 

Volg deze stappen:
1. Inrichten van een service-principal in Azure Active Directory (Azure AD), en noteer de sleutel.
1. De vereiste machtigingen toewijzen aan de service-principal in Data Lake Store.
1. Voor toegang tot een bestand in Data Lake Store, gebruikt u de referenties voor de service-principal in notitieblok.

Zie voor meer informatie, [gebruik Data Lake Store met Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html).

## <a name="fix-common-problems"></a>Veelvoorkomende problemen oplossen

Hier volgen enkele problemen met Databricks optreden.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Probleem: In dit abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.Databricks'

#### <a name="error-message"></a>Foutbericht

"Dit abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.Databricks'. Zie https://aka.ms/rps-not-found voor informatie over het registreren van abonnementen. (Code: MissingSubscriptionRegistration) "

#### <a name="solution"></a>Oplossing

1. Ga naar de [Azure Portal](https://portal.azure.com).
1. Selecteer **abonnementen**, het abonnement dat u gebruikt, en vervolgens **resourceproviders**. 
1. In de lijst van resourceproviders, op basis van **Microsoft.Databricks**, selecteer **registreren**. U moet de rol van inzender of eigenaar hebben op het abonnement om de resourceprovider te registreren.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Probleem: Uw account {email} heeft geen de rol eigenaar of bijdrager van de bron van de Databricks-werkruimte in Azure portal

#### <a name="error-message"></a>Foutbericht

'Uw account {email} heeft geen eigenaar of de rol Inzender voor de bron van de Databricks-werkruimte in Azure portal. Deze fout kan ook optreden als u een gastgebruiker in de tenant. Vraag uw beheerder om te verlenen u toegang tot of u wilt toevoegen als een gebruiker rechtstreeks in de Databricks-werkruimte." 

#### <a name="solution"></a>Oplossing

Hier volgen een aantal oplossingen voor dit probleem:

* Voor het initialiseren van de tenant, moet u zijn aangemeld als een gewone gebruiker van de tenant, niet als een gastgebruiker. U moet ook een rol van inzender hebben op de bron van Databricks-werkruimte. U kunt een gebruiker de toegang van verlenen de **toegangsbeheer (IAM)** tabblad in uw Databricks-werkruimte in de Azure-portal.

* Deze fout kan ook optreden als de naam van uw e-domein is toegewezen aan meerdere mappen in Azure AD. U kunt dit probleem omzeilen, moet u een nieuwe gebruiker maken in de map waarin het abonnement met uw Databricks-werkruimte.

    a. In de Azure-portal, gaat u naar Azure AD. Selecteer **gebruikers en groepen** > **toevoegen van een gebruiker**.

    b. Toevoegen van een gebruiker met een `@<tenant_name>.onmicrosoft.com` in plaats van een e-mail `@<your_domain>` e-mailbericht. U vindt deze optie in **aangepaste domeinen**, onder Azure AD in Azure portal.
    
    c. Verleent u deze nieuwe gebruiker de **Inzender** -rol op de bron van Databricks-werkruimte.
    
    d. Aanmelden bij de Azure-portal met de nieuwe gebruiker en zoeken van de Databricks-werkruimte.
    
    e. Start de Databricks-werkruimte als deze gebruiker.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Probleem: Uw account {email} is niet geregistreerd in Databricks 

#### <a name="solution"></a>Oplossing

Als u niet de werkruimte hebt gemaakt en u wordt toegevoegd als een gebruiker, moet u contact op met de persoon die de werkruimte hebt gemaakt. Hebben dat deze persoon u toevoegen met behulp van de Azure Databricks-beheerconsole. Zie voor instructies [toevoegen en beheren van gebruikers](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Als u de werkruimte hebt gemaakt en nog steeds deze foutmelding, probeert u **werkruimte initialiseren** opnieuw vanuit de Azure-portal.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Probleem: Fout bij het starten van de provider Cloud tijdens het instellen van het cluster (PublicIPCountLimitReached)

#### <a name="error-message"></a>Foutbericht

"Fout bij het starten van de Provider in de cloud: Er is een cloud-provider-fout opgetreden tijdens het instellen van het cluster. Zie de Databricks-gids voor meer informatie. Azure-foutcode: PublicIPCountLimitReached. Azure-foutbericht: kan niet meer dan 60 openbare IP-adressen voor dit abonnement in deze regio maken. "

#### <a name="solution"></a>Oplossing

Databricks-clusters gebruiken één openbaar IP-adres per knooppunt. Als uw abonnement al voor alle bijbehorende openbare IP-adressen gebruikt is, moet u [aanvraag om het quotum te verhogen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Kies **quotum** als de **Type probleem**, en **netwerk: ARM** als de **Quotumtype**. In **Details**, een verhoging van het openbare IP-adres aanvragen. Bijvoorbeeld, als de limiet momenteel 60 is en u wilt maken van een cluster met 100 knooppunten, een hogere limiet aan 160 aanvragen.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Probleem: Een tweede type cloud provider starten fout tijdens het instellen van het cluster (MissingSubscriptionRegistration)

#### <a name="error-message"></a>Foutbericht

"Fout bij het starten van de Provider in de cloud: Er is een cloud-provider-fout opgetreden tijdens het instellen van het cluster. Zie de Databricks-gids voor meer informatie.
Azure-foutcode: MissingSubscriptionRegistration Azure-foutbericht: het abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.Compute'. Zie https://aka.ms/rps-not-found voor informatie over het registreren van abonnementen. "

#### <a name="solution"></a>Oplossing

1. Ga naar de [Azure Portal](https://portal.azure.com).
1. Selecteer **abonnementen**, het abonnement dat u gebruikt, en vervolgens **resourceproviders**. 
1. In de lijst van resourceproviders, op basis van **Microsoft.Compute**, selecteer **registreren**. U moet de rol van inzender of eigenaar hebben op het abonnement om de resourceprovider te registreren.

Zie voor meer instructies, [resourceproviders en typen](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Probleem: Azure Databricks moet machtigingen voor toegang tot bronnen in uw organisatie die alleen een beheerder kan verlenen.

#### <a name="background"></a>Achtergrond

Azure Databricks is geïntegreerd met Azure Active Directory. U kunt machtigingen in Azure Databricks (bijvoorbeeld op laptops en -clusters) instellen door gebruikers van Azure AD op te geven. Voor Azure Databricks om de namen van de gebruikers van uw Azure AD te kunnen hiervoor is leestoegang tot die informatie en toestemming te geven. Als de toestemming nog niet beschikbaar is, ziet u de fout.

#### <a name="solution"></a>Oplossing

Meld u aan als globale beheerder bij de Azure-portal. Voor Azure Active Directory, gaat u naar de **gebruikersinstellingen** tabblad en zorg ervoor dat **gebruikers toestemming kunnen geven voor apps die toegang tot bedrijfsgegevens namens hen** is ingesteld op **Ja**.

## <a name="next-steps"></a>Volgende stappen

- [Snelstartgids: Aan de slag met Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Wat is Azure Databricks?](what-is-azure-databricks.md)

