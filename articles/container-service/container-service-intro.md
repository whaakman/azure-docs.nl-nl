---
title: Docker-containercluster in Azure-cloud | Microsoft Docs
description: Azure Container Service biedt u mogelijkheden voor het eenvoudig maken, configureren en beheren van een cluster virtuele machines die vooraf zijn geconfigureerd voor het uitvoeren van beperkte toepassingen.
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure
ms.assetid: 52da4163-1182-4b2e-be00-4951e5c1da16
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: cc3cc2dce324942f184b6a520dc4db28518a3091
ms.openlocfilehash: 0521fbd689233c0f2359a10006e24c75c8997732


---
# <a name="azure-container-service-introduction"></a>Kennismaking met Azure Container Service
Azure Container Service helpt u bij het eenvoudig maken, configureren en beheren van een cluster virtuele machines die vooraf zijn geconfigureerd voor het uitvoeren van beperkte toepassingen. De service maakt gebruik van een geoptimaliseerde configuratie van populaire open-source tools voor planning en orchestration. U kunt hierbij dan uitgaan van uw eigen vaardigheden of gebruikmaken van een grote en groeiende groep community experts om toepassingen op basis van containers te implementeren en beheren in Microsoft Azure.

![Azure Container Service biedt u de mogelijkheid beperkte toepassingen op meerdere hosts te beheren in Azure.](./media/acs-intro/acs-cluster-new.png)

Azure Container Service maakt gebruik van de Docker-containerindeling om ervoor te zorgen dat uw toepassingscontainers volledig draagbaar zijn. De service biedt ook ondersteuning als u Marathon en DC/OS, Docker Swarm of Kubernetes kiest, zodat u deze toepassingen kunt schalen naar duizenden of zelfs tienduizenden containers.

>[!NOTE]
> Ondersteuning voor Kubernetes in Azure Container Service is momenteel in de preview-fase.
>

Azure Container Service biedt het voordeel van de hoogwaardige functies van Azure in combinatie met draagbare toepassingen, met inbegrip van draagbaarheid in de orchestration-lagen.

## <a name="using-azure-container-service"></a>Azure Container Service gebruiken
Azure Container Service is bedoeld om een hostomgeving voor containers te bieden door gebruik te maken van de open source tools en technologieën die tegenwoordig populair zijn onder onze klanten. Daarom stellen we de standaard-API-eindpunten beschikbaar voor uw gekozen orchestrator (DC/OS, Docker Swarm of Kubernetes). Met behulp van deze eindpunten kunt u gebruikmaken van alle software die geschikt is voor communicatie met deze eindpunten. In het geval van een Docker Swarm-eindpunt kunt u bijvoorbeeld kiezen om gebruik te maken van de Docker CLI (opdrachtregelinterface). Voor DC/OS kunt u kiezen voor de DCOS CLI. Voor Kubernetes zou u kubectl kunnen gebruiken.

## <a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Een Docker-cluster maken met behulp van Azure Container Service
Als u Azure Container Service wilt gebruiken, moet u eerst een Azure Container Service-cluster implementeren via de portal (zoek naar 'Azure Container Service'). U kunt hierbij gebruikmaken van een Azure Resource Manager-sjabloon ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm), [DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) of [Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)), maar u kunt ook de [CLI](/articles/xplat-cli-install.md) gebruiken. De verstrekte snelstartsjablonen kunnen worden aangepast om aanvullende of geavanceerde Azure-configuratie toe te voegen. Zie [Een Azure Container Service-cluster implementeren](container-service-deployment.md) voor meer informatie over de implementatie van een Azure Container Service-cluster.

## <a name="deploying-an-application"></a>Een toepassing implementeren
Voor orchestration met Azure Container Service kunt u kiezen uit Docker Swarm, DC/OS of Kubernetes. De implementatie van uw toepassing hangt af van de orchestrator die u kiest.

### <a name="using-dcos"></a>Met DC/OS
DC/OS is een gedistribueerd besturingssysteem op basis van de Apache Mesos-kernel voor gedistribueerde systemen. Apache Mesos is ondergebracht bij de Apache Software Foundation. Onder de gebruikers en medewerkers van het product tellen we enkele van de [grootste namen in IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/).

![Azure Container Service geconfigureerd voor Swarm met agents en masters.](media/acs-intro/dcos.png)

DC/OS en Apache Mesos bieden een indrukwekkende functieset:

* In de praktijk beproefde schaalbaarheid
* Fouttolerante gerepliceerde master en slaves op basis van Apache ZooKeeper
* Ondersteuning voor containers met Docker-indeling
* Systeemeigen isolatie tussen taken met Linux-containers
* Multiresource planning (geheugen, CPU, schijf en poorten)
* Java, Python en C++ API's voor het ontwikkelen van nieuwe parallelle toepassingen
* Een web-UI voor weergave van de clusterstatus

DC/OS in Azure Container Service omvat standaard het Marathon orchestration platform voor het plannen van workloads. De DC/OS-implementatie van ACS bevat echter ook Mesosphere Universe-services die u kunt toevoegen aan uw service, zoals Spark, Hadoop, Cassandra en nog veel meer.

![DC/OS Universe in Azure Container Service](media/dcos/universe.png)

#### <a name="using-marathon"></a>Met Marathon
Marathon is een clusterbreed initialisatie- en beheersysteem voor services in cgroups of, in het geval van Azure Container Service, voor containers in Docker-indeling. Marathon biedt een web-UI van waaruit u uw toepassingen kunt implementeren. U hebt hiertoe toegang via een URL die er ongeveer als volgt uitziet: `http://DNS_PREFIX.REGION.cloudapp.azure.com`. DNS\_VOORVOEGSEL en REGIO worden gedefinieerd tijdens de implementatie. Indien gewenst, kunt u ook uw eigen DNS-naam opgeven. Zie [Containerbeheer via de web UI](container-service-mesos-marathon-ui.md) voor meer informatie over het uitvoeren van een container met de Marathon web-UI.

![Lijst met Marathon-toepassingen](media/dcos/marathon-applications-list.png)

U kunt ook de REST API's gebruiken voor communicatie met Marathon. Er zijn een aantal clientbibliotheken beschikbaar voor elke tool. Hier vindt u tools in diverse talen. Uiteraard kunt u ook het HTTP-protocol in elke gewenste taal gebruiken. Daarnaast bieden veel populaire DevOps-tools ondersteuning voor Marathon. Zo beschikt u over maximale flexibiliteit voor uw bedrijfsteam wanneer u met een Azure Container Service-cluster werkt. Zie [Containerbeheer met de REST API](container-service-mesos-marathon-rest.md) voor meer informatie over het uitvoeren van een container met de Marathon REST API.

### <a name="using-docker-swarm"></a>Met Docker Swarm
Docker Swarm biedt systeemeigen clustering voor Docker. Aangezien Docker Swarm werkt met de standaard Docker API, kan elke tool die al communiceert met een Docker-daemon, gebruikmaken van Swarm om meerdere hosts op transparante manier te schalen in Azure Container Service.

![Azure Container Service geconfigureerd voor DC/OS, met jumpbox, agents en masters.](media/acs-intro/acs-swarm2.png)

Tools die worden ondersteund voor het beheer van containers in een Swarm-cluster, zijn bijvoorbeeld:

* Dokku
* Docker CLI en Docker Compose
* Krane
* Jenkins

### <a name="using-kubernetes"></a>Kubernetes gebruiken
Kubernetes is een populair open-source containerorchestratieprogramma uit de productieklasse voor geautomatiseerde containerimplementatie, schaling en beheer van containertoepassingen. Omdat het een open-source oplossing is en up-to-date wordt gehouden door de open-source community, werkt het naadloos in Azure Container Service en kan het worden gebruikt om containers op de gewenste schaal uit te voeren in Azure Container Service.

![Azure Container Service, geconfigureerd voor Kubernetes, met agents en masters.](media/acs-intro/kubernetes.png)

De applicatie bevat een uitgebreide set functies, zoals:
* Horizontaal schalen
* Servicedetectie en taakverdeling
* Geheimen en configuratiebeheer
* Op API gebaseerde geautomatiseerde implementaties en terugdraaiacties
* Automatisch herstel
* en nog veel meer.



## <a name="videos"></a>Video's
Aan de slag met Azure Container Service (101):  

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Container-Service-101/player]
>
>

Toepassingen ontwikkelen met Azure Container Service (Build 2016)

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/B822/player]
>
>



<!--HONumber=Feb17_HO1-->


