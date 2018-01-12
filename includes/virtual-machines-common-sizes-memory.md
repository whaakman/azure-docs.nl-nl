
Geoptimaliseerd voor geheugen aanbieding voor VM-grootten een hoge ratio van geheugen voor CPU die zijn ideaal voor relationele database-servers, middelgrote tot grote caches en in het geheugen analytics. In dit artikel bevat informatie over het aantal Vcpu, gegevensschijven en NIC's, evenals opslag doorvoer en bandbreedte voor elke grootte in deze groepering. 

* De M-reeks biedt het hoogste vCPU aantal (maximaal 128 vcpu's) en de grootste geheugen (maximaal 3,8 TiB) van een virtuele machine in de cloud.  Dit is ideaal voor zeer grote databases of andere toepassingen die zo kunnen profiteren van een groot aantal vCPU’s en grote hoeveelheden geheugen.

* Hun tegenhangers uit de Dv2-serie, D-serie, G-serie en DS/GS-serie zijn ideaal voor toepassingen waarvoor snellere CPU's of betere prestaties van de tijdelijke opslag zijn vereist, of waarvoor de geheugeneisen hoger zijn.  Ze bieden een krachtige combinatie voor vele toepassingen op bedrijfsniveau.

* Virtuele machines uit de D-serie zijn ontworpen voor het uitvoeren van toepassingen die meer rekenvermogen en tijdelijke schijfprestaties vereisen. Virtuele machines uit de D-serie hebben snellere processors, een hogere geheugen-naar-vCPU-snelheid en een SSD (solid-state drive) voor tijdelijke opslag. Voor meer informatie leest u de aankondiging in de Azure-blog [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) (Nieuwe grootten van virtuele machines uit de D-serie).

* De Dv2-serie, de opvolger van de oorspronkelijke D-serie, heeft een krachtigere CPU. De CPU van de Dv2-serie is ongeveer 35% sneller dan de CPU van de D-serie. Deze is gebaseerd op de nieuwste generatie Intel Xeon® E5-2673 v3-processor van 2,4 GHz (Haswell). Met Intel Turbo Boost Technology 2.0 kunnen ze maar liefst 3,1 GHz bereiken. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.


## <a name="esv3-series-sup1sup"></a>Esv3-serie <sup>1</sup>

ACU: 160-190

Exemplaren uit de ESv3-serie zijn gebaseerd op Intel XEON ® E5-2673 v4-processors van 2,3 GHz (Broadwell) en kunnen maar liefst 3,5 GHz bereiken door de Intel Turbo Boost Technology 2.0 en maken gebruik van Premium Storage. Ev3-exemplaren zijn ideaal voor geheugenintensieve bedrijfstoepassingen.


| Grootte             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3  | 2      | 16          | 32             | 4              | 4,000 / 32 (50)                                                       | 3200 / 48                                | 2 / 1,000                                   |
| Standard_E4s_v3  | 4      | 32          | 64             | 8              | 8,000 / 64 (100)                                                      | 6400 / 96                                | 2 / 2,000                                   |
| Standard_E8s_v3  | 8      | 64          | 128            | 16             | 16,000 / 128 (200)                                                    | 12.800 / 192                              | 4 / 4,000                                       |
| Standard_E16s_v3 | 16     | 128         | 256            | 32             | 32,000 / 256 (400)                                                    | 25.600 / 384                              | 8 / 8,000                                       |
| Standard_E32s_v3 <sup>2</sup> | 32     | 256         | 512            | 32             | 64,000 / 512 (800)                                                    | 51.200 / 768                              | 8 / 16,000                             |
| Standard_E64s_v3 <sup>2</sup> | 64     | 432         | 864            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |

<sup>1</sup> Esv3-serie van de virtuele machine zijn uitgerust met Intel® Hyper-Threading-technologie <sup>2</sup> beperkte core grootten beschikbaar 

## <a name="ev3-series-sup1sup"></a>Ev3-serie <sup>1</sup>

ACU: 160 - 190 

Exemplaren uit de Ev3-serie zijn gebaseerd op Intel XEON ® E5-2673 v4-processors van 2,3 GHz (Broadwell) en kunnen maar liefst 3,5 GHz bereiken door de Intel Turbo Boost Technology 2.0. Ev3-exemplaren zijn ideaal voor geheugenintensieve bedrijfstoepassingen.

Gegevensschijfopslag wordt apart van virtuele machines in rekening gebracht. Als u Premium Storage-schijven wilt gebruiken, gebruik dan de ESv3-grootten. De prijs- en factureringsmeters voor de ESv3-grootten zijn gelijk aan die van de Ev3-serie. 


| Grootte            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal NIC's/netwerkbandbreedte |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1,000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8,000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16,000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |

<sup>1</sup> Ev3-serie van de virtuele machine zijn uitgerust met Intel® Hyper-Threading-technologie

## <a name="m-series-sup1sup"></a>M-serie <sup>1</sup>

ACU: 160-180

| Grootte            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M64s  | 64   | 1024        | 2048           | 64             | 80,000 / 800 (6348)       | 40.000 / 1,000                            | 8 / 16000          |
| Standard_M64ms  | 64   | 1792        | 2048           | 64             | 80,000 / 800 (6348)       | 40.000 / 1,000                            | 8 / 16000          |
| Standard_M128s <sup>2. 3</sup> | 128  | 2048        | 4096           | 64             | 160,000 / 1,600 (12,696) | 80.000 / 2000                            | 8 / 30000          |
| Standard_M128ms <sup>2. 3</sup> | 128  | 3800        | 4096           | 64             | 160,000 / 1,600 (12,696) | 80.000 / 2000                            | 8 / 30000          |

<sup>1</sup> M-serie van de virtuele machine functie Intel® Hyper-Threading-technologie

<sup>2</sup> meer dan 64 vCPU vereisen een van deze ondersteunde gastbesturingssystemen: Windows Server 2016, Ubuntu 16.04 TNS, SLES 12 SP2, en Red Hat Enterprise Linux, CentOS 7.3 of Oracle Linux 7.3 met LIS 4.2.1 

<sup>3</sup> beperkte core grootten beschikbaar.

<br>

## <a name="gs-series-sup1sup"></a>GS-serie <sup>1</sup>

ACU: 180 - 240

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10.000 / 100 (264) |5000 / 125 |2 / 2000 |
| Standard_GS2 |4 |56 |112 |16 |20.000 / 200 (528) |10.000 / 250 |2 / 4000 |
| Standard_GS3 |8 |112 |224 |32 |40.000 / 400 (1,056) |20.000 / 500 |4 / 8000 |
| Standard_GS4 <sup>3</sup> |16 |224 |448 |64 |80.000 / 800 (2,112) |40.000 / 1,000 |8 / 16000 |
| Standard_GS5 <sup>2, 3</sup> |32 |448 |896 |64 |160.000 / 1600 (4,224) |80.000 / 2000 |8 / 20000 |

<sup>1</sup> de maximale schijf-doorvoer (IOP's of MBps) mogelijk een GS-serie VM kan worden beperkt door het aantal grootte en striping van de gekoppelde schijven. Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie. 

<sup>2</sup> exemplaar is geïsoleerd, zodat de hardware die zijn toegewezen aan één klant.

<sup>3</sup> beperkte core grootten beschikbaar 

<br>

## <a name="g-series"></a>G-serie

ACU: 180 - 240

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000 / 93 / 46                                           | 8 / 8 x 500                       | 2 / 2000                     |
| Standard_G2  | 4         | 56          | 768            | 12.000 / 187 / 93                                         | 16 / 16 x 500                       | 2 / 4000                     |
| Standard_G3  | 8         | 112         | 1536          | 24.000 / 375 / 187                                        | 32 / 32 x 500                     | 4 / 8000                |
| Standard_G4  | 16        | 224         | 3072          | 48.000 / 750 / 375                                        | 64 / 64 x 500                     | 8 / 16000          |
| Standard_G5 <sup>1</sup> | 32        | 448         | 6144          | 96.000 / 1500 / 750                                       | 64 / 64 x 500                     | 8 / 20000           |

<sup>1</sup> exemplaar is geïsoleerd, zodat de hardware die zijn toegewezen aan één klant.
<br>


## <a name="dsv2-series-sup1sup"></a>DSv2-serie <sup>1</sup>

ACU: 210 - 250

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 |2 |14 |28 |8 |8000 / 64 (72) |6400 / 96 |2 / 1500 |
| Standard_DS12_v2 |4 |28 |56 |16 |16.000 / 128 (144) |12.800 / 192 |4 / 3000 |
| Standard_DS13_v2 |8 |56 |112 |32 |32.000 / 256 (288) |25.600 / 384 |8 / 6000 |
| Standard_DS14_v2 |16 |112 |224 |64 |64.000 / 512 (576) |51.200 / 768 |8 / 12000 |
| Standard_DS15_v2 <sup>2</sup> |20 |140 |280 |64 |80.000 / 640 (720) |64.000 / 960 |8 / 25000 <sup>3</sup>

<sup>1</sup> de maximale schijf-doorvoer (IOP's of MBps) mogelijk met een reeks DSv2 VM kan worden beperkt door het aantal grootte en striping van de gekoppelde schijven.  Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.

<sup>2</sup> exemplaar is een geïsoleerde-knooppunt dat zorgt ervoor dat uw virtuele machine de enige virtuele machine op onze Intel Haswell-knooppunt is.

<sup>3</sup> 25000 Mbps met versnelde netwerken.

<br>

## <a name="dv2-series"></a>Dv2-serie

ACU: 210 - 250

| Grootte              | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48.000 / 750 / 375                                        | 64 / 64 x 500                       | 8 / 12000          |
| Standard_D15_v2 <sup>1</sup> | 20        | 140         | 1000          | 60.000 / 937 / 468                                        | 64 / 64 x 500                       | 8 / 25000 <sup>2</sup> |

<sup>1</sup> exemplaar is een geïsoleerde-knooppunt dat zorgt ervoor dat uw virtuele machine de enige virtuele machine op onze Intel Haswell-knooppunt is.

<sup>2</sup> 25000 Mbps met versnelde netwerken.

<br>

## <a name="ds-series-sup1sup"></a>DS-serie <sup>1</sup>

ACU: 160

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8000 / 64 (72) |6400 / 64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16.000 / 128 (144) |12.800 / 128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32.000 / 256 (288) |25.600 / 256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64.000 / 512 (576) |51.200 / 512 |8 / 8000 |

<sup>1</sup> de maximale schijf-doorvoer (IOP's of MBps) mogelijk een DS-serie VM kan worden beperkt door het aantal grootte en striping van de gekoppelde schijven.  Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.


## <a name="d-series"></a>D-serie

ACU: 160

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48.000 / 750 / 375                                        | 64 / 64 x 500                       | 8 / 8000                |

<br>

