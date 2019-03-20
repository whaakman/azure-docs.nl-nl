---
title: Weergeven en downloaden van Azure-gebruik en de kosten | Microsoft Docs
description: Beschrijft hoe u downloaden of weergeven van uw Azure dagelijkse gebruik en kosten in rekening gebracht.
keywords: facturering van gebruik, kosten voor het gebruik, gebruik, gebruik van de weergave, azure-factuur, azure gebruiksgegevens downloaden
services: billing
documentationcenter: ''
author: genlin
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 20404cea2aca984ef35472fa94d37c04eb8080e4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57872636"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Weergeven en downloaden van uw Azure-gebruik en kosten

Als u een EA-klant bent of een [Microsoft KLANTOVEREENKOMST](#check-your-access-to-a-microsoft-customer-agreement), kunt u Azure-gebruik en kosten in rekening gebracht in downloaden de [Azure-portal](https://portal.azure.com/). Voor andere abonnementen, gaat u naar de [Azure-Accountcentrum](https://account.azure.com/Subscriptions) op gebruiksgegevens downloaden.

Alleen bepaalde functies gemachtigd voor gebruik van Azure-informatie, zoals de accountbeheerder of Enterprise-beheerder. Zie [Toegang tot factureringsgegevens voor Azure beheren](billing-manage-access.md) voor meer informatie over het verkrijgen van toegang tot factureringsgegevens.

Als u hebt een [Microsoft KLANTOVEREENKOMST](#Check-your-access-to-a-Microsoft-Customer-Agreement), u moet een factureringsprofiel eigenaar, Inzender, lezer, of factuur manager om uw Azure-gebruik en kosten weer te geven. Zie voor meer informatie over facturering rollen voor Microsoft-klant-overeenkomsten, [facturering profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

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

Zie [Meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md) voor meer informatie over uw dagelijkse gebruik. Zie voor meer informatie over het beheren van uw kosten, [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing-getting-started.md).

## <a name="download-usage-for-ea-customers"></a>Gebruiksgegevens downloaden voor EA-klanten

Als u wilt weergeven en downloaden van gegevens over gebruik als een EA-klant bent, moet u een Enterprise-beheerder, eigenaar van Account of afdeling beheerder met de weergave in rekening gebracht beleid is ingeschakeld.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken naar *kosten Management en facturering*.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Selecteer **gebruik en kosten**.
1. Voor de maand die u wilt downloaden, selecteert u **downloaden**.

## <a name="download-usage-for-your-microsoft-customer-agreement"></a>Gebruik voor uw Microsoft-KLANTOVEREENKOMST downloaden

Als u een KLANTOVEREENKOMST van Microsoft hebt, kunt u uw Azure-gebruik en kosten in rekening gebracht voor uw facturering profiel downloaden. U moet een facturering profiel zijn eigenaar, Inzender, lezer, of de manager voor het downloaden van de Azure-gebruik en kosten CSV factuur.

### <a name="download-usage-for-billed-charges"></a>Gebruiksgegevens voor gefactureerde kosten voor downloaden

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zoeken op **kosten Management en facturering**.
3. Selecteer een profiel voor facturering. Afhankelijk van uw toegang moet u mogelijk eerst een factureringsaccount selecteren.
4. Selecteer **facturen**.
5. In het raster factuur vindt u de rij van de factuur overeenkomt met het gebruik dat u wilt downloaden.
6. Klik op het weglatingsteken (`...`) aan het einde van de rij.

    ![Schermafbeelding van de drie puntjes aan het einde van de rij](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)

7. Selecteer in het contextmenu downloaden **kosten en het gebruik van Azure**.

     ![Schermafbeelding van Azure gebruik en de kosten die zijn geselecteerd](./media/billing-download-azure-usage/contextmenu-usage.png)

### <a name="download-usage-for-pending-charges"></a>Gebruiksgegevens voor in behandeling zijnde kosten downloaden

U kunt ook gebruik van de maand tot heden downloaden voor de huidige factureringsperiode. Deze kosten voor het gebruik dat nog niet is gefactureerd.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zoeken op **kosten Management en facturering**.
3. Selecteer een profiel voor facturering. Afhankelijk van uw toegang moet u mogelijk eerst een factureringsaccount selecteren.
4. In de **overzicht** blade zoeken naar de downloadkoppelingen onder de kosten maand tot heden.
5. Selecteer **kosten en het gebruik van Azure**.

    ![Schermafbeelding van downloaden uit overzicht](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Uw toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over uw factuur- en gebruiksgegevens kosten:

- [Meer informatie over uw Microsoft Azure gedetailleerd gebruik](billing-understand-your-usage.md)
- [Meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md)
- [Weergeven en uw Microsoft Azure-factuur downloaden](billing-download-azure-invoice.md)
- [Weergeven en downloaden van uw organisatie Azure-prijzen](billing-ea-pricing.md)

Als u een Microsoft-KLANTOVEREENKOMST hebt, Zie:

- [Meer informatie over uw Azure overeenkomst van Microsoft Customer gedetailleerd gebruik](billing-mca-understand-your-usage.md)
- [Meer informatie over de kosten op uw factuur KLANTOVEREENKOMST van Microsoft](billing-mca-understand-your-bill.md)
- [Weergeven en uw Microsoft Azure-factuur downloaden](billing-download-azure-invoice.md)
- [Weergeven en btw-documenten voor uw Microsoft-KLANTOVEREENKOMST downloaden](billing-mca-download-tax-document.md)
- [Weergeven en downloaden van uw organisatie Azure-prijzen](billing-ea-pricing.md)
