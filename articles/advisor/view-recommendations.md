---
title: Azure Advisor-aanbevelingen voor weergeven die voor u belangrijk
description: Weergeven en filteren Azure Advisor-aanbevelingen om ruis te verminderen.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 04/03/2019
ms.author: kasparks
ms.openlocfilehash: 9f599a63fd5f52420f1b79e769d4f7bca9683b32
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60467845"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Azure Advisor-aanbevelingen voor weergeven die voor u belangrijk

Azure Advisor biedt aanbevelingen om u te helpen uw Azure-implementaties te optimaliseren. Binnen Advisor hebt u toegang tot een aantal functies die u helpen bij het verfijnen van uw aanbevelingen tot alleen degenen die het belangrijkst voor u.

## <a name="configure-subscriptions-and-resource-groups"></a>Abonnementen en resourcegroepen configureren

Advisor biedt u de mogelijkheid om abonnementen en resourcegroepen die het belangrijkst voor u en uw organisatie te selecteren. U ziet alleen de aanbevelingen voor de abonnementen en resourcegroepen die u selecteert. Standaard zijn alle geselecteerd. Configuratie-instellingen van toepassing op het abonnement of de resource-groep, zodat de dezelfde instellingen gelden voor iedereen die toegang tot die groep abonnement of resourcegroep heeft. Configuratie-instellingen kunnen worden gewijzigd in Azure portal of via een programma.

Wijzigingen aanbrengen in de Azure-portal:

1. Open [Azure Advisor](https://aka.ms/azureadvisordashboard) in Azure portal.

1. Selecteer **configuratie** in het menu.

   ![Menu voor Advisor-configuratie](./media/view-recommendations/configuration.png)

1. Schakel het selectievakje de **opnemen** kolom voor alle abonnementen en resourcegroepen voor het ontvangen van aanbevelingen van Advisor. Als het selectievakje is uitgeschakeld, kan u niet gemachtigd om een configuratiewijziging hebt doorgevoerd in die groep abonnement of resourcegroep. Meer informatie over [machtigingen in Azure Advisor](permissions.md).

1. Klik op **toepassen** onder nadat u een wijziging aanbrengt.

## <a name="filtering-your-view-in-the-azure-portal"></a>Filteren van de weergave in de Azure-portal

Configuratie-instellingen blijven actief totdat gewijzigd. Als u wilt om de weergave van de aanbevelingen voor een enkele weergave te beperken, kunt u de vervolgkeuzelijsten die aan de bovenkant van het paneel Advisor. De deelvensters overzicht, hoge beschikbaarheid, beveiliging, prestaties, kosten en alle aanbevelingen kunt u de abonnementen, resourcetypen en de status van de aanbeveling die u wilt zien selecteren.

   ![Advisor filteren menu](./media/view-recommendations/filtering.png)

## <a name="dismissing-and-postponing-recommendations"></a>Negeren en aanbevelingen uitstellen

Azure Advisor kunt u negeren of uitstellen, aanbevelingen voor één resource. Als u een aanbeveling negeren, ziet niet u deze opnieuw, tenzij u deze handmatig activeren. Een aanbeveling uitstellen kunt u echter om op te geven van een duur waarna de aanbeveling wordt automatisch opnieuw worden geactiveerd. Uitstellen kan worden gedaan in Azure portal of via een programma.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Een enkele aanbeveling in de Azure-portal uitstellen 

1. Open [Azure Advisor](https://aka.ms/azureadvisordashboard) in Azure portal.
1. Selecteer een categorie aanbeveling om uw aanbevelingen weer te geven
1. Selecteer een aanbeveling in de lijst met aanbevelingen
1. Selecteer uitstellen of verwijderen voor de aanbeveling die u wilt stellen of te verwijderen

     ![Advisor filteren menu](./media/view-recommendations/postpone-dismiss.png)

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Uitstellen of te verwijderen van een meerdere aanbevelingen in Azure portal

1. Open [Azure Advisor](https://aka.ms/azureadvisordashboard) in Azure portal.
1. Selecteer een categorie aanbeveling om uw aanbevelingen weer te geven.
1. Selecteer een aanbeveling in de lijst met aanbevelingen.
1. Schakel het selectievakje aan de linkerkant van de rij voor alle resources die u wilt uitstellen of de aanbeveling negeren.
1. Selecteer **uitstellen** of **sluiten** op linksboven in de tabel.

     ![Advisor filteren menu](./media/view-recommendations/postpone-dismiss-multiple.png)

> [!NOTE]
> U moet machtiging Inzender of eigenaar te verwijderen of een aanbeveling uitstellen. Meer informatie over de machtigingen in Azure Advisor.

> [!NOTE]
> Als de selectievakjes zijn uitgeschakeld, kunnen nog steeds aanbevelingen worden geladen. Wacht tot alle aanbevelingen laden voordat u probeert te stellen of te verwijderen.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Activeren van een aanbeveling uitgestelde of gesloten

U kunt een aanbeveling dat is uitgesteld of gesloten activeren. Deze actie kan worden gedaan in Azure portal of via een programma. In Azure Portal:

1. Open [Azure Advisor](https://aka.ms/azureadvisordashboard) in Azure portal.

1. Het filter in het deelvenster Overzicht om te wijzigen **uitgesteld**. Advisor geeft aanbevelingen voor uitgestelde of gesloten.

    ![Advisor filteren menu](./media/view-recommendations/activate-postponed.png)

1. Selecteer een categorie om te zien **uitgesteld** en **genegeerd** aanbevelingen.

1. Selecteer een aanbeveling in de lijst met aanbevelingen. Hiermee opent u aanbevelingen met de **uitgesteld & gesloten** tabblad is al geselecteerd om weer te geven van de bronnen waarvoor deze aanbeveling is uitgesteld of gesloten.

1. Klik op **activeren** aan het einde van de rij. Nadat u hebt geklikt, wordt de aanbeveling is actief voor die bron en dus worden verwijderd uit deze tabel. De aanbeveling is nu zichtbaar in de **Active** tabblad.
 
     ![Advisor filteren menu](./media/view-recommendations/activate-postponed-2.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt uitgelegd hoe u de aanbevelingen die voor u belangrijk in Azure Advisor kunt weergeven. Zie voor meer informatie over Advisor: 

- [Wat is Azure Advisor?](advisor-overview.md)
- [Aan de slag met Advisor](advisor-get-started.md)
- [Machtigingen in Azure Advisor](permissions.md)



