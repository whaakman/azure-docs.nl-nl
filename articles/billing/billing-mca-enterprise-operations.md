---
title: Enterprise Agreement-taken in Microsoft KLANTOVEREENKOMST - Azure | Microsoft Docs
description: Leer hoe u Enterprise Agreement-taken in uw nieuwe account voor de facturering uit te voeren.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/24/2018
ms.author: banders
ms.openlocfilehash: 9b76fa935263904957c87cd062c84d0607771369
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57341374"
---
# <a name="complete-enterprise-agreement-tasks-in-your-billing-account-for-a-microsoft-customer-agreement"></a>Enterprise Agreement-taken uitvoeren in uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST

Als uw organisatie heeft een Microsoft-KLANTOVEREENKOMST vernieuwen van uw Enterprise Agreement-inschrijving is ondertekend, wordt een nieuwe factureringsaccount gemaakt voor de overeenkomst. De facturering in uw nieuwe account is anders dan uw Enterprise Agreement ingedeeld. Dit artikel wordt beschreven hoe u de nieuwe factureringsaccount kunt gebruiken om uit te voeren taken die u in uw Enterprise Agreement hebt uitgevoerd.

## <a name="how-billing-is-organized-in-the-new-account"></a>Hoe facturering wordt georganiseerd in het nieuwe account

Het volgende diagram wordt beschreven hoe de facturering is georganiseerd in uw nieuwe account voor de facturering.

![Afbeelding van ea-mca-post-overgang-hiërarchie](./media/billing-mca-setup-account/mca-post-transition-hierarchy.png)

| Enterprise Agreement   | Microsoft-klantovereenkomst    |
|------------------------|--------------------------------------------------------|
| Registratie            | U kunt een facturering profiel gebruikt voor het beheren van facturering voor uw organisatie, die vergelijkbaar is met uw Enterprise Agreement-inschrijving. Enterprise-beheerders kunnen worden eigenaren van het profiel voor facturering. Zie voor meer informatie over facturering-profielen, [inzicht in facturering profielen](billing-mca-overview.md#understand-billing-profiles).
| Afdeling            | Een gedeelte van de factuur kunt u uw kosten weer te geven, die vergelijkbaar is met afdelingen in uw Enterprise Agreement-inschrijving organiseren. Afdeling wordt factuur secties en beheerders van de afdeling worden eigenaars van de secties met respectieve factuur. Zie voor meer informatie over de secties van de factuur, [begrijpen factuur secties](billing-mca-overview.md#understand-invoice-sections). |
| Account               | De accounts die zijn gemaakt in uw Enterprise Agreement worden niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de respectieve factuur voor hun afdeling. Eigenaars kunnen maken en beheren van abonnementen voor een sectie factuur. |

## <a name="changes-for-enterprise-administrators-in-the-new-billing-account"></a>Wijzigingen voor beheerders in een onderneming in de nieuwe factureringsaccount

De volgende wijzigingen zijn van toepassing op enterprise-beheerders kunnen een Enterprise-overeenkomst die aan de KLANTOVEREENKOMST van een Microsoft is vernieuwd.

- Een profiel voor facturering wordt voor uw inschrijving gemaakt. U gebruikt de facturering profiel voor het beheren van facturering voor uw organisatie, zoals uw Enterprise Agreement-inschrijving. Voor meer informatie over facturering profielen [inzicht in facturering profielen](billing-mca-overview.md#understand-billing-profiles).
- Een gedeelte van de factuur wordt voor elke afdeling in uw Enterprise Agreement-inschrijving gemaakt. U gebruikt de secties van de factuur voor het beheren van uw afdelingen. U kunt nieuwe factuur secties voor het instellen van aanvullende afdelingen maken. Zie voor meer informatie over de secties van de factuur, [begrijpen factuur secties](billing-mca-overview.md#understand-invoice-sections).
- U gebruikt de rol van de maker van Azure-abonnement op factuur secties aan andere gebruikers machtigen om op te maken van Azure-abonnement, zoals de accounts die zijn gemaakt in de Enterprise Agreement-inschrijving.
- U gebruikt de [Azure-portal](https://portal.azure.com) voor het beheren van facturering voor uw organisatie, in plaats van de Azure EA-portal.

Op de nieuwe factureringsaccount krijgt u de volgende rollen:

**Facturering-profieleigenaar** -u de rol van de facturering profiel eigenaar voor de facturering profiel dat is gemaakt tijdens de overeenkomst is ondertekend zijn toegewezen. De rol kunt u de facturering voor uw organisatie beheren. U kunt kosten in rekening gebracht en facturen weergeven, organiseren kosten op de factuur, betalingswijzen beheren en toegang tot de facturering van uw organisatie beheren.

**De eigenaar van de factuur-sectie** -u kunt de rol van eigenaar sectie factuur op alle facturen secties die zijn gemaakt voor de afdelingen in uw Enterprise Agreement-inschrijving zijn toegewezen. De rol kunt u bepalen wie kan Azure-abonnementen maken en andere producten kopen.

### <a name="view-charges-and-credits-balance-for-your-organization"></a>Kosten in rekening gebracht en verantwoording saldo voor uw organisatie weergeven

U uw facturering-profiel gebruiken voor het bijhouden van kosten en het saldo van de Azure-tegoed voor uw organisatie die vergelijkbaar is met uw Enterprise Agreement-inschrijving.

Zie voor meer het weergeven van saldo voor een profiel voor facturering [saldo van de Azure-tegoed voor uw factureringsprofiel volgen](billing-mca-check-azure-credits-balance.md).

Zie voor meer informatie over kosten voor een profiel voor facturering bekijken, [meer informatie over de kosten op uw Microsoft-KLANTOVEREENKOMST factuur](billing-mca-understand-your-bill.md).

### <a name="view-charges-for-a-department"></a>Kosten voor een afdeling weergeven

Een gedeelte van de factuur wordt gemaakt voor elke afdeling die u in uw Enterprise Agreement hebt. U kunt kosten voor een gedeelte van de factuur bekijken in Azure portal. Zie voor meer informatie, [transacties per factuur secties bekijken](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

### <a name="view-charges-for-an-account"></a>Kosten voor een account weergeven

De accounts die zijn gemaakt in uw Enterprise Agreement-inschrijving worden niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de respectieve factuur voor hun afdeling. Eigenaars kunnen maken en beheren van abonnementen voor een sectie factuur.

Als u wilt weergeven in totale kosten voor abonnementen die deel uitmaakten van een account, moet u een kostenplaats voor elk abonnement instellen. Vervolgens kunt u het Azure-gebruik en kosten csv-bestand voor het filteren van de abonnementen van de kostenplaats.

### <a name="download-usage-and-charges-csv-price-sheet-and-tax-documents"></a>Downloaden CSV-gebruik en de kosten, prijslijst en btw-documenten

Een maandelijkse factuur wordt gegenereerd voor elke factureringsprofiel in uw factureringsrekening. U kunt voor elke factuur Azure kosten en het gebruik van csv-bestand, prijslijst en btw-document downloaden (indien van toepassing). U kunt ook downloaden Azure kosten en het gebruik van csv-bestand voor de huidige maand kosten in rekening gebracht.

Zie voor meer informatie over Azure kosten en het gebruik van csv-bestand downloaden, [gebruiksgegevens downloaden voor uw Microsoft-KLANTOVEREENKOMST](billing-download-azure-daily-usage.md#download-usage-for-your-microsoft-customer-agreement).

Zie voor informatie over het downloaden van de prijslijst, [downloaden prijzen voor uw Microsoft-KLANTOVEREENKOMST](billing-ea-pricing.md#view-and-download-pricing-for-your-microsoft-customer-agreement).

Zie voor informatie over het downloaden van de btw-documenten, [de btw-documenten weergeven voor uw Microsoft-KLANTOVEREENKOMST](billing-mca-download-tax-document.md#view-and-download-tax-documents).

### <a name="add-an-additional-enterprise-administrator"></a>Een extra enterprise-beheerder toevoegen

Gebruikerstoegang geven tot de factureringsprofiel om gebruikers te laten weergeven en beheren de facturering voor uw organisatie. U kunt de **Access Control (IAM)** pagina in de Azure portal om toegang te geven.  Zie voor meer informatie over het profiel rollen facturering, [facturering profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

Zie voor meer informatie over bieden, toegang tot uw factureringsprofiel, [facturering rollen in Azure portal beheren](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-department"></a>Maak een nieuwe afdeling

Maak een factuur sectie organiseren van uw kosten op basis van uw behoeften, zoals afdelingen in uw Enterprise Agreement-inschrijving. U kunt een nieuwe sectie van de factuur maken in Azure portal. Zie voor meer informatie, [secties maken op uw factuur voor het ordenen van uw kosten](billing-mca-section-invoice.md).

### <a name="create-a-new-account"></a>Een nieuw account maken

De rol van de maker van Azure-abonnement op factuur secties zodat ze de machtiging voor het maken van Azure-abonnement, zoals de accounts die zijn gemaakt in de Enterprise Agreement-inschrijving voor gebruikers toewijzen. Zie voor meer informatie, [Geef anderen toestemming voor het maken van Azure-abonnementen](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions).

## <a name="changes-for-department-administrators-in-the-new-billing-account"></a>Wijzigingen voor afdeling beheerders in de nieuwe factureringsaccount

De volgende wijzigingen zijn van toepassing op afdeling beheerders op een Enterprise-overeenkomst die aan de KLANTOVEREENKOMST van een Microsoft is vernieuwd.

- Een gedeelte van de factuur wordt voor elke afdeling in uw Enterprise Agreement-inschrijving gemaakt. U gebruikt de factuur sectie (s) voor het beheren van uw afdeling(en). Zie voor meer informatie over de secties van de factuur, [begrijpen factuur secties](billing-mca-overview.md#understand-invoice-sections).
- U gebruikt u de rol van de maker van Azure-abonnement aan het gedeelte factuur anderen om toestemming te geven om te maken van Azure-abonnement, zoals de accounts die zijn gemaakt in de Enterprise Agreement-inschrijving.
- U gebruikt de Azure-portal voor het beheren van facturering voor uw organisatie, in plaats van de Azure EA-portal.

Op de nieuwe factureringsaccount krijgt u de volgende rollen:

**De eigenaar van de factuur-sectie** -u de rol van eigenaar sectie factuur op de factuur-sectie die wordt gemaakt voor de afdeling(en) u in de Enterprise Agreement moest zijn toegewezen. De rol kunt u bekijken en kosten bijhouden en beheren die u kunt Azure-abonnementen maken en andere producten voor het gedeelte factuur kopen.

### <a name="view-charges-for-your-department"></a>Kosten voor uw afdeling weergeven

U kunt kosten in rekening gebracht voor de factuur-sectie die wordt gemaakt voor uw afdeling, in de Azure portal bekijken [kostenbeheer en facturering pagina](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).

### <a name="add-an-additional-department-administrator"></a>De beheerder van een extra afdeling toevoegen

Een gedeelte van de factuur wordt gemaakt voor elke afdeling die u in uw Enterprise Agreement hebt. U kunt de **toegang Control(IAM)** pagina in de Azure portal zodat anderen toegang weergeven en beheren van de factuur-sectie. Zie voor meer informatie over factuur sectie rollen, [factuur sectie rollen en taken](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

Zie voor meer informatie over bieden, toegang tot het gedeelte van uw factuur, [facturering rollen in Azure portal beheren](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-account-in-your-department"></a>Een nieuw account maken in uw afdeling

Gebruikers van de rol van de maker van Azure-abonnement op factuur sectie die wordt gemaakt voor uw afdeling toewijzen. Zie voor meer informatie, [Geef anderen toestemming voor het maken van Azure-abonnementen](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions).

### <a name="view-charges-for-accounts-in-your-departments"></a>Kosten voor accounts in uw afdelingen weergeven

De accounts die zijn gemaakt in uw Enterprise Agreement-inschrijving worden niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de respectieve factuur voor hun afdeling. Eigenaars kunnen maken en beheren van abonnementen voor een sectie factuur.

Als u wilt weergeven in totale kosten voor abonnementen die deel uitmaakten van een account in uw afdeling, moet u een kostenplaats voor elk abonnement instellen. Vervolgens kunt u het Azure-gebruik en de kosten-bestand voor het filteren van de abonnementen van de kostenplaats.

## <a name="changes-for-account-owners-in-the-new-billing-account"></a>Wijzigingen voor eigenaars in het nieuwe account voor facturering

Eigenaars van de Enterprise Agreement get-machtiging voor het maken van Azure-abonnementen op het nieuwe account voor facturering. Het gedeelte factuur die is gemaakt voor uw afdeling deel uitmaken van uw bestaande Azure-abonnementen. Als uw account niet tot een afdeling behoort, worden uw abonnementen behoren tot een factuur sectie met de naam standaardsectie factuur.

Voor het maken van aanvullende Azure-abonnementen, krijgt u de volgende rol op het nieuwe account voor facturering.

**De maker van de Azure-abonnement** -u de rol van de maker van azure-abonnement op de factuur-sectie die wordt gemaakt voor uw afdeling in een Enterprise Agreement zijn toegewezen. Als uw account niet tot een afdeling behoort, krijgt u Azure-abonnement Maker-rol op een sectie met de naam standaardsectie factuur. De rol kunt u Azure-abonnementen voor het gedeelte factuur maken.

### <a name="create-an-azure-subscription"></a>Maak een Azure-abonnement

U kunt Azure-abonnementen maken voor uw factuur in Azure portal. Zie voor meer informatie, [een extra Azure-abonnement voor Microsoft-KLANTOVEREENKOMST maken](billing-mca-create-subscription.md)

### <a name="view-charges-for-your-account"></a>Kosten voor uw account weergeven

Als u wilt kosten voor abonnementen die deel uitmaakten van een account bekijken, gaat u naar de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal. De pagina abonnementen worden kosten in rekening gebracht voor alle uw abonnement.

### <a name="view-charges-for-a-subscription"></a>Kosten voor een abonnement weergeven

U kunt ofwel kosten in rekening gebracht voor een abonnement weergeven op de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) of de Azure analysis kosten. Zie voor meer informatie over Azure kostenanalyse [verkennen en analyseren van kosten met Cost analysis](../cost-management/quick-acm-cost-analysis.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over factureringsaccount voor een Microsoft-KLANTOVEREENKOMST](billing-mca-overview.md)
- [Uw factuur begrijpen](billing-understand-your-bill.md)
- [Meer informatie over uw factuur](billing-understand-your-invoice.md)
- [Ophalen van eigendom van Azure-abonnementen van andere gebruikers](billing-mca-request-billing-ownership.md)
