---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eedce1ef3a4e7d56cfaf3142a72e370c96f9f575
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664454"
---
### <a name="virtual-machines"></a>Virtuele machines

|  |  |
|---------|---------|
| [Aangepaste VM-installatiekopie van een resourcegroep toestaan](../articles/azure-policy/scripts/allow-custom-vm-image.md) |  Zorgt ervoor dat aangepaste installatiekopieën alleen afkomstig kunnen zijn uit een goedgekeurde resourcegroep. U geeft de naam op van de goedgekeurde resourcegroep. |
| [Toegestane SKU's voor opslagaccounts en virtuele machines](../articles/azure-policy/scripts/allowed-skus-storage.md) | Zorgt ervoor dat opslagaccounts en virtuele machines gebruikmaken van goedgekeurde SKU's. Gebruik ingebouwde beleidsregels om verzekerd te zijn van goedgekeurde SKU's. U geeft een reeks goedgekeurde SKU's voor virtuele machines op en een reeks goedgekeurde SKU's voor opslagaccounts. |
| [Goedgekeurde VM-installatiekopieën](../articles/azure-policy/scripts/allowed-custom-images.md) | Zorgt ervoor dat alleen goedgekeurde aangepaste installatiekopieën kunnen worden geïmplementeerd in uw omgeving. U geeft een matrix van goedgekeurde installatiekopie-id’s op. |
| [Controleert of uitbreiding bestaat](../articles/azure-policy/scripts/audit-ext-not-exist.md) | Controleert of er niet een uitbreiding met een virtuele machine wordt geïmplementeerd. U geeft de uitgever van de uitbreiding op en het type om te controleren of deze zijn geïmplementeerd. |
| [Niet-toegestane VM-extensies](../articles/azure-policy/scripts/not-allowed-vm-ext.md) | Verbiedt het gebruik van de opgegeven extensies. U geeft een matrix op met de verboden extensietypen. |