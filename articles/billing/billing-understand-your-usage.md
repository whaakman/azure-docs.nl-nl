---
title: Meer informatie over het gebruik en de kosten | Microsoft Docs
description: Meer informatie over hoe u uw gedetailleerde gebruik en kosten kunt lezen en begrijpen
author: bandersmsft
manager: micflan
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2019
ms.author: banders
ms.openlocfilehash: 66b54c027cde6341b23aef2c10b43fa21bf357da
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383463"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Meer informatie over de voor waarden in het Azure-gebruiks-en-kosten bestand

Het bestand gedetailleerd gebruik en toeslagen bevat dagelijks geclassificeerd gebruik op basis van de overeengekomen tarieven, aankopen (bijvoorbeeld reserve ringen, Marketplace-kosten) en terugbetalingen voor de opgegeven periode.
De kosten omvatten geen tegoed, belastingen of andere kosten of kortingen.
De volgende tabel bevat de kosten voor elk account type.

Accounttype | Azure-gebruik | Gebruik van Marketplace | Module | Restituties
--- | --- | --- | --- | ---
Enterprise Agreement (EA) | Ja | Ja | Ja | Nee
Microsoft-klantovereenkomst (MCA) | Ja | Ja | Ja | Ja
Betalen naar gebruik (PAYG) | Ja | Nee | Nee | Nee

Zie [inzicht krijgen in de kosten van uw Azure-service](billing-understand-your-azure-marketplace-charges.md)voor meer informatie over Marketplace-orders (ook wel bekend als externe services).

Zie [How to load your Azure billing invoice and Daily Usage Data](billing-download-azure-invoice-daily-usage-date.md)(Engelstalig) voor instructies voor het downloaden.
U kunt het CSV-bestand voor gebruik en kosten openen in micro soft Excel of een andere spreadsheet toepassing.

## <a name="list-of-terms-and-descriptions"></a>Lijst met voor waarden en beschrijvingen

In de volgende tabel worden de belangrijkste termen beschreven die worden gebruikt in de meest recente versie van het Azure-gebruik en het kosten bestand.
De lijst bevat een overzicht van betalen per gebruik (PAYG), Enterprise Agreement (EA) en micro soft Customer Agreement (MCA)-accounts.

Begrip | Accounttype | Description
--- | --- | ---
AccountName | EA, PAYG | Weergave naam van het EA-inschrijvings account of het PAYG-facturerings account.
AccountOwnerId | EA, PAYG | De unieke id voor het EA-inschrijvings account of het PAYG-facturerings account.
Extra informatie | Alle | Servicespecifieke meta gegevens. Bijvoorbeeld een afbeeldings type voor een virtuele machine.
BillingAccountId | Alle | De unieke id voor het hoofd account van de facturering.
BillingAccountName | Alle | De naam van het facturerings account.
BillingCurrency | Alle | De valuta die aan het facturerings account is gekoppeld.
BillingPeriod | EA, PAYG | De facturerings periode van de kosten.
BillingPeriodEndDate | Alle | De eind datum van de facturerings periode.
BillingPeriodStartDate | Alle | De begin datum van de facturerings periode.
BillingProfileId | Alle | De unieke id van de EA-inschrijving, het PAYG-abonnement, het MCA-facturerings profiel of het geconsolideerde AWS-account.
BillingProfileName | Alle | Naam van de EA-inschrijving, het PAYG-abonnement, het MCA-facturerings profiel of het geconsolideerde AWS-account.
ChargeType | Alle | Geeft aan of de kosten het gebruik (**gebruik**), een aankoop (**aankoop**) of een restitutie (**restitutie**) vertegenwoordigen.
Verbruikte service | Alle | De naam van de service waaraan de kosten zijn gekoppeld.
CostCenter | EA, MCA | Het kosten centrum is voor het abonnement gedefinieerd voor het bijhouden van kosten (alleen beschikbaar in openstaande facturerings perioden voor MCA-accounts).
Kosten | EA, PAYG | Zie CostInBillingCurrency.
CostInBillingCurrency | MCA | Kost prijs van de kosten in de facturerings valuta vóór kredieten of belastingen.
CostInPricingCurrency | MCA | Kost prijs van de kosten in de prijs valuta vóór kredieten of belastingen.
Currency | EA, PAYG | Zie BillingCurrency.
Date | Alle | Het gebruik of de aankoop datum van de kosten.
EffectivePrice | Alle | Gemengde eenheids prijs voor de periode. De gefactureerde prijzen hebben een gemiddelde van schommelingen in de prijs per eenheid, zoals gegradueerde lagen, waardoor de prijs wordt verlaagd als de hoeveelheid in de loop van de tijd toeneemt.
ExchangeRateDate | MCA | De datum waarop de wissel koers is ingesteld.
ExchangeRatePricingToBilling | MCA | De wissel koers die wordt gebruikt om de kosten in de prijs valuta te converteren naar de facturerings valuta.
Frequentie | Alle | Hiermee wordt aangegeven of verwacht wordt dat er een kosten worden herhaald. Kosten kunnen één keer voor komen (**eenmalige**), herhalen op een maandelijks of jaarlijks (**periodiek**) of op basis van het gebruik (**UsageBased**).
IncludedQuantity | PAYG | De hoeveelheid van de meter die gratis is inbegrepen in uw huidige facturerings periode.
InstanceId | PAGY | Zie ResourceId.
InvoiceId | PAYG, MCA | De unieke document-ID die wordt weer gegeven op de factuur-PDF.
InvoiceSection | MCA | Zie InvoiceSectionName.
InvoiceSectionId | EA, MCA | De unieke id voor de sectie EA Department of MCA invoice.
InvoiceSectionName | EA, MCA | Naam van de sectie EA-afdeling of MCA-factuur.
IsAzureCreditEligible | Alle | Geeft aan of de kosten in aanmerking komen voor betaling met Azure-tegoed (waarden: True, false).
Location | MCA | De locatie van het Data Center waar de resource wordt uitgevoerd.
Metercategorie | Alle | De naam van de classificatie categorie voor de meter. Bijvoorbeeld *Cloud Services* en *netwerken*.
Meter-id | Alle | De unieke id voor de meter.
MeterName | Alle | De naam van de meter.
Meterregio | Alle | Naam van de locatie van het Data Center voor services op basis van locatie. Zie locatie.
MeterSubCategory | Alle | De naam van de categorie voor de subclassificatie van de meter.
OfferId | Alle | De naam van de gekochte aanbieding.
PartNumber | EA, PAYG | Id die wordt gebruikt voor het ophalen van specifieke prijzen voor de meter.
PlanName | EA, PAYG | Naam van Marketplace-abonnement.
PreviousInvoiceId | MCA | Verwijzing naar een oorspronkelijke factuur als het regel item een terugbetaling is.
PricingCurrency | MCA | Valuta die wordt gebruikt als beoordeling op basis van de overeengekomen prijzen.
Product | Alle | De naam van het product.
ProductId | MCA | De unieke id voor het product.
ProductOrderId | Alle | De unieke id voor de product order.
ProductOrderName | Alle | De unieke naam voor de product order.
PublisherName | Alle | Publisher voor Marketplace-Services.
PublisherType | Alle | Type Uitgever (waarden: **Azure**, **AWS**, **Marketplace**).
Hoeveelheid | Alle | Het aantal eenheden dat is gekocht of verbruikt.
ReservationId | EA, MCA | De unieke id voor het aangeschafte reserverings exemplaar.
Reservation | EA, MCA | De naam van het aangeschafte reserverings exemplaar.
ResourceGroup | Alle | De naam van de [resource groep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) waarin de resource zich bevindt.
ResourceId | Alle | De unieke id van de [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) resource.
Resourcelocatie | Alle | De locatie van het Data Center waar de resource wordt uitgevoerd. Zie locatie.
ResourceName | EA, PAYG | Naam van de resource.
ResourceType | MCA | Type resource-exemplaar.
ServiceFamily | MCA | De service familie waartoe de service behoort.
ServiceInfo1 | Alle | Servicespecifieke meta gegevens.
ServiceInfo2 | Alle | Verouderd veld met optionele servicespecifieke meta gegevens.
ServicePeriodEndDate | MCA | De eind datum van de beoordelings periode die de prijzen voor de verbruikte of gekochte service heeft gedefinieerd en vergrendeld.
ServicePeriodStartDate | MCA | De begin datum van de beoordelings periode die de prijzen voor de verbruikte of gekochte service heeft gedefinieerd en vergrendeld.
SubscriptionId | Alle | De unieke id voor het Azure-abonnement.
Abonnementsnaam | Alle | De naam van het Azure-abonnement.
Labels | Alle | Tags toegewezen aan de resource. Bevat geen tags voor een resource groep. Kan worden gebruikt om de kosten voor interne terugstorting te groeperen of te distribueren. Zie [uw Azure-resources organiseren met Tags](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/)voor meer informatie.
Maateenheid | Alle | De maat eenheid voor facturering voor de service. Reken services worden bijvoorbeeld per uur gefactureerd.
UnitPrice | EA, PAYG | De prijs per eenheid voor de kosten.

Houd er rekening mee dat sommige velden kunnen verschillen in hoofdletter gebruik en tussen verschillende account typen.
Oudere versies van betalen per gebruik-gebruiks bestanden bevatten afzonderlijke secties voor het overzicht en dagelijks gebruik.

### <a name="list-of-terms-from-older-apis"></a>Lijst met voor waarden van oudere Api's
In de volgende tabel worden termen die worden gebruikt in oudere Api's, toegewezen aan de nieuwe voor waarden. Raadpleeg de bovenstaande tabel voor die beschrijvingen.

Oude term | Nieuwe term
--- | ---
Verbruikte hoeveelheid | Hoeveelheid
IncludedQuantity | N/A
InstanceId | ResourceId
Snelheid | EffectivePrice
Eenheid | Maateenheid
UsageDate | Date
UsageEnd | Date
UsageStart | Date


## <a name="ensure-charges-are-correct"></a>Controleer of de kosten juist zijn

Meer informatie over het gedetailleerde gebruik en de kosten vindt u in informatie over het begrijpen van uw [betalen per gebruik](./billing-understand-your-bill.md) -of [micro soft Customer Agreement](billing-mca-understand-your-bill.md) -factuur.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Uw Microsoft Azure factuur bekijken en downloaden](billing-download-azure-invoice.md)
- [Het gebruik en de kosten van Microsoft Azure bekijken en downloaden](billing-download-azure-daily-usage.md)
