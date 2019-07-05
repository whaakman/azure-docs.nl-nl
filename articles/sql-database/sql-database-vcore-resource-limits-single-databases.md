---
title: Azure SQL Database vCore gebaseerde resourcelimieten - database met | Microsoft Docs
description: Deze pagina Beschrijving van enkele veelvoorkomende vCore gebaseerde resourcelimieten voor een individuele database in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/22/2019
ms.openlocfilehash: c89aa3b4ecf0c07cfbb579cdc18fac6e822bc047
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536233"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-based-purchasing-model"></a>Resourcelimieten voor individuele databases met behulp van de vCore-aanschafmodel

In dit artikel bevat de gedetailleerde resourcelimieten voor individuele databases van Azure SQL Database met behulp van het op vCore gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten voor individuele databases op een SQL-databaseserver, [overzicht van de resource op een SQL-databaseserver beperkt](sql-database-resource-limits-database-server.md).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

U kunt de servicelaag, rekencapaciteit en hoeveelheid opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), wordt de [ Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases), of de [REST-API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases).

> [!IMPORTANT]
> Zie voor richtlijnen en overwegingen bij schalen, [schalen van één database](sql-database-single-database-scale.md).

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Categorie voor algemeen gebruik-service: Opslaggrootte en compute-grootten

> [!IMPORTANT]
> Nieuwe Gen4 databases worden niet meer ondersteund in de regio AustraliaEast.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Categorie voor algemeen gebruik-service: Generatie 4 computerplatform (deel 1)

|COMPUTE-grootte|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|1|2|3|4|5|6|
|Geheugen (GB)|7|14|21|28|35|42|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|1024|1024|1024|1536|1536|1536|
|Maximale logboekgrootte (GB)|307|307|307|461|461|461|
|TempDB-grootte (GB)|32|64|96|128|160|192|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|500|1000|1500|2000|2500|3000|
|Meld u frequentielimieten (MBps)|3.75|7.5|11.25|15|18.75|22.5|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|600|800|1000|1200|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Categorie voor algemeen gebruik-service: Generatie 4 computerplatform (deel 2)

|COMPUTE-grootte|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|7|8|9|10|16|24|
|Geheugen (GB)|49|56|63|70|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|1536|3072|3072|3072|4096|4096|
|Maximale logboekgrootte (GB)|461|922|922|922|1229|1229|
|TempDB-grootte (GB)|224|256|288|320|384|384|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)
|Doel-IOPS (van 64 KB)|3500|4000|4500|5000|7000|7000|
|Meld u frequentielimieten (MBps)|26.25|30|30|30|30|30|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|1400|1600|1800|2000|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Categorie voor algemeen gebruik-service: Genereren van 5-computerplatform (deel 1)

|COMPUTE-grootte|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W genereren|5|5|5|5|5|5|5|
|vCores|2|4|6|8|10|12|14|
|Geheugen (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maximale logboekgrootte (GB)|307|307|307|461|461|461|461|
|TempDB-grootte (GB)|64|128|192|256|320|384|384|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|1000|2000|3000|4000|5000|6000|7000|
|Meld u frequentielimieten (MBps)|3.75|7.5|11.25|15|18.75|22.5|26.25|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|600|800|1000|1200|1400|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|1|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Categorie voor algemeen gebruik-service: Genereren van 5-computerplatform (deel 2)

|COMPUTE-grootte|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W genereren|5|5|5|5|5|5|5|
|vCores|16|18|20|24|32|40|80|
|Geheugen (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maximale logboekgrootte (GB)|922|922|922|1229|1229|1229|1229|
|TempDB-grootte (GB)|384|384|384|384|384|384|384|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|7000|7000|7000|7000|7000|7000|7000|
|Meld u frequentielimieten (MBps)|30|30|30|30|30|30|30|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|1600|1800|2000|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|1|1|1|1|1|1|1|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

### <a name="serverless-compute-tier"></a>Serverloze compute-laag

De [serverloze compute-laag](sql-database-serverless.md) is in preview en is alleen voor individuele databases met behulp van het aanschaffen van de vCore-model.

#### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5

|COMPUTE-grootte|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|
|:--- | --: |--: |--: |
|H/W genereren|5|5|5|
|Min-max vCores|0.5-1|0.5-2|0.5-4|
|Min-max-geheugen (GB)|2.02-3|2.05-6|2.10-12|
|Min automatisch onderbreken vertraging (uren)|6|6|6|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|
|Maximumgrootte van gegevens (GB)|512|1024|1024|
|Maximale logboekgrootte (GB)|12|24|48|
|TempDB-grootte (GB)|32|64|128|
|Opslagtype|Premium Storage op (extern)|Premium Storage op (extern)|Premium Storage op (extern)|
|I/o-latentie (bij benadering)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|5-7 ms (schrijven)<br>5-10 ms (lezen)|
|Doel-IOPS (van 64 KB)|500|1000|2000|
|Meld u frequentielimieten (MBps)|2.5|5.6|10|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|75|150|300|
|Maximaal toegestane sessies|30.000|30.000|30.000|
|Aantal replica 's|1|1|1|
|Multi-AZ|N/A|N/A|N/A|
|Uitschaling lezen|N/A|N/A|N/A|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

## <a name="business-critical-service-tier-for-provisioned-compute-tier"></a>Kritieke-bedrijfslaag voor de ingerichte Computing-laag

> [!IMPORTANT]
> Nieuwe Gen4 databases worden niet meer ondersteund in de regio AustraliaEast.

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
|Meld u frequentielimieten (MBps)|8|16|24|32|40|48|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|600|800|1000|1200|
|Maximaal aantal gelijktijdige aanmeldingen|200|400|600|800|1000|1200|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>Kritieke-bedrijfslaag: Generatie 4 computerplatform (deel 2)

|COMPUTE-grootte|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W genereren|4|4|4|4|4|4|
|vCores|7|8|9|10|16|24|
|Geheugen (GB)|49|56|63|70|112|168|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|7|8|9.5|11|20|36|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximumgrootte van gegevens (GB)|650|650|650|650|1024|1024|
|Maximale logboekgrootte (GB)|195|195|195|195|307|307|
|TempDB-grootte (GB)|224|256|288|320|384|384|
|I/o-latentie (bij benadering)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|1-2 ms (schrijven)<br>1-2 ms (lezen)|
|Doel-IOPS (van 64 KB)|35000|40000|45000|50000|80000|120000|
|Meld u frequentielimieten (MBps)|56|64|64|64|64|64|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|1400|1600|1800|2000|3200|4800|
|Maximaal aantal gelijktijdige aanmeldingen (aanvragen)|1400|1600|1800|2000|3200|4800|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>Kritieke-bedrijfslaag: Genereren van 5-computerplatform (deel 1)

|COMPUTE-grootte|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
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
|Doel-IOPS (van 64 KB)|8000|16000|24000|32000|40000|48000|56000|
|Meld u frequentielimieten (MBps)|12|24|36|48|60|72|84|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|600|800|1000|1200|1400|
|Maximaal aantal gelijktijdige aanmeldingen|200|400|600|800|1000|1200|1400|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|4|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>Kritieke-bedrijfslaag: Genereren van 5-computerplatform (deel 2)

|COMPUTE-grootte|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
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
|Doel-IOPS (van 64 KB)|64000|72000|80000|96000|128000|160000|320000|
|Meld u frequentielimieten (MBps)|96|96|96|96|96|96|96|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|1600|1800|2000|2400|3200|4000|8000|
|Maximaal aantal gelijktijdige aanmeldingen|1600|1800|2000|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|4|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|1 X-DB-grootte|

## <a name="hyperscale-service-tier"></a>Hyperscale-servicelaag

### <a name="generation-5-compute-platform"></a>Computerplatform generatie 5

|Prestatieniveau|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|H/W genereren|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Geheugen (GB)|10.2|20.4|40.8|81.6|122.4|163.2|204|408|
|Ondersteuning voor Columnstore|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-memory OLTP-opslag (GB)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Maximumgrootte van gegevens (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|Maximale logboekgrootte (TB)|1 |1 |1 |1 |1 |1 |1 |1 |
|TempDB-grootte (GB)|64|128|256|384|384|384|384|384|
|Opslagtype|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Doel-IOPS (van 64 KB)| [Opmerking 1](#note-1) |[Opmerking 1](#note-1)|[Opmerking 1](#note-1) |[Opmerking 1](#note-1) |[Opmerking 1](#note-1) |[Opmerking 1](#note-1) |[Opmerking 1](#note-1) | [Opmerking 1](#note-1) |
|I/o-latentie (bij benadering)|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|Vast te stellen|
|Maximaal aantal gelijktijdige werknemers (aanvragen)|200|400|800|1600|2400|3200|4000|8000|
|Maximaal toegestane sessies|30.000|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Aantal replica 's|2|2|2|2|2|2|2|2|
|Multi-AZ|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Uitschaling lezen|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Back-upopslag opgenomen |7|7|7|7|7|7|7|7|
|||

### <a name="note-1"></a>Opmerking 1

Zeer grootschalige is een architectuur met meerdere lagen met caching op meerdere niveaus. Effectieve IOP's afhankelijk van de werkbelasting.

### <a name="next-steps"></a>Volgende stappen

- Zie voor DTU resourcelimieten voor een individuele database, [resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel](sql-database-dtu-resource-limits-single-databases.md)
- Zie voor vCore resourcelimieten voor elastische pools, [resourcelimieten voor elastische pools met behulp van de vCore-aanschafmodel](sql-database-vcore-resource-limits-elastic-pools.md)
- Zie voor DTU resource-limieten voor elastische pools, [resourcelimieten voor elastische pools met behulp van het op DTU gebaseerde aankoopmodel](sql-database-dtu-resource-limits-elastic-pools.md)
- Zie voor resourcelimieten voor beheerde exemplaren, [beheerd exemplaar resourcelimieten](sql-database-managed-instance-resource-limits.md).
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over de resourcelimieten op een databaseserver [overzicht van resourcelimieten voor een SQL-databaseserver](sql-database-resource-limits-database-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.
