---
title: Meer informatie over de voorwaarden in de prijslijst voor een Microsoft-KLANTOVEREENKOMST - Azure
description: Informatie over het lezen en meer informatie over het gebruik en de factuur voor een Microsoft-KLANTOVEREENKOMST.
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
ms.openlocfilehash: 4d83228fbec395d604e5ce3f988d2a6157f21eed
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490653"
---
# <a name="terms-in-your-microsoft-customer-agreement-price-sheet"></a>Voorwaarden in uw Microsoft-KLANTOVEREENKOMST prijslijst

In dit artikel is van toepassing op een Azure-facturering account voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

Als u een factureringsprofiel eigenaar, bijdrager, lezer of factuur Manager kunt u de prijslijst van uw organisatie kunt downloaden vanuit Azure portal. Zie [weergeven en downloaden van de prijzen voor uw organisatie](billing-ea-pricing.md).

## <a name="terms-and-descriptions-in-your-price-sheet"></a>Voorwaarden en beschrijvingen in de prijslijst

De volgende sectie bevat de belangrijke termen weergegeven in uw Microsoft-KLANTOVEREENKOMST-prijslijst.

| **Veldnaam**   | **Beschrijving**   |
| --- | --- |
| billingAccountId  | De unieke id voor de facturering-account.   |
| billingAccountName  | De naam van het factureringsaccount.  |
| billingProfileId  | De unieke id voor de facturering-profiel.   |
| billingProfileName  | Naam van de facturering profiel dat is ingesteld om facturen te ontvangen. De prijzen in de prijslijst zijn gekoppeld aan dit profiel facturering. |
| productOrderName  | Naam van het plan aangeschafte producten. |
| serviceFamily  | Het type Azure-service. Bijvoorbeeld: COMPUTE-, analyse, beveiliging |
| Product  | De naam van het product de kosten oplopen. Bijvoorbeeld: Algemene SQL DB vs standaard SQL DB  |
| productId  | De unieke id voor het product waarvan meter wordt verbruikt. |
| unitOfMeasure  | Identificeert de maateenheid voor de service wordt gefactureerd. Bijvoorbeeld, worden compute-services gefactureerd per uur. |
| meterId  | De unieke id voor de meter. |
| meterName  | De naam van de meter. De meter vertegenwoordigt het implementeerbare resource van een Azure-service. |
| meterCategory  | De naam van de classificatiecategorie voor de meter. Bijvoorbeeld, _Cloudservices_, _netwerken_, enzovoort. |
| meterType  |  Naam van het metertype. |
| meterSubCategory  | De naam van de onderliggende classificatiecategorie meter.  |
| meterRegion  | De naam van de regio waar de meter voor de service beschikbaar is. De datacenterlocatie voor bepaalde services waarbij de prijs is gebaseerd op de datacenterlocatie.    |
| tierId  | Hiermee geeft u de prijscategorie indien van toepassing. Dit werkt in combinatie met tierMinimumUnits voor gelaagde prijzen wanneer prijzen variëren op basis van het aantal eenheden verbruikt.    |
| tierMinimumUnits  | Hiermee definieert u de ondergrens van de laag bereik waarvoor de prijzen zijn gedefinieerd. Als het bereik 0 tot 100 is, zou tierMinimumUnits 0 zijn.  |
| effectiveStartDate  | Start de datum waarop de prijs van kracht. |
| effectiveEndDate  | Einddatum van de effectieve prijs. |
| unitPrice  | Prijs per eenheid op het moment van facturering (niet de gecombineerde prijs) als specifiek is voor de naam van een meter en het product.  Opmerking: De prijs per eenheid is niet hetzelfde als de effectieve prijs in informatie over het gebruik downloads in het geval van services die differentiële prijzen voor lagen zijn.  In het geval van services met meerdere lagen prijzen, de prijs is een gecombineerde tarief voor de lagen en een laag-specifieke eenheidsprijs niet wordt weergegeven. De gecombineerde prijs of de prijs is de nettoprijs van de verbruikte hoeveelheid spanning voor de verschillende lagen (waarbij elke laag heeft voor een specifieke eenheidsprijs). |
| basePrice  | De prijs van de markt op het moment de klant zich heeft aangemeld of de prijs van de markt op het moment de meter voor de service wordt gestart als deze zich na aanmelding.   |

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Weergeven en downloaden van uw organisatie prijzen](billing-ea-pricing.md)
