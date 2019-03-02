---
title: Meer informatie over uw Azure-gebruik en kosten CSV voor een Microsoft-KLANTOVEREENKOMST | Microsoft Docs
description: Meer informatie over het lezen en begrijpen van de secties van de Azure-gebruik en kosten CSV voor uw facturering profiel
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: banders
ms.openlocfilehash: 774f6bd4beb2c2b8b36588ee595996860a9c7549
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249030"
---
# <a name="understand-terms-on-your-azure-usage-and-charges-csv-for-a-microsoft-customer-agreement"></a>Meer informatie over uw Azure-gebruik en kosten CSV voor een Microsoft-KLANTOVEREENKOMST

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

Het Azure-gebruik en kosten CSV-bestand bevat dagelijks en meter niveau gebruikskosten voor de huidige factureringsperiode.

Als u uw Azure-voor gebruik en de kosten, Zie [weergeven en downloaden gebruik van Azure en kosten voor uw Microsoft-KLANTOVEREENKOMST](billing-download-azure-daily-usage.md).
Het is beschikbaar in een indeling met door komma's gescheiden waarden (.csv) die u in een werkbladtoepassing openen kunt.

Gebruikskosten zijn de totale **maandelijkse** kosten op een abonnement. De kosten voor het gebruik rekening geen gehouden met eventuele tegoeden of kortingen.

## <a name="changes-in-the-enterprise-agreement-azure-usage-and-charges-csv"></a>Wijzigingen in de Azure Enterprise Agreement-gebruik en de kosten CSV

Als u een EA-klant bent, zult u merken dat de voorwaarden in de facturering gebruik CSV-bestand van de Azure-profiel zijn anders dan de voorwaarden in het CSV-bestand van de EA Azure-gebruik. Hier volgt een toewijzing van EA-gebruiksvoorwaarden tot facturering gebruiksvoorwaarden profiel:

| EA Azure-gebruik CSV | Microsoft Customer overeenkomst Azure-gebruik en kosten CSV |
| --- | --- |
| Date | date |
| Maand| date |
| Dag | date |
| Jaar | date |
| Product | product |
| Meter-id | meterID |
| Metercategorie | meterCategory |
| Metersubcategorie | meterSubCategory |
| Meterregio | meterRegion |
| Meternaam | meterName |
| Verbruikte hoeveelheid | quantity |
| Resourcetarief | effectivePrice | <!-- this was highlighted -->
| Berekende kosten | kosten |
| Resourcelocatie | resourceLocation |
| Verbruikte service | consumedService |
| Instantie-id | instanceId |
| Servicegegevens 1 | serviceInfo1 |
| Servicegegevens 2 | serviceInfo2 |
| Extra informatie | additionalInfo |
| Tags | tags |
| Service-id voor de store | N/A |
| Naam van de afdeling | invoiceSection | <!-- this was highlighted -->
| Kostenplaats | costCenter |
| Maateenheid | unitofMeasure |
| ResourceGroup | resourceGroup |
| ChargesBilledSeparately | isAzureCreditEligible | <!-- this was highlighted -->

<!-- TO DO: Marketplace CSV? -->

## <a name="detailed-terms-and-descriptions-in-your-azure-usage-and-charges-file"></a>Gedetailleerde voorwaarden en beschrijvingen in uw Azure-voor gebruik en de kosten

De volgende sectie bevat de belangrijke termen weergegeven in een bestand met de Azure-gebruik en kosten in rekening gebracht.

Termijn | Description
--- | ---
invoiceId | De unieke document-ID weergegeven op de factuur PDF
previousInvoiceId | Verwijzing naar een oorspronkelijke factuur als dit regelitem een restitutie
billingAccountName | Naam van het factureringsaccount
billingAccountId | De unieke id voor de basis-account
billingProfileId | Naam van de facturering profiel dat lopen de kosten die worden gefactureerd
billingProfileName | De unieke id voor de facturering-profiel dat lopen de kosten die worden gefactureerd
invoiceSectionId | De unieke id voor het gedeelte factuur
invoiceSectionName | Naam van de factuur-sectie
costCenter | De kostenplaats die is gedefinieerd voor het abonnement voor het bijhouden van kosten (alleen beschikbaar in open factureringsperioden)
billingPeriodStartDate | De begindatum van de factureringsperiode waarvoor de factuur is gegenereerd
billingPeriodEndDate | De einddatum van de factureringsperiode waarvoor de factuur is gegenereerd
servicePeriodStartDate | De begindatum van de periode voor de classificatie die die is gedefinieerd en vergrendeld prijsgegevens voor de service verbruikt of worden gekocht
servicePeriodEndDate | De einddatum van de periode voor de classificatie die die is gedefinieerd en vergrendeld prijsgegevens voor de service verbruikt of worden gekocht
date | Dit is de datum van de classificatie voor Azure Marketplace op gebruik gebaseerde kosten en. Dit is de aankoopdatum voor eenmalige aankopen (reserveringen, Marketplace) of vaste terugkerende kosten (ondersteuning voor aanbiedingen).
serviceFamily | Service-familie die deel uitmaakt van de service op
productOrderId | De unieke id voor de volgorde van het product
productOrderName | Unieke naam op voor de volgorde van het product
consumedService | Naam van de verbruikte service
meterId | De unieke id voor de meter
meterName | De naam van de meter
meterCategory | De naam van de classificatiecategorie voor de meter. Bijvoorbeeld, *Cloudservices*, *netwerken*, enzovoort.
meterSubCategory | Naam van de onderliggende classificatie metercategorie
meterRegion | De naam van de regio waar de meter voor de service beschikbaar is. Geeft de locatie van het datacenter voor bepaalde services waarbij de prijs is gebaseerd op de datacenterlocatie.
aanbieding | Naam van de aanbieding hebt aangeschaft
productId | De unieke id voor het product de kosten oplopen
product | Naam van het product de kosten oplopen
abonnements-ID | De unieke id voor het abonnement de kosten oplopen
subscriptionName | Naam van het abonnement de kosten oplopen
reservationId | De unieke id voor het exemplaar van de aangeschafte reservering
reservationName | Naam van het exemplaar van de aangeschafte reservering
publisherType | Type van de uitgever (waarden: firstParty, thirdPartyReseller, thirdPartyAgency)
publisherName | Uitgever voor Marketplace-services
resourceGroupId | De unieke id voor de resourcegroep die is gekoppeld aan de resource
resourceGroupName | Naam van de resourcegroep die is gekoppeld aan de resource
resourceId | De unieke id voor de resource-instantie
ResourceType | Type resource-exemplaar
resourceLocation | Geeft de locatie van het datacenter waar de resource wordt uitgevoerd.
location | Genormaliseerde locatie van de resource als andere resource-locaties zijn geconfigureerd voor dezelfde regio 's
quantity | Het aantal eenheden die zijn aangeschaft of die worden gebruikt
unitOfMeasure | De maateenheid voor de service wordt gefactureerd. Bijvoorbeeld, worden compute-services gefactureerd per uur.
chargeType | Het type van de kosten in rekening gebracht. Waarden: <ul><li>AsCharged-Usage: Opgetelde kosten op basis van gebruik van een Azure-service. Dit geldt ook voor gebruik op basis van virtuele machines die geen kosten in rekening vanwege gereserveerde instanties gebracht.</li><li>AsCharged-PurchaseMarketplace: Eenmalig of vaste terugkerende kosten van Marketplace-aankopen</li><li>AsCharged-UsageMarketplace: Kosten voor Marketplace-services die worden in rekening gebracht op basis van eenheden van het verbruik</li></ul>
isAzureCreditEligible | Markering waarmee wordt aangegeven of de kosten worden berekend op basis van de service in aanmerking komende te betalen voor het gebruik van Azure-tegoed (waarden: True, False)
serviceInfo1 | Servicespecifieke metagegevens
serviceInfo2 | Ouder veld met optionele servicespecifieke metagegevens
additionalInfo | Aanvullende servicespecifieke metagegevens.
tags | Tags die u aan de resource toewijst

### <a name="how-do-i-make-sure-that-the-charges-in-my-azure-usage-and-charges-file-are-correct"></a>Hoe ik ervoor zorgen dat de kosten in rekening gebracht in mijn Azure-gebruik en de kosten-bestand correct zijn?

Als er een kosten in rekening gebracht op uw gedetailleerde gebruik-bestand dat u wilt meer informatie over, Zie [meer informatie over de kosten op uw profiel facturering factuur](billing-mca-understand-your-bill.md)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- [Weergeven en uw Microsoft Azure-factuur downloaden](billing-download-azure-invoice.md)
- [Weergeven en downloaden van uw Microsoft Azure-gebruik en kosten](billing-download-azure-daily-usage.md)
