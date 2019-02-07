---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ms.openlocfilehash: 5cd64b806392162fd3bee14ddaf607385ac05264
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55807136"
---
In de werkstromen validatie **planning** een test maakt gebruik van het niveau van de werkstroom algemene parameters die u hebt opgegeven tijdens het maken van de werkstroom (Zie [werkstroom algemene parameters voor Azure Stack-validatie als een Service](../azure-stack-vaas-parameters.md)). Als een van de parameterwaarden test ongeldig geworden, moet u ze resupply volgens de instructies in [wijzigen werkstroomparameters](../azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> Een bestaand exemplaar van een validatietest planning maakt een nieuw exemplaar in plaats van het oude-exemplaar in de portal. Logboeken voor het oude exemplaar worden bewaard, maar zijn niet toegankelijk is vanaf de portal.  
Wanneer een test is voltooid en de **planning** actie wordt uitgeschakeld.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. Selecteer **planning** in het contextmenu om een prompt voor het plannen van de test-exemplaar te openen.

1. Controleer de testparameters en selecteer vervolgens **indienen** voor het plannen van de test voor de uitvoering.