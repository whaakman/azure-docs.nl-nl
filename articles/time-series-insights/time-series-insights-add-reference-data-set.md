---
title: Een referentiegegevensset toevoegen aan uw Azure Time Series Insights-omgeving | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een referentiegegevensset aan het verbeteren van de gegevens in uw Azure Time Series Insights-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/15/2018
ms.custom: seodec18
ms.openlocfilehash: a14f568e2485e59cb3e567eb9ee5c5eb20b02afc
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198419"
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Een referentiegegevensset voor uw Time Series Insights-omgeving met behulp van de Azure portal maken

In dit artikel wordt beschreven hoe u een referentiegegevensset toevoegen aan uw Azure Time Series Insights-omgeving. Referentiegegevens is handig om lid te maken van uw gegevens om te verbeteren van de waarden.

Een Referentiegegevensset is een verzameling van items die de ervaring van de gebeurtenissen uit uw gebeurtenisbron. De engine voor Time Series Insights inkomende lid wordt van elke gebeurtenis uit uw gebeurtenisbron aan de desbetreffende gegevensrij in uw referentiegegevensset. Deze uitgebreide gebeurtenis is vervolgens beschikbaar voor query’s. Deze koppeling is gebaseerd op de primaire sleutel of meer kolommen gedefinieerd in uw referentiegegevensset.

Referentiegegevens is niet met terugwerkende kracht gekoppeld. Dit betekent dat alleen de gegevens van de huidige en toekomstige inkomend is afgestemd en toegevoegd aan de verwijzing datum is ingesteld, zodra deze is geconfigureerd en geüpload.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-time-series-insights-reference-data-modelbr"></a>In deze video behandelen we Time Series Insight referentie-gegevensmodel.</br>

> [!VIDEO https://www.youtube.com/embed/Z0NuWQUMv1o]

## <a name="add-a-reference-data-set"></a>Een referentiegegevensset toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw bestaande Time Series Insights-omgeving. Klik op **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Selecteer de **overzicht** pagina. Zoek de **Time Series Insights explorer-URL** en opent u de koppeling.  

   Bekijk de explorer voor uw omgeving TSI.

4. Vouw de omgevingsselectie in de TSI-Verkenner. Kies de actieve omgeving. Selecteer het pictogram van de gegevens verwijzing in de rechterbovenhoek op de pagina explorer.

   ![Naslaginformatie over gegevens toevoegen](media/add-reference-data-set/add_reference_data.png)

5. Selecteer de **+ toevoegen van een gegevensset** knop om te beginnen met het toevoegen van een nieuwe gegevensset.

   ![Gegevensset toevoegen](media/add-reference-data-set/add_data_set.png)

6. Op de **verwijzingsgegevensset voor nieuw** pagina, kies de indeling van de gegevens: 
   - Kies **CSV** voor gegevens met door komma's gescheiden. De eerste rij wordt beschouwd als een rij met koppen. 
   - Kies **JSON-matrix** voor javascript object notation (JSON)-opgemaakte gegevens.

   ![Kies de indeling.](media/add-reference-data-set/add_data.png)

7. Geef de gegevens, met behulp van een van de twee methoden:
   - Plak de gegevens in de teksteditor. Selecteer **verwijzingsgegevens parseren** knop.
   - Selecteer **bestand kiezen** knop voor het toevoegen van gegevens uit een lokale tekstbestand. 

   Plak bijvoorbeeld CSV-gegevens: ![Geplakte CSV-gegevens](media/add-reference-data-set/csv_data_pasted.png)

   Plak bijvoorbeeld JSON-matrix-gegevens: ![Plak de JSON-gegevens](media/add-reference-data-set/json_data_pasted.png)

   Als er een fout bij het parseren van de gegevenswaarden, de fout in het rood weergegeven aan de onderkant van de pagina, zoals `CSV parsing error, no rows extracted`.

8. Zodra de gegevens wordt geparseerd, wordt het weergeven van kolommen en rijen die de gegevens vertegenwoordigt een gegevensraster weergegeven.  Bekijk het gegevensraster om ervoor te zorgen juistheid.

   ![Naslaginformatie over gegevens toevoegen](media/add-reference-data-set/parse_data.png)

9. Controleer elke kolom als u wilt zien van het gegevenstype is uitgegaan en indien nodig, het gegevenstype wijzigen.  Selecteer het type gegevens symbool in de kolomkop: **#** voor dubbele (numerieke gegevens), **T | F** Boole-waarden, of **Abc** voor tekenreeks.

   ![Kies de gegevenstypen van de kolomkoppen.](media/add-reference-data-set/choose_datatypes.png)

10. Wijzig de naam van de kolomkoppen indien nodig. De naam van de sleutelkolom is nodig om lid te maken van de bijbehorende eigenschap in de gebeurtenisbron. Zorg ervoor dat de namen van verwijzing sleutelkolom exact met de naam van de gebeurtenis naar uw inkomende gegevens overeenkomen, met inbegrip van hoofdlettergevoeligheid. De namen van de niet-sleutelkolom worden gebruikt voor het verbeteren van de binnenkomende gegevens met de waarden van de bijbehorende verwijzing.

11. Klik op **toevoegen van een rij** of **een kolom toevoegen** om toe te voegen meer verwijzing gegevenswaarden, indien nodig.

12. Typ een waarde in de **de rijen filteren...**  veld om te controleren van bepaalde rijen naar behoefte. Het filter is handig voor het bekijken van gegevens, maar wordt niet toegepast wanneer de gegevens worden geüpload.
 
13. Naam van de gegevensset, door het invullen van de **naam van gegevensset** veld boven het gegevensraster.

   ![De naam van de gegevensset.](media/add-reference-data-set/name_reference_dataset.png)

14. Geef de **primaire sleutel** kolom in de gegevensset, door het selecteren van de vervolgkeuzelijst boven het gegevensraster.

   ![Selecteer de sleutel kolom(men).](media/add-reference-data-set/set_primary_key.png)

   Selecteer desgewenst de **+** om toe te voegen een kolom voor de secundaire sleutel als een samengestelde primaire sleutel. Als u nodig hebt om de selectie ongedaan te maken, kiest u de waarde is leeg in de vervolgkeuzelijst om te verwijderen van de secundaire sleutel.

15.  Als u wilt de gegevens uploaden, selecteert u de **rijen uploaden** knop.

   ![Uploaden](media/add-reference-data-set/upload_rows.png)

   De pagina wordt bevestigd dat de voltooide uploaden en het bericht weergegeven **geüpload gegevensset**.

## <a name="next-steps"></a>Volgende stappen
* Programmatisch [referentiegegevens beheren](time-series-insights-manage-reference-data-csharp.md).
* Zie voor de volledige API-verwijzing het document [Reference Data API](/rest/api/time-series-insights/ga-reference-data-api).
