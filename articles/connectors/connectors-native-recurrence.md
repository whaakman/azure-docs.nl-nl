---
title: Taken plant en regelmatig actieve werkstromen - Azure Logic Apps | Microsoft Docs
description: Maken en plannen regelmatig actieve taken, acties, werkstromen, processen en werkbelastingen met logic apps
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 9384752c7f12074aae6ff165241e954eb2a4a01e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="schedule-tasks-and-workflows-that-run-regularly-with-logic-apps"></a>Plannen van taken en werkstromen die regelmatig wordt uitgevoerd met logic apps

Als u wilt plannen van taken, acties, werkbelastingen of processen die regelmatig wordt uitgevoerd, kunt u een logische app werkstroom die met begint de **schema - terugkeerpatroon** [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts). Met deze trigger kunt u een datum en tijd voor het starten van het terugkeerpatroon en een terugkerende planning voor het uitvoeren van taken, zoals deze voorbeelden en nog veel meer kunt instellen:

* Interne gegevens ophalen: [uitvoeren van een opgeslagen SQL-procedure](../connectors/connectors-create-api-sqlazure.md) elke dag.
* Externe gegevens ophalen: Pull-rapporten weer van NOAA om de 15 minuten.
* Rapportgegevens: E-een samenvatting voor alle orders groter is dan een bepaald bedrag in de afgelopen week.
* Gegevens verwerken: comprimeren vandaag heeft geüpload installatiekopieën elke weekdag tijdens de daluren.
* Opruimen van gegevens: verwijderen van alle tweets ouder is dan drie maanden.
* Gegevens archiveren: facturen Push naar een back-upservice elke maand.

Deze trigger ondersteunt veel patronen, bijvoorbeeld:

* Direct uitgevoerd en herhalen elke  *n*  aantal seconden, minuten, uren, dagen weken of maanden.
* Start op een bepaald tijdstip, en vervolgens uitvoeren en herhaal elke  *n*  aantal seconden, minuten, uren, dagen, weken of maanden.
* Voer en Herhaal op een of meer keer per dag, bijvoorbeeld op 8:00 uur en 17:00 uur.
* Voer en herhaal elke week, maar alleen voor specifieke dagen, zoals zaterdag en zondag.
* Uitvoeren en herhaalt u elke week, maar alleen voor bepaalde dagen en tijden, zoals maandag tot vrijdag 8:00 uur en 17:00 uur.

Wanneer de trigger terugkeerpatroon wordt telkens gestart, wordt Logic Apps maakt en een nieuw exemplaar van uw logische app werkstroom wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

## <a name="add-a-recurrence-trigger-to-your-logic-app"></a>Een terugkeerpatroon trigger toevoegen aan uw logische app

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Een lege logische app maken of meer [het maken van een lege logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Nadat Logic Apps Designer wordt weergegeven, in het zoekvak, geeft u "recurrence" Als uw filter. Selecteer de **schema - terugkeerpatroon** trigger. 

   ![Schema - trigger terugkeerpatroon](./media/connectors-native-recurrence/add-recurrence-trigger.png)

   Deze trigger is nu de eerste stap in uw logische app.

3. Stel het interval en de frequentie voor het terugkeerpatroon in. Stel deze eigenschappen uw werkstroom elke week worden uitgevoerd in dit voorbeeld. 

   ![Set-interval en frequentie](./media/connectors-native-recurrence/recurrence-trigger-details.png)

4. Kies voor meer planningsopties **geavanceerde opties weergeven**. 

   ![Meer opties](./media/connectors-native-recurrence/recurrence-trigger-more-options.png)

5. Nu kunt u deze opties instellen: 

   * Stel een begindatum en -tijd voor het starten van de trigger. 
   Als u een begindatum en -tijd opgeeft, kunt u ook een tijdzone toepassen. 

   * Als u 'Day' of 'Week' voor de frequentie selecteert, kunt u specifieke tijden voor het terugkeerpatroon. 

   * Als u 'Week', kunt u specifieke dagen van de week te selecteren.
   
   ![Geavanceerde opties voor planning](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   Stel dat het vandaag is maandag 4 September 2017. 
   De volgende terugkeerpatroon trigger wordt niet gestart *eventuele vroeger* dan de begindatum en de tijd die maandag 18 September 2017 is op 8:00 AM PST. 
   Echter, de terugkeerplanning is ingesteld voor 10:30 uur, 12:30 PM en 2:30 PM op maandag alleen. Daarom de eerste keer dat de trigger wordt geactiveerd en maakt u een logische app-workflowexemplaar is op 10:30 uur. 
   Zie voor meer informatie over hoe start time-out voor werk, deze [start time-voorbeelden](#start-time).
   Toekomstige uitgevoerd gebeuren op 12:30 PM en 2:30 PM op dezelfde dag. 
   Elke herhaling wordt gemaakt van hun eigen workflowexemplaar. Daarna wordt de hele planning herhaald alle via opnieuw volgende maandag. 
   [*Wat zijn enkele andere voorbeeld-exemplaren?*](#example-recurrences)

   ![Voorbeeld van de geavanceerde planning](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > De trigger wordt een voorbeeld voor de opgegeven waarde voor het terugkeerpatroon alleen wanneer u 'Day' of 'Week' als de frequentie selecteert.
   
6. Nu uw resterende werkstroom met acties bouwen of besturingsinstructies stromen. Zie voor meer acties die u kunt toevoegen, [Connectors](../connectors/apis-list.md). 

## <a name="trigger-details"></a>Details van de trigger

U kunt deze eigenschappen voor de trigger terugkeerpatronen configureren.

| Naam | Vereist | Naam van eigenschap | Type | Beschrijving | 
|----- | -------- | ------------- | ---- | ----------- | 
| **Frequentie** | Ja | frequency | Tekenreeks | De tijdseenheid voor het terugkeerpatroon: **tweede**, **minuut**, **uur**, **dag**, **Week**, of  **Maand** | 
| **Interval** | Ja | interval | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de werkstroom wordt uitgevoerd op basis van de frequentie. <p>Het standaardinterval is 1. Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uurs: 1-12.000 uur </br>-Minuut: 1-72.000 minuten </br>-Tweede: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval is ingesteld op 6 en de frequentie is "Maand", is klikt u vervolgens het terugkeerpatroon elke 6 maanden. | 
| **Tijdzone** | Nee | timeZone | Tekenreeks | Geldt alleen wanneer u een begintijd opgeeft omdat deze trigger niet accepteren [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Selecteer de tijdzone die u wilt toepassen. | 
| **Begintijd** | Nee | startTime | Tekenreeks | Geef een begin-tijdstip in deze indeling: <p>JJJJ-MM-ddTUU als u een tijdzone selecteren <p>-of- <p>JJJJ-MM-ssZ als u niet een tijdzone selecteren <p>Dus bijvoorbeeld, als u wilt dat 18 September 2017 op 14:00 uur, geeft u "2017-09-18T14:00:00 ' en selecteer een tijdzone zoals Pacific Time. Of geef ' 2017-09-18T14:00:00Z ' zonder een tijdzone. <p>**Opmerking:** de begintijd moet volgen de [ISO 8601 datum, tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Als u een tijdzone niet selecteert, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze 'Z"verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). <p>Voor eenvoudige schema's van de begintijd is de eerste keer voorkomt, terwijl voor complexe schema's van de trigger vroeger dan de begintijd wordt niet gestart. [*Wat zijn de manieren waarop ik de begindatum en -tijd kunnen gebruiken?*](#start-time) | 
| **Op deze dagen** | Nee | weekDays | String of string array | Als u 'Week' selecteert, kunt u een of meer dagen wanneer u wilt uitvoeren van de werkstroom: **maandag**, **dinsdag**, **woensdag**, **donderdag** , **Vrijdag**, **zaterdag**, en **zondag** | 
| **Op deze uren** | Nee | hours | Geheel getal of een matrix van geheel getal | Als u 'Day' of 'Week' selecteert, kunt u een of meer gehele getallen tussen 0 en 23 als het uur van de dag waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, als u '10', '12' en '14' opgeeft, krijgt u 10 uur, 12 uur en 14 uur als de markeringen uur. | 
| **Op deze minuten** | Nee | minutes | Geheel getal of een matrix van geheel getal | Als u 'Day' of 'Week' selecteert, kunt u een of meer gehele getallen tussen 0 en 59 als de minuten van het uur waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, kunt u '30' als de minuut is ingeschakeld en met het vorige voorbeeld voor het uur van de dag, krijgt u 10:30 AM, 12:30 PM en 2:30 PM. | 
||||| 

## <a name="json-example"></a>JSON-voorbeeld

Hier volgt een voorbeeld terugkeerpatroon trigger definitie:

``` json
{
    "triggers": {
        "Recurrence": {
            "type": "Recurrence",
            "recurrence": {
                "frequency": "Week",
                "interval": 1,
                "schedule": {
                    "hours": [
                        10,
                        12,
                        14
                    ],
                    "minutes": [
                        30
                    ],
                    "weekDays": [
                        "Monday"
                    ]
                },
               "startTime": "2017-09-07T14:00:00",
               "timeZone": "Pacific Standard Time"
            }
        }
    }
}
```

## <a name="faq"></a>Veelgestelde vragen

<a name="example-recurrences"></a>

**V:** wat andere voorbeeld terugkerende schema's zijn? </br>
**A:** Hier vindt u meer voorbeelden:

| Terugkeerpatroon | Interval | Frequentie | Begintijd | Deze dagen | Deze uren | Deze minuten | Opmerking |
| ---------- | -------- | --------- | ---------- | ------------- | -------------- | ---------------- | ---- |
| Elke 15 minuten (geen begindatum en -tijd) wordt uitgevoerd | 15 | Minuut | {none} | {unavailable} | {none} | {none} | Dit schema wordt onmiddellijk gestart en vervolgens toekomstige herhalingen op basis van de laatste uitvoeringstijd berekent. | 
| Om de 15 minuten uitgevoerd (met een begindatum en -tijd) | 15 | Minuut | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Dit schema wordt niet gestart *eventuele vroeger* dan de opgegeven begindatum en -tijd, berekent vervolgens toekomstige herhalingen op basis van de laatste uitvoeringstijd. | 
| Elk uur wordt uitgevoerd op het uur (met een begindatum en -tijd) | 1 | Uur | *startDate*Thh:00:00Z | {unavailable} | {none} | {none} | Dit schema wordt niet gestart *eventuele vroeger* dan de opgegeven begindatum en -tijd. Toekomstige herhalingen wordt elk uur uitgevoerd op de minuut '00' is ingeschakeld. <p>Als de frequentie 'Week' of 'Maand', voert deze planning respectievelijk slechts één dag per week of één dag per maand. | 
| Elk uur, dagelijks (geen begindatum en -tijd) uitgevoerd | 1 | Uur | {none} | {unavailable} | {none} | {none} | Dit schema wordt onmiddellijk gestart en toekomstige herhalingen op basis van de laatste uitvoeringstijd berekent. <p>Als de frequentie 'Week' of 'Maand', voert deze planning respectievelijk slechts één dag per week of één dag per maand. | 
| Elk uur, dagelijks (met een begindatum en -tijd) uitgevoerd | 1 | Uur | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Dit schema wordt niet gestart *eventuele vroeger* dan de opgegeven begindatum en -tijd, berekent vervolgens toekomstige herhalingen op basis van de laatste uitvoeringstijd. <p>Als de frequentie 'Week' of 'Maand', voert deze planning respectievelijk slechts één dag per week of één dag per maand. | 
| Om de 15 minuten na het uur, elk uur uitgevoerd (met een begindatum en -tijd) | 1 | Uur | *startDate*T00:15:00Z | {unavailable} | {none} | {none} | Dit schema wordt niet gestart *eventuele vroeger* dan de opgegeven begindatum en -tijd, uitgevoerd op 00:15 uur, 01:15:00, 2:15 uur, enzovoort. | 
| Elke 15 minuten na het uur, elk uur (geen begindatum en -tijd) wordt uitgevoerd | 1 | Dag | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Deze planning wordt uitgevoerd op 00:15 uur, 01:15:00, 2:15 uur, enzovoort. Ook komt deze planning overeen met een frequentie van een "Uur" en een begintijd met "15" minuten. | 
| Om de 15 minuten worden uitgevoerd bij de 15 minuten (geen begindatum en -tijd) | 1 | Dag | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dit schema start niet totdat de volgende 15 minuten is ingeschakeld opgegeven. | 
| Voer op elke dag (geen begindatum en -tijd) 8:00 uur | 1 | Dag | {none} | {unavailable} | 8 | {none} | Deze planning wordt uitgevoerd op 8:00 uur per dag uit, op basis van de opgegeven planning. | 
| Voer op elke dag (met een begindatum en -tijd) 8:00 uur | 1 | Dag | *startDate*T08:00:00Z | {unavailable} | {none} | {none} | Dit schema 8:00 uur per dag uit, op basis van de opgegeven begintijd wordt uitgevoerd. | 
| Voer op 8:30 uur elke dag (geen begindatum en -tijd) | 1 | Dag | {none} | {unavailable} | 8 | 30 | Deze planning wordt uitgevoerd op 8:30 uur per dag uit, op basis van de opgegeven planning. | 
| Voer op 8:30 uur elke dag (met een begindatum en -tijd) | 1 | Dag | *startDate*T08:30:00Z | {unavailable} | {none} | {none} | Deze planning begint op de opgegeven begindatum om 8:30 uur. | 
| Uitvoeren op 8:30 uur en elke dag 4:30 PM | 1 | Dag | {none} | {unavailable} | 8, 16 | 30 | | 
| 8:30 AM, 8:45 uur uitgevoerd 4:30 PM en 4:45 uur elke dag | 1 | Dag | {none} | {unavailable} | 8, 16 | 30, 45 | | 
| Voer elke zaterdag om 17.00 (geen begindatum en -tijd) | 1 | Week | {none} | "Zaterdag" | 17 | 00 | Dit schema wordt elke zaterdag op 17:00 uur uitgevoerd. | 
| Voer elke zaterdag om 17.00 (met een begindatum en -tijd) | 1 | Week | *startDate*T17:00:00Z | "Zaterdag" | {none} | {none} | Dit schema wordt niet gestart *eventuele vroeger* dan de opgegeven begindatum en -tijd, in dit geval 9 September 2017 op 17:00 uur. Toekomstige herhalingen elke zaterdag op 17:00 uur uitgevoerd. | 
| Elke dinsdag, donderdag om 17: 00 uur wordt uitgevoerd | 1 | Week | {none} | "Dinsdag", "Donderdag" | 17 | {none} | Dit schema wordt elke dinsdag en donderdag op 17:00 uur uitgevoerd. | 
| Elk uur gedurende kantooruren uitgevoerd | 1 | Week | {none} | Selecteer alle dagen behalve zaterdag en zondag. | Selecteer de uren van de dag die u wilt. | Selecteer een aantal minuten van het uur die u wilt. | Bijvoorbeeld, als uw werkuren 8:00 uur tot 5:00 uur, selecteer "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" Als de uren van de dag. <p>Als uw werkuren 8:30 uur tot 5:30 uur, selecteert u de vorige uur van de dag plus '30' als het aantal minuten van het uur. | 
| Eenmaal elke dag worden uitgevoerd tijdens het weekend | 1 | Week | {none} | "Zaterdag", "Zondag" | Selecteer de uren van de dag die u wilt. | Selecteer een aantal minuten van het uur naar gelang van toepassing. | Dit schema wordt elke zaterdag en zondag uitgevoerd op de opgegeven planning. | 
| Om de 15 minuten tweewekelijks op maandag alleen worden uitgevoerd | 2 | Week | {none} | "Maandag" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Deze planning wordt uitgevoerd elke andere maandag op elke 15 minuten is ingeschakeld. | 
| Elk uur gedurende één dag per maand uitvoeren | 1 | Maand | {Zie Opmerking} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {Zie Opmerking} | Als u een begindatum en -tijd niet opgeeft, wordt dit schema gebruikt, de aanmaakdatum en -tijd. Geef het aantal minuten van het uur, een begintijd voor het beheren van de minuten voor de terugkerende planning, of gebruik van de aanmaaktijd. Bijvoorbeeld, als het begin-tijd of Aanmaaktijd 8:25 uur, deze planning wordt uitgevoerd op 8:25 uur, 9:25 uur, 10:25 uur, enzovoort. | 
||||||||| 

<a name="start-time"></a>

**V:** welke manieren dat ik de begindatum en -tijd kan gebruiken? </br>
**A:** hier zijn enkele patronen die laten zien hoe u terugkeerpatroon met de datum en tijd kunt beheren en hoe deze herhalingen wordt uitgevoerd in de engine voor Logic Apps:

| Begintijd | Recurrence zonder schedule | Recurrence met schedule | 
| ---------- | --------------------------- | ------------------------ | 
| {none} | De eerste werkbelasting onmiddellijk uitgevoerd. <p>Toekomstige werkbelasting op basis van de laatste uitvoeringstijd uitvoert. | De eerste werkbelasting onmiddellijk uitgevoerd. <p>Toekomstige werkbelastingen op basis van de opgegeven planning wordt uitgevoerd. | 
| De begintijd in het verleden | Berekent de uitvoeringstijden op basis van de opgegeven begintijd en de tijdstippen van afgelopen verwijderingen worden uitgevoerd. De eerste werkbelasting op de volgende toekomst uitvoeringstijd wordt uitgevoerd. <p>Toekomstige werkbelasting op basis van berekeningen van de laatste uitvoeringstijd uitvoert. <p>Zie het voorbeeld na deze tabel voor meer informatie. | De eerste werkbelasting wordt uitgevoerd *niet sneller* dan de begintijd op basis van de planning die wordt berekend op basis van de begintijd. <p>Toekomstige werkbelastingen op basis van de opgegeven planning wordt uitgevoerd. <p>**Opmerking:** als u een terugkeerpatroon opgeeft met een schema, maar geeft geen uren of minuten op voor de planning en toekomstige uitvoeringstijden zijn berekend met behulp van de uren of minuten, respectievelijk van de eerste keer uitvoeren. | 
| De begintijd op dit moment of in de toekomst | De eerste werkbelasting op de opgegeven begintijd wordt uitgevoerd. <p>Toekomstige werkbelasting op basis van berekeningen van de laatste uitvoeringstijd uitvoert. | De eerste werkbelasting wordt uitgevoerd *niet sneller* dan de begintijd op basis van de planning die wordt berekend op basis van de begintijd. <p>Toekomstige werkbelastingen op basis van de opgegeven planning wordt uitgevoerd. <p>**Opmerking:** als u een terugkeerpatroon opgeeft met een schema, maar geeft geen uren of minuten op voor de planning en toekomstige uitvoeringstijden zijn berekend met behulp van de uren of minuten, respectievelijk van de eerste keer uitvoeren. | 
||||

**Voorbeeld voor een eerdere begintijd met terugkeerpatroon maar zonder schema** 

| Begintijd | Huidige tijd | Terugkeerpatroon | Planning |
| ---------- | ------------ | ---------- | -------- | 
| 2017-09-**07**T14:00:00Z | 2017-09-**08**T13:00:00Z | 2 dagen | {none} | 
||||| 

In dit scenario wordt de Logic Apps engine wordt berekend op basis van de begintijd keer uitvoeren worden verwijderd uit het verleden uitvoeringstijden en maakt gebruik van de volgende toekomst begintijd voor de eerste keer uitvoert. Na die eerst wordt uitgevoerd, worden toekomstige wordt uitgevoerd op basis van de planning die wordt berekend op basis van de begintijd. Hier ziet u hoe dit terugkeerpatroon eruit ziet:

| Begintijd | Voer eerst | Toekomstige keer uitvoeren | 
| ---------- | ------------ | ---------- | 
| 2017-09 -**07** op 14:00 uur | 2017-09 -**09** op 14:00 uur | 2017-09 -**11** op 14:00 uur </br>2017-09 -**13** op 14:00 uur </br>2017-09 -**15** op 14:00 uur </br>enzovoort...
||||| 

Dus voor dit scenario, ongeacht hoe ver in het verleden opgeven van het begin tijd, bijvoorbeeld 2017-09 -**05** op 14:00 uur of 2017-09 -**01** tijdens 14:00 uur, uw eerste runtime is hetzelfde.

## <a name="next-steps"></a>Volgende stappen

* [Werkstroomacties en -triggers](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger)
* [Connectors](../connectors/apis-list.md)
