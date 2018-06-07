---
title: Veelvoorkomende querypatronen in Azure Stream Analytics
description: In dit artikel beschrijft een aantal veelvoorkomende querypatronen en ontwerpen die nuttig in Azure Stream Analytics-taken zijn.
services: stream-analytics
author: jseb225
manager: kfile
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: f63ccd62136fe8d556a4cfb591e3294f3751dfb3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652243"
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Voorbeelden van algemene gebruikspatronen van de Stream Analytics query

## <a name="introduction"></a>Inleiding
Query's in Azure Stream Analytics worden uitgedrukt in een SQL-achtige querytaal. De taalconstructs worden beschreven in de [Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) handleiding. 

Ontwerp van de query kunt snelle eenvoudige Pass Through-logica voor het verplaatsen van gegevens van gebeurtenissen van één invoerstroom in een ander gegevensarchief van de uitvoer. Of het uitgebreide patroon overeenkomende en tijdelijke analyse voor het berekenen van statistische functies via verschillende tijdvensters zoals in het voorbeeld TollApp kunt doen. U kunt deelnemen aan gegevens uit meerdere invoer om te combineren met het streaming-gebeurtenissen en zoekacties tegen statische referentiegegevens zodat de gebeurtenis-waarden aanvullen. U kunt ook gegevens naar meerdere uitgangen schrijven.

In dit artikel bevat een overzicht van oplossingen voor enkele veelvoorkomende querypatronen, op basis van praktijkscenario's. Het is een onderhanden werk en met nieuwe patronen voortdurend wordt bijgewerkt.

## <a name="query-example-convert-data-types"></a>Query-voorbeeld: gegevenstypen converteren
**Beschrijving**: het definiëren van de typen eigenschappen voor de invoerstroom.
Bijvoorbeeld: het gewicht auto is afkomstig uit de invoerstroom als tekenreeksen en moet worden geconverteerd naar **INT** om uit te voeren **som** het.

**Invoer**:

| Maken | Time | Gewicht |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Uitvoer**:

| Maken | Gewicht |
| --- | --- |
| Honda |3000 |

**Oplossing**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Uitleg**: gebruik een **CAST** -instructie in de **gewicht** aan te geven van het gegevenstype. Zie de lijst met ondersteunde gegevenstypen in [gegevenstypen (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).

## <a name="query-example-use-likenot-like-to-do-pattern-matching"></a>Query-voorbeeld: gebruik achtige/niet wilt patroon die overeenkomt met
**Beschrijving**: controleert of de waarde van een veld van de gebeurtenis overeenkomt met een bepaald patroon.
Bijvoorbeeld, Controleer het resultaat overschrijven die beginnen met A en eindigen met 9.

**Invoer**:

| Maken | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| Maken | LicensePlate | Time |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Oplossing**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Uitleg**: Gebruik de **zoals** instructie om te controleren de **LicensePlate** veld waarde. Deze moet beginnen met een A, en vervolgens hebt een willekeurige tekenreeks van nul of meer tekens en vervolgens eindigen met een 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Query-voorbeeld: Geef de logica voor andere gevallen en-waarden (CASE-instructies)
**Beschrijving**: Geef een andere berekeningen voor het veld, op basis van een bepaald criterium.
Geef bijvoorbeeld een beschrijving van de tekenreeks voor het maken van het aantal auto's van dezelfde doorgegeven met een speciaal geval voor 1.

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| CarsPassed | Time |
| --- | --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyotas |2015-01-01T00:00:10.0000000Z |

**Oplossing**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Uitleg**: de **geval** expressie een expressie aan een set van eenvoudige expressies om te bepalen van het resultaat wordt vergeleken. In dit voorbeeld maakt vehicle met een telling van 1 heeft een andere tekenreeks beschrijving geretourneerd dan vehicle met een telling dan 1 maakt. 

## <a name="query-example-send-data-to-multiple-outputs"></a>Query-voorbeeld: gegevens verzenden naar meerdere uitgangen
**Beschrijving**: gegevens verzenden naar meerdere doelen van de uitvoer van een enkele taak.
Bijvoorbeeld, analyseren van gegevens voor een waarschuwing op basis van drempelwaarden en alle gebeurtenissen naar blob-opslag te archiveren.

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output1**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output2**:

| Maken | Time | Count |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Oplossing**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Uitleg**: de **INTO** component Stream Analytics wordt uitgelegd welke van de uitvoer schrijven van de gegevens uit deze instructie.
Is het doorgeven van de gegevens die zijn ontvangen op een uitvoer die met de naam van de eerste query **ArchiveOutput**.
De tweede query biedt een aantal eenvoudige aggregatie en filteren en deze stuurt de resultaten naar een downstream waarschuwingsmethoden systeem.

Opmerking u kunt ook opnieuw gebruiken de resultaten van de algemene tabelexpressies (CTE's) (zoals **WITH** instructies) in meerdere uitvoer-instructies. Deze optie heeft het voordeel van het openen van de invoerbron minder lezers.
Bijvoorbeeld: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a>Voorbeeld van de query: de unieke waarden tellen
**Beschrijving**: het aantal unieke waarden die worden weergegeven in de stroom binnen een tijdvenster tellen.
Bijvoorbeeld hoeveel unieke wordt doorgegeven aan de stand tolstation in een venster 2 seconden auto's?

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Uitvoer:**

| CountMake | Time |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Oplossing:**

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


**Uitleg:**
**COUNT (afzonderlijke zorg)** retourneert het aantal afzonderlijke waarden in de **zorg** kolom binnen een periode.

## <a name="query-example-determine-if-a-value-has-changed"></a>Query-voorbeeld: bepalen of een waarde is gewijzigd
**Beschrijving**: kijken naar een vorige waarde om te bepalen of anders is dan de huidige waarde.
Bijvoorbeeld, is de vorige auto tolstation onderweg de dezelfde maken als de huidige auto?

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Uitvoer**:

| Maken | Time |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Oplossing**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Uitleg**: Gebruik **LAG** als u wilt bekijken in de invoerstroom één gebeurtenis terug en de **zorg** waarde. Vergelijk deze naar de **zorg** -waarde op de huidige gebeurtenis en de uitvoer van de gebeurtenis als deze verschillen.

## <a name="query-example-find-the-first-event-in-a-window"></a>Voorbeeld van de query: de eerste gebeurtenis niet vinden in een venster
**Beschrijving**: de eerste auto niet vinden in het interval van elke 10 minuten.

**Invoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Oplossing**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Nu gaan we het probleem wijzigen en de eerste auto van een bepaalde merk niet vinden in het interval van elke 10 minuten.

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Oplossing**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-the-last-event-in-a-window"></a>Voorbeeld van de query: de laatste gebeurtenis niet vinden in een venster
**Beschrijving**: de laatste auto niet vinden in het interval van elke 10 minuten.

**Invoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Oplossing**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Uitleg**: Er zijn twee stappen in de query. Het eerste beheerpunt vindt de meest recente tijdstempel in windows 10 minuten. De tweede stap koppelt de resultaten van de eerste query met de oorspronkelijke stroom vinden van de gebeurtenissen die overeenkomen met de laatste tijdstempels in elk venster. 

## <a name="query-example-detect-the-absence-of-events"></a>Voorbeeld van de query: de afwezigheid van gebeurtenissen detecteren
**Beschrijving**: Controleer of er een stroom geen waarde die overeenkomt met een bepaald criterium.
Bijvoorbeeld: hebt 2 opeenvolgende auto's uit hetzelfde merk onderweg tolstation ingevoerd gedurende de laatste 90 seconden

**Invoer**:

| Maken | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF-987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI-345 |2015-01-01T00:00:04.0000000Z |

**Uitvoer**:

| Maken | Time | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC-123 |2015-01-01T00:00:01.0000000Z |

**Oplossing**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Uitleg**: Gebruik **LAG** als u wilt bekijken in de invoerstroom één gebeurtenis terug en de **zorg** waarde. Vergelijk deze met de **zorg** waarde in de huidige gebeurtenis en de gebeurtenis vervolgens uitvoer als ze hetzelfde zijn. U kunt ook **LAG** ophalen van gegevens over de vorige auto.

## <a name="query-example-detect-the-duration-between-events"></a>Voorbeeld van de query: de tijd tussen gebeurtenissen detecteren
**Beschrijving**: de duur van een bepaalde gebeurtenis vinden. Bijvoorbeeld: een web-clickstream gezien, bepalen de tijd die op een functie.

**Invoer**:  

| Gebruiker | Functie | Gebeurtenis | Time |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Starten |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |Einde |2015-01-01T00:00:08.0000000Z |

**Uitvoer**:  

| Gebruiker | Functie | Duur |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Oplossing**:

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Uitleg**: Gebruik de **laatste** functie voor het ophalen van de laatste **tijd** waarde wanneer het gebeurtenistype is **Start**. De **laatste** functie maakt gebruik van **PARTITION BY [gebruiker]** om aan te geven dat het resultaat wordt berekend per unieke gebruiker. De query heeft een 1 uur maximale drempelwaarde voor het tijdsverschil tussen **Start** en **stoppen** gebeurtenissen, maar is configureerbaar naar behoefte **(LIMIET DURATION(hour, 1)**.

## <a name="query-example-detect-the-duration-of-a-condition"></a>Voorbeeld van de query: de duur van een voorwaarde detecteren
**Beschrijving**: vinden out hoe lang een voorwaarde is opgetreden.
Stel bijvoorbeeld dat een fout heeft geresulteerd in alle auto's met een onjuiste gewicht (meer dan 20.000 pond) en de duur van die fout moet worden berekend.

**Invoer**:

| Maken | Time | Gewicht |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Uitvoer**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Oplossing**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Uitleg**: Gebruik **LAG** weergeven van de invoerstroom 24 uur en zoek naar waar u exemplaren **StartFault** en **StopFault** zijn omspannen door het gewicht < 20000.

## <a name="query-example-fill-missing-values"></a>Query-voorbeeld: Vul ontbrekende waarden
**Beschrijving**: voor de stroom van gebeurtenissen met ontbrekende waarden produceren van een stream van gebeurtenissen met regelmatige tussenpozen.
Bijvoorbeeld, een gebeurtenis om de vijf seconden die het meest recent waargenomen gegevenspunt rapporten genereren.

**Invoer**:

| t | waarde |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Uitvoer (eerste 10 rijen)**:

| windowend | lastevent.t | lastevent.Value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Oplossing**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Uitleg**: deze query gebeurtenissen genereert elke vijf seconden en levert de laatste gebeurtenis dat u eerder hebt ontvangen. De [Hopping venster](https://msdn.microsoft.com/library/dn835041.aspx "Hopping venster--Azure Stream Analytics") duur bepaalt hoe ver terug de query ziet er als u wilt zoeken naar de nieuwste gebeurtenis (300 seconden in dit voorbeeld).


## <a name="query-example-correlate-two-event-types-within-the-same-stream"></a>Query-voorbeeld: twee typen gebeurtenissen binnen dezelfde stream correleren
**Beschrijving**: soms waarschuwingen moeten worden gegenereerd op basis van meerdere gebeurtenistypen dat is opgetreden in een bepaalde periode.
Bijvoorbeeld in een IoT-scenario voor thuis weerstaan, moet een waarschuwing worden gegenereerd wanneer de temperatuur ventilator korter dan 40 is en de maximale kracht gedurende de laatste 3 minuten kleiner dan 10 is.

**Invoer**:

| tijd | deviceId | sensorName | waarde |
| --- | --- | --- | --- |
| "2018-01-01T16:01:00" | 'Oven1' | 'temp' |120 |
| "2018-01-01T16:01:00" | 'Oven1' | "power" |15 |
| "2018-01-01T16:02:00" | 'Oven1' | 'temp' |100 |
| "2018-01-01T16:02:00" | 'Oven1' | "power" |15 |
| "2018-01-01T16:03:00" | 'Oven1' | 'temp' |70 |
| "2018-01-01T16:03:00" | 'Oven1' | "power" |15 |
| "2018-01-01T16:04:00" | 'Oven1' | 'temp' |50 |
| "2018-01-01T16:04:00" | 'Oven1' | "power" |15 |
| "2018-01-01T16:05:00" | 'Oven1' | 'temp' |30 |
| "2018-01-01T16:05:00" | 'Oven1' | "power" |8 |
| "2018-01-01T16:06:00" | 'Oven1' | 'temp' |20 |
| "2018-01-01T16:06:00" | 'Oven1' | "power" |8 |
| "2018-01-01T16:07:00" | 'Oven1' | 'temp' |20 |
| "2018-01-01T16:07:00" | 'Oven1' | "power" |8 |
| "2018-01-01T16:08:00" | 'Oven1' | 'temp' |20 |
| "2018-01-01T16:08:00" | 'Oven1' | "power" |8 |

**Uitvoer**:

| eventTime | deviceId | TEMP | alertMessage | maxPowerDuringLast3mins |
| --- | --- | --- | --- | --- | 
| "2018-01-01T16:05:00" | 'Oven1' |30 | 'Kortsluitingsoperator verwarming elementen' |15 |
| "2018-01-01T16:06:00" | 'Oven1' |20 | 'Kortsluitingsoperator verwarming elementen' |15 |
| "2018-01-01T16:07:00" | 'Oven1' |20 | 'Kortsluitingsoperator verwarming elementen' |15 |

**Oplossing**:

````
WITH max_power_during_last_3_mins AS (
    SELECT 
        System.TimeStamp AS windowTime,
        deviceId,
        max(value) as maxPower
    FROM
        input TIMESTAMP BY t
    WHERE 
        sensorName = 'power' 
    GROUP BY 
        deviceId, 
        SlidingWindow(minute, 3) 
)

SELECT 
    t1.t AS eventTime,
    t1.deviceId, 
    t1.value AS temp,
    'Short circuit heating elements' as alertMessage,
    t2.maxPower AS maxPowerDuringLast3mins
    
INTO resultsr

FROM input t1 TIMESTAMP BY t
JOIN max_power_during_last_3_mins t2
    ON t1.deviceId = t2.deviceId 
    AND t1.t = t2.windowTime
    AND DATEDIFF(minute,t1,t2) between 0 and 3
    
WHERE
    t1.sensorName = 'temp'
    AND t1.value <= 40
    AND t2.maxPower > 10
````

**Uitleg**: de eerste query `max_power_during_last_3_mins`, gebruikt de [schuifregelaar venster](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics) naar de maximale waarde van de sensor power voor elk apparaat tijdens de afgelopen 3 minuten. De tweede query is gekoppeld aan de eerste query om de energie-waarde in het venster van de meest recente relevant vinden voor de huidige gebeurtenis. En vervolgens, mits de voorwaarden wordt voldaan, wordt een waarschuwing gegenereerd voor het apparaat.

## <a name="query-example-process-events-independent-of-device-clock-skew-substreams"></a>Query-voorbeeld: gebeurtenissen onafhankelijk van het apparaat klok leiden tot onjuiste (substreams)
**Beschrijving**: gebeurtenissen kunnen laat plaatsvinden of volgorde vanwege klok laat tussen producenten gebeurtenis de klok die zijn hellen tussen partities of netwerklatentie. In het volgende voorbeeld wordt de apparaatklok voor TollID 2 is tien seconden achter TollID 1 en de apparaatklok voor TollID 3 is vijf seconden achter TollID 1. 


**Invoer**:
| LicensePlate | Maken | Time | TollID |
| --- | --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:01.0000000Z | 1 |
| YHN 6970 |Toyota |2015-07-27T00:00:05.0000000Z | 1 |
| QYF 9358 |Honda |2015-07-27T00:00:01.0000000Z | 2 |
| GXF 9462 |BMW |2015-07-27T00:00:04.0000000Z | 2 |
| VFE 1616 |Toyota |2015-07-27T00:00:10.0000000Z | 1 |
| RMV 8282 |Honda |2015-07-27T00:00:03.0000000Z | 3 |
| MDR 6128 |BMW |2015-07-27T00:00:11.0000000Z | 2 |
| YZK 5704 |Ford |2015-07-27T00:00:07.0000000Z | 3 |

**Uitvoer**:
| TollID | Count |
| --- | --- |
| 1 | 2 |
| 2 | 2 |
| 1 | 1 |
| 3 | 1 |
| 2 | 1 |
| 3 | 1 |

**Oplossing**:

````
SELECT
      TollId,
      COUNT(*) AS Count
FROM input
      TIMESTAMP BY Time OVER TollId
GROUP BY TUMBLINGWINDOW(second, 5), TollId

````

**Uitleg**: de [TIMESTAMP-door via](https://msdn.microsoft.com/en-us/azure/stream-analytics/reference/timestamp-by-azure-stream-analytics#over-clause-interacts-with-event-ordering) component kijkt naar de tijdlijn van elk apparaat afzonderlijk met substreams. De uitvoergebeurtenissen voor elke TollID zijn gegenereerd als ze worden berekend, wat betekent dat de gebeurtenissen in volgorde ten opzichte van elke TollID in plaats van de volgorde van worden alsof alle apparaten op de dezelfde klok wordt gewijzigd.


## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

