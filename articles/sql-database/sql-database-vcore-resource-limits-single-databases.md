---
title: Azure SQL Database vCore gebaseerde resourcelimieten - database met | Microsoft Docs
description: Deze pagina Beschrijving van enkele veelvoorkomende vCore gebaseerde resourcelimieten voor een individuele database in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/30/2018
ms.author: carlrab
ms.openlocfilehash: 350e472a4c17ed878d8506b4093bcd44ad60f469
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288018"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Azure SQL Database vCore gebaseerde model limieten voor één database aanschaffen

In dit artikel bevat de gedetailleerde resourcelimieten voor individuele databases van Azure SQL Database met behulp van het op vCore gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten, [SQL Database DTU gebaseerde resourcelimieten](sql-database-dtu-resource-limits.md).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).


## <a name="single-database-storage-sizes-and-performance-levels"></a>Individuele database: opslaggrootte en prestatieniveaus

De volgende tabellen ziet voor individuele databases, de beschikbare resources voor een individuele database op elke servicelaag en het prestatieniveau van een serviceniveau. U kunt de servicelaag, het prestatieniveau en de hoeveelheid opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), de [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases), of de [REST-API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Categorie van de service Algemeen gebruik

#### <a name="generation-4-compute-platform"></a>Computerplatform generatie 4
|Prestatieniveau|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
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
|TempDB-grootte (MB)|32|64|128|256|384|384|
|Doel-IOPS (van 64 KB)|500|1000|2000|4000|7000|7000|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|1|1|1|1|1|1|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|000
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

#### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5
|Prestatieniveau|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
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
|TempDB-grootte (MB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (van 64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|1|1|1|1|1|1|1|1|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

### <a name="business-critical-service-tier"></a>Kritieke-bedrijfslaag

#### <a name="generation-4-compute-platform"></a>Computerplatform generatie 4
|Prestatieniveau|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|1|2|4|8|20|36|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (GB)|1024|1024|1024|1024|1024|1024|
|Maximale logboekgrootte (GB)|307|307|307|307|307|307|
|TempDB-grootte (MB)|32|64|128|256|384|384|
|Doel-IOPS (van 64 KB)|5000|10.000|20000|40000|80000|120000|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|3|3|3|3|3|3|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

#### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5
|Prestatieniveau|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
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
|TempDB-grootte (MB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (van 64 KB)|5000|10.000|20000|40000|60000|80000|100000|200000
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|3|3|3|3|3|3|3|3|
|Multi-z|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|
|||

## <a name="next-steps"></a>Volgende stappen

- Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
