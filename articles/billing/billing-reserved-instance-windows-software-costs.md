---
title: Reserveringen softwarekosten voor Azure | Microsoft Docs
description: Meer informatie over welke meter software zijn niet opgenomen in de kosten voor Azure gereserveerde VM-instantie.
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
ms.date: 01/30/2019
ms.author: banders
ms.openlocfilehash: ee7c471cdd76829abc03fa4578d09b8a7303cb38
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57891430"
---
# <a name="software-costs-not-included-with-azure-reserved-vm-instances"></a>Kosten van de software niet zijn opgenomen in Azure Reserved VM Instances

Als u geen een Azure Hybrid Use Benefit op uw gereserveerde VM-instanties, zijn er kosten berekend voor de software-meters die worden vermeld in de volgende secties.

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Windows software meter is niet opgenomen in de reserveringskosten voor

| Meter-id | MeterName in gebruiksbestand | Gebruikt door virtuele machine |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Reservering-Windows Server-Burst (1 Kerngeheugen) | B-serie |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Reservering-Windows Server-Burst (2 Kerngeheugens) | B-serie |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Reservering-Windows Server-Burst (4 Kerngeheugens) | B-serie |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Reservering-Windows Server-Burst (8 Kerngeheugens) | B-serie |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Reservering-Windows-Server (1 Kerngeheugen) | Overal behalve B-serie |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Reservation-Windows Svr (2 Core) | Overal behalve B-serie |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Reservering-Windows-Server (4 Kerngeheugens) | Overal behalve B-serie |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Reservering-Windows-Server (6 Kerngeheugens) | Overal behalve B-serie |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Reservering-Windows-Server (8 Kerngeheugens) | Overal behalve B-serie |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Reservering-Windows-Server (12 Kerngeheugens) | Overal behalve B-serie |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Reservering-Windows-Server (16 Kerngeheugens) | Overal behalve B-serie |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Reservation-Windows Svr (20 Core) | Overal behalve B-serie |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Reservering-Windows-Server (24 Kerngeheugens) | Overal behalve B-serie |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Reservering-Windows-Server (32 Kerngeheugens) | Overal behalve B-serie |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Reservering-Windows-Server (40 Kerngeheugens) | Overal behalve B-serie |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Reservering-Windows-Server (64 Kerngeheugens) | Overal behalve B-serie |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Reservering-Windows-Server (72 Kerngeheugens) | Overal behalve B-serie |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Reservering-Windows-Server (128 Kerngeheugens) | Overal behalve B-serie |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Reservation-Windows Svr (256 Core) | Overal behalve B-serie |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Reservering-Windows-Server (96 Kerngeheugens) | Overal behalve B-serie |

## <a name="cloud-services-software-meters-not-included-in-reservation-cost"></a>Cloud services-meters van software niet opgenomen in de reserveringskosten voor

| Meter-id | MeterName in gebruiksbestand |
| ------- | ------------------------|
|ac9d47ff-ff68-4afc-a145-0c321cf8d0d5|Cloud Services 1 vCPU License|
|e0434559-19ee-4132-9c46-05ad4044f3f7|Cloud Services 2 vCPU License|
|6ecc834e-39b3-48b3-8d10-cc5626bacb66|Cloud Services 4 vCPU License|
|13103090-ca72-4825-ab12-7f16c4931d95|Cloud Services 8 vCPU License|
|ecd2bb6e-45a5-49aa-a58b-3947ba21c364|Cloud Services 16 vCPU License|
|de2c7f1d-06dc-4b16-bc8b-c2ec5f4c8aee|Cloud Services 20 vCPU License|
|ca1af837-4b35-47f5-8d14-b1988149c4ca|Cloud Services 32 vCPU License|
|dc72ee45-2ab7-4698-b435-e2cf10d1f9f6|Cloud Services 64 vCPU License|
|7a803026-244c-4659-834c-11e6b2d6b76f|Cloud Services 80 vCPU License|

## <a name="rates-for-azure-meters"></a>Tarieven voor Azure-meters

U kunt de kosten van elk van deze meter ophalen via Azure RateCard API. Zie voor meer informatie over het ophalen van de tarieven voor een azure-meterkenmerken [prijs en de metagegevens ophalen voor resources die worden gebruikt in een Azure-abonnement](https://msdn.microsoft.com/library/azure/mt219004).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).
