<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->
 
### <a name="basic-elastic-pool-limits"></a>Basisbeperkingen voor elastische groepen

| Poolgrootte (eDTU's)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Maximale gegevensopslag per pool* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Maximale OLTP-opslag in het geheugen per pool | N.v.t. | N.v.t. | N.v.t. | N.v.t. | N.v.t. | N.v.t. | N.v.t. | N.v.t. |
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 |30.000 | 30.000 | 30.000 | 30.000 |
| Minimaal aantal eDTU’s per database | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Maximaal aantal eDTU’s per database | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maximale gegevensopslag per database | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standaardbeperkingen voor elastische pools

| Poolgrootte (eDTU's)  | **50** | **100** | **200**** | **300**** | **400**** | **800****| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Maximale gegevensopslag per pool* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Maximale OLTP-opslag in het geheugen per pool | N.v.t. | N.v.t. | N.v.t. | N.v.t. | N.v.t. | N.v.t. | 
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Minimaal aantal eDTU’s per database** | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Maximaal aantal eDTU’s per database** | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Maximale gegevensopslag per database | 50 GB | 100 GB | 200 GB | 250 GB | 250 GB | 250 GB |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limieten voor Standard elastische pools (vervolg) 

| Poolgrootte (eDTU's)  |  **1200**** | **1600**** | **2000**** | **2500**** | **3000**** |
|:---|---:|---:|---:| ---: | ---: |
| Maximale gegevensopslag per pool* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Maximale OLTP-opslag in het geheugen per pool | N.v.t. | N.v.t. | N.v.t. | N.v.t. | N.v.t. | 
| Maximaal aantal databases per pool | 500 | 500 | 500 | 500 | 500 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige aanmeldingen per pool |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database** | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Maximaal aantal eDTU’s per database** | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Maximale gegevensopslag per database | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Limieten voor Premium elastische pools

| Poolgrootte (eDTU's)  | **125** | **250** | **500** | **1000** | **1500*****| 
|:---|---:|---:|---:| ---: | ---: | 
| Maximale gegevensopslag per pool* | 250 GB | 500 GB | 750 GB | 1 TB | 1,5 TB | 
| Maximale OLTP-opslag in het geheugen per pool | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Maximaal aantal databases per pool | 50 | 100 | 100 | 100 | 100 |  
| Maximaal aantal gelijktijdige werknemers per pool (aanvragen) | 200 | 400 | 800 | 1600 |  2400 | 
| Maximaal aantal gelijktijdige aanmeldingen per pool | 200 | 400 | 800 | 1600 |  2400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000 | 
| Maximaal aantal eDTU’s per database | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000 |
| Maximale gegevensopslag per database | 250 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limieten voor Premium elastische pools (vervolg) 

| Poolgrootte (eDTU's) | **2000***** | **2500***** | **3000***** | **3500***** | **4000*****|
|:---|---:|---:|---:| ---: | ---: | 
| Maximale gegevensopslag per pool* | 2 TB | 2,5 TB | 3 TB | 3,5 TB | 4 TB |
| Maximale OLTP-opslag in het geheugen per pool | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Maximaal aantal databases per pool | 100 | 100 | 100 | 100 | 100 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige aanmeldingen per pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 |  0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maximaal aantal eDTU’s per database | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maximale gegevensopslag per database | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Limieten voor Premium RS elastische pools

| Poolgrootte (eDTU's)  | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Maximale gegevensopslag per pool* | 250 GB| 500 GB | 750 GB | 750 GB |
| Maximale OLTP-opslag in het geheugen per pool | 1 GB | 2 GB | 4 GB | 10 GB |
| Maximaal aantal databases per pool | 50 | 100 | 100 | 100 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 200 | 400 | 800 | 1600 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 200 | 400 | 800 | 1600 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 |
| Minimaal aantal eDTU’s per database | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 |
| Maximaal aantal eDTU’s per database | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 
| Maximale gegevensopslag per database | 250 GB | 500 GB | 500 GB | 500 GB | 
||||||||

> [!IMPORTANT]
>\* Gepoolde databases maken gebruik van een gedeelde poolopslag. De gegevensopslag in een elastische pool wordt hierdoor beperkt tot de overgebleven poolopslag of de maximumopslag per database, afhankelijk van wat het kleinste is. 
>
>
>\*\* Minimaal/maximaal aantal eDTU's per database, vanaf 200 eDTU's en hoger, is in de openbare preview.
>
>\*\*\* De maximale gegevensopslag per pool voor Premium-pools met 500 eDTU's of meer, is standaard 750 GB. Als u de hogere maximale gegevensopslag per Premium-pool wilt gebruiken (1000 eDTU's of meer), moet u deze grootte expliciet selecteren in Azure Portal, [PowerShell](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-powershell), de [Azure CLI](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-azure-cli) of de [REST-API](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-rest-api). Premium-pools met meer dan 1 TB opslag is momenteel in openbare preview in de volgende regio's: VS Oost 2, VS West, VS (overheid) Virginia, West-Europa, Duitsland Centraal, Zuidoost-Azië, Japan - oost, Australië - oost, Canada Centraal en Canada Oost. De maximale opslag per pool voor alle andere regio's is momenteel beperkt tot 750 GB.
>
