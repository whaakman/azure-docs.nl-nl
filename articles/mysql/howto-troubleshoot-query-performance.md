---
title: Problemen oplossen met de prestaties van query's in Azure Database for MySQL
description: In dit artikel wordt beschreven hoe u uitleg gebruiken voor het oplossen van prestaties van query's in Azure Database voor MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 819e2393619766d46385cdd6fe550fff1e1a7631
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53537661"
---
# <a name="how-to-use-explain-to-profile-query-performance-in-azure-database-for-mysql"></a>Hoe u met uitleg profiel prestaties van query's in Azure Database for MySQL
**Leg uit** is een handig hulpmiddel voor query's optimaliseren. Leg uit de instructie kan worden gebruikt voor informatie over hoe SQL-instructies worden uitgevoerd. De volgende uitvoer ziet u een voorbeeld van de uitvoering van een instructie uitleg.

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

Zoals te zien is in dit voorbeeld is de waarde van *sleutel* is NULL. Deze uitvoer betekent MySQL kan geen indexen die is geoptimaliseerd voor de query niet vinden en een volledige tabelcontrole uitgevoerd worden uitgevoerd. Laten we deze query te optimaliseren door het toevoegen van een index op de **ID** kolom.

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

De nieuwe uitleg laat zien dat MySQL nu een index gebruikt op het aantal rijen 1, wat op zijn beurt aanzienlijk de zoektijd verkort beperken.
 
## <a name="covering-index"></a>Die betrekking hebben op index
Een index die bestaat uit alle kolommen van een query in de index te verminderen van de waarde voor het ophalen van gegevenstabellen. Hier volgt een voorbeeld in de volgende **GROUP BY** instructie.
 
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

Zoals u kunt zien in de uitvoer, MySQL gebruikt geen geen indexen omdat er geen juiste indexen beschikbaar zijn. U ziet ook *met behulp van tijdelijke; Met behulp van bestand sorteren*, wat betekent dat MySQL maakt een tijdelijke tabel om te voldoen aan de **GROUP BY** component.
 
Het maken van een index voor kolom **c2** alleen is geen verschil en MySQL nog steeds nodig heeft om een tijdelijke tabel te maken:

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

In dit geval een **gedekte index** op zowel **c1** en **c2** kan worden gemaakt, waarbij de waarde van **c2**'rechtstreeks in de index van Elimineer verder opzoeken van gegevens.

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

Zoals in de bovenstaande uitleg wordt weergegeven, MySQL nu maakt gebruik van de gedekte index- en Vermijd het maken van een tijdelijke tabel. 

## <a name="combined-index"></a>Gecombineerde index
Een gecombineerde index waarden uit meerdere kolommen bestaat, en kan worden beschouwd als een matrix van rijen die door het samenvoegen van waarden van de geïndexeerde kolommen worden gesorteerd. Deze methode kan nuttig zijn bij een **GROUP BY** instructie.

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

MySQL wordt uitgevoerd een *bestand sorteren* bewerking die is redelijk langzaam, met name wanneer er te veel rijen sorteren. Voor het optimaliseren van deze query kan een gecombineerde index worden gemaakt op beide kolommen die zijn gesorteerd.

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

De uitleg nu zien dat MySQL gecombineerde index gebruiken om te voorkomen dat aanvullende sorteren omdat de index is al gesorteerd.
 
## <a name="conclusion"></a>Conclusie
 
Met behulp van uitleg en een ander type indexen kan aanzienlijk prestaties. Omdat er een index op betekent de tabel niet noodzakelijkerwijs dat MySQL zou kunnen gebruiken voor uw query's. Altijd uw veronderstellingen met uitleg te valideren en optimaliseren van uw query's met behulp van indexen.


## <a name="next-steps"></a>Volgende stappen
- Peer-antwoorden op uw meest betrokken vragen of een nieuwe vraag/antwoord plaatsen, gaat u naar [MSDN-forum](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
