---
title: Resourcelimieten voor Azure NetApp Files | Microsoft Docs
description: Beperkingen voor Azure NetApp Files bronnen, met inbegrip van limieten voor capaciteit van toepassingen, volumes en het gedelegeerde subnet worden beschreven.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 01/03/2019
ms.author: b-juche
ms.openlocfilehash: f34afb1df2ae38353f29a80bfb6798c16856dbeb
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54056932"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Resourcelimieten voor Azure NetApp-bestanden
Inzicht krijgen in resourcelimieten voor Azure NetApp-bestanden, kunt u de volumes beheren.

## <a name="capacity_pools"></a>Capaciteitspools

- De minimumgrootte van één capaciteitspool is 4 TiB, de maximumgrootte is 500 TiB. 
- Elke capaciteitspool kan deel uitmaken van slechts één NetApp-account. Er kunnen echter meerdere capaciteitspools binnen een NetApp-account bestaan.  

## <a name="volumes"></a>Volumes

- De minimumgrootte van één volume is 100 GiB, de maximumgrootte is 92 TiB.
- Het maximaal aantal volumes per Azure-abonnement per regio bedraagt honderd.  

## <a name="delegated_subnet"></a>Gedelegeerde subnet 

In elke Azure virtuele netwerk (Vnet) kan slechts één subnet worden overgedragen naar Azure NetApp bestanden.

## <a name="next-steps"></a>Volgende stappen

[Inzicht in de hiërarchie van de opslag van Azure NetApp bestanden](azure-netapp-files-understand-storage-hierarchy.md)
