---
title: Een Azure Service Fabric-toepassing naar een Cluster van derden implementeren | Microsoft Docs
description: Informatie over het implementeren van een toepassing naar een Cluster van derden.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: d7496b0578301713ebae7381e9a54642e226eb96
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="deploy-an-application-to-a-party-cluster-in-azure"></a>Een toepassing implementeert op een Cluster partij in Azure
Deze zelfstudie maakt deel uit van een reeks en ziet u hoe u een Azure Service Fabric-toepassing naar een Cluster partij in Azure implementeert.

In deel twee van de reeks zelfstudie leert u hoe:
> [!div class="checklist"]
> * Een toepassing implementeert op een externe-cluster met Visual Studio
> * Een toepassing verwijderen van een cluster met behulp van Service Fabric Explorer

In deze zelfstudie reeks leert u hoe:
> [!div class="checklist"]
> * [Een .NET-Service Fabric-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md)
> * De toepassing naar een externe-cluster implementeren
> * [CI/CD met behulp van Visual Studio Team Services configureren](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installeer Visual Studio 2017](https://www.visualstudio.com/) en installeer de **ontwikkelen van Azure** en **ASP.NET en web ontwikkeling** werkbelastingen.
- [De Service Fabric SDK installeren](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>De voorbeeldtoepassing Voting downloaden
Als u hebt niet de voorbeeldtoepassing Voting in [deel uitmaken van een van deze zelfstudie reeks](service-fabric-tutorial-create-dotnet-app.md), u kunt dit downloaden. Voer de volgende opdracht voor het klonen van de opslagplaats van de voorbeeld-app op uw lokale computer in een opdrachtvenster.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a>Een Cluster partij instellen
Clusters van derden zijn gratis, tijdelijke Service Fabric-clusters die worden gehost op Azure en uitgevoerd door het Service Fabric-team. Iedereen kan hier toepassingen implementeren en meer te weten komen over het platform. Gratis!

Als u toegang tot een Cluster met derden, bladert u naar deze site: http://aka.ms/tryservicefabric en volg de instructies voor toegang tot een cluster. U moet een Facebook of GitHub-account voor toegang tot een Cluster van derden.

Als u wilt, kunt u uw eigen cluster in plaats van het Cluster partij.  De omgekeerde proxy de ASP.NET core webfront-end gebruikt om te communiceren met de stateful service back-end.  Partijen Clusters en het lokaal ontwikkelcluster hebben omgekeerde proxy standaard ingeschakeld.  Als u de voorbeeldtoepassing Voting aan uw eigen cluster implementeert, moet u [inschakelen van de omgekeerde proxy in het cluster](service-fabric-reverseproxy.md#setup-and-configuration).

> [!NOTE]
> Partijen clusters zijn niet beveiligd, zodat uw toepassingen en gegevens die u in deze plaatsen mogelijk zichtbaar voor anderen. Alles wat die u niet dat anderen wilt kunnen zien niet implementeren. Zorg ervoor dat Lees onze gebruiksvoorwaarden voor de details.

## <a name="deploy-the-app-to-the-azure"></a>De app implementeren naar Azure
Nu dat de toepassing klaar is, kunt u deze aan het Cluster partij directe implementeren vanuit Visual Studio.

1. Met de rechtermuisknop op **Voting** in Solution Explorer en kiest u **publiceren**.

    ![Het dialoogvenster Publiceren](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. Typ het verbindingseindpunt van het Cluster partij in de **verbindingseindpunt** veld en klikt u op **publiceren**.

    Nadat het publiceren is voltooid, moet u mogelijk zijn een aanvraag te verzenden naar de toepassing via een browser.

3. Open de voorkeur browser en typ in het cluster-adres (het verbindingseindpunt zonder de poortinformatie - bijvoorbeeld win1kw5649s.westus.cloudapp.azure.com).

    U ziet nu hetzelfde resultaat als u hebt gezien wanneer de toepassing lokaal wordt uitgevoerd.

    ![API-reactie van Cluster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a>De toepassing verwijderen van een cluster met behulp van Service Fabric Explorer
Service Fabric Explorer is een grafische gebruikersinterface om te verkennen en beheren van toepassingen in een Service Fabric-cluster.

De toepassing verwijderen uit het Cluster partij:

1. Blader naar de Service Fabric Explorer, via de koppeling die is geleverd door de aanmeldingspagina van derden Cluster. Bijvoorbeeld: http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. Navigeer in Service Fabric Explorer naar de **fabric://Voting** knooppunt in de treeview aan de linkerkant.

3. Klik op de **actie** knop in de rechterkolom **Essentials** deelvenster en kies **toepassing verwijderen**. Bevestiging van het verwijderen van het toepassingsexemplaar waarmee het exemplaar van onze toepassing die wordt uitgevoerd in het cluster wordt verwijderd.

![Verwijderen van de toepassing in Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-the-application-type-from-a-cluster-using-service-fabric-explorer"></a>Het toepassingstype verwijderen uit een cluster met behulp van Service Fabric Explorer
Toepassingen geïmplementeerd als de soorten toepassingen in een Service Fabric-cluster, waarmee u kunt meerdere exemplaren en versies van de toepassing uitgevoerd binnen het cluster. Nadat de verwijderd van het exemplaar van onze toepassing kunnen we ook het type, voor het voltooien van het opruimen van de implementatie verwijderd.

Zie voor meer informatie over het toepassingsmodel in Service Fabric [Model van een toepassing in Service Fabric](service-fabric-application-model.md).

1. Navigeer naar de **VotingType** knooppunt in de treeview.

2. Klik op de **actie** knop in de rechterkolom **Essentials** deelvenster en kies **Type verwijderen**. Bevestig het toepassingstype hierbij.

![Inrichting toepassingstype in Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Hiermee is de zelfstudie.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een toepassing implementeert op een externe-cluster met Visual Studio
> * Een toepassing verwijderen van een cluster met behulp van Service Fabric Explorer

Ga naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [Stel doorlopende integratie met behulp van Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)