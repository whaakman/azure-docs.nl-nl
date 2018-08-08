---
title: Phoenix-prestaties in Azure HDInsight
description: Aanbevolen procedures om Phoenix prestaties te optimaliseren.
services: hdinsight
author: ashishthaps
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: db00dcad8f3dffbb958158bef9fdb75423eba2f7
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592268"
---
# <a name="phoenix-performance-best-practices"></a>Best practices voor Phoenix-prestatie

De belangrijkste aspecten van Phoenix prestaties is het optimaliseren van de onderliggende HBase. Phoenix maakt een relationeel gegevensmodel op HBase die SQL-query's in HBase-bewerkingen, zoals scans converteert. Het ontwerp van uw tabelschema, de selectie en volgorde van de velden in uw primaire sleutel en uw gebruik van alle indexen Phoenix prestaties beïnvloeden.

## <a name="table-schema-design"></a>Schema tabelontwerp

Wanneer u een tabel in Phoenix maakt, wordt die tabel opgeslagen in een HBase-tabel. De HBase-tabel bevat de groepen van kolommen (kolomfamilies) die samen worden gebruikt. Een rij in de Phoenix-tabel is een rij in de HBase-tabel, waarbij elke rij uit versioned cellen die zijn gekoppeld aan een of meer kolommen bestaat. Logisch, is één rij HBase een verzameling van sleutel / waarde-paren, die elk die dezelfde waarde hebben rowkey. Dat wil zeggen, elke sleutel / waarde-paar heeft een rowkey-kenmerk en de waarde van dit kenmerk rowkey is hetzelfde voor een bepaalde rij.

Het schemaontwerp van een tabel Phoenix omvat de primaire sleutel van ontwerp, kolom-familie ontwerp, afzonderlijke kolom in ontwerp en hoe de gegevens zijn gepartitioneerd.

### <a name="primary-key-design"></a>Primaire sleutel ontwerp

De primaire sleutel gedefinieerd voor een tabel in Phoenix bepaalt hoe gegevens worden opgeslagen in de rowkey van de onderliggende HBase-tabel. In HBase is de enige manier om toegang tot een bepaalde rij met de rowkey. Bovendien is gegevens die zijn opgeslagen in een HBase-tabel gesorteerd op de rowkey. Phoenix bouwt de rowkey-waarde door het samenvoegen van de waarden van elk van de kolommen in de rij in de volgorde waarin die ze zijn gedefinieerd in de primaire sleutel.

Een tabel met contacten heeft bijvoorbeeld de voornaam, laatste naam, telefoonnummer, en het adres, allemaal in dezelfde kolomfamilie. U kunt een primaire sleutel op basis van een toenemende volgnummer definiëren:

|rowkey|       Adres|   telefoon| Voornaam| lastName|
|------|--------------------|--------------|-------------|--------------|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    John|Davids|
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji|

Echter, als u regelmatig query's met lastName uitvoeren deze primaire sleutel kan niet uitvoeren, omdat voor elke query is vereist voor een volledige tabelcontrole uitgevoerd om te lezen van de waarde van elke lastName. In plaats daarvan kunt u een primaire sleutel op de achternaam, firstName en sociaal-fiscaal nummer kolommen. Deze laatste kolom is te onderscheiden van inwoners van de twee op hetzelfde adres met dezelfde naam, zoals een vader en zoon.

|rowkey|       Adres|   telefoon| Voornaam| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    John|Davids| 111 |
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Met deze nieuwe primaire sleutel van de rij zou sleutels die worden gegenereerd door Phoenix zijn:

|rowkey|       Adres|   telefoon| Voornaam| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Davids-John-111|1111 San Gabriel Dr.|1-425-000-0002|    John|Davids| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

In de eerste rij boven, worden de gegevens voor de rowkey wordt weergegeven zoals wordt weergegeven:

|rowkey|       sleutel|   waarde| 
|------|--------------------|---|
|  Davids-John-111|Adres |1111 San Gabriel Dr.|  
|  Davids-John-111|telefoon |1-425-000-0002|  
|  Davids-John-111|Voornaam |John|  
|  Davids-John-111|lastName |Davids|  
|  Davids-John-111|socialSecurityNum |111| 

Deze rowkey slaat nu een kopie van de gegevens. Houd rekening met de grootte en het aantal kolommen worden opgenomen in uw primaire sleutel, omdat deze waarde opgenomen in elke cel in de onderliggende HBase-tabel is.

Als de primaire sleutel waarden die gestaag toenemende heeft zijn, moet u ook de tabel met maken *salt buckets* te voorkomen dat u het maken van schrijven hotspots-Zie [partitiegegevens](#partition-data).

### <a name="column-family-design"></a>Kolom-familie ontwerp

Als sommige kolommen vaker dan de andere worden gebruikt, moet u meerdere kolomfamilies voor het scheiden van de veelgebruikte kolommen uit zelden gebruikte kolommen maken.

Ook als bepaalde kolommen vaak samen worden geopend, plaatst u deze kolommen in dezelfde kolomfamilie.

### <a name="column-design"></a>Kolomontwerp

* Houd VARCHAR-kolommen in ongeveer 1 MB vanwege de i/o-kosten voor grote kolommen. Bij het verwerken van query's, HBase gebeurtenis zich voordoet cellen in het volledige voordat ze worden verzonden via de client en de client ontvangt ze volledig voordat het afleveren in de toepassingscode.
* Kolomwaarden met behulp van een compacte indeling, zoals protobuf, Avro, msgpack of BSON Store. JSON wordt niet aanbevolen, omdat het groter is.
* Houd rekening met het comprimeren van gegevens voordat u opslag aan knip-i/o-kosten en latentie.

### <a name="partition-data"></a>Partitiegegevens

Phoenix kunt u voor het beheren van het aantal regio's waar uw gegevens worden gedistribueerd, welke kunt lezen/schrijven-prestaties aanzienlijk verbeteren. Bij het maken van een tabel Phoenix, kunt u salt of uw gegevens vooraf te splitsen.

Als u wilt een tabel tijdens het maken van salt, geef het aantal buckets salt:

    CREATE TABLE CONTACTS (...) SALT_BUCKETS = 16

Deze zouten wordt gesplitst in de tabel langs de waarden van primaire sleutels, automatisch de waarden te kiezen. 

Om te bepalen waar de splitsingen tabel voorkomen, kunt u vooraf in de tabel splitsen door op te geven van de waarden langs die het splitsen van optreedt. Bijvoorbeeld, splitsen voor het maken van een tabel langs drie regio's:

    CREATE TABLE CONTACTS (...) SPLIT ON ('CS','EU','NA')

## <a name="index-design"></a>Indexontwerp

Een index Phoenix is een HBase-tabel waarin een kopie van sommige of alle van de gegevens van de tabel. Een index verbetert de prestaties voor specifieke typen query's.

Wanneer u meerdere indexen die zijn gedefinieerd en vervolgens query uitvoeren op een tabel, selecteert Phoenix automatisch de beste index voor de query. De primaire index is gemaakt op basis van de primaire sleutels die u selecteert automatisch.

Voor verwachte query's, kunt u ook secundaire indexen maken door de kolommen op te geven.

Bij het ontwerpen van uw indexen:

* Alleen de indexen die u moet maken.
* Beperk het aantal indexen op tabellen regelmatig wordt bijgewerkt. Updates voor een tabel vertalen naar schrijfbewerkingen naar zowel de hoofdtabel en de indextabellen.

## <a name="create-secondary-indexes"></a>Secundaire indexen maken

Secundaire indexen kunnen lezen prestaties verbeteren door in te schakelen, wat zou worden van een volledige tabelcontrole uitgevoerd in een opzoeken, leiden opslagruimte en schrijfsnelheid. Secundaire indexen kunnen worden toegevoegd of verwijderd na het maken van tabellen en wijzigingen in bestaande query's vereisen: query's alleen sneller uitgevoerd. Afhankelijk van uw behoeften, kunt u maken van de gedekte indexen, functionele indexen of beide.

### <a name="use-covered-indexes"></a>Gebruik waarvoor indexen

Gedekte indexen zijn indexen die gegevens opnemen uit de rij samen met de waarden die zijn geïndexeerd. Als er op de gewenste indexvermelding, is er niet nodig voor toegang tot de primaire tabel.

Bijvoorbeeld, in het voorbeeld contact op met tabel kunt u een secundaire index maken voor alleen de kolom socialSecurityNum. Deze secundaire index query's die op socialSecurityNum waarden filteren wilt versnellen, maar bij het ophalen van andere waarden moet een andere lezen op de belangrijkste tabel.

|rowkey|       Adres|   telefoon| Voornaam| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Davids-John-111|1111 San Gabriel Dr.|1-425-000-0002|    John|Davids| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Als u doorgaans wilt de voornaam en achternaam gegeven van de socialSecurityNum opzoeken, kan u echter een gedekte index die de voornaam en achternaam als de werkelijke gegevens in de indextabel bevat maken:

    CREATE INDEX ssn_idx ON CONTACTS (socialSecurityNum) INCLUDE(firstName, lastName);

Deze gedekt index kunt de volgende query uit om alle gegevens verkrijgen door te lezen van de tabel met de secundaire index:

    SELECT socialSecurityNum, firstName, lastName FROM CONTACTS WHERE socialSecurityNum > 100;

### <a name="use-functional-indexes"></a>Functionele indexen gebruiken

Functionele indexen kunnen u een index te maken op een willekeurige expressie die u verwacht te worden gebruikt in query's. Als u een functionele index in plaats hebben en een query die expressie wordt gebruikt, kan de index kan worden gebruikt om op te halen van de resultaten in plaats van de tabel.

U kunt bijvoorbeeld een index zodat u een niet-hoofdlettergevoelige zoekopdrachten op de gecombineerde voornaam en achternaam van een persoon maken:

     CREATE INDEX FULLNAME_UPPER_IDX ON "Contacts" (UPPER("firstName"||' '||"lastName"));

## <a name="query-design"></a>Het ontwerp van query

De belangrijkste overwegingen in queryontwerp zijn:

* Het queryplan begrijpen en controleer of het verwachte gedrag.
* Deelnemen aan efficiënt.

### <a name="understand-the-query-plan"></a>Het queryplan begrijpen

In [SQLLine](http://sqlline.sourceforge.net/), uitleg gevolgd door uw SQL-query gebruiken om te bekijken van het plan van bewerkingen die door Phoenix worden uitgevoerd. Controleer of het abonnement:

* Maakt gebruik van uw primaire sleutel, indien van toepassing.
* Maakt gebruik van toepassing secundaire indexen, in plaats van de tabel.
* Maakt gebruik van bereik SCANNEN of OVERSLAAN SCAN indien mogelijk, in plaats van tabel SCANNEN.

#### <a name="plan-examples"></a>Voorbeelden van plan bent

Een voorbeeld: Stel dat u hebt een tabel met de naam van VLUCHTEN waarin flight vertraging informatie.

Selecteer de vluchten met een airlineid van `19805`, waarbij airlineid een veld dat zich niet in de primaire sleutel of in een index:

    select * from "FLIGHTS" where airlineid = '19805';

Voer de opdracht uitleggen als volgt uit:

    explain select * from "FLIGHTS" where airlineid = '19805';

Het queryplan ziet er als volgt:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN FULL SCAN OVER FLIGHTS
        SERVER FILTER BY AIRLINEID = '19805'

Houd er rekening mee de woordgroep volledige SCAN via VLUCHTEN in dit plan. Deze zin geeft aan dat de uitvoering wordt het SCANNEN van een tabel alle rijen in de tabel in plaats van met de efficiënter bereik SCANNEN of OVERSLAAN SCAN-optie.

Nu dat u wilt zoeken naar vluchten op 2 januari 2014 voor de provider `AA` waar de flightnum is groter dan 1. Stel dat de kolommen jaar, maand, dayofmonth, vervoerder en flightnum in de voorbeeldtabel bestaat en allemaal deel uit van de samengestelde primaire sleutel. De query zou er als volgt uitzien:

    select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

We gaan het abonnement voor deze query's uitvoeren met:

    explain select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Het resulterende plan is:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER FLIGHTS [2014,1,2,'AA',2] - [2014,1,2,'AA',*]

De waarden tussen vierkante haken zijn het bereik van waarden voor de primaire sleutels. In dit geval de waarden voor het jaar 2014 maand 1 en 2 dayofmonth zijn opgelost, maar -waarden zijn toegestaan voor flightnum opgestart met 2 en klik op (`*`). Deze queryplan wordt bevestigd dat de primaire sleutel wordt gebruikt zoals verwacht.

Maak vervolgens een index voor de VLUCHTEN-tabel met de naam `carrier2_idx` dat zich op het veld carrier alleen. Deze index bevat ook flightdate, tailnum oorsprong en flightnum als gedekte kolommen waarvan de gegevens ook in de index opgeslagen worden.

    CREATE INDEX carrier2_idx ON FLIGHTS (carrier) INCLUDE(FLIGHTDATE,TAILNUM,ORIGIN,FLIGHTNUM);

Stel dat u wilt ophalen van de provider die samen met de flightdate en tailnum, zoals in de volgende query uit:

    explain select carrier,flightdate,tailnum from "FLIGHTS" where carrier = 'AA';

Hier ziet u deze index wordt gebruikt:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER CARRIER2_IDX ['AA']

Voor een volledige lijst van de items die kan worden weergegeven in plan resultaten uitgelegd, Zie de sectie wordt uitgelegd plannen in de [Apache Phoenix Afstemmingshandleiding](https://phoenix.apache.org/tuning_guide.html).

### <a name="join-efficiently"></a>Deelnemen aan efficiënt

Over het algemeen u om te voorkomen dat joins, tenzij een-zijde klein, met name op regelmatig query's is.

Als nodig is, u grote samenvoegingen met doen kunt de `/*+ USE_SORT_MERGE_JOIN */` hint, maar een grote join is een dure bewerking via een zeer groot aantal rijen. Als de totale grootte van alle rechts aan tabellen zou groter zijn dan het beschikbare geheugen, gebruikt u de `/*+ NO_STAR_JOIN */` hint.

## <a name="scenarios"></a>Scenario's

De volgende richtlijnen beschrijven enkele algemene patronen.

### <a name="read-heavy-workloads"></a>Leesintensief workloads

Voor leesintensief use-cases, zorg ervoor dat u met behulp van indexen. Als u wilt lezen-overhead tijd besparen, Bedenk gedekte indexen te maken.

### <a name="write-heavy-workloads"></a>Schrijfintensief workloads

Maak voor schrijfintensief workloads waar de primaire sleutel is gestaag toenemende, salt buckets om te voorkomen schrijven hotspots ten koste van de algemene lezen doorvoer vanwege de extra scant die nodig zijn. Ook als u UPSERT voor het schrijven van een groot aantal records, uitschakelen Committed en batch de records.

### <a name="bulk-deletes"></a>Bulksgewijs verwijderen

Bij het verwijderen van een grote gegevensset, schakel Committed alvorens de query verwijderen zodat de client niet hoeft te onthouden van de rij-sleutels voor alle verwijderde rijen. Committed wordt voorkomen dat de client bufferen van de rijen beïnvloed door het verwijderen, zodat die Phoenix ze rechtstreeks op de regioservers zonder de kosten van de terug te sturen naar de client kunt verwijderen.

### <a name="immutable-and-append-only"></a>Onveranderbare en alleen toevoegen

Als uw scenario schrijfsnelheid via de integriteit van gegevens ligt, kunt u overwegen de write-ahead van logboek uitschakelen bij het maken van tabellen:

    CREATE TABLE CONTACTS (...) DISABLE_WAL=true;

Zie voor meer informatie over deze en andere opties [Phoenix grammatica](http://phoenix.apache.org/language/index.html#options).

## <a name="next-steps"></a>Volgende stappen

* [Phoenix Afstemmingshandleiding](https://phoenix.apache.org/tuning_guide.html)
* [Secundaire indexen](http://phoenix.apache.org/secondary_indexing.html)
