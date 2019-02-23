---
title: Serviceniveaus voor Azure NetApp Files | Microsoft Docs
description: Beschrijving van doorvoerprestaties voor het niveau van de service van Azure NetApp bestanden.
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
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: c2086eb0c5529d8e570a545e35fc716f70c7541f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56668341"
---
# <a name="service-levels-for-azure-netapp-files"></a>Serviceniveau's voor Azure NetApp Files
Azure NetApp-bestanden ondersteunt twee serviceniveaus: Premium en Standard. 

## <a name="Premium"></a>Premium-opslag

De *Premium* storage biedt maximaal 64 MiB/s per TiB aan doorvoer. Prestaties van de netwerkdoorvoer is geïndexeerd op het volume-quotum. Een volume van de Premium-opslag met 2 TiB ingerichte quotum (ongeacht de werkelijke verbruik) heeft bijvoorbeeld een doorvoer van 128 MiB/s.

## <a name="Standard"></a>Standard-opslag

De *Standard* storage biedt maximaal 16 MiB/s per TiB aan doorvoer. Prestaties van de netwerkdoorvoer is geïndexeerd op het volume-quotum. Een volume van de Standard-opslag met 2 TiB ingerichte quotum (ongeacht de werkelijke verbruik) heeft bijvoorbeeld een doorvoer van 32 MiB/s.

## <a name="next-steps"></a>Volgende stappen

- Zie de [pagina met prijzen van Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/) voor de prijs van verschillende serviceniveaus
- [Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)
