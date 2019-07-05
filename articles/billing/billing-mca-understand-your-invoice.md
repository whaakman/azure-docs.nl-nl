---
title: Inzicht in uw factuur KLANTOVEREENKOMST van Microsoft in Azure
description: Meer informatie over het lezen en meer informatie over uw factuur KLANTOVEREENKOMST van Microsoft in Azure
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: fed658d3f672d6116d7c2b0f3e2e9ad989dd67c6
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490644"
---
# <a name="terms-in-your-microsoft-customer-agreement-invoice"></a>Voorwaarden in uw factuur KLANTOVEREENKOMST van Microsoft

In dit artikel is van toepassing op een Azure-facturering account voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

Uw factuur bevat een samenvatting van uw kosten en de instructies voor de betaling. Het is gedownload in de Portable Document Format (PDF) van de [Azure-portal](https://portal.azure.com/) of kunnen worden verzonden via e-mail. Zie voor meer informatie, [weergeven en downloaden van uw Microsoft Azure-factuur](billing-download-azure-invoice.md).

## <a name="billing-period"></a>Factureringsperiode

U worden gefactureerd op maandbasis. U vindt hier informatie welke dag van de maand die u facturen, door het controleren van ontvangt *factuurdatum* onder facturering profieleigenschappen in de [Azure-portal](https://portal.azure.com/). Kosten die worden uitgevoerd tussen het einde van de factureringsperiode en de factuurdatum zijn opgenomen in de factuur van de volgende maand, omdat deze zich in de volgende factureringsperiode. De facturering periode begin- en einddatums voor elke factuur vindt u in de factuur PDF bovenstaande **facturering samenvatting**.

## <a name="invoice-terms-and-descriptions"></a>Factuur-voorwaarden en beschrijvingen

De volgende secties lijst met belangrijke termen die u op uw factuur ziet en bevatten beschrijvingen voor elke term.

### <a name="invoice-summary"></a>Factuuroverzicht

De **factuuroverzicht** is aan de bovenkant van de eerste pagina en bevat informatie over uw facturering profiel en hoe u betaalt.

![Samenvatting van de factuur](./media/billing-understand-your-invoice-mca/invoicesummary.png)

| Termijn | Description |
| --- | --- |
| Verkocht aan |Adres van uw rechtspersoon, gevonden in de facturering van de eigenschappen van account|
| Factureren aan |Facturering-adres van de factureringsprofiel ontvangen van de factuur, gevonden in de facturering van profieleigenschappen|
| Factureringsprofiel |De naam van de facturering profiel dat u de factuur ontvangt |
| POSTBUS getal |Een optionele aankoop volgordenummer door u is toegewezen voor bijhouden |
| Factuurnummer |Een unieke, Microsoft gegenereerde factuurnummer gebruikt voor het bij te houden |
| Factuurdatum |Datum waarop de factuur is gegenereerd, doorgaans 5 tot en met 12 dagen na het einde van de factureringscyclus. U kunt uw factuurdatum in facturering profieleigenschappen controleren.|
| Betalingsvoorwaarden |Hoe betaalt u voor uw Microsoft-factuur. *30 dagen NET* betekent dat u binnen 30 dagen na de factuurdatum betaalt. |

### <a name="billing-summary"></a>Factureringsoverzicht van

De **facturering samenvatting** ziet u de kosten in rekening gebracht tegen de factureringsprofiel sinds de vorige factureringsperiode, eventuele tegoeden die zijn toegepast, belasting en de totale hoeveelheid vervaldatum.

![Samenvatting van facturering](./media/billing-understand-your-invoice-mca/billingsummary.png)

| Termijn | Description |
| --- | --- |
| Kosten in rekening gebracht|Totaal aantal kosten van Microsoft voor dit profiel facturering sinds de laatste factureringsperiode |
| Credits |Tegoeden die u hebt ontvangen van retourneert |
| Azure-tegoed toegepast | Azure-tegoed die automatisch worden toegepast op Azure-kosten worden elke factureringsperiode |
| Subtotaal |Het bedrag vóór belasting |
| Belasting |Het type en de hoeveelheid van het tarief dat u, afhankelijk van het land/de regio van uw facturering-profiel betaalt. Als u geen btw betalen, weergegeven niet u belasting op uw factuur. |
| Geschatte totale besparingen |De geschatte totale hoeveelheid die u hebt opgeslagen in de effectieve kortingen. Indien van toepassing, worden effectieve kortingstarieven weergegeven onder de regelitems aankoop in Details gedeelte factuur. |

### <a name="invoice-sections"></a>Factuur secties

Voor elke sectie factuur onder uw factureringsprofiel ziet u de kosten, de hoeveelheid tegoed voor Azure toegepast, belasting en het totale verschuldigde bedrag.

`Total = Charges - Azure Credit + Tax`

### <a name="details-by-invoice-section"></a>Details voor factuur-sectie

De details van weergeven de kosten voor elke sectie factuur is onderverdeeld op basis van product volgorde. Binnen elke bestelling product kosten uitgesplitst, door het type van de service. U kunt kosten voor het dagelijkse vinden voor uw producten en services in Azure portal en Azure-gebruik en kosten voor CSV. Voor meer informatie over meer Zie [meer informatie over de kosten op uw factuur voor een Microsoft-KLANTOVEREENKOMST](billing-mca-understand-your-bill.md).

Het totale verschuldigde bedrag voor elke service-serie wordt berekend door af te trekken *Azure-tegoed* van *tegoed/kosten* en toe te voegen *belasting*:


![Details voor factuur-sectie](./media/billing-understand-your-invoice-mca/invoicesectiondetails.png)

| Termijn |Description |
| --- | --- |
| Prijs per eenheid | De effectieve prijs per van de service (in voor de valuta) die wordt gebruikt voor het tarief voor het gebruik. Dit is uniek voor een product, service-familie, meter en aanbieding. |
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

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over de kosten op uw profiel facturering factuur](billing-mca-understand-your-bill.md)
- [Uw Azure-factuur en de dagelijkse gebruiksgegevens ophalen](billing-download-azure-invoice-daily-usage-date.md)
- [Bekijk de prijzen van Azure van uw organisatie](billing-ea-pricing.md)
- [Documenten in de belasting voor uw facturering profiel weergeven](billing-mca-download-tax-document.md)
