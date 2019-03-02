---
title: Meer informatie over uw Azure-factuur | Microsoft Docs
description: Meer informatie over het lezen en informatie over het gebruik en de factuur voor uw Azure-abonnement
services: ''
documentationcenter: ''
author: tonguyen10
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
ms.openlocfilehash: ac36c76e8605df0fee3e39341c8be0fef7e58ddf
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57246593"
---
# <a name="understand-your-microsoft-azure-bill"></a>Meer informatie over uw factuur voor Microsoft Azure
Voor meer informatie over uw Azure-factuur, vergelijken met uw factuur met de gedetailleerde dagelijks gebruik van bestands- en -rapporten voor de kosten in de Azure-portal.

In dit artikel geldt niet voor Azure-klanten met een Enterprise Agreement (EA-klanten). Als u een EA-klant bent, Zie [meer informatie over uw factuur voor Azure-klanten met een Enterprise Agreement](billing-understand-your-bill-ea.md).

In dit artikel geldt niet voor een overeenkomst van de klant Microsoft Azure-klanten. Als u een rekening voor de KLANTOVEREENKOMST van een Microsoft hebt, raadpleegt u [inzicht in de Azure-kosten worden op uw factuur voor Microsoft-KLANTOVEREENKOMST](billing-mca-understand-your-bill.md).

Voor een uitleg van de werking van facturering in het programma Azure Cloud Solution Provider (Azure CSP), met inbegrip van de facturering cyclus, prijzen en het gebruik, Zie [facturering overzicht van Azure CSP](/azure/cloud-solution-provider/billing/azure-csp-billing-overview/).

## <a name="charges"></a>Uw kosten controleren

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Als er een kosten in rekening gebracht op uw factuur waarover u meer informatie weergeven wilt, kunt u gebruik en kosten vergelijken met het gebruiksbestand of met de Azure-portal.

### <a name="option-1-compare-usage-and-costs-with-usage-file"></a>Optie 1: Gebruik en kosten met gebruiksbestand vergelijken

Het gedetailleerde gebruik CSV-bestand bevat uw kosten per factureringsperiode en het dagelijks gebruik. Als u het bestand, raadpleegt u [ophalen van uw Azure-factuur en de dagelijkse gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md).

De gebruikskosten worden weergegeven op het niveau van de meter. De volgende termen betekenen hetzelfde te doen in de factuur en het gedetailleerde gebruik-bestand. Bijvoorbeeld, is de factureringscyclus op de factuur hetzelfde als de factureringsperiode weergegeven in het bestand gedetailleerd gebruik.

 | Factuur (PDF-bestand) | Gedetailleerd gebruik (CSV)|
 | --- | --- |
|Factureringscyclus | Factureringsperiode |
 |Name |De categorie van de meter |
 |Type |Subcategorie van de meter |
 |Resource |De naam van de meter |
 |Regio |De regio van de meter |
 |Verbruikt |Verbruikt aantal |
 |Inbegrepen |Inbegrepen hoeveelheid |
 |Factureerbaar |Overschreden hoeveelheid |

De **gebruikskosten** gedeelte van uw factuur bevat de totale waarde voor elke meter die tijdens de factureringsperiode is verbruikt. De volgende schermafbeelding ziet u bijvoorbeeld een gebruik kosten in rekening gebracht voor de Azure Scheduler-service.

![Factuur-gebruikskosten](./media/billing-understand-your-bill/1.png)

De **instructie** gedeelte van uw gedetailleerde gebruik CSV ziet u de dezelfde kosten in rekening gebracht. Zowel de *verbruikt* bedrag en *waarde* overeenkomen met de factuur.

![Kosten voor het gebruik van CSV](./media/billing-understand-your-bill/2.png)

Deze kosten per dag opgesplitst, Ga naar de **dagelijks gebruik** sectie van de CSV. Filteren op *Scheduler* onder *Metercategorie*. U kunt zien welke dagen de meter is gebruikt en hoeveel is verbruikt. De *Resource* en *resourcegroep* informatie wordt ook weergegeven voor de vergelijking. De *verbruikt* waarden moeten oplopen tot wat wordt weergegeven op de factuur.

![De sectie dagelijks gebruik in de CSV](./media/billing-understand-your-bill/3.png)

Als u de kosten per dag, vermenigvuldigt u de *verbruikt* bedragen met de *tarief* waarde uit de **instructie** sectie.

Voor meer informatie zie:

- [Inzicht in uw Azure-factuur](billing-understand-your-invoice.md)
- [Inzicht in uw gedetailleerde gebruik van Azure](billing-understand-your-invoice.md)

### <a name="option-2-compare-the-usage-and-costs-in-the-azure-portal"></a>Optie 2: Vergelijk de gebruik en kosten in de Azure-portal

De Azure-portal kunt u controleren of uw kosten in rekening gebracht. Als u een kort overzicht van uw gefactureerd gebruik en kosten in rekening gebracht, het beheer van kostengrafieken weergeven

1. In de Azure-portal, gaat u naar [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer uw abonnement > **Cost analysis**.
1. Filteren op **Timespan**.
1. Als u wilt doorgaan met het vorige voorbeeld, ziet u een gratis gebruik voor de Azure Scheduler-service.

   ![Kostenanalyse weergave in Azure portal](./media/billing-understand-your-bill/4.png)

1. Selecteer die rij om te zien van de verdeling van dagelijkse kosten weergegeven.

   ![Kostengeschiedenis weergave in Azure portal](./media/billing-understand-your-bill/5.png)

Zie voor meer informatie, [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md#costs).

## <a name="external"></a>Externe services apart in rekening gebracht

Externe services of marketplace-kosten zijn voor resources die zijn gemaakt door leveranciers van software van derden. Deze resources zijn beschikbaar voor gebruik in Azure marketplace. Een Firewall Barracuda is bijvoorbeeld een Azure marketplace-resource die worden aangeboden door een derde partij. Alle kosten voor de firewall en de bijbehorende meters weergegeven als externe servicekosten.

Externe servicekosten worden afzonderlijk gefactureerd. De kosten worden niet weergegeven op uw Azure-factuur. Zie voor meer informatie, [meer informatie over uw Azure externe servicekosten](billing-understand-your-azure-marketplace-charges.md).

## <a name="resources-billed-by-usage-meters"></a>Resources die worden gefactureerd op basis van gebruik meters

Azure biedt geen direct factuur op basis van de resourcekosten. Kosten voor een bron worden berekend met behulp van een of meer meters. Meters worden gebruikt voor het bijhouden van gebruik tijdens de levensduur van een resource. Deze meters worden vervolgens gebruikt voor het berekenen van de factuur.

Bijvoorbeeld, wanneer u een enkel Azure-resource, zoals een virtuele machine, maakt er een of meer meter-exemplaren die zijn gemaakt. Meters worden gebruikt voor het gebruik van de resource gedurende een periode bijhouden. Elke meter verzendt gebruiksrecords die door Azure worden gebruikt voor het berekenen van de factuur.

Een enkele virtuele machine (VM) gemaakt in Azure kan bijvoorbeeld de volgende meters gemaakt voor het bijhouden van het gebruik ervan:

- Rekenuren
- Uren IP-adres
- Inkomende gegevensoverdracht
- Uitgaande gegevensoverdracht
- Standard-beheerde schijven
- Standard-beheerde schijfbewerkingen
- Standaard-IO-schijf
- Standaard-IO-blok-Blob lezen
- Standaard-IO-blok-Blob schrijven
- Standaard-IO-blok-Blob-verwijdering

Wanneer de virtuele machine wordt gemaakt, begint elke meter gebruiksrecords genereren. Dit gebruik en de prijs van de meter wordt bijgehouden in de Azure meetsysteem.

## <a name="payment"></a>Uw factuur betaalt

Als u een creditcard of een betaalpas als uw betalingswijze instellen, de betaling wordt in rekening gebracht automatisch binnen 10 dagen na afloop van de factureringsperiode. Op uw creditcard worden vermeld, het regelitem Denk **MSFT Azure**.

Als u wilt wijzigen van de creditcard die in rekening gebracht, Zie [toevoegen, bijwerken of verwijderen van een creditcard of betaalpas nodig voor Azure](billing-how-to-change-credit-card.md).

Als u [betalen per factuur](billing-how-to-pay-by-invoice.md), verzenden de betaling naar de locatie aan de onderkant van uw factuur weergegeven.

Om te controleren of de status van uw betaling [een ondersteuningsticket maken](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="tips-for-cost-management"></a>Tips voor kostenbeheer

- Schat de kosten met behulp van de:
  - [Azure prijscalculator](https://azure.microsoft.com/pricing/calculator/)
  - [Totale kosten van eigendom calculator](https://aka.ms/azure-tco-calculator)
  - [Gedetailleerde prijsinformatie voor elke service](https://azure.microsoft.com/pricing/)
- [Uw gebruik en kosten regelmatig in Azure portal bekijken](billing-getting-started.md#costs).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="learn-more"></a>Meer informatie

- [Uw Azure-factuur en de dagelijkse gebruiksgegevens ophalen](billing-download-azure-invoice-daily-usage-date.md)
- [Meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-invoice.md)
- [Meer informatie over uw Microsoft Azure gedetailleerd gebruik](billing-understand-your-usage.md)
- [Kostenbeheer van Azure portal](https://docs.microsoft.com/azure/billing/billing-getting-started)
- [Voorkomen van onverwachte kosten met Azure-facturering en -kostenbeheer](billing-getting-started.md#costs)
