---
title: Plannen en uitvoeren van geautomatiseerde taken en werkstromen met Azure Logic Apps | Microsoft Docs
description: Geplande en terugkerende taken automatiseren met de connector terugkeerpatroon in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
tags: connectors
ms.topic: article
ms.date: 01/08/2019
ms.openlocfilehash: 369bdba063f8582b8343682dcbbc990d2f63e21a
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54078062"
---
# <a name="create-and-run-recurring-tasks-and-workflows-with-azure-logic-apps"></a>Maken en uitvoeren van terugkerende taken en werkstromen met Azure Logic Apps

Als u wilt plannen acties, werkbelasting of processen die worden regelmatig uitgevoerd, maakt u een werkstroom voor logische Apps die met begint de **planning - terugkeerpatroon** [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts). U kunt een datum en tijd voor het starten van de werkstroom en een terugkeerpatroon op voor het uitvoeren van taken, zoals deze voorbeelden en meer instellen:

* Interne gegevens ophalen: [Uitvoeren van een in SQL opgeslagen procedure](../connectors/connectors-create-api-sqlazure.md) elke dag.
* Externe gegevens ophalen: Pull-weerberichten van NOAA om de 15 minuten.
* Rapportgegevens: E-mail een overzicht van alle orders groter is dan een bepaald bedrag in de afgelopen week.
* Gegevens verwerken: Comprimeren van vandaag geüploade afbeeldingen elke weekdag tijdens de daluren.
* Opschonen van gegevens: Verwijder alle tweets die ouder is dan drie maanden.
* Archiveer gegevens: Verzenden van facturen naar een back-upservice elke maand.

Deze trigger ondersteunt bijvoorbeeld veel patronen:

* Direct uitvoeren en herhalen elke *n* aantal seconden, minuten, uren, dagen, weken of maanden.
* Start op een bepaald tijdstip, en vervolgens uitvoeren en herhalen elke *n* aantal seconden, minuten, uren, dagen, weken of maanden.
* Voer en herhaald op een of meer keer per dag, bijvoorbeeld 8:00 uur en 17:00 uur.
* Uitvoeren en herhaalt u elke week, maar alleen voor specifieke dagen, zoals zaterdag en zondag.
* Uitvoeren en herhaalt u elke week, maar alleen voor specifieke dagen en tijden, zoals maandag tot en met vrijdag om 8:00 uur en 17:00 uur.

Wanneer de trigger met terugkeerpatroon wordt elke keer geactiveerd, worden de Logic Apps maakt en een nieuw exemplaar van de werkstroom van uw logische app wordt uitgevoerd. 

Als u wilt uw logische app direct te activeren en één keer zonder periodiek wordt uitgevoerd, Zie [uitvoeren taken slechts één keer](#run-once) verderop in dit onderwerp.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

## <a name="add-a-recurrence-trigger-to-your-logic-app"></a>Een trigger met Terugkeerpatroon toevoegen aan uw logische app

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Maak een lege logische app of meer [over het maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Nadat de ontwerper van logische Apps wordt weergegeven, onder het zoekvak typt, kiest u **alle**. Typ "terugkeerpatroon" als filter in het zoekvak. Selecteer deze trigger uit de lijst met triggers: **Herhaling - schema** 

   !['--Terugkeerschema' trigger selecteren](./media/connectors-native-recurrence/add-recurrence-trigger.png)

   Deze trigger is nu de eerste stap in uw logische app.

3. Stel het interval en de frequentie voor het terugkeerpatroon in. In dit voorbeeld moet u deze eigenschappen om uit te voeren van uw werkstroom elke week instelt. 

   ![Set-interval en frequentie](./media/connectors-native-recurrence/recurrence-trigger-details.png)

4. Voor meer opties voor planning, kiest u **geavanceerde opties weergeven**. 

   ![Meer opties](./media/connectors-native-recurrence/recurrence-trigger-more-options.png)

5. U kunt nu deze opties instellen: 

   * Een begindatum en -tijd voor het starten van de trigger instellen. 
   Als u een datum en tijd opgeeft, kunt u ook een tijdzone toepassen. 

   * Als u 'Day' of 'Week' voor de frequentie selecteert, kunt u specifieke tijden voor het terugkeerpatroon. 

   * Als u 'Week', kunt u specifieke dagen van de week te selecteren.
   
   ![Geavanceerde opties voor de planning](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   Stel bijvoorbeeld dat het vandaag is maandag 4 September 2017. 
   De volgende trigger met terugkeerpatroon niet geactiveerd *alle eerder* dan de begindatum en de tijd die maandag 18 September 2017 is op 8:00 AM PST. 
   Het terugkeerschema wordt echter ingesteld voor 10:30 uur, 12:30 PM en 14:30 uur op maandag alleen. De eerste keer dat de trigger wordt geactiveerd en een exemplaar van logic app workflow maakt is om 10:30 uur. 
   Zie voor meer informatie over hoe start werk tijden, deze [start time-voorbeelden](#start-time).
   Toekomstige planningen gebeuren op 12:30 PM en 14:30 uur op dezelfde dag. 
   Elke herhaling wordt gemaakt van hun eigen werkstroomexemplaar. Hierna wordt de hele planning herhaald in het volgende maandag. 
   [*Wat zijn enkele andere voorbeeld-exemplaren?*](#example-recurrences)

   ![Voorbeeld van de geavanceerde planning](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > De trigger wordt een voorbeeld voor het terugkeerpatroon van de opgegeven alleen als u 'Day' of 'Week' als de frequentie selecteert.
   
6. Nu het bouwen van uw resterende werkstroom met acties of flow besturingsinstructies. Zie voor meer acties die u kunt toevoegen, [Connectors](../connectors/apis-list.md). 

## <a name="trigger-details"></a>Triggerdetails

U kunt deze eigenschappen voor de trigger terugkeerpatroon configureren.

| Name | Vereist | Naam van eigenschap | Type | Description | 
|----- | -------- | ------------- | ---- | ----------- | 
| **Frequentie** | Ja | frequency | Reeks | De tijdseenheid voor het terugkeerpatroon: **Tweede**, **minuut**, **uur**, **dag**, **Week**, of **maand** | 
| **Interval** | Ja | interval | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de werkstroom wordt uitgevoerd, is afhankelijk van de frequentie. <p>Het standaardinterval is 1. Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uur: 1-12.000-uur </br>-Minuut: 1-72,000 minuten </br>-Seconde: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval 6 is en de frequency 'Maand' is, is klikt u vervolgens het terugkeerpatroon elke 6 maanden. | 
| **Tijdzone** | Nee | timeZone | Reeks | Geldt alleen wanneer u een begintijd opgeeft omdat deze trigger niet accepteren [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Selecteer de tijdzone die u wilt toepassen. | 
| **Begintijd** | Nee | startTime | Reeks | Geef een begintijd die in deze indeling: <p>JJJJ-MM-ddTUU als u een tijdzone selecteren <p>-of- <p>JJJJ-MM-ddTHH als u niet een tijdzone selecteren <p>Bijvoorbeeld als u wilt dat 18 September 2017 om 14:00 uur, geeft ' 2017-09-18T14:00:00 ' en selecteer een tijdzone zoals Pacific Time. Of geef ' 2017-09-18T14:00:00Z ' zonder een tijdzone. <p>**Opmerking:** De begintijd moet volgen de [ISO 8601 datum tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Als u een tijdzone niet selecteert, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze "Z" verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). <p>Voor eenvoudige schema's van de begintijd is de eerste keer voorkomt, terwijl voor complexe schema's, de trigger alle eerder dan de begintijd wordt niet gestart. [*Wat zijn de manieren waarop ik de begindatum en -tijd kan gebruiken?*](#start-time) | 
| **Deze dagen** | Nee | weekDays | Tekenreeks of tekenreeksmatrix van | Als u 'Week', kunt u een of meer dagen als u wilt uitvoeren van de werkstroom selecteren: **Maandag**, **dinsdag**, **woensdag**, **donderdag**, **vrijdag**, **zaterdag**, en **Zondag** | 
| **Deze uren** | Nee | hours | Geheel getal of een matrix met gehele getallen | Als u 'Day' of 'Week' selecteert, kunt u een of meer gehele getallen tussen 0 en 23 als de uren van de dag waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, als u "10", "12" en "14" opgeeft, krijgt u 10 uur, 12 PM en 2 uur als de merken uur. | 
| **Deze minuten** | Nee | minutes | Geheel getal of een matrix met gehele getallen | Als u 'Day' of 'Week' selecteert, kunt u een of meer gehele getallen tussen 0 en 59 als de minuten van het uur waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, u kunt "30" opgeven als de minuut is ingeschakeld en met behulp van het vorige voorbeeld uur van de dag, krijgt u 10:30 uur, 12:30 PM en 14:30 uur. | 
||||| 

## <a name="json-example"></a>Voorbeeld van JSON

Hier volgt een voorbeeld [definitie van de trigger terugkeerpatroon](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger):

``` json
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
```

## <a name="faq"></a>Veelgestelde vragen

<a name="run-once"></a>

**VRAAG:** Wat gebeurt er als ik wil een logische app alleen onmiddellijk en één keer uitvoeren? </br>
**A:** Als u wilt uw logische app direct te activeren en één keer zonder periodiek wordt uitgevoerd, kunt u de **Scheduler: Eenmaal uitvoeren van taken** sjabloon. Nadat u een nieuwe logische app hebt gemaakt, maar voor het openen van de ontwerpfunctie voor Logic Apps, onder de **sjablonen** sectie van de **categorie** in de lijst met **planning**, en selecteer vervolgens de sjabloon:

![Selecteer ' Scheduler: Eenmaal taken uitvoeren'-sjabloon](./media/connectors-native-recurrence/choose-run-once-template.png)

Of, als u een sjabloon voor lege logische app, start u uw logische app met de **wanneer een HTTP-aanvraag wordt ontvangen - aanvraag** trigger. Begintijd van de trigger als een parameter doorgeven. Voor de volgende stap toevoegen de **vertragen tot - plannen** actie, en geef de tijd voor wanneer de volgende actie wordt uitgevoerd.

<a name="example-recurrences"></a>

**VRAAG:** Wat zijn andere herhalingsplanningen voorbeeld? </br>
**A:** Hier vindt u meer voorbeelden:

| Terugkeerpatroon | Interval | Frequentie | Begintijd | Deze dagen | Deze uren | Deze minuten | Opmerking |
| ---------- | -------- | --------- | ---------- | ------------- | -------------- | ---------------- | ---- |
| Elke 15 minuten (geen begindatum en tijdstip) uitgevoerd | 15 | Minuut | {geen} | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt onmiddellijk gestart en toekomstige herhalingen op basis van de laatste uitvoeringstijd wordt berekend. | 
| Elke 15 minuten uitgevoerd (met de begindatum en -tijd) | 15 | Minuut | *startDate*T*startTime*Z | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* dan de opgegeven datum en tijd, berekent u toekomstige herhalingen op basis van de laatste uitvoeringstijd. | 
| Elk uur wordt uitgevoerd op het hele uur (met de begindatum en -tijd) | 1 | Uur | *startDate*Thh:00:00Z | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* start dan de opgegeven datum en tijd. Toekomstige herhalingen wordt elk uur uitgevoerd op de "00" minuut is ingeschakeld. <p>Als de frequency 'Week' of 'Maand' is, wordt dit schema respectievelijk slechts één dag per week of één dag per maand uitgevoerd. | 
| Elk uur, dagelijks (geen begindatum en tijdstip) uitgevoerd | 1 | Uur | {geen} | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt onmiddellijk gestart en toekomstige herhalingen op basis van de laatste uitvoeringstijd wordt berekend. <p>Als de frequency 'Week' of 'Maand' is, wordt dit schema respectievelijk slechts één dag per week of één dag per maand uitgevoerd. | 
| Elk uur, dagelijks (met de begindatum en -tijd) uitgevoerd | 1 | Uur | *startDate*T*startTime*Z | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* dan de opgegeven datum en tijd, berekent u toekomstige herhalingen op basis van de laatste uitvoeringstijd. <p>Als de frequency 'Week' of 'Maand' is, wordt dit schema respectievelijk slechts één dag per week of één dag per maand uitgevoerd. | 
| Om de 15 minuten na het uur, elk uur uitgevoerd (met de begindatum en -tijd) | 1 | Uur | *startDate*T00:15:00Z | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* dan de opgegeven begindatum en -tijd, uitgevoerd om 00:15 uur, 1:15 uur, 2:15 uur, enzovoort. | 
| Elke 15 minuten na het uur, elk uur (geen begindatum en tijdstip) uitgevoerd | 1 | Dag | {geen} | {niet-beschikbaar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Dit schema wordt uitgevoerd om 00:15 uur, 1:15 uur, 2:15 uur, enzovoort. Ook is dit schema gelijkwaardig met een frequentie van 'Uur' en een begintijd met '15' minuten. | 
| Elke 15 minuten uitgevoerd op de 15 minuten is ingeschakeld (geen begindatum en tijdstip) | 1 | Dag | {geen} | {niet-beschikbaar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dit schema start niet totdat de volgende 15 minuten is ingeschakeld opgegeven. | 
| Uitgevoerd om 8:00 uur op elke dag (geen begindatum en tijdstip) | 1 | Dag | {geen} | {niet-beschikbaar} | 8 | {geen} | Dit schema wordt uitgevoerd om 8:00 uur op elke dag, op basis van de opgegeven planning. | 
| Uitgevoerd om 8:00 uur op elke dag (met de begindatum en -tijd) | 1 | Dag | *startDate*T08:00:00Z | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt elke dag 8:00 uur uitgevoerd op basis van de opgegeven begintijd uitgevoerd. | 
| Uitgevoerd om 8:30 uur voor elke dag (geen begindatum en tijdstip) | 1 | Dag | {geen} | {niet-beschikbaar} | 8 | 30 | Dit schema wordt uitgevoerd om 8:30 uur voor elke dag, op basis van de opgegeven planning. | 
| Uitgevoerd om 8:30 uur voor elke dag (met de begindatum en -tijd) | 1 | Dag | *startDate*T08:30:00Z | {niet-beschikbaar} | {geen} | {geen} | Dit schema wordt gestart op de opgegeven begindatum om 8:30 uur. | 
| Op 8:30:00 uur en 4:30 uur, dagelijks uitvoeren | 1 | Dag | {geen} | {niet-beschikbaar} | 8, 16 | 30 | | 
| Uitgevoerd om 8:30 uur, 8:45 AM, 4:30 PM, en elke dag 4:45 uur | 1 | Dag | {geen} | {niet-beschikbaar} | 8, 16 | 30, 45 | | 
| Voer elke zaterdag om 17: 00 (geen begindatum en tijdstip) | 1 | Wekelijks | {geen} | "Zaterdag" | 17 | 00 | Dit schema wordt elke zaterdag om 17:00 uur uitgevoerd. | 
| Elke zaterdag om 17: 00 uur (met de begindatum en -tijd) wordt uitgevoerd | 1 | Wekelijks | *startDate*T17:00:00Z | "Zaterdag" | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* start dan de opgegeven datum en tijd, in dit geval 9 September 2017 om 17:00 uur. Toekomstige herhalingen elke zaterdag om 17:00 uur uitgevoerd. | 
| Elke dinsdag, donderdag om 17: 00 uur wordt uitgevoerd | 1 | Wekelijks | {geen} | "Dinsdag", "Donderdag" | 17 | {geen} | Dit schema wordt elke dinsdag en donderdag om 17:00 uur uitgevoerd. | 
| Elk uur tijdens werkuren uitgevoerd | 1 | Wekelijks | {geen} | Selecteer alle dagen behalve zaterdag en zondag. | Selecteer de uren van de dag die u wilt. | Selecteer een minuten van het uur dat u wilt. | Bijvoorbeeld, als uw werkuren 8:00 uur tot 5:00 uur zijn, selecteert u "8, 9, 10, 11, 12, 13, 14, 15, 16 en 17" Als de uren van de dag. <p>Als uw werkuren 8:30:00 uur tot 5:30 uur, selecteert u de vorige uur van de dag plus "30" Als het aantal minuten van het uur. | 
| Eenmaal elke dag worden uitgevoerd tijdens het weekend | 1 | Wekelijks | {geen} | "Zaterdag", "Zondag" | Selecteer de uren van de dag die u wilt. | Selecteer een minuten van het uur waar nodig. | Dit schema wordt elke zaterdag en zondag uitgevoerd volgens het opgegeven schema. | 
| Elke 15 minuten uitgevoerd op maandag alleen tweewekelijks | 2 | Wekelijks | {geen} | "Maandag" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dit schema wordt uitgevoerd elke andere maandag tot en met elke 15 minuten is ingeschakeld. | 
| Elk uur gedurende één dag per maand uitgevoerd | 1 | Maand | {Zie Opmerking} | {niet-beschikbaar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {Zie Opmerking} | Als u geen een datum en tijd opgeeft, geeft dit schema maakt gebruik van de datum en tijd. Geef het aantal minuten van het uur, een begintijd voor het beheren van de minuten voor het terugkeerschema of gebruik van de aanmaaktijd. Bijvoorbeeld, als de begin- of Aanmaaktijd 8:25 uur is, dit schema wordt uitgevoerd om 8:25 uur, 9:25 uur, 10:25 uur, enzovoort. | 
||||||||| 

<a name="start-time"></a>

**VRAAG:** Wat zijn de manieren waarop ik de begindatum en -tijd kan gebruiken? </br>
**A:** Hier zijn enkele patronen die laten zien hoe u terugkeerpatroon met de datum en tijd kunt beheren en hoe deze herhaling in de Logic Apps-engine wordt uitgevoerd:

| Begintijd | Recurrence zonder schedule | Recurrence met schedule | 
| ---------- | --------------------------- | ------------------------ | 
| {geen} | De eerste werkbelasting onmiddellijk uitgevoerd. <p>Toekomstige workloads op basis van de laatste uitvoeringstijd worden uitgevoerd. | De eerste werkbelasting onmiddellijk uitgevoerd. <p>Toekomstige workloads op basis van de opgegeven planning worden uitgevoerd. | 
| Begintijd in het verleden | Berekent de uitvoeringstijden op basis van de opgegeven begintijd uitgevoerd en verwijderingen afgelopen uitvoeren. De eerste werkbelasting op de volgende toekomst uitvoeringstijd wordt uitgevoerd. <p>Wordt uitgevoerd toekomstige werkbelastingen op basis van berekeningen van de laatste keer is uitgevoerd. <p>Zie het voorbeeld onder deze tabel voor meer informatie. | De eerste werkbelasting wordt uitgevoerd *nooit* dan de begintijd, op basis van de planning die wordt berekend op basis van de begintijd. <p>Toekomstige workloads op basis van de opgegeven planning worden uitgevoerd. <p>**Opmerking:** Als u een terugkeerpatroon met een schema opgeven, maar geen uren of minuten voor de planning opgeven, zijn klikt u vervolgens toekomstige uitvoeringstijden berekend met behulp van de uren of minuten, respectievelijk van de eerste uitvoeringstijd. | 
| Begintijd op dit moment of in de toekomst | De eerste werkbelasting op de opgegeven begintijd uitgevoerd. <p>Wordt uitgevoerd toekomstige werkbelastingen op basis van berekeningen van de laatste keer is uitgevoerd. | De eerste werkbelasting wordt uitgevoerd *nooit* dan de begintijd, op basis van de planning die wordt berekend op basis van de begintijd. <p>Toekomstige workloads op basis van de opgegeven planning worden uitgevoerd. <p>**Opmerking:** Als u een terugkeerpatroon met een schema opgeven, maar geen uren of minuten voor de planning opgeven, zijn klikt u vervolgens toekomstige uitvoeringstijden berekend met behulp van de uren of minuten, respectievelijk van de eerste uitvoeringstijd. | 
||||

**Voorbeeld voor een eerdere begintijd met terugkeerpatroon, maar geen Schedule is opgegeven** 

| Begintijd | Huidige tijd | Terugkeerpatroon | Planning |
| ---------- | ------------ | ---------- | -------- | 
| 2017-09-**07**T14:00:00Z | 2017-09-**08**T13:00:00Z | 2 dagen | {geen} | 
||||| 

In dit scenario, de Logic Apps engine berekent op basis van de begintijd keer uitvoeren worden verwijderd na uitvoeren, en maakt gebruik van de volgende toekomst begintijd voor de eerste keer uitvoert. Na het die eerst wordt uitgevoerd, worden toekomstige uitvoeringen zijn gebaseerd op de planning die wordt berekend op basis van de begintijd. Hier ziet u hoe dit terugkeerpatroon er uitziet:

| Begintijd | Eerste uitvoeringstijd | Toekomstige keer uitvoeren | 
| ---------- | ------------ | ---------- | 
| 2017-09 -**07** om 14:00 uur | 2017-09 -**09** om 14:00 uur | 2017-09 -**11** om 14:00 uur </br>2017-09 -**13** om 14:00 uur </br>2017-09 -**15** om 14:00 uur </br>enzovoort...
||||| 

Dus voor dit scenario, ongeacht hoe ver in het verleden opgeven van het begin tijd, bijvoorbeeld 2017-09 -**05** om 2:00 of 2017-09 -**01** om 14:00 uur, de eerste uitvoeringstijd is hetzelfde.

## <a name="next-steps"></a>Volgende stappen

* [Werkstroomacties en -triggers](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger)
* [Connectors](../connectors/apis-list.md)
