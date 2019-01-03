---
title: Optimaliseren autovacuum in Azure Database for PostgreSQL-server
description: Dit artikel wordt beschreven hoe u autovacuum in Azure Database for PostgreSQL-server kunt optimaliseren.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 0f8db7dd3a90e06587a7e0e05f33cb6fba5c72e1
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539786"
---
# <a name="optimizing-autovacuum-on-azure-database-for-postgresql-server"></a>Autovacuum op Azure Database for PostgreSQL-server te optimaliseren 
In dit artikel wordt beschreven hoe u effectief optimaliseren autovacuum op Azure Database voor PostgreSQL.

## <a name="overview-of-autovacuum"></a>Overzicht van autovacuum
MVCC PostgreSQL gebruikt voor het toestaan van meer gelijktijdigheid van de database. Elke update resulteert in een invoegen en verwijderen, en elke verwijderen resulteert in de rij of rijen wordt voorlopig gemarkeerd voor verwijdering. De resultaten van de soft-markering in de identificatie van dead tuples die later worden opgeschoond. PostgreSQL bereikt dit door een onderdruk taak wordt uitgevoerd.

Een onderdruk taak kan worden geactiveerd, handmatig of automatisch. Meer dead tuples wordt bestaan wanneer deze wordt de database is een met zware bijwerken of verwijderen van bewerkingen en minder als niet-actieve.  De noodzaak voor het uitvoeren van onderdruk vaker wordt groter wanneer de database zwaar belast is; maken van de uitvoering van taken onderdruk **handmatig** onhandig.

Autovacuum kan worden geconfigureerd en de voordelen van het afstemmen. De standaardwaarden die PostgreSQL wordt geleverd met probeer om te controleren of het product werkt op alle soorten apparaten, waaronder Raspberry verwerkingsinstructies en de ideale configuratiewaarden afhankelijk zijn van een aantal factoren:
- Totaal aantal resources beschikbaar - SKU en opslaggrootte.
- Gebruik van bronnen.
- Kenmerken van het individuele object.

## <a name="autovacuum-benefits"></a>Autovacuum voordelen
Als u niet van tijd tot tijd onderdruk uitvoert, wordt de dode tuples die worden verzameld kan resulteren in:
- Gegevens vollopen - grotere databases en tabellen.
- Grotere suboptimale indexen.
- Hogere i/o's.

## <a name="monitoring-bloat-with-autovacuum-queries"></a>Bewaking van gegevensophoping met autovacuum query 's
De volgende voorbeeldquery is ontworpen om het aantal inactieve en liveregioservers tuples in een tabel met de naam "XYZ" te identificeren: ' SELECT %{relname/, n_dead_tup, n_live_tup, (n_dead_tup / n_live_tup) als DeadTuplesRatio, last_vacuum, last_autovacuum FROM pg_catalog.pg_stat_all_tables waar %{relname/ = 'XYZ' order door n_dead_tup DESC;'

## <a name="autovacuum-configurations"></a>Autovacuum configuraties
De configuratieparameters die autovacuum regelen gebaseerd op twee belangrijke vragen:
- Wanneer moet deze starten?
- Hoeveel moet het opschonen nadat deze is gestart?

Hieronder worden enkele van de autovacuum configuratieparameters die u kunt bijwerken op basis van de bovenstaande vragen, samen met enkele richtlijnen:
Parameter|Description|Standaardwaarde
---|---|---
autovacuum_vacuum_threshold|Hiermee geeft u het minimum aantal bijgewerkte of verwijderde tuples die nodig zijn voor het activeren van een ONDERDRUK in de tabel. De standaardwaarde is 50 tuples. Deze parameter kan alleen worden ingesteld in het bestand postgresql.conf of op de server vanaf de opdrachtregel. De instelling kan worden overschreven voor afzonderlijke tabellen door het wijzigen van tabel storage parameters.|50
autovacuum_vacuum_scale_factor|Hiermee geeft u een fractie van de grootte van de tabel toevoegen aan autovacuum_vacuum_threshold als u besluit een ONDERDRUK activeren. De standaardwaarde is 0.2 (20 procent van de tabelgrootte). Deze parameter kan alleen worden ingesteld in het bestand postgresql.conf of op de server vanaf de opdrachtregel. De instelling kan worden overschreven voor afzonderlijke tabellen door het wijzigen van tabel storage parameters.|5 procent
autovacuum_vacuum_cost_limit|Hiermee geeft u de waarde van de kosten beperken die worden gebruikt in automatische ONDERDRUK bewerkingen. Als de -1 wordt opgegeven (dit is de standaardinstelling), wordt de waarde reguliere vacuum_cost_limit worden gebruikt. De waarde wordt evenredig verdeeld over de actieve autovacuum werknemers, als er meer dan één werknemer. De som van de limieten van elke werknemer niet langer zijn dan de waarde van deze variabele. Deze parameter kan alleen worden ingesteld in het bestand postgresql.conf of op de server vanaf de opdrachtregel. De instelling kan worden overschreven voor afzonderlijke tabellen door het wijzigen van tabel storage parameters.|-1
autovacuum_vacuum_cost_delay|Hiermee geeft u de waarde voor de kosten vertraging die wordt gebruikt in automatische ONDERDRUK bewerkingen. Als de -1 is opgegeven, wordt de waarde reguliere vacuum_cost_delay worden gebruikt. De standaardwaarde is 20 milliseconden. Deze parameter kan alleen worden ingesteld in het bestand postgresql.conf of op de server vanaf de opdrachtregel. De instelling kan worden overschreven voor afzonderlijke tabellen door het wijzigen van tabel storage parameters.|20 ms
autovacuum_nap_time|Hiermee geeft u dat de minimale vertraging tussen autovacuum wordt uitgevoerd op een bepaalde database. In elke ronde de daemon onderzoekt de database en geeft ONDERDRUK en analyseren-opdrachten voor tabellen in de database. De vertraging wordt gemeten in seconden en de standaardwaarde is 1 minuut (1 min). Deze parameter kan alleen worden ingesteld in het bestand postgresql.conf of op de server vanaf de opdrachtregel.|15 s
autovacuum_max_workers|Hiermee geeft u het maximum aantal autovacuum processen (met uitzondering van het startprogramma voor autovacuum) die op elk gewenst moment worden uitgevoerd. De standaardwaarde is 3. Deze parameter kan alleen worden ingesteld op de server start.|3
De bovenstaande instellingen kunnen worden overschreven voor afzonderlijke tabellen door het wijzigen van tabel storage parameters.  

## <a name="autovacuum-cost"></a>Autovacuum kosten
Hieronder vindt u de "kosten" van het uitvoeren van een onderdruk bewerking:
- Een vergrendeling is op de pagina's onderdruk wordt uitgevoerd op gegevens geplaatst.
- Computing en geheugen worden gebruikt wanneer onderdruk wordt uitgevoerd.

Dit betekent dat onderdruk een te vaak al dan niet mogen uitvoeren of te weinig, moet dit adaptieve voor de werkbelasting. Het is raadzaam om alle autovacuum Parameterwijzigingen vanwege de nadelen van elke testen.

## <a name="autovacuum-start-trigger"></a>Autovacuum start trigger
Autovacuum wordt geactiveerd wanneer het aantal dode tuples autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor overschrijdt * reltuples, reltuples hier is een constante.

Het opruimen van autovacuum moet blijven van de belasting van de database, u kunt anders geen opslagruimte meer en ervaring van een algemene vertraging van query's. Afgeschreven na verloop van tijd, de snelheid die onderdruk ruimt dead tuples moet gelijk zijn aan de snelheid waarmee dead tuples worden gemaakt.

Databases met veel bijgewerkt of verwijderd hebben meer dead tuples en meer ruimte nodig hebt. Over het algemeen databases met veel updates/verwijdering voordeel van het lage waarden van autovacuum_vacuum_scale_factor en lage waarden van autovacuum_vacuum_threshold om te voorkomen dat langdurige opeenstapeling van dead tuples. U kunt hogere waarden gebruiken voor beide parameters met kleinere databases, omdat de noodzaak van onderdruk minder urgent is. Herinnering, dat regelmatig vacuuming leiden Computing en geheugen wordt geleverd.

De schaalfactor standaardwaarde van 20 procent werkt goed op tabellen met een lage percentage aan andere tuples, maar niet op tabellen met een hoog percentage van de dode tuples. Het is 200 GB dead tuples bijvoorbeeld in een tabel 20 GB die dit wordt omgezet in 4 GB aan andere tuples en in een tabel 1 TB.

U kunt deze parameters met PostgreSQL instellen op het tabelniveau of exemplaar. Vandaag, kunnen deze parameters worden ingesteld op het tabelniveau van de alleen in de Azure Database voor PostgreSQL.

## <a name="estimating-the-cost-of-autovacuum"></a>De kostenschatting van autovacuum
Actieve autovacuum is 'kostbare' en er zijn parameters voor het beheren van de runtime van onderdruk bewerkingen. De volgende parameters te schatten van de kosten van het uitvoeren van onderdruk:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss = 10
- vacuum_cost_page_dirty = 20

Het proces onderdruk fysieke pagina's leest en controleert op dead tuples. Elke pagina in shared_buffers wordt beschouwd als een waarde van 1 (vacuum_cost_page_hit) hebben, alle andere pagina's worden beschouwd als een kosten van 20 (vacuum_cost_page_dirty) als andere tuples bestaat, of 10 (vacuum_cost_page_miss) als er geen andere tuples bestaat. De bewerking onderdruk stopt wanneer het proces autovacuum_vacuum_cost_limit overschrijdt.  

Nadat de limiet is bereikt, schakelt het proces voor de duur die is opgegeven door de parameter autovacuum_vacuum_cost_delay voordat opnieuw wordt gestart. Als de limiet niet wordt bereikt, wordt na de waarde gespecificeerd door de parameter autovacuum_nap_time autovacuum gestart.

Kortom bepalen de parameters autovacuum_vacuum_cost_delay en autovacuum_vacuum_cost_limit hoeveel gegevens opschonen per tijdseenheid is toegestaan. Houd er rekening mee dat de standaardwaarden zijn te klein voor de meeste Prijscategorieën. De optimale waarden voor deze parameters zijn afhankelijk van het laag-prijzen en dienovereenkomstig moeten worden geconfigureerd.

De parameter autovacuum_max_workers bepaalt het maximum aantal autovacuum processen die tegelijkertijd kunnen worden uitgevoerd.

U kunt deze parameters met PostgreSQL instellen op het tabelniveau of exemplaar. Vandaag, kunnen deze parameters worden ingesteld op het tabelniveau van de alleen in de Azure Database voor PostgreSQL.

## <a name="optimizing-autovacuum-per-table"></a>Autovacuum per tabel optimaliseren
Alle bovenstaande configuratieparameters kunnen worden geconfigureerd per tabel, bijvoorbeeld:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

Autovacuum is een per tabel synchrone proces. De grotere procent van de dode tuples een tabel heeft een hogere de "kosten" autovacuum.  Tabellen waarvoor een hoge frequentie van updates/verwijdering in meerdere tabellen splitsen kunnen parallel autovacuum en verlagen van de "kosten" voor het voltooien van autovacuum voor één tabel. U kunt ook het aantal parallelle autovacuum werknemers om ervoor te zorgen werknemers opneemt zijn gepland verhogen.

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende PostgreSQL documenatation voor meer informatie over het gebruik en het afstemmen van autovacuum:
 - Documentatie voor PostgreSQL - [hoofdstuk 18, Server-configuratie](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html)
 - PostgreSQL-documentatie – [hoofdstuk 24 uur per dag, routinetaken voor Database-onderhoud](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html)
