---
title: Monitor Azure DC/OS-cluster - Dynatrace | Microsoft Docs
description: Een Azure Container Service DC/OS-cluster met Dynatrace bewaken. De Dynatrace OneAgent implementeren met behulp van de DC/OS-dashboard.
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: Containers, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 6fa23728680779e33eda7bb9aa8a01b9cad9a82b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>Een Azure Container Service DC/OS-cluster met Dynatrace SaaS/beheerde bewaken
In dit artikel wordt beschreven hoe u voor het implementeren van de [Dynatrace](https://www.dynatrace.com/) OneAgent voor het bewaken van de agent-knooppunten in uw Azure Container Service-cluster. Voor deze configuratie moet u een account met Dynatrace SaaS/beheerd. 

## <a name="dynatrace-saasmanaged"></a>SaaS Dynatrace/beheerd
Dynatrace is een oplossing voor cloud-systeemeigen controle voor maximaal dynamische container en cluster-omgevingen. Hiermee kunt u beter uw containerimplementaties en geheugentoewijzingen optimaliseren door van realtime-gebruiksgegevens. Het is geschikt voor toepassingen en infrastructuur problemen automatisch dicht door geautomatiseerde basislijnen, probleem correlatie en detectie van de hoofdoorzaak te geven.

De volgende afbeelding ziet de UI Dynatrace:

![Dynatrace gebruikersinterface](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Vereisten 
[Implementeer](container-service-deployment.md) en [verbinding](./../container-service-connect.md) naar een cluster geconfigureerd door Azure Container Service. Verken de [Marathon-gebruikersinterface](container-service-mesos-marathon-ui.md). Ga naar [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) voor het instellen van een Dynatrace SaaS-account.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Een implementatie met Dynatrace met Marathon configureren
Deze stappen ziet u hoe u kunt configureren en implementeren van toepassingen aan het cluster met Marathon Dynatrace.

1. Toegang tot uw DC/OS-Webgebruikersinterface via [http://localhost:80 /](http://localhost:80/). Eenmaal in de DC/OS-gebruikersinterface, navigeer naar de **Universe** tabblad en zoek vervolgens naar **Dynatrace**.

    ![Dynatrace in DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. Voor het voltooien van de configuratie, moet u een Dynatrace SaaS-account of een gratis proefaccount. Zodra u zich bij het Dynatrace dashboard aanmeldt, selecteert u **Dynatrace implementeren**.

    ![Dynatrace PaaS-integratie instellen](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Selecteer op de pagina **PaaS-integratie instellen**. 

    ![Dynatrace API-token](./media/container-service-monitoring-dynatrace/api-token.png) 

4. Voer uw API-token in de configuratie Dynatrace OneAgent binnen de DC/OS-Universe. 

    ![Dynatrace OneAgent configuratie in de DC/OS-Universe](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. De exemplaren ingesteld op het aantal knooppunten dat u van plan bent om uit te voeren. Hoe hoger de waarde ook werkt, maar de DC/OS zal blijven proberen nieuwe instanties vinden totdat dit aantal is bereikt. Als u liever, kunt u dit ook instellen op een waarde zoals 1000000. In dit geval wanneer een nieuw knooppunt aan het cluster wordt toegevoegd, Dynatrace automatisch wordt geïmplementeerd een agent op dat knooppunt nieuwe voor de prijs van DC/OS voortdurend wilt implementeren exemplaren.

    ![Dynatrace configuratie in de DC/OS Universe-exemplaren](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Volgende stappen

Nadat u het pakket hebt geïnstalleerd, gaat u terug naar het dashboard Dynatrace. U kunt het gebruik van andere metrische gegevens voor de containers verkennen binnen het cluster. 