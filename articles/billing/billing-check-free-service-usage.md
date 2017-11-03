---
title: Bewaken en bijhouden gebruik van de gratis services - Azure | Microsoft Docs
description: Informatie over het gebruik van de gratis services controleren. Gebruik van Azure portal en gebruik van csv.
services: 
documentationcenter: 
author: amberbhargava
manager: amberb
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: amberb
ms.openlocfilehash: 27ff6c92904a0b32cd4a37c8b1930adc121a7231
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Controleer gebruik van de beschikbare services in uw gratis Azure-account 

Er zijn niet in rekening gebracht voor services voor het vrije in gratis Azure-account, tenzij u de grenzen van deze services overschrijdt. Om te blijven met de limieten, kunt u ofwel de Azure portal of het bestand met informatie over het gebruik om te controleren en bijhouden van het gebruik van de gratis services gebruiken. 

## <a name="check-usage-on-the-azure-portal"></a>Gebruik in de Azure portal controleren

1.  Meld u aan bij [Azure Portal]( http://portal.azure.com).

2.  Selecteer in de onderkant van de navigatiebalk aan de linkerkant, **meer services**.

3.  Selecteer **abonnementen**.

4.  Selecteer het abonnement dat u hebt gemaakt toen u zich registreerde gratis account.

    ![Schermafbeelding van alle abonnementen](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  De overzichtssectie ziet u essentiële informatie over uw abonnement zoals abonnements-ID, bieden type en de abonnementsnaam van het. U kunt ook informatie over wanneer uw account voor de gratis tegoed zou verlopen vinden.

    ![Schermafbeelding van essentiële informatie abonnement](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Schuif naar beneden zoeken naar informatie over de huidige en verwachte kosten. De kosten bevat informatie over het gebruik van services niet zijn opgenomen in een gratis account en het gebruik van meer dan de grenzen van de beschikbare services. 

    ![Schermafbeelding van abonnement kostengegevens](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Het laatste gedeelte van de overzichtssectie bevat een tabel op het gebruik van de beschikbare services. 

    ![Schermafbeelding van informatie over het gebruik van gratis services](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    De tabel bevat de volgende kolommen:

* **De naam van de meter:** identificeert de maateenheid voor de meter wordt gebruikt. Zie voor meer informatie over service toewijzing van de meter, [gratis service toewijzing van de meter begrijpen](billing-understand-free-service-meter-mapping.md). 
* **Gebruikslimiet:** gebruiks- en de limiet voor het meten van de huidige maand. U kunt deze informatie ook vinden in de statusbalk.
* **Status:** status van de informatie over het gebruik van de meter. Op basis van uw gebruikspatroon, kunt u hebben een van deze statuten.
  * **Niet in gebruik:** u hebt niet de meter gebruikt of het gebruik van de meter heeft niet het factureringssysteem bereikt.
  * **Overschreden op \<datum >:** u hebt de limiet voor het meten van overschreden \<datum >.
  * **Waarschijnlijk niet Exceed:** bent u waarschijnlijk niet langer zijn dan de limiet voor de meter.
  * **Is groter dan op \<datum >:** bent u waarschijnlijk meer dan de limiet voor de meter op \<datum >.


## <a name="check-usage-through-the-usage-file"></a>Gebruik controleren via het gebruik van bestand

Uw gebruik-bestand bevat gedetailleerde informatie voor uw Azure-abonnement. U kunt uw maandelijkse en dagelijks gebruik bestand downloaden van Azure-Accountcentrum. Zie voor meer informatie over het gebruik van bestand downloaden en begrijpen van de toegang is vereist, [factuur ophalen en gebruik](billing-download-azure-invoice-daily-usage-date.md). Zie voor meer informatie over kolommen in het bestand gebruik, [begrijpen van de voorwaarden van uw gebruik](billing-understand-your-usage.md). 

Het gebruik van-bestand bevat informatie over het gebruik voor gratis en betaalde services. Gratis service meters zou hebben **vrije** toegevoegd aan het einde van de naam van de meter. Gratis meters, open het bestand in excel en filter vinden de **kolom Meter categorie** voor cellen met tekst **- vrij** (Filters gebruiken om tekst &rarr; filter Contains)&nbsp;

![Schermafbeelding van informatie over het gebruik van gratis services](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)


## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, moet [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
