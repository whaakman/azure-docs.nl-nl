---
title: Het bouwen van complexe schema's en geavanceerde terugkeerpatronen met Azure Scheduler
description: Het bouwen van complexe schema's en geavanceerde terugkeerpatronen met Azure Scheduler
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: e1e45d394a4c442a4fb255ed6d838a589e98860e
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="how-to-build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Het bouwen van complexe schema's en geavanceerde terugkeerpatronen met Azure Scheduler
## <a name="overview"></a>Overzicht
De kern van een Azure Scheduler taak is de *planning*. De planning wordt bepaald hoe en wanneer de taak wordt uitgevoerd door de planner.

Azure Scheduler kunt u verschillende eenmalige en terugkerende planningen voor een taak opgeven. *Eenmalige* planningen effectief eenmaal op een opgegeven periode – is gestart, zijn *terugkerende* schema's die slechts één keer worden uitgevoerd. Terugkerende planningen geactiveerd op een vooraf vastgestelde frequentie.

Met deze flexibiliteit kunt Azure Scheduler u ondersteuning voor een groot aantal bedrijfsscenario's:

* Opruimen van periodieke gegevens – bijvoorbeeld dagelijks verwijderen voor alle tweets ouder is dan 3 maanden
* Archivering – bijvoorbeeld elke maand factuur geschiedenis push naar de Backup-service
* Aanvragen voor externe gegevens – bijvoorbeeld pull elke 15 minuten,-weerrapport door nieuwe ski van NOAA
* Afbeelding van verwerking – bijvoorbeeld elke weekdag tijdens de daluren, gebruikt u cloudcomputing geüpload voor het comprimeren van afbeeldingen die dag

In dit artikel doorlopen we voorbeeld taken die u met Azure Scheduler maken kunt. We bieden de JSON-gegevens die worden beschreven van het schema. Als u de [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx), kunt u deze dezelfde JSON voor [maken van een Azure Scheduler-taak](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Ondersteunde scenario 's
De vele voorbeelden in dit onderwerp ziet u de breedte van de scenario's die ondersteuning biedt voor Azure Scheduler. Deze voorbeelden illustreren ruime zin is het maken van schema's voor veel gedrag patronen, met inbegrip van de onderstaande:

* Eenmaal uitvoeren op een bepaalde datum en tijd
* Uitvoeren en een aantal expliciete keer herhaald
* Direct uitgevoerd en herhaald
* Voer en herhaald elke  *n*  minuten, uren, dagen, weken of maanden, beginnend bij een bepaalde tijd
* Voer en herhaald in wekelijkse of maandelijkse frequentie, maar alleen op bepaalde dagen, specifieke dagen van de week of specifieke dagen van maand
* Voer en op meerdere keren in een periode – bijvoorbeeld laatste vrijdag en maandag van elke maand of op 5:15 uur en 17:15:00 uur elke dag herhaald

## <a name="dates-and-datetimes"></a>Datum en tijd
Voer de datums in Azure Scheduler-taken de [ISO 8601-specificatie](http://en.wikipedia.org/wiki/ISO_8601) en alleen de datum.

Voer de datum / tijd-verwijzingen in Azure Scheduler-taken de [ISO 8601-specificatie](http://en.wikipedia.org/wiki/ISO_8601) en zowel datum en tijd onderdelen bevatten. Een datum / tijd waarvoor geen een UTC-afwijking wordt ervan uitgegaan dat UTC.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Procedure: JSON en REST-API gebruiken voor het maken van planningen
Maken van een eenvoudige planning met de [Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143), eerste [registreren van uw abonnement met een resourceprovider](https://msdn.microsoft.com/library/azure/dn790548.aspx) (de providernaam van de voor de planner is *Microsoft.Scheduler*), vervolgens [maken van een taakverzameling](https://msdn.microsoft.com/library/mt629159.aspx), en tot slot [maken van een taak](https://msdn.microsoft.com/library/mt629145.aspx). Wanneer u een taak maakt, kunt u de planning en een terugkeerpatroon met een JSON zoals hieronder wordt overgenomen kunt opgeven:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // how often to fire
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default to recur infinitely)
            "endTime": "2012-11-04",      // optional (default to recur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Overzicht: Taak Schema basisbeginselen
De volgende tabel bevat een overzicht van de belangrijkste onderdelen die betrekking hebben op terugkeerpatroon en in een taak plannen:

| **De naam van de JSON** | **Beschrijving** |
|:--- |:--- |
| ***startTime*** |*startTime* is een datum / tijd. Voor eenvoudige planningen *startTime* is het eerste exemplaar en voor complexe schema's van de taak niet eerder dan gaat *startTime*. |
| ***terugkeerpatroon*** |De *terugkeerpatroon* object geeft terugkeerpatroon regels voor de taak en de herhaling van de taak wordt uitgevoerd waarbij. Het object terugkeert ondersteunt de elementen *frequentie, interval, endTime, count,* en *planning*. Als *terugkeerpatroon* is gedefinieerd, *frequentie* is vereist; andere elementen van *terugkeerpatroon* zijn optioneel. |
| ***frequentie*** |De *frequentie* de eenheid van de frequentie waarmee de taak terugkeert type string. Ondersteunde waarden zijn *'minuut', 'uur', 'day', 'week'* of *"maand".* |
| ***interval*** |De *interval* een positief geheel getal is en geeft het interval voor de *frequentie* waarmee wordt bepaald hoe vaak de taak wordt uitgevoerd. Bijvoorbeeld, als *interval* 3 en *frequentie* 'week', is de taak herhaald om de drie weken. Azure Scheduler ondersteunt maximaal *interval* 18 maanden voor maandelijkse frequentie, 78 weken voor de frequentie wekelijks of 548 dagen voor de frequentie van dagelijkse. Voor uren en minuten frequentie, het ondersteunde bereik is 1 < = *interval* < = 1000. |
| ***Eindtijd*** |De *endTime* tekenreeks bevat de laatste datum / tijd-de taak moet niet worden uitgevoerd. Het is geen geldige hebben een *endTime* in het verleden. Als er geen *endTime* of count is opgegeven, wordt de taak oneindig wordt uitgevoerd. Beide *endTime* en *aantal* kan niet worden opgenomen voor hetzelfde project. |
| ***aantal*** |<p>De *aantal* een positief geheel getal (groter dan nul) waarmee het aantal keren dat deze taak voordat wordt voltooid uitvoeren moet.</p><p>De *aantal* geeft het aantal keren dat de taak wordt uitgevoerd voordat er bepaald wordt voltooid. Bijvoorbeeld: voor een taak die dagelijks wordt uitgevoerd met *aantal* 5 en de begindatum van maandag tot, de taak is voltooid na uitvoering op vrijdag. Als de begindatum verstreken is, wordt de eerste uitvoering van de aanmaaktijd berekend.</p><p>Als er geen *endTime* of *aantal* is opgegeven, wordt de taak oneindig wordt uitgevoerd. Beide *endTime* en *aantal* kan niet worden opgenomen voor hetzelfde project.</p> |
| ***planning*** |Een taak met een opgegeven frequentie wijzigt het terugkeerpatroon op basis van een terugkerende planning. Een *planning* bevat wijzigingen die op basis van de minuten, uren, weekdagen, dagen van de maand en weeknummer. |

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Overzicht: De taak Schema standaardwaarden, limieten en voorbeelden
Dit overzicht we Bespreek elk van deze elementen in detail.

| **De naam van de JSON** | **Waardetype** | **Vereist?** | **Standaardwaarde** | **Geldige waarden** | **Voorbeeld** |
|:--- |:--- |:--- |:--- |:--- |:--- |
| ***startTime*** |Tekenreeks |Nee |None |Datums en tijden volgens ISO 8601 |<code>"startTime" : "2013-01-09T09:30:00-08:00"</code> |
| ***terugkeerpatroon*** |Object |Nee |None |Recurrence-object |<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code> |
| ***frequentie*** |Tekenreeks |Ja |None |'minuut', 'uur', 'day', 'week', 'maand' |<code>"frequency" : "hour"</code> |
| ***interval*** |Aantal |Ja |None |1 tot 1000. |<code>"interval":10</code> |
| ***Eindtijd*** |Tekenreeks |Nee |None |Datum-/tijdwaarde van een moment in de toekomst |<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
| ***aantal*** |Aantal |Nee |None |>= 1 |<code>"count": 5</code> |
| ***planning*** |Object |Nee |None |Schedule-object |<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code> |

## <a name="deep-dive-starttime"></a>Deep Dive: *startTime*
De volgende tabel opnamen hoe *startTime* bepaalt hoe een taak wordt uitgevoerd.

| **de waarde startTime** | **Geen terugkeerpatroon** | **Terugkeerpatroon. Er is geen schema** | **Terugkeerpatroon met planning** |
|:--- |:--- |:--- |:--- |
| **Er is geen begintijd** |Eenmaal onmiddellijk uitvoeren |Eenmaal onmiddellijk uitvoeren. Bij een volgende uitvoering op basis van het berekenen van de laatste uitvoertijd uitvoeren |<p>Eenmaal onmiddellijk uitvoeren</p><p>Volgende uitvoeringen worden op basis van de terugkeerplanning uitgevoerd</p> |
| **De begintijd in het verleden** |Eenmaal onmiddellijk uitvoeren |<p>Eerste toekomstige uitvoeringstijd berekenen na de begintijd liggen, en op dat moment worden uitgevoerd</p><p>Bij een volgende uitvoering op basis van oncalculating uitvoeren vanaf laatste uitvoeringstijd</p><p>Zie het voorbeeld na deze tabel voor een nadere uitleg</p> |<p>Taak gestart *niet sooner dan* de opgegeven begintijd. Het eerste exemplaar is gebaseerd op de planning die wordt berekend op basis van de begintijd</p><p>Volgende uitvoeringen worden op basis van de terugkeerplanning uitgevoerd</p> |
| **De begintijd in de toekomst, of op dit moment** |Eenmaal op de opgegeven begintijd uitvoeren |<p>Eenmaal op de opgegeven begintijd uitvoeren</p><p>Bij een volgende uitvoering op basis van het berekenen van de laatste uitvoertijd uitvoeren</p> |<p>Taak gestart *niet sooner dan* de opgegeven begintijd. Het eerste exemplaar is gebaseerd op de planning die wordt berekend op basis van de begintijd</p><p>Volgende uitvoeringen worden op basis van de terugkeerplanning uitgevoerd</p> |

We bekijken een voorbeeld van wat er gebeurt wanneer *startTime* is in het verleden met *terugkeerpatroon* , maar er is geen *planning*.  Wordt ervan uitgegaan dat de huidige tijd 2015-04-08 13:00 *startTime* 2015-04-07 14:00 en *terugkeerpatroon* is elke 2 dagen (gedefinieerd met *frequentie*: dag en *interval*: 2.) Houd er rekening mee dat de *startTime* is in het verleden en vindt plaats voordat de huidige tijd

In deze omstandigheden de *eerste uitvoering* 2015-04-09 worden om 14:00 uur\. De scheduler-engine berekent uitvoeringen vanaf de startTime.  Alle uitvoeringen in het verleden worden genegeerd. De engine gebruikt de eerstvolgende uitvoering die in de toekomst plaatsvindt.  Dus in dit geval *startTime* 2015-04-07 op 14:00 uur is, dus is het volgende exemplaar 2 dagen na deze periode kan die 2015-04-09 op 14:00 uur.

Opmerking dat de eerste uitvoering zou worden de dezelfde zelfs als de startTime 2015-04-05 14:00 of 2015-04-01 14:00\. Na de eerste uitvoering bij een volgende uitvoering wordt berekend met behulp van de geplande – zodat u deze zou op 2015-04-11 op 14:00 uur, vervolgens 2015-04-13 op 14:00 uur, vervolgens 2015-04-15 op 14:00 uur, enzovoort.

Ten slotte wanneer een taak een planning heeft als uren en/of minuten zijn niet ingesteld in de planning, standaard ze de uren en/of de minuten van de eerste uitvoering, respectievelijk.

## <a name="deep-dive-schedule"></a>Deep Dive: *planning*
Enerzijds een *planning* kunt *limiet* het aantal uitvoeringen van de taak.  Bijvoorbeeld, als een taak met een frequentie "maand" heeft een *planning* dat wordt uitgevoerd op enige dag 31, de taak wordt uitgevoerd in alleen deze maanden waarvoor een 31<sup>st</sup> dag.

Aan de andere kant een *planning* kunt ook *Vouw* het aantal uitvoeringen van de taak. Bijvoorbeeld, als een taak met een frequentie "maand" heeft een *planning* dat wordt uitgevoerd op dagen van de maand 1 en 2, de taak wordt uitgevoerd in de 1<sup>st</sup> en 2<sup>nd</sup> dagen van de maand in plaats van slechts één keer per maand.

Als meerdere schema-elementen worden opgegeven, de volgorde van de evaluatie van de grootste waarde is naar de kleinste – week number, maand, dag,, weekdag, uur en minuut.

De volgende tabel beschrijft *planning* elementen in detail.

| **De naam van de JSON** | **Beschrijving** | **Geldige waarden** |
|:--- |:--- |:--- |
| **minuten** |Minuten van het uur waarop de taak wordt uitgevoerd |<ul><li>Matrix van gehele getallen</li></ul> |
| **uren** |Uur van de dag waarop de taak wordt uitgevoerd |<ul><li>Matrix van gehele getallen</li></ul> |
| **Weekdagen** |Dagen van de week de taak wordt uitgevoerd. Kan alleen worden opgegeven met de frequency wekelijks. |<ul><li>Matrix van elk van de onderstaande waarden (maximale grootte van de matrix 7)<ul><li>"Maandag"</li><li>"Dinsdag"</li><li>"Woensdag"</li><li>"Donderdag"</li><li>"Vrijdag"</li><li>"Zaterdag"</li><li>"Zondag"</li></ul></li></ul>*Niet* hoofdlettergevoelig |
| **monthlyOccurrences** |Hiermee wordt bepaald welke dagen van de maand de taak wordt uitgevoerd. Kan alleen worden opgegeven met de frequency maandelijks. |<ul><li>Matrix van monthlyOccurrence objecten:</li></ul> <pre>{ "day": *day*,<br />  "occurrence": *occurrence*<br />}</pre><p> *dag* is van de dag van de week de taak wordt uitgevoerd, bijvoorbeeld {zondag} is elke zondag van de maand. Vereist.</p><p>Het aantal exemplaren is *exemplaar* van de dag tijdens de maand, bijvoorbeeld {zondag, -1} is de laatste zondag van de maand. Optioneel.</p> |
| **maanddagen** |Dag van de maand die de taak wordt uitgevoerd. Kan alleen worden opgegeven met de frequency maandelijks. |<ul><li>Een matrix van onderstaande waarden</li><ul><li>Een waarde < = -1 en > =-31.</li><li>Een willekeurige waarde > = 1 en < = 31.</li></ul></ul> |

## <a name="examples-recurrence-schedules"></a>Voorbeelden: Terugkerende schema 's
Hier volgen verschillende voorbeelden van terugkerende planningen – gericht op het object schema en de onderliggende elementen.

De planningen hieronder alle wordt ervan uitgegaan dat de *interval* is ingesteld op 1\. Ook een moet wordt ervan uitgegaan dat de juiste frequentie overeenkomstig wat is er in de *planning* – bijvoorbeeld een kan niet gebruiken frequentie 'day' en 'maanddagen' wijziging in de planning. Deze beperkingen zijn hierboven beschreven.

| **Voorbeeld** | **Beschrijving** |
|:--- |:--- |
| <code>{"hours":[5]}</code> |Op 5 uur uitgevoerd elke dag. Azure Scheduler komt overeen met elke waarde in 'hours' met elke waarde in 'minutes', één met één voor het maken van een lijst van alle tijden waarop de taak wordt uitgevoerd. |
| <code>{"minutes":[15], "hours":[5]}</code> |Op 5:15 uur uitgevoerd elke dag |
| <code>{"minutes":[15], "hours":[5,17]}</code> |Uitvoeren op 5:15 uur en 17:15:00 uur elke dag |
| <code>{"minutes":[15,45], "hours":[5,17]}</code> |Uitvoeren op 5:15 uur, 5:45 uur 17:15:00 uur en 17:45 uur elke dag |
| <code>{"minutes":[0,15,30,45]}</code> |Elke 15 minuten wordt uitgevoerd |
| <code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code> |Elk uur uitgevoerd. Deze taak elk uur wordt uitgevoerd. De minuut wordt bepaald door de *startTime*, als er een is opgegeven of als niets wordt opgegeven door de aanmaaktijd. Bijvoorbeeld, als de begintijd of Aanmaaktijd (afhankelijk van wat van toepassing) 12:25 uur, de taak wordt uitgevoerd op 00:25, 01:25 02:25,..., 23:25. De planning komt overeen met een taak met *frequentie* van 'uur' een *interval* van 1, maar er is geen *planning*. Het verschil is dat dit schema kan worden gebruikt met andere *frequentie* en *interval* andere taken te maken. Bijvoorbeeld, als de *frequentie* zijn "maand", de planning wordt uitgevoerd slechts één keer per maand in plaats van elke dag als *frequentie* zijn "dag" |
| <code>{minutes:[0]}</code> |Elk uur worden uitgevoerd op het uur. Deze taak wordt ook uitgevoerd voor elk uur, maar ook op het uur (bijvoorbeeld 12: 00 A.M., 1 uur 2 uur, enz.) Dit is gelijkwaardig aan een taak met de frequentie van een "uur", een startTime met nul minuten en geen planning als de frequentie zijn 'day', maar als de frequentie 'week' of 'maand', de planning zou worden gebruikt voor het uitvoeren van slechts één dag een week of één dag per maand, respectievelijk. |
| <code>{"minutes":[15]}</code> |Op 15 minuten worden uitgevoerd uit het verleden uur elk uur. Wordt elk uur uitgevoerd vanaf 00:15, 01:15, 02,15, enz. en eindigt om 22:15 en 23:15. |
| <code>{"hours":[17], "weekDays":["saturday"]}</code> |Uitvoeren op 17: 00 uur op zaterdag elke Week |
| <code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Uitvoeren op 17: 00 uur op maandag, woensdag en vrijdag elke Week |
| <code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Uitvoeren op 17:15:00 uur en 17:45 uur op maandag, woensdag en vrijdag elke Week |
| <code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Uitvoeren op 5 uur en 17: 00 uur op maandag, woensdag en vrijdag elke Week |
| <code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Uitvoeren op 5:15 uur, 5:45 uur 17:15:00 uur en 17:45 uur op maandag, woensdag en vrijdag elke Week |
| <code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Wordt elke 15 minuten uitgevoerd op weekdagen |
| <code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Elke 15 minuten wordt uitgevoerd op weekdagen tussen 9: 00 uur en 4:45 uur |
| <code>{"weekDays":["sunday"]}</code> |Uitvoeren op zondag tijdens het starten |
| <code>{"weekDays":["tuesday", "thursday"]}</code> |Uitvoeren op dinsdag en donderdag tijdens het starten |
| <code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code> |Om 6.00 uur wordt uitgevoerd op de 28 dag van elke maand (ervan uitgaande dat de frequentie van de maand) |
| <code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code> |Om 6.00 uur op de laatste dag van de maand wordt uitgevoerd. Als u een taak uitvoeren op de laatste dag van een maand wilt, gebruik dan -1 in plaats van de dag 28, 29, 30 of 31. |
| <code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code> |Uitvoeren om 6.00 uur op de eerste en laatste dag van elke maand |
| <code>{monthDays":[1,-1]}</code> |Voer op de eerste en laatste dag van elke maand tijdens het starten |
| <code>{monthDays":[1,14]}</code> |Voer op de eerste en veertiende dag van elke maand tijdens het starten |
| <code>{monthDays":[2]}</code> |Voer op de tweede dag van de maand tijdens het starten |
| <code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |Voer op de eerste vrijdag van elke maand om 05: 00 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |: De gegevens worden uitgevoerd op de eerste vrijdag van elke maand tijdens het starten |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code> |Voer op de derde vrijdag uit einde van de maand, elke maand, tijdens het starten |
| <code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Op de eerste en laatste vrijdag van elke maand 5:15 uur uitvoeren |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Voer op de eerste en laatste vrijdag van elke maand tijdens het starten |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code> |Voer op vijfde vrijdag van elke maand tijdens het starten. Als er geen vijfde vrijdag in een maand, wordt dit niet uitgevoerd, omdat deze is gepland voor uitvoering op alleen vijfde vrijdag. U kunt overwegen -1 gebruik in plaats van 5 voor het exemplaar, als u uitvoeren van de taak wilt op de laatste optreedt vrijdag van de maand. |
| <code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code> |Om de 15 minuten worden uitgevoerd op de laatste vrijdag van de maand |
| <code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code> |Uitvoeren op 5:15 uur, 5:45 uur 17:15:00 uur en 17:45 uur op de 3e woensdag van elke maand |

## <a name="see-also"></a>Zie ook
 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met behulp van Scheduler in Azure Portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Hoge beschikbaarheid en betrouwbaarheid van Azure Scheduler](scheduler-high-availability-reliability.md)

 [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)

