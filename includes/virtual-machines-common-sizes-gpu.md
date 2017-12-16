
GPU geoptimaliseerd VM grootten gespecialiseerde virtuele machines met één of meerdere NVIDIA GPU's beschikbaar zijn. Deze grootten zijn ontworpen voor rekenintensieve en grafisch-intensieve visualisatie werkbelastingen. In dit artikel bevat informatie over het aantal en type GPU's, Vcpu, gegevensschijven, en NIC's, evenals opslag doorvoer en bandbreedte voor elke grootte in deze groepering van. 

* **NC NCv2 en ND** grootten zijn geoptimaliseerd voor algoritmen, zoals CUDA - en OpenCL-toepassingen en simulaties, AI en grondige Learning en rekenintensieve en netwerk-intensieve toepassingen. 
* **NV** grootten zijn geoptimaliseerd en ontworpen om externe visualisatie, streaming games, codering en VDI-scenario's met behulp van frameworks zoals OpenGL- en DirectX.  


## <a name="nc-instances"></a>NC-exemplaren

De NC-exemplaren zijn ingeschakeld door de [NVIDIA Tesla R80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) kaart. Gebruikers kunnen crisis door gegevens sneller door gebruik te maken van CUDA voor energie exploratie toepassingen, crash simulaties, ray getraceerde rendering, grondige learning en meer. De configuratie van de NC24r biedt een lage latentie en hoge gegevensdoorvoer netwerkinterface is geoptimaliseerd voor nauw gekoppeld parallelle workloads computing.


| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Maximumaantal gegevensschijven |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 |

1 GPU = halve K80-kaart.

*RDMA-compatibel

## <a name="ncv2-instances"></a>NCv2 exemplaren

NCv2 exemplaren zijn van de machines NC-serie, aangedreven door de volgende generatie [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU's. Deze GPU's kunnen meer dan 2 x de rekenkundige prestaties van de huidige NC-reeks opgeven. Klanten kunnen profiteren van deze bijgewerkte GPU's voor traditionele HPC-workloads zoals modellering van DNA sequentiëren, eiwit analyse, Monte Carlo-simulaties en anderen. Als de NC-reeks biedt de NCv2-reeks een configuratie met een lage latentie en hoge gegevensdoorvoer netwerkinterface is geoptimaliseerd voor nauw gekoppeld parallelle workloads computing.

> [!IMPORTANT]
> Voor deze familie grootte, is het quotum vCPU (core) in uw abonnement ingesteld op 0 in elke regio. [Aanvragen van een verhoging van het quotum vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze reeks in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Maximumaantal gegevensschijven |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6_v2 |6 |112 | 336 | 1 | 12 |
| Standard_NC12_v2 |12 |224 | 672 | 2 | 24 |
| Standard_NC24_v2 |24 |448 | 1344 | 4 | 32 |
| Standard_NC24r_v2 * |24 |1448 | 1344 | 4 | 32 |

1 GPU = één P100-kaart.

*RDMA-compatibel

## <a name="nd-instances"></a>ND-exemplaren

De ND-serie virtuele machines zijn een nieuwe aanvulling op de GPU-familie die zijn bestemd voor AI en grondige Learning werkbelastingen. Ze bieden uitstekende prestaties voor trainings- en Deductie. ND-exemplaren worden van stroom voorzien door [NVIDIA Tesla p 40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU's. Deze instanties bieden uitstekende prestaties voor enkele precisie drijvende punt bewerkingen, voor gebruik van Microsoft cognitieve Toolkit, TensorFlow Caffe en andere frameworks AI-werkbelastingen. De ND-serie biedt daarnaast een veel groter GPU-geheugen (24 GB), waardoor het mogelijk is om veel grotere modellen met een neuraal netwerk in te zetten. Net als bij de NC-serie, krijgen de ND-reeks een configuratie met een secundaire lage latentie, een hoge gegevensdoorvoer netwerk via RDMA, en InfiniBand connectiviteit, zodat u taken van grootschalige training spanning veel GPU's kunt uitvoeren.

> [!IMPORTANT]
> Voor deze reeks grootte is vCPU (core) quota per regio in uw abonnement ingesteld op 0. [Aanvragen van een verhoging van het quotum vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze reeks in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Maximumaantal gegevensschijven |
| --- | --- | --- | --- | --- | --- |
| Standard_ND6 |6 |112 | 336 | 1 | 12 |
| Standard_ND12 |12 |224 | 672 | 2 | 24 |
| Standard_ND24 |24 |448 | 1344 | 4 | 32 |
| Standard_ND24r * |24 |1448 | 1344 | 4 | 32 |

1 GPU = één p 40-kaart.

*RDMA-compatibel

## <a name="nv-instances"></a>NV-exemplaren



De exemplaren NV worden van stroom voorzien door [NVIDIA Tesla M60 ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU's en NVIDIA RASTER technologie voor bureaublad versnelde toepassingen en virtuele bureaubladen waarop klanten kunnen hun gegevens of simulaties visualiseren. Gebruikers kunnen hun grafisch geheugenintensieve werkstromen op de exemplaren NV krijgen hogere grafische mogelijkheden en ook uitvoeren met enkele precisie werkbelastingen, zoals codering en -rendering visualiseren. 

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Maximumaantal gegevensschijven |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 |
| Standard_NV12 |12 |112 |680 | 2 | 48 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 |

1 GPU = halve M60-kaart.


