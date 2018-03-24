---
title: Kosten-rapporten gebruiken in Azure kosten Management | Microsoft Docs
description: In dit artikel wordt beschreven hoe verschillende kostenbeheer rapporten gebruiken in de portal Cloudyn.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: fa263a4b6f41e2b31328f46b8d1341d0d74c9a85
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="use-cost-management-reports"></a>Kostenbeheerrapporten gebruiken

In dit artikel wordt beschreven hoe verschillende kostenbeheer rapporten gebruiken in de portal Cloudyn. De meeste Cloudyn rapporten zijn intuïtieve en een uniform uiterlijk hebben. Zie voor een overzicht over Cloudyn rapporten [Understanding kosten verbonden aan rapporten](understanding-cost-reports.md). Het artikel beschrijft ook verschillende opties en de velden in de meeste rapporten gebruikt.

## <a name="cost-analysis-reports"></a>Kosten verbonden aan rapporten

Rapporten van kosten weergegeven facturering gegevens van uw Cloud-providers. Met behulp van de rapporten, kunt u groeperen en inzoomen op verschillende gegevenssegmenten gespecificeerd in het bestand facturering. De rapporten inschakelen gedetailleerde kosten navigatie tussen de cloud leveranciers onbewerkte factureringsgegevens.

Kosten analyserapporten niet kosten gegroepeerd door tags. Tag-rapporten is alleen beschikbaar in de toewijzing van de kosten-rapporten instellen nadat u een kostprijsmodel met kosten toewijzing 360 maakt.

### <a name="actual-cost-analysis"></a>Actual Cost Analysis

Het rapport analyse van de werkelijke kosten toont de medewerkers van uw belangrijkste kosten, met inbegrip van lopende kosten en eenmalige kosten.

 Het rapport analyse van de werkelijke kosten om te gebruiken:

- Analyseren en controleren van de werkelijke kosten die gedurende een opgegeven periode is besteed
- Plannen van een drempelwaarde voor waarschuwing
- Analyseer showback en terugstorting kosten

#### <a name="to-use-the-actual-cost-analysis-report"></a>Gebruik van het rapport analyse van de werkelijke kosten

Ten minste de volgende stappen uitvoeren. U kunt ook andere opties en velden.

1. Selecteer een datumbereik.
2. Selecteer een filter.

Met de rechtermuisknop op resultaten rapporteren als u wilt inzoomen ze en meer gedetailleerde informatie weer te geven.

![Voorbeeld van een werkelijke kostenanalyse](./media/use-reports/actual-cost-analysis.png)

### <a name="actual-cost-over-time"></a>Werkelijke kosten gedurende een periode

Het rapport werkelijke kosten gedurende een periode is een standaardkosten analyserapport kosten distribueren over een vastgesteld tijdvenster resolutie. Het rapport bevat uitgaven na verloop van tijd kunt u trends zien en bestedingslimiet onregelmatigheden worden gedetecteerd. Dit rapport geeft de medewerkers van uw belangrijkste kosten inclusief lopende kosten en eenmalige gereserveerde exemplaar kosten die worden besteed gedurende een geselecteerde periode.

Het rapport werkelijke kosten gedurende een periode om te gebruiken:

- Zie kosten trends na verloop van tijd.
- Onregelmatigheden in kosten zoeken.
- Alle kosten-gerelateerde vragen met betrekking tot de Amazon Web Services vinden.

#### <a name="to-use-the-actual-cost-over-time-report"></a>Gebruik de werkelijke kosten gedurende een periode-rapport:

Ten minste de volgende stappen uitvoeren. U kunt ook andere opties en velden.

- Selecteer een datumbereik.

Bijvoorbeeld, kunt u groepen om weer te geven van de kosten gedurende een bepaalde periode. En voeg vervolgens de filters voor uw resultaten beperken.

![Voorbeeld van een werkelijke kosten gedurende een periode](./media/use-reports/actual-cost-over-time.png)



### <a name="amortized-cost-reports"></a>Afgeschreven kosten-rapporten

Deze reeks afgeschreven kosten rapporten linearisatie bevat niet-gebruik op basis van servicetarief of eenmalige leveranciers kosten en de daaraan verbonden kosten verspreid over gelijkmatig tijdens hun levensduur.

Bijvoorbeeld kunnen eenmalige kosten opnemen:

- Jaarlijkse kosten van ondersteuning
- Jaarlijkse beveiliging onderdeel kosten
- Gereserveerde exemplaren kopen kosten
- Sommige Azure Marketplace-items

In het bestand facturering eenmalige kosten worden gekenmerkt wanneer de service verbruik begin- en einddatum- of timestamp, gelijk waarden hebben. Cloudyn herkent vervolgens als eenmalige kosten die kunnen worden afgelost. Andere services op basis van verbruik met gebruikskosten op aanvraag kunnen niet worden afgelost.

Bekijk de volgende voorbeeldafbeelding van een rapport van de tijd werkelijke kosten via ter illustratie van afgeschreven kosten. In het voorbeeld wordt een piek kosten op augustus 23. Het lijkt misschien een afwijkingsdetectie vergeleken met de gebruikelijke dagelijkse trend in de kosten. Analyse van de hoofdoorzaak en gegevens navigatie deze kosten geïdentificeerd als een jaarlijkse AWS service APN-reservering, dit is een eenmalige kosten die zijn gekocht en kosten in rekening gebracht op die dag. U kunt zien hoe deze kosten is afgelost in de volgende sectie.

![Werkelijke kosten gedurende een periode rapport voorbeeld met eenmalige kosten](./media/use-reports/actual-amort-example.png)

#### <a name="to-use-the-amortized-cost-over-time-report"></a>Het rapport Over tijd kosten afgelost gebruiken:

Ten minste de volgende stappen uitvoeren. U kunt ook andere opties en velden.

1. Selecteer een datumbereik.
2. Selecteer een Service en een Provider.

Uitvoering doorsturen in het vorige voorbeeld, kunt u zien dat de eenmalige kosten nu wordt afgelost in de volgende afbeelding:

![Voorbeeld van een afgeschreven kosten gedurende een periode](./media/use-reports/amort-cost-over-time.png)

De voorgaande afbeelding toont de afgeschreven kosten voor de kosten van de reservering APN gedurende een bepaalde periode. Dit rapport geeft de afschrijving eenmalige kosten en de kosten van de APN als een jaarlijkse reservering aankoop. De kosten van de APN gelijkmatig dagelijks als 1/365th van de reservering vooraf kosten.

## <a name="cost-allocation-analysis-reports"></a>Kosten verbonden aan rapporten voor toewijzing

Kosten toewijzing Analysis rapporten zijn beschikbaar nadat u een kostprijsmodel met kosten toewijzing 360 maakt. Cloudyn kosten/factureringsgegevens verwerkt en overeenkomt met de gegevens op de informatie over het gebruik en code van uw cloud-accounts. Cloudyn vereist zodat deze overeenkomen met de gegevens, toegang tot uw gegevens over het gebruik. Accounts die ontbreken referenties, worden aangeduid als niet-gecategoriseerde resources.

### <a name="cost-analysis-report"></a>Kostenanalyserapport

Het rapport analyse van kosten verschaft inzicht in uw cloud gebruiks- en uitgaven gedurende een geselecteerde periode. De beleidsregels voor kosten toewijzing Manager worden gebruikt in het rapport analyse van de kosten.

Hoe wordt Cloudyn berekend in dit rapport?

Cloudyn zorgt ervoor dat toegewezen behoudt de integriteit van elke gekoppelde account door toe te passen Account affiniteit. Affiniteit zorgt u ervoor een account die geen van een specifieke service gebruikmaakt heeft geen eventuele kosten van deze service is toegewezen. De kosten in dat account in het account blijven en niet worden berekend door de beleidsregels voor brontoewijzing. Bijvoorbeeld wellicht vijf gekoppelde accounts. Als er slechts drie ervan opslagservices gebruiken, klikt u vervolgens de kosten van het storage-services alleen toegewezen over labels in de drie accounts.

 Het rapport analyse van de kosten te gebruiken:

- Een samengevoegde weergave van uw hele implementatie voor een specifiek tijdsbestek.
- Kosten weergeven door de tag categorieën op basis van beleid dat is gemaakt in het kostprijsmodel.

#### <a name="to-use-the-cost-analysis-report"></a>Het rapport analyse van kosten gebruiken:

1. Selecteer een datumbereik.
2. Labels, toevoegen, indien nodig.
3. Groepen toevoegen.
4. Kies een kostprijsmodel die u eerder hebt gemaakt.

De volgende afbeelding toont een voorbeeld kosten analyserapport sunburst-indeling. De ringen weergeven groepen. De buitenste ring toont Service en de interne cirkel eenheid bevat.

![Voorbeeld van een analyse kosten](./media/use-reports/cost-analysis01.png)



Hier volgt een voorbeeld van dezelfde gegevens in een tabel.

![Voorbeeld van een analyse kosten](./media/use-reports/cost-analysis02.png)



### <a name="cost-over-time-report"></a>Rapport met overuren

Het rapport kosten gedurende een periode bevat uitgaven gedurende een bepaalde periode, zodat u kunt zien van trends en onregelmatigheden in uw implementatie detecteren. In wezen ziet u kosten verdeeld over een opgegeven periode. Het rapport bevat de medewerkers van uw belangrijkste kosten inclusief lopende kosten en eenmalige gereserveerde exemplaar kosten die worden besteed gedurende een geselecteerde periode. Beleid dat is ingesteld in kosten Manager 360° kunnen worden gebruikt in dit rapport.

Het rapport kosten gedurende een periode om te gebruiken:

- Wijzigingen voor een bepaalde tijd en welke invloed wijzigen van één dag (of datumbereik) in de volgende bekijken.
- Analyseer kosten gedurende een bepaalde periode voor een specifiek exemplaar.
- Begrijpen waarom er een kostenverhoging van de voor een specifiek exemplaar is.

#### <a name="to-use-the-cost-over-time-report"></a>Het rapport kosten gedurende een periode gebruiken:

1. Selecteer een datumbereik.
2. Labels, toevoegen, indien nodig.
3. Groepen toevoegen.
4. Kies een kostprijsmodel die u eerder hebt gemaakt.
5. Selecteer de werkelijke kosten of afgeschreven kosten.
6. Kies of u toewijzingsregels toepassen op weergave onbewerkte gegevensweergave facturering of berekend naar kosten weergeven.

Hier volgt een voorbeeld van het rapport.

![Voorbeeld van de kosten gedurende een periode](./media/use-reports/cost-over-time.png)



## <a name="next-steps"></a>Volgende stappen

- Als u nog niet de eerste zelfstudie hebt voltooid voor het beheer van kosten, leest u op het [gebruik en kosten bekijken](tutorial-review-usage.md).
