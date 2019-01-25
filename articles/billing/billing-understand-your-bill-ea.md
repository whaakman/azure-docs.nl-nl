---
title: Meer informatie over uw factuur voor Azure Enterprise | Microsoft Docs
description: Meer informatie over het lezen en meer informatie over het gebruik en de factuur voor Azure-klanten met een Enterprise Agreement
services: ''
documentationcenter: ''
author: adpick
manager: dougeby
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: banders
ms.openlocfilehash: 36ce4d96e02bac1eae1791acf811da468726b4a6
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902700"
---
# <a name="understand-your-bill-for-azure-customers-with-an-enterprise-agreement"></a>Meer informatie over uw factuur voor Azure-klanten met een Enterprise Agreement

Azure-klanten met een Enterprise Agreement ontvangt een factuur wanneer ze langer zijn dan de creditcard van de organisatie of services die niet zijn gekoppeld aan het tegoed gebruikt. 

Tegoed van uw organisatie bevat uw monetaire toezegging. De monetaire toezegging wordt het bedrag dat uw organisatie vooraf betaald voor het gebruik van Azure-services. U kunt monetaire toezeggingsgelden naar uw Enterprise Agreement toevoegen door contact op met uw Microsoft-accountmanager of reseller.  

## <a name="when-credit-exceeded-or-doesnt-apply"></a>Wanneer tegoed overschreden of is niet van toepassing

U profiteert van een of meer facturen wanneer het volgende gebeurt:

- **Serviceoverschrijding**: Kosten voor het gebruik van uw organisatie meer dan uw saldo.
- **De kosten apart in rekening gebracht**: De services die wordt gebruikt door uw organisatie niet zijn gekoppeld aan het tegoed. U bent voor de volgende services gefactureerd, ongeacht uw saldo:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop 
    - Geregistreerde gebruiker
    - Openlogic
    - Externe toegang rechten XenApp Essentials geregistreerde gebruiker
    - Ubuntu Advantage
    - Visual Studio Enterprise (maandelijks)
    - Visual Studio Enterprise (jaarlijks)
    - Visual Studio Professional (maandelijks)
    - Visual Studio Professional (jaarlijks)
- **Marketplace-kosten**: Azure Marketplace-aankopen en gebruik zijn niet inbegrepen bij uw organisatie tegoed en worden afzonderlijk gefactureerd. De Enterprise-beheerder heeft de mogelijkheid inschakelen en uitschakelen van Marketplace-aankopen voor hun organisatie in de Enterprise Portal. 

Wanneer er vanwege de kosten voor serviceoverschrijding en de kosten apart in rekening gebracht tijdens de factureringsperiode, krijgt u een factuur met beide typen kosten in rekening gebracht. Marktplaatsen in de kosten worden altijd afzonderlijk gefactureerd.

## <a name="review-charges"></a>Kosten controleren

Als u wilt controleren en verifiëren van de kosten op uw factuur, moet u een Enterprise-beheerder zijn. Zie voor meer informatie, [beheerdersrollen in Azure begrijpen Azure Enterprise overeenkomst](billing-understand-ea-roles.md). Als u niet weet wie de Enterprise-beheerder is voor uw organisatie, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="review-service-overage-invoice"></a>Overschrijding factuur controleren

Uw totale gebruiksbedrag van de Enterprise-portal in vergelijken **rapporten** > **Gebruiksoverzicht** met uw service overschrijding factuur. De overschrijding factuur bevat gebruik die langer is dan tegoed van uw organisatie en/of services die niet zijn gekoppeld aan het tegoed. De bedragen op de **Gebruiksoverzicht** zijn exclusief btw. 

1. Aanmelden bij de [Enterprise portal](https://ea.azure.com).
1. Selecteer **rapporten**.
1. Overschakelen van de weergave in de rechterbovenhoek van het tabblad **M** naar **C** en overeenkomen met de periode op de factuur.
 
   ![Schermafbeelding van M + C-optie op Samenvatting van gebruik.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. De **Totaalgebruik** bedrag moet overeenkomen met de **uitgebreid totaalbedrag** op uw factuur overschrijding. De volgende tabel bevat de voorwaarden en beschrijvingen die worden weergegeven op de factuur en klik op de **Gebruiksoverzicht** in de Enterprise portal:

   |Factuur-term|Overzicht gebruik-term|Description|
   |---|---|---|
   |Totale hoeveelheid uitgebreid|Totaal gebruik|De totale vóór belastingen gebruik kosten in rekening gebracht voor de specifieke periode voordat het tegoed wordt toegepast.|
   |Toezegging|Toezegging|Het tegoed toegepast die specifieke periode.|
   |Totale verkoop|Totale overschrijding|De kosten van het totale gebruik dat hoger is dan de hoeveelheid tegoed. Dit bedrag bevat geen belasting.|
   |Bedrag|Niet van toepassing|Belasting die voor het bedrag van de totale verkoop voor de specifieke periode geldt.|
   |Totaalbedrag|Niet van toepassing|Het bedrag moet de factuur nadat het tegoed is toegepast en de btw wordt toegevoegd.|
1. Voor meer informatie over uw kosten, gaat u naar **gebruiksgegevens downloaden** > **Geavanceerd rapport downloaden**. Dit rapport bevat geen belastingen of kosten voor reserveringen of marketplace-kosten.

      ![Schermafbeelding van Geavanceerd rapport downloaden op het tabblad downloaden gebruik.](./media/billing-understand-your-bill-ea/ea-portal-download-usage-advanced.png)

### <a name="review-marketplace-invoice"></a>Marketplace-factuur controleren

Vergelijk het totaal op Azure Marketplace **rapporten** > **Gebruiksoverzicht** in de Enterprise-portal met uw marketplace-factuur. De marketplace-factuur is alleen voor Azure Marketplace-aankopen en het gebruik. De bedragen op de **Gebruiksoverzicht** zijn exclusief btw. 

1. Aanmelden bij de [Enterprise portal](https://ea.azure.com).
1. Selecteer **rapporten**.
1. Overschakelen van de weergave in de rechterbovenhoek van het tabblad **M** naar **C** en overeenkomen met de periode op de factuur.

     ![Schermafbeelding van M + C-optie op Samenvatting van gebruik.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. De **Azure Marketplace** totaal moet overeenkomen met de **totale verkoop** op uw marketplace-factuur.
1. Voor meer informatie over uw kosten op basis van gebruik, gaat u naar **gebruiksgegevens downloaden**. Onder **Marketplace-kosten**, selecteer **downloaden**. Dit rapport kan niet ook belastingen of eenmalig aankopen weergeven.

     ![Schermafbeelding van downloaden optie onder Marketplace-kosten.](./media/billing-understand-your-bill-ea/ea-portal-download-usage-marketplace.png)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
