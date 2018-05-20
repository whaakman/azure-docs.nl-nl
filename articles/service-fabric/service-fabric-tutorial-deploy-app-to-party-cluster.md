---
title: Een Azure Service Fabric-toepassing implementeren in een cluster | Microsoft Docs
description: Lees hoe u vanuit Visual Studio een toepassing implementeert naar een cluster.
services: service-fabric
documentationcenter: .net
-author: rwike77
-manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: f75a05e965a025a3041036679ac06cfe4f1ec8d7
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="tutorial-deploy-an-application-to-a-service-fabric-cluster-in-azure"></a>Zelfstudie: een toepassing implementeren naar een Service Fabric-cluster in Azure
Deze zelfstudie is deel twee van een reeks en laat zien hoe u direct vanuit Visual Studio een Azure Service Fabric-toepassing implementeert naar een nieuw cluster in Azure.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een cluster maken vanuit Visual Studio
> * Een toepassing implementeren in een extern cluster met behulp van Visual Studio


In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * [Een .NET Service Fabric-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md)
> * De toepassing implementeren in een extern cluster
> * [Een HTTPS-eindpunt toevoegen aan een front-end-service van ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [CI/CD configureren met behulp van Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="prerequisites"></a>Vereisten
Voor u met deze zelfstudie begint:
- Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installeer Visual Studio 2017](https://www.visualstudio.com/) en installeer de workloads **Azure-ontwikkeling** en **ASP.NET-ontwikkeling en webontwikkeling**.
- [Installeer de Service Fabric-SDK](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>De voorbeeldtoepassing om te stemmen downloaden
Als u in [deel één van deze zelfstudiereeks](service-fabric-tutorial-create-dotnet-app.md) niet de voorbeeldtoepassing om te stemmen hebt gemaakt, kunt u deze downloaden. Voer in een opdrachtvenster de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen op de lokale computer.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="create-a-service-fabric-cluster"></a>Een Service Fabric-cluster maken
Nu de toepassing klaar is, kunt u deze rechtstreeks vanuit Visual Studio implementeren naar een cluster. Een [Service Fabric-cluster](/service-fabric/service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd

U hebt twee opties voor implementatie binnen Visual Studio:
- Een cluster maken in Azure vanuit Visual Studio. Met deze optie kunt u een beveiligd cluster rechtstreeks vanuit Visual Studio met de configuraties van uw voorkeur maken. Dit type cluster is ideaal voor testscenario's, waar u het cluster kunt maken en er vervolgens direct naar kunt publiceren binnen Visual Studio.
- Publiceren naar een bestaand cluster in uw abonnement.  U kunt Service Fabric-clusters maken via [Azure Portal](https://portal.azure.com), met behulp van [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)- of [Azure CLI](./scripts/cli-create-cluster.md)-scripts, of vanuit een [Azure Resource Manager-sjabloon](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

In deze zelfstudie maakt u een cluster vanuit Visual Studio. Als u al een cluster hebt geïmplementeerd, kunt u uw verbindingseindpunt kopiëren en plakken, of kunt u het kiezen vanuit uw abonnement.
> [!NOTE]
> Veel services gebruiken de omgekeerde proxy om met elkaar te communiceren. Clusters die zijn gemaakt vanuit Visual Studio en clusters van derden hebben omgekeerde proxy standaard ingeschakeld.  Als u een bestaand cluster gebruikt, moet u [de omgekeerde proxy in het cluster inschakelen](service-fabric-reverseproxy.md#setup-and-configuration).

### <a name="find-the-votingweb-service-endpoint"></a>Het service-eindpunt van VotingWeb vinden
Zoek eerst het eindpunt van de front-end webservice op.  De front-end webservice luistert op een specifieke poort.  Wanneer de toepassing in een cluster in Azure wordt geïmplementeerd, worden zowel het cluster als de toepassing achter een load balancer van Azure uitgevoerd.  De poort van de toepassing moet open zijn in de load balancer van Azure zodat binnenkomend verkeer de webservice kan bereiken.  De poort (bijvoorbeeld 8080) wordt gevonden in het bestand *VotingWeb/PackageRoot/ServiceManifest.xml* in het element **Eindpunt**:

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

In de volgende stap geeft u deze poort op het tabblad **Geavanceerd** van het dialoogventer **Cluster maken** op.  Als u de toepassing in een bestaand cluster implementeert, kunt u deze poort openen in de load balancer van Azure met een [PowerShell-script](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) of in [Azure Portal](https://portal.azure.com).

### <a name="create-a-cluster-in-azure-through-visual-studio"></a>Een cluster maken in Azure via Visual Studio
Klik met de rechtermuisknop op het toepassingsproject in Solution Explorer en kies **Publiceren**.

Meld u aan met behulp van uw Azure-account, zodat u toegang hebt tot uw abonnement(en). Deze stap is optioneel als u een cluster van derden gebruikt.

Selecteer de vervolgkeuzelijst voor het **verbindingseindpunt** en selecteer de optie **<Create New Cluster...>**.
    
![Het dialoogvenster Publiceren](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)
    
Wijzig de volgende instellingen in het dialoogvenster **Cluster maken**:

1. Geef de naam van het cluster op in het veld **Clusternaam**, evenals het abonnement en de locatie die u wilt gebruiken.
2. Optioneel: u kunt het aantal knooppunten wijzigen. Standaard beschikt u over drie knooppunten, het minimale aantal dat is vereist om Service Fabric-scenario's te kunnen testen.
3. Selecteer het tabblad **Certificaat**. Typ op dit tabblad een wachtwoord dat u wilt gebruiken om het certificaat van uw cluster te beschermen. Met dit certificaat is uw cluster beter beveiligd. U kunt ook het pad wijzigen waar u het certificaat wilt opslaan. Visual Studio kan het certificaat voor u importeren, aangezien dit een vereiste stap is om de toepassing naar het cluster te kunnen publiceren.
4. Selecteer het tabblad **VM-details**. Geef het wachtwoord op dat u wilt gebruiken voor de virtuele machines (VM's) die het cluster vormen. De gebruikersnaam en het wachtwoord kunnen worden gebruikt om een externe verbinding met de virtuele machines tot stand te brengen. U moet ook een VM-machinegrootte selecteren, en u kunt indien nodig de VM-installatiekopie wijzigen.
5. Op het tabblad **Geavanceerd** kunt u de lijst met poorten wijzigen die u wilt openen in de load balancer van Azure die samen met het cluster wordt gemaakt.  Voeg het VotingWeb-service-eindpunt toe dat u hebt gedetecteerd in een vorige stap. U kunt ook een bestaande Application Insights-sleutel toevoegen om toepassingslogboekbestanden naar door te sturen.
6. Wanneer u klaar bent met het wijzigen van de instellingen, selecteert u de knop **Maken**. Het maakproces duurt maar een paar minuten; in het uitvoervenster wordt aangegeven wanneer het cluster helemaal klaar is.

![Dialoogvenster Cluster maken](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

## <a name="deploy-the-sample-application"></a>De voorbeeldtoepassing implementeren
Zodra het cluster dat u wilt gebruiken gereed is, klikt u met de rechtermuisknop op het toepassingsproject en kiest u **Publiceren**.

Nadat het publiceren is voltooid, moet u via een browser een aanvraag kunnen verzenden naar de toepassing.

Open de browser van uw voorkeur en typ het adres van het cluster (het verbindingseindpunt zonder de poortgegevens, bijvoorbeeld win1kw5649s.westus.cloudapp.azure.com).

U moet nu hetzelfde resultaat zien als bij het lokaal uitvoeren van de toepassing.

![API-reactie van cluster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een cluster maken vanuit Visual Studio
> * Een toepassing implementeren in een extern cluster met behulp van Visual Studio

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [HTTPS inschakelen](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
