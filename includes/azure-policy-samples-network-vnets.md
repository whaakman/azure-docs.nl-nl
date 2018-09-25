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
ms.openlocfilehash: df9c291136fc6a48effb08cd59eeb66486eb641c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003669"
---
### <a name="virtual-networks"></a>Virtuele netwerken

|  |  |
|---------|---------|
| [Toegestane Application Gateway-SKU’s](../articles/governance/policy/samples/allowed-app-gate-sku.md) | Zorgt ervoor dat toepassingsgateways een goedgekeurde SKU gebruiken. U geeft een matrix van goedgekeurde SKU’s op. |
| [Geen netwerkpeering naar ER-netwerk](../articles/governance/policy/samples/no-peering-er-net.md) | Voorkomt de koppeling van een netwerkpeering aan een netwerk in een opgegeven brongroep. Gebruiken om de verbinding met centraal beheerde netwerkinfrastructuur te voorkomen. U geeft de naam op van de brongroep die niet gekoppeld mag worden. |
| [Geen door de gebruiker gedefinieerde routetabellen](../articles/governance/policy/samples/no-user-def-route-table.md)  |Voorkomt dat virtuele netwerken worden geïmplementeerd met een door de gebruiker gedefinieerde routetabel. |
| [NSG X on every subnet](../articles/governance/policy/samples/nsg-on-subnet.md) (NSG X in elk subnet) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elk virtueel subnet. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [Use approved subnet for VM network interfaces](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) (Goedgekeurd subnet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd subnet gebruiken. U geeft de id op van het goedgekeurde subnet. |
| [Use approved vNet for VM network interfaces](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) (Goedgekeurd vNet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd virtueel netwerk gebruiken. U geeft de id op van het goedgekeurde virtuele netwerk. |