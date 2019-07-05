---
title: Bekijk uw facturering accounts in Azure portal | Microsoft Docs
description: Leer hoe u uw facturering om accounts te bekijken in Azure portal.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2018
ms.author: banders
ms.openlocfilehash: 36430e9b0a4554761d53b537d3c32fa57068eabb
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490222"
---
# <a name="view-billing-accounts-in-azure-portal"></a>Facturering-accounts weergeven in Azure portal  

Factureringsaccount wordt gemaakt wanneer u zich aanmeldt om Azure te gebruiken. U gebruikt uw factureringsrekening voor het beheren van uw facturen, betalingen en kosten bijhouden. U kunt hebben toegang tot meerdere accounts voor facturering. Bijvoorbeeld, u mogelijk hebben geregistreerd voor Azure voor uw persoonlijke projecten. U kunt ook toegang via een Enterprise Agreement of een Microsoft-KLANTOVEREENKOMST van uw organisatie hebben. Voor elk van deze scenario's moet u een afzonderlijke rekening.

Azure portal ondersteunt momenteel het volgende type facturering accounts:

- **Microsoft Online Services Program**: Factureringsaccount voor een Microsoft Online Services-programma wordt gemaakt wanneer u zich registreren voor Azure via de Azure-website. Bijvoorbeeld, wanneer u zich registreren voor een [gratis Azure-Account](https://azure.microsoft.com/offers/ms-azr-0044p/), [-account met betalen per gebruik-tarieven](https://azure.microsoft.com/offers/ms-azr-0003p/) of als een [Visual studio-abonnee](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).

- **Enterprise Overeenkomst**: Factureringsaccount voor een Enterprise Agreement wordt gemaakt wanneer uw organisatie zich een [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) om Azure te gebruiken.

- **Microsoft KLANTOVEREENKOMST**: Een facturering voor de KLANTOVEREENKOMST van een Microsoft-account wordt gemaakt wanneer uw organisatie werkt met een medewerker van Microsoft een overeenkomst van de klant Microsoft te ondertekenen. Sommige klanten in bepaalde regio's, die zich aanmelden via de Azure-website voor een [-account met betalen per gebruik-tarieven](https://azure.microsoft.com/offers/ms-azr-0003p/) of upgrade hun [gratis Azure-Account](https://azure.microsoft.com/offers/ms-azr-0044p/) mogelijk een rekening voor een Microsoft Customer Ook overeenkomst. Zie voor meer informatie, [aan de slag met uw factureringsrekening voor Microsoft-KLANTOVEREENKOMST](billing-mca-overview.md).

<!--Todo Add section to identify the type of accounts -->

## <a name="scopes-for-billing-accounts"></a>Bereiken voor de facturering van accounts
Een scope is een knooppunt binnen een factureringsaccount die gebruikers gebruiken voor het weergeven en beheren van facturering. Het is waar gebruikers factureringsgegevens, betalingen, facturen, beheren en uitvoeren van algemene accountbeheer. 

### <a name="microsoft-online-services-program"></a>Microsoft Online Services Program

|Scope  |Definitie  |
|---------|---------|
|Factureringsaccount     | Hiermee geeft u de eigenaar van een enkel (accountbeheerder) voor een of meer Azure-abonnementen. Een beheerder is gemachtigd voor het uitvoeren van verschillende facturering taken zoals het maken van abonnementen, facturen weergeven of wijzigen van de facturering voor abonnementen.  |
|Abonnement     |  Hiermee geeft u een groepering van Azure-resources. Factuur is gegenereerd toen dit bereik. Heeft een eigen betalingswijzen die worden gebruikt voor de factuur betalen.|


### <a name="enterprise-agreement"></a>Enterprise Agreement

|Scope  |Definitie  |
|---------|---------|
|Factureringsaccount    | Hiermee geeft u een Enterprise Agreement-inschrijving. Factuur is gegenereerd toen dit bereik. Het is opgebouwd met behulp van afdelingen en inschrijvingsaccounts.  |
|Afdeling     |  Optionele groepering van inschrijvingsaccounts.      |
|Inschrijvingsaccount     |  Hiermee geeft u de eigenaar van een één-account. Azure-abonnementen zijn gemaakt onder dit bereik.  |


### <a name="microsoft-customer-agreement"></a>Microsoft-klantovereenkomst

|Scope  |Taken  |
|---------|---------|
|Factureringsaccount     |   Hiermee geeft u een KLANTOVEREENKOMST voor meerdere Microsoft-producten en services. Het is opgebouwd met behulp van profielen voor facturering en -secties van de factuur.   |
|Facturering-profiel     |  Hiermee geeft u een factuur en de betalingswijzen. Factuur is gegenereerd toen dit bereik. Er kunnen meerdere factuur secties.      |
|Factuur-sectie     |   Vertegenwoordigt een groep van de kosten voor een factuur. Abonnementen en andere aankopen zijn gekoppeld aan dit bereik.    |


## <a name="switch-billing-scope-in-the-azure-portal"></a>Switch facturering bereik in de Azure-portal


1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken naar **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-view-all-accounts/billing-search-cost-management-billing.png)

3. Selecteer **alle facturering bereiken** aan de linkerkant.

   ![Schermafbeelding van alle facturering bereiken](./media/billing-view-all-accounts/billing-list-of-accounts.png)

   ** Worden er geen **alle facturering bereiken** als u alleen toegang tot een bereik hebt.

4. Selecteer een bereik om details weer te geven.



## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [analyseren van uw kosten](../cost-management/quick-acm-cost-analysis.md).