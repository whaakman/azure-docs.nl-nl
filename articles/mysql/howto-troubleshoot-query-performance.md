---
title: Het oplossen van de prestaties van query's in Azure-Database voor MySQL
description: Dit artikel wordt beschreven hoe u uitleg oplossen van problemen met prestaties van query's in Azure-Database voor MySQL gebruikt.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 3af6ad347cec171132ddfbec21137775c0f71245
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-use-explain-to-profile-query-performance-in-azure-database-for-mysql"></a>Het gebruik van uitleg voor queryprestaties in Azure-Database voor MySQL profiel
**UITLEGGEN** is een handig hulpmiddel om te optimaliseren van query's. Leg uit de instructie kan worden gebruikt voor informatie over hoe SQL-instructies worden uitgevoerd. De volgende uitvoer toont een voorbeeld van de uitvoering van een instructie uitleg.

```sql
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 10.00
        Extra: Using where
```

Zoals te zien is in dit voorbeeld wordt de waarde van *sleutel* is NULL. Deze uitvoer betekent MySQL indexen geoptimaliseerd voor de query kan niet worden gevonden en deze een volledige tabelscan uitvoert. Laten we deze query te optimaliseren door het toevoegen van een index op de **ID** kolom.

```sql
mysql> ALTER TABLE tb1 ADD KEY (id);
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ref
possible_keys: id
          key: id
      key_len: 4
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
```

De nieuwe uitleg ziet u dat MySQL nu een index gebruikt voor het aantal rijen 1, wat op zijn beurt aanzienlijk de zoektijd verkort beperken.
 
## <a name="covering-index"></a>Die betrekking hebben op index
Een index voor bestaat uit alle kolommen van een query in de index van waarde voor het ophalen van gegevenstabellen verminderen. Hier volgt een afbeelding in de volgende **GROUP BY** instructie.
 
```sql
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

Zoals u kunt zien van de uitvoer, MySQL gebruikt geen indexen omdat geen juiste indexen beschikbaar zijn. U ziet ook *met tijdelijke; Met de opdracht bestand sorteren*, wat betekent dat MySQL maakt een tijdelijke tabel om te voldoen aan de **GROUP BY** component.
 
Een index te maken in de kolom **c2** alleen zorgt ervoor dat er geen verschil en MySQL nog moet een tijdelijke tabel maken:

```sql 
mysql> ALTER TABLE tb1 ADD KEY (c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

In dit geval een **gedekte index** op zowel **c1** en **c2** kan worden gemaakt, waarbij de waarde van **c2**'rechtstreeks in de index van meer gegevens lookup elimineren.

```sql 
mysql> ALTER TABLE tb1 ADD KEY covered(c1,c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: covered
          key: covered
      key_len: 108
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using index
```

Zoals blijkt uit de bovenstaande uitleg, MySQL nu maakt gebruik van de gedekte index- en Vermijd het maken van een tijdelijke tabel. 

## <a name="combined-index"></a>Gecombineerde index
Een gecombineerde index waarden uit meerdere kolommen bestaat en kan worden beschouwd als een matrix van rijen die worden gesorteerd door waarden van de geïndexeerde kolommen samen te voegen. Deze methode is handig in een **GROUP BY** instructie.

```sql
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using filesort
```

MySQL voert een *bestand sorteren* bewerking die is redelijk vertragen, vooral wanneer er te veel rijen sorteren. Voor het optimaliseren van deze query kan een gecombineerde index worden gemaakt op beide kolommen die zijn gesorteerd.

```sql 
mysql> ALTER TABLE tb1 ADD KEY my_sort2 (c1, c2);
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: NULL
          key: my_sort2
      key_len: 108
          ref: NULL
         rows: 10
     filtered: 11.11
        Extra: Using where; Using index
```

De uitleg nu zien dat MySQL gecombineerde index gebruiken om te voorkomen dat extra sorteer omdat de index is al gesorteerd.
 
## <a name="conclusion"></a>Conclusie
 
Met uitleg en een ander type indexen groter aanzienlijk worden prestaties. Omdat er een index op betekent de tabel niet noodzakelijkerwijs dat MySQL zou kunnen gebruiken voor uw query's. Altijd uw veronderstellingen met uitleg te valideren en optimaliseren van uw query's met behulp van indexen.


## <a name="next-steps"></a>Volgende stappen
- Als u wilt zoeken peer antwoorden op uw meest betrokken vragen of een nieuwe vraag/antwoord post, gaat u naar [MSDN-forum](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
