---
title: Een gegevensset verwijzing toevoegen aan uw omgeving Azure Time Series Insights
description: Dit artikel wordt beschreven hoe u een gegevensset verwijzing naar het verbeteren van de gegevens in uw omgeving Azure Time Series Insights toevoegt.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/15/2018
ms.openlocfilehash: 962c3c8d439884cacf010b0bbcab03bd216dbd43
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294197"
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Een verwijzing gegevensset voor uw Time Series Insights-omgeving met de Azure portal maken

In dit artikel wordt beschreven hoe een gegevensset verwijzing toevoegen aan uw Azure Time Series Insights-omgeving. Referentiegegevens is handig voor het koppelen aan de brongegevens aan de waarden te verbeteren.

Een gegevensset verwijzing is een verzameling van items die verbeteren van de gebeurtenissen uit de gebeurtenisbron. Tijd reeks Insights inkomend engine koppelt elke gebeurtenis van de gebeurtenisbron-met de bijbehorende gegevensrij in uw gegevensset verwijzing. Deze uitgebreide gebeurtenis is vervolgens beschikbaar voor query’s. Deze koppeling is gebaseerd op de primaire sleutel kolommen gedefinieerd in uw gegevensset verwijzing.

Referentiegegevens met terugwerkende kracht niet is toegevoegd. Dit betekent dat alleen de gegevens van de huidige en toekomstige inkomend komt overeen met en gekoppeld aan de verwijzing naar datum is ingesteld, zodra deze is geconfigureerd en geüpload.

## <a name="add-a-reference-data-set"></a>Een gegevensset verwijzing toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek uw bestaande Time Series Insights-omgeving. Klik op **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Selecteer de **overzicht** pagina. Zoek de **Time Series Insights explorer URL** en open de koppeling.  

   De Verkenner voor uw omgeving TSI weergeven.

4. Vouw de selector omgeving in TSI explorer. Kies de actieve omgeving. Selecteer het pictogram van de verwijzing naar gegevens op de rechterbovenhoek op de pagina explorer.

   ![Referentiegegevens toevoegen](media/add-reference-data-set/add_reference_data.png)

5. Selecteer de **+ toevoegen van een gegevensset** knop om te beginnen met het toevoegen van een nieuwe gegevensset.

   ![De set gegevens toevoegen](media/add-reference-data-set/add_data_set.png)

6. Op de **nieuw verwijst naar gegevensset** pagina, kiest u de indeling van de gegevens: 
   - Kies **CSV** voor CSV-gegevens. De eerste rij wordt als koprij behandeld. 
   - Kies **JSON-matrix** voor javascript object notation (JSON) opgemaakte gegevens.

   ![Kies gegevensindeling.](media/add-reference-data-set/add_data.png)

7. Geef de gegevens, met een van de twee methoden:
   - De gegevens in de teksteditor plakken. Selecteer **Parse referentiegegevens** knop.
   - Selecteer **bestand kiezen** knop gegevens toevoegen via een lokaal tekstbestand. 

   Plak bijvoorbeeld CSV-gegevens: ![geplakt CSV-gegevens](media/add-reference-data-set/csv_data_pasted.png)

   Plak bijvoorbeeld JSON-matrix-gegevens: ![plakken JSON-gegevens](media/add-reference-data-set/json_data_pasted.png)

   Als er een fout bij het parseren van de gegevenswaarden, de fout wordt weergegeven in rood aan de onderkant van de pagina, zoals `CSV parsing error, no rows extracted`.

8. Zodra de gegevens wordt geparseerd, kan een gegevensraster wordt weergegeven om de kolommen en rijen representatie van de gegevens weer te geven.  Bekijk het gegevensraster om ervoor te zorgen juist is.

   ![Referentiegegevens toevoegen](media/add-reference-data-set/parse_data.png)

9. Bekijk elke kolom als u wilt zien van het gegevenstype wordt aangenomen en het gegevenstype wijzigen indien nodig.  Selecteer het type gegevens symbool in de kolomkop: **#** voor dubbele (numerieke gegevens), **T | F** als Boole-waarden, of **Abc** voor tekenreeks.

   ![Gegevenstypen op de kolomkoppen kiezen.](media/add-reference-data-set/choose_datatypes.png)

10. Wijzig de naam van de kolomkoppen indien nodig. De naam van de sleutelkolom is nodig om toe te voegen aan de overeenkomende eigenschap in de gebeurtenisbron. Zorg ervoor dat de verwijzing sleutelkolomnamen exact met de naam van de gebeurtenis naar uw binnenkomende gegevens overeenkomen, inclusief hoofdlettergevoeligheid. De namen van de niet-sleutelkolom worden gebruikt voor het verbeteren van de inkomende gegevens met de waarden van de bijbehorende verwijzing.

11. Klik op **toevoegen van een rij** of **een kolom toevoegen** om toe te voegen meer gegevens referentiewaarden, indien nodig.

12. Typ een waarde in de **de rijen filteren...**  veld om te controleren van bepaalde rijen zo nodig. Het filter is nuttig voor het evalueren van gegevens, maar wordt niet toegepast wanneer de gegevens worden geüpload.
 
13. Naam voor de set gegevens door in te vullen de **gegevenssetnaam** veld boven het gegevensraster.

   ![De naam van de gegevensset.](media/add-reference-data-set/name_reference_dataset.png)

14. Geef de **primaire sleutel** kolom in de gegevensset, door het selecteren van de vervolgkeuzelijst boven het gegevensraster.

   ![Selecteer de-sleutelkolommen.](media/add-reference-data-set/set_primary_key.png)

   Selecteer desgewenst de **+** om een secundaire sleutelkolom toevoegen als een samengestelde primaire sleutel. Als u de selectie ongedaan maken moet, kiest u de lege waarden in de vervolgkeuzelijst om te verwijderen van de secundaire sleutel.

15.  Als u wilt de gegevens uploaden, selecteer de **rijen uploaden** knop.

   ![Uploaden](media/add-reference-data-set/upload_rows.png)

   De pagina bevestigt de voltooide uploaden en het bericht wordt weergegeven **geüpload gegevensset**.

## <a name="next-steps"></a>Volgende stappen
* Programmatisch [referentiegegevens beheren](time-series-insights-manage-reference-data-csharp.md).
* Zie voor de volledige API-verwijzing het document [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
