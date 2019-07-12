---
title: Hoe een Azure Databricks pre-Purchase korting wordt toegepast
description: Meer informatie over hoe een Azure Databricks pre-Purchase korting is van toepassing op uw gebruik.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: banders
ms.openlocfilehash: 7c1855b587ab1d603e9c6ac24a67b0f50065361f
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827640"
---
# <a name="how-azure-databricks-pre-purchase-discount-is-applied"></a>Hoe Azure Databricks pre-Purchase korting wordt toegepast

U kunt vooraf aangeschafte Azure Databricks commit-eenheden (DBCU) op elk gewenst moment gedurende de termijn van aankoop. Automatisch trekt een Azure Databricks-gebruik van de vooraf aangeschafte DBCUs.

In tegenstelling tot virtuele machines verlopen niet vooraf aangeschafte eenheden op uurbasis. U kunt deze gebruiken op elk gewenst moment gedurende de termijn van de aankoop. Als u de kortingen pre-Purchase, moet u niet opnieuw implementeren of een vooraf aangeschafte abonnement toewijzen aan uw Azure Databricks-werkruimten voor het gebruik.

De korting pre-Purchase geldt alleen voor Azure Databricks-eenheden (DBU) worden gebruikt. Andere kosten in rekening gebracht, zoals rekentijd, opslag en netwerken worden afzonderlijk in rekening gebracht.

## <a name="pre-purchase-discount-application"></a>Toepassing van korting op vooraf kunt kopen

Databricks pre-Purchase is van toepassing op alle lagen en Databricks-werkbelastingen. U kunt zien van de pre-Purchase als een groep van de vooraf betaalde Databricks commit-eenheden. Gebruik wordt afgetrokken van de groep, ongeacht de werkbelasting of laag. Gebruik, wordt in mindering gebracht in de volgende verhouding:

| **Workload** | **DBU-toepassing-verhouding, Standard-laag** | **DBU-toepassing-verhouding, Premium-laag** |
| --- | --- | --- |
| Gegevensanalyse | 0.4 | 0.55 |
| Data Engineering | 0.15 | 0,30 |
| Data Engineering Light | 0.07 | 0.22 |

Wanneer u een aantal van de Data Analytics-Standard-laag is verbruikt, wordt bijvoorbeeld de Databricks-eenheden voor vooraf aangeschafte doorvoeren 0,4 eenheden afgetrokken. Wanneer de hoeveelheid gegevens Engineering licht – Standard-laag wordt gebruikt, wordt het vooraf aangeschafte eenheid van de Databricks-doorvoer wordt in mindering gebracht door 0,07 eenheden

## <a name="determine-plan-use"></a>Plan bepalen

Om te bepalen van uw plan DBCU gebruikt, gaat u naar de Azure portal > **reserveringen** en klik op de aangeschafte Databricks-plan. Uw gebruik tot heden wordt weergegeven met alle resterende eenheden. Voor meer informatie over het bepalen van uw reservering gebruiken, raadpleegt u de [Zie reservering gebruik](billing-reservation-apis.md#see-reservation-usage) artikel.

## <a name="how-discount-application-shows-in-usage-data"></a>Hoe het effect van toepassing van korting op in de gegevens over gebruik

Wanneer de pre-Purchase-korting van toepassing op uw Databricks-gebruik is, wordt op aanvraag kosten worden als nul in de gebruiksgegevens. Zie voor meer informatie over de kosten voor reservering en het verbruik, [ophalen Enterprise Agreement-reservering kosten en het verbruik](billing-understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van een reservering, [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md).
- Zie voor meer informatie over het vooraf aanschaffen van Azure Databricks om geld te besparen, [kosten van Azure Databricks optimaliseren met een pre-Purchase](billing-prepay-databricks-reserved-capacity.md).
- Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:
  - [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
  - [Reserveringen in Azure beheren](billing-manage-reserved-vm-instance.md)
  - [Informatie over het gebruik van de reservering voor een abonnement met betalen per gebruik-tarieven](billing-understand-reserved-instance-usage.md)
  - [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
