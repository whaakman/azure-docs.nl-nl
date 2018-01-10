---
title: Inzicht in uw factuur voor Azure
description: Meer informatie over het gelezen en begrepen gebruiks- en factuur voor uw Azure-abonnement
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.openlocfilehash: be15c74456b0cec64455f03dd72b8b64eef2bd5d
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Meer informatie over uw factuur voor Microsoft Azure
Voor inzicht in uw Azure-factuur, Vergelijk uw factuur met de gedetailleerde dagelijks gebruik-bestand en de kosten-rapporten in de Azure portal.

>[!NOTE]
>In dit artikel geldt niet voor klanten met Enterprise Agreement (EA). Als u een klant EA [factuur documentatie vindt u in de Enterprise Portal.](https://ea.azure.com/helpdocs/understandingYourInvoice)  

Als u een PDF-bestand van uw factuur en een kopie van het gedetailleerde dagelijks gebruik bestand CSV downloaden, Zie [ophalen van uw Azure facturering facturen en dagelijks gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md). 

Zie voor gedetailleerde voorwaarden en beschrijvingen van uw factuur en gedetailleerde dagelijks gebruik bestand [begrijpen voorwaarden op uw Microsoft Azure-factuur](billing-understand-your-invoice.md) en [Understand termen in uw Microsoft Azure gedetailleerde informatie over het gebruik](billing-understand-your-usage.md). 

Zie voor meer informatie over de kosten-rapporten, [Azure-portal kostenbeheer](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="charges"></a>Hoe maak ik ervoor dat de kosten in mijn factuur correct zijn?

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Als er een kosten op uw factuur die u op meer informatie wilt, moet u er een aantal opties zijn.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Optie 1: Uw factuur en het gebruik en de kosten te vergelijken met het CSV-bestand voor gedetailleerde informatie over het gebruik

Het gebruik van gedetailleerde CSV-bestand bevat uw kosten per factureringsperiode en het dagelijks gebruik. Als u uw CSV-bestand voor gedetailleerde informatie over het gebruik, Zie [ophalen van uw Azure facturering facturen en dagelijks gebruiksgegevens](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date).

Uw gebruikskosten worden weergegeven op het niveau van de meter. De volgende termen betekenis dezelfde in zowel de factuur als het bestand voor gedetailleerde informatie over het gebruik. Bijvoorbeeld, is de factureringscyclus op de factuur gelijk aan de factureringsperiode die wordt weergegeven in het bestand voor gedetailleerde informatie over het gebruik.

 | Factuur (PDF) | Gedetailleerde informatie over het gebruik (CSV)|
 | --- | --- |
|Factureringscyclus | Factureringsperiode |
 |Naam |De categorie van de meter |
 |Type |Meter subcategorie |
 |Resource |De naam van de meter |
 |Regio |De regio van de meter |
 |Verbruikt |Verbruikt aantal |
 |Inbegrepen |Inbegrepen hoeveelheid |
 |Factureerbaar |Overschreden hoeveelheid |

De **gebruikskosten** gedeelte van uw factuur is de totale waarde voor elke meter die is gebruikt tijdens de factureringsperiode. De volgende schermafbeelding ziet u bijvoorbeeld een usage-kosten voor de Azure Scheduler-service.

![Factuur gebruikskosten](./media/billing-understand-your-bill/1.png)

De **instructie** sectie van de gedetailleerde informatie over het gebruik CSV wordt dezelfde kosten. Zowel de *verbruikt* bedrag en *waarde* overeenkomen met de factuur.

![CSV-gebruikskosten](./media/billing-understand-your-bill/2.png)

Een verdeling van deze kosten per dag, Ga naar de **dagelijkse gebruik** sectie van de CSV. Filteren op 'Scheduler' onder *Meter categorie* en ziet u welke dagen de meter is gebruikt en hoeveel is verbruikt. De *Resource* en *resourcegroep* informatie is ook vermeld voor vergelijking. De *verbruikt* waarden tot wat wordt weergegeven op de factuur moeten toevoegen.

![De sectie dagelijks gebruik van CSV](./media/billing-understand-your-bill/3.png)

Als u de kosten per dag, Vermenigvuldig de *verbruikt* bedragen met de *snelheid* waarde uit de **instructie** sectie.

Zie voor meer informatie over de factuur, [inzicht in uw Azure-factuur](billing-understand-your-invoice.md).

Zie voor meer informatie over elk van de kolommen in de CSV, [inzicht in het gebruik van uw Azure gedetailleerde](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Optie 2: Uw factuur en vergelijken met het gebruik en de kosten in de Azure-portal

De Azure-portal kunt u controleren of uw kosten. De Azure portal biedt kosten management grafieken voor een snel overzicht van uw gebruik en de kosten op uw factuur.

Als u wilt doorgaan met het voorbeeld hierboven, gaat u naar de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecteer uw abonnement en kies vervolgens **analysis kosten**. U kunt daar de tijdsspanne opgeven en Zie gebruik kosten voor de Azure Scheduler-service.

![Analyse van kosten weergeven in Azure-portal](./media/billing-understand-your-bill/4.png)

Om te zien van de dag kosten opgesplitst in **kosten geschiedenis**, klik op de rij.

![Kosten geschiedenis-weergave in Azure-portal](./media/billing-understand-your-bill/5.png)

Zie voor meer informatie, [te voorkomen dat onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md#costs).

## <a name="external"></a>Hoe zit het met externe servicekosten?
Externe services (ook wel bekend als Azure Marketplace orders) worden geleverd door onafhankelijke serviceleveranciers en worden afzonderlijk gefactureerd. De kosten worden niet weergegeven op uw Azure-factuur. Zie voor meer informatie, [inzicht in uw Azure externe servicekosten](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Hoe maak ik een betaling?

Als u een creditcard of een betaalpas als uw betalingsmethode instellen, wordt de betaling betalen automatisch binnen tien dagen na afloop van de factureringsperiode. Op uw creditcard-instructie het regelitem zou spreken **MSFT Azure**.

Als u [betaalde door facturering](billing-how-to-pay-by-invoice.md), uw betaling naar de locatie die onder van uw factuur verzenden. Voor meer informatie [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Hoe kan ik de status van een betaling van creditcard controleren?

[Maak een ondersteuningsticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om de status van uw betaling te vragen. 

## <a name="tips-for-cost-management"></a>Tips voor het kostenbeheer van
- Kosten schatten met behulp van de [prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/) en [totale kosten van eigendom Rekenmachine](https://aka.ms/azure-tco-calculator), en de [gedetailleerde prijsinformatie voor elke service](https://azure.microsoft.com/en-us/pricing/).
- [Instellen van waarschuwingen facturering](billing-set-up-alerts.md).
- [Controleer de informatie over het gebruik en kosten regelmatig in de Azure-portal](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
