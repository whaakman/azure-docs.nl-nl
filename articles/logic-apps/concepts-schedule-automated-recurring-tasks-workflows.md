---
title: Planning terugkerende taken en werkstromen in Azure Logic Apps
description: Een overzicht over het plannen van terugkerende, geautomatiseerde taken, processen en werkstromen met Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 7f15dc5b28a44dc8405e2f0524913e6012ebe380
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66356052"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Plannen en uitvoeren van terugkerende, geautomatiseerde taken, processen en werkstromen met Azure Logic Apps

Logic Apps helpt u bij het maken en geautomatiseerde terugkerende taken en processen uitvoeren volgens een schema. Door het maken van een werkstroom voor logische Apps die met een ingebouwde terugkeerpatroon of verschuivend venster-trigger die triggers van het schema-type zijn begint, kunt u taken uitvoeren onmiddellijk op een later tijdstip, of op een periodiek interval. U kunt services binnen en buiten Azure, zoals HTTP of HTTPS-eindpunten worden aangeroepen, berichten plaatsen in Azure-services zoals Azure Storage en Azure Service Bus of bestanden geüpload naar een bestandsshare ophalen. Met de trigger terugkeerpatroon, u kunt ook instellen complexe schema's en geavanceerde terugkeerpatronen voor het uitvoeren van taken. Zie voor meer informatie over de ingebouwde schematriggers en acties, [planning en uitvoering terugkerende, geautomatiseerde, taken en werkstromen met Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Hier volgen enkele voorbeelden die laten zien van het soort taken die u kunt uitvoeren:

* Interne gegevens, ophalen, zoals een in SQL opgeslagen procedure uitvoeren elke dag.

* Externe gegevens, zoals pull weerberichten van NOAA om de 15 minuten ophalen.

* Rapportgegevens verzenden, zoals e-mail een overzicht van alle orders groter is dan een bepaald bedrag in de afgelopen week.

* De gegevens verwerken, zoals vandaag comprimeren heeft geüploade afbeeldingen elke weekdag tijdens de daluren.

* Opschonen van gegevens, zoals verwijderen alle tweets die ouder is dan drie maanden.

* Archiveren van gegevens, zoals facturen push met een Backup-service om 01:00 uur elke dag van de volgende negen maanden.

U kunt de planning ingebouwde acties ook gebruiken om uw werkstroom te onderbreken voordat de volgende actie wordt uitgevoerd, bijvoorbeeld:

* Wachten tot een weekdag voor een statusupdate verzenden via e-mail.

* De werkstroom uitstellen totdat er een HTTP-aanroep is tijd om te voltooien voordat u hervatten en bij het ophalen van het resultaat.

In dit artikel beschrijft de mogelijkheden voor de planning ingebouwde triggers en acties.

## <a name="schedule-triggers"></a>Schematriggers

U kun de werkstroom van uw logische app met behulp van de trigger met terugkeerpatroon of verschuivend venster trigger, die niet zijn gekoppeld aan een specifieke service of het systeem, bijvoorbeeld Office 365 Outlook of SQL Server. Deze triggers start en het uitvoeren van uw werkstroom op basis van de opgegeven herhaling waar u het interval en frequentie, zoals het aantal seconden, minuten en uren voor zowel triggers of het aantal dagen, weken of maanden voor de trigger met terugkeerpatroon selecteren. U kunt ook de datum en tijd, evenals de tijdzone instellen. Telkens wanneer die een trigger wordt geactiveerd, Logic Apps maakt en een nieuw werkstroomexemplaar voor uw logische app wordt uitgevoerd.

Dit zijn de verschillen tussen deze triggers:

* **Terugkeerpatroon**: De werkstroom uitvoert met regelmatige tussenpozen op basis van de opgegeven planning. Als terugkeerpatronen zijn gemist, wordt de trigger met terugkeerpatroon niet verwerkt de gemiste herhalingen maar herhalingen met het volgende geplande interval opnieuw wordt opgestart. U kunt een datum en tijd, evenals de tijdzone. Als u 'Day' selecteert, kunt u opgeven hoeveel uren van de dag en minuten van het uur, bijvoorbeeld elke dag om 2:30. Als u 'Week' selecteert, kunt u ook dagen van de week, zoals woensdag en zaterdag liggen. Zie voor meer informatie, [maken, plannen, en terugkerende taken uitvoeren en werkstromen met de trigger met terugkeerpatroon](../connectors/connectors-native-recurrence.md).

* **Sliding Window**: De werkstroom uitvoert met regelmatige tussenpozen die de verwerking van gegevens in segmenten continue. Als terugkeerpatronen zijn gemist, wordt de trigger Verschuivend venster gaat terug en de gemiste herhalingen verwerkt. U kunt een startdatum en tijd, tijdzone en duur wilt uitstellen elke terugkeerpatroon in uw werkstroom. Deze trigger beschikt niet over opties om op te geven dagen, weken en maanden, uur van de dag, de minuten van het uur en dagen van de week. Zie voor meer informatie, [maken, plannen, en terugkerende taken uitvoeren en werkstromen met de trigger Verschuivend venster](../connectors/connectors-native-sliding-window.md).

## <a name="schedule-actions"></a>Schema-acties

Na elke actie in de werkstroom van uw logische app, kunt u de vertraging en vertraging tot acties om uw werkstroom gewacht voordat de volgende actie uitgevoerd.

* **vertraging**: Wachttijd voor het uitvoeren van de volgende actie voor het opgegeven aantal tijdseenheden, zoals seconden, minuten, uren, dagen, weken of maanden. Zie voor meer informatie, [vertraging van de volgende actie in werkstromen](../connectors/connectors-native-delay.md).

* **Vertragen tot**: Wachttijd voor het uitvoeren van de volgende actie tot de opgegeven datum en tijd. Zie voor meer informatie, [vertraging van de volgende actie in werkstromen](../connectors/connectors-native-delay.md).

## <a name="patterns-for-start-date-and-time"></a>Patronen voor datum en tijd

<a name="start-time"></a>

Hier zijn enkele patronen die laten zien hoe u terugkeerpatroon met de datum en tijd kunt beheren en hoe deze herhaling in de Logic Apps-services wordt uitgevoerd:

| Begintijd | Recurrence zonder schedule | Recurrence met schedule (alleen trigger met terugkeerpatroon) |
|------------|-----------------------------|----------------------------------------------------|
| {geen} | De eerste werkbelasting onmiddellijk uitgevoerd. <p>Toekomstige workloads op basis van de laatste uitvoeringstijd worden uitgevoerd. | De eerste werkbelasting onmiddellijk uitgevoerd. <p>Toekomstige workloads op basis van de opgegeven planning worden uitgevoerd. |
| Begintijd in het verleden | **Terugkeerpatroon** trigger: Berekent de uitvoeringstijden op basis van de opgegeven begintijd uitgevoerd en verwijderingen afgelopen uitvoeren. De eerste werkbelasting op de volgende toekomst uitvoeringstijd wordt uitgevoerd. <p>Wordt uitgevoerd toekomstige werkbelastingen op basis van berekeningen van de laatste keer is uitgevoerd. <p><p>**Sliding Window** trigger: Wordt berekend op basis van de opgegeven begintijd uitgevoerd en de afgelopen uitvoeren aan tijdstippen uitvoeringstijden. <p>Toekomstige werkbelastingen op basis van berekeningen van de opgegeven begintijd uitgevoerd. <p><p>Zie het voorbeeld onder deze tabel voor meer informatie. | De eerste werkbelasting wordt uitgevoerd *nooit* dan de begintijd, op basis van de planning die wordt berekend op basis van de begintijd. <p>Toekomstige workloads op basis van de opgegeven planning worden uitgevoerd. <p>**Opmerking:** Als u een terugkeerpatroon met een schema opgeven, maar geen uren of minuten voor de planning opgeven, zijn klikt u vervolgens toekomstige uitvoeringstijden berekend met behulp van de uren of minuten, respectievelijk van de eerste uitvoeringstijd. |
| Begintijd op dit moment of in de toekomst | De eerste werkbelasting op de opgegeven begintijd uitgevoerd. <p>Wordt uitgevoerd toekomstige werkbelastingen op basis van berekeningen van de laatste keer is uitgevoerd. | De eerste werkbelasting wordt uitgevoerd *nooit* dan de begintijd, op basis van de planning die wordt berekend op basis van de begintijd. <p>Toekomstige workloads op basis van de opgegeven planning worden uitgevoerd. <p>**Opmerking:** Als u een terugkeerpatroon met een schema opgeven, maar geen uren of minuten voor de planning opgeven, zijn klikt u vervolgens toekomstige uitvoeringstijden berekend met behulp van de uren of minuten, respectievelijk van de eerste uitvoeringstijd. |
||||

*Voorbeeld voor afgelopen begintijd en terugkeerpatroon maar geen Schedule is opgegeven*

Stel dat de huidige datum en tijd loopt 8 September 2017 13:00 uur. U opgeven de begindatum en begintijd als 7 September 2017 om 2:00 uur, die zich in het verleden en een terugkeer die wordt uitgevoerd elke twee dagen.

| Begintijd | Huidige tijd | Terugkeerpatroon | Planning |
|------------|--------------|------------|----------|
| 2017-09-**07**T14:00:00Z <br>(2017-09 -**07** om 14:00 uur) | 2017-09-**08**T13:00:00Z <br>(2017-09 -**08** om 1:00 uur) | 2 dagen | {geen} |
|||||

Voor de trigger terugkeerpatroon de Logic Apps engine berekent op basis van de begintijd keer uitvoeren worden verwijderd na uitvoeren, maakt gebruik van de volgende toekomst begintijd voor de eerste uitvoeren en berekent de toekomst op wordt uitgevoerd op basis van de laatste uitvoeringstijd.

Hier ziet u hoe dit terugkeerpatroon er uitziet:

| Begintijd | Eerste uitvoeringstijd | Toekomstige keer uitvoeren |
|------------|----------------|------------------|
| 2017-09 -**07** om 14:00 uur | 2017-09 -**09** om 14:00 uur | 2017-09 -**11** om 14:00 uur </br>2017-09 -**13** om 14:00 uur </br>2017-09 -**15** om 14:00 uur </br>enzovoort... |
||||

Ja, ongeacht hoe ver in het verleden die u bijvoorbeeld, de starttijd geeft 2017-09 -**05** om 2:00 of 2017-09 -**01** om 14:00 uur, uw eerste uitgevoerd altijd maakt gebruik van de toekomst van de volgende keer start.

Maakt gebruik van de begintijd voor de eerste keer uitvoert en berekent de toekomstige wordt uitgevoerd op basis van de begintijd voor de trigger venster schuiven, de Logic App-engine berekent op basis van de begintijd keer uitvoeren aan afgelopen keer uitgevoerd.

Hier ziet u hoe dit terugkeerpatroon er uitziet:

| Begintijd | Eerste uitvoeringstijd | Toekomstige keer uitvoeren |
|------------|----------------|------------------|
| 2017-09 -**07** om 14:00 uur | 2017-09 -**07** om 14:00 uur | 2017-09 -**09** om 14:00 uur </br>2017-09 -**11** om 14:00 uur </br>2017-09 -**13** om 14:00 uur </br>2017-09 -**15** om 14:00 uur </br>enzovoort... |
||||

Ja, ongeacht hoe ver in het verleden die u bijvoorbeeld, de starttijd geeft 2017-09 -**05** om 2:00 of 2017-09 -**01** om 14:00 uur, uw eerste uitgevoerd altijd maakt gebruik van de opgegeven begintijd.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Voorbeeld van de herhalingen

Hier volgen verschillende voorbeeld herhalingen die u kunt instellen voor de triggers die ondersteuning bieden voor de opties:

| Trigger | Terugkeerpatroon | Interval | Frequentie | Begintijd | Deze dagen | Deze uren | Deze minuten | Opmerking |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Terugkeerpatroon <br>Sliding window | Elke 15 minuten (geen begindatum en tijdstip) uitgevoerd | 15 | Minuut | {geen} | {unavailable} | {geen} | {geen} | Dit schema wordt onmiddellijk gestart en toekomstige herhalingen op basis van de laatste uitvoeringstijd wordt berekend. |
| Terugkeerpatroon <br>Sliding window | Elke 15 minuten uitgevoerd (met de begindatum en -tijd) | 15 | Minuut | *startDate*T*startTime*Z | {unavailable} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* dan de opgegeven datum en tijd, berekent u toekomstige herhalingen op basis van de laatste uitvoeringstijd. |
| Terugkeerpatroon <br>Sliding window | Elk uur wordt uitgevoerd op het hele uur (met de begindatum en -tijd) | 1 | Uur | *startDate*Thh:00:00Z | {unavailable} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* start dan de opgegeven datum en tijd. Toekomstige herhalingen wordt elk uur uitgevoerd op de "00" minuut is ingeschakeld, die wordt berekend op basis van de begintijd. <p>Als de frequency 'Week' of 'Maand' is, wordt dit schema respectievelijk slechts één dag per week of één dag per maand uitgevoerd. |
| Terugkeerpatroon <br>Sliding window | Elk uur, dagelijks (geen begindatum en tijdstip) uitgevoerd | 1 | Uur | {geen} | {unavailable} | {geen} | {geen} | Dit schema wordt onmiddellijk gestart en toekomstige herhalingen op basis van de laatste uitvoeringstijd wordt berekend. <p>Als de frequency 'Week' of 'Maand' is, wordt dit schema respectievelijk slechts één dag per week of één dag per maand uitgevoerd. |
| Terugkeerpatroon <br>Sliding window | Elk uur, dagelijks (met de begindatum en -tijd) uitgevoerd | 1 | Uur | *startDate*T*startTime*Z | {unavailable} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* dan de opgegeven datum en tijd, berekent u toekomstige herhalingen op basis van de laatste uitvoeringstijd. <p>Als de frequency 'Week' of 'Maand' is, wordt dit schema respectievelijk slechts één dag per week of één dag per maand uitgevoerd. |
| Terugkeerpatroon <br>Sliding window | Om de 15 minuten na het uur, elk uur uitgevoerd (met de begindatum en -tijd) | 1 | Uur | *startDate*T00:15:00Z | {unavailable} | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* start dan de opgegeven datum en tijd. Toekomstige herhalingen uitvoeren op de "15" minuut is ingeschakeld, die wordt berekend vanaf het begin tijd dus om 00:15 uur, 1:15 uur, 2:15 uur, enzovoort. |
| Terugkeerpatroon | Elke 15 minuten na het uur, elk uur (geen begindatum en tijdstip) uitgevoerd | 1 | Dag | {geen} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Dit schema wordt uitgevoerd om 00:15 uur, 1:15 uur, 2:15 uur, enzovoort. Ook is dit schema gelijkwaardig met een frequentie van 'Uur' en een begintijd met '15' minuten. |
| Terugkeerpatroon | Elke 15 minuten uitgevoerd op de opgegeven minuut merken (geen begindatum en tijdstip). | 1 | Dag | {geen} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dit schema start niet totdat de volgende 15 minuten is ingeschakeld opgegeven. |
| Terugkeerpatroon | Uitgevoerd om 8:00 uur op elke dag (geen begindatum en tijdstip) | 1 | Dag | {geen} | {unavailable} | 8 | {geen} | Dit schema wordt uitgevoerd om 8:00 uur op elke dag, op basis van de opgegeven planning. |
| Terugkeerpatroon | Uitgevoerd om 8:00 uur op elke dag (met de begindatum en -tijd) | 1 | Dag | *startDate*T08:00:00Z | {unavailable} | {geen} | {geen} | Dit schema wordt elke dag 8:00 uur uitgevoerd op basis van de opgegeven begintijd uitgevoerd. | 
| Terugkeerpatroon | Uitgevoerd om 8:30 uur voor elke dag (geen begindatum en tijdstip) | 1 | Dag | {geen} | {unavailable} | 8 | 30 | Dit schema wordt uitgevoerd om 8:30 uur voor elke dag, op basis van de opgegeven planning. |
| Terugkeerpatroon | Uitgevoerd om 8:30 uur voor elke dag (met de begindatum en -tijd) | 1 | Dag | *startDate*T08:30:00Z | {unavailable} | {geen} | {geen} | Dit schema wordt gestart op de opgegeven begindatum om 8:30 uur. |
| Terugkeerpatroon | Op 8:30:00 uur en 4:30 uur, dagelijks uitvoeren | 1 | Dag | {geen} | {unavailable} | 8, 16 | 30 | |
| Terugkeerpatroon | Uitgevoerd om 8:30 uur, 8:45 AM, 4:30 PM, en elke dag 4:45 uur | 1 | Dag | {geen} | {unavailable} | 8, 16 | 30, 45 | |
| Terugkeerpatroon | Voer elke zaterdag om 17: 00 (geen begindatum en tijdstip) | 1 | Wekelijks | {geen} | "Zaterdag" | 17 | 00 | Dit schema wordt elke zaterdag om 17:00 uur uitgevoerd. |
| Terugkeerpatroon | Elke zaterdag om 17: 00 uur (met de begindatum en -tijd) wordt uitgevoerd | 1 | Wekelijks | *startDate*T17:00:00Z | "Zaterdag" | {geen} | {geen} | Dit schema wordt niet gestart *alle eerder* start dan de opgegeven datum en tijd, in dit geval 9 September 2017 om 17:00 uur. Toekomstige herhalingen elke zaterdag om 17:00 uur uitgevoerd. |
| Terugkeerpatroon | Elke dinsdag, donderdag om 17: 00 uur wordt uitgevoerd | 1 | Wekelijks | {geen} | "Dinsdag", "Donderdag" | 17 | {geen} | Dit schema wordt elke dinsdag en donderdag om 17:00 uur uitgevoerd. |
| Terugkeerpatroon | Elk uur tijdens werkuren uitgevoerd | 1 | Wekelijks | {geen} | Selecteer alle dagen behalve zaterdag en zondag. | Selecteer de uren van de dag die u wilt. | Selecteer een minuten van het uur dat u wilt. | Bijvoorbeeld, als uw werkuren 8:00 uur tot 5:00 uur zijn, selecteert u "8, 9, 10, 11, 12, 13, 14, 15, 16 en 17" Als de uren van de dag. <p>Als uw werkuren 8:30:00 uur tot 5:30 uur, selecteert u de vorige uur van de dag plus "30" Als het aantal minuten van het uur. |
| Terugkeerpatroon | Eenmaal elke dag worden uitgevoerd tijdens het weekend | 1 | Wekelijks | {geen} | "Zaterdag", "Zondag" | Selecteer de uren van de dag die u wilt. | Selecteer een minuten van het uur waar nodig. | Dit schema wordt elke zaterdag en zondag uitgevoerd volgens het opgegeven schema. |
| Terugkeerpatroon | Elke 15 minuten uitgevoerd op maandag alleen tweewekelijks | 2 | Wekelijks | {geen} | "Maandag" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dit schema wordt uitgevoerd elke andere maandag tot en met elke 15 minuten is ingeschakeld. |
| Terugkeerpatroon | Iedere maand uitvoeren | 1 | Maand | *startDate*T*startTime*Z | {unavailable} | {unavailable} | {unavailable} | Dit schema wordt niet gestart *alle eerder* dan de opgegeven begindatum en -tijd en toekomstige herhalingen berekend op de datum en tijd. Als u geen een datum en tijd opgeeft, geeft dit schema maakt gebruik van de datum en tijd. |
| Terugkeerpatroon | Elk uur gedurende één dag per maand uitgevoerd | 1 | Maand | {Zie Opmerking} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {Zie Opmerking} | Als u geen een datum en tijd opgeeft, geeft dit schema maakt gebruik van de datum en tijd. Geef het aantal minuten van het uur, een begintijd voor het beheren van de minuten voor het terugkeerschema of gebruik van de aanmaaktijd. Bijvoorbeeld, als de begin- of Aanmaaktijd 8:25 uur is, dit schema wordt uitgevoerd om 8:25 uur, 9:25 uur, 10:25 uur, enzovoort. |
|||||||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Slechts één keer worden uitgevoerd

Als u uitvoeren van uw logische app wilt alleen in één keer in de toekomst kunt u de **Scheduler: Eenmaal uitvoeren van taken** sjabloon. Nadat u een nieuwe logische app hebt gemaakt, maar voor het openen van de ontwerper van logische Apps, onder de **sjablonen** sectie van de **categorie** in de lijst met **planning**, en selecteer vervolgens Deze sjabloon:

![Selecteer ' Scheduler: Eenmaal taken uitvoeren'-sjabloon](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

Of, als kun u uw logische app met de **wanneer een HTTP-aanvraag wordt ontvangen - aanvraag** activeren en de begintijd geven als een parameter voor de trigger. Voor de eerste actie, gebruikt u de **vertragen tot - plannen** actie, en geef de tijd voor wanneer de volgende actie wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

* [Maken, plannen en uitvoeren van terugkerende taken en werkstromen met de trigger met terugkeerpatroon](../connectors/connectors-native-recurrence.md)
* [Maken, plannen en uitvoeren van terugkerende taken en werkstromen met de trigger venster schuiven](../connectors/connectors-native-sliding-window.md)
* [Werkstromen onderbreken met de acties die vertraging](../connectors/connectors-native-delay.md)