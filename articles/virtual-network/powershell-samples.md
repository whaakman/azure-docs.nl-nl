---
title: Azure PowerShell-voorbeelden voor het virtuele netwerk | Microsoft Docs
description: Azure PowerShell-voorbeelden voor het virtuele netwerk.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: c9cb8da2a6a7512daa6721af90d5b21c6ba5e8e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Azure PowerShell-voorbeelden voor het virtuele netwerk

De volgende tabel bevat koppelingen naar Azure Powershell-scripts:

| | |
|----|----|
| [Een virtueel netwerk voor toepassingen met meerdere lagen maken](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Het verkeer naar de front-end-subnet is beperkt tot HTTP, terwijl het verkeer naar de back-end-subnet wordt beperkt tot de SQL-poort 1433. |
| [Peer twee virtuele netwerken](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Maakt en verbinding maakt twee virtuele netwerken in dezelfde regio. |
| [-Routeverkeer via een virtueel netwerkapparaat](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Maakt een virtueel netwerk met de front-end en back-end-subnetten en een virtuele machine kan verkeer leiden tussen de twee subnetten. |
| [Filteren van binnenkomende en uitgaande netwerkverkeer voor VM](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Binnenkomend netwerkverkeer naar de front-end-subnet is beperkt tot HTTP en HTTPS. Uitgaand verkeer naar internet van de back-end-subnet is niet toegestaan. |
