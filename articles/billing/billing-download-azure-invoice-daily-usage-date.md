---
title: Azure-facturering facturen en dagelijks gebruiksgegevens downloaden | Microsoft Docs
description: Beschrijft hoe downloaden of weergeven van uw Azure facturen en dagelijks gebruik factureringsgegevens.
keywords: factuur, factuur downloaden, azure factuur, azure gebruik
services: 
documentationcenter: 
author: genlin
manager: tonguyen
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/26/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eea55735d0e17de4fe543847d0d521b0e8c0c3f7
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Downloaden of uw Azure-factuur en de dagelijkse gebruiksgegevens weergeven
U kunt downloaden via uw factuur van de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) of wordt verzonden in e-mailbericht. Het gebruik van uw dagelijkse downloaden, gaat u naar de [Azure-Accountcentrum](https://account.azure.com/Subscriptions). Alleen bepaalde functies gemachtigd factuur en informatie over het gebruik, zoals de accountbeheerder. Zie voor meer informatie over het verkrijgen van toegang tot factureringsgegevens, [beheren van toegang tot Azure-facturering rollen](billing-manage-access.md).

>[!NOTE]
>In dit artikel geldt niet voor klanten met Enterprise Agreement (EA). Als u een EA-klant bent, worden uw facturen rechtstreeks naar de beheerders inschrijving verzonden.

## <a name="get-your-invoice-in-email-pdf"></a>Uw factuur ophalen in e-mailbericht (PDF)
U kunt aanmelden en configureren van extra ontvangers voor het ontvangen van uw Azure factuur in een e-mailbericht. Deze functie is mogelijk niet beschikbaar voor bepaalde abonnementen zoals ondersteuning biedt, Enterprise overeenkomsten of Azure in Open.

1. Selecteer uw abonnement uit de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Aanmelden voor elk abonnement dat u bezit. Klik op **facturen** vervolgens **e-mijn factuur**. 

    ![Schermafbeelding van de stroom opt-in](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Klik op **Opt-in** en accepteer de voorwaarden.

    ![Schermafbeelding van de stroom opt-in](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Nadat u de overeenkomst hebt geaccepteerd, kunt u extra ontvangers configureren.

    ![Schermafbeelding van de stroom opt-in](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Als u een e-mailbericht geen krijgt nadat de stappen hebt uitgevoerd, controleert u of uw e-mailadres klopt de [communicatievoorkeuren in uw profiel](https://account.windowsazure.com/profile).

## <a name="download-invoice-from-azure-portal-pdf"></a>Facturen downloaden vanuit Azure-portal (PDF)

1. Selecteer uw abonnement uit de [pagina abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure-portal als [een gebruiker met toegang tot facturen](billing-manage-access.md).

2. Selecteer **facturen**. 

    ![Schermafbeelding van de optie facturering en gebruik](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Klik op **factuur downloaden** om een kopie van uw factuur PDF weer te geven. Als er op het **niet beschikbaar**, Zie [waarom zie ik niet een factuur voor de laatste factureringsperiode?](#noinvoice)

    ![Schermafbeelding van facturering perioden, de opties voor het downloaden en de totale kosten voor elke factureringsperiode](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. U kunt ook het gebruik van uw dagelijkse weergeven door te klikken op de factureringsperiode. 

Zie voor meer informatie over uw factuur [inzicht in uw factuur voor Microsoft Azure](billing-understand-your-bill.md). Zie voor meer informatie over het beheren van kosten [te voorkomen dat onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md).

## <a name="download-usage-from-the-account-center-csv"></a>Informatie over het gebruik van de Account Center (.csv) downloaden

1. Meld u aan bij de [Azure-Accountcentrum](https://account.windowsazure.com/subscriptions) als accountbeheerder.

2. Selecteer het abonnement waarvoor u de informatie over facturen en gebruiksgegevens wilt.

3. Selecteer **facturering geschiedenis**. 

    ![Schermafbeelding van de optie factureren geschiedenis](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. U ziet de instructies voor de laatste zes facturering perioden en de huidige gefactureerd periode. 

    ![Schermafbeelding van facturering perioden, opties voor het downloaden van factuur en de dagelijkse gebruik en de totale kosten voor elke factureringsperiode](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Selecteer **weergave huidige instructie** om te zien van een schatting maken van uw kosten op het moment dat de schatting is gegenereerd. Deze informatie wordt alleen dagelijks bijgewerkt en kan niet alle gebruik van uw bevatten. Uw maandelijkse factuur kan afwijken van deze schatting.

    ![Schermafbeelding van de optie voor de huidige instructie weergeven](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Schermafbeelding van de schatting van de huidige kosten](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Selecteer **gebruiksgegevens downloaden** voor het downloaden van de dagelijkse gebruiksgegevens als een CSV-bestand. Als er twee versies beschikbaar zijn, wordt versie 2 downloaden.

    ![Schermafbeelding van de optie gebruiksgegevens downloaden](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Alleen de accountbeheerder toegang tot het Azure-Accountcentrum. Andere facturering beheerders, zoals een eigenaar krijgt bij het gebruik van informatie over het gebruik de [facturering API's](billing-usage-rate-card-overview.md).

Zie voor meer informatie over het gebruik van uw dagelijkse [inzicht in uw factuur voor Microsoft Azure](billing-understand-your-bill.md). Zie voor meer informatie over het beheren van kosten [te voorkomen dat onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md).

## <a name="noinvoice"></a>Waarom zie ik een factuur niet voor de laatste factureringsperiode?

Er zijn diverse redenen dat een factuur niet wordt weergegeven:

- U hebt een maandbedrag tegoed bij uw abonnement die u hebt niet langer zijn dan of u hebt een gratis proefversie. Een factuur wordt alleen gegenereerd wanneer u geld verschuldigd bent.

- Het is minder dan 30 dagen na de die uw op Azure abonnement.

- De factuur niet is nog gegenereerd. Wacht tot het einde van de factureringsperiode.

- Als u niet de accountbeheerder bent oudere facturen mogelijk niet beschikbaar.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

