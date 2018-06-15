---
title: Opbouwen van complexe schema's en geavanceerde terugkeerpatronen met Azure Scheduler
description: Informatie over het bouwen van complexe schema's en geavanceerde terugkeerpatronen met Azure Scheduler.
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 4293442e13fc4bae871b1f32a3ed4231d9f32632
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29692331"
---
# <a name="build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Opbouwen van complexe schema's en geavanceerde terugkeerpatronen met Azure Scheduler

De kern van een Azure Scheduler-taak is het schema. De planning bepaalt wanneer en hoe Scheduler de taak uitvoert. 

Scheduler kunt u meerdere eenmalige en terugkerende schema's instellen voor een taak. Eenmalige planningen gestart eenmaal op een opgegeven periode. Eenmalige schema's zijn effectief terugkerende schema's die slechts één keer worden uitgevoerd. Terugkerende planningen geactiveerd op een vooraf vastgestelde frequentie.

U kunt met deze flexibiliteit Scheduler gebruiken voor een groot aantal bedrijfsscenario's:

* **Opruimen van periodieke gegevens**. Elke dag worden bijvoorbeeld alle tweets die ouder dan drie maanden zijn verwijderen.
* **Archiveren**. Bijvoorbeeld: elke maand, push factuur geschiedenis met een Backup-service.
* **Aanvragen voor externe gegevens**. Bijvoorbeeld, halen om de 15 minuten een nieuw rapport voor ski weer uit NOAA.
* **Verwerking van afbeeldingen**. Bijvoorbeeld elke weekdag, tijdens de daluren, gebruiken voor het comprimeren van afbeeldingen die zijn geüpload die dag voor cloudcomputing.

In dit artikel doorlopen we voorbeeld taken die u maken kunt met behulp van Scheduler. We bieden de JSON-gegevens die worden beschreven van het schema. Als u de [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx), kunt u deze dezelfde JSON naar [maken van een Scheduler-job](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Ondersteunde scenario's
De voorbeelden in dit artikel ziet de breedte van de scenario's die ondersteuning biedt voor Scheduler. De voorbeelden laten grote schaal het maken van schema's voor veel gedrag patronen, met inbegrip van:

* Eenmaal uitvoeren op een specifieke datum en tijd.
* Voer en een bepaald aantal keren herhaald.
* Direct uitgevoerd en herhaald.
* Voer en herhaald elke  *n*  minuten, uren, dagen, weken of maanden, beginnend bij een bepaalde tijd.
* Voer en herhaald met een wekelijkse of maandelijkse frequentie, maar alleen op bepaalde dagen van de week of op bepaalde dagen van de maand.
* Voer en meerdere keren in een periode wordt herhaald. Bijvoorbeeld, op de laatste vrijdag en afgelopen maandag van elke maand of op 5:15 uur en 17:15:00 uur elke dag.

## <a name="date-and-date-time"></a>Datum- en datum / tijd
Datum-verwijzingen in Taakplanner-taken volgen de [ISO 8601-specificatie](http://en.wikipedia.org/wiki/ISO_8601), en alleen de datum.

Voer de datum / tijd-verwijzingen in taken de [ISO 8601-specificatie](http://en.wikipedia.org/wiki/ISO_8601), en bevatten zowel datum en tijd. Een datum / tijd die een UTC-afwijking niet opgeeft, wordt aangenomen dat UTC.  

## <a name="use-json-and-the-rest-api-to-create-a-schedule"></a>JSON en de REST-API gebruiken om een planning te maken
Een eenvoudige planning te maken met behulp van de [Scheduler REST API](https://msdn.microsoft.com/library/mt629143), eerste [registreren van uw abonnement met een resourceprovider](https://msdn.microsoft.com/library/azure/dn790548.aspx). De providernaam van de voor de planner is **Microsoft.Scheduler**. Vervolgens [maken van een taakverzameling](https://msdn.microsoft.com/library/mt629159.aspx). Ten slotte [maken van een taak](https://msdn.microsoft.com/library/mt629145.aspx). 

Wanneer u een taak maakt, kunt u plannen en het terugkeerpatroon opgeven met behulp van de JSON, zoals in dit fragment:

    {
        "startTime": "2012-08-04T00:00Z", // Optional
         …
        "recurrence":                     // Optional
        {
            "frequency": "week",     // Can be "year", "month", "day", "week", "hour", or "minute"
            "interval": 1,                // How often to fire
            "schedule":                   // Optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // Optional (default to recur infinitely)
            "endTime": "2012-11-04",      // Optional (default to recur infinitely)
        },
        …
    }

## <a name="job-schema-basics"></a>Taak schema basisbeginselen
De volgende tabel bevat een overzicht van de belangrijkste onderdelen die u gebruikt voor het terugkeerpatroon en -planning instellen in een taak:

| JSON-naam | Beschrijving |
|:--- |:--- |
| **startTime** |Een datum / tijd-waarde. Voor basic-schema's **startTime** is het eerste exemplaar. Voor complexe schema's, begint de taak niet eerder dan **startTime**. |
| **recurrence** |Hiermee geeft u een terugkeerpatroon regels voor de taak en het terugkeerpatroon waarop de taak wordt uitgevoerd. Het object terugkeert ondersteunt de elementen **frequentie**, **interval**, **endTime**, **aantal**, en **planning**. Als **terugkeerpatroon** is gedefinieerd, **frequentie** is vereist. Andere **terugkeerpatroon** elementen zijn optioneel. |
| **frequency** |Een tekenreeks met de eenheid van de frequentie waarmee de taak terugkeert. Ondersteunde waarden zijn 'minuut', 'uur', 'day', 'week' en 'maand'. |
| **interval** |Een positief geheel getal. **interval** geeft het interval voor de **frequentie** waarde die bepaalt hoe vaak de taak wordt uitgevoerd. Bijvoorbeeld, als **interval** 3 en **frequentie** 'week', is de taak herhaald elke drie weken.<br /><br />Ondersteunt maximaal Scheduler **interval** van 18 voor maandelijkse frequentie, 78 voor wekelijkse frequentie en 548 voor dagelijkse frequentie. Voor uren en minuten frequentie, het ondersteunde bereik is 1 < = **interval** < = 1000. |
| **endTime** |Een tekenreeks waarmee de datum-tijd waarop de taak niet wordt uitgevoerd. U kunt een waarde instellen voor **endTime** die zich in het verleden. Als **endTime** en **aantal** niet zijn opgegeven, wordt de taak oneindig wordt uitgevoerd. U kunt geen beide **endTime** en **aantal** in dezelfde taak. |
| **count** |Een positief geheel getal (groter dan nul) die hoe vaak aangeeft de taak wordt uitgevoerd voordat deze voltooid.<br /><br />**aantal** geeft het aantal keren dat de taak wordt uitgevoerd voordat bepaald voltooid. Bijvoorbeeld: voor een taak die dagelijks wordt uitgevoerd met een **aantal** van 5 en een begindatum van maandag tot de taak is voltooid na uitvoering op vrijdag. Als de begindatum verstreken is, wordt de eerste uitvoering van de aanmaaktijd berekend.<br /><br />Als er geen **endTime** of **aantal** is opgegeven, wordt de taak oneindig wordt uitgevoerd. U kunt geen beide **endTime** en **aantal** in dezelfde taak. |
| **schedule** |Een taak met een opgegeven frequentie wijzigt het terugkeerpatroon op basis van een terugkerende planning. Een **planning** waarde bevat wijzigingen die op basis van de minuten, uren, weekdagen, dagen van de maand en weeknummer. |

## <a name="job-schema-defaults-limits-and-examples"></a>Taak schema standaardwaarden, limieten en voorbeelden
Later in dit artikel bespreken we elk van de volgende elementen in detail:

| JSON-naam | Waardetype | Vereist? | Standaardwaarde | Geldige waarden | Voorbeeld |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** |tekenreeks |Nee |Geen |ISO 8601 datums en tijden die- |`"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** |object |Nee |None |Het object terugkeerpatroon |`"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **frequency** |tekenreeks |Ja |None |'minuut', 'uur', 'day', 'week', 'maand' |`"frequency" : "hour"` |
| **interval** |nummer |Ja |None |1-1000 |`"interval":10` |
| **endTime** |tekenreeks |Nee |Geen |Datum / tijd-waarde waarmee een tijd in de toekomst |`"endTime" : "2013-02-09T09:30:00-08:00"` |
| **count** |nummer |Nee |None |>= 1 |`"count": 5` |
| **schedule** |object |Nee |None |Het schema-object |`"schedule" : { "minute" : [30], "hour" : [8,17] }` |

## <a name="deep-dive-starttime"></a>Gedetailleerde informatie over startTime
De volgende tabel beschrijft hoe **startTime** bepaalt de manier waarop een taak is uitgevoerd:

| startTime-waarde | Geen terugkeerpatroon | Terugkeerpatroon zonder schema | Recurrence met schedule |
|:--- |:--- |:--- |:--- |
| **Er is geen begintijd** |Eenmaal onmiddellijk uitvoeren. |Eenmaal onmiddellijk uitvoeren. Bij een volgende uitvoering berekend op basis van de laatste uitvoertijd worden uitgevoerd. |Eenmaal onmiddellijk uitvoeren.<br /><br />Bij een volgende uitvoering op basis van een terugkerend schema uitgevoerd. |
| **De begintijd in het verleden** |Eenmaal onmiddellijk uitvoeren. |De eerste toekomstige uitvoeringstijd berekenen na de begintijd liggen en op dat moment worden uitgevoerd.<br /><br />Bij een volgende uitvoering berekend op basis van de laatste uitvoertijd worden uitgevoerd. <br /><br />Zie het voorbeeld na deze tabel voor meer informatie. |Taak gestart *niet sooner dan* de opgegeven begintijd. De eerste uitvoering is gebaseerd op de planning die wordt berekend op basis van de starttijd.<br /><br />Bij een volgende uitvoering op basis van een terugkerend schema uitgevoerd. |
| **De begintijd in de toekomst, of de huidige tijd** |Eenmaal op het opgegeven begin-tijdstip worden uitgevoerd. |Eenmaal op het opgegeven begin-tijdstip worden uitgevoerd.<br /><br />Bij een volgende uitvoering berekend op basis van de laatste uitvoertijd worden uitgevoerd.|Taak gestart *niet sooner dan* de opgegeven begintijd. Het eerste exemplaar is gebaseerd op de planning, berekend op basis van de begintijd.<br /><br />Bij een volgende uitvoering op basis van een terugkerend schema uitgevoerd. |

Bekijk een voorbeeld van wat er gebeurt wanneer **startTime** in het verleden ligt, met het terugkeerpatroon, maar zonder schema.  Wordt ervan uitgegaan dat de huidige tijd 2015-04-08 13:00 **startTime** 2015-04-07 14:00 en **terugkeerpatroon** is elke twee dagen (gedefinieerd met **frequentie**: dag en **interval**: 2.) Houd er rekening mee dat **startTime** is in het verleden en vindt plaats voordat de huidige tijd.

In deze omstandigheden worden uitgevoerd tijdens de eerste uitvoering in 2015-04-09 op 14:00\. De scheduler-engine berekent uitvoeringen vanaf de startTime. Alle uitvoeringen in het verleden worden genegeerd. De engine gebruikt de eerstvolgende uitvoering die in de toekomst plaatsvindt. In dit geval **startTime** 2015-04-07 op 14:00 uur is, dus is het volgende exemplaar twee dagen vanaf dat moment 2015-04-09 op 14:00 uur is.

Houd er rekening mee dat de eerste uitvoering hetzelfde zijn zou of **startTime** 2015-04-05 is 14:00 of 2015-04-01 14:00 uur\. Na de eerste uitvoering worden volgende uitvoeringen berekend met behulp van het schema. Ze zullen worden op 2015-04-11 op 14:00 uur, vervolgens 2015-04-13 op 14:00 uur en vervolgens 2015-04-15 op 14:00 uur, enzovoort.

Ten slotte wanneer een taak heeft een planning als uren en minuten zijn niet ingesteld in de planning, de standaard waarden naar de uren en minuten van de eerste uitvoering, respectievelijk.

## <a name="deep-dive-schedule"></a>Diepgaand: planning
U kunt **planning** naar *limiet* het aantal uitvoeringen van de taak. Bijvoorbeeld, als een taak met een **frequentie** van "maand" heeft een schema dat alleen op dag 31 werkt, de taak alleen uitgevoerd in maanden die een dertig eerste dag.

U kunt ook **planning** naar *Vouw* het nummer van de taak uitvoeringen. Bijvoorbeeld, als een taak met een **frequentie** van "maand" heeft een planning die wordt uitgevoerd op dagen van de maand 1 en 2, wordt de taak wordt uitgevoerd op de eerste en tweede dag van de maand in plaats van slechts één keer per maand.

Als u meerdere schema-elementen opgeeft, wordt de volgorde van de evaluatie is vanaf de grootste waarde kleinste tot: weeknummer, maand, dag,, weekdag, uur en minuut.

In de volgende tabel worden de schedule-elementen in detail beschreven:

| JSON-naam | Beschrijving | Geldige waarden |
|:--- |:--- |:--- |
| **minutes** |Minuten van het uur waarop de taak wordt uitgevoerd. |Een matrix van gehele getallen. |
| **hours** |Uur van de dag waarop de taak wordt uitgevoerd. |Een matrix van gehele getallen. |
| **weekDays** |Dagen van de week de taak wordt uitgevoerd. Kan alleen met een wekelijkse frequentie worden opgegeven. |Een matrix van elk van de volgende waarden (Maximale matrixgrootte is 7):<br />-"Maandag"<br />-"Dinsdag"<br />-"Woensdag"<br />-"Donderdag"<br />-"Vrijdag"<br />-"Zaterdag"<br />-"Zondag"<br /><br />Niet hoofdlettergevoelig. |
| **monthlyOccurrences** |Hiermee wordt bepaald welke dagen van de maand van de taak is uitgevoerd. Kan alleen met een maandelijkse frequentie worden opgegeven. |Een matrix van **monthlyOccurrences** objecten:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **dag** is de dag van de week de taak wordt uitgevoerd. Bijvoorbeeld: *{zondag}* elke zondag van de maand. Vereist.<br /><br />**exemplaar** het exemplaar van de dag in de maand. Bijvoorbeeld: *{zondag, -1}* is de laatste zondag van de maand. Optioneel. |
| **monthDays** |Dag van de maand die de taak wordt uitgevoerd. Kan alleen met een maandelijkse frequentie worden opgegeven. |Een matrix van de volgende waarden:<br />-Een waarde < = -1 en > =-31<br />-Elke waarde > = 1 en < = 31|

## <a name="examples-recurrence-schedules"></a>Voorbeelden: Terugkerende schema 's
De volgende voorbeelden ziet diverse terugkerende schema's. De voorbeelden ligt de nadruk op het object schema en de subelementen.

Deze schema's wordt ervan uitgegaan dat **interval** is ingesteld op 1\. De voorbeelden ook wordt ervan uitgegaan dat de juiste **frequentie** waarden voor de waarden in **planning**. Bijvoorbeeld, u niet gebruiken een **frequentie** van 'day' en hebben een **maanddagen** wijziging in **planning**. Deze beperkingen eerder in dit artikel worden beschreven.

| Voorbeeld | Beschrijving |
|:--- |:--- |
| `{"hours":[5]}` |Voer op 05: 00 elke dag.<br /><br />Scheduler komt overeen met elke waarde in 'hours' met elke waarde in 'minutes', één met één voor het maken van een lijst van alle tijden waarop de taak wordt uitgevoerd. |
| `{"minutes":[15], "hours":[5]}` |Wordt elke dag om 5:15 uur uitgevoerd. |
| `{"minutes":[15], "hours":[5,17]}` |Wordt elke dag om 05:15 en 17:15 uur uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17]}` |Wordt elke dag om 05:15, 5:45, 17:15 en 17:45 uur uitgevoerd. |
| `{"minutes":[0,15,30,45]}` |Wordt elke 15 minuten uitgevoerd. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Wordt elk uur uitgevoerd.<br /><br />Deze taak elk uur wordt uitgevoerd. De minuut wordt bepaald door de waarde voor **startTime**, als deze is opgegeven. Als er geen **startTime** waarde die is opgegeven, de minuut wordt bepaald door de aanmaaktijd. Bijvoorbeeld, als de begintijd of Aanmaaktijd (afhankelijk van wat van toepassing) 12:25 uur, de taak wordt uitgevoerd op 00:25, 01:25 02:25,..., 23:25.<br /><br />De planning is gelijk aan een taak met een **frequentie** van 'uur' een **interval** van 1, maar er is geen **planning** waarde. Het verschil is dat u dit schema met andere gebruiken kunt **frequentie** en **interval** waarden voor het maken van andere taken. Bijvoorbeeld, als **frequentie** "maand", de planning wordt uitgevoerd is slechts één keer per maand in plaats van elke dag (als **frequentie** ' dag '). |
| `{minutes:[0]}` |Wordt elk uur op het hele uur uitgevoerd.<br /><br />Deze taak wordt ook uitgevoerd voor elk uur, maar ook op het uur (12 uur, 1 uur, 2 uur, enzovoort). Dit is gelijkwaardig aan een taak met een **frequentie** van 'uur' een **startTime** waarde van nul minuten, maar er is geen **planning**, als de frequentie 'dag'. Echter, als de **frequentie** is "week" of "maand", de planning wordt uitgevoerd slechts één dag een week of één dag per maand, respectievelijk. |
| `{"minutes":[15]}` |Op 15 minuten na het uur elk uur uitgevoerd.<br /><br />Elk uur, vanaf 00:15 uur, 01:15:00, 2:15 uur wordt uitgevoerd enzovoort. Deze wordt beëindigd om 11:15 uur. |
| `{"hours":[17], "weekDays":["saturday"]}` |Voer op 17: 00 uur op zaterdag elke week. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Voer op 17: 00 uur op maandag, woensdag en vrijdag elke week. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Wordt elke week op maandag, woensdag en vrijdag om 17:15 en 17:45 uur uitgevoerd. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Voer op 5 uur en 17: 00 uur op maandag, woensdag en vrijdag elke week. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Uitvoeren op 5:15 uur, 5:45 uur 17:15:00 uur en 17:45 uur op maandag, woensdag en vrijdag elke week. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Wordt op weekdagen elke 15 minuten uitgevoerd. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Om de 15 minuten worden uitgevoerd op weekdagen tussen 9: 00 uur en 4:45 uur. |
| `{"weekDays":["sunday"]}` |Voer op de zondag tijdens het starten. |
| `{"weekDays":["tuesday", "thursday"]}` |Uitvoeren op dinsdag en donderdag tijdens het starten. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Uitvoeren om 6.00 uur op de twintig achtste dag van elke maand (ervan uitgaande dat een **frequentie** van "maand"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Om 6.00 uur op de laatste dag van de maand wordt uitgevoerd.<br /><br />Als u een taak uitvoeren op de laatste dag van een maand wilt, gebruikt u in plaats van de dag 28, 29, 30 of 31 -1. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Om 6.00 uur op de eerste en laatste dag van elke maand worden uitgevoerd. |
| `{monthDays":[1,-1]}` |Voer op de eerste en laatste dag van elke maand tijdens het starten. |
| `{monthDays":[1,14]}` |Voer op de eerste en veertiende dag van elke maand tijdens het starten. |
| `{monthDays":[2]}` |Voer op de tweede dag van de maand tijdens het starten. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Voer op de eerste vrijdag van elke maand om 05: 00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Voer op de eerste vrijdag van elke maand tijdens het starten. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Voer op de derde vrijdag vanaf het einde van de maand, elke maand, tijdens het starten. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Wordt op de eerste en laatste vrijdagdag van elke maand om 5:15 uur uitgevoerd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Voer op de eerste en laatste vrijdag van elke maand tijdens het starten. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Voer op de vijfde vrijdag van elke maand tijdens het starten.<br /><br />Als er geen vijfde vrijdag in een maand, wordt niet de taak uitgevoerd. U kunt overwegen -1 gebruik in plaats van 5 voor het exemplaar, als u uitvoeren van de taak wilt op de laatste optreedt vrijdag van de maand. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Wordt op de laatste vrijdag van de maand elke 15 minuten uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Wordt elke maand op de derde woensdag om 5:15, 5:45, 17:15 en 17:45 uur uitgevoerd. |

## <a name="see-also"></a>Zie ook

- [Wat is Scheduler?](scheduler-intro.md)
- [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)
- [Aan de slag met behulp van Scheduler in Azure Portal](scheduler-get-started-portal.md)
- [Plannen en facturering in Azure Scheduler](scheduler-plans-billing.md)
- [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)
- [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)
- [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)
- [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)
- [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)

