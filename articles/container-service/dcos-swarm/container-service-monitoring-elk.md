---
title: (AFGESCHAFT) Een Azure DC/OS-cluster - ELK-stack bewaken
description: Een DC/OS-cluster in Azure Container Service-cluster met ELK (Elasticsearch, Logstash en Kibana) bewaken.
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 342cf23db2df7d7c79a2b56df96d1a78d6ba215e
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998143"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-elk"></a>(AFGESCHAFT) Een Azure Container Service-cluster met ELK bewaken

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

In dit artikel wordt gedemonstreerd hoe u de ELK (Elasticsearch, Logstash, Kibana)-stack op een DC/OS-cluster in Azure Container Service implementeert. 

## <a name="prerequisites"></a>Vereisten
[Implementeer](container-service-deployment.md) en [verbinding](../container-service-connect.md) een DC/OS-cluster dat is geconfigureerd door Azure Container Service. De DC/OS-dashboard en de Marathon-services verkennen [hier](container-service-mesos-marathon-ui.md). Installeer ook de [Marathon Load Balancer](container-service-load-balancing.md).


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch, Logstash, Kibana)
ELK-stack is een combinatie van Elasticsearch, Logstash en Kibana waarmee een end-to-stack die kan worden gebruikt om te controleren en analyseren van Logboeken in uw cluster.

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a>De ELK-stack op een DC/OS-cluster configureren
Toegang tot uw DC/OS-gebruikersinterface via [ http://localhost:80/ ](http://localhost:80/) eenmaal in de DC/OS-gebruikersinterface Navigeer naar **universum**. Zoeken en Elasticsearch, Logstash en Kibana installeren van de DC/OS-universum en in die specifieke volgorde. U kunt meer wilt weten over de configuratie als u gaat u naar de **geavanceerde installatie** koppeling.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

Zodra de ELK-containers en zijn ingesteld en geactiveerd, u Kibana moet toegankelijk via Marathon-LB inschakelen. Navigeer naar **Services** > **kibana**, en klikt u op **bewerken** zoals hieronder wordt weergegeven.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


In-/ uitschakelen op **JSON-modus** en blader naar de sectie labels.
U wilt toevoegen een `"HAPROXY_GROUP": "external"` vermelding hier zoals hieronder.
Nadat u op **wijzigingen implementeren**, uw container opnieuw is opgestart.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Als u controleren of wilt dat Kibana is geregistreerd als een service in het dashboard HAPROXY, moet u poort 9090 op het cluster agent openen terwijl HAPROXY wordt uitgevoerd op poort 9090.
Standaard wordt geopend. poorten 80, 8080, en 443 in het DC/OS-agent-cluster.
Vindt u instructies voor het openen van een poort en bieden openbare beoordelen [hier](container-service-enable-public-access.md).

Voor toegang tot het dashboard HAPROXY, opent u de Marathon-LB-beheerinterface op: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.
Navigeer naar de URL, ziet u het dashboard HAPROXY zoals hieronder wordt weergegeven en ziet u een servicevermelding voor Kibana.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


Voor toegang tot de Kibana-dashboard, dat is geïmplementeerd op poort 5601, moet u open poort 5601. Volg de instructies [hier](container-service-enable-public-access.md). Open de Kibana-dashboard op: `http://localhost:5601`.

## <a name="next-steps"></a>Volgende stappen

* Voor systeem- en logboek doorsturen en instellingen, Zie [Log Management in DC/OS met ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* Om te filteren van Logboeken, Zie [Logboeken filteren met ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

