---
title: Hardware voor StorSimple 10 GbE-netwerkinterfaces | Microsoft Docs
description: Beschrijft de ondersteunde kleine vormfactor pluggable (SFP) infraroodzenders, kabels en switches voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: df8d40c7-f5ad-4f84-93eb-779fbd5f7243
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 8303195f0f3228ee145cbba9e322ea4e5e4c1264
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60302531"
---
# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
In dit artikel bevat informatie over aanvullende hardware die geschikt is voor uw Microsoft Azure StorSimple-apparaat.

## <a name="list-of-devices-tested-by-microsoft"></a>Lijst met apparaten die door Microsoft getest door
Microsoft heeft getest voor de volgende kleine vormfactor pluggable (SFP) infraroodzenders, kabels en switches om ervoor te zorgen dat ze optimaal met apparaten werken. (In de volgende tabellen wordt bijgewerkt als nieuwe hardware is getest.)

### <a name="sfp-transceivers"></a>SFP+ Transceivers
| Maken | Model |
| --- | --- |
| Cisco |SFP-10G-SR |

### <a name="cables"></a>Kabels
| S. Nee. | Maken | Model |
| --- | --- | --- |
| 1. |Cisco |SFP-H10GB-CU1M |
| 2. |Cisco |SFP-H10GB-CU2M |
| 3. |Cisco |SFP-H10GB-CU3M |
| 4. |Tripp-Lite |N820-05M (OM3) |

### <a name="switches"></a>Switches
| S. Nee. | Maken | Model |
| --- | --- | --- |
| 1. |Cisco |N3K-C3172PQ-10GE |
| 2. |Cisco |N3K-C3048-ZM-F |
| 3. |Cisco |N5K-C5596UP-FA |

## <a name="list-of-devices-tested-in-the-field"></a>Lijst met apparaten die zijn getest in het veld
In deze sectie bevat de lijst met apparaten die zijn geïmplementeerd in het veld door StorSimple-klanten. Deze zijn niet getest door Microsoft, maar waarschijnlijk om te werken met uw StorSimple-apparaat.

| Parameter | Value |
| --- | --- |
| Switch maken |Juniper |
| Switch-model |ex4550-32F |
| De versie van de switch-besturingssysteem |JunOS 12.3R9.4 |
| Blade-model |Poorten onboarding (PIC 0) |
| Transceiver maken |Juniper |
| Transceiver model |Onderdeelnummer 740 021308 <br></br> Onderdeelnummer 740 030658 |
| Firmwareversie transceiver |Rev 01 versie 0,0 (vermeld) |
| Model van de kabel |Duplex jumper LC/LC 50/125µ,   OM3, LSZH |
| StorSimple-model |8600 |
| Softwareversie StorSimple |6.3.9600.17491 |

## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Lijst met apparaten die zijn getest door de OEM-provider (Mellanox)
Mellanox is getest door de volgende kleine vormfactor pluggable (SFP) infraroodzenders, kabels en switches om ervoor te zorgen dat ze optimaal met Mellanox-netwerkinterfaces, zoals de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat werken.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kabels en ondersteund door de Mellanox-modules
De volgende tabel bevat de kabels en ondersteund door de Mellanox-modules. Deze zijn niet getest door Microsoft, maar waarschijnlijk om te werken met uw StorSimple-apparaat.

| S. Nee. | Snelheid | Model | Description | Maken |
| --- | --- | --- | --- | --- |
| 1. |10 GbE |CAB-SFP-SFP-1M |passieve koperen kabel SFP + 10 Gb/s-1 min. |Arista |
| 2. |10 GbE |CAB-SFP-SFP-2M |passieve koperen kabel SFP + 10 Gb/s 2 min. |Arista |
| 3. |10 GbE |CAB-SFP-SFP-3M |passieve koperen kabel SFP + 10 Gb/s 3m |Arista |
| 4. |10 GbE |CAB-SFP-SFP-5M |passieve koperen kabel SFP + 10 Gb/s 5 min. |Arista |
| 5. |10 GbE |Cisco SFP-H10GBCU1M |Cisco SFP+ cable |Cisco |
| 6. |10 GbE |Cisco SFP-H10GBCU3M |Cisco SFP+ cable |Cisco |
| 7. |10 GbE |Cisco SFP-H10GBCU5M |Cisco SFP+ cable |Cisco |
| 8. |10 GbE |J9281B HP X242 10 G |SFP + naar SFP + 1m bedrijfsklare koperen kabel |HP |
| 9. |10 GbE |455883-B21 HP BLc |10Gb SR SFP + Opt |HP |
| 10. |10 GbE |455886-B21 HP BLc |10Gb LR SFP + Opt |HP |
| 11. |10 GbE |487649-B21 HP BLc |SFP + 0,5 m 10GbE koperen kabel |HP |
| 12. |10 GbE |487652 B21 HP BLc |SFP+ 1m 10GbE Copper Cable |HP |
| 13. |10 GbE |487655 B21 HP BLc |SFP+ 3m 10GbE Copper Cable |HP |
| 14. |10 GbE |487658 B21 HP BLc |SFP+ 7m 10GbE Copper Cable |HP |
| 15. |10 GbE |537963-B21 HP BLc |SFP+ 5m 10GbE Copper Cable |HP |
| 16. |10 GbE |AP784A HP |3m C-serie passieve Koperen SFP + kabel |HP |
| 17. |10 GbE |AP785A HP |de C-serie passieve Koperen SFP + 5m-kabel |HP |
| 18. |10 GbE |HP AP818A |1m B-serie Active Koperen SFP + kabel |HP |
| 19. |10 GbE |AP819A HP |3m B-serie Active Koperen SFP + kabel |HP |
| 20. |10 GbE |HP J9150A |X132 10G SFP+ LC SR Transceiver |HP |
| 21. |10 GbE |J9151A HP |X132 10G SFP+ LC LR Transceiver |HP |
| 22. |10 GbE |J9283B HP |X242 10 G SFP + SFP + 3 m DAC-kabel |HP |
| 23. |10 GbE |HP J9285B |X242 10 G SFP + SFP + 7 m DAC-kabel |HP |
| 24. |10 GbE |HP JD095B |X240 10 G SFP + SFP + 0.65 m DAC-kabel |HP |
| 25. |10 GbE |JD096B HP |X240 10 G SFP + SFP + 1,2 miljoen DAC-kabel |HP |
| 26. |10 GbE |HP JD097B |X240 10G SFP+ SFP+ 3m DAD Cable |HP |
| 27. |10 GbE |MAM1Q00A-QSA Mellanox |QSFP To SFP+ Adapter |Mellanox-technologieën |
| 28. |10 GbE |MC2309124-006 Mt |Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 24awg 7 min. |Mellanox-technologieën |
| 29. |10 GbE |MC2309124-007 Mt |Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 24awg 7 min. |Mellanox-technologieën |
| 30. |10 GbE |MC2309130-003 Mt |Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 30awg 3 m |Mellanox-technologieën |
| 31. |10 GbE |MC2309130-00A Mt |Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 30awg 0,5 min. |Mellanox-technologieën |
| 32. |10 GbE |MC3309124-005 Mt |Passieve koperen kabel 1 x SFP+ 10 Gb/s 24awg 5 min. |Mellanox-technologieën |
| 33. |10 GbE |MC3309124-007 Mt |Passieve koperen kabel 1 x SFP+ 10 Gb/s 24awg 7 min. |Mellanox-technologieën |
| 34. |10 GbE |MC3309130-003 Mt |Passieve koperen kabel 1 x SFP+ 10 Gb/s 30awg 3 m |Mellanox-technologieën |
| 35. |10 GbE |MC3309130-00A Mt |Passieve koperen kabel 1 x SFP+ 10 Gb/s 30awg 0,5 min. |Mellanox-technologieën |

### <a name="switches-supported-by-mellanox"></a>Switches die worden ondersteund door Mellanox
De volgende tabel bevat de switches die door de Mellanox worden ondersteund. Deze zijn niet getest door Microsoft, maar waarschijnlijk om te werken met uw StorSimple-apparaat.

| S. Nee. | Snelheid | Model | Description | Maken |
| --- | --- | --- | --- | --- |
| 1. |10GbE |516733-B21 |HP ProCurve 6120XG 10GbE Ethernet-Blade Switch |HP |
| 2. |10GbE |538113-B21 |HP 10GbE Pass Through-Module (PTM) |HP |
| 3. |10GbE |EN4093 |IBM PureFlex Fabric EN4093 10 Gigabit schaalbare Switch-Systeemmodule |IBM |
| 4. |1 GbE |3020 |Cisco katalysator 3020 1 GbE-switch-blade |Cisco |
| 5. |1 GbE |3020X |Cisco katalysator 3020 X 1 GbE-switch-blade |Cisco |
| 6. |1 GbE |438030-B21 |1 GbE-switch-module voor HP - GbE2c Layer 2/3 Ethernet-Blade Switch |HP |
| 7. |1 GbE |6120G |HP ProCurve 6120G/XG 1 GbE-switch-blade |HP |

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over StorSimple-hardware-onderdelen en status](storsimple-monitor-hardware-status.md).

