---
title: (AFGESCHAFT) Agentpools van DC/OS voor Azure Container Service
description: De werking van de openbare en persoonlijke agents van toepassingen met een Azure Container Service DC/OS-cluster
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 01/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 03cacda1aa405cb2d0ded579c8ddb5f6011ce3bb
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113784"
---
# <a name="deprecated-dcos-agent-pools-for-azure-container-service"></a>(AFGESCHAFT) Agentpools van DC/OS voor Azure Container Service

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

DC/OS-clusters in Azure Container Service bevatten agentknooppunten in twee groepen, een openbare groep en een persoonlijke groep. Een toepassing kan worden geïmplementeerd met een groep, die betrekking hebben op toegankelijkheid tussen machines in uw containerservice. De machines kunnen worden blootgesteld aan internet (openbaar) of interne (particuliere) bewaard. Dit artikel geeft een kort overzicht van waarom er openbare en privé-groepen zijn.


* **Privé-agents**: Privé-agent-knooppunten worden uitgevoerd via een niet-routeerbare netwerk. Dit netwerk is alleen toegankelijk vanuit de zone van de beheerder of via de openbare zone edge router. DC/OS wordt standaard gestart van apps op privé-agent-knooppunten. 

* **Openbare agents**: Openbare agent-knooppunten DC/OS-apps en services worden uitgevoerd via een openbaar netwerk. 

Zie voor meer informatie over netwerkbeveiliging DC/OS, de [DC/OS-documentatie](https://docs.mesosphere.com/).

## <a name="deploy-agent-pools"></a>Agentpools implementeren

De agentpools van DC/OS In Azure Container Service zijn als volgt gemaakt:

* De **persoonlijke groep** bevat het aantal agentknooppunten dat u wanneer opgeeft u [de DC/OS-cluster implementeren](container-service-deployment.md). 

* De **openbare groep** een vooraf bepaald aantal agentknooppunten in eerste instantie bevat. Deze groep wordt automatisch toegevoegd wanneer het DC/OS-cluster is ingericht.

De persoonlijke groep en de openbare groep zijn schaalsets voor virtuele machine van Azure. U kunt het formaat van deze groepen na de implementatie.

## <a name="use-agent-pools"></a>Agentpools gebruiken
Standaard **Marathon** implementeert een nieuwe toepassing aan de *persoonlijke* agentknooppunten. U moet expliciet de toepassing implementeren in de *openbare* knooppunten tijdens het maken van de toepassing. Selecteer de **optioneel** tabblad en voer **slave_public** voor de **geaccepteerde resourcerollen** waarde. Dit proces wordt gedocumenteerd [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) en in de [DC/OS](https://docs.mesosphere.com/1.7/administration/installing/oss/custom/create-public-agent/) documentatie.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [beheer van uw DC/OS-containers](container-service-mesos-marathon-ui.md).

* Meer informatie over het [opent u de firewall](container-service-enable-public-access.md) verstrekt door Azure voor openbare toegang tot uw DC/OS-containers.

