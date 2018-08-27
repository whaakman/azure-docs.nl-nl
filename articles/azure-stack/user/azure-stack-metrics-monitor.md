---
title: Bewakingsgegevens van Azure Stack gebruiken | Microsoft Docs
description: Meer informatie over opties voor het controleren van de gegevens uit Azure Stack gebruiken.
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
ms.date: 08/28/2018
ms.author: mabrigg
ms.openlocfilehash: 949dee06089ed381010a03ac2fae73aaf4a00c54
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889635"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Het gebruik van bewakingsgegevens van Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt gegevens op één plek met de Azure Monitor-pijplijn, net zoals Azure Monitor in de globale Azure. Maar niet alle van de gegevens gevonden in de globale Azure is beschikbaar in Azure Stack. In dit artikel vindt u een overzicht van de verschillende manieren om dat u bewakingsgegevens van de service programmatisch kan opnemen.
 
## <a name="options-for-data-consumption"></a>Opties voor het gegevensverbruik

| Gegevenstype | Categorie | Ondersteunde services | Methoden voor toegang |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Metrische gegevens van Azure Monitor-platform-niveau | Metrieken | [Ondersteunde metrische gegevens met Azure Monitor in Azure Stack](azure-stack-metrics-supported.md) | REST API |
| COMPUTE Gast OS metrische gegevens (bijvoorbeeld aantal prestaties) | Metrieken | Windows en Linux-Machines | - Of blob Storage-tabel:<br>Windows- of Linux Azure Diagnostics <br>Event Hub:<br>Windows Azure Diagnostics |
| Metrische opslaggegevens | Metrieken | Azure Storage | Storage-tabel:<br>Storage Analytics |
| Activiteitenlogboeken | Gebeurtenissen | Alle Azure-Services | REST-API:<br>Azure Monitor Event-API |
| COMPUTE Gast OS-Logboeken (bijvoorbeeld, IIS, ETW, syslogs) | Gebeurtenissen | Windows en Linux-Machines | - Of blob Storage-tabel:<br>Windows- of Linux Azure Diagnostics <br>Event Hub:<br>Windows Azure Diagnostics |
| Opslaglogboeken | Gebeurtenissen | Azure Storage | Storage-tabel:<br>Storage Analytics<br>`Vita: how about hybrid OMS/AppInsights, shall we mention?` |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure bewaken in Azure Stack](azure-stack-metrics-azure-data.md).