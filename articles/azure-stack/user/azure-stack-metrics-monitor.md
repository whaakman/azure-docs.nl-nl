---
title: Bewakingsgegevens van Azure Stack gebruiken | Microsoft Docs
description: Meer informatie over opties voor het gebruik van bewakingsgegevens van Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2018
ms.author: mabrigg
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: 174038c611d9e804817d49478f99c643f5280f7b
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57773043"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Het gebruik van bewakingsgegevens van Azure Stack

*Van toepassing op: Azure Stack-geïntegreerde systemen*

U kunt gegevens op één plek met de Azure Monitor-pijplijn, net zoals Azure Monitor in de globale Azure. Maar niet alle van de gegevens gevonden in de globale Azure is beschikbaar in Azure Stack. In dit artikel vindt u een overzicht van de verschillende manieren om dat u bewakingsgegevens van de service programmatisch kan opnemen.
 
## <a name="options-for-data-consumption"></a>Opties voor het gegevensverbruik

| Gegevenstype | Categorie | Ondersteunde services | Methoden voor toegang |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Metrische gegevens op Azure Monitor-platform-niveau | Metrische gegevens | [Ondersteunde metrische gegevens met Azure Monitor in Azure Stack](azure-stack-metrics-supported.md) | REST-API |
| COMPUTE Gast OS metrische gegevens (bijvoorbeeld aantal prestaties) | Metrische gegevens | Windows en Linux-Machines | - Of blob Storage-tabel:<br>Windows- of Linux Azure Diagnostics <br>Event Hub:<br>Windows Azure Diagnostics |
| Metrische gegevens over opslag | Metrische gegevens | Azure Storage | Storage-tabel:<br>Storage Analytics |
| Activiteitenlogboek | Gebeurtenissen | Alle Azure-services | REST-API:<br>Azure Monitor Event-API |
| COMPUTE Gast OS-Logboeken (bijvoorbeeld, IIS, ETW, syslogs) | Gebeurtenissen | Windows en Linux-Machines | - Of blob Storage-tabel:<br>Windows- of Linux Azure Diagnostics <br>Event Hub:<br>Windows Azure Diagnostics |
| Opslaglogboeken | Gebeurtenissen | Azure Storage | Storage-tabel:<br>Storage Analytics |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure bewaken in Azure Stack](azure-stack-metrics-azure-data.md).
