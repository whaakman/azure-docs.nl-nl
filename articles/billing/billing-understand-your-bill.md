---
title: Meer informatie over uw factuur voor Azure | Microsoft Docs
description: Meer informatie over het lezen en informatie over het gebruik en de factuur voor uw Azure-abonnement
services: ''
documentationcenter: ''
author: tonguyen10
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2018
ms.author: cwatson
ms.openlocfilehash: f7ec113a7fa51fc6d3684c2e7ee9379bd1534e30
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392405"
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Meer informatie over uw factuur voor Microsoft Azure
Voor meer informatie over uw Azure-factuur, vergelijken met uw factuur met de gedetailleerde dagelijks gebruik van bestands- en -rapporten voor de kosten in de Azure-portal.

>[!NOTE]
>In dit artikel is niet van toepassing op Enterprise Agreement (EA)-klanten. Als u een EA-klant bent [factuur documentatie vindt u in de Enterprise Portal.](https://ea.azure.com/helpdocs/understandingYourInvoice)  

Als u wilt een PDF-bestand van uw factuur en een kopie van uw gedetailleerde dagelijks gebruik van CSV downloaden van bestanden, Zie [ophalen van uw Azure-factuur en de dagelijkse gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md). 

Zie voor gedetailleerde voorwaarden en beschrijvingen van uw factuur en gedetailleerde dagelijkse gebruiksbestand [meer informatie over uw Microsoft Azure-factuur](billing-understand-your-invoice.md) en [begrijpen voorwaarden op uw Microsoft Azure gedetailleerde gebruik](billing-understand-your-usage.md). 

Zie voor meer informatie over de kostenbeheerrapporten, [kostenbeheer van Azure portal](https://docs.microsoft.com/azure/billing/billing-getting-started).

> [!div class="nextstepaction"]
> [Help bij het verbeteren van documenten over Azure-facturering](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="charges"></a>Hoe ik ervoor zorgen dat de kosten in rekening gebracht op mijn factuur correct zijn?

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Als er een kosten in rekening gebracht op uw factuur die u op meer informatie wilt, moet u er een aantal opties zijn.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Optie 1: Uw factuur bekijken en vergelijken van het gebruik en kosten met het gedetailleerde gebruik CSV-bestand

Het gedetailleerde gebruik CSV-bestand bevat uw kosten per factureringsperiode en het dagelijks gebruik. Als u uw gedetailleerde gebruik CSV-bestand, raadpleegt u [ophalen van uw Azure-factuur en de dagelijkse gebruiksgegevens](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date).

De gebruikskosten worden weergegeven op het niveau van de meter. De volgende termen betekenen hetzelfde te doen in de factuur en het gedetailleerde gebruik-bestand. Bijvoorbeeld, is de factureringscyclus op de factuur gelijk aan de factureringsperiode weergegeven in het bestand gedetailleerd gebruik.

 | Factuur (PDF-bestand) | Gedetailleerd gebruik (CSV)|
 | --- | --- |
|Factureringscyclus | Factureringsperiode |
 |Naam |De categorie van de meter |
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

Als u wilt een uitsplitsing van deze kosten in rekening gebracht op basis van de dagelijkse ziet, gaat u naar de **dagelijks gebruik** sectie van de CSV. Filteren op 'Planner' onder *Metercategorie* en ziet u welke dagen de meter is gebruikt en hoeveel is verbruikt. De *Resource* en *resourcegroep* informatie wordt ook weergegeven voor de vergelijking. De *verbruikt* waarden moeten oplopen tot wat wordt weergegeven op de factuur.

![De sectie dagelijks gebruik in de CSV](./media/billing-understand-your-bill/3.png)

Als u de kosten per dag, vermenigvuldigt u de *verbruikt* bedragen met de *tarief* waarde uit de **instructie** sectie.

Zie voor meer informatie over de factuur, [inzicht in uw Azure-factuur](billing-understand-your-invoice.md).

Zie voor meer informatie over elk van de kolommen in de CSV, [inzicht in uw gedetailleerde gebruik van Azure](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Optie 2: Uw factuur bekijken en vergelijken met het gebruik en kosten in de Azure-portal

De Azure-portal kunt u controleren of uw kosten in rekening gebracht. De Azure portal biedt kostengrafieken management voor een snel overzicht van uw gebruik en de kosten op uw factuur.

Als u wilt doorgaan met het voorbeeld hierboven, gaat u naar de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecteer uw abonnement en kies vervolgens **Cost analysis**. U kunt daar de tijdspanne opgeven en Zie gebruik kosten in rekening gebracht voor de Azure Scheduler-service.

![Kostenanalyse weergave in Azure portal](./media/billing-understand-your-bill/4.png)

Om te zien van de dagelijkse kostenanalyse in **kostenoverzicht**, klikt u op de rij.

![Kostengeschiedenis weergave in Azure portal](./media/billing-understand-your-bill/5.png)

Zie voor meer informatie, [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md#costs).

## <a name="external"></a>Hoe zit het met externe servicekosten?
Externe services (ook wel bekend als de Azure Marketplace orders) worden geleverd door een onafhankelijke serviceleveranciers en worden afzonderlijk gefactureerd. De kosten worden niet weergegeven op uw Azure-factuur. Zie voor meer informatie, [meer informatie over uw Azure externe servicekosten](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Hoe maak ik een betaling?

Als u een creditcard of een betaalpas als uw betalingswijze instellen, de betaling wordt in rekening gebracht automatisch binnen 10 dagen na afloop van de factureringsperiode. Op uw creditcard worden vermeld, het regelitem Denk **MSFT Azure**.

Als u [betalen per facturering](billing-how-to-pay-by-invoice.md), verzenden de betaling naar de locatie aan de onderkant van uw factuur weergegeven. Voor meer informatie [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Hoe kan ik de status van een betaling per creditcard controleren?

[Maak een ondersteuningsticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om de status van de betaling te vragen. 

## <a name="are-there-different-azure-customer-types-how-do-i-know-what-customer-type-i-am"></a>Zijn er verschillende Azure-klant typen? Hoe weet ik welke ik ben klanttype?
Er zijn verschillende soorten Azure-klanten. Zie voor meer informatie over de prijzen en de factuur, de volgende beschrijvingen van de klant-type.

- **Enterprise**: Enterprise-klanten zich hebben aangemeld voor een Enterprise Agreement van Azure om te maken in de onderhandelde monetaire toezeggingen en toegang krijgen tot aangepaste prijzen voor Azure-resources.
- **Web-Direct**: Web Direct-klanten niet alle aangepaste overeenkomst met Azure hebben ondertekend. Deze klanten voor Azure hebben geregistreerd via azure.com en ontvangen van openbare gerichte prijzen voor alle Azure-resources.
- **Cloud Service Provider**: Cloud-serviceproviders zijn doorgaans bedrijven die door een eindklant hebben ingehuurd om oplossingen op basis van Azure te ontwikkelen.

## <a name="why-dont-i-see-the-cost-the-resource-i-have-created-in-my-bill"></a>Waarom zie ik niet de kosten voor de resource die ik in mijn factuur gemaakt heb?
Azure voert geen factuur rechtstreeks op basis van resourcekosten. Facturering wordt op basis van uit een of meer meters die worden gebruikt voor het bijhouden van gebruik tijdens de levensduur van een resource gedaan. Deze meters worden vervolgens gebruikt voor het berekenen van de factuur. Zie meer informatie over Azure softwarelicentiecontrole hieronder.

## <a name="how-does-azure-charge-metering-work"></a>Hoe Azure kosten in rekening gebracht softwarelicentiecontrole werk?
Wanneer u een enkel Azure-resource, zoals een virtuele machine instellen is er een of meerdere meter exemplaren ook worden gemaakt. Deze meters worden gebruikt voor het gebruik van de resource gedurende een periode bijhouden. Elke meter verzendt gebruiksrecords die worden gebruikt door Azure in onze meetsysteem kosten voor het berekenen van de factuur. 

Een enkele virtuele machine gemaakt in Azure kan bijvoorbeeld de volgende meters gemaakt voor het bijhouden van het gebruik ervan:

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

Nadat de virtuele machine is gemaakt, begint een van de bovenstaande meters gebruiksrecords genereren. Dit gebruik wordt vervolgens in meetsysteem samen met de prijs van de meters van Azure worden gebruikt om te bepalen hoeveel een klant in rekening gebracht.

> [!Note]
> Het bovenstaande voorbeeld-meters mogelijk alleen een subset van de meters die zijn gemaakt van een virtuele machine die wordt gemaakt.

## <a name="what-is-the-difference-between-azure-1st-party-charges-and-azure-marketplace-charges"></a>Wat is het verschil tussen Azure 1e kosten voor derden en kosten voor Azure Marketplace?
Azure 1e kosten voor derden zijn voor resources die rechtstreeks zijn ontwikkeld en die worden aangeboden door Azure. Azure Marketplace-kosten zijn voor resources die zijn gemaakt door externe leveranciers die beschikbaar voor gebruik via de Azure marketplace zijn. Een Firewall Barracuda is bijvoorbeeld een Azure marketplace-resource die worden aangeboden door een derde partij. Alle kosten voor de firewall en de bijbehorende meters weergegeven als marketplace-kosten. 

## <a name="tips-for-cost-management"></a>Tips voor kostenbeheer
- Schat de kosten met behulp van de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) en [totale kosten van eigendom Rekenmachine](https://aka.ms/azure-tco-calculator), en krijg de [gedetailleerde prijsinformatie voor elke service](https://azure.microsoft.com/pricing/).
- [Meldingen voor facturering instellen](billing-set-up-alerts.md).
- [Uw gebruik en kosten regelmatig in Azure portal bekijken](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
