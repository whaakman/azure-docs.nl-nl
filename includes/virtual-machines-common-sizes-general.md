
<!-- A-series, Av2-series, D-series, Dv2-series, DS-series*, DSv2-series* -->

- Virtuele machines uit de A- en Av2-serie kunnen worden geïmplementeerd op diverse hardwaretypen en processors. De grootte is afhankelijk van de hardware, zodat er consistente processorprestaties voor het actieve exemplaar kunnen worden geboden, ongeacht de hardware waarop deze is geïmplementeerd. Om de fysieke hardware te bepalen waarop deze grootte is geïmplementeerd, vraagt u vanuit de virtuele machine gegevens over de virtuele hardware op.

- Virtuele machines uit de D-serie zijn ontworpen voor het uitvoeren van toepassingen die meer rekenvermogen en tijdelijke schijfprestaties vereisen. Virtuele machines uit de D-serie hebben snellere processors, een hogere geheugen-naar-vCPU-snelheid en een SSD (solid-state drive) voor de tijdelijke schijf. Voor meer informatie leest u de aankondiging in de Azure-blog [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) (Nieuwe grootten van virtuele machines uit de D-serie).

- De Dv2-serie, de opvolger van de oorspronkelijke D-serie, heeft een krachtigere CPU. De CPU van de Dv2-serie is ongeveer 35% sneller dan de CPU van de D-serie. Deze is gebaseerd op de nieuwste generatie Intel Xeon® E5-2673 v3-processor van 2,4 GHz (Haswell). Met Intel Turbo Boost Technology 2.0 kunnen ze maar liefst 3,1 GHz bereiken. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.

- De basislaaggrootten zijn met name bedoeld voor ontwikkelworkloads en andere toepassingen waarvoor geen taakverdeling, automatische schaling of geheugenintensieve virtuele machines zijn vereist. Zie [Prijzen van virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) voor informatie over de VM-grootten die geschikter zijn voor productietoepassingen (grootten voor virtuele machines) [virtual-machines-size-specs.md] en voor informatie over prijzen van virtuele machines.

## <a name="dsv3-series"></a>Dsv3-serie

ACU: 160-190

Grootten uit de Dsv3-serie zijn gebaseerd op Intel XEON ® E5-2673 v4-processors van 2,3 GHz (Broadwell) en kunnen maar liefst 3,5 GHz bereiken door de Intel Turbo Boost Technology 2.0 en maken gebruik van Premium Storage. De Dsv3-serie biedt een combinatie van vCPU, geheugen en tijdelijke opslag voor de meeste productieworkloads.


| Grootte             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2      | 8           | 16             | 4              | 4,000 / 32 (50)                                                       | 3200 / 48                                | 2/gemiddeld                                   |
| Standard_D4s_v3  | 4      | 16          | 32             | 8              | 8,000 / 64 (100)                                                      | 6400 / 96                                | 2/gemiddeld                                   |
| Standard_D8s_v3  | 8      | 32          | 64             | 16             | 16,000 / 128 (200)                                                    | 12.800 / 192                              | 4/hoog                                       |
| Standard_D16s_v3 | 16     | 64          | 128            | 32             | 32,000 / 256 (400)                                                    | 25.600 / 384                              | 8/hoog                                       |


## <a name="dv3-series"></a>Dv3-serie

ACU: 160-190

Grootten uit de Dv3-serie zijn gebaseerd op Intel XEON ® E5-2673 v4-processors van 2,3 GHz (Broadwell) en kunnen maar liefst 3,5 GHz bereiken door de Intel Turbo Boost Technology 2.0. De Dv3-serie biedt een combinatie van vCPU, geheugen en tijdelijke opslag voor de meeste productieworkloads.

Gegevensschijfopslag wordt apart van virtuele machines in rekening gebracht. Als u Premium Storage-schijven wilt gebruiken, gebruik dan de Dsv3-grootten. De prijs- en factureringsmeters voor de Dsv3-grootten zijn gelijk aan die van de Dv3-serie. 


| Grootte            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal NIC's/netwerkbandbreedte |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_D2_v3  | 2         | 8           | 50             | 4              | 3000/46/23                                               | 2/gemiddeld                 |
| Standard_D4_v3  | 4         | 16          | 100            | 8              | 6000/93/46                                               | 2/gemiddeld                 |
| Standard_D8_v3  | 8         | 32          | 200            | 16             | 12000/187/93                                             | 4/hoog                     |
| Standard_D16_v3 | 16        | 64          | 400            | 32             | 24000/375/187                                            | 8/hoog                     |


## <a name="dsv2-series"></a>DSv2-serie

ACU: 210-250

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4000 / 32 (43) |3200 / 48 |2 / 750 |
| Standard_DS2_v2 |2 |7 |14 |4 |8000 / 64 (86) |6400 / 96 |2 / 1500 |
| Standard_DS3_v2 |4 |14 |28 |8 |16.000 / 128 (172) |12.800 / 192 |4 / 3000 |
| Standard_DS4_v2 |8 |28 |56 |16 |32.000 / 256 (344) |25.600 / 384 |8 / 6000 |
| Standard_DS5_v2 |16 |56 |112 |32 |64.000 / 512 (688) |51.200 / 768 |8 / 6000 - 12000 &#8224;|



## <a name="dv2-series"></a>Dv2-serie

ACU: 210-250

| Grootte              | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2 x 500                         | 2 / 750                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4 x 500                         | 2 / 1500                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12.000 / 187 / 93                                         | 8 / 8 x 500                         | 4 / 3000                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24.000 / 375 / 187                                        | 16 / 16 x 500                       | 8 / 6000                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48.000 / 750 / 375                                        | 32 / 32 x 500                       | 8 / 6000 - 12000 &#8224;          |


<br>

## <a name="ds-series"></a>DS-serie

ACU: 160

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4000 / 32 (43) |3200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |4 |8000 / 64 (86) |6400 / 64 |2 / 1000 |
| Standard_DS3 |4 |14 |28 |8 |16.000 / 128 (172) |12.800 / 128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |16 |32.000 / 256 (344) |25.600 / 256 |8 / 4000 |

<br>

## <a name="d-series"></a>D-serie 

ACU: 160

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2 x 500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4 x 500                         | 2 / 1000                     |
| Standard_D3  | 4         | 14          | 200            | 12.000 / 187 / 93                                         | 8 / 8 x 500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24.000 / 375 / 187                                        | 16 / 16 x 500                       | 8 / 4000                     |

<br>


## <a name="av2-series"></a>Av2-serie

ACU: 100

| Grootte            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000 / 20 / 10                                           | 2 / 2 x 500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000 / 40 / 20                                           | 4 / 4 x 500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000 / 80 / 40                                           | 8 / 8 x 500               | 4 / 1000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000 / 160 / 80                                          | 16 / 16 x 500             | 8 / 2000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000 / 40 / 20                                           | 4 / 4 x 500               | 2 / 500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000 / 80 / 40                                           | 8 / 8 x 500               | 4 / 1000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000 / 160 / 80                                          | 16 / 16 x 500             | 8 / 2000                     |

<br>

## <a name="a-series"></a>A-serie

ACU: 50-100

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (HDD): GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0* |1 |0,768 |20 |1 |1 x 500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2 x 500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4 x 500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8 x 500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16 x 500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4 |4 x 500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8 x 500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16 x 500 |4 / 2000 |
<br>

*De A0-grootte wordt overgeschreven naar de fysieke hardware. Alleen bij deze specifieke grootte kunnen implementaties van andere klanten invloed hebben op de prestaties van uw uitgevoerde workload. De relatieve prestaties worden hieronder beschreven, zoals de verwachte basislijn, met een variabiliteit van ongeveer 15 procent.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0 - A4 met CLI en PowerShell
In het klassieke implementatiemodel verschillen sommige namen van VM-grootten enigszins in CLI en in PowerShell:

* Standard_A0 is ExtraSmall 
* Standard_A1 is Small
* Standard_A2 is Medium
* Standard_A3 is Large
* Standard_A4 is ExtraLarge

## <a name="basic-a"></a>Basic A

|Grootte - grootte\naam | vCPU |Geheugen|NIC's (max.)|Maximumgrootte van tijdelijke schijf |Met maximaal gegevensschijven 1023 GB elk)|Met maximaal IOPS (300 per schijf)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1x300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2x300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4x300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8x300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16x300|
