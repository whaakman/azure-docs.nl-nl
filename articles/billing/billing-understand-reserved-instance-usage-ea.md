---
title: Meer informatie over Azure reserveringen gebruik voor Enterprise Agreements
description: Informatie over het lezen van uw gebruik voor meer informatie over hoe de Azure-reservering voor uw Enterprise-inschrijving wordt toegepast.
author: bandersmsft
manager: yashar
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/07/2019
ms.author: banders
ms.openlocfilehash: b2452580eaecc0ab922f8e7db48676f70831a8ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66126849"
---
# <a name="get-enterprise-agreement-reservation-costs-and-usage"></a>Kosten voor Enterprise Agreement-reservering en gebruiksgegevens ophalen

Kosten van de reservering en gebruiksgegevens zijn beschikbaar voor Enterprise Agreement-klanten in de Azure portal en de REST-API's. In dit artikel helpt u bij:

- Reservering kopen gegevens ophalen
- Reservering te voorzichtige gebruiksgegevens ophalen
- Afgeschreven kosten voor reservering
- Terugstorting voor het gebruik van de reservering
- Reservering besparing berekenen

Marketplace-kosten worden geconsolideerd in gegevens over het gebruik. U weergeven kosten in rekening gebracht voor de eerste partij gebruik, het gebruik van marketplace en aankopen van één gegevensbron.

## <a name="reservation-charges-in-azure-usage-data"></a>Kosten voor reservering in gebruik van Azure-gegevens

Gegevens is onderverdeeld in twee afzonderlijke gegevenssets: _Werkelijke kosten_ en _afgeschreven kosten_. Hoe worden deze twee gegevenssets verschillen:

**Werkelijke kosten** -gegevens levert aan afstemmen op uw maandelijkse factuur. Deze gegevens hebben een reservering kopen kosten. Heeft nul EffectivePrice voor het gebruik dat de reserveringskorting hebt ontvangen.

**Afgeschreven kosten** -de resource EffectiveCost die de reserveringskorting krijgt is de naar rato kosten van de gereserveerde instantie. De gegevensset heeft ook niet-gebruikte reservering kosten. De som van de reservering kosten en niet-gebruikte reservering biedt de dagelijkse afgeschreven kosten van de reservering.

Vergelijking van twee gegevenssets:

| Gegevens | Werkelijke kosten-gegevensset | Afgeschreven kosten-gegevensset |
| --- | --- | --- |
| Aankopen in de reservering | Beschikbaar in deze weergave.<br><br>  Voor dit gegevensfilter over ChargeType = &quot;aankoop&quot;. <br><br> Raadpleeg ReservationID of ReservationName weten welke reservering is bedoeld voor de kosten in rekening gebracht.  | Niet van toepassing op deze weergave. <br><br> Aankoop kosten zijn niet opgegeven in de afgeschreven gegevens. |
| EffectivePrice | De waarde is nul voor gebruik dat opgehaald van de reserveringskorting. | De waarde is de kosten per uur Pro rata berekend van de reservering voor gebruik met de reserveringskorting. |
| Niet-gebruikte reservering (biedt het aantal uren dat de reservering niet is gebruikt in een dag en de monetaire waarde van de verspilling) | Niet van toepassing in deze weergave. | Beschikbaar in deze weergave.<br><br> Als u deze gegevens, filteren op ChargeType = &quot;UnusedReservation&quot;.<br><br>  Raadpleeg ReservationID of ReservationName weten welke reservering is gebruikt. Dit is de hoeveelheid van de reservering is verloren voor die dag.  |
| Prijs per eenheid (prijs van de bron van de prijslijst) | Beschikbaar | Beschikbaar |

Andere informatie die beschikbaar zijn in gebruik van Azure-gegevens is gewijzigd:

- Product- en informatie van de Meter - Azure niet vervangen door de oorspronkelijk verbruikte meter de ReservationId en ReservationName, als deze eerder hebt gedaan.
- ReservationId en ReservationName - ze hun eigen velden in de gegevens zijn. Het gebruikt voorheen alleen onder aanvullende informatie beschikbaar.
- ProductOrderId - de reserveringsorder-ID, als een eigen veld toegevoegd.
- ProductOrderName - de naam van het product van de aangeschafte reservering.
- Term - twaalf maanden of 36 maanden.
- RINormalizationRatio - onder aanvullende informatie beschikbaar. Dit is de waarde waar de reservering wordt toegepast op de record gebruik. Als grootte-instantieflexibiliteit op voor de reservering is ingeschakeld, kunt klikt u vervolgens deze toepassen op andere grootten. De waarde geeft de verhouding tussen de reservering is toegepast op voor de record van het gebruik.

## <a name="get-azure-consumption-and-reservation-usage-data-using-api"></a>Azure-verbruik en de reservering gebruiksgegevens met behulp van API ophalen

U kunt de gegevens met behulp van de API ophalen of het downloaden van Azure portal.

U belt de [Usage Details API](/rest/api/consumption/usagedetails/list) met API-versie &quot;2019-04-01-preview&quot; om op te halen van de nieuwe gegevens. Zie voor meer informatie over terminologie [gebruiksvoorwaarden](billing-understand-your-usage.md). De aanroeper moet een Enterprise-beheerder voor de enterprise overeenkomst met de [EA-portal](https://ea.azure.com). Alleen-lezen beheerders van de onderneming kunt ook de gegevens ophalen.

De gegevens zijn niet beschikbaar in [rapportage-API's voor klanten met Enterprise - informatie over het gebruik](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail).

Hier volgt een voorbeeld van de aanroep naar de API:

```
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{enrollmentId}/providers/Microsoft.Billing/billingPeriods/{billingPeriodId}/providers/Microsoft.Consumption/usagedetails?metric={metric}&amp;api-version=2019-04-01-preview&amp;$filter={filter}
```

Zie voor meer informatie over {enrollmentId} en {billingPeriodId}, de [informatie over het gebruik – lijst](https://docs.microsoft.com/rest/api/consumption/usagedetails/list) -API-artikel.

Informatie over metrische gegevens en filter in de volgende tabel kunt voor reservering van veelvoorkomende problemen oplossen.

| **Type van de gegevens van de API** | API-aanroep actie |
| --- | --- |
| **Alle kosten (gebruik en aankopen)** | {Metriek} vervangen door ActualCost |
| **Gebruik dat de reserveringskorting is** | {Metriek} vervangen door ActualCost<br><br>{Filter} vervangen door: properties/reservationId%20ne%20 |
| **Gebruik dat de reserveringskorting is niet ontvangen** | {Metriek} vervangen door ActualCost<br><br>{Filter} vervangen door: properties/reservationId%20eq%20 |
| **Afgeschreven kosten (gebruik en aankopen)** | {Metriek} vervangen door AmortizedCost |
| **Rapport voor niet-gebruikte reservering** | {Metriek} vervangen door AmortizedCost<br><br>{Filter} vervangen door: properties/ChargeType%20eq%20'UnusedReservation' |
| **Aankopen in de reservering** | {Metriek} vervangen door ActualCost<br><br>{Filter} vervangen door: properties/ChargeType%20eq%20'Purchase'  |
| **Restituties** | {Metriek} vervangen door ActualCost<br><br>{Filter} vervangen door: properties/ChargeType%20eq%20'Refund' |

## <a name="download-the-usage-csv-file-with-new-data"></a>Het gebruik van CSV-bestand met nieuwe gegevens downloaden

Als u een EA-beheerder bent, kunt u het CSV-bestand met nieuwe gegevens over het gebruik van Azure-portal kunt downloaden. Deze gegevens niet beschikbaar is via de [EA-portal](https://ea.azure.com).

In de Azure-portal, gaat u naar [kostenbeheer en facturering](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/BillingAccounts).

1. Selecteer het account van de facturering.
2. Klik op **gebruik en kosten**.
3. Klik op **Downloaden**.  
![Voorbeeld van het downloaden van het bestand met CSV-gebruiksgegevens in Azure portal](./media/billing-understand-reserved-instance-usage-ea/portal-download-csv.png)
4. In **Download Usage + kosten** onder **gebruik Details versie 2** , selecteer **alle kosten (gebruik en aankopen)** en klik op downloaden. Herhaal dit voor **afgeschreven kosten in rekening gebracht (gebruik en aankopen)** .

De CSV-bestanden die u downloadt bevatten werkelijke kosten en de afgeschreven kosten.

## <a name="common-cost-and-usage-tasks"></a>Algemene taken voor kosten en gebruik

De volgende secties worden algemene taken die de meeste mensen gebruiken om hun reservering kosten en gebruik de gegevens weer te geven.

### <a name="get-reservation-purchase-costs"></a>Reservering kopen kosten ophalen

Reservering kopen kosten zijn beschikbaar in de werkelijke kosten van gegevens. Filteren op _ChargeType = aankoop_. Raadpleeg ProductOrderID om te bepalen welke de aanschaf is voor reserveringsorder.

### <a name="get-underutilized-reservation-quantity-and-costs"></a>Weinig gebruikte reserveringshoeveelheid en kosten

Afgeschreven kosten-gegevens ophalen en filteren op _ChargeType_ _UnusedReservation =_ . U profiteert van de dagelijkse ongebruikte gereserveerde hoeveelheid en de kosten. U kunt de gegevens voor een reservering of een order met behulp van reservering filteren _ReservationId_ en _ProductOrderId_ velden, respectievelijk. Als een reservering 100 is % gebruikt, heeft de record een aantal van 0.

### <a name="amortize-reservation-costs"></a>Afgeschreven kosten voor reservering

Gegevens van de afgeschreven kosten en filter voor het gebruik van een reservering _ProductOrderID_ dagelijkse afgeschreven kosten ophalen voor een reservering.

### <a name="chargeback-for-a-reservation"></a>Terugstorting voor een reservering

U kunt terugstorting reservering gebruiken met andere organisaties door abonnementen, resourcegroepen of tags. Afgeschreven kosten biedt monetaire waarde van het gebruik van een reservering op de volgende gegevenstypen:

- Resources (zoals een virtuele machine)
- Resourcegroep
- Tags
- Abonnement

### <a name="get-the-blended-rate-for-chargeback"></a>Het gecombineerde tarief voor terugstorting ophalen

Om te bepalen het gecombineerde tarief, de afgeschreven kosten-gegevens ophalen en samenvoegen van de totale kosten. Voor virtuele machines, kunt u MeterName of ServiceType informatie uit AdditionalInfo JSON-gegevens. De totale kosten door het aantal dat wordt gebruikt voor het ophalen van de gecombineerde snelheid verdelen.

### <a name="audit-optimum-reservation-use-for-instance-size-flexibility"></a>Audit optimale reservering bijvoorbeeld de flexibiliteit van de grootte gebruiken

Meerdere hoeveelheid met de _RINormalizationRatio_, van aanvullende informatie. De resultaten aangeven hoeveel uur gebruik van de reservering is toegepast op de record gebruik.

### <a name="determine-reservation-savings"></a>Reservering besparingen bepalen

De kosten Amortized gegevens ophalen en filteren van gegevens voor een gereserveerde instantie. Vervolgens:

1. De geschatte kosten van de betalen per gebruik. Vermenigvuldigt u de _PrijsPerEenheid_ waarde met _hoeveelheid_ waarden om op te halen de geschatte kosten van betalen per gebruik, als de reserveringskorting is niet van toepassing op het gebruik.
2. De kosten voor reservering ophalen. Som de _kosten_ waarden om op te halen van de geldwaarde van wat u hebt betaald voor de gereserveerde instantie. Dit omvat de kosten die worden gebruikt en niet-gebruikte van de reservering.
3. Kosten van de reservering van de geschatte kosten voor betalen per gebruik om op te halen van de geschatte besparing aftrekken.

## <a name="reservation-purchases-and-amortization-in-azure-cost-analysis"></a>Aankopen in de reservering en afschrijving in Azure kostenanalyse

Kosten van gereserveerde instantie is beschikbaar in [Azure cost analysis preview-modus](https://preview.portal.azure.com/?feature.canmodifystamps=true&amp;microsoft_azure_costmanagement=stage2&amp;Microsoft_Azure_CostManagement_arm_canary=true&amp;Microsoft_Azure_CostManagement_apiversion=2019-04-01-preview&amp;Microsoft_Azure_CostManagement_amortizedCost=true#blade/Microsoft_Azure_CostManagement/Menu/costanalysis). De gegevensweergave kosten is standaard voor de werkelijke kosten. U kunt overschakelen naar de afgeschreven kosten. Hier volgt een voorbeeld.

![Voorbeeld van waar u de afgeschreven kosten selecteren in kostenanalyse](./media/billing-understand-reserved-instance-usage-ea/portal-cost-analysis-amortized-view.png)

Toepassen van filters om te zien van uw kosten met een type reservering of kosten in rekening gebracht. Groeperen op de Reserveringsnaam voor de kosten voor onderverdeeld op basis van reserveringen.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)
