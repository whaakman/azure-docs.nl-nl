---
title: Azure Batch Analytics | Microsoft Docs
description: Verwijzing voor Azure Batch-analyses.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: 4c81acb282b24bbd899227c4dcc449fed4ba6c7d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="batch-analytics"></a>Batch-analyse
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