---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: 6517aec56839295af30d251f4cb5d3bb9426282c
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57457787"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Virtuele machine grootte flexibiliteit in combinatie met gereserveerde VM-instanties

Met een gereserveerde virtuele machine-instantie die is geoptimaliseerd voor flexibiliteit van de grootte, worden de reservering die u koopt kunt toepassen op de grootten van virtuele machines (VM's) in de groep voor reeks van dezelfde grootte. Bijvoorbeeld, als u een reservering voor een VM-grootte die wordt vermeld in de tabel uit de DSv2-serie, zoals Standard_DS5_v2, koop kunt de reserveringskorting toepassen op de andere vier grootten die worden vermeld in dezelfde tabel:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Maar dat de reserveringskorting niet van toepassing op VM's met grootten die worden vermeld in verschillende tabellen, zoals wat is er in de tabel uit de DSv2-serie hoge geheugen: Standard_DS11_v2, Standard_DS12_v2, enzovoort.

In de FormaatGroep-serie, is het aantal virtuele machines de reserveringskorting is van toepassing op afhankelijk van de VM-grootte u kiest wanneer u een reservering koopt. Ook afhankelijk van de grootte van de virtuele machines die u hebt uitgevoerd. De verhouding kolom die wordt vermeld in de volgende tabel vergelijkt de relatieve grootte van elke VM-grootte in die groep. Gebruik de verhouding te berekenen hoe de reserveringskorting is van toepassing op de virtuele machines u waarde hebt uitgevoerd.

## <a name="examples"></a>Voorbeelden

De volgende voorbeelden gebruiken de grootte en verhoudingen in de tabel uit de DSv2-serie.

 U koopt een gereserveerde VM-instantie met de grootte Standard_DS4_v2 waar de verhouding of relatieve footprint in vergelijking met de andere grootten uit de reeks 8 is.

- Scenario 1: Uitvoeren van acht grootte Standard_DS1_v2 voor de virtuele machines met een ratio van 1. De reserveringskorting is van toepassing op alle 8 van deze virtuele machines.
- Scenario 2: Voer twee grootte Standard_DS2_v2 virtuele machines met een verhouding van 2, elke. Een Standard_DS3_v2 grootte virtuele machine met een ratio van 4 ook uitvoeren. De totale footprint is 2 + 2 + 4 = 8. Dus geldt uw reserveringskorting voor alle drie deze virtuele machines.
- Scenario 3: Voer een Standard_DS5_v2 met een verhouding van 16. De reserveringskorting is van toepassing op de helft aan die VM rekenkosten.

De volgende secties ziet welke grootten in de groep dezelfde grootte uit de serie zijn als u een gereserveerde VM-instantie koopt geoptimaliseerd voor flexibiliteit van de grootte.

## <a name="b-series"></a>B-serie

| Grootte | Verhouding|
|---|---|
| Standard_B1s | 1 |
|Standard_B2s|4|

Zie voor meer informatie, [B-serie ' burstable ' VM-grootten](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="b-series-high-memory"></a>Hoge geheugen uit de B-serie

| Grootte | Verhouding|
|---|---|
| Standard_B1ms |1|
|Standard_B2ms|4|
|Standard_B4ms|8|
|Standard_B8ms|16|

Zie voor meer informatie, [B-serie ' burstable ' VM-grootten](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="d-series"></a>D-serie

| Grootte | Verhouding|
|---|---|
| Standard_D1|1|
|Standard_D2|2|
|Standard_D3|4|
|Standard_D4|8|

Zie voor meer informatie, [vorige generaties groottes van virtuele machines](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="d-series-high-memory"></a>Hoge geheugen uit de D-serie

| Grootte | Verhouding|
|---|---|
| Standard_D11|1|
|Standard_D12|2|
|Standard_D13|4|
|Standard_D14|8|

Zie voor meer informatie, [vorige generaties groottes van virtuele machines](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series"></a>DS-serie

| Grootte | Verhouding|
|---|---|
|Standard_DS1|1|
|Standard_DS2|2|
|Standard_DS3|4|
|Standard_DS4|8|

Zie voor meer informatie, [vorige generaties groottes van virtuele machines](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series-high-memory"></a>Hoge geheugen uit de DS-serie

| Grootte | Verhouding|
|---|---|
|Standard_DS11|1|
|Standard_DS12|2|
|Standard_DS13|4|
|Standard_DS14|8|

Zie voor meer informatie, [vorige generaties groottes van virtuele machines](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series"></a>DSv2-serie

| Grootte | Verhouding|
|---|---|
|Standard_DS1_v2|1|
|Standard_DS2_v2|2|
|Standard_DS3_v2|4|
|Standard_DS4_v2|8|
|Standard_DS5_v2|16|

Zie voor meer informatie, [grootten van virtuele machines voor algemeen gebruik](../articles/virtual-machines/windows/sizes-general.md#dv2-series).

## <a name="dsv2-series-high-memory"></a>Hoge geheugen uit de DSv2-serie

| Grootte | Verhouding|
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

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#dsv2-series-11-15).

## <a name="dsv3-series"></a>DSv3-serie

| Grootte | Verhouding|
|---|---|
|Standard_D2s_v3|1|
|Standard_D4s_v3|2|
|Standard_D8s_v3|4|
|Standard_D16s_v3|8|
|Standard_D32s_v3|16|
|Standard_D64s_v3|32|

Zie voor meer informatie, [grootten van virtuele machines voor algemeen gebruik](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-sup1sup).

## <a name="dv2-series"></a>Dv2-serie

| Grootte | Verhouding|
|---|---|
|Standard_D1_v2|1|
|Standard_D2_v2|2|
|Standard_D3_v2|4|
|Standard_D4_v2|8|
|Standard_D5_v2|16|

Zie voor meer informatie, [grootten van virtuele machines voor algemeen gebruik](../articles/virtual-machines/windows/sizes-general.md#dv2-series).

## <a name="dv2-series-high-memory"></a>Hoge geheugen uit de Dv2-serie

| Grootte | Verhouding|
|---|---|
|Standard_D11_v2|1|
|Standard_D12_v2|2|
|Standard_D13_v2|4|
|Standard_D14_v2|8|
|Standard_D15_v2|10|

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#dv2-series-11-15).

## <a name="dv3-series"></a>Dv3-serie

| Grootte | Verhouding|
|---|---|
| Standard_D2_v3|1|
|Standard_D4_v3|2|
|Standard_D8_v3|4|
|Standard_D16_v3|8|
|Standard_D32_v3|16|
|Standard_D64_v3|32|

Zie voor meer informatie, [grootten van virtuele machines voor algemeen gebruik](../articles/virtual-machines/windows/sizes-general.md#dv3-series-sup1sup).

## <a name="esv3-series"></a>ESv3-serie

| Grootte | Verhouding|
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
|Standard_E64s_v3|28.8|
|Standard_E64-16s_v3|28.8|
|Standard_E64-32s_v3|28.8|

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#esv3-series).

## <a name="ev3-series"></a>Ev3-serie

| Grootte | Verhouding|
|---|---|
| Standard_E2_v3|1|
|Standard_E4_v3|2|
|Standard_E8_v3|4|
|Standard_E16_v3|8|
|Standard_E20_v3|10|
|Standard_E32_v3|16|
|Standard_E64_v3|32|

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#ev3-series).

## <a name="f-series"></a>F-serie

| Grootte | Verhouding|
|---|---|
| Standard_F1|1|
|Standard_F2|2|
|Standard_F4|4|
|Standard_F8|8|
Standard_F16|16|

Zie voor meer informatie, [geoptimaliseerde VM-grootten](../articles/virtual-machines/windows/sizes-compute.md#f-series).

## <a name="fs-series"></a>FS-serie

| Grootte | Verhouding|
|---|---|
| Standard_F1s|1|
|Standard_F2s|2|
|Standard_F4s|4|
|Standard_F8s|8|
|Standard_F16s|16|

Zie voor meer informatie, [geoptimaliseerde VM-grootten](../articles/virtual-machines/windows/sizes-compute.md#fs-series-sup1sup).

## <a name="fsv2-series"></a>Fsv2-serie

| Grootte | Verhouding|
|---|---|
|Standard_F2s_v2|1|
|Standard_F4s_v2|2|
|Standard_F8s_v2|4|
|Standard_F16s_v2|8|
|Standard_F32s_v2|16|
|Standard_F64s_v2|32|
|Standard_F72s_v2|36|

Zie voor meer informatie, [geoptimaliseerde VM-grootten](../articles/virtual-machines/windows/sizes-compute.md#fsv2-series-sup1sup).

## <a name="h-series"></a>H-serie

| Grootte | Verhouding|
|---|---|
| Standard_H8|1|
|Standard_H16|2|

Zie voor meer informatie, [High performance computing-VM-grootten](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="h-series-high-memory"></a>Hoge geheugen uit de H-serie

| Grootte | Verhouding|
|---|---|
| Standard_H8m|1|
|Standard_H16m|2|

Zie voor meer informatie, [High performance computing-VM-grootten](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="ls-series"></a>Ls-serie

| Grootte | Verhouding|
|---|---|
| Standard_L4s|1|
|Standard_L8s|2|
|Standard_L16s|4|
|Standard_L32s|8|

Zie voor meer informatie, [grootten van virtuele machines met geoptimaliseerde opslag](../articles/virtual-machines/windows/sizes-storage.md).

## <a name="m-series"></a>M-serie

| Grootte | Verhouding|
|---|---|
| Standard_M64s|1|
|Standard_M128s|2|

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional"></a>M-serie decimale

| Grootte | Verhouding|
|---|---|
| Standard_M16s|1|
|Standard_M32s|2|

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-high-memory"></a>Decimale veel geheugen van de M-serie

| Grootte | Verhouding|
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

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-large"></a>M-serie decimale grote

| Grootte | Verhouding|
|---|---|
| Standard_M32ls|1|
|Standard_M64ls|2|

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-high-memory"></a>Hoge geheugen uit de M-serie

| Grootte | Verhouding|
|---|---|
| Standard_M64ms|1|
|Standard_M64-16ms|1|
|Standard_M64-32ms|1|
|Standard_M128ms|2|
|Standard_M128-32ms|2|
|Standard_M128-64ms|2|

Zie voor meer informatie, [grootten van virtuele machines geoptimaliseerd voor geheugen](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="nc-series"></a>NC-serie

| Grootte | Verhouding|
|---|---|
| Standard_NC6|1|
|Standard_NC12|2|
|Standard_NC24|4|

Zie voor meer informatie, [GPU VM-grootten geoptimaliseerd](../articles/virtual-machines/windows/sizes-gpu.md).

## <a name="ncv2-series"></a>NCv2-serie

| Grootte | Verhouding|
|---|---|
| Standard_NC6s_v2|1|
|Standard_NC12s_v2|2|
|Standard_NC24s_v2|4|

Zie voor meer informatie, [GPU VM-grootten geoptimaliseerd](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series).

## <a name="ncv3-series"></a>NCv3-serie

| Grootte | Verhouding|
|---|---|
| Standard_NC6s_v3|1|
|Standard_NC12s_v3|2|
|Standard_NC24s_v3|4|

Zie voor meer informatie, [GPU VM-grootten geoptimaliseerd](../articles/virtual-machines/windows//sizes-gpu.md#ncv3-series).

## <a name="nd-series"></a>ND-serie

| Grootte | Verhouding|
|---|---|
| Standard_ND6s|1|
|Standard_ND12s|2|
|Standard_ND24s|4|

Zie voor meer informatie, [GPU VM-grootten geoptimaliseerd](../articles/virtual-machines/windows//sizes-gpu.md#nd-series).

## <a name="nv-series"></a>NV-serie

| Grootte | Verhouding|
|---|---|
| Standard_NV6|1|
|Standard_NV12|2|
|Standard_NV24|4|

Zie voor meer informatie, [GPU VM-grootten geoptimaliseerd](../articles/virtual-machines/windows//sizes-gpu.md#nv-series).


