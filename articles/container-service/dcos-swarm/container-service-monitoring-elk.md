---
title: Een Azure-DC/OS-cluster - stack van ELK bewaken | Microsoft Docs
description: Bewaken van een DC/OS-cluster in Azure Container Service-cluster met ELK (Elasticsearch, Logstash en Kibana).
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: Containers, DC/OS, Azure, bewaking, elk
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: fcfa277cdd0f3cebc0fbbb23e771fb23ffbe2ca6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a>Een Azure Container Service-cluster met ELK bewaken
In dit artikel ziet u hoe de stack ELK (Elasticsearch, Logstash, Kibana) op een DC/OS-cluster in Azure Container Service implementeren. 

## <a name="prerequisites"></a>Vereisten
[Implementeer](container-service-deployment.md) en [verbinding](../container-service-connect.md) een DC/OS-cluster dat is geconfigureerd met Azure Container Service. Bekijk het dashboard in DC/OS en Marathon services [hier](container-service-mesos-marathon-ui.md). Installeer ook het [Marathon-taakverdeling](container-service-load-balancing.md).


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch, Logstash, Kibana)
ELK stack is een combinatie van Elasticsearch, Logstash en Kibana waarmee een end-to-end-stack die kan worden gebruikt om te controleren en analyseren van Logboeken in uw cluster.

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a>De stack ELK op een DC/OS-cluster configureren
Toegang tot uw DC/OS-Webgebruikersinterface via [http://localhost:80 /](http://localhost:80/) eenmaal in de DC/OS-gebruikersinterface Navigeer naar **Universe**. Zoek en installeer Elasticsearch Logstash en Kibana uit de DC/OS-Universe en in die specifieke volgorde. Voor meer informatie over de configuratie van wanneer u naar de **installatie in de geavanceerde** koppeling.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

Zodra de containers voor ELK en weet zetten, u Kibana moet toegankelijk via Marathon-Taakverdeling inschakelen. Navigeer naar **Services** > **kibana**, en klik op **bewerken** zoals hieronder wordt weergegeven.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


Schakelen naar **JSON-modus** en blader naar de sectie labels.
U moet toevoegen een `"HAPROXY_GROUP": "external"` vermelding hier zoals hieronder.
Nadat u op **wijzigingen implementeren**, de container wordt opnieuw opgestart.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Als u controleren of Kibana is geregistreerd als een service in het dashboard HAPROXY wilt, moet u poort 9090 op het cluster agent openen als HAPROXY wordt uitgevoerd op poort 9090.
Standaard openen we poorten 80, 8080, en 443 in het cluster DC/OS-agent.
Vindt u instructies voor het openen van een poort en bieden openbare beoordelen [hier](container-service-enable-public-access.md).

Voor toegang tot het dashboard HAPROXY, opent u de beheerinterface van de Marathon-Taakverdeling op: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.
Zodra u naar de URL navigeren, ziet u het dashboard HAPROXY zoals hieronder wordt weergegeven en ziet u een servicevermelding voor Kibana.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


Voor toegang tot het dashboard Kibana, dat is geïmplementeerd op poort 5601, moet u poort 5601 openen. Volg de instructies [hier](container-service-enable-public-access.md). Open het dashboard Kibana op: `http://localhost:5601`.

## <a name="next-steps"></a>Volgende stappen

* Voor systeem- en logboekbestanden doorsturen en instellingen, Zie [Log-beheer in DC/OS met ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* Om te filteren van Logboeken, Zie [Logboeken filteren met ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

