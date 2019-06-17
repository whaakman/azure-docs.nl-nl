---
title: Meer informatie over uw gedetailleerde gebruik kosten | Microsoft Docs
description: Meer informatie over het lezen en begrijpen van uw gedetailleerde gebruik en kosten
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
ms.openlocfilehash: 9ff9b6b5313026d2102b98659183fa97c6a5ef84
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683991"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Informatie over de voorwaarden in uw Azure-gebruik en de kosten-bestand

Het gedetailleerde gebruik en de kosten-bestand bevat dagelijkse geclassificeerde gebruik op basis van overeengekomen tarieven, aankopen (bijvoorbeeld reserveringen, Marketplace-kosten) en terugbetalingen voor de opgegeven periode.
Kosten omvatten tegoed, belastingen, of andere kosten in rekening gebracht of kortingen.
De volgende tabel behandelt welke kosten opgenomen voor elk type zijn.

Accounttype | Gebruik van Azure | Gebruik van Marketplace | Aankopen | Restituties
--- | --- | --- | --- | ---
Enterprise Agreement (EA) | Ja | Ja | Ja | Nee
Microsoft-klantovereenkomst (MCA) | Ja | Ja | Ja | Ja
Betalen per gebruik (betalen per gebruik) | Ja | Nee | Nee | Nee

Zie voor meer informatie over de Marketplace-bestellingen (ook wel bekend als externe services), [meer informatie over uw Azure externe servicekosten](billing-understand-your-azure-marketplace-charges.md).

Zie [over het verkrijgen van uw Azure-factuur en de dagelijkse gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md) voor downloadinstructies.
Het gebruik en de kosten-bestand is beschikbaar in een indeling met door komma's gescheiden waarden (.csv), waarmee kan worden geopend in een werkbladtoepassing.

## <a name="list-of-terms-and-descriptions"></a>Lijst met termen en beschrijvingen

De volgende tabel beschrijft de belangrijke termen in de meest recente versie van het Azure-gebruik en de kosten-bestand.
De lijst bevat informatie over betalen naar gebruik (betalen per gebruik), Enterprise Agreement (EA) en Microsoft Customer overeenkomst (MCA)-accounts.

Termijn | Accounttype | Description
--- | --- | ---
AccountName | EA | Weergavenaam van het inschrijvingsaccount.
AccountOwnerId | EA | De unieke id voor het inschrijvingsaccount.
Aanvullende informatie | Alle | Servicespecifieke metagegevens. Bijvoorbeeld, een type installatiekopie voor een virtuele machine.
BillingAccountId | EA, MCA | De unieke id voor de basis-account.
BillingAccountName | EA, MCA | De naam van het factureringsaccount.
BillingCurrency | EA, MCA | De valuta die zijn gekoppeld aan het factureringsaccount.
BillingPeriod | EA | De factureringsperiode van de kosten in rekening gebracht.
BillingPeriodEndDate | EA, MCA | De einddatum van de factureringsperiode.
BillingPeriodStartDate | EA, MCA | De begindatum van de factureringsperiode.
BillingProfileId | EA, MCA | De unieke id van de EA-inschrijving of MCA profiel facturering.
BillingProfileName | EA, MCA | Naam van de EA-inschrijving of MCA profiel facturering.
ChargeType | EA, MCA | Geeft aan of de kosten voor gebruik vertegenwoordigt (**gebruik**), een aankoop (**kopen**), of een restitutie (**restitutie**).
ConsumedQuantity | BETALEN PER GEBRUIK | Hoeveelheid bekijken.
ConsumedService | Alle | Naam van de kosten van de service is gekoppeld.
Kosten | EA | Zie CostInBillingCurrency.
CostCenter | EA, MCA | De kostenplaats gedefinieerd voor het abonnement voor het bijhouden van kosten (alleen beschikbaar in open factureringsperioden voor MCA accounts).
CostInBillingCurrency | MCA | Kosten van de kosten in rekening gebracht in de factureringsvaluta voordat tegoeden of belastingen.
CostInPricingCurrency | MCA | Kosten van de kosten in rekening gebracht in de prijscategorie valuta voordat tegoeden of belastingen.
Valuta | BETALEN PER GEBRUIK | Zie BillingCurrency.
Date | EA, MCA | De datum gebruik of aankoop van de kosten in rekening gebracht.
ExchangeRateDate | MCA | De datum waarop die de wisselkoers tot stand is gebracht.
ExchangeRatePricingToBilling | MCA | Wisselkoers gebruikt voor het converteren van de kosten in de prijscategorie valuta naar de factureringsvaluta.
Frequentie | EA, MCA | Geeft aan of een post wordt verwacht te herhalen. Kosten vindt een keer (**OneTime**), op basis van de maandelijkse of jaarlijkse herhalen (**periodiek**), of worden op basis van gebruik (**UsageBased**).
includedQuantity | BETALEN PER GEBRUIK | De hoeveelheid van de meter die is opgenomen gratis inbegrepen in uw huidige factureringsperiode.
InstanceId | PAGY | Zie ResourceId.
InvoiceId | EA, MCA | De unieke document-ID weergegeven op de factuur PDF-bestand.
invoiceSection | MCA | Zie InvoiceSectionName.
InvoiceSectionId | EA, MCA | De unieke id voor de EA-afdeling of een gedeelte van de factuur MCA.
InvoiceSectionName | EA, MCA | De naam van de EA-afdeling of een gedeelte van de factuur MCA.
IsAzureCreditEligible | EA, MCA | Geeft aan of de kosten in rekening gebracht in aanmerking komende te betalen voor het gebruik van Azure-tegoed (waarden: True, False).
Locatie | EA, MCA | Datacenter-locatie waar de resource wordt uitgevoerd.
MeterCategory | Alle | De naam van de classificatiecategorie voor de meter. Bijvoorbeeld, *Cloudservices* en *netwerken*.
Meter-id | Alle | De unieke id voor de meter.
MeterName | Alle | De naam van de meter.
MeterRegion | Alle | Naam van de locatie van het datacenter voor services waarbij de prijs is gebaseerd op locatie. Locatie wordt weergegeven.
MeterSubCategory | Alle | De naam van de meter subclassificatie categorie.
OfferId | EA, MCA | De naam van de aanbieding hebt aangeschaft.
PartNumber | EA | De id die wordt gebruikt om op te halen specifieke meter prijzen.
PlanName | EA | Naam van Marketplace-abonnement.
PreviousInvoiceId | MCA | Verwijzing naar een oorspronkelijke factuur als dit regelitem een terugbetaling.
pricingCurrency | MCA | Valuta gebruikt wanneer de classificatie op basis van overeengekomen prijzen.
Product | MCA | Zie de productnaam.
ProductId | EA, MCA | De unieke id voor het product.
Productnaam | EA | De naam van het product.
ProductOrderId | EA, MCA | De unieke id voor de volgorde van het product.
productOrderName | EA, MCA | Unieke naam op voor de volgorde van het product.
PublisherName | EA, MCA | De uitgever voor Marketplace-services.
PublisherType | EA, MCA | Type van de uitgever (waarden: firstParty, thirdPartyReseller, thirdPartyAgency).
Aantal | EA, MCA | Het aantal eenheden die zijn aangeschaft of die worden gebruikt.
Tarief | BETALEN PER GEBRUIK | Zie de prijs per eenheid.
ReservationId | EA, MCA | De unieke id voor het exemplaar van de aangeschafte reservering.
reservationName | EA, MCA | De naam van het exemplaar van de aangeschafte reservering.
resourceGroupId | EA, MCA | De unieke id voor de [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) de resource heeft.
ResourceGroupName | EA, MCA | Naam van de [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) de resource heeft.
ResourceId | EA, MCA | De unieke id van de [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) resource.
resourceLocation | EA, MCA | Datacenter-locatie waar de resource wordt uitgevoerd. Locatie wordt weergegeven.
ResourceName | EA | Naam van de resource.
ResourceType | MCA | Het type resource-exemplaar.
serviceFamily | EA, MCA | Service-familie die deel uitmaakt van de service op.
ServiceInfo1 | Alle | Servicespecifieke metagegevens.
ServiceInfo2 | Alle | Ouder veld met optionele servicespecifieke metagegevens.
ServicePeriodEndDate | MCA | De einddatum van de periode voor de classificatie die gedefinieerd en vergrendeld prijsgegevens voor de service verbruikt of worden gekocht.
ServicePeriodStartDate | MCA | De begindatum van de periode voor de classificatie die gedefinieerd en vergrendeld prijsgegevens voor de service verbruikt of worden gekocht.
SubscriptionId | Alle | De unieke id voor het abonnement.
subscriptionName | Alle | De naam van het abonnement.
Tags | Alle | Tags die zijn toegewezen aan de resource. Geen groep resourcetags. Kan worden gebruikt te groeperen of kosten voor interne terugstorting te distribueren. Zie voor meer informatie, [ordenen van uw Azure-resources met tags](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/).
Eenheid | BETALEN PER GEBRUIK | Zie UnitOfMeasure.
unitOfMeasure | Alle | De maateenheid voor de service wordt gefactureerd. Bijvoorbeeld, worden compute-services gefactureerd per uur.
UnitPrice | EA | De prijs per eenheid voor de kosten in rekening gebracht.
UsageDate | BETALEN PER GEBRUIK | Datum bekijken.

Houd er rekening mee dat voor sommige velden in hoofdlettergebruik en de afstand tussen accounttypen kunnen verschillen.
Oudere versies van gebruiksbestanden van de betalen per gebruik hebben afzonderlijke secties voor de instructie en het dagelijks gebruik.

## <a name="ensure-that-your-charges-are-correct"></a>Zorg ervoor dat uw kosten correct zijn

Voor meer informatie over het gedetailleerde gebruik en kosten meer informatie over het inzicht in uw [betalen per gebruik](./billing-understand-your-bill.md) of [Microsoft KLANTOVEREENKOMST](billing-mca-understand-your-bill.md) factuur.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Weergeven en uw Microsoft Azure-factuur downloaden](billing-download-azure-invoice.md)
- [Weergeven en downloaden van uw Microsoft Azure-gebruik en kosten](billing-download-azure-daily-usage.md)
