---
title: Inzicht in uw factuur voor Microsoft-KLANTOVEREENKOMST | Microsoft Docs
description: Meer informatie over het lezen en meer informatie over uw factuur MCA
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: e5dc8ac0716e194dd0949e2e270346ee338a792a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57839726"
---
# <a name="understand-terms-on-your-microsoft-customer-agreement-invoice"></a>Meer informatie over uw factuur KLANTOVEREENKOMST van Microsoft

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

Uw factuur bevat een samenvatting van uw kosten en de instructies voor de betaling. Het is gedownload in de Portable Document Format (PDF) van de [Azure-portal](https://portal.azure.com/) of kunnen worden verzonden via e-mail. Zie voor meer informatie, [weergeven en downloaden van uw Microsoft Azure-factuur](billing-download-azure-invoice.md).

<!-- ## When am I billed?

You are invoiced on a monthly basis. You can find out which day of the month you receive invoices by checking *invoice date* under billing profile properties in the [Azure portal](https://portal.azure.com/). Charges that occur between the end of the billing period and the invoice date are included in the next month's invoice, since they are in the next billing period. The billing period start and end dates for each invoice are listed in the invoice PDF above **Billing Summary**. -->

## <a name="invoice-terms-and-descriptions"></a>Factuur-voorwaarden en beschrijvingen

De volgende secties worden belangrijke termen die u op uw factuur en beschrijvingen voor elke term ziet.

### <a name="invoice-summary"></a>Factuuroverzicht

De **factuuroverzicht** is boven aan de eerste pagina en bevat informatie over uw facturering profiel en hoe u betaalt.

![Samenvatting van de factuur](./media/billing-understand-your-invoice-mca/invoicesummary.png)

| Termijn | Description |
| --- | --- |
| Verkocht aan |Adres van uw rechtspersoon, gevonden in de facturering van de eigenschappen van account|
| Factureren aan |Facturering-adres van de factureringsprofiel ontvangen van de factuur, gevonden in de facturering van profieleigenschappen|
| Factureringsprofiel |De naam van de facturering profiel dat u de factuur ontvangt |
| POSTBUS getal |Een optionele aankoop volgordenummer door u is toegewezen voor bijhouden |
| Factuurnummer |Een unieke, Microsoft gegenereerde factuurnummer gebruikt voor het bij te houden |
| Factuurdatum |Datum waarop de factuur is gegenereerd, doorgaans 5 tot en met 12 dagen na het einde van de factureringscyclus. U kunt uw factuurdatum in facturering profieleigenschappen controleren.|
| Betalingsvoorwaarden |Hoe betaalt u voor uw Microsoft-factuur. *30 dagen NET* betekent dat u betaalt door het controleren of overschrijving binnen 30 dagen na de factuurdatum. |

### <a name="billing-summary"></a>Factureringsoverzicht van

De **facturering samenvatting** ziet u de kosten in rekening gebracht tegen de factureringsprofiel sinds de vorige factureringsperiode, eventuele tegoeden die zijn toegepast, belasting en de totale hoeveelheid vervaldatum.

![Samenvatting van facturering](./media/billing-understand-your-invoice-mca/billingsummary.png)

| Termijn | Description |
| --- | --- |
| Kosten|Totaal aantal kosten van Microsoft voor dit profiel facturering sinds de laatste factureringsperiode |
| Credits |Tegoeden die u hebt ontvangen van retourneert |
| Azure-tegoed toegepast | Azure-tegoed die automatisch worden toegepast op Azure-kosten worden elke factureringsperiode |
| Subtotaal |Het bedrag vóór belasting |
| Btw |Het type en de hoeveelheid van het tarief dat u, afhankelijk van het land van uw facturering-profiel betaalt. Als u geen btw betalen, weergegeven niet u belasting op uw factuur. |
| Geschatte totale besparingen |De geschatte totale hoeveelheid die u hebt opgeslagen in de effectieve kortingen. Indien van toepassing, worden effectieve kortingstarieven weergegeven onder de regelitems aankoop in Details gedeelte factuur. |

### <a name="invoice-sections"></a>Factuursecties

Voor elke sectie factuur onder uw factureringsprofiel ziet u de kosten, de hoeveelheid tegoed voor Azure toegepast, belasting en het totale verschuldigde bedrag.

`Total = Charges - Azure Credit + Tax`

### <a name="details-by-invoice-section"></a>Details voor factuur-sectie

De details van weergeven de kosten voor elke sectie factuur is onderverdeeld op basis van product volgorde. Binnen elke bestelling product kosten uitgesplitst, door het type van de service. U kunt kosten voor het dagelijkse vinden voor uw producten en services in Azure portal en Azure-gebruik en kosten voor CSV. Voor meer informatie over meer Zie [meer informatie over de kosten op uw factuur voor een Microsoft-KLANTOVEREENKOMST](billing-mca-understand-your-bill.md).

Het totale verschuldigde bedrag voor elke service-serie wordt berekend door af te trekken *Azure-tegoed* van *tegoed/kosten* en toe te voegen *belasting*:

<!-- `Total = Charges/Credits - Azure Credit + Tax` -->

![Details voor factuur-sectie](./media/billing-understand-your-invoice-mca/invoicesectiondetails.png)

| Termijn |Description |
| --- | --- |
| Eenheidsprijs | De effectieve prijs per van de service (in voor de valuta) die wordt gebruikt voor het tarief voor het gebruik. Dit is uniek voor een product, service-familie, meter en aanbieding. |
| Defect | Aantal hebt aangeschaft, of tijdens de factureringsperiode verbruikt |
| Kosten/tegoed | NET hoeveelheid kosten in rekening gebracht nadat tegoed/restituties zijn toegepast |
| Azure-tegoed | De hoeveelheid tegoed voor Azure toegepast op de kosten/tegoeden|
| BTW-tarief | Belastingtarief of-tarieven, afhankelijk van het land/regio |
| Bedrag | Bedrag van de btw toegepast om te kopen op basis van de btw-tarief |
| Totaal | Het totale verschuldigde bedrag voor de aankoop |

### <a name="how-to-pay"></a>Betaling

Aan de onderkant van de factuur zijn er instructies voor het betalen van uw factuur. U kunt betalen door het controleren van, kabel, of online. Als u online betaalt, kunt u een creditcard of Azure-tegoed, indien van toepassing.

### <a name="publisher-information"></a>Informatie over de uitgever

Als u services van derden hebt op uw factuur, wordt de naam en het adres van elke uitgever aan de onderkant van uw factuur weergegeven.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over de kosten op uw profiel facturering factuur](billing-mca-understand-your-bill.md)
- [Uw Azure-factuur en de dagelijkse gebruiksgegevens ophalen](billing-download-azure-invoice-daily-usage-date.md)
- [Bekijk de prijzen van Azure van uw organisatie](billing-ea-pricing.md)
- [Documenten in de belasting voor uw facturering profiel weergeven](billing-mca-download-tax-document.md)
