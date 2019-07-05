---
title: Controleren en bijhouden van gebruik van Azure gratis service
description: Informatie over het gebruik van de gratis service in de Azure portal en gebruik van CSV-bestand te controleren.
author: amberbhargava
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 3543bed7f699fd149ca7f2a6f61e9eb5aad5f1a3
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491428"
---
# <a name="check-free-service-usage-included-with-your-azure-free-account"></a>Gebruik van de gratis service opgenomen met uw gratis Azure-account controleren

U bent niet in rekening gebracht voor services voor gratis deel uit van een gratis Azure-account, tenzij u de grenzen van de services overschrijdt. Om te blijven in de limieten, kunt u de Azure portal of uw gebruiksbestand om te controleren en bijhouden van het gebruik van de gratis service.

## <a name="check-usage-in-the-azure-portal"></a>Gebruik in Azure portal controleren

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).

2.  Selecteer in de navigatiebalk aan de linkerkant, **alle services**.

3.  Selecteer **Abonnementen**.

4.  Selecteer het abonnement dat u hebt gemaakt toen u zich hebt geregistreerd voor een gratis account.

    ![Schermafbeelding van alle abonnementen](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  De overzichtssectie ziet u essentiële informatie over uw abonnement. Bijvoorbeeld, abonnements-ID, aanbiedingtype en de naam van abonnement. U kunt ook informatie vinden wanneer uw tegoed gratis account is verlopen.

    ![Schermafbeelding van essentiële informatie van abonnement](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Schuif omlaag naar meer informatie over uw huidige en geraamde kosten. De kosten voor het bevat gebruik van de service niet is opgenomen met uw gratis account en het gebruik van meer dan de grenzen van gratis services.

    ![Schermafbeelding van informatie over de kosten van abonnementen](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Het laatste gedeelte van de overzichtssectie bevat een tabel met gebruik van de gratis service.

    ![Schermafbeelding van het gebruik van gratis services](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    De tabel bevat de volgende kolommen:

* **Meternaam:** Identificeert de maateenheid voor de meter wordt gebruikt. Zie voor meer informatie over de toewijzing van de meter-service, [inzicht in de gratis service toewijzing van de meter](billing-understand-free-service-meter-mapping.md).
* **Gebruikslimiet:** Huidige maand gebruik en de limiet voor de meter. U kunt ook deze informatie vinden in de statusbalk.
* **Status:** Gebruik de status van de meter. Op basis van uw gebruikspatroon, kunt u een hebben van de volgende statuten:
  * **Niet in gebruik:** U hebt de meter nog niet hebt gebruikt of het gebruik van de meter nog niet het factureringssysteem bereikt.
  * **Overschreden op \<datum >:** U hebt de limiet voor de meter overschreden op \<datum >.
  * **Waarschijnlijk geen overschrijding:** U kunt waarschijnlijk geen overschrijding van de limiet voor de meter.
  * **Overschrijdt op \<datum >:** U bent waarschijnlijk zal overschrijden de limiet voor de meter op \<datum >.

## <a name="check-usage-with-the-usage-file"></a>Gebruik controleren met het gebruiksbestand

Uw gebruiksbestand geeft gedetailleerde informatie voor uw Azure-abonnement. U kunt uw bestand per maand en de dagelijkse gebruiksgegevens downloaden van het Azure-Accountcentrum. Zie voor meer informatie over het van-gebruiksbestand downloaden en begrijpen van de toegang is vereist, [ophalen factuur- en gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md). Zie voor meer informatie over de kolommen in het gebruiksbestand, [meer informatie over uw gebruik](billing-understand-your-usage.md).

Het gebruiksbestand bevat informatie over het gebruik van gratis en betaalde services. Gratis service meters zou hebben **gratis** toegevoegd aan het einde van de meternaam van de. Zoeken naar gratis meters, open het bestand in excel en filter de **Metercategorie kolom** voor cellen die de tekst **- vrij** (Filters gebruiken om tekst &rarr; filter Contains).


![Schermafbeelding van het gebruik van gratis services](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen
- [Uw abonnement bijwerken](billing-upgrade-azure-subscription.md)
