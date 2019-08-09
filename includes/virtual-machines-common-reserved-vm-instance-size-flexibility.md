---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: 790fb39c3d8964c053ffe1d7ee04418fcbc0913c
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68857359"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Flexibele grootte van virtuele machines met gereserveerde VM-instanties

Met een gereserveerde virtuele-machine-instantie die is geoptimaliseerd voor flexibiliteit van de instantie grootte, kan de door u gekochte reserve ring gelden voor de grootte van virtuele machines (Vm's) in dezelfde grootte reeks groep. Als u bijvoorbeeld een reserve ring aanschaft voor een VM-grootte die wordt vermeld in de tabel DSv2, zoals Standard_DS5_v2, kan de reserverings korting gelden voor de andere vier grootten die in diezelfde tabel worden weer gegeven:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Maar dat de reserverings korting niet van toepassing is op Vm's die in verschillende tabellen worden weer gegeven, zoals in de tabel DSv2-serie high memory: Standard_DS11_v2, Standard_DS12_v2, enzovoort.

In de groep grootte reeks is het aantal Vm's waarop de reserverings korting van toepassing is afhankelijk van de grootte van de virtuele machine die u kiest wanneer u een reserve ring koopt. Het is ook afhankelijk van de grootte van de virtuele machines die u uitvoert. In de kolom verhouding die wordt weer gegeven in de volgende tabellen wordt de relatieve footprint voor elke VM-grootte in die groep vergeleken. Gebruik de verhouding waarde om te berekenen hoe de reserverings korting van toepassing is op de virtuele machines die u uitvoert.

## <a name="examples"></a>Voorbeelden

De volgende voor beelden gebruiken de grootten en verhoudingen in de tabel DSv2.

 U koopt een gereserveerde VM-instantie met de grootte Standard_DS4_v2 waarbij de verhouding of de relatieve footprint vergeleken met de andere grootten in die reeks 8 is.

- Scenario 1: Voer acht Standard_DS1_v2-grootte-Vm's uit met een ratio van 1. Uw reserverings korting is van toepassing op alle acht Vm's.
- Scenario 2: Voer twee Standard_DS2_v2-formaat Vm's uit met een ratio van 2 elke. Voer ook een Standard_DS3_v2-VM-grootte uit met een ratio van 4. De totale footprint is 2 + 2 + 4 = 8. Daarom is uw reserverings korting van toepassing op alle drie de virtuele machines.
- Scenario 3: Voer één Standard_DS5_v2 uit met een ratio van 16. Uw reserverings korting is van toepassing op de helft van de reken kosten van de virtuele machine.

In de volgende secties ziet u welke grootten zich in dezelfde reeks groep bevinden wanneer u een gereserveerde VM-instantie koopt die is geoptimaliseerd voor de flexibiliteit van de instantie grootte.

## <a name="b-series"></a>B-serie

| Size | Quote|
|---|---|
| Standard_B1s | 1 |
|Standard_B2s|4|

Zie grootten van de [virtuele machines van de B-serie](../articles/virtual-machines/windows/b-series-burstable.md)voor meer informatie.

## <a name="b-series-high-memory"></a>High-Memory van de B-serie

| Size | Quote|
|---|---|
| Standard_B1ms |1|
|Standard_B2ms|4|
|Standard_B4ms|8|
|Standard_B8ms|16|

Zie grootten van de [virtuele machines van de B-serie](../articles/virtual-machines/windows/b-series-burstable.md)voor meer informatie.

## <a name="d-series"></a>D-serie

| Size | Quote|
|---|---|
| Standard_D1|1|
|Standard_D2|2|
|Standard_D3|4|
|Standard_D4|8|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="d-series-high-memory"></a>High-Memory van D-serie

| Size | Quote|
|---|---|
| Standard_D11|1|
|Standard_D12|2|
|Standard_D13|4|
|Standard_D14|8|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series"></a>DS-serie

| Size | Quote|
|---|---|
|Standard_DS1|1|
|Standard_DS2|2|
|Standard_DS3|4|
|Standard_DS4|8|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series-high-memory"></a>Hoog geheugen van de DS-serie

| Size | Quote|
|---|---|
|Standard_DS11|1|
|Standard_DS12|2|
|Standard_DS13|4|
|Standard_DS14|8|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series"></a>DSv2-serie

| Size | Quote|
|---|---|
|Standard_DS1_v2|1|
|Standard_DS2_v2|2|
|Standard_DS3_v2|4|
|Standard_DS4_v2|8|
|Standard_DS5_v2|16|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series-high-memory"></a>Hoog geheugen van de DSv2-serie

| Size | Quote|
|---|---|
|Standard_DS11_v2|1|
|Standard_DS11-1_v2|1|
|Standard_DS12_v2|2|
|Standard_DS12-1_v2|2|
|Standard_DS12-2_v2|2|
|Standard_DS13_v2|4|
|Standard_DS13-2_v2|4|
|Standard_DS13-4_v2|4|
|Standard_DS14_v2|8|
|Standard_DS14-4_v2|8|
|Standard_DS14-8_v2|8|
|Standard_DS15_v2|10|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-high-memory-isolated-series"></a>Geïsoleerde serie DSv2 High Memory

| Size | Quote|
|---|---|
|Standard_DS15i_v2|1|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv3-series"></a>DSv3-serie

| Size | Quote|
|---|---|
|Standard_D2s_v3|1|
|Standard_D4s_v3|2|
|Standard_D8s_v3|4|
|Standard_D16s_v3|8|
|Standard_D32s_v3|16|
|Standard_D64s_v3|32|

Zie grootten [virtuele machine grootten](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-1)voor meer informatie.

## <a name="dv2-series"></a>Dv2-serie

| Size | Quote|
|---|---|
|Standard_D1_v2|1|
|Standard_D2_v2|2|
|Standard_D3_v2|4|
|Standard_D4_v2|8|
|Standard_D5_v2|16|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv2-series-high-memory"></a>Hoog geheugen van de dv2-serie

| Size | Quote|
|---|---|
|Standard_D11_v2|1|
|Standard_D12_v2|2|
|Standard_D13_v2|4|
|Standard_D14_v2|8|
|Standard_D15_v2|10|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv2--high-memory-isolated-series"></a>Geïsoleerde serie dv2 High Memory

| Size | Quote|
|---|---|
|Standard_D15i_v2|1|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).


## <a name="dv3-series"></a>Dv3-serie

| Size | Quote|
|---|---|
| Standard_D2_v3|1|
|Standard_D4_v3|2|
|Standard_D8_v3|4|
|Standard_D16_v3|8|
|Standard_D32_v3|16|
|Standard_D64_v3|32|

Zie grootten [virtuele machine grootten](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1)voor meer informatie.

## <a name="esv3-series"></a>ESv3-serie

| Size | Quote|
|---|---|
|Standard_E2s_v3|1|
|Standard_E4s_v3|2|
|Standard_E4-2s_v3|2|
|Standard_E8s_v3|4|
|Standard_E8-2s_v3|4|
|Standard_E8-4s_v3|4|
|Standard_E16s_v3|8|
|Standard_E16-4s_v3|8|
|Standard_E16-8s_v3|8|
|Standard_E20s_v3|10|
|Standard_E32s_v3|16|
|Standard_E32-8s_v3|16|
|Standard_E32-16s_v3|16|
|Standard_E64s_v3|28,8|
|Standard_E64-16s_v3|28,8|
|Standard_E64-32s_v3|28,8|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#esv3-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="esv3-series-isolated-series"></a>ESv3 Series, geïsoleerde serie

| Size | Quote|
|---|---|
|Standard_E64is_v3|1|

## <a name="ev3-series"></a>Ev3-serie

| Size | Quote|
|---|---|
|Standard_E2_v3|1|
|Standard_E4_v3|2|
|Standard_E8_v3|4|
|Standard_E16_v3|8|
|Standard_E20_v3|10|
|Standard_E32_v3|16|
|Standard_E64_v3|32|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#ev3-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="ev3-series-isolated-series"></a>Ev3 Series, geïsoleerde serie

| Size | Quote|
|---|---|
|Standard_E64i_v3|1|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#ev3-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="f-series"></a>F-serie

| Size | Quote|
|---|---|
| Standard_F1|1|
|Standard_F2|2|
|Standard_F4|4|
|Standard_F8|8|
Standard_F16|16|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fs-series"></a>FS-serie

| Size | Quote|
|---|---|
| Standard_F1s|1|
|Standard_F2s|2|
|Standard_F4s|4|
|Standard_F8s|8|
|Standard_F16s|16|

Zie voor meer informatie [vorige generaties virtuele machine grootten](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fsv2-series"></a>Fsv2-serie

| Size | Quote|
|---|---|
|Standard_F2s_v2|1|
|Standard_F4s_v2|2|
|Standard_F8s_v2|4|
|Standard_F16s_v2|8|
|Standard_F32s_v2|16|
|Standard_F64s_v2|32|
|Standard_F72s_v2|36|

Zie grootten van berekenings [geoptimaliseerde virtuele machines](../articles/virtual-machines/windows/sizes-compute.md#fsv2-series-1)voor meer informatie.

## <a name="h-series"></a>H-serie

| Size | Quote|
|---|---|
| Standard_H8|1|
|Standard_H16|2|

Zie [High Performance Compute VM sizes](../articles/virtual-machines/windows/sizes-hpc.md)(Engelstalig) voor meer informatie.

## <a name="h-series-high-memory"></a>Hoog geheugen van de H-serie

| Size | Quote|
|---|---|
| Standard_H8m|1|
|Standard_H16m|2|

Zie [High Performance Compute VM sizes](../articles/virtual-machines/windows/sizes-hpc.md)(Engelstalig) voor meer informatie.


## <a name="hcs-series"></a>HCS-serie

| Size | Quote|
|---|---|
|Standard_HC44rs|1|

Zie [High Performance Compute VM sizes](../articles/virtual-machines/windows/sizes-hpc.md)(Engelstalig) voor meer informatie.

## <a name="hbs-series"></a>HBS-serie

| Size | Quote|
|---|---|
|Standard_HB60rs|1|

Zie [High Performance Compute VM sizes](../articles/virtual-machines/windows/sizes-hpc.md)(Engelstalig) voor meer informatie.

## <a name="hbs-series"></a>HBS-serie

| Size | Quote|
|---|---|
|Standard_HB60rs|1|

Zie [High Performance Compute VM sizes](../articles/virtual-machines/windows/sizes-hpc.md)(Engelstalig) voor meer informatie.

## <a name="h-series-low-latency-series"></a>Lage latentie reeks van de H-serie

| Size | Quote|
|---|---|
|Standard_H16r|1|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-storage.md)die zijn geoptimaliseerd voor opslag voor meer informatie.

## <a name="h-series-high-memory-low-latency-series"></a>Lage latentie in hoge geheugen-reeks van H-serie

| Size | Quote|
|---|---|
|Standard_H16mr|1|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-storage.md)die zijn geoptimaliseerd voor opslag voor meer informatie.

## <a name="h-series"></a>H-serie

| Size | Quote|
|---|---|
|Standard_H8|1|
|Standard_H16|2|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-storage.md)die zijn geoptimaliseerd voor opslag voor meer informatie.

## <a name="lsv2-series-series"></a>LSv2-serie

| Size | Quote|
|---|---|
|Standard_L8s_v2|1|
|Standard_L16s_v2|2|
|Standard_L32s_v2|4|
|Standard_L48s_v2|6|
|Standard_L64s_v2|8|
|Standard_L80s_v2|10|
|Standard_L96s_v2|12|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows//sizes-storage.md#lsv2-series)die zijn geoptimaliseerd voor opslag voor meer informatie.

## <a name="m-series"></a>M-serie

| Size | Quote|
|---|---|
| Standard_M64s|1|
|Standard_M128s|2|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="m-series-fractional"></a>Breuken uit de M-serie

| Size | Quote|
|---|---|
| Standard_M16s|1|
|Standard_M32s|2|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="m-series-fractional-high-memory"></a>Fractioneel hoog geheugen uit de M-serie

| Size | Quote|
|---|---|
|Standard_M8ms|1|
|Standard_M8-2ms|1|
|Standard_M8-4ms|1|
|Standard_M16ms|2|
|Standard_M16-4ms|2|
|Standard_M16-8ms|2|
|Standard_M32ms|4|
|Standard_M32-8ms|4|
|Standard_M32-16ms|4|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="m-series-fractional-large"></a>M-serie, Fractioneel groot

| Size | Quote|
|---|---|
|Standard_M32ls|1|
|Standard_M64ls|2|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="m-series-high-memory"></a>Hoge geheugen uit de M-serie

| Size | Quote|
|---|---|
|Standard_M64ms|1|
|Standard_M64-16ms|1|
|Standard_M64-32ms|1|
|Standard_M128ms|2|
|Standard_M128-32ms|2|
|Standard_M128-64ms|2|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="ms-series-fractional-tiny"></a>MS-serie, Fractional, Tiny

| Size | Quote|
|---|---|
|Standard_M32ls|1|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="msv2-series-high-memory-series"></a>MSv2 Series hoge geheugen-serie

| Size | Quote|
|---|---|
|Standard_M208ms_v2|1|
|Standard_M416ms_v2|2|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="msv2-series"></a>MSv2-serie

| Size | Quote|
|---|---|
|Standard_M208s_v2|1|
|Standard_M416s_v2|2|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-memory.md#m-series)die zijn geoptimaliseerd voor geheugen voor meer informatie.

## <a name="nc-series"></a>NC-serie

| Size | Quote|
|---|---|
|Standard_NC6|1|
|Standard_NC12|2|
|Standard_NC24|4|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows/sizes-gpu.md)die zijn geoptimaliseerd voor GPU voor meer informatie.

## <a name="ncv2-series"></a>NCv2-serie

| Size | Quote|
|---|---|
| Standard_NC6s_v2|1|
|Standard_NC12s_v2|2|
|Standard_NC24s_v2|4|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series)die zijn geoptimaliseerd voor GPU voor meer informatie.

## <a name="ncv3-series"></a>NCv3-serie

| Size | Quote|
|---|---|
| Standard_NC6s_v3|1|
|Standard_NC12s_v3|2|
|Standard_NC24s_v3|4|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows//sizes-gpu.md#ncv3-series)die zijn geoptimaliseerd voor GPU voor meer informatie.

## <a name="nd-series"></a>ND-serie

| Size | Quote|
|---|---|
| Standard_ND6s|1|
|Standard_ND12s|2|
|Standard_ND24s|4|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows//sizes-gpu.md#nd-series)die zijn geoptimaliseerd voor GPU voor meer informatie.

## <a name="nv-series"></a>NV-serie

| Size | Quote|
|---|---|
| Standard_NV6|1|
|Standard_NV12|2|
|Standard_NV24|4|

## <a name="nvsv3-series"></a>NVSv3-serie

| Size | Quote|
|---|---|
|Standard_NV12s_v3|1|
|Standard_NV24s_v3|2|
|Standard_NV48s_v3|4|

## <a name="nds-series-low-latency-series"></a>Lage latentie-serie van NDS-reeks

| Size | Quote|
|---|---|
|Standard_ND24rs|1|

## <a name="ncsv3-series-low-latency-series"></a>NCSv3-serie met lage latentie-serie

| Size | Quote|
|---|---|
|Standard_NC24rs_v3|1|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows//sizes-gpu.md#ncv3-series)die zijn geoptimaliseerd voor GPU voor meer informatie.

## <a name="ncsv2-series-low-latency-series"></a>NCSv2-serie met lage latentie-serie

| Size | Quote|
|---|---|
|Standard_NC24rs_v2|1|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series)die zijn geoptimaliseerd voor GPU voor meer informatie.

## <a name="nc-series-low-latency-series"></a>NC-serie, lage latentie-reeks

| Size | Quote|
|---|---|
|Standard_NC24r|1|

Zie grootten van [virtuele machines](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series)die zijn geoptimaliseerd voor GPU voor meer informatie.





