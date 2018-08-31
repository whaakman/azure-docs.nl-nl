---
title: Kosten voor Azure-reserveringen Windows software | Microsoft Docs
description: Meer informatie over welke Windows software meters zijn niet opgenomen in de kosten voor Azure gereserveerde VM-instantie.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: ec4869ed5c4d823a8144ba430ebb29c65c2abc13
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43302063"
---
# <a name="windows-software-costs-not-included-with-azure-reserved-vm-instances"></a>Kosten van de Windows-software is niet opgenomen met Azure Reserved VM Instances

Als u geen een Azure Hybrid Use Benefit op uw gereserveerde VM-instanties, klikt u vervolgens in rekening gebracht voor de Windows-software-meters die worden vermeld in de volgende sectie.

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Windows software meter is niet opgenomen in de reserveringskosten voor

| Meter-id | MeterName in gebruiksbestand | Gebruikt door virtuele machine |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Reservering-Windows Server-Burst (1 Kerngeheugen) | B-serie |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Reservering-Windows Server-Burst (2 Kerngeheugens) | B-serie |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Reservering-Windows Server-Burst (4 Kerngeheugens) | B-serie |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Reservering-Windows Server-Burst (8 Kerngeheugens) | B-serie |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Reservering-Windows-Server (1 Kerngeheugen) | Overal behalve B-serie |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Reservering-Windows-Server (2 Kerngeheugens) | Overal behalve B-serie |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Reservering-Windows-Server (4 Kerngeheugens) | Overal behalve B-serie |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Reservering-Windows-Server (6 Kerngeheugens) | Overal behalve B-serie |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Reservering-Windows-Server (8 Kerngeheugens) | Overal behalve B-serie |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Reservering-Windows-Server (12 Kerngeheugens) | Overal behalve B-serie |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Reservering-Windows-Server (16 Kerngeheugens) | Overal behalve B-serie |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Reservering-Windows-Server (20 Kerngeheugens) | Overal behalve B-serie |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Reservering-Windows-Server (24 Kerngeheugens) | Overal behalve B-serie |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Reservering-Windows-Server (32 Kerngeheugens) | Overal behalve B-serie |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Reservering-Windows-Server (40 Kerngeheugens) | Overal behalve B-serie |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Reservering-Windows-Server (64 Kerngeheugens) | Overal behalve B-serie |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Reservering-Windows-Server (72 Kerngeheugens) | Overal behalve B-serie |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Reservering-Windows-Server (128 Kerngeheugens) | Overal behalve B-serie |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Reservering-Windows-Server (256-Core) | Overal behalve B-serie |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Reservering-Windows-Server (96 Kerngeheugens) | Overal behalve B-serie |

U kunt de kosten van elk van deze meter ophalen via Azure RateCard API. Zie voor meer informatie over het ophalen van de tarieven voor een azure-meterkenmerken [prijs en de metagegevens ophalen voor resources die worden gebruikt in een Azure-abonnement](https://msdn.microsoft.com/library/azure/mt219004).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.



