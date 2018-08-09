---
title: Een Service Fabric-toepassing implementeren in een cluster in Azure | Microsoft Docs
description: Lees hoe u vanuit Visual Studio een toepassing implementeert naar een cluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: 81cd4d247ba6153fd205ead36f29a52b420bb427
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502825"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Zelfstudie: Een Service Fabric-toepassing implementeren naar een cluster in Azure

Deze zelfstudie is deel twee van een reeks en laat zien hoe u een Azure Service Fabric-toepassing implementeert in een nieuw cluster in Azure.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een cluster van derden maken.
> * Een toepassing implementeren in een extern cluster met behulp van Visual Studio.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * [Een .NET Service Fabric-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md)
> * De toepassing implementeren in een extern cluster
> * [Een HTTPS-eindpunt toevoegen aan een front-endservice van ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [CI/CD configureren met behulp van Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan.
* [Installeer Visual Studio 2017](https://www.visualstudio.com/) en installeer de workloads **Azure-ontwikkeling** en **ASP.NET-ontwikkeling en webontwikkeling**.
* [Installeer de Service Fabric-SDK](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>De voorbeeldtoepassing om te stemmen downloaden

Als u in [deel één van deze zelfstudiereeks](service-fabric-tutorial-create-dotnet-app.md) niet de voorbeeldtoepassing om te stemmen hebt gemaakt, kunt u deze downloaden. Voer in een opdrachtvenster de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen op de lokale computer.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="publish-to-a-service-fabric-cluster"></a>Een Service Fabric-cluster publiceren

Nu de toepassing klaar is, kunt u deze rechtstreeks vanuit Visual Studio implementeren naar een cluster. Een [Service Fabric-cluster](https://docs.microsoft.com/en-gb/azure/service-fabric/service-fabric-deploy-anywhere) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd.

Voor deze zelfstudie kunt u de stemtoepassing op twee manieren met behulp van Visual Studio implementeren naar een Service Fabric-cluster:

* Publiceren naar een (extern) testcluster.
* Publiceren naar een bestaand cluster in uw abonnement.  U kunt Service Fabric-clusters maken via [Azure Portal](https://portal.azure.com), met behulp van [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)- of [Azure CLI](./scripts/cli-create-cluster.md)-scripts, of vanuit een [Azure Resource Manager-sjabloon](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

> [!NOTE]
> Veel services gebruiken de omgekeerde proxy om met elkaar te communiceren. Clusters die zijn gemaakt vanuit Visual Studio en clusters van derden hebben omgekeerde proxy standaard ingeschakeld.  Als u een bestaand cluster gebruikt, moet u [de omgekeerde proxy in het cluster inschakelen](service-fabric-reverseproxy-setup.md#).


### <a name="find-the-votingweb-service-endpoint-for-your-azure-subscription"></a>Het service-eindpunt van VotingWeb vinden voor uw Azure-abonnement

Als u de stemtoepassing wilt publiceren naar uw eigen Azure-abonnement, dient u het eindpunt van de front-end-webservice op te zoeken. Als u van een cluster van derden gebruikmaakt, is het gebruik van poort 8080 door de Voting-voorbeeldtoepassing automatisch toegestaan en hoeft u deze niet te configureren in de load balancer van het externe cluster.

De front-endwebservice luistert op een specifieke poort.  Wanneer de toepassing in een cluster in Azure wordt geïmplementeerd, worden zowel het cluster als de toepassing achter een load balancer van Azure uitgevoerd.  De poort van de toepassing moet met behulp van een regel geopend zijn in de load balancer van Azure die voor dit cluster wordt gebruikt, zodat binnenkomend verkeer de webservice kan bereiken.  De poort (bijvoorbeeld 8080) wordt gevonden in het bestand *VotingWeb/PackageRoot/ServiceManifest.xml* in het element **Eindpunt**:

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Open deze poort voor uw Azure-abonnement met behulp van een taakverdelingsregel in Azure. Gebruik hiervoor een [PowerShell-script](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) of de ga naar de load balancer voor dit cluster in [Azure Portal](https://portal.azure.com).

### <a name="join-a-party-cluster"></a>Deelnemen aan een Party-cluster

> [!NOTE]
> Ga in de volgende sectie naar De toepassing implementeren met behulp van Visual Studio, indien u de toepassing wilt publiceren in uw eigen cluster binnen een Azure-abonnement.

Party-clusters zijn gratis, tijdelijke Service Fabric-clusters die worden gehost op Azure en uitgevoerd door het Service Fabric-team. Iedereen kan hier toepassingen implementeren en meer te weten komen over het platform. Het cluster gebruikt één zelfondertekend certificaat voor beveiliging van knooppunt-naar-knooppunt en client-naar-knooppunt.

Meld u aan en [neem deel aan een Windows-cluster](http://aka.ms/tryservicefabric). Download het PFX-certificaat naar uw computer door op de koppeling **PFX** te klikken. Klik op de koppeling **Hoe kan ik verbinding maken met een beveiligd Party-cluster?** en kopieer het certificaatwachtwoord. Het certificaat, het certificaatwachtwoord en de waarde van het **verbindingseindpunt** worden in volgende stappen gebruikt.

![PFX en verbindingseindpunt](./media/service-fabric-quickstart-dotnet/party-cluster-cert.png)

> [!Note]
> Er zijn per uur een beperkt aantal Party-clusters beschikbaar. Als er een fout optreedt wanneer u zich probeert aan te melden voor een cluster van derden, kunt u een bepaalde tijd wachten en het opnieuw proberen, of kunt u deze stappen in de zelfstudie [Een .NET-app implementeren](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) volgen om een Service Fabric-cluster in uw Azure-abonnement te maken en daarin de toepassing te implementeren. Als u nog geen abonnement op Azure hebt, kunt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) maken.
>

Installeer de PFX op uw Windows-computer in het certificaatarchief *CurrentUser\My*.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

Onthoud de vingerafdruk voor de volgende stap.

> [!Note]
> Standaard is de web-front-endservice geconfigureerd om te luisteren op poort 8080 naar binnenkomend verkeer. Poort 8080 is geopend in het cluster van derden.  Als u de poort van de toepassing moet wijzigen, moet u dat doen op een van de poorten die in het cluster van derden zijn geopend.
>

### <a name="publish-the-application-using-visual-studio"></a>De toepassing publiceren met Visual Studio

Nu de toepassing klaar is, kunt u deze rechtstreeks vanuit Visual Studio implementeren naar een cluster.

1. Klik met de rechtermuisknop op **Voting** in Solution Explorer en kies **Publiceren**. Het dialoogvenster Publiceren wordt weergegeven.

2. Kopieer het **verbindingseindpunt** van het cluster van derden of van uw Azure-abonnement naar het veld **Verbindingseindpunt**. Bijvoorbeeld `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Klik op **Geavanceerde verbindingsparameters** en controleer of de waarden *FindValue* en *ServerCertThumbprint* overeenkomen met de vingerafdruk van het certificaat dat in een vorige stap voor een cluster van derden is geïnstalleerd of het certificaat dat overeenkomt met uw Azure-abonnement.

    ![Het dialoogvenster Publiceren](./media/service-fabric-quickstart-dotnet/publish-app.png)

    Elke toepassing in het cluster moet een unieke naam hebben.  Clusters van derden vormen echter een openbare, gedeelde omgeving en er kan een conflict met een bestaande toepassing optreden.  Als er een naamconflict is, wijzigt u de naam van het Visual Studio-project en voert u de implementatie opnieuw uit.

3. Klik op **Publish**.

4. Open een browser en typ het adres van het cluster gevolgd door ':8080' (of het poortnummer dat u zelf hebt geconfigureerd) om bij uw stemtoepassing in het cluster te komen. Dit ziet er ongeveer zo uit: `http://zwin7fh14scd.westus.cloudapp.azure.com:8080`. U zou nu moeten kunnen zien dat de toepassing in het cluster in Azure wordt uitgevoerd. Probeer op de webpagina Voting enkele stemopties toe te voegen en te verwijderen en stem op een of meer van deze opties.

    ![Front-end van toepassing](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een cluster van derden maken.
> * Een toepassing implementeren in een extern cluster met behulp van Visual Studio.

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [HTTPS inschakelen](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
