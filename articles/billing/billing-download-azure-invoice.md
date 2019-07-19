---
title: Uw Microsoft Azure-factuur weergeven en downloaden
description: Hierin wordt beschreven hoe u uw Microsoft Azure factuur kunt weer geven en downloaden.
keywords: facturerings factuur, factuur downloaden, Azure-factuur, Azure-gebruik
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: banders
ms.openlocfilehash: 4dc5f88f7a5994e5bcd50d71c86bf1ba35b10734
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68321771"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Uw Microsoft Azure-factuur weergeven en downloaden

Voor de meeste abonnementen kunt u uw factuur downloaden van de [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) of deze in een e-mail bericht laten verzenden. Als u een Azure-klant bent met een Enterprise Agreement (EA-klant), kunt u de facturen van uw organisatie niet downloaden. Facturen worden verzonden naar iedereen die is ingesteld voor het ontvangen van facturen voor de inschrijving.

Alleen bepaalde rollen zijn gemachtigd om facturen weer te geven. Bijvoorbeeld de account beheerder of de beheerder van de onderneming. Zie [toegang tot Azure-facturering beheren met behulp van rollen](billing-manage-access.md)voor meer informatie over het verkrijgen van toegang tot facturerings gegevens.

Als u een [micro soft-klant overeenkomst](#check-your-access-to-a-microsoft-customer-agreement)hebt, moet u een van de volgende rollen hebben om uw facturen te ontvangen:

- Eigenaar facturerings profiel
- Inzender
- Lezer
- Factuur beheer

Zie [facturerings profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks)voor meer informatie over de facturerings rollen voor micro soft-klant overeenkomsten.

## <a name="noinvoice"></a>Waarom u mogelijk geen factuur ontvangt

Er kunnen verschillende redenen voor zijn dat u geen factuur ziet:

- Het is minder dan 30 dagen geleden dat u zich geabonneerd hebt op Azure.

- Azure factureert u aan het einde van uw factuur periode. Het is dus mogelijk dat er nog geen factuur is gegenereerd. Wacht tot het einde van de factureringsperiode.

- U bent niet gemachtigd om facturen te bekijken. Als u een micro soft-klant overeenkomst hebt, moet u de eigenaar van het facturerings profiel, de bijdrager, de lezer of de factuur Manager zijn. Voor andere abonnementen ziet u mogelijk geen oude facturen als u de account beheerder bent. Zie [Toegang tot factureringsgegevens voor Azure beheren](billing-manage-access.md) voor meer informatie over het verkrijgen van toegang tot factureringsgegevens.

- Als u een gratis proef versie of een maandelijks tegoed voor uw abonnement hebt, ontvangt u alleen een factuur wanneer u het maandelijkse tegoed overschrijdt. Als u een micro soft-klant overeenkomst hebt, ontvangt u altijd een factuur.

## <a name="download-your-azure-invoices-pdf"></a>Uw Azure-facturen (. PDF) downloaden

Voor de meeste abonnementen kunt u uw factuur downloaden van de Azure Portal. Als u een micro soft-klant overeenkomst hebt, raadpleegt u [facturen voor een micro soft-klant overeenkomst downloaden](#download-invoices-for-a-microsoft-customer-agreement).

### <a name="download-invoices-for-an-individual-subscription"></a>Facturen voor een afzonderlijk abonnement downloaden

1. Selecteer uw abonnement op de [pagina Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in de Azure portal als [een gebruiker met toegang tot facturen](billing-manage-access.md).

2. Selecteer **Facturen**.

    ![Scherm opname van de optie facturerings & gebruik](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Klik op **factuur downloaden** om een kopie van uw PDF-factuur weer te geven. Als deze **niet beschikbaar**is, raadpleegt u [Waarom wordt er geen factuur weer geven voor de laatste facturerings periode?](#noinvoice)

    ![Scherm opname van de facturerings perioden, de download optie en de totale kosten voor elke facturerings periode](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. U kunt ook uw dagelijkse gebruik weer geven door te klikken op de facturerings periode.

Zie [uw factuur begrijpen voor Microsoft Azure](billing-understand-your-bill.md)voor meer informatie over uw facturering. Zie [onverwachte kosten voor komen met Azure billing and cost management](billing-getting-started.md)voor meer informatie over het beheren van uw kosten.

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Facturen voor een micro soft-klant overeenkomst downloaden

Voor elk [facturerings profiel](billing-mca-overview.md#billing-profiles) in de micro soft-klant overeenkomst worden facturen gegenereerd. U moet een facturerings profiel eigenaar, bijdrager, lezer of factuur beheerder zijn om facturen te downloaden van de Azure Portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken naar *Cost Management en facturering*.
1. Selecteer een facturerings profiel. Afhankelijk van uw toegang moet u mogelijk eerst een facturerings account selecteren.
1. Selecteer **Facturen**.
1. Zoek in het factuur raster de rij van de factuur die u wilt downloaden.
1. Klik op het weglatings teken (`...`) aan het einde van de rij.
    ![Scherm opname van het weglatings teken aan het einde van de rij](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)
1. Selecteer **Factureren**in het menu down load context.

    ![Scherm opname van het context menu](./media/billing-download-azure-invoice/contextmenu.png)

Als u geen factuur voor de laatste facturerings periode ziet, raadpleegt u [Waarom wordt er geen factuur weer geven voor de laatste facturerings periode?](#noinvoice)

## <a name="get-your-invoice-in-email-pdf"></a>Uw factuur ontvangen in e-mail (. PDF)

U kunt zich aanmelden en extra ontvangers configureren om uw Azure-factuur in een e-mail bericht te ontvangen. Deze functie is mogelijk niet beschikbaar voor bepaalde abonnementen, zoals ondersteunings aanbiedingen, Enter prise agreements of Azure in Open. Als u een micro soft-klant overeenkomst hebt, raadpleegt u de volgende sectie, [uw facturerings profiel facturen ontvangen in een e-mail](#get-your-subscriptions-invoices-in-email).

### <a name="get-your-subscriptions-invoices-in-email"></a>De facturen van uw abonnement in een e-mail ontvangen

1. Selecteer uw abonnement op de [pagina Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Opt-in voor elk abonnement waarvan u eigenaar bent. Klik op **facturen** en vervolgens **e-mail op mijn factuur**.

    ![Scherm opname van de opt-in flow](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Klik op **opt in** en accepteer de voor waarden.

    ![Scherm opname van de opt-in flow stap 2](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Wanneer u de overeenkomst hebt geaccepteerd, kunt u extra ontvangers configureren. Wanneer een ontvanger wordt verwijderd, wordt het e-mail adres niet meer opgeslagen. Als u van gedachten verandert, moet u deze lezen.

    ![Scherm afbeelding met de opt-in flow-stap 3](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Als u na het volgen van de stappen geen e-mail bericht ontvangt, controleert u of uw e-mail adres juist is in de [communicatie voorkeuren van uw profiel](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Afmelden voor het ophalen van de facturen van uw abonnement in een e-mail bericht

Volg de voor gaande stappen om u af te melden bij het ophalen van uw factuur via e-mail en klik op afmelden bij **facturen**. Met deze optie worden alle e-mailadressen verwijderd die zijn ingesteld voor het ontvangen van facturen via e-mail. U kunt de ontvangers opnieuw configureren als u zich opnieuw aanmeldt.

 ![Scherm afbeelding waarin de opt-out-stroom wordt weer gegeven](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Ontvang uw klanten overeenkomst van micro soft in het e-mail bericht

Als u een micro soft-klant overeenkomst hebt, kunt u zich aanmelden om uw factuur in een e-mail bericht op te halen. Alle eigen aren van facturerings profielen, mede werkers, lezers en factuur managers ontvangen de factuur per e-mail. Lezers kunnen de e-mail factuur voorkeur niet bijwerken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken naar **Cost Management en facturering**.
1. Selecteer een facturerings profiel. Afhankelijk van uw toegang moet u mogelijk eerst een facturerings account selecteren.
1. Onder **instellingen**, selecteert u **Eigenschappen**.
1. Onder **e-mail factuur**selecteert u **e-mail factuur voorkeur bijwerken**.

    ![Scherm opname van factuur eigenschappen van e-mail](./media/billing-download-azure-invoice/billingprofile-email.png)

1. Selecteer **Aanmelden**.
1. Klik op **Bijwerken**.

### <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Meld u af bij het ophalen van uw klanten overeenkomst in e-mail van micro soft

Volg de voor gaande stappen om u af te melden bij het ophalen van uw factuur via e-mail. Klik vervolgens op **opt-out**. Alle eigen aars, mede werkers, lezers en factuur managers worden ook bij het ophalen van de factuur via e-mail afgemeld. Als u een lezer bent, kunt u de voor keur voor de e-mail factuur niet wijzigen.



## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Controleer uw toegang tot een micro soft-klant overeenkomst
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over uw factuur en kosten:

- [Het gebruik en de kosten van Microsoft Azure bekijken en downloaden](billing-download-azure-daily-usage.md)
- [Meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md)
- [Voor waarden op uw Azure-factuur begrijpen](billing-understand-your-invoice.md)
- [Meer informatie over uw gedetailleerde gebruik van Microsoft Azure](billing-understand-your-usage.md)
- [De Azure-prijzen van uw organisatie weer geven](billing-ea-pricing.md)

Als u een micro soft-klant overeenkomst hebt, raadpleegt u:

- [Meer informatie over de kosten op de factuur voor uw facturerings profiel](billing-mca-understand-your-bill.md)
- [Meer informatie over de voor waarden van de factuur voor uw facturerings profiel](billing-mca-understand-your-invoice.md)
- [Meer informatie over het Azure-gebruik en het kosten bestand voor uw facturerings profiel](billing-mca-understand-your-usage.md)
- [BTW-documenten voor uw facturerings profiel weer geven en downloaden](billing-mca-download-tax-document.md)
- [De Azure-prijzen van uw organisatie weer geven](billing-ea-pricing.md)
