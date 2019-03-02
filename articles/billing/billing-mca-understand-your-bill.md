---
title: Meer informatie over de kosten op uw Microsoft-KLANTOVEREENKOMST factuur - Azure | Microsoft Docs
description: Meer informatie over het lezen en de kosten op uw factuur begrijpen
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 3c5f5778d68b8df4d4b3132ff96bedb9d5da24ee
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249007"
---
# <a name="understand-the-charges-on-your-microsoft-customer-agreements-invoice"></a>Meer informatie over de kosten op uw Microsoft-KLANTOVEREENKOMST factuur

Door het analyseren van de afzonderlijke transacties op de factuur, kunt u de kosten afstemmen op uw factuur.

In de facturering-account voor een Microsoft-KLANTOVEREENKOMST, wordt een factuur per maand voor elke factureringsprofiel gegenereerd. De factuur bevat alle kosten van de vorige maand. U kunt uw facturen weergeven in Azure portal. Zie voor meer informatie, [weergeven en downloaden van uw Microsoft Azure-factuur](billing-download-azure-invoice.md).

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Transacties weergeven voor een factuur in Azure portal

1. Meld u aan bij [Azure Portal](https://www.azure.com).

2. Zoeken op **kosten Management en facturering**.

    ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Selecteer **alle transacties** vanaf de linkerkant van het scherm. Afhankelijk van uw toegang mogelijk moet u een factureringsaccount of een profiel voor facturering selecteren, en selecteer vervolgens **alle transacties**.

4. De pagina alle transacties bevat de volgende informatie:

    ![Schermafbeelding van de lijst met gefactureerde transacties](./media/billing-understand-your-bill-mca/mca-billed-transactions-list.png)

    |Kolom  |Definitie  |
    |---------|---------|
    |Date     | De datum van transactie  |
    |Factuur-ID     | De id voor de factuur waarop de transactie in rekening is gebracht. Als u een ondersteuningsaanvraag indient,-ID delen met de ondersteuning van Azure voor het versnellen van uw ondersteuningsaanvraag |
    |Transactietype     |  Het type transactie, zoals aankoop, annuleren, en het gebruik kosten  |
    |Productfamilie     | De categorie van het product berekenen voor virtuele machines of database voor Azure SQL database|
    |Product-sku     | Een unieke code identificeren van het exemplaar van uw product |
    |Bedrag     |  Het bedrag van transactie      |
    |Factuursectie     | De transactie wordt weergegeven in deze sectie van de facturering van het profiel factuur |
    |Factureringsprofiel     | De transactie wordt weergegeven in dit profiel facturering factuur |

5. Zoek op de factuur-ID voor het filteren van de transacties voor de factuur.

### <a name="view-transactions-by-invoice-sections"></a>Transacties per factuur secties weergeven

Factuur secties kunnen u de kosten op een factureringsprofiel factuur organiseren. Zie voor meer informatie, [gedeelte van de factuur begrijpen](billing-mca-overview.md#understand-invoice-sections). Wanneer een factuur wordt gegenereerd, weergegeven kosten in rekening gebracht voor alle secties in het profiel van de facturering op de factuur.

De volgende afbeelding ziet u de kosten voor IT-afdeling factuur sectie op de factuur voor een voorbeeld.

![Voorbeeldafbeelding van de details van de door de sectie factuurgegevens](./media/billing-understand-your-bill-mca/invoicesection-details.png)

Nadat u hebt vastgesteld dat de kosten voor een gedeelte van de factuur, kunt u de transacties weergeven in Azure portal om af te stemmen, de kosten in rekening gebracht.

1. Ga naar de pagina alle transacties in de Azure-portal transacties voor een factuur wilt weergeven. Zie voor meer informatie, [transacties voor een factuur weergeven in Azure portal](#view-transactions-for-an-invoice-in-the-azure-portal).

2. Filteren op naam van de factuur sectie transacties voor het gedeelte factuur bekijken.

## <a name="understand-pending-charges-to-estimate-your-next-invoice"></a>In behandeling zijnde kosten in rekening gebracht om in te schatten van de volgende factuur begrijpen

In de facturering-account voor een Microsoft-KLANTOVEREENKOMST, totdat de kosten worden gefactureerd, zijn schatting en beschouwd als in behandeling. Ze zijn voltooid en beschouwd als gefactureerd wanneer ze worden weergegeven op de factuur. U kunt in behandeling zijnde kosten in rekening gebracht voor een profiel voor facturering om een schatting van de kosten op uw volgende factuur te bekijken.

### <a name="view-charges-summary"></a>Overzicht van de kosten weergeven

1. Meld u aan bij [Azure Portal](https://www.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Selecteer een profiel voor facturering. Mogelijk moet u, afhankelijk van uw toegang facturering-account selecteren. Selecteer in het factureringsaccount **facturering profielen** selecteert u een profiel voor facturering.

4. Selecteer **samenvatting** tabblad vanaf de bovenkant van het scherm.

5. De sectie kosten maand tot heden en kosten afgelopen maand weergegeven.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-understand-your-bill-mca/mca-billing-profile-summary.png)

De kosten maand tot heden zijn de kosten van in behandeling voor de huidige maand en worden in rekening gebracht wanneer de factuur is gegenereerd voor de maand. Als de factuur voor de afgelopen maand nog steeds niet worden gegenereerd, wordt de kosten afgelopen maand ook zijn in behandeling.

### <a name="view-pending-transactions"></a>In behandeling transacties weergeven

Wanneer u geïdentificeerd in behandeling zijnde kosten in rekening gebracht, kunt u afzonderlijke transacties die aan de kosten in rekening gebracht op de pagina met alle transacties bijdragen weergeven. Op dit moment in behandeling zijnde gebruik worden kosten niet weergegeven op de pagina alle transactie. U kunt de in behandeling gebruikskosten weergeven op de pagina Azure-abonnementen. Zie voor meer informatie, [in de wachtrij kosten voor het gebruik weergeven](#view-pending-usage-charges)

1. Meld u aan bij [Azure Portal](https://www.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Selecteer een profiel voor facturering. Mogelijk moet u, afhankelijk van uw toegang facturering-account selecteren. Selecteer in het factureringsaccount **facturering profielen** selecteert u een profiel voor facturering.

4. Selecteer **alle transacties** vanaf de linkerkant van het scherm.

5. Zoeken naar **in behandeling**. Gebruik de **Timespan** filter om weer te geven in behandeling zijnde kosten in rekening gebracht voor de huidige of afgelopen maand.

   ![Schermafbeelding van de lijst met lopende transacties](./media/billing-understand-your-bill-mca/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Kosten voor het gebruik van in behandeling weergeven

1. Meld u aan bij [Azure Portal](https://www.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Selecteer een profiel voor facturering. Mogelijk moet u, afhankelijk van uw toegang facturering-account selecteren. Selecteer in het factureringsaccount **facturering profielen** selecteert u een profiel voor facturering.

4. Selecteer **alle abonnementen** vanaf de linkerkant van het scherm.

5. De pagina Azure-abonnementen weergegeven huidige en vorige maand kosten in rekening gebracht voor elk abonnement in het profiel voor facturering. De kosten maand tot heden zijn de kosten van in behandeling voor de huidige maand en worden in rekening gebracht wanneer de factuur is gegenereerd voor de maand. Als de factuur voor de afgelopen maand nog steeds niet worden gegenereerd, wordt de kosten afgelopen maand ook zijn in behandeling.

    ![Schermafbeelding van de lijst met Azure-abonnementen voor factureringsprofiel](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list.png)

U kunt de gedetailleerde gebruikskosten met het Azure-gebruik en de kosten-bestand kunt weergeven. Zie de volgende sectie voor meer informatie.

## <a name="analyze-your-azure-usage-charges"></a>Analyseren van uw Azure-gebruikskosten

De Azure-gebruik en kosten csv-bestand gebruiken voor het analyseren van uw gebruikskosten op basis van. U kunt het bestand voor een factuur of voor downloaden in behandeling zijnde kosten in rekening gebracht. Zie voor meer informatie, [ophalen van uw Azure-factuur en de dagelijkse gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md).

### <a name="view-detailed-usage-by-invoice-section"></a>Gedetailleerd gebruik door gedeelte factuur weergeven

U kunt het Azure bestand voor gebruik en de kosten voor het afstemmen van de kosten voor het gebruik van uw factuur secties filteren.

De volgende stappen helpen u bij het afstemmen van rekenkosten in rekening gebracht voor het gedeelte van de afdeling boekhouding factuur:

![Voorbeeldafbeelding van de details van de door de sectie factuurgegevens](./media/billing-understand-your-bill-mca/invoicesection-details.png)

 | Factuur PDF | Azure-gebruik en kosten CSV |
 | --- | --- |
 |IT-afdeling |invoiceSectionName |
 |Gebruikskosten - Azure Standard |productOrderName |
 |Opslag |serviceFamily |

1. Filter de **invoiceSectionName** kolom in het CSV-bestand naar **Accounting schuld**.
2. Filter de **productOrderName** kolom in het CSV-bestand naar **Microsoft Azure-abonnement**.
3. Filter de **serviceFamily** kolom in het CSV-bestand naar **Microsoft.Compute**.

![Schermafbeelding die toont het gebruik en kosten van bestand gefilterd op factuur sectie](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-invoice-section.png)

<!--Todo Add screenshot of usage file -->

### <a name="view-detailed-usage-by-subscription"></a>Gedetailleerd gebruik door abonnement weergeven

U kunt het Azure-gebruik en kosten csv-bestand voor het afstemmen van de kosten voor het gebruik van uw abonnementen filteren. Zie voor informatie over het bekijken van alle abonnementen in een profiel voor facturering [in de wachtrij gebruikskosten weergeven](#view-pending-usage-charges).

Wanneer u de kosten voor een abonnement hebt geïdentificeerd, kunt u de Azure gebruik en de kosten csv-bestand gebruiken voor het analyseren van de kosten in rekening gebracht.

De volgende schermafbeelding wordt weergegeven de lijst met abonnementen in Azure portal.

![Schermafbeelding van de lijst met Azure-abonnementen voor factureringsprofiel](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list-highlighted.png)

Filter de **subscriptionName** kolom in de Azure-gebruik en kosten CSV-bestand naar **WA_Subscription** om de kosten voor het gedetailleerde gebruik voor WA_Subscription weer te geven.

![Schermafbeelding die toont het gebruik en kosten van bestand gefilterd op abonnement](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Uw factuur betaalt

Instructies voor het betalen van uw factuur worden weergegeven aan de onderkant van de factuur. [Meer informatie over het betalen](billing-mca-understand-your-invoice.md#how-to-pay).

Als u uw factuur al hebt betaald, kunt u de status van de betaling op de pagina facturen in Azure portal controleren. 

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over uw factuur en het gedetailleerde gebruik:

- [Uw Azure-factuur en de dagelijkse gebruiksgegevens ophalen](billing-download-azure-invoice-daily-usage-date.md)
- [Meer informatie over uw factuur KLANTOVEREENKOMST van Microsoft](billing-mca-understand-your-invoice.md)
- [Meer informatie over het gebruik van uw Microsoft-KLANTOVEREENKOMST CSV](billing-mca-understand-your-usage.md)
