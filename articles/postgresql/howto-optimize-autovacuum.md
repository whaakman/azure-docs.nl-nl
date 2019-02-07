---
title: Optimaliseren autovacuum op een Azure Database for PostgreSQL-server
description: Dit artikel wordt beschreven hoe u autovacuum op een Azure Database for PostgreSQL-server kunt optimaliseren.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: e8e9991f20481deee85a6d582582335eb98e3c24
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55815214"
---
# <a name="optimize-autovacuum-on-an-azure-database-for-postgresql-server"></a>Optimaliseren autovacuum op een Azure Database for PostgreSQL-server 
In dit artikel wordt beschreven hoe u effectief autovacuum op een Azure Database for PostgreSQL-server te optimaliseren.

## <a name="overview-of-autovacuum"></a>Overzicht van autovacuum
PostgreSQL maakt gebruik van multiversion gelijktijdigheidsbeheer (MVCC) om toe te staan van meer gelijktijdigheid van de database. Elke update resulteert in een invoegen en verwijderen, en elke verwijderen resulteert in rijen wordt voorlopig gemarkeerd voor verwijdering. Voorlopig-markering identificeert dead tuples die later worden opgeschoond. Als u wilt deze taken uit te voeren, voert PostgreSQL onderdruk een taak.

Een onderdruk taak kan worden geactiveerd, handmatig of automatisch. Meer dead tuples bestaat wanneer de database zware update optreedt of verwijderen van bewerkingen. Minder dead tuples bestaan als de database niet actief is. U moet vaker vacuüm wanneer de belasting van de database is zwaar, waardoor de uitvoering van taken onderdruk *handmatig* onhandig.

Autovacuum kan worden geconfigureerd en de voordelen van het afstemmen. De standaardwaarden die PostgreSQL wordt geleverd met probeert te controleren of dat het product werkt voor alle typen apparaten. Deze apparaten omvatten Raspberry verwerkingsinstructies. De ideale configuratiewaarden afhankelijk zijn van de:
- Totaal aantal resources beschikbaar, zoals SKU en opslaggrootte.
- Gebruik van bronnen.
- Kenmerken van het individuele object.

## <a name="autovacuum-benefits"></a>Autovacuum voordelen
Als u niet van tijd tot tijd vacuüm, kan de dode tuples die worden verzameld resulteren in:
- Gegevens snel, zoals grotere databases en tabellen.
- Grotere suboptimale indexen.
- Hogere i/o's.

## <a name="monitor-bloat-with-autovacuum-queries"></a>Monitor gegevensophoping met autovacuum query 's
De volgende voorbeeldquery is ontworpen om het aantal inactieve en liveregioservers tuples in een tabel met de naam XYZ te identificeren:
 
    'SELECT relname, n_dead_tup, n_live_tup, (n_dead_tup/ n_live_tup) AS DeadTuplesRatio, last_vacuum, last_autovacuum FROM pg_catalog.pg_stat_all_tables WHERE relname = 'XYZ' order by n_dead_tup DESC;'

## <a name="autovacuum-configurations"></a>Autovacuum configuraties
De configuratieparameters die autovacuum regelen zijn gebaseerd op de antwoorden op twee belangrijke vragen:
- Wanneer moet deze starten?
- Hoeveel moet het opschonen nadat deze is gestart?

Hier volgen enkele parameters die u kunt bijwerken op basis van de vorige vragen, samen met enkele richtlijnen autovacuum-configuratie.
Parameter|Description|Standaardwaarde
---|---|---
autovacuum_vacuum_threshold|Hiermee geeft u het minimum aantal bijgewerkte of verwijderde tuples die nodig zijn voor het activeren van een onderdruk bewerking in de tabel. De standaardwaarde is 50 tuples. Stel deze parameter alleen in het bestand postgresql.conf of op de server vanaf de opdrachtregel. Als u wilt overschrijven de instellingen voor afzonderlijke tabellen, moet u de parameters van de opslag tabel wijzigen.|50
autovacuum_vacuum_scale_factor|Hiermee geeft u een fractie van de grootte van de tabel toevoegen aan autovacuum_vacuum_threshold als u besluit een onderdruk opnieuw activeren. De standaardwaarde is 0.2, 20 procent van de tabelgrootte. Stel deze parameter alleen in het bestand postgresql.conf of op de server vanaf de opdrachtregel. Als u wilt overschrijven de instellingen voor afzonderlijke tabellen, moet u de parameters van de opslag tabel wijzigen.|5 procent
autovacuum_vacuum_cost_limit|Hiermee geeft u de waarde voor het beperken van kosten gebruikt in automatische onderdruk bewerkingen. Als de -1 is opgegeven, dit de standaardinstelling is, wordt de waarde reguliere vacuum_cost_limit gebruikt. Als er meer dan één werknemer, wordt de waarde evenredig verdeeld over de actieve autovacuum werknemers. De som van de limieten van elke werknemer niet langer zijn dan de waarde van deze variabele. Stel deze parameter alleen in het bestand postgresql.conf of op de server vanaf de opdrachtregel. Als u wilt overschrijven de instellingen voor afzonderlijke tabellen, moet u de parameters van de opslag tabel wijzigen.|-1
autovacuum_vacuum_cost_delay|Hiermee geeft u de waarde voor de vertraging kosten die worden gebruikt in automatische onderdruk bewerkingen. Als u -1 opgeeft, wordt de waarde reguliere vacuum_cost_delay gebruikt. De standaardwaarde is 20 milliseconden. Stel deze parameter alleen in het bestand postgresql.conf of op de server vanaf de opdrachtregel. Als u wilt overschrijven de instellingen voor afzonderlijke tabellen, moet u de parameters van de opslag tabel wijzigen.|20 ms
autovacuum_nap_time|Hiermee geeft u dat de minimale vertraging tussen autovacuum wordt uitgevoerd op een bepaalde database. In elke ronde de daemon onderzoekt de database en geeft ONDERDRUK en analyseren-opdrachten voor tabellen in de database. De vertraging wordt gemeten in seconden en de standaardwaarde is 1 minuut (1 min). Stel deze parameter alleen in het bestand postgresql.conf of op de server vanaf de opdrachtregel.|15 s
autovacuum_max_workers|Hiermee geeft u het maximum aantal autovacuum processen, dan het autovacuum startprogramma voor, die kunnen worden uitgevoerd op elk willekeurig moment. De standaardwaarde is 3. Stel deze parameter alleen aan begin van de server.|3
Als u wilt overschrijven de instellingen voor afzonderlijke tabellen, moet u de parameters van de opslag tabel wijzigen. 

## <a name="autovacuum-cost"></a>Autovacuum kosten
Hier volgen de "kosten" van het uitvoeren van een onderdruk bewerking:

- De data-pagina's die de onderdruk wordt uitgevoerd op zijn vergrendeld.
- Computing en geheugen worden gebruikt wanneer een onderdruk taak wordt uitgevoerd.

Als gevolg hiervan niet onderdruk taken uitvoeren: te veel of te weinig. Er moet een onderdruk taak aan te passen aan de werkbelasting. Test alle autovacuum Parameterwijzigingen vanwege de nadelen van elke service.

## <a name="autovacuum-start-trigger"></a>Autovacuum start trigger
Autovacuum wordt geactiveerd wanneer het aantal dode tuples autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor overschrijdt * reltuples. Reltuples is hier een constante.

Het opruimen van autovacuum moet houden met de belasting van de database. U kunt anders geen opslagruimte meer en zich een algemene vertraging van query's. Afgeschreven na verloop van tijd, de snelheid waarmee een onderdruk bewerking dead tuples ruimt moet gelijk zijn aan de snelheid waarmee dead tuples worden gemaakt.

Databases met een groot aantal updates en verwijderingen hebben meer dead tuples en meer ruimte nodig hebt. Over het algemeen databases met een groot aantal updates en verwijdert u voordeel van lage waarden van autovacuum_vacuum_scale_factor en autovacuum_vacuum_threshold. De lage waarden te voorkomen dat langdurige opeenstapeling van dead tuples. Omdat de noodzaak van vacuuming minder urgent is, kunt u hogere waarden voor beide parameters met kleinere databases. Frequente vacuuming wordt geleverd leiden Computing en geheugen.

De schaalfactor standaardwaarde van 20 procent voor tabellen met een laag percentage dead tuples goed werkt. Werkt niet goed op tabellen met een hoog percentage dead tuples. Bijvoorbeeld in een tabel 20 GB deze schaalfactor wordt omgezet in 4 GB aan andere tuples. Het is 200 GB dead tuples in een tabel 1 TB.

U kunt deze parameters met PostgreSQL instellen op het tabelniveau of exemplaar. U kunt nu deze parameters instellen op het tabelniveau van de alleen in de Azure Database voor PostgreSQL.

## <a name="estimate-the-cost-of-autovacuum"></a>Maak een schatting van de kosten van autovacuum
Actieve autovacuum "kostbare" is en er zijn parameters voor het beheren van de runtime van onderdruk bewerkingen. De volgende parameters te schatten van de kosten van het uitvoeren van onderdruk:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss = 10
- vacuum_cost_page_dirty = 20

Het proces onderdruk fysieke pagina's leest en controleert op dead tuples. Elke pagina in shared_buffers wordt beschouwd als een waarde van 1 (vacuum_cost_page_hit) hebben. Alle andere pagina's worden beschouwd als een kosten van 20 (vacuum_cost_page_dirty), als dead tuples bestaat, of 10 (vacuum_cost_page_miss), hebben als er geen andere tuples bestaat. De bewerking onderdruk stopt wanneer het proces is groter dan de autovacuum_vacuum_cost_limit. 

Nadat de limiet is bereikt, schakelt het proces voor de duur die is opgegeven door de parameter autovacuum_vacuum_cost_delay voordat deze wordt opnieuw gestart. Als de limiet niet wordt bereikt, wordt na de waarde gespecificeerd door de parameter autovacuum_nap_time autovacuum gestart.

Kortom bepalen de parameters autovacuum_vacuum_cost_delay en autovacuum_vacuum_cost_limit hoeveel gegevens opschonen per tijdseenheid is toegestaan. Houd er rekening mee dat de standaardwaarden te klein voor de meeste Prijscategorieën zijn. De optimale waarden voor deze parameters zijn afhankelijk van het laag-prijzen en dienovereenkomstig moeten worden geconfigureerd.

De parameter autovacuum_max_workers bepaalt het maximum aantal autovacuum processen die tegelijkertijd kunnen worden uitgevoerd.

U kunt deze parameters met PostgreSQL instellen op het tabelniveau of exemplaar. U kunt nu deze parameters instellen op het tabelniveau van de alleen in de Azure Database voor PostgreSQL.

## <a name="optimize-autovacuum-per-table"></a>Autovacuum per tabel optimaliseren
U kunt alle vorige configuratieparameters per tabel configureren. Hier volgt een voorbeeld:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

Autovacuum is een synchrone per tabel-proces. De grotere hoeveelheid dead tuples die een tabel, hoe hoger heeft de "kosten" autovacuum. U kunt tabellen waarvoor een hoge frequentie van updates en verwijderingen in meerdere tabellen op te splitsen. Tabellen splitsen helpt bij het parallel autovacuum en de toegangskosten "" voor het voltooien van autovacuum voor één tabel. U kunt ook het aantal parallelle autovacuum werknemers om ervoor te zorgen dat werknemers opneemt zijn gepland verhogen.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende PostgreSQL-documentatie voor meer informatie over het gebruiken en af te stemmen autovacuum:

 - [Hoofdstuk 18, Server-configuratie](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html)
 - [Hoofdstuk 24, Routine databaseonderhoudstaken](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html)
