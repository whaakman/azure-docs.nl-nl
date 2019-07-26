---
title: Meer informatie over uw factuur voor micro soft-klanten overeenkomst in azure
description: Meer informatie over uw factuur voor micro soft Customer overeenkomst in azure lezen en begrijpen
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/22/2019
ms.author: banders
ms.openlocfilehash: a5f77120c1d4e8a6721f3bc207132bee19a7772f
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383533"
---
# <a name="terms-in-your-microsoft-customer-agreement-invoice"></a>Voor waarden in de klant overeenkomst van micro soft

Dit artikel is van toepassing op een Azure-facturerings account voor een klant overeenkomst van micro soft. [Controleer of u toegang hebt tot een micro soft-klant overeenkomst](#check-access-to-a-microsoft-customer-agreement).

Uw factuur bevat een samen vatting van uw kosten en instructies voor de betaling. Het is beschikbaar als down load in de Portable Document Format (. PDF) van de [Azure Portal](https://portal.azure.com/) of kan via e-mail worden verzonden. Zie [uw Microsoft Azure factuur bekijken en downloaden](billing-download-azure-invoice.md)voor meer informatie.

## <a name="billing-period"></a>Factureringsperiode

U wordt maandelijks gefactureerd. U kunt zien op welke dag van de maand u facturen ontvangt door *factuur datum* te controleren onder de eigenschappen van het facturerings profiel in de [Azure Portal](https://portal.azure.com/). Kosten die plaatsvinden tussen het einde van de facturerings periode en de factuur datum, worden opgenomen in de factuur van de volgende maand, omdat deze zich in de volgende facturerings periode bevinden. De begin-en eind datum van de facturerings periode voor elke factuur worden weer gegeven in het factuur-PDF-bestand boven **facturerings overzicht**.

## <a name="invoice-terms-and-descriptions"></a>Factuur voorwaarden en-beschrijvingen

In de volgende secties vindt u belang rijke termen die u op uw factuur ziet en vindt u beschrijvingen voor elke term.

### <a name="invoice-summary"></a>Factuur overzicht

Het **factuur overzicht** bevindt zich bovenaan de eerste pagina en toont informatie over uw facturerings profiel en hoe u betaalt.

![Sectie factuur overzicht](./media/billing-understand-your-invoice-mca/invoicesummary.png)

| Begrip | Description |
| --- | --- |
| Verkocht aan |Adres van uw juridische entiteit, gevonden in de eigenschappen van het facturerings account|
| Factureren aan |Het factuur adres van het facturerings profiel dat de factuur ontvangt, te vinden in de eigenschappen van het facturerings profiel|
| Factureringsprofiel |De naam van het facturerings profiel dat de factuur ontvangt |
| KOOPORDER nummer |Een optioneel inkooporder nummer dat door u is toegewezen voor het bijhouden van |
| Factuur nummer |Een uniek, door micro soft gegenereerd factuur nummer dat wordt gebruikt voor tracking doeleinden |
| Factuurdatum |De datum waarop de factuur is gegenereerd, doorgaans vijf tot 12 dagen na het einde van de facturerings cyclus. U kunt de factuur datum controleren in de eigenschappen van het facturerings profiel.|
| Betalings voorwaarden |Hoe u betaalt voor uw micro soft-factuur. *Net 30 dagen* betekent dat u betaalt binnen 30 dagen na de factuur datum. |

### <a name="billing-summary"></a>Facturerings overzicht

In het **facturerings overzicht** worden de kosten weer gegeven voor het facturerings profiel sinds de vorige facturerings periode, eventuele tegoeden die zijn toegepast, belastingen en het totale verschuldigde bedrag.

![Sectie facturerings overzicht](./media/billing-understand-your-invoice-mca/billingsummary.png)

| Begrip | Description |
| --- | --- |
| Kosten|Het totale aantal micro soft-kosten voor dit facturerings profiel sinds de laatste facturerings periode |
| Credits |Tegoed dat u hebt ontvangen van retourneert |
| Azure-tegoed toegepast | Azure-tegoed dat automatisch wordt toegepast op Azure-kosten per facturerings periode |
| Subtotaal |Het verschuldigde bedrag vóór BTW |
| Btw |Het type en de belasting hoeveelheid die u betaalt, afhankelijk van het land of de regio van uw facturerings profiel. Als u geen BTW hoeft te betalen, ziet u geen belasting op uw factuur. |
| Geschatte totale besparingen |De geschatte totale hoeveelheid die u hebt bespaard op basis van efficiënte kortingen. Als dit van toepassing is, worden de juiste kortings tarieven vermeld onder de sectie aankoop regel items in Details per factuur. |

### <a name="invoice-sections"></a>Factuur secties

Voor elk factuur gedeelte onder uw facturerings profiel ziet u de kosten, het bedrag van de toegepaste Azure-tegoeden, de belasting en het totale verschuldigde bedrag.

`Total = Charges - Azure Credit + Tax`

### <a name="details-by-invoice-section"></a>De sectie Details per factuur

In de details worden de kosten voor elk factuur gedeelte per product order weer gegeven. Binnen elke product order worden kosten uitgesplitst op basis van het type service. U vindt de dagelijkse kosten voor uw producten en services in het Azure Portal en het gebruik van Azure en de kosten CSV. Zie [inzicht krijgen in de kosten op uw factuur voor een micro soft-klant overeenkomst voor](billing-mca-understand-your-bill.md)meer informatie.

Het totale verschuldigde bedrag voor elke service groep wordt berekend door *Azure-tegoed* af te trekken van *tegoeden/kosten* en het toevoegen van *belasting*:


![De sectie Details per factuur](./media/billing-understand-your-invoice-mca/invoicesectiondetails.png)

| Begrip |Description |
| --- | --- |
| Eenheidsprijs | De werkelijke eenheids prijs van de service (in de prijs valuta) die wordt gebruikt om het gebruik te beoordelen. Dit is uniek voor een product, een service familie, een meter en een aanbieding. |
| Hoeveelheid | Hoeveelheid die is aangeschaft of verbruikt tijdens de facturerings periode |
| Kosten/tegoeden | Het nettobedrag van kosten na tegoeden/restituties wordt toegepast |
| Azure-tegoed | De hoeveelheid Azure-tegoeden die worden toegepast op de kosten/tegoeden|
| BTW-tarief | BTW-tarief (s), afhankelijk van land/regio |
| Belasting bedrag | BTW die op aankoop wordt toegepast op basis van het BTW-tarief |
| Totaal | Het totale verschuldigde bedrag voor de aankoop |

### <a name="how-to-pay"></a>Betalen

Onder aan de factuur staan instructies voor het betalen van uw factuur. U kunt betalen per cheque, draad of online. Als u online betaalt, kunt u een credit card of Azure-tegoed gebruiken, indien van toepassing.

### <a name="publisher-information"></a>Informatie over Uitgever

Als u services van derden in uw factuur hebt, worden de naam en het adres van elke uitgever onder aan uw factuur weer gegeven.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een micro soft-klant overeenkomst controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over de kosten voor de factuur van uw facturerings profiel](billing-mca-understand-your-bill.md)
- [Uw Azure-factuur en de dagelijkse gebruiksgegevens ophalen](billing-download-azure-invoice-daily-usage-date.md)
- [De Azure-prijzen van uw organisatie weer geven](billing-ea-pricing.md)
- [BTW-documenten voor uw facturerings profiel weer geven](billing-mca-download-tax-document.md)
