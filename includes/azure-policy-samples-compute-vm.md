---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: b6587a3928c2ddf0d00c90e07643525314690e42
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318209"
---
### <a name="virtual-machines"></a>Virtuele machines

|  |  |
|---------|---------|
| [Aangepaste VM-installatiekopie van een resourcegroep toestaan](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Zorgt ervoor dat aangepaste installatiekopieën alleen afkomstig kunnen zijn uit een goedgekeurde resourcegroep. U geeft de naam op van de goedgekeurde resourcegroep. |
| [Toegestane SKU's voor opslagaccounts en virtuele machines](../articles/governance/policy/samples/allowed-skus-storage.md) | Zorgt ervoor dat opslagaccounts en virtuele machines gebruikmaken van goedgekeurde SKU's. Gebruik ingebouwde beleidsregels om verzekerd te zijn van goedgekeurde SKU's. U geeft een reeks goedgekeurde SKU's voor virtuele machines op en een reeks goedgekeurde SKU's voor opslagaccounts. |
| [Goedgekeurde VM-installatiekopieën](../articles/governance/policy/samples/allowed-custom-images.md) | Zorgt ervoor dat alleen goedgekeurde aangepaste installatiekopieën kunnen worden geïmplementeerd in uw omgeving. U geeft een matrix van goedgekeurde installatiekopie-id’s op. |
| [Controleert of uitbreiding bestaat](../articles/governance/policy/samples/audit-extension-not-exist.md) | Controleert of er niet een uitbreiding met een virtuele machine wordt geïmplementeerd. U geeft de uitgever van de uitbreiding op en het type om te controleren of deze zijn geïmplementeerd. |
| [Niet-toegestane VM-extensies](../articles/governance/policy/samples/not-allowed-vm-extension.md) | Verbiedt het gebruik van de opgegeven extensies. U geeft een matrix op met de verboden extensietypen. |