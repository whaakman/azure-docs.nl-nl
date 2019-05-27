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
ms.openlocfilehash: f189843e865863d9b4088363e691ebc42ad9f2a9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155509"
---
### <a name="network-interfaces"></a>Netwerkinterfaces

|  |  |
|---------|---------|
| [NSG X on every NIC](../articles/governance/policy/samples/nsg-on-nic.md) (NSG X op elke NIC) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elke interface van een virtueel netwerk. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [Use approved subnet for VM network interfaces](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) (Goedgekeurd subnet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd subnet gebruiken. U geeft de id op van het goedgekeurde subnet. |
| [Use approved vNet for VM network interfaces](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) (Goedgekeurd vNet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd virtueel netwerk gebruiken. U geeft de id op van het goedgekeurde virtuele netwerk. |