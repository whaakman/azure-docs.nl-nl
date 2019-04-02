---
title: Met Geofencing en georuimtelijke aggregatie-scenario's met Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics gebruikt voor geofencing en georuimtelijke aggregatie.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: cc301855e4cdcb8eb687e753835577399cfe72b0
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58806960"
---
# <a name="geofencing-and-geospatial-aggregation-scenarios-with-azure-stream-analytics"></a>Met Geofencing en georuimtelijke aggregatie-scenario's met Azure Stream Analytics

Met ingebouwde georuimtelijke functies kunt u toepassingen bouwen voor scenario's zoals apparatenpark, delen, verbonden auto's en bijhouden van Assets rijden Azure Stream Analytics.

## <a name="geofencing"></a>Geofencing

Azure Stream Analytics biedt ondersteuning voor lage latentie realtime geofencing berekeningen in de cloud en op IoT Edge-runtime.

### <a name="geofencing-scenario"></a>Met Geofencing scenario

Een productiebedrijf moet bijhouden van assets op hun gebouwen. Ze voorzien van elk apparaat met een GPS en meldingen wilt ontvangen als een apparaat wordt verwijderd uit een bepaald gebied.

Referentiegegevens die in dit voorbeeld bevat de geofence-informatie voor de gebouwen en de apparaten die zijn toegestaan in elk van de gebouwen. Houd er rekening mee dat referentiegegevens een statisch of traag wijzigen kunnen zou. Statische referentiegegevens wordt gebruikt voor dit scenario. Een stroom van gegevens verzendt continu de apparaat-ID en de huidige positie.

### <a name="define-geofences-in-reference-data"></a>Geofences in referentiegegevens definiëren

Een geofence kan worden gedefinieerd met behulp van een GeoJSON-object. Voor taken met compatibiliteitsversie 1.2 of hoger, geofences kunnen ook worden gedefinieerd met behulp van Well Known Text invoer (WELL-KNOWN) als `NVARCHAR(MAX)`. WELL-KNOWN is een Open georuimtelijke Consortium (OGC) standaard die wordt gebruikt voor ruimtelijke gegevens in een tekstindeling.

De ingebouwde georuimtelijke functies kunnen gedefinieerde geofences gebruiken om erachter te komen of een element in of uit een specifieke geofence veelhoek is.

De volgende tabel is een voorbeeld van de geofence verwijzen naar gegevens die kunnen worden opgeslagen in Azure blob-opslag of een Azure SQL-tabel. Elke site wordt vertegenwoordigd door een veelhoek met georuimtelijke en elk apparaat is gekoppeld aan een toegestane site-ID.

|SiteID|SiteName|Geofence|AllowedDeviceID|
|------|--------|--------|---------------|
|1|"Redmond Building 41"|"VEELHOEK ((-122.1337357922017 47.63782998329432,-122.13373042778369 47.637634793257305,-122.13346757130023 47.637642022530954,-122.13348902897235 47.637508280806806,-122.13361777500506 47.637508280806806,-122.13361241058703 47.63732393354484,-122.13265754417773 47.63730947490855,-122.13266290859576 47.637519124743164,-122.13302232460376 47.637515510097955,-122.13301696018573 47.63764925180358,-122.13272728161212 47.63764925180358,-122.13274873928424 47.63784082716388,-122.13373579220172 47.63782998329432)) "|"B"|
|2|"Redmond Building 40"|"VEELHOEK ((-122.1336154507967 47.6366745947009,-122.13361008637867 47.636483015064535,-122.13349206918201 47.636479400347675,-122.13349743360004 47.63636372927573,-122.13372810357532 47.63636372927573,-122.13373346799335 47.63617576323771,-122.13263912671528 47.63616491902258,-122.13264985555134 47.63635649982525,-122.13304682248554 47.636367344000604,-122.13305218690357 47.63650831807564,-122.13276250832996 47.636497473929516,-122.13277323716602 47.63668543881025,-122.1336154507967 47.6366745947009)) "|"A"|
|3|"Redmond Building 22"|"VEELHOEK ((-122.13611660248233 47.63758544698554,-122.13635263687564 47.6374083293018,-122.13622389084293 47.63733603619712,-122.13622389084293 47.63717699101473,-122.13581619507266 47.63692757827657,-122.13559625393344 47.637046862778135,-122.13569281345798 47.637144458985965,-122.13570890671207 47.637314348246214,-122.13611660248233 47.63758544698554)) "|"C"|

### <a name="generate-alerts-with-geofence"></a>Genereer waarschuwingen met de geofence bevindt

Apparaten kunnen verzenden hun-ID en de locatie van elke minuut tot en met een stroom met de naam `DeviceStreamInput`. De volgende tabel is een stream van invoer.

|Apparaat-id|GeoPosition|
|--------|-----------|
|"A"|"POINT(-122.13292341559497 47.636318374032726)"|
|"B"|"POINT(-122.13338475554553 47.63743531308874)"|
|"C"|"POINT(-122.13354001095752 47.63627622505007)"|

U kunt een query die aan de apparaat-stream met de referentiegegevens geofence wordt toegevoegd en wordt een waarschuwing gegenereerd telkens wanneer een apparaat buiten een toegestane gebouw wordt schrijven.

```SQL
SELECT DeviceStreamInput.DeviceID, SiteReferenceInput.SiteID, SiteReferenceInput.SiteName 
INTO Output
FROM DeviceStreamInput 
JOIN SiteReferenceInput
ON st_within(DeviceStreamInput.GeoPosition, SiteReferenceInput.Geofence) = 0
WHERE DeviceStreamInput.DeviceID = SiteReferenceInput.AllowedDeviceID
```

De volgende afbeelding geeft de geofences. U kunt zien waar de apparaten zijn in overeenstemming met de streaming-gegevens invoeren.

![Building geofences](./media/geospatial-scenarios/building-geofences.png)

Apparaat 'C' bevindt zich in het gebouw-ID 2, wat niet is toegestaan op basis van de referentiegegevens. Dit apparaat moet zich bevinden in het bouwen van 3-ID. Deze taak wordt een waarschuwing gegenereerd voor deze specifieke fout.

### <a name="site-with-multiple-allowed-devices"></a>Site met meerdere toegestane apparaten

Als een site kunt u meerdere apparaten, een matrix van apparaat-id's kan worden gedefinieerd in `AllowedDeviceID` en een door de gebruiker gedefinieerde functie kan worden gebruikt op de `WHERE` component om te controleren of de stream-apparaat-ID overeenkomt met een apparaat-ID in die lijst. Bekijk voor meer informatie de [Javascript-UDF](stream-analytics-javascript-user-defined-functions.md) zelfstudie voor cloudtaken en de [ C# UDF](stream-analytics-edge-csharp-udf.md) zelfstudie voor het edge-taken.

## <a name="geospatial-aggregation"></a>Georuimtelijke aggregatie

Azure Stream Analytics biedt ondersteuning voor lage latentie realtime georuimtelijke aggregatie in de cloud en op IoT Edge-runtime.

### <a name="geospatial-aggregation-scenario"></a>Georuimtelijke aggregatie scenario

Een cab-bedrijf wil een realtime toepassing de CAB-bestand stuurprogramma's die op zoek naar rijpositie naar de gebieden van de steden momenteel een grotere vraag voor het bouwen.

Het bedrijf worden logische gebieden van de plaats opgeslagen als referentiegegevens. Elke regio wordt gedefinieerd door een RegionID, RegionName en geofence bevindt.

### <a name="define-the-geofences"></a>De geofences definiëren

De volgende tabel is een voorbeeld van de geofence verwijzen naar gegevens die kunnen worden opgeslagen in Azure blob-opslag of een Azure SQL-tabel. Elke regio wordt vertegenwoordigd door een veelhoek georuimtelijke, die wordt gebruikt voor het correleren met de aanvragen die afkomstig zijn van het streamen van gegevens.

Deze veelhoeken zijn alleen ter informatie en geen daadwerkelijke plaats logische of fysieke kleurscheidingen vertegenwoordigen.

|RegionID|RegionName|Geofence|
|--------|----------|--------|
|1|"SoHo"|"VEELHOEK ((-74.00279525078275 40.72833625216264,-74.00547745979765 40.721929158663244,-74.00125029839018 40.71893680218994,-73.9957785919998 40.72521409075776,-73.9972377137039 40.72557184584898,-74.00279525078275 40.72833625216264) )"|
|2|"Chinatown"|"VEELHOEK ((-73.99712367114876 40.71281582267133,-73.9901070123658 40.71336881907936,-73.99023575839851 40.71452359088633,-73.98976368961189 40.71554823078944,-73.99551434573982 40.717337246783735,-73.99480624255989 40.718491949759304,-73.99652285632942 40.719109951574,-73.99776740131233 40.7168005470334,-73.99903340396736 40.71727219249899,-74.00193018970344 40.71938642421256,-74.00409741458748 40.71688186545551,-74.00051398334358 40.71517415773184,-74.0004281526551 40.714377212470005,-73.99849696216438 40.713450141693166,-73.99748845157478 40.71405192594819,-73.99712367114876 40.71281582267133)) "|
|3|"Tribeca"|"VEELHOEK ((-74.01091641815208 40.72583120006787,-74.01338405044578 40.71436586362705,-74.01370591552757 40.713617702123415,-74.00862044723533 40.711308107057235,-74.00194711120628 40.7194238654018,-74.01091641815208 40.72583120006787))"|

### <a name="aggregate-data-over-a-window-of-time"></a>Cumulatieve gegevens gedurende een periode

De volgende tabel bevat de streaminggegevens van "ritjes."

|Gebruikers-id|FromLocation|ToLocation|TripRequestedTime|
|------|------------|----------|-----------------|
|"A"|"POINT(-74.00726861389182 40.71610611981975)"|"POINT(-73.98615095917779 40.703107386025835)"|"2019-03-12T07:00:00Z"|
|"B"|"POINT(-74.00249841021645 40.723827238895666)"|"POINT(-74.01160699942085 40.71378884930115)"|"2019-03-12T07:01:00Z"|
|"C"|"POINT(-73.99680120565864 40.716439898624024)"|"POINT(-73.98289663412544 40.72582343969828)"|"2019-03-12T07:02:00Z"|
|"D"|"POINT(-74.00741090068288 40.71615626755086)"|"POINT(-73.97999843120539 40.73477895807408)"|"2019-03-12T07:03:00Z"|

De volgende query lid wordt van het apparaat streamen met de referentiegegevens geofence bevindt en het aantal aanvragen per regio op een bepaalde periode van 15 minuten per minuut berekend.

```SQL
SELECT count(*) as NumberOfRequests, RegionsRefDataInput.RegionName 
FROM UserRequestStreamDataInput
JOIN RegionsRefDataInput 
ON st_within(UserRequestStreamDataInput.FromLocation, RegionsRefDataInput.Geofence) = 1
GROUP BY RegionsRefDataInput.RegionName, hoppingwindow(minute, 1, 15)
```

Deze query voert een aantal aanvragen per minuut voor de laatste 15 minuten door elke regio in de plaats. Deze informatie kan eenvoudig worden weergegeven door Power BI-dashboard of aan alle stuurprogramma's kan worden uitgezonden als SMS-berichten door de integratie met services zoals Azure functions.

De onderstaande afbeelding ziet de uitvoer van de query voor het Power BI-dashboard. 

![Resultaat uitvoer op Power BI-dashboard](./media/geospatial-scenarios/power-bi-output.png)


## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Stream Analytics georuimtelijke functies](stream-analytics-geospatial-functions.md)
* [Georuimtelijke functies (Azure Stream Analytics)](https://docs.microsoft.com/stream-analytics-query/geospatial-functions)
