---
title: Azure SQL-Database op basis van vCore limieten - elastische pools | Microsoft Docs
description: Deze pagina beschrijft een aantal algemene vCore gebaseerde limieten voor elastische pools in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 01f213c7cf5f6be3ef84601a50bb4455422faf22
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309916"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools-preview"></a>Azure SQL Database vCore gebaseerde aanschaffen van een model limieten voor elastische pools (preview)

Dit artikel bevat de limieten voor gedetailleerde voor Azure SQL Database elastische pools en gegroepeerde databases op basis van de vCore gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten, [resource op basis van SQL Database DTU - limieten voor elastische pools](sql-database-dtu-resource-limits-elastic-pools.md).


## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Elastische pool: opslaggrootte en prestatieniveaus

De volgende tabellen geven voor de elastische pools SQL-Database, de beschikbare bronnen op elke prijscategorie en prestatieniveau serviceniveau. U kunt instellen, de servicelaag, het prestatieniveau en de opslag met behulp van de mate waarin de [Azure-portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), wordt de [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), of de [REST-API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> De limieten van de afzonderlijke databases in een elastische pools zijn in het algemeen hetzelfde als voor individuele databases buiten heeft van hetzelfde prestatieniveau van toepassingen. Bijvoorbeeld, is het maximumaantal gelijktijdige werknemers voor een database GP_Gen4_1 200 werknemers. Het maximumaantal gelijktijdige werknemers voor een database in een pool GP_Gen4_1 is dus ook 200 werknemers. Opmerking: het totale aantal gelijktijdige werknemers in de groep GP_Gen4_1 is 210.

### <a name="general-purpose-service-tier"></a>Algemene doel servicelaag

#### <a name="generation-4-compute-platform"></a>Rekenplatform generatie 4
|Prestatieniveau|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generatie H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Geheugen (GB)|7|14|28|56|112|168|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|
|Maximale grootte van gegevens (GB)|512|756|1536|2048|3584|4096|
|Maximale logboekgrootte|154|227|461|614|1075|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Doel-IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|
|Maximum aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|3360|5040|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximum aantal toepassingen dichtheid|100|200|500|500|500|500|
|Minimale/maximale elastische pool Klik-stopt|0, 0,25, 0,5, 1|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Aantal replica's|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

#### <a name="generation-5-compute-platform"></a>Rekenplatform generatie 5
|Prestatieniveau|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Generatie H/W|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|11|22|44|88|132|176|220|440|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|
|Maximale grootte van gegevens (GB)|512|756|1536|2048|3072|4096|4096|4096|
|Maximale logboekgrootte|154|227|461|614|922|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Doel-IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|
|Maximum aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|2520|3360|4200|8400
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximum aantal toepassingen dichtheid|100|200|500|500|500|500|500|500|
|Minimale/maximale elastische pool Klik-stopt|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
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
|Maximum aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|3360|5040|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximum aantal toepassingen dichtheid|N/A|50|100|100|100|100|
|Minimale/maximale elastische pool Klik-stopt|N/A|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Aantal replica's|3|3|3|3|3|3|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

#### <a name="generation-5-compute-platform"></a>Rekenplatform generatie 5
|Prestatieniveau|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
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
|Maximum aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|2520|3360|5040|8400|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximum aantal toepassingen dichtheid|N/A|50|100|100|100|100|100|100|
|Minimale/maximale elastische pool Klik-stopt|N/A|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Aantal replica's|3|3|3|3|3|3|3|3|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

Als alle vCores van een pool voor elastische bezet zijn, krijgt elke database in de groep dezelfde hoeveelheid rekenresources voor het verwerken van query's. De service SQL Database verdeelt resources eerlijk over databases door gelijke hoeveelheden rekentijd te garanderen. Er is een elastische pool resource delen billijkheid naast elke gewenste hoeveelheid resource anders gegarandeerd in elke database wanneer de vCore min. per database is ingesteld op een andere waarde dan nul.

### <a name="database-properties-for-pooled-databases"></a>Database-eigenschappen voor gegroepeerde databases

De volgende tabel beschrijft de eigenschappen voor gegroepeerde databases.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Maximum aantal vCores per database |Het maximum aantal vCores die van elke database in de groep, gebruikmaken mogelijk indien beschikbaar op basis van gebruik door andere databases in de pool. Maximum aantal vCores per database is geen garantie resource voor een database. Het is een algemene instelling voor alle databases in de groep. Stel max vCores per database hoog genoeg is voor het afhandelen van pieken in het gebruik van de database. Enige mate van overtoewijzing is normaal, omdat de groep in het algemeen uitgaat van pieken en dalen in gebruikspatronen voor databases, waarbij alle databases niet tegelijkertijd pieken.|
| Min vCores per database |Het minimum aantal vCores die elke database in de groep kan worden gegarandeerd. Het is een algemene instelling voor alle databases in de groep. De vCores min. per database kan worden ingesteld op 0, en is ook de standaardwaarde. Deze eigenschap is ingesteld op een willekeurige plaats tussen 0 en het gebruik van de gemiddelde vCores per database. Het product van het aantal databases in de groep en de vCores min. per database kan niet groter zijn dan de vCores per groep.|
| Maximale opslag per database |De maximale databasegrootte ingesteld door de gebruiker voor een database in een pool. Gegroepeerde databases Groepsopslag wordt gedeeld door toegewezen, zodat de grootte van een database kan bereiken beperkt tot is de kleinste van de resterende servergroep van opslag en grootte van de database. Maximale grootte van de database verwijst naar de maximale grootte van de gegevensbestanden en omvat niet de ruimte die wordt gebruikt door logboekbestanden. |
|||
 
## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over de algemene Azure limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
