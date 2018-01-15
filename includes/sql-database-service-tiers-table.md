<!--
Used in:
sql-database-performance-guidance.md 
sql-database-single-database-resources.md 
-->

### <a name="basic-service-tier"></a>Basisservicelaag
| **Prestatieniveau** | **Basic** |
| :--- | --: |
| Maximaal aantal DTU's | 5 |
| Opgenomen opslag (GB) | 2 |
| Maximum aantal opslagopties (GB) | 2 |
| Maximale OLTP-opslag in het geheugen (GB) |N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen) | 30 |
| Maximaal aantal gelijktijdige aanmeldingen | 30 |
| Maximaal aantal gelijktijdige sessies | 300 |
|||

### <a name="standard-service-tier"></a>Standaardservicelaag
| **Prestatieniveau** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Maximale aantal dtu's ** | 10 | 20 | 50 | 100 |
| Opgenomen opslag (GB) | 250 | 250 | 250 | 250 |
| Maximum aantal opslagopties (GB) * | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige aanmeldingen | 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige sessies |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standaard-servicelaag (vervolg)
| **Prestatieniveau** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Maximale aantal dtu's ** | 200 | 400 | 800 | 1600 | 3000 |
| Opgenomen opslag (GB) | 250 | 250 | 250 | 250 | 250 |
| Maximum aantal opslagopties (GB) * | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige aanmeldingen | 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige sessies |4800 | 9600 | 19200 | 30.000 |30.000 |
|||||||

### <a name="premium-service-tier"></a>Premium servicelaag 
| **Prestatieniveau** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Opgenomen opslag (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Maximum aantal opslagopties (GB) * | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maximale OLTP-opslag in het geheugen (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige aanmeldingen | 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige sessies | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
|||||||

### <a name="premium-rs-service-tier"></a>Premium-RS servicelaag 
| **Prestatieniveau** | **PRS1** | **PRS2** | **PRS4** | **PRS6** |
| :--- |---:|---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 125 | 250 | 500 | 1000 |
| Opgenomen opslag (GB) | 500 | 500 | 500 | 500 |
| Maximum aantal opslagopties (GB) * | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | 1 | 2 | 4 | 8 |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 200 | 400 | 800 | 1600 |
| Maximaal aantal gelijktijdige aanmeldingen | 200 | 400 | 800 | 1600 |
| Maximaal aantal gelijktijdige sessies | 30.000 | 30.000 | 30.000 | 30.000 |
|||||||

> [!IMPORTANT]
> \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
>
>\*In de laag Premium meer dan 1 TB aan opslag is momenteel beschikbaar in de volgende gebieden: Australië-Oost, Australië-Zuidoost, Canada centraal, Canada Oost, Frankrijk centraal, Duitsland centraal, Japan-Oost, centraal Korea Zuid-centraal VS Zuid-Oost-Azië, ons East2 , VS-west, VS Gov Virginia en West-Europa. Zie [P11-P15: huidige beperkingen](../articles/sql-database/sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 
>\*\*Maximale aantal dtu's per database starten op 200 dtu's en hoger in de standaardmodus zijn Preview-versie.
>
