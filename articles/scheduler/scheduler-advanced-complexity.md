---
title: Bouw geavanceerde taakplanningen en herhalingen - Azure Scheduler
description: Informatie over het maken van geavanceerde schema's en herhalingen voor taken in Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.suite: infrastructure-services
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: f5a8b929cf5af6e4e43c6003e6b622d04a50b93e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980937"
---
# <a name="build-advanced-schedules-and-recurrences-for-jobs-in-azure-scheduler"></a>Bouw geavanceerde schema's en herhalingen voor taken in Azure Scheduler

> [!IMPORTANT]
> [Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, is buiten gebruik gesteld. Voor het plannen van taken, [Azure Logic Apps in plaats daarvan probeert](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Binnen een [Azure Scheduler](../scheduler/scheduler-intro.md) taak, de planning is het kern-waarmee wordt bepaald hoe en wanneer de taak wordt uitgevoerd door de Scheduler-service. U kunt meerdere eenmalige en terugkerende schema's voor een taak instellen met Scheduler. Eenmalige planning slechts één keer worden uitgevoerd op een bepaald tijdstip en zijn in feite terugkerende schema's die slechts één keer uitgevoerd. Terugkerende schema's worden uitgevoerd op een opgegeven frequentie. Met deze flexibiliteit, kunt u Taakplanner gebruiken voor verschillende bedrijfsscenario's, bijvoorbeeld:

* **Opschonen van gegevens regelmatig**: Maak een dagelijkse taak die wordt verwijderd van alle tweets die ouder is dan drie maanden.

* **Gegevens archiveren**: maken van een maandelijkse taak dat pushes Geschiedenis om een back-upservice te factureren.

* **Externe gegevens opvraagt**: maken van een taak die wordt uitgevoerd om de 15 minuten en haalt een nieuwe weerrapport van NOAA.

* **Afbeeldingen verwerken**: maken van een weekdag-taak die wordt uitgevoerd tijdens de daluren en maakt gebruik van cloudcomputing voor het comprimeren van afbeeldingen geüpload gedurende de dag.

Dit artikel wordt beschreven voorbeeld van de taken die u maken kunt met behulp van Scheduler en en de [Azure Scheduler REST API](https://docs.microsoft.com/rest/api/schedule), en bevat de definitie van de JavaScript Object Notation (JSON) voor elke planning. 

## <a name="supported-scenarios"></a>Ondersteunde scenario's

Deze voorbeelden van het bereik van scenario's die ondersteuning biedt voor Azure Scheduler en over het maken van schema's voor verschillende patronen in klantgedrag, bijvoorbeeld:

* Eenmaal uitgevoerd op een specifieke datum en tijd.
* Uitvoeren en een bepaald aantal keren herhaald.
* Direct uitvoeren en herhaald.
* Uitvoeren en herhalen elke *n* minuten, uren, dagen, weken of maanden, vanaf een bepaald tijdstip.
* Voer en herhaald wekelijks of maandelijks, maar alleen op bepaalde dagen van de week of op bepaalde dagen van de maand.
* Voer en herhaald meerdere keren voor een bepaalde periode. Bijvoorbeeld, elke maand op de laatste vrijdag en maandag, of elke dag om 5:15 uur en 17:15 uur.

Dit artikel beschrijft later deze scenario's in meer detail.

<a name="create-scedule"></a>

## <a name="create-schedule-with-rest-api"></a>Schema maken met REST-API

Het maken van een eenvoudige planning met de [Azure Scheduler REST API](https://docs.microsoft.com/rest/api/schedule), als volgt te werk:

1. Uw Azure-abonnement met een resourceprovider registreren met behulp van de [bewerking - Resource Manager REST-API registreren](https://docs.microsoft.com/rest/api/resources/providers#Providers_Register). De naam van de provider voor de Azure Scheduler-service is **Microsoft.Scheduler**. 

1. Een jobverzameling maken met behulp van de [bewerking voor maken of bijwerken voor taakverzamelingen](https://docs.microsoft.com/rest/api/scheduler/jobcollections#JobCollections_CreateOrUpdate) in de Scheduler REST API. 

1. Een taak maken met behulp van de [bewerking voor maken of bijwerken voor taken](https://docs.microsoft.com/rest/api/scheduler/jobs/createorupdate). 

## <a name="job-schema-elements"></a>Taak schema-elementen

Deze tabel bevat een overzicht op hoog niveau voor de belangrijkste JSON-elementen die u gebruiken kunt bij het instellen van het terugkeerpatroon en schema's voor taken. 

| Element | Vereist | Beschrijving | 
|---------|----------|-------------|
| **startTime** | Nee | Een datum/tijd-tekenreeks-waarde in [ISO 8601-notatie](http://en.wikipedia.org/wiki/ISO_8601) die aangeeft wanneer de taak eerst wordt gestart in een eenvoudige planning. <p>In complexe planningen begint de taak niet eerder dan **startTime**. | 
| **recurrence** | Nee | Het terugkeerpatroon van de regels voor wanneer de taak wordt uitgevoerd. De **terugkeerpatroon** object ondersteunt deze elementen: **frequentie**, **interval**, **planning**, **aantal**, en **endTime**. <p>Als u de **terugkeerpatroon** -element, moet u ook gebruiken de **frequentie** -element, terwijl andere **terugkeerpatroon** elementen zijn optioneel. |
| **frequency** | Ja, wanneer u **terugkeerpatroon** | De tijdseenheid tussen exemplaren en ondersteuning biedt voor deze waarden: 'Minuut', 'Uur', 'Dag', 'Week', 'Month' en 'Year' | 
| **interval** | Nee | Een positief geheel getal dat het aantal tijdseenheden tussen exemplaren bepaalt op basis van **frequentie**. <p>Bijvoorbeeld, als **interval** is 10 en **frequentie** ' week ', de taak elke 10 weken uitgevoerd. <p>Hier vindt u het maximum aantal intervallen voor elke frequentie: <p>-18 maanden <br>-78 weken <br>-548 dagen <br>-Voor uren en minuten, het bereik is 1 < = <*interval*>< = 1000. | 
| **schedule** | Nee | Wijzigingen definieert voor het terugkeerpatroon op basis van de opgegeven minuut-merken uur-markeringen, dagen van de week en dagen van de maand | 
| **Aantal** | Nee | Een positief geheel getal waarmee het aantal keren dat de taak wordt uitgevoerd voordat u klaar bent. <p>Bijvoorbeeld, wanneer een dagelijkse taak heeft **aantal** ingesteld op 7 en de begindatum is maandag, de taak is uitgevoerd op zondag. Als de begindatum al is gepasseerd, wordt de eerste keer uitvoert berekend vanaf het moment dat het maken. <p>Zonder **endTime** of **aantal**, de taak oneindig wordt uitgevoerd. U kunt de beide gebruiken **aantal** en **endTime** in dezelfde taak, maar de regel die klaar is met het eerst wordt herkend. | 
| **endTime** | Nee | De waarde voor een datum of datum/tijd-tekenreeks in [ISO 8601-notatie](http://en.wikipedia.org/wiki/ISO_8601) die aangeeft wanneer de taak stopt uitgevoerd. U kunt een waarde instellen voor **endTime** die zich in het verleden. <p>Zonder **endTime** of **aantal**, de taak oneindig wordt uitgevoerd. U kunt de beide gebruiken **aantal** en **endTime** in dezelfde taak, maar de regel die klaar is met het eerst wordt herkend. |
|||| 

Deze JSON-schema worden beschreven, bijvoorbeeld voor een eenvoudige planning en het terugkeerpatroon voor een taak: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z", 
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]                      
      },
      "count": 10,       
      "endTime": "2012-11-04"
   },
},
``` 

*Datums en datum-/ tijdwaarden*

* Datums in de Scheduler-taken alleen de datum en volg de [ISO 8601-specificatie](http://en.wikipedia.org/wiki/ISO_8601).

* En tijden in Scheduler-taken omvatten zowel datum en tijd, volgt u de [ISO 8601-specificatie](http://en.wikipedia.org/wiki/ISO_8601), en wordt ervan uitgegaan dat UTC wanneer er geen UTC-offset is opgegeven. 

Zie voor meer informatie, [concepten, terminologie en entiteiten](../scheduler/scheduler-concepts-terms.md).

<a name="start-time"></a>

## <a name="details-starttime"></a>Details: startTime

Deze tabel wordt beschreven hoe **startTime** bepaalt de manier waarop een taak wordt uitgevoerd:

| startTime | Er is geen terugkeerpatroon | Er is geen schema terugkeerpatroon | Recurrence met schedule |
|-----------|---------------|-------------------------|--------------------------|
| **Er is geen begintijd** | Eenmaal direct uitvoeren. | Eenmaal direct uitvoeren. Voer de volgende uitvoeringen berekend op basis van de laatste uitvoertijd. | Eenmaal direct uitvoeren. Volgende uitvoeringen worden op basis van een terugkerend schema uitgevoerd. | 
| **Begintijd in het verleden** | Eenmaal direct uitvoeren. | Berekenen van de eerste toekomstige tijd na de begintijd uitgevoerd en op dat moment worden uitgevoerd. <p>Voer de volgende uitvoeringen berekend op basis van de laatste uitvoertijd. <p>Zie het voorbeeld na deze tabel. | Taak starten *nooit* de opgegeven begintijd uitgevoerd. De eerste uitvoering is gebaseerd op de planning die wordt berekend op basis van de starttijd. <p>Volgende uitvoeringen worden op basis van een terugkerend schema uitgevoerd. | 
| **Begintijd in de toekomst of de huidige tijd** | Voer één keer op de opgegeven begintijd uitgevoerd. | Voer één keer op de opgegeven begintijd uitgevoerd. <p>Voer de volgende uitvoeringen berekend op basis van de laatste uitvoertijd. | Taak starten *nooit* de opgegeven begintijd uitgevoerd. De eerste uitvoering is gebaseerd op de planning die wordt berekend op basis van de starttijd. <p>Volgende uitvoeringen worden op basis van een terugkerend schema uitgevoerd. |
||||| 

Stel dat u in dit voorbeeld met deze voorwaarden: een begintijd in het verleden ligt een recurrence, maar geen Schedule is opgegeven.

```json
"properties": {
   "startTime": "2015-04-07T14:00Z", 
   "recurrence": {
      "frequency": "Day",
      "interval": 2
   }
}
```

* De huidige datum en tijd ' 2015-04-08 13:00 ".

* De begindatum en-tijd ' 2015-04-07 14:00 ", die zich voor de huidige datum en tijd.

* Het terugkeerpatroon is elke twee dagen.

1. In deze omstandigheden vindt plaats de eerste uitvoering op 2015-04-09 om 14:00 uur. 

   Scheduler wordt berekend voor de uitvoering van-exemplaren op basis van de begintijd, verwijdert alle exemplaren in het verleden en gebruikt de eerstvolgende uitvoering in de toekomst. 
   In dit geval **startTime** is in 2015-04-07 om 14:00 uur, waardoor de volgende uitvoering twee dagen vanaf dat moment 2015-04-09 om 14:00 uur is.

   De eerste uitvoering is hetzelfde of **startTime** is 2015-04-05 14:00 of 2015-04-01 14:00. Na de eerste uitvoering worden volgende uitvoeringen berekend op basis van de planning. 
   
1. De uitvoeringen Volg vervolgens in de aangegeven volgorde: 
   
   1. 2015-04-11 om 14:00 uur
   1. 2015-04-13 om 14:00 uur 
   1. 2015-04-15 om 14:00 uur
   1. enzovoort...

1. Ten slotte, als een taak heeft een schema, maar er is geen opgegeven uren en minuten, deze worden standaard de uren en minuten in de eerste uitvoering, respectievelijk.

<a name="schedule"></a>

## <a name="details-schedule"></a>Details: planning

U kunt gebruiken **planning** naar *limiet* het nummer van taakuitvoeringen. Bijvoorbeeld, als een taak met een **frequentie** van 'maand' heeft een planning die wordt uitgevoerd op dag 31, wordt de taak alleen uitgevoerd in maanden die een dertig op de eerste dag hebben.

U kunt ook **planning** naar *Vouw* het aantal taakuitvoeringen. Bijvoorbeeld, als een taak met een **frequentie** van 'maand' is een schema dat wordt uitgevoerd op de maanddagen 1 en 2, de taak wordt uitgevoerd op de eerste en tweede dag van de maand in plaats van slechts één keer per maand.

Als u meerdere schedule-elementen opgeeft, wordt de volgorde van de evaluatie is van de grootste naar kleinste: weeknummer, maand, weekdag, uur en minuut.

In de volgende tabel worden de schedule-elementen in detail beschreven:

| JSON-naam | Beschrijving | Geldige waarden |
|:--- |:--- |:--- |
| **minutes** |Minuten van het uur waarop de taak wordt uitgevoerd. |Een matrix met gehele getallen. |
| **hours** |Uur van de dag waarop de taak wordt uitgevoerd. |Een matrix met gehele getallen. |
| **weekDays** |Dagen van de week waarop de taak wordt uitgevoerd. Kan alleen worden opgegeven met een weekfrequentie. |Een matrix van elk van de volgende waarden (Maximale matrixgrootte is 7):<br />-"Maandag"<br />-"Dinsdag"<br />-"Woensdag"<br />-"Donderdag"<br />-"Vrijdag"<br />-"Zaterdag"<br />-"Zondag"<br /><br />Niet hoofdlettergevoelig. |
| **monthlyOccurrences** |Hiermee bepaalt u welke dagen van de maand de taak wordt uitgevoerd. Kan alleen met een maandfrequentie worden opgegeven. |Een matrix met **monthlyOccurrences** objecten:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **dag** is de dag van de week waarop de taak wordt uitgevoerd. Bijvoorbeeld, *{zondag}* is elke zondag van de maand. Vereist.<br /><br />**exemplaar** het exemplaar van de dag van de maand. Bijvoorbeeld, *{zondag, -1}* is de laatste zondag van de maand. Optioneel. |
| **monthDays** |Dag van de maand dat de taak wordt uitgevoerd. Kan alleen met een maandfrequentie worden opgegeven. |Een matrix van de volgende waarden:<br />- Alle waarden < = -1 en > =-31<br />- Alle waarden > = 1 en < =31|

## <a name="examples-recurrence-schedules"></a>Voorbeelden: Herhalingsplanningen

De volgende voorbeelden tonen verschillende schema's voor uitvoeringen. De voorbeelden ligt de nadruk op het object schedule en de bijbehorende subelementen.

Deze schema's wordt ervan uitgegaan dat **interval** is ingesteld op 1\. De voorbeelden ook wordt ervan uitgegaan dat de juiste **frequentie** waarden voor de waarden in **planning**. Bijvoorbeeld, u kunt niet gebruiken een **frequentie** 'dag' en hebben een **monthDays** wijziging in **planning**. Deze beperkingen eerder in dit artikel wordt beschreven.

| Voorbeeld | Beschrijving |
|:--- |:--- |
| `{"hours":[5]}` |Elke dag om 5 uur worden uitgevoerd.<br /><br />Scheduler komt overeen met elke waarde in 'uur' met elke waarde in 'minuten', één voor één, maakt u een lijst van de tijden waarop de taak wordt uitgevoerd. |
| `{"minutes":[15], "hours":[5]}` |Wordt elke dag om 5:15 uur uitgevoerd. |
| `{"minutes":[15], "hours":[5,17]}` |Wordt elke dag om 05:15 en 17:15 uur uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17]}` |Wordt elke dag om 05:15, 5:45, 17:15 en 17:45 uur uitgevoerd. |
| `{"minutes":[0,15,30,45]}` |Wordt elke 15 minuten uitgevoerd. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Wordt elk uur uitgevoerd.<br /><br />Deze taak wordt elk uur uitgevoerd. De minuut wordt bepaald door de waarde voor **startTime**, als deze is opgegeven. Als er geen **startTime** waarde is opgegeven, de minuut wordt bepaald door de aanmaaktijd. Bijvoorbeeld, als de starttijd of Aanmaaktijd (afhankelijk van wat van toepassing) 12:25 uur is, de taak wordt uitgevoerd om 00:25, 01:25, 02:25,..., 23:25.<br /><br />Het schema is gelijkwaardig met een taak met een **frequentie** 'uur', een **interval** van 1 en geen **planning** waarde. Het verschil is dat u dit schema met andere gebruiken kunt **frequentie** en **interval** waarden om te maken van andere taken. Bijvoorbeeld, als **frequentie** 'maand', de planning wordt uitgevoerd is slechts eenmaal per maand in plaats van elke dag (als **frequentie** ' dag '). |
| `{minutes:[0]}` |Wordt elk uur op het hele uur uitgevoerd.<br /><br />Deze taak ook elk uur uitgevoerd, maar op het hele uur (12 AM, 1 uur, 2 uur, enzovoort). Dit is gelijk aan een taak met een **frequentie** 'uur', een **startTime** nul minuten, en geen waarde **planning**, als de frequentie 'dag'. Echter, als de **frequentie** is 'week' of 'maand', het schema respectievelijk slechts één dag per week of één dag per maand uitgevoerd. |
| `{"minutes":[15]}` |Om de 15 minuten na het uur elk uur uitgevoerd.<br /><br />Elk uur uitgevoerd, beginnen om 00:15 uur, 1:15 uur, 2:15 uur, enzovoort. Deze eindigt om 23:15 uur. |
| `{"hours":[17], "weekDays":["saturday"]}` |Elke week om 17: 00 uur op zaterdag uitgevoerd. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Elke week om 17: 00 uur op maandag, woensdag en vrijdag uitgevoerd. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Wordt elke week op maandag, woensdag en vrijdag om 17:15 en 17:45 uur uitgevoerd. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Elke week om 5 uur en 17: 00 uur op maandag, woensdag en vrijdag uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Uitgevoerd om 05:15:00, 5:45 uur, 17:15 en 17:45 uur op maandag, woensdag en vrijdag elke week. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Wordt op weekdagen elke 15 minuten uitgevoerd. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Elke 15 minuten uitgevoerd op weekdagen tussen 9: 00 en 16:45 uur. |
| `{"weekDays":["sunday"]}` |Wordt op zondag om begintijd uitgevoerd. |
| `{"weekDays":["tuesday", "thursday"]}` |Uitgevoerd op dinsdag en donderdag om begintijd. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Uitgevoerd om 06.00 op de dag 28 van elke maand (ervan uitgaande dat het een **frequentie** van 'maand'). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Uitgevoerd om 06.00 op de laatste dag van de maand.<br /><br />Als u een taak uitvoeren op de laatste dag van een maand wilt, gebruik dan -1 in plaats van dag 28, 29, 30 of 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Voer op de eerste en laatste dag van elke maand om 6 uur. |
| `{monthDays":[1,-1]}` |Voer op de eerste en laatste dag van elke maand op de begintijd. |
| `{monthDays":[1,14]}` |Voer op de eerste en veertiende dag van elke maand op de begintijd. |
| `{monthDays":[2]}` |Voer op de tweede dag van de maand op begintijd. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Voer op de eerste vrijdag van elke maand om 05: 00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Voer op de eerste vrijdag van elke maand op de begintijd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Voer op de derde vrijdag vanaf het einde van de maand, maandelijks, aan het begin. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Wordt op de eerste en laatste vrijdagdag van elke maand om 5:15 uur uitgevoerd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Voer op de eerste en laatste vrijdag van elke maand op begintijd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Voer op de vijfde vrijdag van elke maand op de begintijd.<br /><br />Als er geen vijfde vrijdag in een maand, wordt de taak niet uitgevoerd. Kunt u overwegen 1 in plaats van 5 voor het exemplaar te gebruiken als u uitvoeren van de taak wilt op de laatste optreden vrijdag van de maand. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Wordt op de laatste vrijdag van de maand elke 15 minuten uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Wordt elke maand op de derde woensdag om 5:15, 5:45, 17:15 en 17:45 uur uitgevoerd. |

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)
* [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)