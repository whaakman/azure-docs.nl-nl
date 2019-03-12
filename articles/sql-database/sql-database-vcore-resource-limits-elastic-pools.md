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
ms.date: 03/06/2019
ms.openlocfilehash: a1e76aafa271ff021517c3d06c3c6e02103413d3
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57571470"
---
# <a name="resource-limits-for-elastic-pools-using-the-vcore-based-purchasing-model-limits"></a>Resourcelimieten voor elastische pools met behulp van de limieten aankopen model op basis van vCore

Dit artikel bevat de gedetailleerde resourcelimieten voor Azure SQL Database elastische pools en gepoolde databases met behulp van het op vCore gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten, [SQL Database DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

U kunt de servicelaag, rekencapaciteit en opslag met behulp van bedrag instellen de [Azure-portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), wordt de [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), of de [REST-API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!IMPORTANT]
> Zie voor richtlijnen en overwegingen bij schalen, [een elastische pool schalen](sql-database-elastic-pool-scale.md)
> [!NOTE]
> De resourcelimieten van afzonderlijke databases in elastische pools zijn algemeen hetzelfde als voor individuele databases buiten pools met dezelfde grootte berekenen. Bijvoorbeeld, is de maximale gelijktijdige werknemers voor een database GP_Gen4_1 200 werknemers. De maximale gelijktijdige werknemers voor een database in een pool GP_Gen4_1 is dus ook 200 werknemers. Opmerking: het totale aantal gelijktijdige werknemers in de groep GP_Gen4_1 is 210.

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Categorie voor algemeen gebruik-service: Opslaggrootte en compute-grootten

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Categorie voor algemeen gebruik-service: Generatie 4 computerplatform (deel 1)

|COMPUTE-grootte|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|3|4|5|6|
|Geheugen (GB)|7|14|21|28|35|42|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|512|756|756|1536|1536|1536|
|Maximale logboekgrootte|154|227|227|461|461|461|
|TempDB-grootte (GB)|32|64|96|128|160|192|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|500|1000|1500|2000|2500|3000|
|Meld u frequentielimieten (MBps)|2,5|5|7.5|10|12.5|15|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) * |210|420|630|840|1050|1260|
|Maximaal aantal gelijktijdige aanmeldingen per pool * |210|420|630|840|1050|1260|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|100|200|300|500|500|500|
|Min./Max. elastische pool vCore-opties per database|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Aantal replica's|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Categorie voor algemeen gebruik-service: Generatie 4 computerplatform (deel 2)

|COMPUTE-grootte|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|7|8|9|10|16|24|
|Geheugen (GB)|49|56|63|70|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|1536|2048|2048|2048|3584|4096|
|Maximale logboekgrootte (GB)|461|614|614|614|1075|1229|
|TempDB-grootte (GB)|224|256|288|320|384|384|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|3500|4000|4500|5000|7000|7000|
|Meld u frequentielimieten (MBps)|17.5|20|20|20|20|20|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) *|1470|1680|1890|2100|3360|5040|
|Maximum aantal gelijktijdige aanmeldingen pool (aanvragen) *|1470|1680|1890|2100|3360|5040|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|200|500|500|500|500|500|
|Min./Max. elastische pool vCore-opties per database|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Aantal replica's|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Categorie voor algemeen gebruik-service: Genereren van 5-computerplatform (deel 1)

|COMPUTE-grootte|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|
|vCores|2|4|6|8|10|12|14|
|Geheugen (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|512|756|756|1536|1536|1536|
|Maximale logboekgrootte (GB)|154|227|227|461|461|461|461|
|TempDB-grootte (GB)|64|128|192|256|320|384|384|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|500|1000|1500|2000|2500|3000|3500|
|Meld u frequentielimieten (MBps)|2,5|56|7.5|10|12.5|15|17.5|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) *|210|420|630|840|1050|1260|1470|
|Maximaal aantal gelijktijdige aanmeldingen per pool (aanvragen) *|210|420|630|840|1050|1260|1470|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|200|500|500|500|500|500|500|
|Min./Max. elastische pool vCore-opties per database|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Aantal replica's|1|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Categorie voor algemeen gebruik-service: Genereren van 5-computerplatform (deel 2)

|COMPUTE-grootte|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|
|vCores|16|18|20|24|32|40|80|
|Geheugen (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|2048|2048|3072|3072|4096|4096|4096|
|Maximale logboekgrootte (GB)|614|614|922|922|1229|1229|1229|
|TempDB-grootte (GB)|384|384|384|384|384|384|384|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|4000|4500|5000|6000|7000|7000|7000|
|Meld u frequentielimieten (MBps)|20|20|20|20|20|20|20|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) *|1680|1890|2100|2520|33600|4200|8400|
|Maximaal aantal gelijktijdige aanmeldingen per pool (aanvragen) *|1680|1890|2100|2520|33600|4200|8400|
|Maximaal aantal databases per pool|500|500|500|500|500|500|500|
|Min./Max. elastische pool vCore-opties per database|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...16, 24, 32, 40|0, 0.25, 0.5, 1...16, 24, 32, 40, 80|
|Aantal replica's|1|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>Kritieke-bedrijfslaag: Opslaggrootte en compute-grootten

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>Kritieke-bedrijfslaag: Generatie 4 computerplatform (deel 1)

|COMPUTE-grootte|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|3|4|5|6|
|Geheugen (GB)|7|14|21|28|35|42|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|1|2|3|4|5|6|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (GB)|650|650|650|650|650|650|
|Maximale logboekgrootte (GB)|195|195|195|195|195|195|
|TempDB-grootte (GB)|32|64|96|128|160|192|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Doel-IOPS (van 64 KB)|5000|10.000|15.000|20000|25000|30.000|
|Meld u frequentielimieten (MBps)|6|12|18|24|30|36|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) *|210|420|630|840|1050|1260|
|Maximaal aantal gelijktijdige aanmeldingen per pool (aanvragen) *|210|420|630|840|1050|1260|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|Slechts één DB's worden ondersteund voor deze compute-grootte|50|100|100|100|100|
|Min./Max. elastische pool vCore-opties per database|N/A|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Aantal replica's|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>Kritieke-bedrijfslaag: Generatie 4 computerplatform (deel 2)

|COMPUTE-grootte|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|7|8|9|10|16|24|
|Geheugen (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Ondersteuning voor Columnstore|N/A|N/A|N/A|N/A|N/A|N/A|
|In-memory OLTP-opslag (GB)|7|8|9.5|11|20|36|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (GB)|650|650|650|650|1024|1024|
|Maximale logboekgrootte (GB)|195|195|195|195|307|307|
|TempDB-grootte (GB)|224|256|288|320|384|384|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Doel-IOPS (van 64 KB)|35000|40000|45000|50000|80000|120000|
|Meld u frequentielimieten (MBps)|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) *|1470|1680|1890|2100|3360|5040|
|Maximaal aantal gelijktijdige aanmeldingen per pool (aanvragen) *|1470|1680|1890|2100|3360|5040|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|100|100|100|100|100|100|
|Min./Max. elastische pool vCore-opties per database|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Aantal replica's|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>Kritieke-bedrijfslaag: Genereren van 5-computerplatform (deel 1)

|COMPUTE-grootte|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|
|vCores|2|4|6|8|10|12|14|
|Geheugen (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|1.571|3.142|4.713|6.284|8.655|11.026|13.397|
|Maximumgrootte van gegevens (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maximale logboekgrootte (GB)|307|307|307|461|461|922|922|
|TempDB-grootte (GB)|64|128|192|256|320|384|384|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Doel-IOPS (van 64 KB)|5000|10.000|15.000|20000|25000|30.000|35000|
|Meld u frequentielimieten (MBps)|48|48|48|48|48|48|48|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) *|210|420|630|840|1050|1260|1470|
|Maximaal aantal gelijktijdige aanmeldingen per pool (aanvragen) *|210|420|630|840|1050|1260|1470|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|Slechts één DB's worden ondersteund voor deze compute-grootte|50|100|100|100|100|100|
|Min./Max. elastische pool vCore-opties per database|N/A|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Aantal replica's|4|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>Kritieke-bedrijfslaag: Genereren van 5-computerplatform (deel 2)

|COMPUTE-grootte|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|
|vCores|16|18|20|24|32|40|80|
|Geheugen (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|15.768|18.139|20.51|25.252|37.936|52.22|131.64|
|Maximumgrootte van gegevens (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maximale logboekgrootte (GB)|922|922|922|1229|1229|1229|1229|
|TempDB-grootte (GB)|384|384|384|384|384|384|384|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Doel-IOPS (van 64 KB)|40000|45000|50000|60000|80000|100000|200000|
|Meld u frequentielimieten (MBps)|48|48|48|48|48|48|48|
|Maximaal aantal gelijktijdige werknemers per pool (aanvragen) *|1680|1890|2100|2520|3360|4200|8400|
|Maximaal aantal gelijktijdige aanmeldingen per pool (aanvragen) *|1680|1890|2100|2520|3360|4200|8400|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Maximaal aantal databases per pool|100|100|100|100|100|100|100|
|Min./Max. elastische pool vCore-opties per database|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...20, 24, 32, 40|0, 0.25, 0.5, 1...20, 24, 32, 40, 80|
|Aantal replica's|4|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

\* Zie voor de maximale gelijktijdige werkers (aanvragen) voor een individuele database, [één database-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md). Bijvoorbeeld, als de elastische groep is met Gen5 en de maximale vCore per database is 2, dan is de maximale gelijktijdige werknemers 200.  Als maximale vCore per database is 0,5, klikt u vervolgens de maximale gelijktijdige werknemers is 50 omdat op Gen5 er zijn maximaal 100 gelijktijdige werknemers per vcore.  Voor andere maximale vCore-instellingen per database die minder 1 vCore zijn of minder, het aantal gelijktijdige werknemers max wordt op dezelfde manier schaal gebracht weglating.

Als alle vCores van een elastische pool bezet zijn, ontvangt elke database in de groep een gelijke hoeveelheid rekenresources voor het verwerken van query's. De service SQL Database verdeelt resources eerlijk over databases door gelijke hoeveelheden rekentijd te garanderen. Er is een elastische pool resources eerlijk naast een bepaalde hoeveelheid van de resource wordt aan elke database wordt gegarandeerd wanneer het vCore-minimum per database is ingesteld op een andere waarde dan nul.

## <a name="database-properties-for-pooled-databases"></a>Database-eigenschappen voor databases in pools

De volgende tabel beschrijft de eigenschappen voor databases in pools.

| Eigenschap | Description |
|:--- |:--- |
| Maximum aantal vCores per database |Het maximale aantal vCores op dat elke database in de groep gebruiken mag, beschikbaar op basis van gebruik door andere databases in de groep. Maximum aantal vCores per database is geen resourcegarantie voor een database. Het is een algemene instelling voor alle databases in de groep. Maximale vCores per database hoog genoeg is voor het afhandelen van pieken in Databasegebruik instellen. Enige mate van het doorvoeren van te veel wordt verwacht, omdat de groep in het algemeen uitgaat hot en cold gebruikspatronen voor databases waarin alle databases niet tegelijkertijd pieken.|
| Min vCores per database |Het minimum aantal vCores op dat elke database in de groep kan worden gegarandeerd. Het is een algemene instelling voor alle databases in de groep. De min-vCores per database kan worden ingesteld op 0, en is ook de standaardwaarde. Deze eigenschap is ingesteld op een willekeurige plaats tussen 0 en het gebruik van de gemiddelde vCores per database. Het product van het aantal databases in de groep en de min-vCores per database kan niet groter zijn dan de vCores per groep.|
| Maximale opslagruimte per database |De maximale databasegrootte ingesteld door de gebruiker voor een database in een pool. Databases in pools delen de poolopslag is toegewezen, zodat de grootte van een database kan bereiken beperkt tot is de kleinste hoeveelheid van het resterende storage pool en grootte van de database. Maximale databasegrootte verwijst naar de maximale grootte van de gegevensbestanden en exclusief de ruimte die logboekbestanden innemen. |
|||

## <a name="next-steps"></a>Volgende stappen

- Zie voor vCore resourcelimieten voor een individuele database, [resourcelimieten voor individuele databases met behulp van de vCore-aanschafmodel](sql-database-vcore-resource-limits-single-databases.md)
- Zie voor DTU resourcelimieten voor een individuele database, [resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel](sql-database-dtu-resource-limits-single-databases.md)
- Zie voor DTU resource-limieten voor elastische pools, [resourcelimieten voor elastische pools met behulp van het op DTU gebaseerde aankoopmodel](sql-database-dtu-resource-limits-elastic-pools.md)
- Zie voor resourcelimieten voor beheerde exemplaren, [beheerd exemplaar resourcelimieten](sql-database-managed-instance-resource-limits.md).
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over de resourcelimieten op een databaseserver [overzicht van resourcelimieten voor een SQL-databaseserver](sql-database-resource-limits-database-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.
