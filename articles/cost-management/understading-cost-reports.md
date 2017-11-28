---
title: Kostenrapporten in Azure kosten Management begrijpen | Microsoft Docs
description: In dit artikel helpt u begrijpen basisstructuur Cloudyn-rapporten en -functies.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/27/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: df2108a6e2a01195340a09eacf1c56f9d738c923
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="understanding-cost-reports"></a>Understanding kosten verbonden aan rapporten

In dit artikel helpt u begrijpen basisstructuur Cloudyn-rapporten en -functies. De meeste Cloudyn rapporten zijn intuïtieve en een uniform uiterlijk hebben. Nadat u dit artikel hebt gelezen, zijn klaar voor gebruik van alle rapporten. Veel standaard functies zijn beschikbaar in de verschillende rapporten, zodat u kunt de rapporten met gemak navigeren. Rapporten kunnen worden aangepast en u kunt kiezen uit diverse opties voor het berekenen en de resultaten weer te geven.

## <a name="report-fields-and-options"></a>Rapportvelden en opties

Hier volgt een voorbeeld van het rapport kosten gedurende een periode bekijken. De meeste Cloudyn rapporten hebben vergelijkbare lay-out.

![Voorbeeldrapport](./media/understanding-cost-reports/sample-report.png)

Elke genummerde gebied in de voorgaande afbeelding wordt beschreven in de volgende informatie:

1. **Datumbereik**

    Gebruik de lijst datumbereik voor het definiëren van een rapport tijdsinterval met behulp van een vooraf ingestelde of aangepast.
2. **Opgeslagen Filter**

    Gebruik de opgeslagen filterlijst de huidige groepen en filters die worden toegepast op het rapport op te slaan. Opgeslagen filters zijn beschikbaar via de kosten en prestaties van rapporten, waaronder:

      - Kosten analyse
      - Toewijzing
      - Asset-management
      - Optimalisatie

  Typ een naam van het filter en klik op **opslaan**.

3. **Tags**

    Gebruik het gebied van de labels aan groep door de tag categorieën. Labels die worden vermeld in het menu Azure afdeling of kosten center tags of ze zijn van Cloudyn kosten entiteit en dit abonnement labels. Selecteer de tags voor het filteren van resultaten. U kunt ook de naam van een tag (trefwoord) om resultaten te filteren typen.

    ![opties selecteren](./media/understanding-cost-reports/select-options.png)

    Klik op **toevoegen** een nieuw filter toe te voegen.

    ![filter toevoegen](./media/understanding-cost-reports/add-filter.png)

    Label groeperen of filteren heeft geen betrekking op Azure-resources of resource group-tags.

    Toewijzing van kosten tag groeperen en filteren zijn beschikbaar in de **groepen** menuoptie.

4. **Groepen in rapporten**

    Groepen gebruiken in kostenanalyse rapporten om weer te geven standaard, categorieën van de gegevens in uw rapport facturering gespecificeerd.  Groepen in rapporten weergeven van toewijzing van kosten weergeven echter categorieën op basis van codes. Categorieën op basis van een label worden gedefinieerd in het model van de toewijzing van kosten en de standaard gespecificeerde categorieën van de factureringsgegevens.

    ![labels van groepen](./media/understanding-cost-reports/groups-tags01.png)

    ![labels van groepen](./media/understanding-cost-reports/groups-tags02.png)

    Groepen in de groep op basis van een tag categorieën kunnen in kosten toewijzing rapporten, onder andere:
      - Tags
      - resource group-tags
      - Cloudyn kosten entity-tags
      - Abonnement tag categorieën voor doel voor het toewijzen van kosten

  Voorbeelden zijn onder andere:
     - Kostenplaats
     - Afdeling
     - Toepassing
     - Omgeving
     - Kosten code

5. **Filters**

    Enkelvoudige of meervoudige selectie filters gebruiken om geselecteerde waarden bereiken. Als een filter, klikt u op **toevoegen** en selecteer vervolgens filtercategorieën en waarden.

6. **Kosten Model**

    Gebruik Model kosten kostprijsmodel dat u eerder hebt gemaakt met de kosten toewijzing 360 selecteren. U hebt mogelijk meerdere Cloudyn kosten modellen, afhankelijk van uw kosten toewijzing. Sommige van uw organisatie teams mogelijk kosten toewijzing vereisten die van anderen verschillen. Elk team kan hun eigen toegewezen kostprijsmodel hebben.

    Zie voor meer informatie over het maken van een modeldefinitie van kosten toewijzing [gebruik aangepaste labels toe te wijzen kosten](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **Afschrijving**

    Gebruik afschrijving in kostentoewijzing rapporten voor niet-adresgebruik weergeven op basis van servicetarief of eenmalige leveranciers kosten en de daaraan verbonden kosten verspreid over gelijkmatig tijdens hun levensduur. Voorbeelden van eenmalige kosten kunnen onder andere:
    - Jaarlijkse kosten van ondersteuning
    - Jaarlijkse beveiliging onderdelen kosten
    - Gereserveerde exemplaren kopen kosten
    - Sommige Azure Marketplace-items.

  Selecteer onder afschrijving, **afgelost kosten** of **werkelijke kosten**.

8. **Naamomzetting**

    De resolutie gebruiken de resolutie tijd binnen het geselecteerde datumbereik selecteren. Uw oplossing tijd bepaalt hoe eenheden worden weergegeven in het rapport en kunnen:
    - Dagelijks
    - Per week
    - Maandelijks
    - Per kwartaal
    - Jaarlijks

9. **Toewijzingsregels**

    Gebruik toewijzingsregels toe te passen of uitschakelen van de kostentoewijzing kosten herberekening. U kunt in- of uitschakelen van de herberekening van de toewijzing van kosten voor factureringsgegevens. De herberekening geldt voor de geselecteerde categorieën in het rapport. Hiermee kunt u nagaan wat het resultaat kosten toewijzing herberekening tegen onbewerkte factureringsgegevens.

10. **Niet-gecategoriseerde**

    Gebruik niet-gecategoriseerd wilt opnemen of uitsluiten van niet-gecategoriseerde kosten in het rapport.

11. **Velden weergeven/verbergen**

    De optie weergeven/verbergen heeft geen invloed in rapporten.

12.   **Weergave-indelingen**

    Gebruik de weergave-indelingen verschillende grafiek of tabel weergaven selecteren.

    ![weergave-indelingen](./media/understanding-cost-reports/display-formats.png)

13. **Meerdere kleuren**

    Meerdere kleuren gebruiken om in te stellen de kleur van de grafieken in het rapport.

14. **Acties**

    Gebruik acties wilt opslaan, exporteren of het rapport plant.

## <a name="next-steps"></a>Volgende stappen

- Als u nog niet de eerste zelfstudie hebt voltooid voor het beheer van kosten, leest u op het [gebruik en kosten bekijken](tutorial-review-usage.md).
