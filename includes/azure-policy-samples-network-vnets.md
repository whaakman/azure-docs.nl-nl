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
ms.openlocfilehash: b7bbf1fb8ffc1f57a53682d3baf288077b144572
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664481"
---
### <a name="virtual-networks"></a>Virtuele netwerken

|  |  |
|---------|---------|
| [Toegestane Application Gateway-SKU’s](../articles/azure-policy/scripts/allowed-app-gate-sku.md) | Zorgt ervoor dat toepassingsgateways een goedgekeurde SKU gebruiken. U geeft een matrix van goedgekeurde SKU’s op. |
| [Geen netwerkpeering naar ER-netwerk](../articles/azure-policy/scripts/no-peering-er-net.md) | Voorkomt de koppeling van een netwerkpeering aan een netwerk in een opgegeven brongroep. Gebruiken om de verbinding met centraal beheerde netwerkinfrastructuur te voorkomen. U geeft de naam op van de brongroep die niet gekoppeld mag worden. |
| [Geen door de gebruiker gedefinieerde routetabellen](../articles/azure-policy/scripts/no-user-def-route-table.md)  |Voorkomt dat virtuele netwerken worden geïmplementeerd met een door de gebruiker gedefinieerde routetabel. |
| [NSG X on every subnet](../articles/azure-policy/scripts/nsg-on-subnet.md) (NSG X in elk subnet) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elk virtueel subnet. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [Use approved subnet for VM network interfaces](../articles/azure-policy/scripts/use-approved-subnet-vm-nics.md) (Goedgekeurd subnet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd subnet gebruiken. U geeft de id op van het goedgekeurde subnet. |
| [Use approved vNet for VM network interfaces](../articles/azure-policy/scripts/use-approved-vnet-vm-nics.md) (Goedgekeurd vNet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd virtueel netwerk gebruiken. U geeft de id op van het goedgekeurde virtuele netwerk. |