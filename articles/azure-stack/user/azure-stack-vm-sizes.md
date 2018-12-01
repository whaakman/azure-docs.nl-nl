---
title: Grootten van virtuele machines worden ondersteund in Azure Stack | Microsoft Docs
description: Referentie voor de ondersteunde VM-grootten in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 5127b748111f993f95f940f4add81c42a032e038
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722085"
---
# <a name="virtual-machine-sizes-supported-in-azure-stack"></a>Grootten van virtuele machines worden ondersteund in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In dit artikel geeft een lijst van de virtuele machine (VM)-grootten die beschikbaar in Azure Stack zijn.

## <a name="general-purpose"></a>Algemeen doel

VM-grootten voor algemeen gebruik bieden een evenwichtige CPU-geheugenverhouding. Ze worden gebruikt voor testen en ontwikkelen, kleine tot middelgrote databases en met weinig of gemiddeld verkeer webservers. Elke gegevensschijf is 2300 IOP's voor de premium VM-grootten, met uitzondering van de Basic A-serie. Basic a is de grootte van de gegevensschijf 500 IOPS.

### <a name="basic-a"></a>Basic A

> [!NOTE]
> *Basic A* grootten van virtuele machines buiten gebruik worden gesteld voor [het maken van virtuele-machineschaalsets](../azure-stack-compute-add-scalesets.md) (VMSS) via de portal. Als u wilt een VMSS te maken met deze grootte, PowerShell of een sjabloon te gebruiken.

|Grootte - grootte\naam |vCPU     |Geheugen | Maximumgrootte van tijdelijke schijf | Max OS schijfdoorvoer: (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. doorvoer gegevensschijf (IOPS) | Max. aantal NIC's |    
|-----------------|-----|---------|---------|-----|------|-----------|----|
|**A0\Basic_A0**  |1    |768 MB   | 20 GB   |300  | 300  |1 / 1 x 300  |1   |
|**A1\Basic_A1**  |1    |1,75 GB  | 40 GB   |300  | 300  |2 / 2 x 300  |1   |
|**A2\Basic_A2**  |2    |3,5 GB   | 60 GB   |300  | 300  |4 / 4 x 300  |1   |
|**A3\Basic_A3**  |4    |7 GB     | 120 GB  |300  | 300  |8 / 8 x 300  |1   |
|**A4\Basic_A4**  |8    |14 GB    | 240 GB  |300  | 300  |16 / 16 X 300 |1   |

### <a name="standard-a"></a>Standard A 
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |    
|----------------|--|------|----|----|----|-------|---------|
|**Standard_A0** |1 |0,768 |20  |500 |500 |1 x 500  |1 / 100  |
|**Standard_A1** |1 |1,75  |70  |500 |500 |2 x 500  |1 / 500  |
|**Standard_A2** |2 |3,5   |135 |500 |500 |4 x 500  |1 / 500  |
|**Standard_A3** |4 |7     |285 |500 |500 |8 x 500  |2 / 1000 |
|**Standard_A4** |8 |14    |605 |500 |500 |16 x 500 |4 / 2000 |
|**Standard_A5** |2 |14    |135 |500 |500 |4 x 500  |2 / 500  |
|**Standard_A6** |4 |28    |285 |500 |500 |8 x 500  |2 / 1000 |
|**Standard_A7** |8 |56    |605 |500 |500 |16 x 500 |4 / 2000 |

### <a name="av2-series"></a>Av2-serie
*Azure Stack 1804 is of hoger vereist*

|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|-----------------|----|----|-----|-----|------|--------------|---------|
|**Standard_A1_v2**  |1   |2   |10   |500 |1000  |2 / 2 x 500   |2 / 250  |
|**Standard_A2_v2**  |2   |4   |20   |500 |2000  |4 / 4 x 500   |2 / 500  |
|**Standard_A4v2**   |4   |8   |40   |500 |4000  |8 / 8 x 500   |4 / 1000 |
|**Standard_A8_v2**  |8   |16  |80   |500 |8000  |16 / 16 x 500 |8 / 2000 |
|**Standard_A2m_v2** |2   |16  |20   |500 |2000  |4 / 4 x 500   |2 / 500  |
|**Standard_A4m_v2** |4   |32  |40   |500 |4000  |8 / 8 x 500   |4 / 1000 |
|**Standard_A8m_v2** |8   |64  |80   |500 |8000  |16 / 16 x 500 |8 / 2000 |

### <a name="d-series"></a>D-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|----------------|----|----|-----|----|------|------------|---------|
|**Standard_D1** |1   |3,5 |50   |500 |3000  |4 / 4 x 500   |1 / 500  |
|**Standard_D2** |2   |7   |100  |500 |6000  |8 / 8 x 500   |2 / 1000 |
|**Standard_D3** |4   |14  |200  |500 |12000 |16 / 16 x 500 |4 / 2000 |
|**Standard_D4** |8   |28  |400  |500 |24000 |32 / 32 x 500 |8 / 4000 |


### <a name="ds-series"></a>DS-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|-----------------|----|----|-----|-----|------|-------------|---------|
|**Standard_DS1** |1   |3,5 |7    |1000 |4000  |4 / 4x2300   |1 / 500  |
|**Standard_DS2** |2   |7   |14   |1000 |8000  |8 / 8 x 2300   |2 / 1000 |
|**Standard_DS3** |4   |14  |28   |1000 |16000 |16 / 16 x 2300 |4 / 2000 |
|**Standard_DS4** |8   |28  |56   |1000 |32000 |32 / 32x2300 |8 / 4000 |

### <a name="dv2-series"></a>Dv2-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3,5 |50   |500 |3000  |4 / 4 x 500   |1 / 500  |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8 / 8 x 500   |2 / 1000 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16 / 16 x 500 |4 / 2000 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32 / 32 x 500 |8 / 4000 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64 / 64 x 500 |8 / 8000 |

### <a name="dsv2-series"></a>DSv2-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3,5 |7   |1000 |4000  |4 / 4x2300   |1 / 750  |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8 / 8 x 2300   |2 / 1500 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16 / 16 x 2300 |4 / 3000 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32 / 32x2300 |8 / 6000 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64 / 64x2300 |8 / 10000 |


## <a name="compute-optimized"></a>Geoptimaliseerde rekenkracht
### <a name="f-series"></a>F-serie
*Azure Stack 1804 is of hoger vereist*

|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|-----------------|----|----|-----|----|------|------------|---------|
|**Standard_F1**  |1   |2   |16   |500 |3000  |4 / 4 x 500   |2 / 750  |
|**Standard_F2**  |2   |4   |32   |500 |6000  |8 / 8 x 500   |2 / 1500 |
|**Standard_F4**  |4   |8   |64   |500 |12000 |16 / 16 x 500 |4 / 3000 |
|**Standard_F8**  |8   |16  |128  |500 |24000 |32 / 32 x 500 |8 / 6000 |
|**Standard_F16** |16  |32  |256  |500 |48000 |64 / 64 x 500 |8 / 6000 12000  |


### <a name="fs-series"></a>Fs-serie
*Azure Stack 1804 is of hoger vereist*  

|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|------------------|----|----|----|-----|------|-------------|---------|
|**Standard_F1s**  |1   |2   |4   |1000 |4000  |4 / 4x2300   |2 / 750  |
|**Standard_F2s**  |2   |4   |8   |1000 |8000  |8 / 8 x 2300   |2 / 1500 |
|**Standard_F4s**  |4   |8   |16  |1000 |16000 |16 / 16 x 2300 |4 / 3000 |
|**Standard_F8s**  |8   |16  |32  |1000 |32000 |32 / 32x2300 |8 / 6000 |
|**Standard_F16s** |16  |32  |64  |1000 |64000 |64 / 64x2300 |8 / 6000 12000  |


### <a name="fsv2-series"></a>Fsv2-serie
*Azure Stack 1804 is of hoger vereist* 

|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_F2s_v2**  |2   |4   |16   |1000 |4000   |4 / 4x2300    |Gemiddeld |
|**Standard_F4s_v2**  |4   |8   |32   |1000 |8000   |8 / 8 x 2300    |Gemiddeld |
|**Standard_F8s_v2**  |8   |16  |64   |1000 |16000  |16 / 16 x 2300  |Hoog     |
|**Standard_F16s_v2** |16  |32  |128  |1000 |32000  |32 / 32x2300  |Hoog     |
|**Standard_F32s_v2** |32  |64  |256  |1000 |64000  |32 / 32x2300  |Hoog  |
|**Standard_F64s_v2** |64  |128 |512  |1000 |128000 |32 / 32x2300  |Zeer hoog  |


## <a name="memory-optimized"></a>Geoptimaliseerd geheugen

Geheugen geoptimaliseerde VM-grootten bieden een hoge geheugen-naar-CPU-snelheid die is ontworpen voor relationele-databaseservers, middelgrote tot grote caches en analysefuncties in het geheugen.

### <a name="mo-d"></a>Uit de D-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|------------------|---|----|----|--------|------|------------|---------|
|**Standard_D11**  |2  |14  |100 |500     |6000  |8 / 8 x 500   |2 / 1000 |
|**Standard_D12**  |4  |28  |200 |500     |12000 |16 / 16 x 500 |4 / 2000 |
|**Standard_D13**  |8  |56  |400 |500     |24000 |32 / 32 x 500 |8 / 4000 |
|**Standard_D14**  |16 |112 |800 |500     |48000 |64 / 64 x 500 |8 / 8000 |

### <a name="mo-ds"></a>DS-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|-------------------|---|----|----|--------|------|-------------|---------|
|**Standard_DS11**  |2  |14  |28  |1000    |8000  |8 / 8 x 2300   |2 / 1000 |
|**Standard_DS12**  |4  |28  |56  |1000    |12000 |16 / 16 x 2300 |4 / 2000 |
|**Standard_DS13**  |8  |56  |112 |1000    |32000 |32 / 32x2300 |8 / 4000 |
|**Standard_DS14**  |16 |112 |224 |1000    |64000 |64 / 64x2300 |8 / 8000 |

### <a name="mo-dv2"></a>Dv2-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8 / 8 x 500    |2 / 1500 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16 / 16 x 500  |4 / 3000 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32 / 32 x 500  |8 / 6000 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64 / 64 x 500  |8 / 10000 |


### <a name="mo-dsv2"></a>DSv2-serie
|Grootte     |vCPU     |Geheugen (GiB) | Tijdelijke opslag (GiB)  | Besturingssysteem van de maximale schijfdoorvoer (IOPS) | Maximale tijdelijke opslagdoorvoer (IOPS) | Max. aantal gegevensschijven / doorvoer (IOPS) | Max. NIC's / verwachte netwerkbandbreedte (Mbps) |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |8 / 8 x 2300    |2 / 1500 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |16 / 16 x 2300  |4 / 3000 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |32 / 32x2300  |8 / 6000 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |64 / 64x2300  |8 / 10000 |


## <a name="next-steps"></a>Volgende stappen

[Overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md)
