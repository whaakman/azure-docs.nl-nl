---
title: Zelfstudie over het installeren van een toepassing op uw zelfstandige Service Fabric cluster - Azure Service Fabric | Microsoft Docs
description: In deze zelfstudie leert u hoe u een toepassing installeren in uw zelfstandige Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 58daa53dba8f18c3f73253008fe687ba8b0d0839
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274071"
---
# <a name="tutorial-deploy-an-application-on-your-service-fabric-standalone-cluster"></a>Zelfstudie: Een toepassing op uw zelfstandige Service Fabric cluster implementeren

Zelfstandige Service Fabric-clusters bieden u de mogelijkheid om uw eigen omgeving te kiezen en een cluster te maken als onderdeel van de benadering "Elk besturingssysteem, elke cloud" die we in Service Fabric hanteren. In deze zelfstudie, een zelfstandige cluster die worden gehost op AWS maken en implementeren van een toepassing erin.

Deze zelfstudie is deel drie van een serie.  Service Fabric zelfstandige clusters bieden u de optie voor het kiezen van uw eigen omgeving en een cluster maken als onderdeel van onze aanpak voor "Elk besturingssysteem, elke cloud" met Service Fabric. Deze zelfstudie laat zien hoe u de AWS-infrastructuur kunt maken die nodig is om dit zelfstandige cluster te hosten.

In deel drie van de serie leert u het volgende:

> [!div class="checklist"]
> * De voorbeeld-app downloaden
> * In het cluster implementeren

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* [Installeer Visual Studio 2019](https://www.visualstudio.com/) en installeer de **Azure-ontwikkeling** en **ASP.NET en webontwikkeling** werkbelastingen.
* [Installeer de Service Fabric-SDK](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>De voorbeeldtoepassing om te stemmen downloaden

Voer in een opdrachtvenster de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen op de lokale computer.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

### <a name="deploy-the-app-to-the-service-fabric-cluster"></a>De app in het Service Fabric-cluster implementeren

Wanneer de toepassing is gedownload, kunt u deze rechtstreeks vanuit Visual Studio implementeren naar een cluster.

1. Open Visual Studio

2. Selecteer **Bestand** > **Openen**

3. Navigeer naar de map waarin u de git-opslagplaats hebt gekloond en selecteer Voting.sln

4. Klik met de rechtermuisknop op het toepassingsproject `Voting` in Solution Explorer en kies **Publiceren**

5. Selecteer de vervolgkeuzelijst voor het **verbindingseindpunt** en voer de openbare DNS-naam van een van de knooppunten in uw cluster in.  Bijvoorbeeld `ec2-34-215-183-77.us-west-2.compute.amazonaws.com:19000`. Houd er rekening mee dat in Azure, een volledig gekwalificeerde domeinnaam (FQDN) niet automatisch, maar eenvoudig gegeven is [worden ingesteld op de pagina overzicht van de virtuele machine.](https://docs.microsoft.com/azure/virtual-machines/linux/portal-create-fqdn)

6. Open uw voorkeursbrowser en typ het clusteradres in (het eindpunt van de verbinding, deze app wordt geïmplementeerd op poort 8080 - bijvoorbeeld ec2-34-215-183-77.us-west-2.compute.amazonaws.com:8080).

    ![API-reactie van cluster](./media/service-fabric-tutorial-standalone-cluster/deployed-app.png)

## <a name="next-steps"></a>Volgende stappen

In deel drie van de serie hebt u geleerd hoe u een toepassing in uw cluster kunt implementeren:

> [!div class="checklist"]
> * De voorbeeld-app downloaden
> * In het cluster implementeren

Ga door naar deel van de serie om uw cluster op te schonen.

> [!div class="nextstepaction"]
> [Uw resources opschonen](service-fabric-tutorial-standalone-clean-up.md)