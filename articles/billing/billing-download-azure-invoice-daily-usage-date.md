---
title: Azure-factuur en de dagelijkse gebruiksgegevens downloaden | Microsoft Docs
description: Beschrijft hoe u om te downloaden of uw Azure facturering factuur en de dagelijkse gebruiksgegevens weergeven.
keywords: factuur downloaden van facturen, azure-factuur, gebruik van azure
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: b03b787e245129e5e51304a2bcc8740ebf3f6196
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57406138"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Downloaden of uw Azure-factuur en de dagelijkse gebruiksgegevens weergeven

Voor de meeste abonnementen, kunt u uw factuur op basis van de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) of wordt verzonden in e-mailbericht. Als u een Azure-klant met een Enterprise Agreement (EA-klant), kunt u niet van uw organisatie facturen downloaden. Facturen worden verzonden naar degene die is ingesteld voor het ontvangen van facturen voor de inschrijving.

Als u een EA-klant bent of een [Microsoft KLANTOVEREENKOMST](#check-access-to-a-microsoft-customer-agreement), kunt u gebruik in de [Azure-portal](https://portal.azure.com/). Voor andere abonnementen, gaat u naar de [Azure-Accountcentrum](https://account.azure.com/Subscriptions) op gebruiksgegevens downloaden.

Alleen bepaalde functies zijn gemachtigd te factuurgegevens ophalen factuur- en gebruiksgegevens, zoals de accountbeheerder of Enterprise-beheerder. Zie [Toegang tot factureringsgegevens voor Azure beheren](billing-manage-access.md) voor meer informatie over het verkrijgen van toegang tot factureringsgegevens.

Als u hebt een [Microsoft KLANTOVEREENKOMST](#Check-your-access-to-a-Microsoft-Customer-Agreement), u moet een factureringsprofiel eigenaar, Inzender, lezer, of factuur manager om gegevens over facturering en gebruik weer te geven. Zie voor meer informatie over facturering rollen voor Microsoft-klant-overeenkomsten, [facturering profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>Uw Azure-facturen (PDF) downloaden

Voor de meeste abonnementen, kunt u uw factuur downloaden vanuit de Azure-portal. Als u een KLANTOVEREENKOMST van Microsoft hebt, raadpleegt u [downloaden van facturen voor een profiel voor facturering](#download-invoices-for-a-billing-profile).

### <a name="download-invoices-for-an-individual-subscription"></a>Downloaden van facturen voor een afzonderlijk abonnement

1. Selecteer uw abonnement uit de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in de Azure portal als [een gebruiker met toegang tot facturen](billing-manage-access.md).

2. Selecteer **facturen**.

    ![Schermafbeelding van de facturerings- en gebruik de optie](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Klik op **factuur downloaden** om een kopie van uw factuur PDF-bestand weer te geven. De status **niet beschikbaar**, Zie [waarom zie ik geen een factuur voor de afgelopen betalingsperiode?](#noinvoice)

    ![Schermafbeelding van factureringsperioden, de opties voor het downloaden en de totale kosten voor elke factureringsperiode](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. U kunt ook het gebruik van uw dagelijkse weergeven door te klikken op de factureringsperiode.

Zie voor meer informatie over uw factuur [meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md). Zie voor meer informatie over het beheren van uw kosten, [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Downloaden van facturen voor een Microsoft-KLANTOVEREENKOMST

Facturen worden gegenereerd voor elke [facturering profiel](billing-mca-overview.md#understand-billing-profiles) in de Microsoft-overeenkomst voor de klant. U moet een factureringsprofiel eigenaar, Inzender, lezer, of factuur manager voor het downloaden van facturen van de Azure-portal. 

1. Zoeken op **kosten Management en facturering**.
2. Selecteer een profiel voor facturering.
3. Selecteer **facturen**.
4. In het raster factuur vindt u de rij van de factuur die u wilt downloaden.
5. Klik op het weglatingsteken (`...`) aan het einde van de rij.
6. Selecteer in het contextmenu downloaden **factuur**.

Als er geen een factuur voor de afgelopen betalingsperiode, Zie **aanvullende informatie**. <!-- Fix this -->
### <a name="noinvoice"></a> Waarom zie ik een factuur niet voor de afgelopen betalingsperiode?

Er kunnen verschillende redenen voor zijn dat u geen factuur ziet:

- Het is minder dan 30 dagen geleden dat u zich geabonneerd hebt op Azure.

- De factuur is nog niet gegenereerd. Wacht tot het einde van de factureringsperiode.

- U bent niet gemachtigd om facturen te bekijken. Als u een KLANTOVEREENKOMST van Microsoft hebt, moet u de facturering profiel zijn eigenaar, Inzender, lezer, of de manager factuur. Voor andere abonnementen, kunnen er geen oude facturen als u de accountbeheerder niet. Zie [Toegang tot factureringsgegevens voor Azure beheren](billing-manage-access.md) voor meer informatie over het verkrijgen van toegang tot factureringsgegevens.

- Als u een gratis proefversie of een maandelijks krediet met het abonnement dat u niet overschrijdt hebt, krijgen u geen een factuur, tenzij u een Microsoft-KLANTOVEREENKOMST hebt.

## <a name="get-your-invoice-in-email-pdf"></a>Uw factuur in e-mailbericht (PDF) niet ophalen

U kunt ervoor kiezen en configureren van extra ontvangers voor het ontvangen van uw Azure facturen in een e-mailbericht. Deze functie mogelijk niet beschikbaar voor bepaalde abonnementen, zoals ondersteuning biedt, Enterprise Agreements of Azure in Open. Als u een overeenkomst met Microsoft Customer hebt, raadpleegt u [uw facturen profiel ophalen in e-mailbericht](#get-your-billing-profile-invoices-in-email).

### <a name="get-your-subscriptions-invoices-in-email"></a>Ophalen van uw abonnement facturen in e-mailbericht

1. Selecteer uw abonnement uit de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). U meldt zich voor elk abonnement dat u de eigenaar bent. Klik op **facturen** vervolgens **e-mijn factuur**.

    ![Schermafbeelding van de stroom opt-in](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Klik op **Opt-in** en accepteer de voorwaarden.

    ![Schermafbeelding van de stroom opt-in voor-stap 2](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Wanneer u de overeenkomst hebt geaccepteerd, kunt u extra ontvangers configureren. Als een geadresseerde wordt verwijderd, wordt het e-mailadres niet meer opgeslagen. Als u van gedachten verandert, moet u deze opnieuw toevoegen.

    ![Schermafbeelding van de stroom opt-in voor-stap 3](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Als u niet een e-mail ontvangen nadat de stappen hebt uitgevoerd, controleert u of uw e-mailadres klopt in de [communicatievoorkeuren in uw profiel](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Afmelden voor facturen van uw abonnement ophalen in e-mailbericht

U kunt uw factuur per e-mail aan met de volgende stappen hierboven en klik op Afmelden **afmelden voor facturen per e-mail**. Met deze optie worden alle e-mailadressen verwijderd die zijn ingesteld voor het ontvangen van facturen via e-mail. U kunt de ontvangers opnieuw te configureren als u zich opnieuw aan.

 ![Schermafbeelding van de stroom opt-out](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Ophalen van uw Microsoft-KLANTOVEREENKOMST facturen in e-mailbericht

Als u een KLANTOVEREENKOMST van Microsoft hebt, kunt u meldt zich aan uw factuur in een e-mailbericht niet ophalen. Alle facturering profiel eigenaren, bijdragers lezers en factuur managers de factuur wordt ontvangen via e-mail. Lezers bijwerken niet de voorkeur van de factuur e-mailadres. 

1. Zoeken op **kosten Management en facturering**.
1. Selecteer een profiel voor facturering.
1. Onder **instellingen**, selecteer **eigenschappen**.
1. Onder **e-Mailfactuur**, selecteer **Update e factuur voorkeur**.
1. Selecteer **Opt-in**.
1. Klik op **Update**.

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>Afmelden voor uw facturen profiel ophalen in e-mailbericht

U kunt uw factuur per e-mail aan met de volgende stappen hierboven en klik op Afmelden **Opt-out**. Alle eigenaren, bijdragers, lezers en factuur managers zal worden uitgeschreven voor het ophalen van de factuur via e-mail, te. Als u een lezer, kunt u de voorkeur van de factuur e-mailbericht niet wijzigen.

## <a name="download-usage"></a>Gebruiksgegevens downloaden

 Voor de meeste abonnementen, zoek het dagelijkse gebruiksbestand in de [Azure-Accountcentrum](https://account.azure.com/Subscriptions). Als u een EA-klant bent of een KLANTOVEREENKOMST van Microsoft hebt, kunt u downloaden gebruik in de [Azure-portal](https://portal.azure.com/). <!--te doen: betalen per gebruik-ervaring bijwerken naar Ibiza verschijning >

### <a name="download-usage-from-the-account-center-csv"></a>Gebruiksgegevens downloaden via het Accountcentrum (.csv)

1. Meld u aan bij de [Azure-Accountcentrum](https://account.windowsazure.com/subscriptions) als de accountbeheerder.

2. Selecteer het abonnement waarvan u de factuur- en gebruiksgegevens informatie wilt.

3. Selecteer **FACTURERINGSGESCHIEDENIS**.

    ![Schermafbeelding van de optie factureren geschiedenis](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. U ziet uw overzichten voor de afgelopen zes factureringsperioden en de huidige niet-gefactureerde periode weergegeven.

    ![Schermafbeelding van factureringsperioden, opties voor het downloaden van de factuur en dagelijkse gebruik en de totale kosten voor elke factureringsperiode](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Selecteer **Huidig overzicht weergeven** om een schatting van uw kosten te zien voor het moment waarop de schatting wordt gegenereerd. Deze informatie wordt dagelijks bijgewerkt en omvat mogelijk niet alle uw gebruik. Uw maandelijkse factuur kan afwijken van deze schatting.

    ![Schermafbeelding van de optie Huidig overzicht weergeven](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Schermafbeelding van de schatting van de huidige kosten](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Selecteer **Gebruiksgegevens downloaden** om de dagelijkse gebruiksgegevens te downloaden als een CSV-bestand. Als er twee versies beschikbaar zijn, moet u versie 2 downloaden.

    ![Schermafbeelding van de optie gebruiksgegevens downloaden](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Alleen de accountbeheerder kan toegang tot het Azure-Accountcentrum. Andere facturering beheerders, zoals de eigenaar van een krijgt bij het gebruik van informatie over het gebruik de [facturering-API's](billing-usage-rate-card-overview.md).

Zie [Meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md) voor meer informatie over uw dagelijkse gebruik. Zie voor meer informatie over het beheren van uw kosten, [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md).

### <a name="download-usage-for-ea-customers"></a>Gebruiksgegevens downloaden voor EA-klanten

Als u wilt weergeven en downloaden van gegevens over gebruik als een EA-klant bent, moet u een Enterprise-beheerder, eigenaar van Account of afdeling beheerder met de weergave in rekening gebracht beleid is ingeschakeld.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken naar *kosten Management en facturering*.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Selecteer **gebruik en kosten**.
1. Voor de maand die u wilt downloaden, selecteert u **downloaden**.

### <a name="download-usage-for-your-microsoft-customer-agreement"></a>Gebruik voor uw Microsoft-KLANTOVEREENKOMST downloaden

Als u wilt weergeven en downloaden van gegevens over gebruik voor een profiel voor facturering, moet u een factureringsprofiel eigenaar, Inzender, lezer, of factuur manager.

#### <a name="download-usage-for-billed-charges"></a>Gebruiksgegevens voor gefactureerde kosten voor downloaden

1. Zoeken op **kosten Management en facturering**.
2. Selecteer een profiel voor facturering.
3. Selecteer **facturen**.
4. In het raster factuur vindt u de rij van de factuur overeenkomt met het gebruik dat u wilt downloaden.
5. Klik op het weglatingsteken (`...`) aan het einde van de rij.
6. Selecteer in het contextmenu downloaden **kosten en het gebruik van Azure**.

#### <a name="download-usage-for-open-charges"></a>Gebruiksgegevens voor open kosten downloaden

U kunt het gebruik van de maand tot heden ook downloaden voor de huidige factureringsperiode, wat betekent dat de kosten zijn nog niet gefactureerd.

1. Zoeken op **kosten Management en facturering**.
2. Selecteer een profiel voor facturering.
3. In de **overzicht** blade, klikt u op **gebruik van Azure downloaden en de kosten**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over uw factuur en de kosten in rekening gebracht:

- [Meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md)
- [Meer informatie over uw Azure-factuur](billing-understand-your-invoice.md)
- [Meer informatie over uw Microsoft Azure gedetailleerd gebruik](billing-understand-your-usage.md)
- [Bekijk de prijzen van Azure van uw organisatie](billing-ea-pricing.md)

Als u een Microsoft-KLANTOVEREENKOMST hebt, Zie:

- [Meer informatie over de kosten op de factuur voor uw facturering profiel ](billing-mca-understand-your-bill.md)
- [Meer informatie over de factuur voor uw facturering profiel](billing-mca-understand-your-invoice.md)
- [Inzicht in het Azure bestand voor gebruik en de kosten voor uw facturering profiel](billing-mca-understand-your-usage.md)
- [Weergeven en btw-documenten voor uw facturering profiel downloaden](billing-mca-download-tax-document.md)
- [Bekijk de prijzen van Azure van uw organisatie](billing-ea-pricing.md)
