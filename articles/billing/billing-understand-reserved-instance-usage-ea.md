---
title: Het gebruik van Azure-reserve ringen voor Enter prise agreements begrijpen
description: Meer informatie over hoe u uw gebruik kunt lezen om te begrijpen hoe de Azure-reserve ring voor uw Enter prise-registratie wordt toegepast.
author: bandersmsft
manager: yashar
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 507ad62a917120689bee3f1e293e23c9ab8b0f66
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598094"
---
# <a name="get-enterprise-agreement-reservation-costs-and-usage"></a>Enterprise Agreement reserverings kosten en gebruik ophalen

Er zijn reserverings kosten en gebruiks gegevens beschikbaar voor Enterprise Agreement klanten in de Azure Portal en REST-Api's. Dit artikel helpt u bij het volgende:

- Inkoop gegevens reserve ring ophalen
- Weet welk abonnement, resource groep of resource de reserve ring heeft gebruikt
- Terugstorting voor reserverings gebruik
- Reserverings besparingen berekenen
- Reserve ring verkrijgen onder gebruiks gegevens
- Reserverings kosten aflossen

Marketplace-kosten worden geconsolideerd in gebruiks gegevens. U kunt kosten bekijken voor het gebruik van de eerste partij, het gebruik van Marketplace en aankopen vanuit één gegevens bron.

## <a name="reservation-charges-in-azure-usage-data"></a>Reserverings kosten in azure-gebruiks gegevens

Gegevens worden onderverdeeld in twee afzonderlijke gegevens sets: De _werkelijke kosten_ en de afgeschreven _kosten_. Hoe deze twee gegevens sets verschillen:

**Werkelijke kosten** : bevat gegevens die u kunt afstemmen op uw maandelijkse factuur. Deze gegevens hebben reserve ring van de inkoop kosten en de reserverings toepassings gegevens. Met deze gegevens kunt u weten welk abonnement of welke resource groep of resource de reserverings korting heeft ontvangen op een bepaalde dag. De EffectivePrice voor het gebruik dat de reserverings korting ontvangt, is nul.

**Afgeschreven kosten** : deze gegevensset is vergelijkbaar met de werkelijke kosten gegevensset, met uitzonde ring van de EffectivePrice voor het gebruik dat de reserverings korting krijgt, de evenredige kosten van de reserve ring (in plaats van nul). Dit helpt u bij het bepalen van de monetaire waarde van reserverings verbruik door een abonnement, resource groep of resource, en kan u helpen om het gebruik van de reserve ring intern terug te brengen. De gegevensset heeft ook ongebruikte reserverings uren. De gegevensset heeft geen reserve ring van de inkoop records. 

Vergelijking van twee gegevens sets:

| Data | Gegevensset voor werkelijke kosten | Gegevensset voor afgeschreven kosten |
| --- | --- | --- |
| Reserverings aankopen | Beschikbaar in deze weer gave.<br><br>  Als u dit gegevens filter op ChargeType = &quot;aankoop&quot;wilt ophalen. <br><br> Raadpleeg ReservationID of reservernaam om te weten welke reserve ring de kosten voor zijn.  | Niet van toepassing op deze weer gave. <br><br> Er worden geen inkoop kosten in afgeschreven gegevens gegeven. |
| EffectivePrice | De waarde is nul voor het gebruik van de reserverings korting. | De waarde is per uur naar evenredige kosten van de reserve ring voor gebruik met de reserverings korting. |
| Ongebruikte reserve ring (geeft het aantal uur dat de reserve ring niet is gebruikt op een dag en de monetaire waarde van het afval) | Niet van toepassing in deze weer gave. | Beschikbaar in deze weer gave.<br><br> Als u deze gegevens wilt ophalen, filtert &quot;u&quot;op ChargeType = UnusedReservation.<br><br>  Raadpleeg ReservationID of Reservationnaam om te weten welke reserve ring is ondergebruikt. Dit is het gedeelte van de reserve ring dat voor de dag is verspild.  |
| UnitPrice (prijs van de resource in uw prijzen overzicht) | Beschikbaar | Beschikbaar |

Andere informatie die beschikbaar is in azure-gebruiks gegevens is gewijzigd:

- Product-en meter gegevens-Azure vervangt niet de oorspronkelijk gebruikte meter met de ReservationId en reserverings naam, zoals eerder was gebruikt.
- ReservationId en reservernaam: ze zijn hun eigen velden in de gegevens. Voorheen werd deze alleen beschikbaar onder AdditionalInfo.
- ProductOrderId: de reserverings Order-ID, die als een eigen veld is toegevoegd.
- ProductOrderName: de product naam van de aangeschafte reserve ring.
- Periode van 12 maanden of 36 maanden.
- RINormalizationRatio: beschikbaar onder AdditionalInfo. Dit is de verhouding waar de reserve ring wordt toegepast op de gebruiks record. Als de flexibiliteit van de instantie grootte is ingeschakeld voor uw reserve ring, kan deze van toepassing zijn op andere grootten. De waarde toont de verhouding waarop de reserve ring is toegepast voor de gebruiks record.

## <a name="get-azure-consumption-and-reservation-usage-data-using-api"></a>Gebruiks gegevens voor Azure-verbruik en-reserve ring ophalen met behulp van API

U kunt de gegevens ophalen met behulp van de API of downloaden van Azure Portal.

U roept de [gebruiks gegevens-API](/rest/api/consumption/usagedetails/list) aan met &quot;API versie 2019-04-01&quot; -Preview om de nieuwe gegevens te verkrijgen. Zie [gebruiks voorwaarden](billing-understand-your-usage.md)voor meer informatie over terminologie. De aanroeper moet een ondernemings beheerder voor de Enter prise Agreement zijn met behulp van de [EA-Portal](https://ea.azure.com). Alleen-lezen ondernemings Administrators kunnen de gegevens ook ophalen.

De gegevens zijn niet beschikbaar in [rapportage-api's voor zakelijke klanten-gebruiks gegevens](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail).

Hier volgt een voor beeld van een aanroep naar de API:

```
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{enrollmentId}/providers/Microsoft.Billing/billingPeriods/{billingPeriodId}/providers/Microsoft.Consumption/usagedetails?metric={metric}&amp;api-version=2019-04-01-preview&amp;$filter={filter}
```

Voor meer informatie over {enrollmentId} en {billingPeriodId} raadpleegt u het artikel [gebruiks gegevens-List-](https://docs.microsoft.com/rest/api/consumption/usagedetails/list) API.

Informatie in de volgende tabel over metrische gegevens en filters kan helpen bij het oplossen van veelvoorkomende reserverings problemen.

| **Type API-gegevens** | API-aanroep actie |
| --- | --- |
| **Alle kosten (gebruik en aankoop)** | {Metrisch} vervangen door ActualCost |
| **Gebruik dat de reserverings korting kreeg** | {Metrisch} vervangen door ActualCost<br><br>{Filter} vervangen door: eigenschappen/reservationId% 20ne% 20 |
| **Gebruik waarvoor geen reserverings korting is verkregen** | {Metrisch} vervangen door ActualCost<br><br>{Filter} vervangen door: eigenschappen/reservationId% 20eq% 20 |
| **Afgeschreven kosten (gebruik en aankoop)** | {Metrisch} vervangen door AmortizedCost |
| **Rapport ongebruikte reserverings rapporten** | {Metrisch} vervangen door AmortizedCost<br><br>{Filter} vervangen door: eigenschappen/ChargeType% 20eq% 20 ' UnusedReservation ' |
| **Reserverings aankopen** | {Metrisch} vervangen door ActualCost<br><br>{Filter} vervangen door: eigenschappen/ChargeType% 20eq% 20 ' aankoop '  |
| **Restituties** | {Metrisch} vervangen door ActualCost<br><br>{Filter} vervangen door: eigenschappen/ChargeType% 20eq% 20 ' restitutie ' |

## <a name="download-the-usage-csv-file-with-new-data"></a>Down load het CSV-gebruiks bestand met nieuwe gegevens

Als u een EA-beheerder bent, kunt u het CSV-bestand downloaden dat nieuwe gebruiks gegevens bevat uit Azure Portal. Deze gegevens zijn niet beschikbaar in de [EA-Portal](https://ea.azure.com).

Ga in het Azure Portal naar [Cost Management + billing](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/BillingAccounts).

1. Selecteer het facturerings account.
2. Klik op **gebruik + kosten**.
3. Klik op **Downloaden**.  
![Voor beeld van het downloaden van het bestand met CSV-gebruiks gegevens in de Azure Portal](./media/billing-understand-reserved-instance-usage-ea/portal-download-csv.png)
4. Selecteer in gebruik en **kosten downloaden** onder **gebruiks Details versie 2** **alle kosten (gebruik en inkoop)** en klik vervolgens op downloaden. Herhaal dit voor **afgeschreven kosten (gebruik en inkoop)** .

De CSV-bestanden die u downloadt, bevatten de werkelijke kosten en afgeschreven kosten.

## <a name="common-cost-and-usage-tasks"></a>Algemene kosten-en gebruiks taken

De volgende secties bevatten algemene taken die de meeste mensen gebruiken om de reserverings kosten en gebruiks gegevens weer te geven.

### <a name="get-reservation-purchase-costs"></a>Aanschaf kosten van reserve ring ophalen

Inkoop kosten voor reserve ring zijn beschikbaar in gegevens over de werkelijke kosten. Filter voor _ChargeType = Purchase_. Raadpleeg ProductOrderID om te bepalen welke reserverings order voor de aankoop geldt.

### <a name="get-underutilized-reservation-quantity-and-costs"></a>Gereserveerde reserverings hoeveelheid en kosten voor gebruik

Ontvang afgeschreven kosten gegevens en filter voor _ChargeType_ _= UnusedReservation_. U krijgt de dagelijks ongebruikte reserverings hoeveelheid en de kosten. U kunt de gegevens filteren voor een reserve ring of reserverings order, respectievelijk met de velden _ReservationId_ en _ProductOrderId_ . Als een reserve ring 100% is gebruikt, heeft de record een hoeveelheid van 0.

### <a name="amortize-reservation-costs"></a>Reserverings kosten aflossen

Ontvang afgeschreven kosten gegevens en filter voor een reserverings order met _ProductOrderID_ om dagelijkse afgeschreven kosten voor een reserve ring te verkrijgen.

### <a name="chargeback-for-a-reservation"></a>Terugstorting voor een reserve ring

U kunt reserve ringen terugstortingen naar andere organisaties per abonnement, resource groepen of tags. Afgeschreven kosten gegevens bieden een financiële waarde voor het gebruik van een reserve ring bij de volgende gegevens typen:

- Bronnen (zoals een virtuele machine)
- Resource group
- Labels
- Subscription

### <a name="get-the-blended-rate-for-chargeback"></a>De overvloei factor voor terugstorting ophalen

Als u de overvloei factor wilt bepalen, haalt u de afgeschreven kosten gegevens op en voegt u de totale kosten samen. Voor virtuele machines kunt u de gegevens van de meet waarde of Service type van AdditionalInfo JSON-gegevens gebruiken. De totale kosten delen op basis van het aantal dat is gebruikt om het overvloei percentage op te halen.

### <a name="audit-optimum-reservation-use-for-instance-size-flexibility"></a>Optimale reserverings gebruik controleren voor de flexibiliteit van de instantie grootte

Meerdere hoeveel heden met de _RINormalizationRatio_, van AdditionalInfo. De resultaten geven aan hoeveel uur van het gebruik van de reserve ring is toegepast op de gebruiks record.

### <a name="determine-reservation-savings"></a>Reserverings besparingen bepalen

De afgeschreven kosten gegevens ophalen en de gegevens filteren voor een gereserveerde instantie. Kies

1. Profiteer van de geschatte kosten voor betalen per gebruik. Vermenigvuldig de waarde _UnitPrice_ met _hoeveelheids_ waarden om geschatte betalen per gebruik-kosten te krijgen, als de reserverings korting niet van toepassing is op het gebruik.
2. De reserverings kosten ophalen. Som van de _kosten_ waarden om de financiële waarde op te halen van wat u hebt betaald voor de gereserveerde instantie. Deze bevat de gebruikte en ongebruikte kosten van de reserve ring.
3. Retrekken kosten van de geschatte betalen per gebruik-kosten af om de geschatte besparingen te krijgen.

## <a name="reservation-purchases-and-amortization-in-cost-analysis"></a>Reserverings aankopen en aflossingen in kosten analyse

De reserverings kosten zijn beschikbaar in [kosten analyse](https://aka.ms/costanalysis). Standaard worden in kosten analyse de **werkelijke kosten**weer gegeven. Dit is de manier waarop de kosten op uw factuur worden weer gegeven. Voor het weer geven van de reserverings aankopen die zijn gesplitst en gekoppeld aan de resources die het voor deel hebben gebruikt, schakelt u over naar **afgeschreven kosten**:

![Voor beeld van het selecteren van afgeschreven kosten in kosten analyse](./media/billing-understand-reserved-instance-usage-ea/portal-cost-analysis-amortized-view.png)

Groeperen op kosten type om een uitsplitsing van het gebruik, de aanschaf en de restitutie te bekijken; of door reserve ring voor een uitsplitsing van de reserve ring en kosten op aanvraag. Onthoud de enige reserverings kosten die u zult zien wanneer de werkelijke kosten worden gekocht, maar de kosten worden toegewezen aan de afzonderlijke resources die de benfit hebben gebruikt bij het bekijken van de kosten. U ziet ook een nieuw **UnusedReservation** -kosten type wanneer u de afgeschreven kosten wilt bekijken.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Azure Reservations:

- [Wat zijn Azure Reservations?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserverings korting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Het gebruik van de reserve ring begrijpen voor uw abonnement voor betalen naar gebruik](billing-understand-reserved-instance-usage.md)
- [Windows-software kosten niet inbegrepen bij reserve ringen](billing-reserved-instance-windows-software-costs.md)
