---
title: 'Azure Databricks: Veelgestelde vragen en help | Microsoft Docs'
description: Vind antwoorden op veelgestelde vragen en informatie over probleemoplossing over Azure Databricks.
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: nitinme
ms.openlocfilehash: fb77ec001f9f52e0a974f8765f458f831fb63908
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Veelgestelde vragen over Azure Databricks

Dit artikel worden de bovenste query's die u mogelijk met betrekking tot Azure Databricks. Enkele veelvoorkomende problemen die u tijdens het gebruik van Databricks wellicht ook geeft. Zie voor meer informatie [wat is Azure Databricks](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Kan ik mijn eigen sleutels gebruiken voor lokale versleuteling? 
In de huidige release wordt met behulp van uw eigen sleutels van Azure Sleutelkluis niet ondersteund. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Kan ik virtuele netwerken in Azure met Databricks gebruiken?
Een nieuw virtueel netwerk wordt gemaakt als onderdeel van het Databricks inrichten. In deze release kunt u uw eigen Azure virtual network niet gebruiken.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Hoe krijg ik toegang tot Azure Data Lake Store van een laptop? 

Volg deze stappen:
1. Inrichten van een service-principal in Azure Active Directory (Azure AD), en noteer de sleutel.
2. De vereiste machtigingen toewijzen aan de service-principal in Data Lake Store.
3. Voor toegang tot een bestand in Data Lake Store, de referenties voor de service-principal in Notebook te gebruiken.

Zie voor meer informatie [gebruik Data Lake Store met Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="fix-common-problems"></a>Veelvoorkomende problemen oplossen

Hier volgen enkele mogelijke problemen met Databricks.

### <a name="this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Dit abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.Databricks'

#### <a name="error-message"></a>Bericht invoeren

"Dit abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.Databricks'. Zie https://aka.ms/rps-not-found voor het registreren van abonnementen. (Code: MissingSubscriptionRegistration) '

#### <a name="solution"></a>Oplossing

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Selecteer **abonnementen**, het abonnement dat u gebruikt, en vervolgens **resourceproviders**. 
3. In de lijst met resourceproviders, tegen **Microsoft.Databricks**, selecteer **registreren**. U moet de rol van inzender of eigenaar hebben op het abonnement op de resourceprovider registreren.


### <a name="your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Uw account {e} heeft geen de eigenaar of bijdrager-rol op de bron van de werkruimte Databricks in de Azure portal

#### <a name="error-message"></a>Bericht invoeren

'Uw account {e} heeft geen eigenaar of bijdrager rol op de bron van de werkruimte Databricks in de Azure portal. Deze fout kan ook optreden als u een gastgebruiker in de tenant. Vraag uw beheerder u toegang tot of u wilt toevoegen als een gebruiker rechtstreeks in de werkruimte Databricks toe te kennen." 

#### <a name="solution"></a>Oplossing

Hier volgen een aantal oplossingen voor dit probleem:

* Voor het initialiseren van de tenant, moet u zijn aangemeld als een gewone gebruiker van de tenant, niet als een gastgebruiker. U moet ook de rol van inzender hebben op de bron van de werkruimte Databricks. U kunt een toegang gebruiker van de **toegangsbeheer (IAM)** tab binnen uw werkruimte Databricks in de Azure portal.

* Deze fout kan ook optreden als de domeinnaam van uw e-mailbericht is toegewezen aan meerdere mappen in Azure AD. Om dit probleem omzeilen door in de map die het abonnement met uw werkruimte Databricks bevat een nieuwe gebruiker te maken.

    a. In de Azure portal, gaat u naar Azure AD. Selecteer **gebruikers en groepen** > **toevoegen van een gebruiker**.

    b. Een gebruiker toevoegen met een `@<tenant_name>.onmicrosoft.com` in plaats van een e-mail `@<your_domain>` e-mailbericht. U vindt dit in **aangepaste domeinen**, onder Azure AD in de Azure portal.
    
    c. Deze nieuwe gebruiker toestaan het **Inzender** -rol op de bron van de werkruimte Databricks.
    
    d. Aanmelden bij de Azure-portal met de nieuwe gebruiker en de werkruimte Databricks vinden.
    
    e. Start de Databricks werkruimte als deze gebruiker.


### <a name="your-account-email-has-not-been-registered-in-databricks"></a>Uw account {e} is niet geregistreerd in Databricks 

#### <a name="solution"></a>Oplossing

Als u de werkruimte niet hebt gemaakt en u wordt toegevoegd als een gebruiker, moet u contact op met de persoon die de werkruimte hebt gemaakt. Deze persoon u toevoegen met behulp van de beheerconsole van Azure Databricks hebben. Zie voor instructies [toevoegen en het beheren van gebruikers](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Als u de werkruimte hebt gemaakt en nog steeds u deze fout krijgt, probeert u **initialiseren werkruimte** opnieuw vanuit de Azure-portal.

### <a name="cloud-provider-launch-failure-while-setting-up-the-cluster"></a>Fout bij het starten van de provider cloud tijdens het instellen van het cluster

#### <a name="error-message"></a>Bericht invoeren

"Fout bij het starten van de Provider cloud: een cloud-providerfout opgetreden tijdens het instellen van het cluster. Zie de handleiding Databricks voor meer informatie. Azure-foutcode: PublicIPCountLimitReached. Azure-foutbericht: kan meer dan 60 openbare IP-adressen voor dit abonnement niet maken in deze regio. "

#### <a name="solution"></a>Oplossing

Databricks clusters gebruiken één openbaar IP-adres per knooppunt. Als uw abonnement al voor het openbare IP's gebruikt is, moet u [aanvraag het quotum te verhogen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Kies **quotum** als de **probleemtype**, en **Networking: ARM** als de **Quotumtype**. In **Details**, een verhoging van de quota voor openbaar IP-adres aanvragen. Bijvoorbeeld als de limiet momenteel 60 is en u wilt maken van een cluster met 100-knooppunt, een hogere limiet aan 160 aanvragen.

### <a name="a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster"></a>Een tweede type cloud providerfout starten tijdens het instellen van het cluster

#### <a name="error-message"></a>Bericht invoeren

"Fout bij het starten van de Provider cloud: een cloud-providerfout opgetreden tijdens het instellen van het cluster. Zie de handleiding Databricks voor meer informatie.
Azure-foutcode: MissingSubscriptionRegistration Azure-foutbericht: het abonnement is niet geregistreerd voor gebruik van de naamruimte 'Microsoft.Compute'. Zie https://aka.ms/rps-not-found voor het registreren van abonnementen."

#### <a name="solution"></a>Oplossing

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Selecteer **abonnementen**, het abonnement dat u gebruikt, en vervolgens **resourceproviders**. 
3. In de lijst met resourceproviders, tegen **Microsoft.Compute**, selecteer **registreren**. U moet de rol van inzender of eigenaar hebben op het abonnement op de resourceprovider registreren.

Zie voor meer instructies, [resourceproviders en typen](../azure-resource-manager/resource-manager-supported-services.md).

## <a name="next-steps"></a>Volgende stappen

- [Snelstartgids: Aan de slag met Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Wat is Azure Databricks?](what-is-azure-databricks.md)

