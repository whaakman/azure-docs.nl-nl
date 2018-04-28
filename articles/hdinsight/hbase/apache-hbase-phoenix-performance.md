---
title: Phoenix prestaties in Azure HDInsight | Microsoft Docs
description: Aanbevolen procedures om Phoenix prestaties te optimaliseren.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 58ecf22fa0f9349a767455fe3ab08fca058d02da
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="phoenix-performance-best-practices"></a>Best practices voor Phoenix-prestatie

De belangrijkste aspecten van Phoenix prestaties is het optimaliseren van de onderliggende HBase. Phoenix maakt een model relationele gegevens op HBase die SQL-query's in HBase-bewerkingen, zoals scans omzet. Het ontwerp van uw tabelschema, de selectie en ordening van de velden in de primaire sleutel en het gebruik van alle indexen Phoenix prestaties beïnvloeden.

## <a name="table-schema-design"></a>Tabelontwerp schema

Wanneer u een tabel in Phoenix maakt, wordt deze tabel wordt opgeslagen in een HBase-tabel. De HBase-tabel bevat de groepen van de kolommen (kolomfamilies) die samen worden gebruikt. Een rij in de tabel Phoenix is een rij in de HBase-tabel, waarbij elke rij van samengestelde cellen die zijn gekoppeld aan een of meer kolommen bestaat. Logisch, is één rij HBase een verzameling van sleutel-waardeparen, elk met dezelfde rowkey waarde. Dat wil zeggen, elke sleutel / waarde-paar heeft het kenmerk rowkey en de waarde van dit kenmerk rowkey is hetzelfde voor een bepaalde rij.

Het schemaontwerp van een tabel Phoenix bevat de primaire sleutel ontwerp, familie kolomontwerp afzonderlijke kolomontwerp en hoe de gegevens is gepartitioneerd.

### <a name="primary-key-design"></a>Primaire sleutel ontwerp

De primaire sleutel gedefinieerd voor een tabel in Phoenix bepaalt hoe gegevens worden opgeslagen in de rowkey van de onderliggende HBase-tabel. In HBase is de enige manier om toegang tot een bepaalde rij met de rowkey. Bovendien is gegevens die zijn opgeslagen in een HBase-tabel gesorteerd op de rowkey. Phoenix bouwt de waarde rowkey door de waarden van elk van de kolommen in de rij in de volgorde waarin die ze zijn gedefinieerd in de primaire sleutel samen te voegen.

Een tabel voor contactpersonen heeft bijvoorbeeld de voornaam, laatste naam, telefoonnummer en adres, allemaal in dezelfde kolomfamilie. U kunt een primaire sleutel op basis van een toenemende volgnummer definiëren:

|rowkey|       Adres|   Telefoon| Voornaam| lastName|
|------|--------------------|--------------|-------------|--------------|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    Jan|Dole|
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji|

Als u vaak een query door lastName kan deze primaire sleutel echter niet uitvoeren, omdat elke query een volledige tabelscan lezen van de waarde van elke achternaam is vereist. In plaats daarvan kunt u een primaire sleutel op de achternaam, firstName en sociaal-fiscaal nummer kolommen definiëren. Deze laatste kolom is twee inwoners op hetzelfde adres met dezelfde naam, zoals een vader en zoon toe.

|rowkey|       Adres|   Telefoon| Voornaam| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    Jan|Dole| 111 |
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Met deze nieuwe primaire sleutel van de rij zou sleutels die zijn gegenereerd door Phoenix zijn:

|rowkey|       Adres|   Telefoon| Voornaam| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole-John-111|1111 San Gabriel Dr.|1-425-000-0002|    Jan|Dole| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

De gegevens voor de rowkey wordt in de eerste rij hierboven weergegeven zoals wordt weergegeven:

|rowkey|       sleutel|   waarde| 
|------|--------------------|---|
|  Dole-John-111|Adres |1111 San Gabriel Dr.|  
|  Dole-John-111|Telefoon |1-425-000-0002|  
|  Dole-John-111|Voornaam |Jan|  
|  Dole-John-111|lastName |Dole|  
|  Dole-John-111|socialSecurityNum |111| 

Deze rowkey slaat nu een kopie van de gegevens. Houd rekening met de grootte en het aantal kolommen worden opgenomen in de primaire sleutel, omdat deze waarde opgenomen in elke cel in de onderliggende HBase-tabel is.

Als de primaire sleutel waarden die zijn gestaag toenemende heeft, moet u ook de tabel met maken *salt buckets* te voorkomen maakt schrijven hotspots-Zie [gegevens partitie](#partition-data).

### <a name="column-family-design"></a>Kolom familie ontwerp

Als sommige kolommen vaker dan andere toegankelijk zijn, moet u meerdere kolomfamilies afzonderlijke de veelgebruikte kolommen uit zelden gebruikte kolommen maken.

Ook als bepaalde kolommen vaak samen worden geopend, plaatsen die kolommen in dezelfde kolomfamilie.

### <a name="column-design"></a>Kolomontwerp

* Houd VARCHAR kolommen onder ongeveer 1 MB vanwege de i/o-kosten van grote kolommen. Bij het verwerken van query's, HBase gebeurtenis zich voordoet cellen in de volledige voordat ze worden verzonden via de client en de client ontvangt deze volledig voordat het afleveren aan de toepassingscode.
* Kolomwaarden met een gecomprimeerde indeling zoals protobuf, Avro, msgpack of BSON opslaan. JSON wordt niet aanbevolen, omdat het groter is.
* Houd rekening met het comprimeren van gegevens voordat u opslag knippen latentie en i/o-kosten.

### <a name="partition-data"></a>Partitiegegevens

Phoenix kunt u bepalen het aantal regio's waar uw gegevens is gedistribueerd, die u kunt lezen/schrijven-prestaties aanzienlijk verbeteren. Bij het maken van een tabel Phoenix, kunt u salt of uw gegevens vooraf te splitsen.

Als u wilt een tabel salt tijdens het maken, geef het aantal buckets salt:

    CREATE TABLE CONTACTS (...) SALT_BUCKETS = 16

Deze zouten splitst in de tabel langs de waarden van de primaire sleutels, de waarden automatisch kiezen. 

Om te bepalen waar de tabel splitsingen worden uitgevoerd, kunt u vooraf de tabel aan de hand van de waarden voor het waarlangs het splitsen van deze gebeurtenis treedt op te splitsen. Bijvoorbeeld, splitsen als u wilt maken van een tabel langs drie gebieden:

    CREATE TABLE CONTACTS (...) SPLIT ON ('CS','EU','NA')

## <a name="index-design"></a>Index ontwerpen

Een index Phoenix is een HBase-tabel die een kopie van sommige of alle van de gegevens van de tabel opslaat. Een index verbetert de prestaties voor specifieke typen query's.

Wanneer u meerdere indexen die zijn gedefinieerd en een querytabel, geselecteerd Phoenix automatisch de aanbevolen index voor de query. De primaire index is gemaakt op basis van de primaire sleutels die u selecteert automatisch.

U kunt ook secundaire indexen door te geven van de kolommen voor verwachte query's maken.

Bij het ontwerpen van uw indexen:

* Alleen de indexen die u moet maken.
* Beperk het aantal indexen op tabellen regelmatig wordt bijgewerkt. Updates aan een tabel vertalen naar schrijfbewerkingen naar zowel de hoofdtabel en de index-tabellen.

## <a name="create-secondary-indexes"></a>Secundaire indexen maken

Secundaire indexen kunnen lezen prestaties verbeteren door het inschakelen van wat zou worden van een volledige tabelscan in een punt-lookup koste opslagruimte en snelheid schrijven. Secundaire indexen kunnen worden toegevoegd of verwijderd na het maken van de tabel en wijzigingen in bestaande query's niet vereist – query's alleen sneller worden uitgevoerd. Overweeg te maken van gedekte indexen, functionele indexen of beide, afhankelijk van uw behoeften.

### <a name="use-covered-indexes"></a>Gebruik waarvoor indexen

Gedekte indexen zijn indexen die gegevens bevatten uit de rij naast de waarden die zijn geïndexeerd. Als er op de gewenste indexvermelding, is er niet nodig voor toegang tot de primaire tabel.

Bijvoorbeeld, in het voorbeeld tabel kunt u een secundaire index maken voor alleen de kolom socialSecurityNum contactpersoon. Deze secundaire index query's die worden gefilterd op waarden socialSecurityNum zou versnellen, maar bij het ophalen van andere veldwaarden is vereist een andere tegen de hoofdtabel lezen.

|rowkey|       Adres|   Telefoon| Voornaam| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole-John-111|1111 San Gabriel Dr.|1-425-000-0002|    Jan|Dole| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Als u doorgaans wilt de voornaam en achternaam die zijn opgegeven van de socialSecurityNum opzoeken, kan u echter een gedekte index die de voornaam en achternaam als de werkelijke gegevens in de indextabel bevat maken:

    CREATE INDEX ssn_idx ON CONTACTS (socialSecurityNum) INCLUDE(firstName, lastName);

De volgende query voor het verkrijgen van alle gegevens door te lezen van de tabel met de secundaire index maakt gebruik van deze gedekte index:

    SELECT socialSecurityNum, firstName, lastName FROM CONTACTS WHERE socialSecurityNum > 100;

### <a name="use-functional-indexes"></a>Functionele indexen gebruiken

Functionele indexen kunnen u een index maken op een willekeurige expressie die u verwacht te worden gebruikt in query's. Als u beschikken over een functionele index en een query die expressie gebruikt, kan de index kan worden gebruikt voor het ophalen van de resultaten in plaats van de tabel.

U kunt bijvoorbeeld een index zodat u kunt niet-hoofdlettergevoelige zoekopdrachten op de gecombineerde voornaam en achternaam van een persoon uitvoeren maken:

     CREATE INDEX FULLNAME_UPPER_IDX ON "Contacts" (UPPER("firstName"||' '||"lastName"));

## <a name="query-design"></a>Queryontwerp

De belangrijkste overwegingen in queryontwerp zijn:

* Het queryplan begrijpen en controleer of het verwachte gedrag.
* Voeg efficiënt.

### <a name="understand-the-query-plan"></a>Het queryplan begrijpen

In [SQLLine](http://sqlline.sourceforge.net/), uitleg gevolgd door uw SQL-query te gebruiken om het plan van bewerkingen die Phoenix voert weer te geven. Controleer of het plan:

* Maakt gebruik van uw primaire sleutel, indien van toepassing.
* Maakt gebruik van passende secundaire indexen in plaats van de tabel.
* Maakt gebruik van bereik SCANNEN of SCAN OVERSLAAN waar mogelijk, in plaats van tabel SCANNEN.

#### <a name="plan-examples"></a>Voorbeelden plannen

Als u bijvoorbeeld dat u hebt een tabel VLUCHT waarmee vluchtgegevens vertraging worden opgeslagen.

Selecteer de vlucht met een airlineid van `19805`, waarbij airlineid een veld dat niet in de primaire sleutel of in een index is:

    select * from "FLIGHTS" where airlineid = '19805';

Voer de uitleg-opdracht als volgt:

    explain select * from "FLIGHTS" where airlineid = '19805';

Het queryplan ziet er als volgt:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN FULL SCAN OVER FLIGHTS
        SERVER FILTER BY AIRLINEID = '19805'

Noteer de woordgroep volledige SCAN via VLUCHT in dit plan. Deze zin Hiermee geeft u dat de uitvoering wordt het SCANNEN van een tabel via alle rijen in de tabel in plaats van met de optie voor het bereik SCANNEN of SCAN OVERSLAAN efficiënter.

Nu dat u wilt zoeken naar vlucht op 2 januari 2014 voor de provider `AA` waarop de flightnum is groter dan 1. Stel de kolommen jaar, maand, dayofmonth, carrier en flightnum in de voorbeeldtabel bestaat en allemaal deel van de samengestelde primaire sleutel uitmaken. De query zou er als volgt uitzien:

    select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Het plan voor deze query met behandeld:

    explain select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Het resulterende plan is:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER FLIGHTS [2014,1,2,'AA',2] - [2014,1,2,'AA',*]

De waarden tussen vierkante haken zijn het bereik van waarden voor de primaire sleutels. In dit geval de waarden voor het worden opgelost met 2014 jaar, maand 1 en 2 dayofmonth, maar waarden flightnum starten met 2 en klik op toestaan (`*`). Deze queryplan bevestigt dat de primaire sleutel wordt gebruikt zoals verwacht.

Maak vervolgens een index voor de VLUCHT tabel met de naam `carrier2_idx` dat zich op het veld carrier alleen. Deze index bevat ook flightdate, tailnum oorsprong en flightnum als gedekte kolommen waarvan gegevens ook in de index opgeslagen worden.

    CREATE INDEX carrier2_idx ON FLIGHTS (carrier) INCLUDE(FLIGHTDATE,TAILNUM,ORIGIN,FLIGHTNUM);

Dat u wilt ophalen van de provider samen met de flightdate en tailnum, zoals in de volgende query:

    explain select carrier,flightdate,tailnum from "FLIGHTS" where carrier = 'AA';

Hier ziet u deze index wordt gebruikt:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER CARRIER2_IDX ['AA']

Voor een volledig overzicht van de items die kunnen worden weergegeven in de resultaten plan wordt uitgelegd, Zie de sectie wordt uitgelegd plannen in de [Apache Phoenix afstemmen handleiding](https://phoenix.apache.org/tuning_guide.html).

### <a name="join-efficiently"></a>Efficiënt koppelen

U wilt over het algemeen joins voorkomen, tenzij een-zijde klein, met name op vaak query's is.

Als nodig is, u grote joins met doen kunt de `/*+ USE_SORT_MERGE_JOIN */` hint, maar een grote join is een dure bewerking via een groot aantal rijen. Als de totale grootte van alle rechts naast tabellen zou groter zijn dan het beschikbare geheugen, gebruikt u de `/*+ NO_STAR_JOIN */` hint.

## <a name="scenarios"></a>Scenario's

De volgende richtlijnen worden enkele algemene patronen beschreven.

### <a name="read-heavy-workloads"></a>Lees zware workloads

Voor lezen zware gebruiksvoorbeelden, controleert u of dat u gebruikmaakt van indexen. Lees-overhead tijd besparen Let gedekte indexen te maken.

### <a name="write-heavy-workloads"></a>Schrijven zware workloads

Maak voor schrijven zware workloads waarbij de primaire sleutel gestaag toeneemt, salt buckets om te voorkomen schrijven hotspots ten koste van de totale doorvoer van gelezen vanwege de extra scans nodig. Ook als u UPSERT voor het schrijven van een groot aantal records, Schakel automatische doorvoer en batch van de records uit.

### <a name="bulk-deletes"></a>Bulksgewijs verwijderen

Als een grote gegevensset te verwijderen, schakelt automatische doorvoer alvorens de query verwijderen zodat de client niet hoeft te herinneren van de rij-sleutels voor alle verwijderde rijen. Automatische doorvoer wordt voorkomen dat de client het bufferen van de rijen zijn beïnvloed door het verwijderen zodat deze Phoenix u deze rechtstreeks op de servers regio, zonder de kosten verwijderen kunt van de terug te sturen naar de client.

### <a name="immutable-and-append-only"></a>Niet-wijzigbaar en alleen toevoegen

Als uw scenario meer gericht op schrijven snelheid dan de integriteit van gegevens, kunt u het logboek schrijven-ahead uitschakelen bij het maken van uw tabellen:

    CREATE TABLE CONTACTS (...) DISABLE_WAL=true;

Zie voor meer informatie over deze en andere opties [Phoenix grammatica](http://phoenix.apache.org/language/index.html#options).

## <a name="next-steps"></a>Volgende stappen

* [Phoenix afstemmen handleiding](https://phoenix.apache.org/tuning_guide.html)
* [Secundaire indexen](http://phoenix.apache.org/secondary_indexing.html)
