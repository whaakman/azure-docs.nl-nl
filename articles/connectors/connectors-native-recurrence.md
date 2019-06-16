---
title: Plan terugkerende taken met een trigger met terugkeerpatroon - Azure Logic Apps
description: Plannen en uitvoeren van terugkerende, geautomatiseerde taken en werkstromen met de trigger terugkeerpatroon in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: f5fc778ee4d8f91232bc732cc276f642f748b29d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66297562"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-recurrence-trigger-in-azure-logic-apps"></a>Maken, plannen en uitvoeren van terugkerende taken en werkstromen met de trigger terugkeerpatroon in Azure Logic Apps

Regelmatig uitvoeren van taken, processen of taken op basis van specifieke schema, kunt u uw werkstroom voor logische Apps beginnen met de ingebouwde **herhaling - schema** trigger. U kunt een datum en tijd, evenals een tijdzone voor het starten van de werkstroom en een terugkeerpatroon op die werkstroom herhalende instellen. Als terugkeerpatronen zijn voor een bepaalde reden hebt gemist, blijft deze trigger terugkerende op basis van de volgende geplande interval. Zie voor meer informatie over de ingebouwde schematriggers en acties, [planning en uitvoering terugkerende, geautomatiseerde, taken en werkstromen met Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Hier zijn enkele patronen die ondersteuning biedt voor deze trigger, samen met de meer geavanceerde terugkeerpatronen en complexe schema's:

* Direct uitvoeren en herhalen elke *n* aantal seconden, minuten, uren, dagen, weken of maanden.

* Start op een specifieke datum en tijd, en vervolgens uitvoeren en herhalen elke *n* aantal seconden, minuten, uren, dagen, weken of maanden.

* Voer en herhaald op een of meer keer per dag, bijvoorbeeld 8:00 uur en 17:00 uur.

* Uitvoeren en herhaalt u elke week, maar alleen voor specifieke dagen, zoals zaterdag en zondag.

* Uitvoeren en herhaalt u elke week, maar alleen voor specifieke dagen en tijden, zoals maandag tot en met vrijdag om 8:00 uur en 17:00 uur.

Zie voor verschillen tussen deze trigger en de trigger venster schuiven of voor meer informatie over het plannen van terugkerende werkstromen [planning en uitvoering terugkerende geautomatiseerde taken, processen en werkstromen met Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Als u wilt uw logische app activeren en slechts één keer uitgevoerd in de toekomst, Zie [uitvoeren taken slechts één keer](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Basiskennis over [logische apps](../logic-apps/logic-apps-overview.md). Als u geen ervaring met logische apps, krijgt u informatie [over het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-recurrence-trigger"></a>Trigger met Terugkeerpatroon toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Een lege, logische app maken.

1. Nadat de ontwerper van logische App wordt weergegeven, in het zoekvak, typ "terugkeerpatroon" als filter. Selecteer deze trigger als de eerste stap in uw werkstroom voor logische apps in de lijst met triggers: **Terugkeerpatroon**

   !["Terugkeerpatroon" trigger selecteren](./media/connectors-native-recurrence/add-recurrence-trigger.png)

1. Stel het interval en de frequentie voor het terugkeerpatroon in. In dit voorbeeld moet u deze eigenschappen om uit te voeren van uw werkstroom elke week instelt.

   ![Set-interval en frequentie](./media/connectors-native-recurrence/recurrence-trigger-details.png)

   | Eigenschap | Vereist | JSON-naam | Type | Description |
   |----------|----------|-----------|------|-------------|
   | **Interval** | Ja | interval | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de werkstroom wordt uitgevoerd, is afhankelijk van de frequentie. Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uur: 1-12.000-uur </br>-Minuut: 1-72,000 minuten </br>-Seconde: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval 6 is en de frequency 'Maand' is, is klikt u vervolgens het terugkeerpatroon elke 6 maanden. |
   | **Frequentie** | Ja | frequency | String | De tijdseenheid voor het terugkeerpatroon: **Tweede**, **minuut**, **uur**, **dag**, **Week**, of **maand** |
   ||||||

   Voor meer opties voor planning, opent u de **toevoegen van nieuwe parameter** lijst. 
   Alle opties die u selecteert worden weergegeven op de trigger na selectie.

   ![Geavanceerde opties voor de planning](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   | Eigenschap | Vereist | JSON-naam | Type | Description |
   |----------|----------|-----------|------|-------------|
   | **Tijdzone** | Nee | timeZone | String | Geldt alleen wanneer u een begintijd opgeeft omdat deze trigger niet accepteren [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Selecteer de tijdzone die u wilt toepassen. |
   | **Begintijd** | Nee | startTime | String | Geef een begindatum en tijd in deze indeling: <p>JJJJ-MM-ddTUU als u een tijdzone selecteren <p>-of- <p>JJJJ-MM-ddTHH als u niet een tijdzone selecteren <p>Bijvoorbeeld als u wilt dat 18 September 2017 om 14:00 uur, geeft ' 2017-09-18T14:00:00 ' en selecteer een tijdzone zoals Pacific (standaardtijd). Of geef ' 2017-09-18T14:00:00Z ' zonder een tijdzone. <p>**Opmerking:** De begintijd moet volgen de [ISO 8601 datum tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Als u een tijdzone niet selecteert, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze "Z" verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). <p>Voor eenvoudige schema's van de begintijd is de eerste keer voorkomt, terwijl voor complexe schema's, de trigger alle eerder dan de begintijd wordt niet gestart. [*Wat zijn de manieren waarop ik de begindatum en -tijd kan gebruiken?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   | **Deze dagen** | Nee | weekDays | Tekenreeks of tekenreeksmatrix van | Als u 'Week', kunt u een of meer dagen als u wilt uitvoeren van de werkstroom selecteren: **Maandag**, **dinsdag**, **woensdag**, **donderdag**, **vrijdag**, **zaterdag**, en **Zondag** |
   | **Deze uren** | Nee | hours | Geheel getal of een matrix met gehele getallen | Als u 'Day' of 'Week' selecteert, kunt u een of meer gehele getallen tussen 0 en 23 als de uren van de dag waarop u wilt de werkstroom uitvoert. <p><p>Bijvoorbeeld, als u "10", "12" en "14" opgeeft, krijgt u 10 uur, 12 PM en 2 uur voor de uren van de dag, maar de minuten van de dag worden berekend op basis van wanneer het terugkeerpatroon wordt gestart. Om in te stellen de minuten van de dag, geef de waarde voor de **deze minuten** eigenschap. |
   | **Deze minuten** | Nee | minutes | Geheel getal of een matrix met gehele getallen | Als u 'Day' of 'Week' selecteert, kunt u een of meer gehele getallen tussen 0 en 59 als de minuten van het uur waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, u kunt "30" opgeven als de minuut is ingeschakeld en met behulp van het vorige voorbeeld uur van de dag, krijgt u 10:30 uur, 12:30 PM en 14:30 uur. |
   |||||

   Stel bijvoorbeeld dat het vandaag is maandag 4 September 2017. De volgende trigger met terugkeerpatroon niet geactiveerd *alle eerder* dan de begindatum en de tijd die maandag 18 September 2017 is op 8:00 AM PST. Het terugkeerschema wordt echter ingesteld voor 10:30 uur, 12:30 PM en 14:30 uur op maandag alleen. De eerste keer dat de trigger wordt geactiveerd en een exemplaar van logic app workflow maakt is om 10:30 uur. Zie voor meer informatie over hoe start werk tijden, deze [start time-voorbeelden](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time).

   Toekomstige planningen gebeuren op 12:30 PM en 14:30 uur op dezelfde dag. Elke herhaling wordt gemaakt van hun eigen werkstroomexemplaar. Hierna wordt de hele planning herhaald in het volgende maandag. [*Wat zijn enkele andere voorbeeld-exemplaren?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#example-recurrences)

   ![Voorbeeld van de geavanceerde planning](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > De trigger wordt een voorbeeld voor het terugkeerpatroon van de opgegeven alleen als u 'Day' of 'Week' als de frequentie selecteert.

1. Nu uw resterende werkstroom met andere acties bouwen. Zie voor meer acties die u kunt toevoegen, [Connectors voor Azure Logic Apps](../connectors/apis-list.md).

## <a name="workflow-definition---recurrence"></a>Definitie van de werkstroom - terugkeerpatroon

U kunt weergeven in uw logische app onderliggende werkstroom-definitie, die gebruikmaakt van JSON de [definitie van de trigger terugkeerpatroon](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger) met de opties die u hebt gekozen. U kunt deze definitie bekijken op de werkbalk van de ontwerper **codeweergave**. Kies op de werkbalk van de ontwerper om terug te keren naar de ontwerpfunctie, **Designer**.

Dit voorbeeld laat zien hoe een definitie van de trigger terugkeerpatroon in een definitie van de onderliggende werkstroom uitzien:

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
         "startTime": "2017-09-07T14:00:00Z",
         "timeZone": "Pacific Standard Time"
      }
   }
}
```

## <a name="next-steps"></a>Volgende stappen

* [Werkstromen onderbreken met de acties die vertraging](../connectors/connectors-native-delay.md)
* [Connectors voor Logic Apps](../connectors/apis-list.md)
