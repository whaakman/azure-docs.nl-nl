---
title: Azure SQL-Database op basis van vCore limieten - individuele database | Microsoft Docs
description: Deze pagina beschrijft een aantal algemene vCore gebaseerde limieten voor een individuele database in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 1a14e1a7c50f458067491a8605a0518056ac0aa8
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309895"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database-preview"></a>Azure SQL Database vCore gebaseerde aanschaffen van een model limieten voor een individuele database (preview)

Dit artikel bevat de gedetailleerde limieten voor individuele databases van Azure SQL Database met behulp van de vCore gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten, [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Individuele database: opslaggrootte en prestatieniveaus

De volgende tabellen tonen voor individuele databases, de beschikbare resources voor één database op elke prijscategorie en prestatieniveau serviceniveau. U kunt de servicelaag, het prestatieniveau en de opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-servers-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases), de [Azure CLI](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases), of de [REST-API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Algemene doel servicelaag

#### <a name="generation-4-compute-platform"></a>Rekenplatform generatie 4
|Prestatieniveau|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Generatie H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|
|Maximale grootte van gegevens (GB)|1024|1024|1536|3072|4096|4096|
|Maximale logboekgrootte|307|307|461|922|1229|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Doel-IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|Maximum aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|000
|Scale-out lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

#### <a name="generation-5-compute-platform"></a>Rekenplatform generatie 5
|Prestatieniveau|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|Generatie H/W|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|
|Maximale grootte van gegevens (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Maximale logboekgrootte|307|307|461|614|1229|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Maximum aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|1|1|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

### <a name="business-critical-service-tier"></a>Kritieke service bedrijfslaag

#### <a name="generation-4-compute-platform"></a>Rekenplatform generatie 4
|Prestatieniveau|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generatie H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|1|2|4|8|20|36|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale grootte van gegevens (GB)|1024|1024|1024|1024|1024|1024|
|Maximale logboekgrootte|307|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|384|
|Doel-IOPS (64 KB)|5000|10.000|20000|40000|80000|120000|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|
|Maximum aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|3|3|3|3|3|3|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

#### <a name="generation-5-compute-platform"></a>Rekenplatform generatie 5
|Prestatieniveau|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Generatie H/W|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|
|Maximale grootte van gegevens (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maximale logboekgrootte|307|307|307|307|614|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (64 KB)|5000|10.000|20000|40000|60000|80000|100000|200000
|Maximum aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica's|3|3|3|3|3|3|3|3|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over de algemene Azure limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
