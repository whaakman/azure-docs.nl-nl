---
title: Plan terugkerende taken met Verschuivend venster trigger - Azure Logic Apps
description: Plannen en uitvoeren van terugkerende, geautomatiseerde taken en werkstromen met de graduele venster-trigger in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 44944955019fcf81fb0d296592577e2b00a15928
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299500"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-sliding-window-trigger-in-azure-logic-apps"></a>Maken, plannen en uitvoeren van terugkerende taken en werkstromen met de graduele venster-trigger in Azure Logic Apps

Regelmatig uitvoeren van taken, processen of taken die gegevens in reeksen continue moeten verwerken, kun u uw werkstroom voor logische Apps met de **Verschuivend venster - schema** trigger. U kunt een datum en tijd, evenals een tijdzone voor het starten van de werkstroom en een terugkeerpatroon op die werkstroom herhalende instellen. Als terugkeerpatronen zijn om enige reden hebt gemist, verwerkt deze trigger die gemiste herhalingen. Bijvoorbeeld bij het synchroniseren van gegevens tussen uw database en de back-upopslag, de graduele venster trigger gebruiken zodat de gegevens zonder hiaten wordt gesynchroniseerd. Zie voor meer informatie over de ingebouwde schematriggers en acties, [planning en uitvoering terugkerende, geautomatiseerde, taken en werkstromen met Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Hier zijn enkele patronen die ondersteuning biedt voor deze trigger:

* Direct uitvoeren en herhalen elke *n* aantal seconden, minuten of uren.

* Start op een specifieke datum en tijd, en vervolgens uitvoeren en herhalen elke *n* aantal seconden, minuten of uren. Met deze trigger kunt u een begintijd opgeven in het verleden, waarop alle eerdere herhalingen worden uitgevoerd.

* Elke herhaling gedurende een bepaalde periode voordat u de wachttijd.

Zie voor verschillen tussen deze trigger en de trigger met terugkeerpatroon of voor meer informatie over het plannen van terugkerende werkstromen [planning en uitvoering terugkerende geautomatiseerde taken, processen en werkstromen met Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Als u wilt uw logische app activeren en slechts één keer uitgevoerd in de toekomst, Zie [uitvoeren taken slechts één keer](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Basiskennis over [logische apps](../logic-apps/logic-apps-overview.md). Als u geen ervaring met logische apps, krijgt u informatie [over het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-sliding-window-trigger"></a>Venster Verschuivend trigger toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Een lege, logische app maken.

1. Nadat Logic App Designer wordt weergegeven, in het zoekvak, typ 'sliding window' als filter. Selecteer deze trigger als de eerste stap in uw werkstroom voor logische apps in de lijst met triggers: **Sliding Window**

   !["In het venster Verschuivend" trigger selecteren](./media/connectors-native-sliding-window/add-sliding-window-trigger.png)

1. Stel het interval en de frequentie voor het terugkeerpatroon in. In dit voorbeeld moet u deze eigenschappen om uit te voeren van uw werkstroom elke week instelt.

   ![Set-interval en frequentie](./media/connectors-native-sliding-window/sliding-window-trigger-details.png)

   | Eigenschap | Vereist | JSON-naam | Type | Description |
   |----------|----------|-----------|------|-------------|
   | **Interval** | Ja | interval | Integer | Een positief geheel getal dat wordt beschreven hoe vaak de werkstroom wordt uitgevoerd, is afhankelijk van de frequentie. Hier volgen de minimale en maximale intervallen: <p>-Uur: 1-12.000-uur </br>-Minuut: 1-72,000 minuten </br>-Seconde: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval 6 is en de frequentie is 'Uur', is klikt u vervolgens het terugkeerpatroon om de 6 uur. |
   | **Frequentie** | Ja | frequency | String | De tijdseenheid voor het terugkeerpatroon: **Tweede**, **minuut**, of **uur** |
   ||||||

   ![Opties voor Geavanceerd terugkeerpatroon](./media/connectors-native-sliding-window/sliding-window-trigger-more-options-details.png)

   Voor meer opties voor terugkeerpatroon, opent u de **toevoegen van nieuwe parameter** lijst. 
   Alle opties die u selecteert worden weergegeven op de trigger na selectie.

   | Eigenschap | Vereist | JSON-naam | Type | Description |
   |----------|----------|-----------|------|-------------|
   | **Delay** | Nee | delay | String | De duur van de vertraging elke herhaling met behulp van de [ISO 8601 datum tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Durations) |
   | **Tijdzone** | Nee | timeZone | String | Geldt alleen wanneer u een begintijd opgeeft omdat deze trigger niet accepteren [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Selecteer de tijdzone die u wilt toepassen. |
   | **Begintijd** | Nee | startTime | String | Geef een begindatum en tijd in deze indeling: <p>JJJJ-MM-ddTUU als u een tijdzone selecteren <p>-of- <p>JJJJ-MM-ddTHH als u niet een tijdzone selecteren <p>Bijvoorbeeld als u wilt dat 18 September 2017 om 14:00 uur, geeft ' 2017-09-18T14:00:00 ' en selecteer een tijdzone zoals Pacific (standaardtijd). Of geef ' 2017-09-18T14:00:00Z ' zonder een tijdzone. <p>**Opmerking:** De begintijd moet volgen de [ISO 8601 datum tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Als u een tijdzone niet selecteert, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze "Z" verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). <p>Voor eenvoudige schema's van de begintijd is de eerste keer voorkomt, terwijl voor geavanceerde terugkeerpatronen alle eerder dan de begintijd wordt de trigger niet geactiveerd. [*Wat zijn de manieren waarop ik de begindatum en -tijd kan gebruiken?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   |||||

1. Nu uw resterende werkstroom met andere acties bouwen. Zie voor meer acties die u kunt toevoegen, [Connectors voor Azure Logic Apps](../connectors/apis-list.md).

## <a name="workflow-definition---sliding-window"></a>Definitie van de werkstroom - venster schuiven

U kunt de definitie van de trigger venster schuiven met de opties die u hebt gekozen weergeven in uw logische app onderliggende werkstroom-definitie, die gebruikmaakt van JSON. U kunt deze definitie bekijken op de werkbalk van de ontwerper **codeweergave**. Kies op de werkbalk van de ontwerper om terug te keren naar de ontwerpfunctie, **Designer**.

Dit voorbeeld laat zien hoe een definitie van de trigger Verschuivend venster ziet er in een definitie van de onderliggende werkstroom waar de vertraging voor elke terugkeerpatroon is vijf seconden voor een terugkeerpatroon per uur:

``` json
"triggers": {
   "Recurrence": {
      "type": "SlidingWindow",
      "Sliding_Window": {
         "inputs": {
            "delay": "PT5S"
         },
         "recurrence": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2019-05-13T14:00:00Z",
            "timeZone": "Pacific Standard Time"
         }
      }
   }
}
```

## <a name="next-steps"></a>Volgende stappen

* [Vertraging van de volgende actie in werkstromen](../connectors/connectors-native-delay.md)
* [Connectors voor Logic Apps](../connectors/apis-list.md)