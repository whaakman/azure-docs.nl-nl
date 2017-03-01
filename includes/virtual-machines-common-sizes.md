


Er zijn meerdere standaardgrootten waaruit u in Azure kunt kiezen. Enkele overwegingen voor een aantal van deze grootten zijn:

* Virtuele machines uit de D-serie zijn ontworpen voor het uitvoeren van toepassingen die meer rekenvermogen en tijdelijke schijfprestaties vereisen. Virtuele machines uit de D-serie hebben snellere processors, een hogere geheugen-naar-core-snelheid en een SSD (solid-state drive) voor de tijdelijke schijf. Voor meer informatie leest u de aankondiging in de Azure-blog [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) (Nieuwe grootten van virtuele machines uit de D-serie).
* De Dv2-serie, de opvolger van de oorspronkelijke D-serie, heeft een krachtigere CPU. De CPU van de Dv2-serie is ongeveer 35% sneller dan de CPU van de D-serie. Deze is gebaseerd op de nieuwste generatie Intel Xeon® E5-2673 v3-processor van 2,4 GHz (Haswell). Met Intel Turbo Boost Technology 2.0 kunnen ze maar liefst 3,1 GHz bereiken. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.
* De F-serie is gebaseerd op de 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-processor, die met Intel Turbo Boost Technology 2.0 een kloksnelheid kan halen van 3,1 GHz. Dit zijn dezelfde CPU-prestaties als de virtuele machines uit de Dv2-serie.  Tegen een lagere prijs per uur levert de F-serie de beste prijs/kwaliteit-verhouding van de Azure-portfolio, gebaseerd op de ACU (Azure Compute Unit) per core. 
  
    De F-serie introduceert ook een nieuwe standaard op het gebied van groottebenamingen voor virtuele machines in Azure. Voor deze serie en VM-grootten die in de toekomst worden uitgebracht, komt de numerieke waarde achter de letter van de familienaam overeen met het aantal CPU-kernen. Aanvullende mogelijkheden, zoals geoptimaliseerd voor Premium-opslag, worden dan aangeduid met letters, volgend op het aantal numerieke CPU-kernen. Deze naamgevingsconventie wordt gebruikt om de grootte aan te geven van virtuele machines die in de toekomst worden uitgebracht. De grootteaanduiding voor bestaande virtuele machines verandert niet.
* Virtuele machines uit de G-serie bieden het meeste geheugen en worden uitgevoerd op hosts met een processor uit de Intel Xeon E5 V3-familie.
* Virtuele machines uit de DS-serie, DSv2-serie, Fs-serie en GS-serie kunnen gebruikmaken van Premium Storage. Deze biedt opslag met hoge prestaties en lage latentie voor I/O-intensieve workloads. Deze virtuele machines gebruiken SSD's (solid-state drives) om de schijven van virtuele machines te hosten en een lokale SSD-schijfcache te bieden. Premium Storage is beschikbaar in bepaalde regio's. Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.
*   Virtuele machines uit de A- en Av2-serie kunnen worden geïmplementeerd op diverse hardwaretypen en processors. De grootte is afhankelijk van de hardware, zodat er consistente processorprestaties voor het actieve exemplaar kunnen worden geboden, ongeacht de hardware waarop deze is geïmplementeerd. Om de fysieke hardware te bepalen waarop deze grootte is geïmplementeerd, vraagt u vanuit de virtuele machine gegevens over de virtuele hardware op.
* De A0-grootte wordt overgeschreven naar de fysieke hardware. Alleen bij deze specifieke grootte kunnen implementaties van andere klanten invloed hebben op de prestaties van uw uitgevoerde workload. De relatieve prestaties worden hieronder beschreven, zoals de verwachte basislijn, met een variabiliteit van ongeveer 15 procent.

De grootte van de virtuele machine heeft invloed op de prijs. De grootte heeft ook invloed op de verwerkings-, geheugen- en opslagcapaciteit van de virtuele machine. Opslagkosten worden afzonderlijk berekend op basis van het aantal pagina's dat in het opslagaccount is gebruikt. Zie [Prijsinformatie voor virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) en [Prijsinformatie voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie. 

De volgende overwegingen kunnen u helpen bij het kiezen van een grootte:

* Grootten uit de A8-A11- en H-serie worden ook wel *rekenintensieve exemplaren* genoemd. De hardware waarop deze grootten worden uitgevoerd, is ontworpen en geoptimaliseerd voor rekenintensieve en netwerkintensieve toepassingen, waaronder HPC-clustertoepassingen (high-performance computing), modellerings- en simulatietoepassingen. De A8-A11-serie gebruikt Intel Xeon E5-2670 @ 2,6 GHZ en de H-serie gebruikt Intel Xeon E5-2667 v3 @ 3,2 GHz. Zie [About the H-series and compute-intensive A-series VMs](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Over virtuele machines uit de H-serie en rekenintensieve A-serie) voor meer informatie over en overwegingen bij het gebruik van deze grootten. 
* Hun tegenhangers uit de Dv2-serie, D-serie, G-serie en DS/GS-serie zijn ideaal voor toepassingen waarvoor snellere CPU's of betere prestaties van de lokale schijf zijn vereist of de geheugeneisen hoger zijn.  Ze bieden een krachtige combinatie voor vele toepassingen op bedrijfsniveau.
* Virtuele machines uit de F-serie vormen een uitstekende keuze voor workloads die snellere CPU's nodig hebben, maar niet zo veel geheugen of lokale SSD per CPU-kern.  Voor workloads voor analysen, gameservers, webservers en batchverwerking is de F-serie wellicht een betere keuze.
* Sommige fysieke hosts in Azure-datacenters bieden mogelijk geen ondersteuning voor grotere VM-formaten, zoals A5 – A11. Als gevolg daarvan is het mogelijk dat het foutbericht **Kan virtuele machine <machine name> niet configureren** of **Kan virtuele machine <machine name> niet maken** wordt weergegeven wanneer u de grootte van een bestaande virtuele machine wilt wijzigen, een nieuwe virtuele machine wilt maken in een virtueel netwerk dat is gemaakt vóór 16 april 2013, of een nieuwe virtuele machine wilt toevoegen aan een bestaande cloudservice. Zie [Error: “Failed to configure virtual machine”](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) (Fout: 'Kan virtuele machine niet maken') op het ondersteuningsforum voor tijdelijke oplossingen voor elk implementatiescenario.  
* Het is ook mogelijk dat uw abonnement het aantal kernen beperkt dat u in een bepaalde groottefamilie mag implementeren. Neem contact op met ondersteuning van Azure als u een quotum wilt verhogen.

## <a name="performance-considerations"></a>Prestatieoverwegingen
We hebben het concept van de ACU (Azure Compute Unit) ontwikkeld om een manier te bieden waarop rekenprestaties (CPU) van Azure-SKU's met elkaar kunnen worden vergeleken. Op die manier kunt u eenvoudig bepalen welke SKU hoogstwaarschijnlijk aan uw prestatiebehoeften zal voldoen.  ACU is momenteel gestandaardiseerd op 100 voor een kleine virtuele machine (Standard_A1). Alle andere SKU's geven vervolgens weer hoeveel sneller die SKU een standaardbenchmark ongeveer kan uitvoeren. 

> [!IMPORTANT]
> De ACU is slechts een richtlijn.  De resultaten voor uw workload kunnen verschillen. 
> 
> 

<br>

| SKU-familie | ACU/kern |
| --- | --- |
| [A0](#a-series) |50 |
| [A1-A4](#a-series) |100 |
| [A5-A7](#a-series) |100 |
| [A1_v2-A8_v2](#av2-series) |100 |
| [A2m_v2-A8m_v2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-D14](#d-series) |160 |
| [D1_v2-D15_v2](#dv2-series) |210 - 250* |
| [DS1-DS14](#ds-series) |160 |
| [DS1_v2-DS15_v2](#dsv2-series) |210-250* |
| [F1-F16](#f-series) |210-250* |
| [F1s-F16s](#fs-series) |210-250* |
| [G1-G5](#g-series) |180 - 240* |
| [GS1-GS5](#gs-series) |180 - 240* |
| [H](#h-series) |290 - 300* |
| [L4s-L32s](#l-series) |180 - 240* |

ACU's die met een * zijn gemarkeerd, maken gebruik van Intel® Turbo-technologie om de CPU-frequentie te verhogen en nóg betere prestaties te leveren.  Hoe groot die extra prestaties zijn, is afhankelijk van de VM-grootte, de workload en de andere workloads die op dezelfde host worden uitgevoerd.

## <a name="size-tables"></a>Groottetabellen
In de volgende tabellen ziet u de grootten en de capaciteiten die ze bieden.

* De opslagcapaciteit wordt weergegeven in GiB-eenheden of 1024^3 bytes. Wanneer u schijven die zijn gemeten in GB (1000^3 bytes), vergelijkt met schijven die zijn gemeten in GiB (1024^3), moet u voor ogen houden dat de capaciteit in GiB kleiner lijkt te zijn. 1023 GiB is bijvoorbeeld gelijk aan 1098,4 GB
* De schijfdoorvoer wordt gemeten in I/O-bewerkingen per seconde (IOPS) en MBps, waarbij MBps = 10^6 bytes per seconde.
* Gegevensschijven kunnen in de modus met of zonder caching werken.  Voor schijfbewerkingen met gegevenscaching is de cachemodus van de host ingesteld op **ReadOnly** of **ReadWrite**.  Voor schijfbewerkingen zonder gegevenscaching is de cachemodus van de host ingesteld op **Geen**.
* De maximale netwerkbandbreedte is de maximale geaggregeerde bandbreedte die is toegekend en toegewezen per VM-type. De maximale bandbreedte geeft richtlijnen voor het selecteren van het juiste type virtuele machine om ervoor te zorgen dat er voldoende netwerkcapaciteit beschikbaar is. Wanneer u wisselt tussen Laag, Gemiddeld, Hoog en Zeer hoog, zal de doorvoer dienovereenkomstig toenemen. De werkelijke netwerkprestaties zijn afhankelijk van talloze factoren, waaronder de netwerk- en toepassingsbelastingen en de instellingen van het toepassingsnetwerk.

## <a name="a-series"></a>A-serie
| Grootte | CPU-kernen | Geheugen: GiB | Lokale HDD: GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 |1 |0,768 |20 |1 |1 x&500; |1/laag |
| Standard_A1 |1 |1,75 |70 |2 |2 x&500; |1/gemiddeld |
| Standard_A2 |2 |3,5 |135 |4 |4 x&500; |1/gemiddeld |
| Standard_A3 |4 |7 |285 |8 |8 x&500; |2/hoog |
| Standard_A4 |8 |14 |605 |16 |16 x&500; |4/hoog |
| Standard_A5 |2 |14 |135 |4 |4 x&500; |1/gemiddeld |
| Standard_A6 |4 |28 |285 |8 |8 x&500; |2/hoog |
| Standard_A7 |8 |56 |605 |16 |16 x&500; |4/hoog |

<br>

## <a name="a-series---compute-intensive-instances"></a>A-serie: rekenintensieve exemplaren
Zie [About the H-series and compute-intensive A-series VMs](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Over virtuele machines uit de H-serie en rekenintensieve A-serie) voor informatie over en overwegingen bij het gebruik van deze grootten.

| Grootte | CPU-kernen | Geheugen: GiB | Lokale HDD: GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16 x&500; |2/hoog |
| Standard_A9* |16 |112 |382 |16 |16 x&500; |4/zeer hoog |
| Standard_A10 |8 |56 |382 |16 |16 x&500; |2/hoog |
| Standard_A11 |16 |112 |382 |16 |16 x&500; |4/zeer hoog |

*RDMA-compatibel

<br>

## <a name="av2-series"></a>Av2-serie

| Grootte        | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A1_v2 | 1         | 2            | 10                   | 2              | 2 x&500;              | 1/gemiddeld              |
| Standard_A2_v2 | 2         | 4            | 20                   | 4              | 4 x&500;              | 2/gemiddeld              |
| Standard_A4_v2 | 4         | 8            | 40                   | 8              | 8 x&500;              | 4/hoog                  |
| Standard_A8_v2 | 8         | 16           | 80                   | 16             | 16 x&500;             | 8/hoog                  |
| Standard_A2m_v2 | 2        | 16           | 20                   | 4              | 4 x&500;              | 2/gemiddeld              |
| Standard_A4m_v2 | 4        | 32           | 40                   | 8              | 8 x&500;              | 4/hoog                  |
| Standard_A8m_v2 | 8        | 64           | 80                   | 16             | 16 x&500;             | 8/hoog                  |


## <a name="d-series"></a>D-serie
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1 |1 |3,5 |50 |2 |2 x&500; |1/gemiddeld |
| Standard_D2 |2 |7 |100 |4 |4 x&500; |2/hoog |
| Standard_D3 |4 |14 |200 |8 |8 x&500; |4/hoog |
| Standard_D4 |8 |28 |400 |16 |16 x&500; |8/hoog |
| Standard_D11 |2 |14 |100 |4 |4 x&500; |2/hoog |
| Standard_D12 |4 |28 |200 |8 |8 x&500; |4/hoog |
| Standard_D13 |8 |56 |400 |16 |16 x&500; |8/hoog |
| Standard_D14 |16 |112 |800 |32 |32 x&500; |8/zeer hoog |

<br>

## <a name="dv2-series"></a>Dv2-serie
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1_v2 |1 |3,5 |50 |2 |2 x&500; |1/gemiddeld |
| Standard_D2_v2 |2 |7 |100 |4 |4 x&500; |2/hoog |
| Standard_D3_v2 |4 |14 |200 |8 |8 x&500; |4/hoog |
| Standard_D4_v2 |8 |28 |400 |16 |16 x&500; |8/hoog |
| Standard_D5_v2 |16 |56 |800 |32 |32 x&500; |8/zeer hoog |
| Standard_D11_v2 |2 |14 |100 |4 |4 x&500; |2/hoog |
| Standard_D12_v2 |4 |28 |200 |8 |8 x&500; |4/hoog |
| Standard_D13_v2 |8 |56 |400 |16 |16 x&500; |8/hoog |
| Standard_D14_v2 |16 |112 |800 |32 |32 x&500; |8/zeer hoog |
| Standard_D15_v2** |20 |140 |1000 |40 |40 x&500; |8/zeer hoog* |

*In sommige regio's is Versneld netwerken beschikbaar voor de grootte Standard_D15_v2. Voor meer informatie over het gebruik en de beschikbaarheid raadpleegt u [Accelerated Networking is in Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) (Preview-versie van Versneld netwerken) en [Accelerated Networking for a virtual machine](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md) (Versneld netwerken voor een virtuele machine).

**Exemplaar is geïsoleerd voor hardware, speciaal voor één klant.

<br>

## <a name="ds-series"></a>DS-serie*
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer voor schijf met caching: IOPS/MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4000 / 32 (43) |3200 / 32 |1/gemiddeld |
| Standard_DS2 |2 |7 |14 |4 |8000 / 64 (86) |6400 / 64 |2/hoog |
| Standard_DS3 |4 |14 |28 |8 |16.000 / 128 (172) |12.800 / 128 |4/hoog |
| Standard_DS4 |8 |28 |56 |16 |32.000 / 256 (344) |25.600 / 256 |8/hoog |
| Standard_DS11 |2 |14 |28 |4 |8000 / 64 (72) |6400 / 64 |2/hoog |
| Standard_DS12 |4 |28 |56 |8 |16.000 / 128 (144) |12.800 / 128 |4/hoog |
| Standard_DS13 |8 |56 |112 |16 |32.000 / 256 (288) |25.600 / 256 |8/hoog |
| Standard_DS14 |16 |112 |224 |32 |64.000 / 512 (576) |51.200 / 512 |8/zeer hoog |

MBps = 10^6 bytes per seconde en GiB = 1024^3 bytes.

*De maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de DS-serie kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven.  Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.

<br>

## <a name="dsv2-series"></a>DSv2-serie*
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer voor schijf met caching: IOPS/MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4000 / 32 (43) |3200 / 48 |1 gemiddeld |
| Standard_DS2_v2 |2 |7 |14 |4 |8000 / 64 (86) |6400 / 96 |2 hoog |
| Standard_DS3_v2 |4 |14 |28 |8 |16.000 / 128 (172) |12.800 / 192 |4 hoog |
| Standard_DS4_v2 |8 |28 |56 |16 |32.000 / 256 (344) |25.600 / 384 |8 hoog |
| Standard_DS5_v2 |16 |56 |112 |32 |64.000 / 512 (688) |51.200 / 768 |8 zeer hoog |
| Standard_DS11_v2 |2 |14 |28 |4 |8000 / 64 (72) |6400 / 96 |2 hoog |
| Standard_DS12_v2 |4 |28 |56 |8 |16.000 / 128 (144) |12.800 / 192 |4 hoog |
| Standard_DS13_v2 |8 |56 |112 |16 |32.000 / 256 (288) |25.600 / 384 |8 hoog |
| Standard_DS14_v2 |16 |112 |224 |32 |64.000 / 512 (576) |51.200 / 768 |8 zeer hoog |
| Standard_DS15_v2*** |20 |140 |280 |40 |80.000 / 640 (720) |64.000 / 960 |8 zeer hoog** |

MBps = 10^6 bytes per seconde en GiB = 1024^3 bytes.

*De maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de DSv2-serie kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven.  Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.

**In sommige regio's is Versneld netwerken beschikbaar voor de grootte Standard_DS15_v2. Voor meer informatie over het gebruik en de beschikbaarheid raadpleegt u [Accelerated Networking is in Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) (Preview-versie van Versneld netwerken) en [Accelerated Networking for a virtual machine](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md) (Versneld netwerken voor een virtuele machine).

***Exemplaar is geïsoleerd voor hardware, speciaal voor één klant.
<br>

## <a name="f-series"></a>F-serie
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer schijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_F1 |1 |2 |16 |2 |2 x&500; |1/gemiddeld |
| Standard_F2 |2 |4 |32 |4 |4 x&500; |2/hoog |
| Standard_F4 |4 |8 |64 |8 |8 x&500; |4/hoog |
| Standard_F8 |8 |16 |128 |16 |16 x&500; |8/hoog |
| Standard_F16 |16 |32 |256 |32 |32 x&500; |8/zeer hoog |

<br>

## <a name="fs-series"></a>Fs-serie*
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer voor schijf met caching: IOPS/MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4000 / 32 (12) |3200 / 48 |1/gemiddeld |
| Standard_F2s |2 |4 |8 |4 |8000 / 64 (24) |6400 / 96 |2/hoog |
| Standard_F4s |4 |8 |16 |8 |16.000 / 128 (48) |12.800 / 192 |4/hoog |
| Standard_F8s |8 |16 |32 |16 |32.000 / 256 (96) |25.600 / 384 |8/hoog |
| Standard_F16s |16 |32 |64 |32 |64.000 / 512 (192) |51.200 / 768 |8/zeer hoog |

MBps = 10^6 bytes per seconde en GiB = 1024^3 bytes.

*De maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de Fs-serie kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven.  Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.

<br>

## <a name="g-series"></a>G-serie
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer schijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_G1 |2 |28 |384 |4 |4 x 500 |1/hoog |
| Standard_G2 |4 |56 |768 |8 |8 x 500 |2/hoog |
| Standard_G3 |8 |112 |1536 |16 |16 x 500 |4/zeer hoog |
| Standard_G4 |16 |224 |3072 |32 |32 x 500 |8/zeer hoog |
| Standard_G5* |32 |448 |6144 |64 |64 x 500 |8/zeer hoog |

*Exemplaar is geïsoleerd voor hardware, speciaal voor één klant.
<br>

## <a name="gs-series"></a>GS-serie*
| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer voor schijf met caching: IOPS/MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10.000 / 100 (264) |5000 / 125 |1/hoog |
| Standard_GS2 |4 |56 |112 |8 |20.000 / 200 (528) |10.000 / 250 |2/hoog |
| Standard_GS3 |8 |112 |224 |16 |40.000 / 400 (1,056) |20.000 / 500 |4/zeer hoog |
| Standard_GS4 |16 |224 |448 |32 |80.000 / 800 (2,112) |40.000 / 1,000 |8/zeer hoog |
| Standard_GS5** |32 |448 |896 |64 |160.000 / 1600 (4,224) |80.000 / 2000 |8/zeer hoog |

MBps = 10^6 bytes per seconde en GiB = 1024^3 bytes.

*De maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de GS-serie kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven. 

**Exemplaar is geïsoleerd voor hardware, speciaal voor één klant.
<br>

## <a name="h-series"></a>H-serie
Virtuele Azure-machines uit de H-serie zijn de volgende generatie HPC-VM's, gericht op intensieve rekenbehoeften, zoals moleculaire modellering en numerieke stromingsleer. Deze virtuele machines met 8 en 16 kernen zijn gebaseerd op de Intel Haswell E5 2667 V3 processor-technologie met DDR4-geheugen en lokale op SSD gebaseerde opslag. 

Naast een zeer hoge CPU-kracht biedt de H-serie ook verschillende opties voor RDMA-netwerken met lage latentie met gebruik van FDR InfiniBand, evenals verschillende geheugenconfiguraties om geheugenintensieve rekenvereisten te ondersteunen.

Zie [About the H-series and compute-intensive A-series VMs](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Over virtuele machines uit de H-serie en rekenintensieve A-serie) voor informatie over en overwegingen bij het gebruik van deze grootten.

| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer schijf: IOPS | Max. aantal NIC's/netwerkbandbreedte |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16 x 500 |2/hoog |
| Standard_H16 |16 |112 |2000 |32 |32 x 500 |4/zeer hoog |
| Standard_H8m |8 |112 |1000 |16 |16 x 500 |2/hoog |
| Standard_H16m |16 |224 |2000 |32 |32 x 500 |4/zeer hoog |
| Standard_H16r* |16 |112 |2000 |32 |32 x 500 |4/zeer hoog |
| Standard_H16mr* |16 |224 |2000 |32 |32 x 500 |4/zeer hoog |

*RDMA-compatibel

<br>


## <a name="ls-series"></a>Ls-serie 

De Ls-serie is geoptimaliseerd voor workloads die lokale opslag met lage latentie vereisen, zoals NoSQL-databases (bijvoorbeeld Cassandra, MongoDB, Cloudera en Redis). De Ls-serie biedt maximaal 32 CPU-kernen en maakt gebruik van een [Intel Xeon®-processor uit de E5 v3-familie](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Deze biedt dezelfde CPU-prestaties als de G/GS-serie en wordt geleverd met 8 GiB geheugen per CPU-kern.  

 
| Grootte          | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | Max. aantal gegevensschijven | Max. doorvoer voor schijf met caching: IOPS/MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's/netwerkbandbreedte | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | N.V.T./N.V.T. (0)          | 5000 / 125                               | 2/hoog       | 
| Standard_L8s  | 8    | 64   | 1388 | 16             | N.V.T./N.V.T. (0)          | 10.000 / 250                              | 4/zeer hoog  | 
| Standard_L16s | 16   | 128  | 2807 | 32             | N.V.T./N.V.T. (0)          | 20.000 / 500                              | 8/zeer hoog | 
| Standard_L32s | 32   | 256  | 5630 | 64             | N.V.T./N.V.T. (0)          | 40.000 / 1,000                            | 8/zeer hoog | 
 
MBps = 10^6 bytes per seconde en GiB = 1024^3 bytes. 



## <a name="n-series"></a>N-serie
De NC- en NV-grootten worden ook wel GPU-exemplaren genoemd. Dit zijn speciale virtuele machines die NVIDIA GPU-kaarten bevatten, geoptimaliseerd voor verschillende scenario’s en toepassingen. De NV-grootten zijn geoptimaliseerd en ontworpen voor externe visualisatie, streaming, games, codering en VDI-scenario’s waarin frameworks als OpenGL en DirectX worden gebruikt. De NC-grootten zijn meer geoptimaliseerd voor reken- en netwerkintensieve toepassingen en algoritmen, waaronder CUDA - en OpenCL-toepassingen en -simulaties. 


### <a name="nv-instances"></a>NV-exemplaren
De NV-exemplaren worden aangestuurd door een NVIDIA Tesla M60 GPU-kaart en NVIDIA GRID voor bureaubladtoepassingen en virtuele bureaubladen waarmee klanten hun gegevens of simulatie kunnen visualiseren. Gebruikers kunnen hun grafisch intensieve werkstromen op de NV-exemplaren visualiseren voor grafische mogelijkheden en daarnaast enkelvoudige precisieworkloads uitvoeren, zoals codering en rendering. De Tesla M60 biedt 4096 CUDA-kernen in een ontwerp met dubbele GPU, met maximaal 36 streams van 1080p H.264. 


| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 |
| Standard_NV12 |12 |112 |680 | 2 |
| Standard_NV24 |24 |224 |1440 | 4 |

1 GPU = halve M60-kaart.

**Ondersteunde besturingssystemen**

* Windows Server 2016, Windows Server 2012 R2; zie [N-series driver setup for Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md) (Stuurprogramma’s uit de N-serie instellen voor Windows)

### <a name="nc-instances"></a>NC-exemplaren
De NC-exemplaren worden aangestuurd door een NVIDIA Tesla R80-kaart. Gebruikers kunnen gegevens nu veel sneller verwerken door gebruik te maken van CUDA voor energie-exploratietoepassingen, crashsimulaties, ray traced rendering, 'deep learning' en meer. De Tesla R80 levert 4992 CUDA-kernen met een ontwerp met dubbele GPU prestaties tot 2,91 Teraflops dubbele precisie en maximaal 8,93 Teraflops enkele precisie.

| Grootte | CPU-kernen | Geheugen: GiB | Lokale SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 |

1 GPU = halve K80-kaart.

*RDMA-compatibel

**Ondersteunde besturingssystemen**

* Windows Server 2016, Windows Server 2012 R2; zie [N-series driver setup for Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md) (Stuurprogramma’s uit de N-serie instellen voor Windows)
* Ubuntu 16.04 TNS; zie [N-series driver setup for Linux](../articles/virtual-machines/virtual-machines-linux-n-series-driver-setup.md) (Stuurprogramma’s uit de N-serie instellen voor Linux)

<br>

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Opmerkingen: Standard A0 - A4 met CLI en PowerShell
In het klassieke implementatiemodel verschillen sommige namen van VM-grootten enigszins in CLI en in PowerShell:

* Standard_A0 is ExtraSmall 
* Standard_A1 is Small
* Standard_A2 is Medium
* Standard_A3 is Large
* Standard_A4 is ExtraLarge

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie: [Azure subscription and service limits, quotas, and constraints](../articles/azure-subscription-service-limits.md) (Limieten van Azure-abonnementen en -services, quota en beperkingen).
* Zie voor meer informatie: [About the H-series and compute-intensive A-series VMs](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Over virtuele machines uit de H-serie en rekenintensieve A-serie) voor HPC-workloads (High Performance Computing).



<!--HONumber=Feb17_HO3-->


