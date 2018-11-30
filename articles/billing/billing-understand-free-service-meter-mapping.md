---
title: Service kan de toewijzing van de Meter voor gratis Azure-account | Microsoft Docs
description: Krijg inzicht in de toewijzing van de meter voor services die zijn opgenomen met gratis account-service.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: cwatson
ms.openlocfilehash: 89abf64e6b47700bb16aaf6d4d4c0efbbbd9f520
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581576"
---
# <a name="understand-free-service-to-meter-mapping"></a>Inzicht in de gratis service toewijzing van de meter

Elke Azure-service verzendt gebruik ten opzichte van de meter, waarbij de Azure-facturering systeem gebruik wordt gemaakt in rekening te brengen van gebruikers voor services. Voor meer informatie over het gebruik van gratis services, kunt u de toewijzing van de meter voor deze services-service laten we kijken. Zie voor meer informatie over het maken van gratis services, [gratis services maken met gratis Azure-account](billing-create-free-services-included-free-account.md).

## <a name="service-to-meter-mapping-for-free-account-eligible-services"></a>De service wilt meten toewijzing gratis account in aanmerking komende services 

|    Service   | Meternaam in Azure portal | De Meternaam van de in bestands-gebruik/API | Id van de meter |
| ------------ | -------------------------- | -------------------------| -------- |
| B1S virtuele Linux-machine | Rekenuren - standaard_b1 VM | Rekenuren - gratis | 8260cba2-4437-47d1-a31e-2561cd370f50
| B1S Windows VM | Rekenuren - standaard_b1 VM (Windows) | Rekenuren - gratis | ff3e6fa5-ee46-478e-8d0e-b629f4f8a8ac
| B1S VM - openbare IP-adressen  | Uren IP-adres - openbare IP-adressen | Uren IP-adres - gratis | ae56b367-2708-4454-a3d9-2be7b2364ea1
| CosmosDB | Opslag (GB) - Cosmos DB | Opslag (GB) - gratis | 59c78b09-08e2-466a-9f3b-57a94c9e2f31
| CosmosDB | 100 aanvraageenheden (uren) - Cosmos DB | 100 aanvraageenheden (uren) - gratis | 5d638a6f-e221-41cf-ae3f-0f81d368cef6 
| File Storage | Standaard-IO - bestanden (GB) - lokaal Redundant | Standaard-IO - bestanden (GB) - gratis | a7f2aa67-b9a2-4593-a413-6ec86d6c8e5b
| File Storage | Standaard-IO - eenheden voor bestanden lezen (per 10.000) | Standaard-IO - eenheden voor bestanden lezen (per 10.000) - gratis | 6207404d-3389-4d20-9087-cc078ddc3fd9
| File Storage | Standaard-IO - eenheden voor bestanden wegschrijven (per 10.000) | Standaard-IO - eenheden voor bestanden wegschrijven (per 10.000) - gratis | 223d8004-d29a-46cf-b4f4-d2d34b12548b
| File Storage | Standaard-IO - eenheden voor bestandsprotocolbewerkingen (per 10.000) | Standaard-IO - eenheden voor Bestandsprotocolbewerkingen (per 10.000) - gratis | a347d8cc-51d1-4a0e-b9eb-76f67566c3f5
| File Storage | Standaard-IO - eenheden voor opvragen bestanden (per 10.000) | Standaard-IO - eenheden voor opvragen bestanden (per 10.000) - gratis | e8ae79ad-c2ab-4D82-b226-dd3c33dfd40c
| ' Hot ' blok-Blob-opslag | Standaard-IO - leesbewerkingen ' hot ' blok-Blob (per 10.000) | Standaard-IO - ' hot ' blok-Blob leesbewerkingen (per 10.000) - gratis |fd7cfa1e-026e-4be1-871b-1c2386e8902e
| ' Hot ' blok-Blob-opslag | Standaard-IO - ' hot ' blok-Blob (GB) - lokaal Redundant | Standaard-IO - ' hot ' blok-Blob (GB) - gratis | 67a3a3fd-826f-42c1-8843-bffa14f0da13
| ' Hot ' blok-Blob-opslag | Standaard-IO - schrijfbewerkingen van ' hot ' blok-Blob (per 10.000) | Standaard-IO - ' hot ' blok-Blob schrijfbewerkingen (per 10.000) - gratis | b34bbb76-edce-4c2d-a288-81a2db1fea53
| ' Hot ' blok-Blob-opslag  | Standaard-IO - ' hot ' blok-Blob schrijfbewerkingen/bewerkingen na opvragen (per 10.000) | Standaard-IO - ' hot ' blok-Blob Schrijf/Opvraagbewerkingen (per 10.000) - gratis | 7e68cf36-1198-4d3b-baa7-86a74c5b3079
| Beheerde schijf *  | Standaard beheerde schijf/momentopnamen (GB) - lokaal Redundant | Standaard beheerde schijf/momentopnamen (GB) - gratis | ad94c237-52a5-4804-ae65-38c5bf85ef42
| Beheerde schijf *  | Standard-beheerde schijfbewerkingen (per 10.000) | Standard-beheerde schijfbewerkingen (per 10.000) - gratis | 82cc6ea4-0abd-43ac-acc0-ec34edf0f14c
| Beheerde schijf *  | Premium Storage pagina-Blob/P6 (eenheden) - lokaal Redundant | Premium Storage pagina-Blob/P6 (eenheden) - gratis | 2b98c168-27ca-4cc1-b509-e887dec87657
| SQL Database | Standard S0-Databasedagen - SQL-Database | Standard S0-Databasedagen - gratis | dd6b69d3-9be0-4a91-abff-2c58bbcafd1d
| Gedeeld - bandbreedte ** | Uitgaande gegevensoverdracht (GB) | Gegevensoverdracht uit (GB) - gratis | 0fc067a1-65d2-46da-b24b-7a9cbe2c69bd

\* Als u een Windows-machine maken en kies de beheerde schijf, kunt u beheerde schijf meter als onderdeel van de virtuele machine wordt verbruikt.

\** Gedeelde meters kunnen worden gebruikt door meerdere services. Zowel virtuele machines en opslag introduceren bijvoorbeeld gebruik ten opzichte van de gegevens overdragen Out(GB) meter.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
