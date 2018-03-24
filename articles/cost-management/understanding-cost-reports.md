---
title: Kosten-rapporten in Azure kosten Management begrijpen | Microsoft Docs
description: In dit artikel helpt u begrijpen Cloudyn kosten management rapporten basisstructuur en functies.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/07/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: bc2c696dceb3ed4741c10a5c611bd2d438b71bd5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="understanding-cost-management-reports"></a>Understanding kosten-rapporten

In dit artikel helpt u begrijpen Cloudyn kosten management rapporten basisstructuur en functies. De meeste Cloudyn rapporten zijn intuïtieve en een uniform uiterlijk hebben. Nadat u dit artikel hebt gelezen, zijn klaar voor gebruik van alle kosten-rapporten. Veel standaard functies zijn beschikbaar in de verschillende rapporten, zodat u kunt de rapporten met gemak navigeren. Rapporten kunnen worden aangepast en u kunt kiezen uit diverse opties voor het berekenen en de resultaten weer te geven.

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

    Hier volgt een lijst van ingebouwde groepen in rapporten beschikbaar:

    - **Kostentype**
      - Selecteer een kostentype of meerdere kostentypen of te selecteren. Kosten typen zijn:
        - Eenmalige kosten
        - Ondersteuning
        - Gebruikskosten
    - **De klant**
        - Selecteer een specifieke klant meerdere klanten, of alle klanten.
    - **Accountnaam**
        - De naam van het account of abonnement. In Azure is de naam van het Azure-abonnement.
    - **Er is geen account**
        - Selecteer een account, meerdere accounts of alle accounts. In Azure is de GUID van het Azure-abonnement.
    - **Bovenliggende Account**
        - Selecteer de bovenliggende account, meerdere accounts of selecteren.
    - **Service**
        - Selecteer een service, meerdere services of alle services.
    - **Provider**
        - De cloudprovider waarin activa en kosten gekoppeld worden.
    - **Regio**
        - De regio waar de resource wordt gehost.
    - **Beschikbaarheid Zone**
        - AWS geïsoleerd locaties binnen een regio.
    - **Resourcetype**
        - Het type van de bron wordt gebruikt.
    - **Sub-Type**
        - Selecteer het subtype.
    - **Bewerking**
        - Selecteer de bewerking of **Alles weergeven**.
    - **Prijsmodel**
        - Alle tevoren
        - Er is geen tevoren
        - Gedeeltelijk tevoren
        - Op aanvraag
        - Reservering
        - Positie
    - **Kosten Type**
        - Selecteer de kosten van het type negatief of positief of beide.
    - **Tenancy**
        - Hiermee wordt aangegeven of een machine wordt uitgevoerd als een specifieke computer.
    -   **Gebruikstype**
          - Gebruikstype kan eenmalige kosten of terugkerende kosten zijn.

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
    - Wekelijks
    - Maandelijks
    - Per kwartaal
    - Jaarlijks

9. **Toewijzingsregels**

    Gebruik toewijzingsregels toe te passen of uitschakelen van de kostentoewijzing kosten herberekening. U kunt in- of uitschakelen van de herberekening van de toewijzing van kosten voor factureringsgegevens. De herberekening geldt voor de geselecteerde categorieën in het rapport. Hiermee kunt u nagaan wat het resultaat kosten toewijzing herberekening tegen onbewerkte factureringsgegevens.

10. **Niet-gecategoriseerde**

    Gebruik niet-gecategoriseerd wilt opnemen of uitsluiten van niet-gecategoriseerde kosten in het rapport.

11. **Velden weergeven/verbergen**

    De optie weergeven/verbergen heeft geen invloed in rapporten.

12.   **weergave-indelingen**

    Gebruik de weergave-indelingen verschillende grafiek of tabel weergaven selecteren.

    ![weergave-indelingen](./media/understanding-cost-reports/display-formats.png)

13. **Multi-color**

    Meerdere kleuren gebruiken om in te stellen de kleur van de grafieken in het rapport.

14. **Acties**

    Gebruik acties wilt opslaan, exporteren of het rapport plant.

## <a name="save-and-schedule-reports"></a>Opslaan en rapporten plannen

Nadat u een rapport maakt, kunt u deze opslaan voor toekomstig gebruik. Opgeslagen rapporten zijn beschikbaar in **My Tools** > **mijn rapporten**. Als u wijzigingen in een bestaand rapport aanbrengen en dit opslaat, wordt het rapport wordt opgeslagen als een nieuwe versie. Of kunt u deze opslaan als een nieuw rapport.

### <a name="save-a-report-to-the-cloudyn-portal"></a>Een rapport opslaan in de portal Cloudyn

Wanneer u rapporten weergeeft, klikt u op **acties** en selecteer vervolgens **opslaan in Mijn rapporten**. Het rapport een naam en voeg vervolgens een zelf-URL of de automatisch gemaakte URL gebruiken. U kunt eventueel **delen** het rapport openbaar met anderen in uw organisatie of u kunt delen op uw entiteit. Als u het rapport niet deelt, blijft deze persoonlijk en kunt u alleen weergeven. Sla het rapport.


### <a name="save-a-report-to-cloud-provider-storage"></a>Een rapport in de cloud provider opslag opslaan

Om een rapport opslaan in uw cloudserviceprovider, moet u al hebt geconfigureerd een opslagaccount. Wanneer u rapporten weergeeft, klikt u op **acties** en selecteer vervolgens **rapport plant**. Het rapport een naam en voeg vervolgens een zelf-URL of de automatisch gemaakte URL gebruiken. Selecteer **opslaan naar opslag** en selecteer het opslagaccount of toevoegen van een nieuwe account. Voer een voorvoegsel dat wordt toegevoegd aan de naam van het rapport. Selecteer een CSV- of JSON-indeling en sla het rapport.

### <a name="schedule-a-report"></a>Een rapport plannen

U kunt rapporten uitvoeren met regelmatige tussenpozen en u kunt ze verzonden naar een ontvanger lijst of cloud serviceprovider storage-account. Wanneer u rapporten weergeeft, klikt u op **acties** en selecteer vervolgens **rapport plant**. U kunt het rapport verzenden via e-mail en opslaan in een opslagaccount. Onder **planning**, selecteert u het interval (dagelijks, wekelijks of maandelijks). Selecteer de dag of datums om uit te leveren en selecteer de tijd voor een wekelijkse en maandelijkse. Sla het geplande rapport op. Als u de indeling van de Excel-rapport selecteert, wordt het rapport als bijlage verzonden. Wanneer u inhoud e-mailadresnotatie selecteert, worden de rapportresultaten die worden weergegeven in de grafiekopmaak geleverd als een grafiek.

### <a name="export-a-report-as-a-csv-file"></a>Een rapport exporteren als een CSV-bestand

Wanneer u rapporten weergeeft, klikt u op **acties** en selecteer vervolgens **alle rapportgegevens exporteren**. Een pop-upvenster wordt weergegeven en een CSV-bestand wordt gedownload.

## <a name="next-steps"></a>Volgende stappen

- Als u nog niet de eerste zelfstudie hebt voltooid voor het beheer van kosten, leest u op het [gebruik en kosten bekijken](tutorial-review-usage.md).
