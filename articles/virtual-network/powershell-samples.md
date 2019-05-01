---
title: Azure PowerShell-voorbeelden voor virtueel netwerk | Microsoft Docs
description: Azure PowerShell-voorbeelden voor virtueel netwerk.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/17/2019
ms.author: kumud
ms.openlocfilehash: e10f7ad4ae8995d9efffec97a37dc64b28fa37b0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64688026"
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Azure PowerShell-voorbeelden voor virtueel netwerk

De volgende tabel bevat links naar Azure PowerShell-scripts:

| | |
|----|----|
| [Een virtueel netwerk maken voor toepassingen met meerdere lagen](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Hiermee wordt een virtueel netwerk met front-end- en back-end-subnetten gemaakt. Verkeer naar het front-end-subnet wordt beperkt tot HTTP, terwijl verkeer naar het back-end-subnet wordt beperkt tot SQL, poort 1433. |
| [Peering van twee virtuele netwerken](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Hiermee worden twee virtuele netwerken in dezelfde regio gemaakt en verbonden. |
| [Verkeer routeren via een virtueel netwerkapparaat](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Hiermee wordt een virtueel netwerk gemaakt met front-end- en back-end-subnetten en een VM die verkeer tussen de twee subnetten kan routeren. |
| [Binnenkomend en uitgaand VM-netwerkverkeer filteren](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Hiermee wordt een virtueel netwerk met front-end- en back-end-subnetten gemaakt. Binnenkomend netwerkverkeer naar het front-end-subnet wordt beperkt tot HTTP en HTTPS. Uitgaand verkeer van het back-end-subnet naar internet is niet toegestaan. |
|[Configureer IPv4 + IPv6 dual stack-virtueel netwerk](./scripts/virtual-network-powershell-sample-ipv6-dual-stack.md)|Virtueel netwerk met twee virtuele machines en een Azure Basic Load Balancer met openbare IP-adressen van IPv4 en IPv6 dual stack (IPv4 + IPv6) implementeert. |
