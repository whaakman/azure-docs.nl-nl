---
title: Azure SQL Database vCore gebaseerde resourcelimieten - database met | Microsoft Docs
description: Deze pagina Beschrijving van enkele veelvoorkomende vCore gebaseerde resourcelimieten voor een individuele database in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 5f0e5de7503d06d1aff319434d763d3b034053b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166351"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Azure SQL Database vCore gebaseerde model limieten voor één database aanschaffen

In dit artikel bevat de gedetailleerde resourcelimieten voor individuele databases van Azure SQL Database met behulp van het op vCore gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten, [SQL Database DTU gebaseerde resourcelimieten](sql-database-dtu-resource-limits.md).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).


## <a name="single-database-storage-sizes-and-compute-sizes"></a>Individuele database: opslaggrootte en compute-grootten

Voor individuele databases in de volgende tabellen tonen de beschikbare resources voor een individuele database op elke servicelaag en compute grootte. U kunt de servicelaag, rekencapaciteit en hoeveelheid opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), wordt de [ Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases), of de [REST-API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Categorie van de service Algemeen gebruik

#### <a name="generation-4-compute-platform"></a>Computerplatform generatie 4
|COMPUTE-grootte|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Maximumgrootte van gegevens (GB)|1024|1024|1536|3072|4096|4096|
|Maximale logboekgrootte (GB)|307|307|461|922|1229|1229|
|TempDB-grootte (GB)|32|64|128|256|384|384|
|Doel-IOPS (van 64 KB)|500|1000|2000|4000|7000|7000|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|1|1|1|1|1|1|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|000
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

#### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5
|COMPUTE-grootte|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Maximumgrootte van gegevens (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Maximale logboekgrootte (GB)|307|307|461|614|1229|1229|1229|1229|
|TempDB-grootte (GB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (van 64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|1|1|1|1|1|1|1|1|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

### <a name="business-critical-service-tier"></a>Kritieke-bedrijfslaag

#### <a name="generation-4-compute-platform"></a>Computerplatform generatie 4
|COMPUTE-grootte|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Ondersteuning voor Columnstore|N/A|N/A|N/A|N/A|N/A|N/A|
|In-memory OLTP-opslag (GB)|1|2|4|8|20|36|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (GB)|1024|1024|1024|1024|1024|1024|
|Maximale logboekgrootte (GB)|307|307|307|307|307|307|
|TempDB-grootte (GB)|32|64|128|256|384|384|
|Doel-IOPS (van 64 KB)|5000|10.000|20000|40000|80000|120000|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|3|3|3|3|3|3|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

#### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5
|COMPUTE-grootte|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Maximumgrootte van gegevens (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maximale logboekgrootte (GB)|307|307|307|307|614|1229|1229|1229|
|TempDB-grootte (GB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (van 64 KB)|5000|10.000|20000|40000|60000|80000|100000|200000
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|3|3|3|3|3|3|3|3|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

### <a name="hyperscale-service-tier-preview"></a>Zeer grootschalige servicelaag (preview)

#### <a name="generation-4-compute-platform"></a>Computerplatform generatie 4
|Prestatieniveau|HS_Gen4_1|HS_Gen4_2|HS_Gen4_4|HS_Gen4_8|HS_Gen4_16|HS_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (TB)|100 |100 |100 |100 |100 |100 |
|Maximale logboekgrootte (TB)|1 |1 |1 |1 |1 |1 |
|TempDB-grootte (GB)|32|64|128|256|384|384|
|Doel-IOPS (van 64 KB)|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|
|I/o-latentie (bij benadering)|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|2|2|2|2|2|2|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|7|7|7|7|7|7|
|||
### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5
|Prestatieniveau|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|Maximale logboekgrootte (TB)|1 |1 |1 |1 |1 |1 |1 |1 |
|TempDB-grootte (GB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (van 64 KB)|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|
|I/o-latentie (bij benadering)|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|2|2|2|2|2|2|2|2|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Opgenomen back-upopslag (limiet voor Preview-versie)|7|7|7|7|7|7|7|7|
|||

## <a name="next-steps"></a>Volgende stappen

- Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
