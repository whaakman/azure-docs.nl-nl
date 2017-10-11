---
title: Azure Batch Analytics | Microsoft Docs
description: Verwijzing voor Azure Batch-analyses.
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="batch-analytics"></a>Batch Analytics
De onderwerpen in Batch Analytics bevatten informatie over de gebeurtenissen en waarschuwingen die beschikbaar zijn voor Batch-service-resources.

Zie [Diagnostische logboekregistratie van Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) voor meer informatie over het inschakelen en gebruiken van Batch diagnostische logboeken.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

De Azure Batch-service verzendt de volgende diagnostische gebeurtenissen gedurende de levensduur van bepaalde Batch-resources.

**Service-logboekgebeurtenissen**
* [Groep maken](batch-pool-create-event.md)
* [Toepassingen verwijderen starten](batch-pool-delete-start-event.md)
* [Groep verwijderen is voltooid](batch-pool-delete-complete-event.md)
* [Groep formaat starten](batch-pool-resize-start-event.md)
* [Groep formaat voltooid](batch-pool-resize-complete-event.md)
* [Taak starten](batch-task-start-event.md)
* [De taak is voltooid](batch-task-complete-event.md)
* [Taak is mislukt](batch-task-fail-event.md)