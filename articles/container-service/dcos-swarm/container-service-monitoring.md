---
title: Monitor Azure DC/OS-cluster - Datadog | Microsoft Docs
description: Een Azure Container Service-cluster met Datadog bewaken. Gebruik de DC/OS-webgebruikersinterface voor het implementeren van de agents Datadog met uw cluster.
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Containers, DC/OS Docker Swarm-Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 9dd451f994940d7cc3a59bd7fd08a8f067345e34
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Een Azure Container Service DC/OS-cluster met Datadog bewaken
In dit artikel wordt we Datadog agents op alle knooppunten van een agent in uw Azure Container Service-cluster implementeren. U moet een account met Datadog voor deze configuratie. 

## <a name="prerequisites"></a>Vereisten
[Implementeer](container-service-deployment.md) en [verbind](../container-service-connect.md) een cluster dat door Azure Container Service is geconfigureerd. Verken de [Marathon-gebruikersinterface](container-service-mesos-marathon-ui.md). Ga naar [http://datadoghq.com](http://datadoghq.com) voor het instellen van een Datadog-account. 

## <a name="datadog"></a>Datadog
Datadog is een controleservice die bewakingsgegevens moeten worden verzameld uit uw containers in uw Azure Container Service-cluster. Datadog heeft een Docker-integratie Dashboard waarin u de specifieke metrische gegevens binnen uw containers kunt zien. Metrische gegevens die afkomstig zijn van uw containers worden ingedeeld op basis van CPU, geheugen, netwerk en i/o. Datadog splitst metrische gegevens in containers en afbeeldingen. Er is een voorbeeld van hoe de gebruikersinterface voor CPU-gebruik eruit hieronder.

![Datadog gebruikersinterface](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Een implementatie met Datadog met Marathon configureren
Deze stappen wordt beschreven hoe u configureren en implementeren van toepassingen Datadog aan het cluster met Marathon. 

Toegang tot uw DC/OS-Webgebruikersinterface via [http://localhost:80 /](http://localhost:80/). Eenmaal in de DC/OS-gebruikersinterface Navigeer naar de 'Universe' die is op de links onder en zoek naar 'Datadog' en klik op 'Installeren'.

![Datadog pakket binnen de DC/OS-Universe](./media/container-service-monitoring/datadog1.png)

Nu om de configuratie te voltooien, moet u een account Datadog of een gratis proefaccount. Nadat u bent aangemeld bij het uiterlijk van de website Datadog naar links en gaat u naar integraties -> vervolgens [API's](https://app.datadoghq.com/account/settings#api). 

![Datadog API-sleutel](./media/container-service-monitoring/datadog2.png)

Naast uw API-sleutel in de configuratie van de Datadog binnen de DC/OS-Universe invoeren. 

![Datadog configuratie in de DC/OS-Universe](./media/container-service-monitoring/datadog3.png) 

In de bovenstaande configuratie exemplaren zijn ingesteld op 10000000 doet wanneer een nieuw knooppunt wordt toegevoegd aan het cluster Datadog implementeren automatisch een agent naar dat knooppunt. Dit is een tijdelijke oplossing. Als u het pakket moet u Ga terug naar de website Datadog en vinden hebt geïnstalleerd '[Dashboards](https://app.datadoghq.com/dash/list). " Daar ziet u aangepaste en integratie-Dashboards. De [Docker-dashboard](https://app.datadoghq.com/screen/integration/docker) hebben alle de container metrische gegevens die u nodig hebt voor het bewaken van uw cluster. 

