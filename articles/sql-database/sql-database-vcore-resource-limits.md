---
title: Azure SQL-Database op basis van vCore limieten | Microsoft Docs
description: Deze pagina beschrijft een aantal algemene vCore gebaseerde limieten voor Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: 23bab643a88fe27eb34750f970f962041f8c18f4
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-preview"></a>Azure SQL Database vCore gebaseerde aanschaffen van een model limieten (preview)

> [!IMPORTANT]
> Zie voor DTU gebaseerde aankopen model limieten, [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Individuele database: opslaggrootte en prestatieniveaus

De volgende tabellen tonen voor individuele databases, de beschikbare resources voor één database op elke prijscategorie en prestatieniveau serviceniveau. U kunt de servicelaag, het prestatieniveau en de opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), de [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli), of de [REST-API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api).

### <a name="general-purpose-service-tier"></a>Algemene doel servicelaag
|Prestatieniveau|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Generatie H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Geheugen (GB)|7|14|28|56|112|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|
|Maximale grootte van gegevens (GB)|1024|1024|1536|3072|4096|
|Maximale logboekgrootte|307|307|461|922|1229|
|TempDB size(DB)|32|64|128|256|384|
|Doel-IOPS|320|640|1280|2560|5120|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)
|Maximum aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|
|Maximaal aantal gelijktijdige aanmeldingen|200|400|800|1600|3200|
|Maximaal toegestane sessies|3000|3000|3000|3000|3000|
|Aantal replica 's|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

### <a name="business-critical-service-tier"></a>Kritieke service bedrijfslaag
|Prestatieniveau|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Generatie H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Geheugen (GB)|7|14|28|56|112|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|1|2|4|8|20|
|Opslagtype|Gekoppelde SSD|Gekoppelde SSD|Gekoppelde SSD|Gekoppelde SSD|Gekoppelde SSD|
|Maximale grootte van gegevens (GB)|1024|1024|1024|1024|1024|
|Maximale logboekgrootte|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|
|Doel-IOPS|5000|10.000|20000|40000|80000|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|
|Maximum aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|3200|
|Maximaal aantal gelijktijdige aanmeldingen|200|400|800|1600|3200|
|Maximaal toegestane sessies|3000|3000|3000|3000|3000|
|Aantal replica 's|3|3|3|3|3|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|
|Scale-out lezen|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

## <a name="single-database-change-storage-size"></a>Individuele database: opslaggrootte wijzigen

- Tot de maximale grootte bereikt met stappen van 1 GB worden opslagruimte ingericht. De minimale configureerbare gegevensopslag is 5GB 
- Opslag voor één database kan worden bevoorraad door vergroten of verkleinen van het gebruik van de maximale grootte van de [Azure-portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update).
- SQL-Database wijst automatisch 30% van extra opslagruimte voor de logboekbestanden en 32GB per vCore voor TempDB, maar niet meer dan 384GB. TempDB bevindt zich op een gekoppelde SSD in alle Servicelagen.
- De prijs van opslag voor één database is de som van de gegevens opslag- en logboekbestanden opslag bedragen vermenigvuldigd met de eenheidsprijs opslag van de servicetier. De kosten van TempDB is opgenomen in de prijs vCore. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-vcores"></a>Individuele database: vCores wijzigen

Nadat u in eerste instantie het aantal vCores, u kunt een individuele database omhoog of omlaag schalen dynamisch op basis van het werkelijke ervaring met behulp van de [Azure-portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update). 

Als u de servicelaag en/of het prestatieniveau van een database wijzigt, wordt er een replica van de oorspronkelijke database gemaakt op het nieuwe prestatieniveau en worden vervolgens de verbindingen overgeschakeld op de replica. Er gaan geen gegevens verloren tijdens dit proces, maar tijdens het korte moment waarop we overschakelen naar de replica, worden verbindingen met de database uitgeschakeld, zodat sommige actieve transacties kunnen worden teruggedraaid. De tijdsduur voor de overgeschakeld verschilt, maar wordt doorgaans onder 4 seconden is minder dan 30 seconden 99% van de tijd. Als er grote aantallen transacties tijdens de vlucht op het moment dat verbindingen zijn uitgeschakeld, wordt de tijdsduur voor de overgeschakeld mogelijk ook langer. 

De duur van het volledige proces voor omhoog schalen is afhankelijk van de grootte en de servicelaag van de database vóór en na de wijziging. Bijvoorbeeld, moet een 250 GB-database die wordt gewijzigd naar, uit of binnen een servicelaag voor algemene doeleinden uitvoeren binnen zes uur. Voor een database dezelfde grootte hebben prestatieniveaus binnen de kritieke zakelijke servicelaag wordt gewijzigd, moet de scale-up uitvoeren binnen drie uur.

> [!TIP]
> Voor het bewaken van bewerkingen in-nu wordt uitgevoerd, Zie: [bewerkingen met de REST-API van SQL beheren](/rest/api/sql/Operations/List), [beheren met CLI](/cli/azure/sql/db/op), [bewaak de bewerkingen met T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) en deze twee PowerShell-opdrachten: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) en [Stop AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Als u een naar een hogere laag of prestaties serviceniveau upgrade, wordt de maximale grootte van de database niet verhogen tenzij u expliciet een groter formaat (maxsize) opgeeft.
* Om het downgraden van een database, moet de ruimte van de database die wordt gebruikt kleiner zijn dan de maximaal toegestane grootte van het doel prijscategorie en prestatieniveau serviceniveau. 
* Bij een upgrade van een database met [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de secundaire databases upgraden naar de gewenste prestatielaag vóór de upgrade van de primaire database (algemene richtlijnen voor de beste prestaties). Bij een upgrade naar een andere, is upgraden van de secundaire database eerst vereist.
* Wanneer een database met downgraden [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de primaire databases naar de gewenste prestatielaag downgraden voordat het downgraden van de secundaire database (algemene richtlijnen voor de beste prestaties). Wanneer het downgraden naar een andere editie, is de primaire database downgraden eerst vereist.
* De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Elastische pool: opslaggrootte en prestatieniveaus

De volgende tabellen geven voor de elastische pools SQL-Database, de beschikbare bronnen op elke prijscategorie en prestatieniveau serviceniveau. U kunt instellen, de servicelaag, het prestatieniveau en de opslag met behulp van de mate waarin de [Azure-portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), wordt de [Azure CLI](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli), of de [REST-API](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api).

> [!NOTE]
> De limieten van de afzonderlijke databases in een elastische pools zijn in het algemeen hetzelfde als voor individuele databases buiten heeft van hetzelfde prestatieniveau van toepassingen. Bijvoorbeeld, is het maximumaantal gelijktijdige werknemers voor een database GP_Gen4_1 200 werknemers. Het maximumaantal gelijktijdige werknemers voor een database in een pool GP_Gen4_1 is dus ook 200 werknemers. Opmerking: het totale aantal gelijktijdige werknemers in de groep GP_Gen4_1 is 210.

### <a name="general-purpose-service-tier"></a>Algemene doel servicelaag
|Prestatieniveau|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Generatie H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Geheugen (GB)|7|14|28|56|112|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|
|Opslagtype|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|(Externe) Premium-opslag|
|Maximale grootte van gegevens (GB)|512|756|1536|2048|3584|
|Maximale logboekgrootte|154|227|461|614|1075|
|TempDB size(DB)|32|64|128|256|384|
|Doel-IOPS|320|640|1280|2560|5120|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|5-7 ms (schrijven)<br>5-10 ms (gelezen)|
|Maximum aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|3360|
|Maximaal aantal gelijktijdige aanmeldingen|210|420|840|1680|3360|
|Maximaal toegestane sessies|3000|3000|3000|3000|3000|
|Maximum aantal toepassingen dichtheid|100|200|500|500|500|
|Minimale/maximale elastische pool Klik-stopt|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1, 2, 4|0, 0.25, 0.5, 1, 2, 4, 8|0, 0.25, 0.5, 1, 2, 4, 8, 16|
|Aantal replica 's|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|
|Scale-out lezen|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
|||

### <a name="business-critical-service-tier"></a>Kritieke service bedrijfslaag
|Prestatieniveau|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Generatie H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Geheugen (GB)|7|14|28|56|112|
|Columnstore-ondersteuning|Ja|Ja|Ja|Ja|Ja|
|In het geheugen OLTP-opslag (GB)|1|2|4|8|20|
|Opslagtype|Gekoppelde SSD|Gekoppelde SSD|Gekoppelde SSD|Gekoppelde SSD|Gekoppelde SSD|
|Maximale grootte van gegevens (GB)|1024|1024|1024|1024|1024|
|Maximale logboekgrootte|307|307|307|461|614|
|TempDB size(DB)|32|64|128|256|384|
|Doel-IOPS|320|640|1280|2560|5120|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|1-2 ms (schrijven)<br>1-2-ms (gelezen)|
|Maximum aantal gelijktijdige werknemers (aanvragen)|210|420|840|1680|3360|
|Maximaal aantal gelijktijdige aanmeldingen|210|420|840|1680|3360|
|Maximaal toegestane sessies|3000|3000|3000|3000|3000|
|Maximum aantal toepassingen dichtheid|N/A|50|100|100|100|
|Minimale/maximale elastische pool Klik-stopt|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1, 2, 4|0, 0.25, 0.5, 1, 2, 4, 8|0, 0.25, 0.5, 1, 2, 4, 8, 16|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|
|Scale-out lezen|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|De grootte van 1 X-DB|
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
 
## <a name="elastic-pool-change-storage-size"></a>Elastische pool: opslaggrootte wijzigen

- Opslagruimte worden ingericht tot de maximale grootte bereikt: 
 - Voor Standard-opslag vergroten of verkleinen in stappen van 10 GB
 - Voor Premium-opslag vergroten of verkleinen in stappen van 250 GB
- Opslag voor een elastische groep kan worden bevoorraad door vergroten of verkleinen van het gebruik van de maximale grootte van de [Azure-portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), of de [ REST-API](/rest/api/sql/elasticpools/update).
- De prijs van opslag voor een elastische groep is de opslaghoeveelheid vermenigvuldigd met de eenheidsprijs opslag van de servicetier. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="elastic-pool-change-vcores"></a>Elastische pool: vCores wijzigen

U kunt vergroten of verkleinen van het prestatieniveau voor een elastische pool op basis van de resource die behoeften met behulp van de [Azure-portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), of de [REST-API](/rest/api/sql/elasticpools/update).

- Wanneer schaling aanpassen vCores groep, worden de databaseverbindingen kort verwijderd. Dit is hetzelfde gedrag als deze gebeurtenis treedt op wanneer schaling dtu's aanpassen voor een individuele database (niet in een groep). Zie voor meer informatie over de duur en de gevolgen van het verbroken verbindingen voor een database tijdens bewerkingen schaling aanpassen [dtu's schaling aanpassen voor een individuele database](#single-database-change-storage-size). 
- De duur op de oorspronkelijke groep vCores kan afhankelijk van de totale hoeveelheid opslagruimte die wordt gebruikt door alle databases in de groep. In het algemeen de rescaling latentie gemiddelde 90 minuten of minder per 100 GB. Bijvoorbeeld, als de totale ruimte gebruikt door alle databases in de pool is 200 GB dan drie uur is de verwachte latentie voor schaling aanpassen van de groep of minder. In sommige gevallen binnen de categorie Standard of Basic mag de rescaling latentie onder vijf minuten ongeacht de hoeveelheid ruimte die wordt gebruikt.
- De duur voor het wijzigen van de vCores min. per database of max vCores per database is in het algemeen vijf minuten of minder.
- Wanneer toepassingen vCores downsizing, moet de gebruikte ruimte kleiner zijn dan de maximaal toegestane grootte van de doel-service tier en de pool vCores.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Wat is het maximum aantal servers en databases?

| Maximum | Waarde |
| :--- | :--- |
| Databases per server | 5000 |
| Aantal servers per abonnement per regio | 20 |
|||

> [!IMPORTANT]
> Als het aantal databases de limiet is per server nadert, kan het volgende gebeuren:
> <br> • Verhogen latentie in met het uitvoeren van query's in de database master.  Het gaat hierbij om weergaven van de gebruiksstatistieken resource zoals sys.resource_stats.
> <br> • Latentie in beheerbewerkingen vergroten en opbouwen van de portal standpunten die betrekking hebben op inventariseren van de server-databases.

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>Wat gebeurt er wanneer de database en limieten elastische pool wordt bereikt?

### <a name="compute-vcores"></a>COMPUTE (vCores)

Wanneer database compute-gebruik (gemeten door vCore gebruik) toeneemt, query uitvoeren op de toename in latentie en kan zelfs time-out. In deze omstandigheden query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource komen.
Wanneer zich voordoen tijdens een hoge compute-gebruik, wordt een risicobeperking opties zijn onder andere:

- Het prestatieniveau van de database of de elastische groep om te voorzien van de database meer vCores te vergroten. Zie [één database: wijzigen cVcores](#single-database-change-vcores) en [elastische pool: wijzigen vCores](#elastic-pool-change-vcores).
- Query's te verlagen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wanneer databaseruimte gebruikt de maximale limiet bereikt, database worden ingevoegd en updates die Verhoog de gegevensgrootte niet als clients ontvangen een [foutbericht](sql-database-develop-error-messages.md). Database selecteert en VERWIJDERINGEN blijven slagen.

Wanneer zich voordoen tijdens een hoge ruimtegebruik, wordt een risicobeperking opties zijn onder andere:

- De maximale grootte van de database of elastische pool, of wijzig het prestatieniveau te verhogen van de maximale opslag verhogen. Zie [SQL-Database op basis van vCore limieten](sql-database-vcore-resource-limits.md).
- Als de database zich in een elastische groep, kan klikt u vervolgens ook de database worden verplaatst buiten de groep zodat de opslagruimte wordt niet gedeeld met andere databases.

### <a name="sessions-and-workers-requests"></a>Sessies en werknemers (aanvragen) 

Het maximum aantal sessies en werknemers worden bepaald door de prijscategorie en prestatieniveau serviceniveau. Nieuwe aanvragen worden geweigerd als sessie- of worker limieten zijn bereikt en clients een foutbericht weergegeven wordt. Terwijl het aantal verbindingen beschikbaar kan worden beheerd door de toepassing, is het aantal gelijktijdige werknemers vaak moeilijker te schatten en te beheren. Dit is vooral van toepassing tijdens de piekuren van load wanneer database limieten zijn bereikt en werknemers opstapelen vanwege langer actieve query's. 

Wanneer zich voordoen tijdens een hoog gebruik van sessie- of worker, wordt een risicobeperking opties zijn onder andere:
- Het serviceniveau voor laag of de prestaties van de database of elastische pool te vergroten. Zie [één database: opslaggrootte wijzigen](#single-database-change-storage-size), [één database: wijzigen vCores](#single-database-change-vcores), [elastische pool: opslaggrootte wijzigen](#elastic-pool-change-storage-size), en [elastische pool: vCores wijzigen ](#elastic-pool-change-vcores).
- Optimaliseren query's te verlagen van het Resourcegebruik van elke query als de oorzaak van het gebruik van verhoogde worker vanwege conflicten over berekenen bronnen. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over de algemene Azure limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
