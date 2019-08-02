---
title: Meer informatie over uw Azure-factuur
description: Meer informatie over het gebruik en de factuur voor uw Azure-abonnement.
author: bandersmsft
manager: dougeby
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: banders
ms.openlocfilehash: 51143644a62a77a61c4540d9f2ad3dce401c496b
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68610134"
---
# <a name="understand-your-microsoft-azure-bill"></a>Meer informatie over uw Microsoft Azure factuur
Om inzicht te krijgen in uw Azure-factuur, vergelijkt u uw factuur met het gedetailleerde dagelijks gebruiks bestand en met rapporten voor kosten beheer in de Azure Portal.

Dit artikel is niet van toepassing op de volgende klanten:
- Azure-klanten met een Enterprise Agreement (EA-klanten). Als u een EA-klant bent, raadpleegt u [uw factuur voor Azure-klanten begrijpen met een Enterprise Agreement](billing-understand-your-bill-ea.md).
- Azure-klanten met een [micro soft-klant overeenkomst](#check-access-to-a-microsoft-customer-agreement). Als u een micro soft-klant overeenkomst hebt, raadpleegt u [de Azure-kosten op uw factuur voor micro soft-klanten overeenkomst begrijpen](billing-mca-understand-your-bill.md).

Zie [overzicht van Azure CSP-facturering](/azure/cloud-solution-provider/billing/azure-csp-billing-overview/)voor een uitleg over de werking van facturering in het Azure Cloud Solution Provider-programma (Azure CSP), met inbegrip van de facturerings cyclus, prijzen en het gebruik.

## <a name="charges"></a>Uw kosten controleren

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Als er kosten op uw factuur staan waarover u meer informatie wilt, kunt u het gebruik en de kosten vergelijken met het gebruiks bestand of met de Azure Portal.

### <a name="option-1-compare-usage-and-costs-with-usage-file"></a>Optie 1: Gebruik en kosten vergelijken met gebruiks bestand

In het uitgebreide gebruik van het CSV-bestand worden uw kosten weer gegeven per facturerings periode en dagelijks gebruik. Zie [uw Azure-factuur en dagelijks gebruiks gegevens ophalen](billing-download-azure-invoice-daily-usage-date.md)om het bestand te downloaden of weer te geven.

Uw gebruiks kosten worden weer gegeven op het niveau van de meter. De volgende termen betekenen hetzelfde in zowel de factuur als het gedetailleerde gebruiks bestand. De facturerings cyclus op de factuur is bijvoorbeeld hetzelfde als de facturerings periode die wordt weer gegeven in het gedetailleerde gebruiks bestand.

 | Factuur (PDF) | Gedetailleerd gebruik (CSV)|
 | --- | --- |
|Factureringscyclus | Factureringsperiode |
 |Name |Metercategorie |
 |type |Subcategorie van de meter |
 |Resource |Meternaam |
 |Regio |Gebruiksregio |
 |Gebruikt |Verbruikte hoeveelheid |
 |Inbegrepen |Opgenomen hoeveelheid |
 |Factureerbaar |Hoeveelheid overschrijding |

In het gedeelte **gebruiks kosten** van uw factuur ziet u de totale waarde voor elke meter die tijdens uw facturerings periode is verbruikt. De volgende afbeelding toont bijvoorbeeld een gebruiks kosten voor de Azure Scheduler-service.

![Kosten voor factuur gebruik](./media/billing-understand-your-bill/1.png)

In het gedeelte **overzicht** van uw gedetailleerde gebruik CSV worden dezelfde kosten weer gegeven. Het *verbruikte* bedrag en de *waarde* komen overeen met de factuur.

![CSV-gebruiks kosten](./media/billing-understand-your-bill/2.png)

Ga naar de sectie **dagelijks gebruik** van het CSV-bestand om een dagelijkse uitsplitsing van de kosten weer te geven. Filter voor *scheduler* onder *meter categorie*. U kunt zien op welke dagen de meter is gebruikt en hoeveel er is verbruikt. De *resource* -en *resource groeps* gegevens worden ook weer gegeven ter vergelijking. De *verbruikte* waarden moeten worden toegevoegd aan en overeenkomen met wat er op de factuur wordt weer gegeven.

![De sectie dagelijks gebruik in het CSV-bestand](./media/billing-understand-your-bill/3.png)

Als u de kosten per dag wilt berekenen, vermenigvuldigt u de verbruikte bedragen met de waarde voor het *aantal* uit de sectie **overzicht** .

Voor meer informatie zie:

- [Meer informatie over uw Azure-factuur](billing-understand-your-invoice.md)
- [Meer informatie over uw uitgebreide Azure-gebruik](billing-understand-your-invoice.md)

### <a name="option-2-compare-the-usage-and-costs-in-the-azure-portal"></a>Optie 2: Het gebruik en de kosten in de Azure Portal vergelijken

U kunt de Azure Portal ook gebruiken om uw kosten te controleren. Als u een beknopt overzicht wilt krijgen van het gebruik en de kosten voor gefactureerd, bekijkt u de kosten beheer grafieken.

1. Ga in het Azure Portal naar [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer uw abonnement > **kosten analyse**.
1. Filteren op **time span**.
1. Als u wilt door gaan met het vorige voor beeld, ziet u een gebruiks kosten voor de Azure Scheduler-service.

   ![Weer gave kosten analyse in Azure Portal](./media/billing-understand-your-bill/4.png)

1. Selecteer de rij met de kosten om de dagelijkse kosten analyse weer te geven.

   ![De weer gave kosten geschiedenis in Azure Portal](./media/billing-understand-your-bill/5.png)

Zie [onverwachte kosten voor komen met Azure billing and cost management](billing-getting-started.md#costs)voor meer informatie.

## <a name="external"></a>Externe services worden afzonderlijk gefactureerd

Externe services of Marketplace-kosten zijn voor resources die zijn gemaakt door software leveranciers van derden. Deze resources zijn beschikbaar voor gebruik vanuit Azure Marketplace. Een Barracuda-firewall is bijvoorbeeld een Azure Marketplace-resource die wordt aangeboden door een derde partij. Alle kosten voor de firewall en de bijbehorende meters worden als externe service kosten weer gegeven.

Kosten voor externe services worden afzonderlijk in rekening gebracht. De kosten worden niet weer gegeven op uw Azure-factuur. Zie [inzicht krijgen in de kosten van uw Azure externe service](billing-understand-your-azure-marketplace-charges.md)voor meer informatie.

## <a name="resources-billed-by-usage-meters"></a>Resources die worden gefactureerd met gebruik meters

Azure factureert niet rechtstreeks op basis van de resource kosten. Kosten voor een resource worden berekend met behulp van een of meer meters. Meters worden gebruikt om het gebruik van een resource gedurende de levens duur te volgen. Deze meters worden vervolgens gebruikt om de factuur te berekenen.

Wanneer u bijvoorbeeld één Azure-resource maakt, zoals een virtuele machine, worden er een of meer meter exemplaren gemaakt. Meters worden gebruikt om het gebruik van de resource gedurende een periode bij te houden. Elke meting verzendt gebruiks records die door Azure worden gebruikt om de factuur te berekenen.

Eén virtuele machine (VM) die in Azure is gemaakt, kan bijvoorbeeld de volgende meters hebben om het gebruik te volgen:

- Rekenuren
- Uren IP-adres
- Inkomende gegevensoverdracht
- Uitgaande gegevensoverdracht
- Standard Managed Disk
- Standard - Beheerde schijfbewerkingen
- Standaard-IO-schijf
- Standaard-IO-blok-BLOB lezen
- Standaard-IO-schrijven blok-BLOB
- Standaard-IO-blok-BLOB verwijderen

Wanneer de virtuele machine wordt gemaakt, worden met elke meter gebruiks records gegenereerd. Dit gebruik en de prijs van de meter worden bijgehouden in het Azure-meet systeem.

## <a name="payment"></a>Betaal uw factuur

Als u een credit card hebt ingesteld als uw betalings wijze, wordt de betaling automatisch binnen 10 dagen na het einde van de facturerings periode in rekening gebracht. Op uw creditcard overzicht zou het regel item het meest **MSFT Azure**.

Zie [een credit card voor Azure toevoegen, bijwerken of verwijderen](billing-how-to-change-credit-card.md)als u de kosten van de Credit Card wilt wijzigen.

Als u [betaalt per factuur](billing-how-to-pay-by-invoice.md), stuurt u uw betaling naar de locatie die onder aan uw factuur wordt weer gegeven.

Als u de status van uw betaling wilt controleren, [maakt u een ondersteunings ticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="tips-for-cost-management"></a>Tips voor Cost Management

- Kosten ramen met behulp van:
  - [Azure-prijs calculator](https://azure.microsoft.com/pricing/calculator/)
  - [Totale eigendoms kosten calculator](https://aka.ms/azure-tco-calculator)
  - [Gedetailleerde prijs informatie voor elke service](https://azure.microsoft.com/pricing/)
- [Controleer uw gebruik en de kosten regel matig in de Azure Portal](billing-getting-started.md#costs)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een micro soft-klant overeenkomst controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, kunt u [een ondersteunings aanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="learn-more"></a>Meer informatie

- [Uw Azure-factuur en dagelijks gebruiks gegevens ophalen](billing-download-azure-invoice-daily-usage-date.md)
- [De voor waarden op uw Microsoft Azure factuur begrijpen](billing-understand-your-invoice.md)
- [Meer informatie over uw gedetailleerde gebruik van Microsoft Azure](billing-understand-your-usage.md)
- [Kosten beheer voor Azure Portal](https://docs.microsoft.com/azure/billing/billing-getting-started)
- [Onverwachte kosten voor komen met facturering en kosten beheer van Azure](billing-getting-started.md#costs)
