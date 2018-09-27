---
title: Azure-factuur en de dagelijkse gebruiksgegevens downloaden | Microsoft Docs
description: Beschrijft hoe u om te downloaden of uw Azure facturering factuur en de dagelijkse gebruiksgegevens weergeven.
keywords: factuur downloaden van facturen, azure-factuur, gebruik van azure
services: billing
documentationcenter: ''
author: genlin
manager: tonguyen
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: f0cdfef50c07674a08766933f2f7edfc946462a4
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395012"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Downloaden of uw Azure-factuur en de dagelijkse gebruiksgegevens weergeven
U kunt uw factuur downloaden de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) of wordt verzonden in e-mailbericht. Als u wilt uw dagelijkse gebruiksgegevens downloaden, gaat u naar de [Azure-Accountcentrum](https://account.azure.com/Subscriptions). Alleen bepaalde functies gemachtigd om facturering factuur en gebruiksgegevens, zoals de accountbeheerder te verkrijgen. Zie [Toegang tot factureringsgegevens voor Azure beheren](billing-manage-access.md) voor meer informatie over het verkrijgen van toegang tot factureringsgegevens.

In dit artikel is niet van toepassing op Enterprise Agreement (EA)-klanten. Als u een EA-klant bent, worden uw facturen rechtstreeks naar de beheerders van de inschrijving verzonden.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

> [!div class="nextstepaction"]
> [Help bij het verbeteren van documenten over Azure-facturering](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="get-your-invoice-in-email-pdf"></a>Uw factuur in e-mailbericht (PDF) niet ophalen
U kunt ervoor kiezen en configureren van extra ontvangers voor het ontvangen van uw Azure facturen in een e-mailbericht. Deze functie mogelijk niet beschikbaar voor bepaalde abonnementen, zoals ondersteuning biedt, Enterprise Agreements of Azure in Open.

1. Selecteer uw abonnement uit de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Aanmelden voor elk abonnement dat u de eigenaar bent. Klik op **facturen** vervolgens **e-mijn factuur**. 

    ![Schermafbeelding van de stroom opt-in](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Klik op **Opt-in** en accepteer de voorwaarden.

    ![Schermafbeelding van de stroom opt-in voor-stap 2](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Wanneer u de overeenkomst hebt geaccepteerd, kunt u extra ontvangers configureren. Als een geadresseerde wordt verwijderd, wordt het e-mailadres niet meer opgeslagen. Als u van gedachten verandert, moet u deze opnieuw toevoegen.

    ![Schermafbeelding van de stroom opt-in voor-stap 3](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Als u niet een e-mail ontvangen nadat de stappen hebt uitgevoerd, controleert u of uw e-mailadres klopt in de [communicatievoorkeuren in uw profiel](https://account.windowsazure.com/profile).

### <a name="opt-out-from-getting-your-invoice-in-email"></a>Opt-out van het ontvangen van uw factuur in e-mailbericht
Als u niet wilt ophalen van uw factuur in e-mailbericht, klikt u op Opt uit van facturen per e-mail. Hiermee verwijdert u alle e-mailadressen die is ingesteld op facturen in e-mailbericht ontvangt. Als u ervoor kiezen moeten back in uw ontvangers configureren.

 ![Schermafbeelding van de stroom opt-out](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

## <a name="download-invoice-from-azure-portal-pdf"></a>Factuur downloaden via Azure portal (PDF)

1. Selecteer uw abonnement uit de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal als [een gebruiker met toegang tot facturen](billing-manage-access.md).

2. Selecteer **facturen**. 

    ![Schermafbeelding van de facturerings- en gebruik de optie](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Klik op **factuur downloaden** om een kopie van uw factuur PDF-bestand weer te geven. De status **niet beschikbaar**, Zie [waarom zie ik geen een factuur voor de afgelopen betalingsperiode?](#noinvoice)

    ![Schermafbeelding van factureringsperioden, de opties voor het downloaden en de totale kosten voor elke factureringsperiode](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. U kunt ook het gebruik van uw dagelijkse weergeven door te klikken op de factureringsperiode. 

Zie voor meer informatie over uw factuur [meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md). Zie voor meer informatie over het beheren van kosten, [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md).

## <a name="download-usage-from-the-account-center-csv"></a>Gebruiksgegevens downloaden via het Accountcentrum (.csv)

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

Zie [Meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md) voor meer informatie over uw dagelijkse gebruik. Zie voor meer informatie over het beheren van kosten, [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md).

## <a name="noinvoice"></a> Waarom zie ik een factuur niet voor de afgelopen betalingsperiode?

Er zijn verschillende redenen dat er geen een factuur:

- U hebt een maandelijks krediet aan uw abonnement die u niet langer zijn dan of u hebt een gratis proefversie. Een factuur wordt alleen gegenereerd wanneer u geld verschuldigd.

- Het is minder dan 30 dagen vanaf de dag die u geabonneerd op Azure.

- De factuur niet is nog gegenereerd. Wachten tot het einde van de factureringsperiode.

- Als u niet de accountbeheerder bent, oudere facturen mogelijk niet beschikbaar voor u.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

