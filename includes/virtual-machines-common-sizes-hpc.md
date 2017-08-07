<!-- A-series - compute-intensive instances, H-series -->

Grootten uit de A8-A11- en H-serie worden ook wel *rekenintensieve exemplaren* genoemd. De hardware waarop deze grootten worden uitgevoerd, is ontworpen en geoptimaliseerd voor rekenintensieve en netwerkintensieve toepassingen, waaronder HPC-clustertoepassingen (high-performance computing), modellerings- en simulatietoepassingen. De A8-A11-serie gebruikt Intel Xeon E5-2670 @ 2,6 GHZ en de H-serie gebruikt Intel Xeon E5-2667 v3 @ 3,2 GHz. 

Virtuele Azure-machines uit de H-serie zijn de volgende generatie HPC-VM's, gericht op intensieve rekenbehoeften, zoals moleculaire modellering en numerieke stromingsleer. Deze VM's met 8 en 16 vCPU zijn gebaseerd op de Intel Haswell E5 2667 V3 processor-technologie met DDR4-geheugen en tijdelijke op SSD gebaseerde opslag. 

Naast een zeer hoge CPU-kracht biedt de H-serie ook verschillende opties voor RDMA-netwerken met lage latentie met gebruik van FDR InfiniBand, evenals verschillende geheugenconfiguraties om geheugenintensieve rekenvereisten te ondersteunen.



## <a name="h-series"></a>H-serie

ACU: 290-300

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Max. doorvoer schijf: IOPS | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16 x 500 |2  |
| Standard_H16 |16 |112 |2000 |32 |32 x 500 |4 |
| Standard_H8m |8 |112 |1000 |16 |16 x 500 |2  |
| Standard_H16m |16 |224 |2000 |32 |32 x 500 |4  |
| Standard_H16r* |16 |112 |2000 |32 |32 x 500 |4  |
| Standard_H16mr* |16 |224 |2000 |32 |32 x 500 |4 |

*Toegewezen RDMA-back-endnetwerk is ingeschakeld voor MPI-toepassingen via FDR InfiniBand-netwerk, dat zeer lage latentie en hoge bandbreedte biedt.

<br>



## <a name="a-series---compute-intensive-instances"></a>A-serie: rekenintensieve exemplaren

ACU: 225

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (HDD): GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16 x 500 |2 |
| Standard_A9* |16 |112 |382 |16 |16 x 500 |4 |
| Standard_A10 |8 |56 |382 |16 |16 x 500 |2  |
| Standard_A11 |16 |112 |382 |16 |16 x 500 |4 |

*Toegewezen RDMA-back-endnetwerk is ingeschakeld voor MPI-toepassingen via FDR InfiniBand-netwerk, dat zeer lage latentie en hoge bandbreedte biedt.

<br>



