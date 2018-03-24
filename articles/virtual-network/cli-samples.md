---
title: Azure CLI-voorbeelden voor het virtuele netwerk | Microsoft Docs
description: Azure CLI-voorbeelden voor het virtuele netwerk.
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
ms.openlocfilehash: 9459bad1060daa0c9f6c34272cecfbc67463be66
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-cli-samples-for-virtual-network"></a>Azure CLI-voorbeelden voor het virtuele netwerk

De volgende tabel bevat koppelingen naar bash-scripts met Azure CLI-opdrachten:

| | |
|----|----|
| [Een virtueel netwerk voor toepassingen met meerdere lagen maken](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Het verkeer naar de front-end-subnet is beperkt tot HTTP- en SSH, terwijl het verkeer naar de back-end-subnet wordt beperkt tot MySQL, poort 3306. |
| [Peer twee virtuele netwerken](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Maakt en verbinding maakt twee virtuele netwerken in dezelfde regio. |
| [-Routeverkeer via een virtueel netwerkapparaat](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Maakt een virtueel netwerk met de front-end en back-end-subnetten en een virtuele machine kan verkeer leiden tussen de twee subnetten. |
| [Filteren van binnenkomende en uitgaande netwerkverkeer voor VM](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Binnenkomend netwerkverkeer naar de front-end-subnet is beperkt tot HTTP, HTTPS en SSH. Uitgaand verkeer naar internet van de back-end-subnet is niet toegestaan. |