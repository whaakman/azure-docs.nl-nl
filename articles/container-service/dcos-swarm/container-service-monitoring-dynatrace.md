---
title: (AFGESCHAFT) Azure DC/OS-cluster - Dynatrace bewaken
description: Een Azure Container Service DC/OS-cluster bewaken met Dynatrace. Het Dynatrace OneAgent implementeren met behulp van de DC/OS-dashboard.
services: container-service
author: MartinGoodwell
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 8f34a00d9256c288a2842e905c06d5336522eece
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53002005"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>(AFGESCHAFT) Een Azure Container Service DC/OS-cluster met Dynatrace SaaS/beheerde bewaken

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

In dit artikel laten we zien u over het implementeren van de [Dynatrace](https://www.dynatrace.com/) OneAgent voor het bewaken van alle agentknooppunten in uw Azure Container Service-cluster. U hebt een account met Dynatrace SaaS/beheerde nodig voor deze configuratie. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS of worden beheerd
Dynatrace is een cloud-eigen bewakingsoplossing voor zeer dynamische container en cluster-omgevingen. Hiermee kunt u beter uw containerimplementaties en geheugentoewijzingen met behulp van realtime gebruiksgegevens kunt optimaliseren. Het is geschikt voor toepassingen en infrastructuur automatisch dicht door geautomatiseerde basislijnen, probleem correlatie en detectie van de hoofdoorzaak te geven.

De volgende afbeelding ziet u de UI Dynatrace:

![Dynatrace-UI](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Vereisten 
[Implementeer](container-service-deployment.md) en [verbinding](./../container-service-connect.md) naar een cluster dat is geconfigureerd door Azure Container Service. Verken de [Marathon-gebruikersinterface](container-service-mesos-marathon-ui.md). Ga naar [ https://www.dynatrace.com/trial/ ](https://www.dynatrace.com/trial/) voor het instellen van een Dynatrace SaaS-account.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Een implementatie met Dynatrace met Marathon configureren
Deze stappen ziet u hoe u kunt configureren en implementeren van Dynatrace toepassingen in uw cluster met Marathon.

1. Toegang tot uw DC/OS-gebruikersinterface via [ http://localhost:80/ ](http://localhost:80/). Eenmaal in de gebruikersinterface van DC/OS, navigeer naar de **universum** tabblad en zoek vervolgens **Dynatrace**.

    ![Dynatrace in DC/OS-universum](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. De configuratie te voltooien, moet u een Dynatrace SaaS-account of een gratis proefaccount. Zodra u zich bij het Dynatrace-dashboard aanmeldt, selecteert u **implementeren Dynatrace**.

    ![Dynatrace PaaS-integratie instellen](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Selecteer op de pagina **PaaS-integratie instellen**. 

    ![Dynatrace API-token](./media/container-service-monitoring-dynatrace/api-token.png) 

4. Voer uw API-token in de configuratie van de Dynatrace OneAgent binnen de DC/OS-universum. 

    ![Dynatrace OneAgent configuratie in het DC/OS-universum](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. De exemplaren ingesteld op het aantal knooppunten dat u van plan bent om uit te voeren. Hoe hoger de waarde ook werkt, DC/OS, maar zal blijven proberen te vinden van nieuwe exemplaren totdat dit aantal is bereikt. Als u liever, kunt u dit ook instellen op een waarde zoals 1000000. In dit geval, wanneer een nieuw knooppunt wordt toegevoegd aan het cluster, Dynatrace automatisch wordt geïmplementeerd een agent op dat nieuwe knooppunt voor de prijs van DC/OS voortdurend probeert te implementeren van verdere exemplaren.

    ![Dynatrace-configuratie in de DC/OS-universum-exemplaren](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Volgende stappen

Nadat u het pakket hebt geïnstalleerd, gaat u terug naar het Dynatrace-dashboard. U kunt de verschillende gebruikswaarden voor de containers in uw cluster verkennen. 