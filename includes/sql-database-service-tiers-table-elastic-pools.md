<!--
Used in:
sql-database-elastic-pool.md 
-->

 
### <a name="basic-elastic-pool-limits"></a>Basisbeperkingen voor elastische groepen

| eDTU's per groep | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Opgenomen opslag per groep van toepassingen (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Maximum aantal opslagopties per groep (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 |30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Opties van de Max edtu's per database | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maximale opslag per database (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standaardbeperkingen voor elastische pools

| eDTU's per groep | **50** | **100** | **200** | **300** | **400** | **800**| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Opgenomen opslag per groep van toepassingen (GB) | 50 | 100 | 200 | 300 | 400 | 800 | 
| Maximum aantal opslagopties per groep (GB) * | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 | 
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | N/A | 
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 | 800 | 1600 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 100 | 200 | 400 | 600 | 800 | 1600 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database ** | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Opties van max edtu's per database ** | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Maximale opslag per database (GB)* | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limieten voor Standard elastische pools (vervolg) 

| eDTU's per groep | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Opgenomen opslag per groep van toepassingen (GB) | 1200 | 1600 | 2000 | 2500 | 3000 | 
| Maximum aantal opslagopties per groep (GB) * | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | 
| Maximaal aantal databases per pool | 500 | 500 | 500 | 500 | 500 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Opties voor min-edtu's per database ** | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Opties van max edtu's per database ** | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Maximum aantal opslagopties per database (GB) * | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Limieten voor Premium elastische pools

| eDTU's per groep | **125** | **250** | **500** | **1000** | **1500**| 
|:---|---:|---:|---:| ---: | ---: | 
| Opgenomen opslag per groep van toepassingen (GB) | 250 | 500 | 750 | 1024 | 1536 | 
| Maximum aantal opslagopties per groep (GB) * | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | 1 | 2 | 4 | 10 | 12 | 
| Maximaal aantal databases per pool | 50 | 100 | 100 | 100 | 100 | 
| Maximaal aantal gelijktijdige werknemers per pool (aanvragen) | 200 | 400 | 800 | 1600 | 2400 | 
| Maximaal aantal gelijktijdige aanmeldingen per pool | 200 | 400 | 800 | 1600 | 2400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 | 
| Maximaal aantal eDTU’s per database | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Maximale opslag per database (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limieten voor Premium elastische pools (vervolg) 

| eDTU's per groep | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: | 
| Opgenomen opslag per groep van toepassingen (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Maximum aantal opslagopties per groep (GB) * | 2048 | 2560 | 3072 | 3548 | 4096|
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | 16 | 20 | 24 | 28 | 32 |
| Maximaal aantal databases per pool | 100 | 100 | 100 | 100 | 100 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Opties voor min-edtu's per database | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Opties van de Max edtu's per database | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maximale opslag per database (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Limieten voor Premium RS elastische pools

| eDTU's per groep | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Opgenomen opslag per groep van toepassingen (GB) | 250 | 500 | 750 | 750 |
| Maximum aantal opslagopties per groep (GB) * | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | 1 | 2 | 4 | 10 |
| Maximaal aantal databases per pool | 50 | 100 | 100 | 100 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 200 | 400 | 800 | 1600 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 200 | 400 | 800 | 1600 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 |
| Opties van de Max edtu's per database | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 
| Maximale opslag per database (GB)* | 1024 | 1024 | 1024 | 1024 | 
||||||||

> [!IMPORTANT]
> \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie voor meer informatie de [pagina met prijzen SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/). Opslag groter zijn dan de hoeveelheid opslagruimte die opgenomen zijn in preview en extra kosten gelden. Zie voor meer informatie de [pagina met prijzen SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/).
>
> \*In de laag Premium meer dan 1 TB aan opslag is momenteel beschikbaar in de volgende gebieden: Australië-Oost, Australië-Zuidoost, Canada centraal, Canada Oost, Frankrijk centraal, Duitsland centraal, Japan-Oost, centraal Korea Zuid-centraal VS Zuid-Oost-Azië, ons East2 , VS-west, VS Gov Virginia en West-Europa. 
>
>\*\*Min/max edtu's per database vanaf op 200 edtu's en hoger **standaard** pools zijn Preview-versie.
>
