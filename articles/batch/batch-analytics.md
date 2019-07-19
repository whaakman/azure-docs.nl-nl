---
title: Azure Batch Analytics | Microsoft Docs
description: Naslag informatie voor Azure Batch Analytics.
services: batch
author: laurenhughes
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 0f8233846f3835a2bbccf9a7072f223d435b90af
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323830"
---
# <a name="batch-analytics"></a>Batch-analyse
De onderwerpen in batch Analytics bevatten referentie-informatie voor de gebeurtenissen en waarschuwingen die beschikbaar zijn voor batch-service resources.

Zie [Azure batch diagnostische logboek registratie](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) voor meer informatie over het inschakelen en gebruiken van batch diagnose Logboeken.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

De Azure Batch-service verzendt de volgende diagnostische logboek gebeurtenissen tijdens de levens duur van bepaalde batch-resources.

**Gebeurtenissen van de service-logboek**
* [Groep maken](batch-pool-create-event.md)
* [Pool verwijderen starten](batch-pool-delete-start-event.md)
* [Pool verwijderen voltooid](batch-pool-delete-complete-event.md)
* [Pool resize start](batch-pool-resize-start-event.md)
* [Formaat van pool wijzigen voltooid](batch-pool-resize-complete-event.md)
* [Taak starten](batch-task-start-event.md)
* [De taak is voltooid](batch-task-complete-event.md)
* [Taak is mislukt](batch-task-fail-event.md)