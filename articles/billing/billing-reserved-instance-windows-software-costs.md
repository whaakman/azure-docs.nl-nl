---
title: Reserve ringen van software kosten voor Azure | Microsoft Docs
description: Meer informatie over welke software meters niet zijn opgenomen in de kosten voor gereserveerde VM-instanties van Azure.
services: billing
documentationcenter: ''
author: yashar
manager: yashar
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: banders
ms.openlocfilehash: 52e2e2503303c2a7525a3a6c156f648c097b27dd
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68478616"
---
# <a name="software-costs-not-included-with-azure-reserved-vm-instances"></a>Software kosten niet inbegrepen bij Azure Reserved VM Instances

Gereserveerde exemplaren van virtuele machines en korting op gereserveerde capaciteit van SQL zijn alleen van toepassing op de kosten van de infra structuur en niet op de software kosten. Als u Windows VM gebruikt en geen Azure Hybrid Benefit hebt op uw gereserveerde exemplaren van virtuele machines, worden de software meters in de volgende sectie in rekening gebracht. Voor SQL PaaS-implementaties worden de IP-kosten in rekening gebracht met behulp van een afzonderlijke meter als Azure Hybrid Benefit niet is geselecteerd.

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Windows-software meters die niet zijn opgenomen in de reserverings kosten

| Meter-id | Meting van de meter in het gebruiks bestand | Gebruikt door VM |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Reserve ring-Windows Server-burst (1 kern geheugen) | B-serie |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Reserve ring-Windows Server-burst (2 kern geheugens) | B-serie |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Reserve ring-Windows Server-burst (4-core) | B-serie |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Reserve ring-Windows Server-burst (8 kern geheugens) | B-serie |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Reserve ring-Windows Server (1-core) | Alle behalve de B-serie |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Reserve ring-Windows Server (2-Core) | Alle behalve de B-serie |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Reserve ring-Windows Server (4-core) | Alle behalve de B-serie |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Reserve ring-Windows Server (6-core) | Alle behalve de B-serie |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Reserve ring-Windows Server (8-core) | Alle behalve de B-serie |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Reserve ring-Windows Server (12-core) | Alle behalve de B-serie |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Reserve ring-Windows Server (16-core) | Alle behalve de B-serie |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Reserve ring-Windows Server (20 kern geheugens) | Alle behalve de B-serie |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Reserve ring-Windows Server (24-Core) | Alle behalve de B-serie |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Reserve ring-Windows Server (32-core) | Alle behalve de B-serie |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Reserve ring-Windows Server (40-core) | Alle behalve de B-serie |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Reserve ring-Windows Server (64-core) | Alle behalve de B-serie |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Reserve ring-Windows Server (72-core) | Alle behalve de B-serie |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Reserve ring-Windows Server (128-core) | Alle behalve de B-serie |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Reserve ring-Windows Server (256-core) | Alle behalve de B-serie |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Reserve ring-Windows Server (96-core) | Alle behalve de B-serie |

## <a name="cloud-services-software-meters-not-included-in-reservation-cost"></a>Cloud Services-Software meters die niet zijn opgenomen in de reserverings kosten

| Meter-id | Meting van de meter in het gebruiks bestand |
| ------- | ------------------------|
|ac9d47ff-ff68-4afc-a145-0c321cf8d0d5|Cloud Services 1 vCPU-licentie|
|e0434559-19ee-4132-9c46-05ad4044f3f7|Cloud Services 2 vCPU-licentie|
|6ecc834e-39b3-48b3-8d10-cc5626bacb66|Cloud Services 4 vCPU-licentie|
|13103090-ca72-4825-ab12-7f16c4931d95|Cloud Services 8 vCPU-licentie|
|ecd2bb6e-45a5-49aa-a58b-3947ba21c364|Cloud Services 16 vCPU-licentie|
|de2c7f1d-06dc-4b16-bc8b-c2ec5f4c8aee|Cloud Services 20 vCPU-licentie|
|ca1af837-4b35-47f5-8d14-b1988149c4ca|Cloud Services 32 vCPU License|
|dc72ee45-2ab7-4698-b435-e2cf10d1f9f6|Cloud Services 64 vCPU-licentie|
|7a803026-244c-4659-834c-11e6b2d6b76f|Cloud Services 80 vCPU-licentie|

## <a name="rates-for-azure-meters"></a>Tarieven voor Azure-meters

U kunt de kosten van elk van deze meters ophalen via de Azure RateCard-API. Zie voor meer informatie over het ophalen van de tarieven voor een Azure meter de [informatie over prijzen en meta gegevens ophalen voor bronnen die worden gebruikt in een Azure-abonnement](/previous-versions/azure/reference/mt219004(v=azure.100)).

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen voor meer informatie over reserve ringen voor Azure:

- [Wat zijn reserve ringen voor Azure?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Reserve ringen voor Azure beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserverings korting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Het gebruik van de reserve ring begrijpen voor uw abonnement voor betalen naar gebruik](billing-understand-reserved-instance-usage.md)
- [Het gebruik van de reserve ring begrijpen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, kunt u [een ondersteunings aanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).
