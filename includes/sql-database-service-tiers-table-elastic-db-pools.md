<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->

### <a name="basic-elastic-pool-limits"></a>Basisbeperkingen voor elastische groepen

| Poolgrootte (eDTU's)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Maximale opslag per pool* | 5 GB| 10 GB| 20 GB| 29 GB| 39 GB| 78 GB| 117 GB| 156 GB|
| Maximumaantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers per groep | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige aanmeldingen per groep | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige sessies per groep | 30.000 | 30.000 | 30.000 | 30.000 |30.000 | 30.000 | 30.000 | 30.000 |
| Minimaal aantal eDTU’s per database | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} |
| Maximaal aantal eDTU’s per database | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} |
||||||||

### <a name="standard-elastic-pool-limits"></a>Standaardbeperkingen voor elastische groepen

| Poolgrootte (eDTU's)  | **50** | **100** | **200** | **300** | **400** | **800** | 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Maximale opslag per pool* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Maximumaantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maximaal aantal gelijktijdige werknemers per groep | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximaal aantal gelijktijdige aanmeldingen per groep | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximaal aantal gelijktijdige sessies per groep | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Minimaal aantal eDTU’s per database | {0,10,20,<br>50} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Maximaal aantal eDTU’s per database | {10,20,<br>50} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limieten voor Standard elastische pools (vervolg) 

| Poolgrootte (eDTU's)  |  **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Maximale opslag per pool* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Maximumaantal databases per pool | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers per groep |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige aanmeldingen per groep |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige sessies per groep | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Maximaal aantal eDTU’s per database | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Premium beperkingen voor elastische groepen

| Poolgrootte (eDTU's)  | **125** | **250** | **500** | **1000** | **1500** | 
|:---|---:|---:|---:| ---: | ---: | 
| Maximale opslag per pool* | 250 GB| 500 GB| 750 GB| 750 GB| 750 GB| 
| Maximumaantal databases per pool | 50 | 100 | 100 | 100 | 100 |  
| Maximaal aantal gelijktijdige werknemers per groep | 200 | 400 | 800 | 1600 |  2400 | 
| Maximaal aantal gelijktijdige aanmeldingen per groep | 200 | 400 | 800 | 1600 |  2400 |
| Maximaal aantal gelijktijdige sessies per groep | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database | {0,25,50,75,<br>125} | {0,25,50,75,<br>125,250} | {0,25,50,75,<br>125,250,500} | {0,25,50,75,<br>125,250,500,<br>1000} | {0,25,50,75,<br>125,250,500,<br>1000,1500} | 
| Maximaal aantal eDTU’s per database | {25,50,75,<br>125} | {25,50,75,<br>125,250} | {25,50,75,<br>125,250,500} | {25,50,75,<br>125,250,500,<br>1000} | {25,50,75,<br>125,250,500,<br>1000,1500} |  
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limieten voor Premium elastische pools (vervolg) 

| Poolgrootte (eDTU's)  |  **2000** | **2500** | **3000** | **3500** | **4000** |
|:---|---:|---:|---:| ---: | ---: | 
| Maximale opslag per pool* | 750 GB | 750 GB | 750 GB | 750 GB | 750 GB |
| Maximumaantal databases per pool | 100 | 100 | 100 | 100 | 100 | 
| Maximaal aantal gelijktijdige werknemers per groep |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige aanmeldingen per groep |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige sessies per groep | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} |  {0,25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
| Maximaal aantal eDTU’s per database | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
||||||||

\* Gedeelde opslag voor pool voor Elastic Database, zodat database-opslag wordt beperkt tot de overgebleven poolopslag of de maximumopslag per database, afhankelijk van wat het kleinste is



<!--HONumber=Dec16_HO3-->


