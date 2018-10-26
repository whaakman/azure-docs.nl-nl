---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 79fb8517ec6880e8a3eae0e74275567a24644b87
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134271"
---
## <a name="deployment-customization"></a>Implementatieaanpassing

Het implementatieproces wordt ervan uitgegaan dat het ZIP-bestand dat u een kant-en-klaar-app bevat. Standaard worden zonder aanpassingen uitgevoerd. Om in te schakelen de dezelfde buildprocessen die u aan met continue integratie, moet u het volgende toevoegen aan uw toepassing-instellingen:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Wanneer u ZIP-push implementeren, deze instelling is **false** standaard. De standaardwaarde is **waar** voor continue integratie-implementaties. Als de waarde **waar**, uw instellingen met betrekking tot implementatie worden gebruikt tijdens de implementatie. U kunt deze instellingen configureren als app-instellingen of in een configuratiebestand .deployment die zich in de hoofdmap van het ZIP-bestand. Zie voor meer informatie, [opslagplaats en met betrekking tot implementatie-instellingen](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) in de implementatie.