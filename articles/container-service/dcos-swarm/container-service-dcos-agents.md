---
title: Groepen met DC/OS-agent voor Azure Container Service
description: De werking van de openbare en persoonlijke agent-adresgroepen met een Azure Container Service DC/OS-cluster
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: e82a6c1ee2d45cd07f4e87c43ad4fb1149ef555c
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a>Groepen met DC/OS-agent voor Azure Container Service
DC/OS-clusters in Azure Container Service bevatten agent knooppunten in twee groepen, een openbare toepassingen en een persoonlijke toepassingen. Een toepassing kan worden geïmplementeerd aan de groep, toegankelijkheid tussen computers in uw containerservice beïnvloeden. De machines kunnen worden blootgesteld aan internet (openbaar) of interne (particuliere) bewaard. Dit artikel bevat een kort overzicht van waarom er groepen van openbare en persoonlijke zijn.


* **Persoonlijke agents**: persoonlijke agent knooppunten uitvoeren via een niet-routeerbaar netwerk. Dit netwerk is alleen toegankelijk vanuit de zone van de beheerder of via de router van de rand openbare zone. Standaard start DC/OS-apps op persoonlijke agent knooppunten. 

* **Openbare agents**: openbare agent knooppunten uitvoeren DC/OS-apps en services via een openbaar toegankelijk netwerk. 

Zie voor meer informatie over netwerkbeveiliging DC/OS de [DC/OS-documentatie](https://dcos.io/docs/1.7/administration/securing-your-cluster/).

## <a name="deploy-agent-pools"></a>Agent-groepen implementeren

De DC/OS-agent van toepassingen In Azure Container Service gemaakt als volgt:

* De **persoonlijke groep** bevat het nummer van de agent-knooppunten opgeven wanneer u [de DC/OS-cluster implementeren](container-service-deployment.md). 

* De **openbare groep** in eerste instantie bevat een vooraf vastgesteld aantal knooppunten van de agent. Deze groep wordt automatisch toegevoegd wanneer de DC/OS-cluster is ingericht.

De persoonlijke groep en de openbare toepassingen worden virtuele Azure-machine-schaalsets. U kunt deze groepen wijzigen na de implementatie.

## <a name="use-agent-pools"></a>Agent pools gebruiken
Standaard **Marathon** implementeert een nieuwe toepassing aan de *persoonlijke* agent knooppunten. U moet expliciet de toepassing implementeren naar de *openbare* knooppunten tijdens het maken van de toepassing. Selecteer de **optioneel** tabblad en voer **slave_public** voor de **geaccepteerde resourcerollen** waarde. Dit proces wordt beschreven [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) en in de [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentatie.

## <a name="next-steps"></a>Volgende stappen
* Lees meer over [beheer van uw DC/OS-containers](container-service-mesos-marathon-ui.md).

* Meer informatie over hoe [de firewall openen](container-service-enable-public-access.md) verstrekt door Azure voor openbare toegang tot uw DC/OS-containers.

