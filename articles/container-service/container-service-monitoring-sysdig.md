---
title: Een Azure Container Service-cluster met Sysdig bewaken | Microsoft Docs
description: Een Azure Container Service-cluster met Sysdig bewaken.
services: container-service
documentationcenter: 
author: rbitia
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Containers, DC/OS, Azure
ms.assetid: 91d9a28a-3a52-4194-879e-30f2fa3d946b
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: t-ribhat
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 6df2b6ee0064a2baa79a154641f097b11cb55059


---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Een Azure Container Service-cluster met Sysdig bewaken
In dit artikel implementeren we Sysdig-agents op alle agentknooppunten in uw Azure Container Service-cluster. Voor deze configuratie hebt u een Sysdig-account nodig. 

## <a name="prerequisites"></a>Vereisten
[Implementeer](container-service-deployment.md) en [verbind](container-service-connect.md) een cluster dat door Azure Container Service is geconfigureerd. Verken de [Marathon-gebruikersinterface](container-service-mesos-marathon-ui.md). Ga naar [http://app.sysdigcloud.com](http://app.sysdigcloud.com) om een Sysdig-cloudaccount in te stellen. 

## <a name="sysdig"></a>Sysdig
Sysdig is een bewakingsservice waarmee u de containers in uw cluster kunt bewaken. Sysdig is handig bij het oplossen van problemen, maar biedt ook algemene bewakingswaarden voor CPU, netwerk, geheugen en I/O. Met Sysdig kunt u gemakkelijk zien welke containers het hardst werken of in wezen het geheugen en de CPU het meest gebruiken. Deze weergave vindt u in de sectie ‘Overview’ (Overzicht), die zich momenteel in de bètafase bevindt. 

![Gebruikersinterface van Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Een Sysdig-implementatie met Marathon configureren
Deze stappen geven aan hoe u met Marathon Sysdig-toepassingen in uw cluster configureert en implementeert. 

Ga naar uw DC/OS-gebruikersinterface via [http://localhost:80/](http://localhost:80/). Zodra u in de gebruikersinterface van DC/OS bent, navigeert u naar ‘Universe’ (Universum), onderaan links, en zoekt u daarna ‘Sysdig’.

![Sysdig in het DC/OS-universum](./media/container-service-monitoring-sysdig/sysdig1.png)

Om nu de configuratie te voltooien, hebt u een Sysdig-cloudaccount of gratis proefaccount nodig. Nadat u bent aangemeld bij de Sysdig-cloudwebsite, klikt u op uw gebruikersnaam. Op de pagina ziet u uw ‘toegangssleutel’. 

![Sysdig API-sleutel](./media/container-service-monitoring-sysdig/sysdig2.png) 

Voer vervolgens in het DC/OS-universum uw toegangssleutel in de Sysdig-configuratie in. 

![Sysdig-configuratie in het DC/OS-universum](./media/container-service-monitoring-sysdig/sysdig3.png)

Stel nu de exemplaren in op 10000000, zodat telkens wanneer aan het cluster een nieuw knooppunt wordt toegevoegd, Sysdig automatisch op dat nieuwe knooppunt een agent implementeert. Dit is een tijdelijke oplossing om ervoor te zorgen dat Sysdig op alle nieuwe agents binnen het cluster wordt geïmplementeerd. 

![Sysdig-configuratie in de instanties van het DC/OS-universum](./media/container-service-monitoring-sysdig/sysdig4.png)

Nadat u het pakket hebt geïnstalleerd, navigeert u terug naar de Sysdig-gebruikersinterface en kunt u de verschillende gebruikswaarden voor de containers in uw cluster verkennen. 




<!--HONumber=Dec16_HO1-->


