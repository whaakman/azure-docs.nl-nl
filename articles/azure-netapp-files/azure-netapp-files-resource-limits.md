---
title: Resourcelimieten voor Azure NetApp Files | Microsoft Docs
description: Beperkingen voor Azure NetApp Files bronnen, met inbegrip van limieten voor NetApp accounts, pools capaciteit, volumes, momentopnamen en het gedelegeerde subnet worden beschreven.
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
ms.topic: concepts
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: 196d85917e0a9900e141d58bff171beeb8540409
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430010"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Resourcelimieten voor Azure NetApp Files

Inzicht krijgen in resourcelimieten voor Azure NetApp-bestanden, kunt u de volumes beheren.

- Elk Azure-abonnement kan maximaal 10 NetApp-accounts hebben.
- Elk account NetApp kan een maximum van 25 capaciteit groepen hebben.
- Elke capaciteitspool kan deel uitmaken van slechts één NetApp-account.  
- De minimumgrootte van één capaciteitspool is 4 TiB, de maximumgrootte is 500 TiB. 
- Elke capaciteit van toepassingen kan maximaal 500 volumes hebben.
- De minimumgrootte van één volume is 100 GiB, de maximumgrootte is 92 TiB.
- Elk volume mag maximaal 255 momentopnamen hebben.
- Elke Azure-netwerk (Vnet) kan slechts één subnet gedelegeerd naar Azure NetApp bestanden hebben.

**Volgende stappen**

[Inzicht in de hiërarchie van de opslag van Azure NetApp bestanden](azure-netapp-files-understand-storage-hierarchy.md)
