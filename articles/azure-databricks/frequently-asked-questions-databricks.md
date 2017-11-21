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
ms.openlocfilehash: 6bb542537ec713be272f7e58e0b247763214ef4a
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="azure-databricks-preview-common-questions-and-help"></a>Azure Databricks Preview: Veelgestelde vragen en help

Dit artikel worden de bovenste query's die moet u wellicht hebben betrekking op Azure Databricks. Het bevat ook enkele veelvoorkomende problemen die u tegenkomen kunt tijdens het gebruik van Azure Databricks. Zie voor meer informatie over Azure Databricks [wat is Azure Databricks?](what-is-azure-databricks.md) 

## <a name="common-questions"></a>Veelgestelde vragen

Deze sectie vindt de algemene vragen met betrekking tot Azure Databricks.

### <a name="can-i-use-my-own-keys-for-local-encryption"></a>Kan ik mijn eigen sleutels gebruiken voor lokale versleuteling? 
In de huidige release wordt met behulp van uw eigen sleutels van Azure Keyvault niet ondersteund. 

### <a name="can-i-use-azure-vnets-with-azure-databricks"></a>Kan ik Azure VNETs met Azure Databricks gebruiken?
Een nieuw VNET wordt gemaakt als onderdeel van Azure Databricks inrichten. U kunt uw eigen Azure VNET niet gebruiken als onderdeel van deze release.

### <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Hoe krijg ik toegang tot Azure Data Lake Store van een laptop? 

1. Inrichten van een Service-Principal in Azure Active Directory, en noteer de sleutel.
2. Benodigde machtigingen toewijzen aan de Service-Principal in Azure Data Lake Store.
3. Voor toegang tot een bestand in Azure Data Lake Store, door de Service-Principal-referenties in de notebook te gebruiken.

Zie voor meer informatie [gebruik Data Lake Store met Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="troubleshooting"></a>Problemen oplossen

Deze sectie beschrijft het oplossen van veelvoorkomende problemen met Azure Databricks.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Probleem: Dit abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.Databricks'

**Foutbericht**

Dit abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.Databricks'. Zie https://aka.ms/rps-not-found voor het registreren van abonnementen. (Code: MissingSubscriptionRegistration)

**Oplossing**

1. Ga naar [Azure-portal](https://portal.azure.com).
2. Klik op **abonnementen**, het abonnement dat u gebruikt, en klik vervolgens op **resourceproviders**. 
3. In de lijst met resourceproviders, tegen **Microsoft.Databricks**, klikt u op **registreren**. U moet de rol van inzender of eigenaar hebben op het abonnement op de registerbronprovider is.


### <a name="issue-your-account-email-does-not-have-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Probleem: Uw account {e} heeft geen eigenaar of bijdrager rol op de bron van de werkruimte Databricks in de Azure portal.

**Foutbericht**

Uw account {e} heeft geen eigenaar of bijdrager rol op de bron van de werkruimte Databricks in de Azure portal. Deze fout kan ook optreden als u een gastgebruiker in de tenant. Vraag uw beheerder u toegang tot of u wilt toevoegen als een gebruiker rechtstreeks in de werkruimte Databricks toe te kennen. 

**Oplossing**

Hier volgen een aantal oplossingen voor dit probleem:

* Voor het initialiseren van de tenant, moet u zijn aangemeld als een gewone gebruiker van de tenant, niet een gastgebruiker. U moet ook de rol van inzender hebben op de bron van de werkruimte Databricks. U kunt een toegang gebruiker van de **toegangsbeheer (IAM)** tab binnen uw Azure-Databricks werkruimte in de Azure portal.

* Deze fout kan ook optreden als de domeinnaam van uw e-mailbericht is toegewezen aan meerdere Active Directory's. Maak een nieuwe gebruiker in de Active Directory met het abonnement met uw werkruimte Databricks om dit probleem omzeilen.

    a. In de Azure portal, gaat u naar Azure Active Directory, klikt u op **gebruikers en groepen**, klikt u op **toevoegen van een gebruiker**.

    b. Een gebruiker toevoegen met een `@<tenant_name>.onmicrosoft.com` in plaats van een e-mail @< uw_domein > e-mail. U vindt de < tenant_name >. onmicrosoft.com die zijn gekoppeld aan uw Active Directory in de **aangepaste domeinen** onder Azure Active Directory in de Azure portal.
    
    c. Deze nieuwe gebruiker verlenen **Inzender** -rol op de bron van de werkruimte Databricks.
    
    d. Meld u aan bij de Azure-portal met de nieuwe gebruiker en de werkruimte Databricks vinden.
    
    e. Start de Databricks werkruimte als deze gebruiker.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Probleem: Uw account {e} is niet geregistreerd in Databricks 

**Oplossing**

Als u de werkruimte niet hebt gemaakt en u wordt toegevoegd als een gebruiker van de werkruimte, neem dan contact op met de persoon die de werkruimte toe te voegen u met de Azure Databricks-beheerconsole hebt gemaakt. Zie voor instructies [toevoegen en het beheren van gebruikers](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Als u de werkruimte hebt gemaakt en nog steeds dat u deze fout, probeer 'Werkruimte initialiseren' nogmaals op via de Azure-portal.

### <a name="issue-cloud-provider-launch-failure-publicipcountlimitreached-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Probleem: Cloud starten providerfout (PublicIPCountLimitReached): een cloud-providerfout opgetreden tijdens het instellen van het cluster

**Foutbericht**

Cloud Provider starten is mislukt: Er is een cloud provider-fout opgetreden tijdens het instellen van het cluster. Zie de handleiding Databricks voor meer informatie. Azure-foutcode: PublicIPCountLimitReached. Azure-foutbericht: kan meer dan 60 openbare IP-adressen voor dit abonnement niet maken in deze regio.

**Oplossing**

Azure Databricks clusters gebruiken één openbaar IP-adres per knooppunt. Als uw abonnement al voor het openbare IP's gebruikt is, moet u [aanvraag het quotum te verhogen](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request). Kies **quotum** als de **Type probleem**, **Networking: ARM** als de **Quotumtype**, en een toename van de quota voor openbaar IP-adres van aanvragen **Details**. Bijvoorbeeld als de limiet momenteel 60 is en u wilt maken van een cluster met 100 knooppunt, een hogere limiet aan 160 aanvragen.

### <a name="issue-cloud-provider-launch-failure-missingsubscriptionregistration-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Probleem: Cloud starten providerfout (MissingSubscriptionRegistration): een cloud-providerfout opgetreden tijdens het instellen van het cluster

**Foutbericht**

Cloud Provider starten is mislukt: Er is een cloud provider-fout opgetreden tijdens het instellen van het cluster. Zie de handleiding Databricks voor meer informatie.
Azure-foutcode: MissingSubscriptionRegistration Azure-foutbericht: het abonnement is niet geregistreerd voor gebruik van de naamruimte 'Microsoft.Compute'. Zie https://aka.ms/rps-not-found voor het registreren van abonnementen

**Oplossing**

1. Ga naar [Azure-portal](https://portal.azure.com).
2. Klik op **abonnementen**, het abonnement dat u gebruikt, en klik vervolgens op **resourceproviders**. 
3. In de lijst met resourceproviders, tegen **Microsoft.Compute**, klikt u op **registreren**. U moet de rol van inzender of eigenaar hebben op het abonnement op de registerbronprovider is.

Zie [resourceproviders en typen](../azure-resource-manager/resource-manager-supported-services.md) voor instructies gedetailleerde.

## <a name="next-steps"></a>Volgende stappen
Zie voor stapsgewijze instructies voor het maken van een gegevensfactory versie 2 van de volgende zelfstudies:

- [Snelstartgids: Aan de slag met Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Wat is Azure Databricks?](what-is-azure-databricks.md)

