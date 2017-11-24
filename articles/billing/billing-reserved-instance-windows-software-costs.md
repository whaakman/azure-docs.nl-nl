---
title: Azure reserveren VM-exemplaren Windows softwarekosten | Microsoft Docs
description: Meer informatie over welke meters worden gebruikt voor Windows-software voor de virtuele machine van Windows die in aanmerking komen voor gereserveerde exemplaar.
services: billing
documentationcenter: 
author: manish-shukla01
manager: manshuk
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: b985e6e9575ffeedcac5bcb3f94a43d23fdbb85e
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="windows-software-costs-not-included-with-reserved-instances"></a>Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren

Als u geen Azure hybride gebruik voordeel op de gereserveerde virtuele machine voor exemplaar, klikt u in rekening worden gebracht voor de Windows-software meters die worden vermeld in de volgende tabel:

| meterId | MeterName in bestand in gebruik | Gebruikt door VM |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Reservering Windows Svr Burst (1 Kerngeheugen) | Reeks B |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Reservering Windows Svr Burst (2 Kerngeheugens) | Reeks B |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Reservering Windows Svr Burst (4 Kerngeheugens) | Reeks B |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Reservering Windows Svr Burst (8 Kerngeheugens) | Reeks B |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Reservering Windows Svr (1 Kerngeheugen) | Alles behalve B-reeks |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Reservering Windows Svr (2 Kerngeheugens) | Alles behalve B-reeks |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Reservering Windows Svr (4 Kerngeheugens) | Alles behalve B-reeks |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Reservering Windows Svr (6 Kerngeheugens) | Alles behalve B-reeks |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Reservering Windows Svr (8 Kerngeheugens) | Alles behalve B-reeks |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Reservering Windows Svr (12 Kerngeheugens) | Alles behalve B-reeks |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Reservering Windows Svr (16 Kerngeheugens) | Alles behalve B-reeks |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Reservering Windows Svr (20 Kerngeheugens) | Alles behalve B-reeks |
| 77eb6dd0-88f5-4a16-AB39-05d1742efb25 | Reservering Windows Svr (24 Kerngeheugens) | Alles behalve B-reeks |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Reservering Windows Svr (32 Kerngeheugens) | Alles behalve B-reeks |
| f1214b5c-cc16-445F-be6c-a3bb75f8395a | Reservering Windows Svr (40 Kerngeheugens) | Alles behalve B-reeks |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Reservering Windows Svr (64 kernen) | Alles behalve B-reeks |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Reservering Windows Svr (72 Kerngeheugens) | Alles behalve B-reeks |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Reservering Windows Svr (128 Kerngeheugens) | Alles behalve B-reeks |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Reservering Windows Svr (256 Kerngeheugens) | Alles behalve B-reeks |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Reservering Windows Svr (96 Kerngeheugens) | Alles behalve B-reeks |

U kunt de kosten van elk van deze meter ophalen via Azure RateCard API. Zie voor meer informatie over het verkrijgen van de tarieven voor een azure meter [prijs en de metagegevens voor resources die worden gebruikt in een Azure-abonnement ophalen](https://msdn.microsoft.com/library/azure/mt219004).