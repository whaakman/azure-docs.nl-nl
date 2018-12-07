---
title: (AFGESCHAFT) Azure DC/OS-cluster - Datadog bewaken
description: Een Azure Container Service-cluster bewaken met Datadog. Gebruik de DC/OS-webgebruikersinterface voor het implementeren van de agents Datadog met uw cluster.
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: d881a5c0f994b627b4c7c3da362672b3b887cd5e
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52996144"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>(AFGESCHAFT) Een Azure Container Service DC/OS-cluster bewaken met Datadog

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

In dit artikel implementeren we Datadog agents op alle agentknooppunten in uw Azure Container Service-cluster. Voor deze configuratie moet u een account met Datadog. 

## <a name="prerequisites"></a>Vereisten
[Implementeer](container-service-deployment.md) en [verbind](../container-service-connect.md) een cluster dat door Azure Container Service is geconfigureerd. Verken de [Marathon-gebruikersinterface](container-service-mesos-marathon-ui.md). Ga naar [ http://datadoghq.com ](http://datadoghq.com) het instellen van een account met Datadog. 

## <a name="datadog"></a>Datadog
Datadog is een bewakingsservice waarmee gegevens uit uw containers in uw Azure Container Service-cluster verzamelt. Datadog heeft een Docker-integratie-Dashboard waar u specifieke metrische gegevens in uw containers kunt zien. Metrische gegevens die zijn verzameld uit uw containers worden georganiseerd door CPU, geheugen, netwerk- en i/o. Datadog splitst metrische gegevens in containers en afbeeldingen. Een voorbeeld van de gebruikersinterface ziet er als CPU-gebruik lager is dan.

![Gebruikersinterface met Datadog](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Een implementatie met Datadog met Marathon configureren
Deze stappen ziet u hoe u kunt configureren en implementeren van Datadog toepassingen in uw cluster met Marathon. 

Toegang tot uw DC/OS-gebruikersinterface via [ http://localhost:80/ ](http://localhost:80/). Eenmaal in de DC/OS-gebruikersinterface Navigeer naar de "Universe" is onderaan links en zoek naar 'Datadog' en klik op "Installeren".

![Pakket met Datadog binnen de DC/OS-universum](./media/container-service-monitoring/datadog1.png)

Nu de configuratie te voltooien moet u een Datadog-account of een gratis proefaccount. Nadat u bent aangemeld bij het zoeken van de website Datadog aan de linkerkant en gaat u naar integraties vervolgens -> [API's](https://app.datadoghq.com/account/settings#api). 

![Datadog API-sleutel](./media/container-service-monitoring/datadog2.png)

Voer vervolgens uw API-sleutel in de configuratie van de Datadog binnen de DC/OS-universum. 

![Datadog configuratie in het DC/OS-universum](./media/container-service-monitoring/datadog3.png) 

In de bovenstaande configuratie exemplaren zijn ingesteld op 10000000, wanneer een nieuw knooppunt wordt toegevoegd aan het cluster Datadog wordt automatisch een agent worden geïmplementeerd met dat knooppunt. Dit is een tijdelijke oplossing. Nadat u het pakket moet u Ga terug naar de website Datadog en vinden hebt geïnstalleerd '[Dashboards](https://app.datadoghq.com/dash/list). " Daar ziet u aangepaste en integratie van Dashboards. De [Docker dashboard](https://app.datadoghq.com/screen/integration/docker) heeft alle de container metrische gegevens die u nodig hebt voor het bewaken van uw cluster. 

