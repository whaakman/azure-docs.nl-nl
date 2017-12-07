 



We hebben het concept van de ACU (Azure Compute Unit) ontwikkeld om een manier te bieden waarop rekenprestaties (CPU) van Azure-SKU's met elkaar kunnen worden vergeleken. Op die manier kunt u eenvoudig bepalen welke SKU hoogstwaarschijnlijk aan uw prestatiebehoeften zal voldoen.  ACU is momenteel gestandaardiseerd op 100 voor een kleine virtuele machine (Standard_A1). Alle andere SKU's geven vervolgens weer hoeveel sneller die SKU een standaardbenchmark ongeveer kan uitvoeren. 

> [!IMPORTANT]
> De ACU is slechts een richtlijn.  De resultaten voor uw workload kunnen verschillen. 
> 
> 

<br>

| SKU-familie | ACU \ vCPU | vCPU:Core |
| --- | --- |---|
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 | 1:1 |
| [A1-A4](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A5-A7](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A1_v2-A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A2m_v2-A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A8-A11](../articles/virtual-machines/windows/sizes-hpc.md) |225* | 1:1 |
| [D1-D14](../articles/virtual-machines/windows/sizes-general.md) |160 | 1:1 |
| [D1_v2-D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210 - 250* | 1:1 |
| [DS1-DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 | 1:1 |
| [DS1_v2-DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* | 1:1 |
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** |
| [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** |
| [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** |
| [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** |
| [F2s_v2 F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195-210* | 2:1** |
| [F1-F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [F1s-F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [G1-G5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* | 1:1 |
| [GS1-GS5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* | 1:1 |
| [H](../articles/virtual-machines/windows/sizes-hpc.md) |290 - 300* | 1:1 |
| [L4s-L32s](../articles/virtual-machines/windows/sizes-storage.md) |180 - 240* | 1:1 |
| [M](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) | 160-180 | 2:1** |

ACU's die met een * zijn gemarkeerd, maken gebruik van Intel® Turbo-technologie om de CPU-frequentie te verhogen en nóg betere prestaties te leveren.  Hoe groot die extra prestaties zijn, is afhankelijk van de VM-grootte, de workload en de andere workloads die op dezelfde host worden uitgevoerd.

**Hyper-threaded. 
