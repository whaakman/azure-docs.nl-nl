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
ms.openlocfilehash: 5980b8b701695d002d58b6b5239e6fa04c02fb2f
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57248881"
---
# <a name="complete-enterprise-agreement-tasks-in-your-billing-account-for-a-microsoft-customer-agreement"></a>Enterprise Agreement-taken uitvoeren in uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST

Als uw organisatie heeft een Microsoft-KLANTOVEREENKOMST vernieuwen van uw Enterprise Agreement-inschrijving is ondertekend, moet u de nieuwe factureringsaccount gemaakt voor de overeenkomst instellen. Uw nieuwe factureringsrekening biedt dat u uitgebreide mogelijkheden voor facturering en de kosten door middel van een nieuwe gestroomlijnde, gebundeld beheer-ervaring. Zie voor meer informatie, [uw factureringsrekening instellen voor een Microsoft-KLANTOVEREENKOMST](billing-mca-setup-account.md).

Zodra de installatie is voltooid, gebruikt u in dit artikel om te begrijpen hoe u taken uitvoeren die u in uw Enterprise Agreement-inschrijving hebt uitgevoerd.

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-your-access-to-a-microsoft-customer-agreement).

## <a name="mapping-of-enterprise-agreements-billing-to-the-new-billing-account"></a>Toewijzing van de facturering van de Enterprise-overeenkomst aan de nieuwe factureringsrekening

De facturering in uw nieuwe account is anders dan uw Enterprise Agreement-inschrijving ingedeeld. De volgende tabel beschrijft de toewijzing van de facturering van uw Enterprise Agreement-inschrijving naar uw nieuwe account voor facturering. Zie voor meer informatie over uw nieuwe factureringsrekening [aan de slag met uw factureringsrekening voor Microsoft-KLANTOVEREENKOMST](billing-mca-overview.md).

| Enterprise Agreement   | Microsoft-klantovereenkomst    |
|------------------------|--------------------------------------------------------|
| Registratie            | U kunt een facturering profiel gebruikt voor het beheren van facturering voor uw organisatie, die vergelijkbaar is met uw Enterprise Agreement-inschrijving. Enterprise-beheerders kunnen worden eigenaren van het profiel voor facturering. Zie voor meer informatie over facturering-profielen, [inzicht in facturering profielen](billing-mca-overview.md#understand-billing-profiles).
| Afdeling            | Een gedeelte van de factuur kunt u uw kosten weer te geven, die vergelijkbaar is met afdelingen in uw Enterprise Agreement-inschrijving organiseren. Afdeling wordt factuur secties en beheerders van de afdeling worden eigenaars van de secties met respectieve factuur. Zie voor meer informatie over de secties van de factuur, [begrijpen factuur secties](billing-mca-overview.md#understand-invoice-sections). |
| Account               | De accounts die zijn gemaakt in uw Enterprise Agreement worden niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de respectieve factuur voor hun afdeling. Eigenaars kunnen maken en beheren van abonnementen voor een sectie factuur. |

## <a name="tasks-performed-by-an-enterprise-administrator"></a>Taken die worden uitgevoerd door een enterprise-beheerder

Als u een enterprise-beheerder een Enterprise-overeenkomst die aan de KLANTOVEREENKOMST van een Microsoft hebt vernieuwd, kunt u de volgende rollen op de nieuwe factureringsaccount gemaakt voor de overeenkomst zijn toegewezen:

**Facturering-profieleigenaar** -u de rol van de facturering profiel eigenaar voor de facturering profiel dat is gemaakt tijdens de overeenkomst is ondertekend zijn toegewezen. De rol kunt u de facturering voor uw organisatie beheren. U kunt kosten in rekening gebracht en facturen weergeven, organiseren kosten op de factuur, betalingswijzen beheren en toegang tot de facturering van uw organisatie beheren.

**De eigenaar van de factuur-sectie** -u kunt de rol van eigenaar sectie factuur op alle facturen secties die zijn gemaakt voor de afdelingen in uw Enterprise Agreement-inschrijving zijn toegewezen. De rol kunt u bepalen wie kan Azure-abonnementen maken en andere producten kopen.

### <a name="view-charges-and-credits-balance-for-your-organization"></a>Kosten in rekening gebracht en verantwoording saldo voor uw organisatie weergeven

U uw facturering-profiel gebruiken voor het bijhouden van kosten en het saldo van de Azure-tegoed voor uw organisatie die vergelijkbaar is met uw Enterprise Agreement-inschrijving.

Zie voor meer het weergeven van saldo voor een profiel voor facturering [bijhouden Azure saldo van uw factureringsprofiel](billing-mca-check-azure-credits-balance.md).

Zie voor meer informatie over kosten voor een profiel voor facturering bekijken, [meer informatie over de kosten op uw Microsoft-KLANTOVEREENKOMST factuur](billing-mca-understand-your-bill.md).

### <a name="view-charges-for-a-department"></a>Kosten voor een afdeling weergeven

Een gedeelte van de factuur wordt gemaakt voor elke afdeling die u in uw Enterprise Agreement hebt. U kunt kosten voor een gedeelte van de factuur bekijken in Azure portal. Zie voor meer informatie, [transacties per factuur secties bekijken](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

### <a name="view-charges-for-an-account"></a>Kosten voor een account weergeven

De accounts die zijn gemaakt in uw Enterprise Agreement-inschrijving worden niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de respectieve factuur voor hun afdeling. Eigenaars kunnen maken en beheren van abonnementen voor een sectie factuur.

Als u wilt weergeven in totale kosten voor abonnementen die deel uitmaakten van een account, moet u een kostenplaats voor elk abonnement instellen. Vervolgens kunt u het Azure-gebruik en kosten csv-bestand voor het filteren van de abonnementen van de kostenplaats.

### <a name="download-usage-and-charges-csv-price-sheet-and-tax-documents"></a>Downloaden CSV-gebruik en de kosten, prijslijst en btw-documenten

Een maandelijkse factuur wordt gegenereerd voor elke factureringsprofiel in uw factureringsrekening. U kunt voor elke factuur Azure kosten en het gebruik van csv-bestand, prijslijst en btw-document downloaden (indien van toepassing). U kunt ook downloaden Azure kosten en het gebruik van csv-bestand voor de huidige maand kosten in rekening gebracht.

Zie voor meer informatie over Azure kosten en het gebruik van csv-bestand downloaden, [downloaden of weergeven van uw Azure-factuur en de dagelijkse gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md). <!--Todo update the link -->

Voor informatie over het downloaden van prijs blad en btw-documenten, Zie [btw-documenten downloaden voor uw factuur](billing-download-azure-invoice-daily-usage-date.md). <!--Todo update the link -->

### <a name="add-an-additional-enterprise-administrator"></a>Een extra enterprise-beheerder toevoegen

Gebruik de **toegang Control(IAM)** pagina in de Azure portal zodat anderen toegang weergeven en beheren van het profiel voor facturering. Zie voor meer informatie over het profiel rollen facturering, [facturering profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

Zie voor meer informatie over bieden, toegang tot uw factureringsprofiel, [facturering rollen in Azure portal beheren](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-department"></a>Maak een nieuwe afdeling

In uw nieuwe factureringsrekening gebruikt u een gedeelte van de factuur voor het ordenen van uw kosten op basis van uw behoeften, vergelijkbaar met afdelingen in uw Enterprise Agreement-inschrijving. U kunt een nieuwe sectie van de factuur maken in de Azure portal voor het instellen van Azure voor een nieuwe afdeling. Zie voor meer informatie, [secties maken op uw factuur voor het ordenen van uw kosten](billing-mca-section-invoice.md).

### <a name="create-a-new-account"></a>Een nieuw account maken

De context van de facturering in uw Enterprise Agreement wordt niet ondersteund in het nieuwe account voor facturering. U kunt echter Azure-abonnement Maker rol bieden aan gebruikers in uw organisatie zodat ze Azure-abonnementen maken. Zie voor meer informatie, [Geef anderen toestemming voor het maken van Azure-abonnementen](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions).

## <a name="tasks-performed-by-a-department-administrator"></a>Taken die worden uitgevoerd door de beheerder van een afdeling

Als u de beheerder van een afdeling in een Enterprise-overeenkomst die aan de KLANTOVEREENKOMST van een Microsoft vernieuwd, kunt u de volgende rollen op de nieuwe factureringsaccount zijn toegewezen:

**De eigenaar van de factuur-sectie** -u de rol van eigenaar sectie factuur op de factuur-sectie die wordt gemaakt voor de afdelingen die u in de Enterprise Agreement hebt zijn toegewezen. De rol kunt u bekijken en kosten bijhouden en beheren die u kunt Azure-abonnementen maken en andere producten voor het gedeelte factuur kopen.

Als u de beheerder van een afdeling op meerdere afdelingen in de Enterprise Agreement, kunt u de rol voor de eigenaar van factuur sectie op alle facturen secties die zijn gemaakt voor de afdelingen wordt toegewezen.

### <a name="view-charges-for-your-department"></a>Kosten voor uw afdeling weergeven

Een gedeelte van de factuur wordt gemaakt voor elke afdeling die u in uw Enterprise Agreement hebt. Het gedeelte factuur heeft dezelfde naam als uw afdeling in de Enterprise Agreement. U kunt de kosten voor het gedeelte factuur weergeven in Azure portal. <!-- Todo Add a link -->

### <a name="add-an-additional-department-administrator"></a>De beheerder van een extra afdeling toevoegen

Een gedeelte van de factuur wordt gemaakt voor elke afdeling die u in uw Enterprise Agreement hebt. U kunt de **toegang Control(IAM)** pagina in de Azure portal zodat anderen toegang weergeven en beheren van de factuur-sectie. Zie voor meer informatie over factuur sectie rollen, [factuur profiel rollen en taken](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

Zie voor meer informatie over bieden, toegang tot het gedeelte van uw factuur, [facturering rollen in Azure portal beheren](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-account-in-your-department"></a>Een nieuw account maken in uw afdeling

De context van de facturering in uw Enterprise Agreement wordt niet ondersteund in het nieuwe account voor facturering. U kunt echter Azure-abonnement Maker rol bieden aan gebruikers in uw organisatie zodat ze Azure-abonnementen maken. Zie voor meer informatie, [Geef anderen toestemming voor het maken van Azure-abonnementen](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions).

### <a name="view-charges-for-accounts-in-your-departments"></a>Kosten voor accounts in uw afdelingen weergeven

De context van de facturering in uw Enterprise Agreement wordt niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de factuur voor uw afdeling gemaakt. Eigenaars kunnen maken en beheren van abonnementen voor de factuur-secties.

Als u wilt weergeven in totale kosten voor abonnementen die deel uitmaakten van een account in uw afdeling, moet u een kostenplaats voor elk abonnement instellen. Vervolgens kunt u het Azure-gebruik en de kosten-bestand voor het filteren van de abonnementen van de kostenplaats. <!-- Todo - can they go to cost analysis -->

## <a name="tasks-performed-by-an-account-owner"></a>Taken die worden uitgevoerd door de eigenaar van een account

Als u de eigenaar van een account op een Enterprise-overeenkomst die aan de KLANTOVEREENKOMST van een Microsoft hebt vernieuwd, kunt u de volgende rol op de nieuwe factureringsaccount gemaakt voor de overeenkomst zijn toegewezen:

**De maker van de Azure-abonnement** -u de rol van de maker van azure-abonnement op de factuur-sectie die wordt gemaakt voor uw afdeling in een Enterprise Agreement zijn toegewezen. Als uw account niet tot een afdeling behoort, uw abonnementen behoren tot het gedeelte van de factuur met de naam standaardsectie factuur en krijgt u de maker van de Azure-abonnement in de sectie. De rol kunt u Azure-abonnementen voor het gedeelte factuur maken.

### <a name="view-charges-for-your-account"></a>Kosten voor uw account weergeven

De context van de facturering in uw Enterprise Agreement wordt niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de factuur voor de afdeling van uw account is gemaakt.

Als u wilt kosten voor abonnementen die deel uitmaakten van een account bekijken, gaat u naar de **pagina abonnementen** in Azure portal. De pagina abonnementen worden kosten in rekening gebracht voor alle uw abonnement. <!-- Todo - can they go to cost analysis -->

### <a name="view-charges-for-a-subscription"></a>Kosten voor een abonnement weergeven

U kunt Azure kostenanalyse gebruiken om de kosten voor een abonnement weer te geven. Zie voor meer informatie, [verkennen en analyseren van kosten met Cost analysis](../cost-management/quick-acm-cost-analysis.md).

### <a name="create-an-azure-subscription"></a>Maak een Azure-abonnement

U kunt Azure-abonnementen maken voor uw factuur in Azure portal. Zie voor meer informatie, [een extra Azure-abonnement voor Microsoft-KLANTOVEREENKOMST maken](billing-mca-create-subscription.md)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over factureringsaccount voor een Microsoft-KLANTOVEREENKOMST](billing-mca-overview.md)
- [Uw factuur begrijpen](billing-understand-your-bill.md)
- [Meer informatie over uw factuur](billing-understand-your-invoice.md)
- [Ophalen van eigendom van Azure-abonnementen van andere gebruikers](billing-mca-request-billing-ownership.md)
