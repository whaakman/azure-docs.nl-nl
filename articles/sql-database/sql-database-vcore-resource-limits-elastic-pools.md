---
title: Azure SQL Database vCore gebaseerde resourcelimieten - elastische pools | Microsoft Docs
description: Deze pagina Beschrijving van enkele veelvoorkomende vCore gebaseerde resourcelimieten voor elastische pools in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: f41974c6e2b2b0565f0a2703cfd638777a6bb9eb
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52878020"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools"></a>Azure SQL Database vCore gebaseerde model limieten voor elastische pools aanschaffen

Dit artikel bevat de gedetailleerde resourcelimieten voor Azure SQL Database elastische pools en gepoolde databases met behulp van het op vCore gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten, [SQL Database DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

U kunt de servicelaag, rekencapaciteit en opslag met behulp van bedrag instellen de [Azure-portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), wordt de [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), of de [REST-API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> De resourcelimieten van afzonderlijke databases in elastische pools zijn algemeen hetzelfde als voor individuele databases buiten pools met dezelfde grootte berekenen. Bijvoorbeeld, is de maximale gelijktijdige werknemers voor een database GP_Gen4_1 200 werknemers. De maximale gelijktijdige werknemers voor een database in een pool GP_Gen4_1 is dus ook 200 werknemers. Opmerking: het totale aantal gelijktijdige werknemers in de groep GP_Gen4_1 is 210.

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Algemeen doel-servicelaag: opslaggrootte en compute-grootten

### <a name="generation-4-compute-platform"></a>Computerplatform generatie 4

|COMPUTE-grootte|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|Maximumgrootte van gegevens (GB)|512|756|1536|2048|3584|4096|
|Maximale logboekgrootte|154|227|461|614|1075|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Doel-IOPS (van 64 KB)|500|1000|2000|4000|7000|7000|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|3360|5040|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|100|200|500|500|500|500|
|Min./Max. elastische pool vCore-opties per database|0, 0,25, 0,5, 1|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24 uur per dag|
|Aantal replica's|1|1|1|1|1|1|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5

|COMPUTE-grootte|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|Maximumgrootte van gegevens (GB)|512|756|1536|2048|3072|4096|4096|4096|
|Maximale logboekgrootte|154|227|461|614|922|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (van 64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|2520|3360|4200|8400
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|100|200|500|500|500|500|500|500|
|Min./Max. elastische pool vCore-opties per database|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24 uur per dag|0, 0,5, 1, 2, 4, 8, 16, 24 uur per dag, 32|0, 0,5, 1, 2, 4, 8, 16, 24 uur per dag, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24 uur per dag, 32, 40, 80|
|Aantal replica's|1|1|1|1|1|1|1|1|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>Kritieke-bedrijfslaag: opslaggrootte en compute-grootten

### <a name="generation-4-compute-platform"></a>Computerplatform generatie 4

|COMPUTE-grootte|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|1|2|4|8|20|36|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (GB)|1024|1024|1024|1024|1024|1024|
|Maximale logboekgrootte|307|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|384|
|Doel-IOPS (van 64 KB)|5000|10.000|20000|40000|80000|120000|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|3360|5040|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|Slechts één DB's worden ondersteund voor deze compute-grootte|50|100|100|100|100|
|Min./Max. elastische pool vCore-opties per database|N/A|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24 uur per dag|
|Aantal replica's|3|3|3|3|3|3|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

#### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5

|COMPUTE-grootte|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Maximumgrootte van gegevens (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maximale logboekgrootte|307|307|307|307|614|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (van 64 KB)|5000|10.000|20000|40000|60000|80000|100000|200000
|Maximaal aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|2520|3360|5040|8400|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|N/A|50|100|100|100|100|100|100|
|Min./Max. elastische pool vCore-opties per database|N/A|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24 uur per dag|0, 0,5, 1, 2, 4, 8, 16, 24 uur per dag, 32|0, 0,5, 1, 2, 4, 8, 16, 24 uur per dag, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24 uur per dag, 32, 40, 80|
|Aantal replica's|3|3|3|3|3|3|3|3|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

Als alle vCores van een elastische pool bezet zijn, ontvangt elke database in de groep een gelijke hoeveelheid rekenresources voor het verwerken van query's. De service SQL Database verdeelt resources eerlijk over databases door gelijke hoeveelheden rekentijd te garanderen. Er is een elastische pool resources eerlijk naast een bepaalde hoeveelheid van de resource wordt aan elke database wordt gegarandeerd wanneer het vCore-minimum per database is ingesteld op een andere waarde dan nul.

## <a name="database-properties-for-pooled-databases"></a>Database-eigenschappen voor databases in pools

De volgende tabel beschrijft de eigenschappen voor databases in pools.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Maximum aantal vCores per database |Het maximale aantal vCores op dat elke database in de groep gebruiken mag, beschikbaar op basis van gebruik door andere databases in de groep. Maximum aantal vCores per database is geen resourcegarantie voor een database. Het is een algemene instelling voor alle databases in de groep. Maximale vCores per database hoog genoeg is voor het afhandelen van pieken in Databasegebruik instellen. Enige mate van het doorvoeren van te veel wordt verwacht, omdat de groep in het algemeen uitgaat hot en cold gebruikspatronen voor databases waarin alle databases niet tegelijkertijd pieken.|
| Min vCores per database |Het minimum aantal vCores op dat elke database in de groep kan worden gegarandeerd. Het is een algemene instelling voor alle databases in de groep. De min-vCores per database kan worden ingesteld op 0, en is ook de standaardwaarde. Deze eigenschap is ingesteld op een willekeurige plaats tussen 0 en het gebruik van de gemiddelde vCores per database. Het product van het aantal databases in de groep en de min-vCores per database kan niet groter zijn dan de vCores per groep.|
| Maximale opslagruimte per database |De maximale databasegrootte ingesteld door de gebruiker voor een database in een pool. Databases in pools delen de poolopslag is toegewezen, zodat de grootte van een database kan bereiken beperkt tot is de kleinste hoeveelheid van het resterende storage pool en grootte van de database. Maximale databasegrootte verwijst naar de maximale grootte van de gegevensbestanden en exclusief de ruimte die logboekbestanden innemen. |
|||

## <a name="next-steps"></a>Volgende stappen

- Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie [overzicht van resource beperkt op een logische server](sql-database-resource-limits-logical-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
