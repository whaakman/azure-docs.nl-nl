---
title: Een Azure Service Fabric-containertoepassing maken in Linux | Microsoft Docs
description: Maak uw eerste Linux-containertoepassing in Azure Service Fabric.  Bouw een Docker-installatiekopie met uw toepassing, push de installatiekopie naar een containerregister, en bouw en implementeer een Service Fabric-containertoepassing.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.openlocfilehash: 42a2542b7564f1692146b3e3927792ac339f30be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Een Linux-containertoepassing voor Azure Service Fabric implementeren in Azure
Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u schaalbare en betrouwbare microservices en containers implementeert en beheert. 

In deze Quick Start leert u hoe u Linux-containers kunt implementeren in een Service Fabric-cluster. Nadat dit is voltooid, beschikt u over een stemtoepassing die bestaat uit een Python-web-front-end en een Redis-back-end. Beide worden uitgevoerd in een Service Fabric-cluster. 

![quickstartpic][quickstartpic]

In deze snelstartgids leert u de volgende zaken:
> [!div class="checklist"]
> * Linux-containers implementeren op Service Fabric
> * Containers schalen en er failovers voor uitvoeren in Service Fabric

## <a name="prerequisite"></a>Vereiste
Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/en-us/free/) aan voordat u begint.
  
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de opdrachtregelinterface (CLI)lokaal te installeren en te gebruiken, zorgt u ervoor dat versie Azure CLI 2.0.4 of later wordt uitgevoerd. Voer az --version uit om de versie te zoeken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="get-application-package"></a>Een toepassingspakket ophalen
Als u containers wilt implementeren op Service Fabric, hebt u een set manifestbestanden nodig (de toepassingsdefinitie), waarin de afzonderlijke containers en de toepassing worden beschreven.

Gebruik git in Cloud Shell om een kopie van de toepassingsdefinitie te maken.

```azurecli-interactive
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

cd service-fabric-dotnet-containers/Linux/container-tutorial/Voting
```

## <a name="deploy-the-containers-to-a-service-fabric-cluster-in-azure"></a>De containers implementeren in een Service Fabric-cluster in Azure
Gebruik uw eigen cluster of een cluster van derden om de toepassing te implementeren in een cluster in Azure.

Clusters van derden zijn gratis tijdelijke Service Fabric-clusters die worden gehost in Azure. Ze worden onderhouden door het Service Fabric-team. Iedereen kan hier toepassingen implementeren en informatie krijgen over het platform. [Volg de instructies](http://aka.ms/tryservicefabric) om toegang te krijgen tot een cluster van derden. 

Zie voor meer informatie over het maken van uw eigen cluster [Uw eerste Service Fabric-cluster maken op Azure](service-fabric-get-started-azure-cluster.md).

> [!Note]
> De web-front-endservice is geconfigureerd om te luisteren op poort 80 naar binnenkomend verkeer. Zorg ervoor dat de poort is geopend in het cluster. Als u een cluster van derden gebruikt, is deze poort geopend.
>

### <a name="deploy-the-application-manifests"></a>De toepassingsmanifesten implementeren 
De [SFCTL (Service Fabric-CLI)](service-fabric-cli.md) installeren in de CLI-omgeving

```azurecli-interactive
pip3 install --user sfctl 
export PATH=$PATH:~/.local/bin
```
Maak verbinding met het Service Fabric-cluster in Azure met behulp van Azure CLI. Het eindpunt is het beheereindpunt van het cluster - bijvoorbeeld: `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.

```azurecli-interactive
sfctl cluster select --endpoint http://linh1x87d1d.westus.cloudapp.azure.com:19080
```

Gebruik het opgegeven installatiescript om de stemtoepassingsdefinitie te kopiëren naar het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

```azurecli-interactive
./install.sh
```

Open een browser en ga naar Service Fabric Explorer op http://\<my-azure-service-fabric-cluster-url>:19080/Explorer - bijvoorbeeld: `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`. Als u het toepassingsknooppunt uitvouwt, ziet u nu een vermelding voor het type stemtoepassing en het exemplaar dat u hebt gemaakt.

![Service Fabric Explorer][sfx]

Maak verbinding met de actieve container.  Open een webbrowser die verwijst naar de URL van het cluster - bijvoorbeeld: `http://linh1x87d1d.westus.cloudapp.azure.com:80`. U ziet nu de stemtoepassing in de browser.

![quickstartpic][quickstartpic]

## <a name="fail-over-a-container-in-a-cluster"></a>Failover uitvoeren voor een container in een cluster
Service Fabric zorgt ervoor dat containerexemplaren automatisch worden verplaatst naar andere knooppunten in het cluster, mocht er een fout optreden. U kunt een knooppunt voor containers ook handmatig leegmaken en vervolgens probleemloos verplaatsen naar andere knooppunten in het cluster. Er zijn meerdere manieren om services te schalen. In dit voorbeeld gebruiken we Service Fabric Explorer.

Doe het volgende om een failover uit te voeren voor de front-endcontainer:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Klik op het knooppunt **fabric:/Voting/azurevotefront** in de structuurweergave en vouw het partitieknooppunt uit (vertegenwoordigd door een GUID). Let op: de naam van het knooppunt in de structuurweergave laat zien op welke knooppunten de container momenteel wordt uitgevoerd, bijvoorbeeld: `_nodetype_4`
3. Vouw het knooppunt **Knooppunten** in de structuurweergave uit. Klik op het weglatingsteken (drie punten) naast het knooppunt waarop de container wordt uitgevoerd.
4. Kies **Opnieuw starten** om dit knooppunt opnieuw te starten en bevestig deze actie. Door het opnieuw starten wordt voor de container een failover uitgevoerd naar een ander knooppunt in het cluster.

![sfxquickstartshownodetype][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Toepassingen en services voor schalen in een cluster
Service Fabric-services kunnen eenvoudig worden geschaald in een cluster om plaats te bieden aan de belasting voor de services. U schaalt een service door het aantal exemplaren te wijzigen dat wordt uitgevoerd in het cluster.

Voer de volgende stappen uit om de web-front-endservice te schalen:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Klik op het weglatingsteken (drie punten) naast het knooppunt **fabric:/Voting/azurevotefront** in de structuurweergave en kies **Service schalen**.

    ![containersquickstartscale][containersquickstartscale]

  U kunt er nu voor kiezen om het aantal exemplaren van de web-front-endservice te schalen.

3. Wijzig het aantal in **2** en klik op **Service schalen**.
4. Klik op het knooppunt **fabric:/Voting/azurevotefront** in de structuurweergave en vouw het partitieknooppunt uit (vertegenwoordigd door een GUID).

    ![containersquickstartscaledone][containersquickstartscaledone]

    U ziet nu dat de service twee exemplaren heeft. In de structuurweergave ziet u op welke knooppunten de exemplaren worden uitgevoerd.

Met deze eenvoudige beheertaak is het aantal beschikbare resources voor het verwerken van gebruikersbelasting voor onze front-endservice verdubbeld. Het is belangrijk te weten dat u niet meerdere exemplaren van een service nodig hebt om ervoor te zorgen dat deze op betrouwbare wijze wordt uitgevoerd. Als de service mislukt, wordt in Service Fabric een nieuw exemplaar van de service uitgevoerd in het cluster.

## <a name="clean-up"></a>Opruimen
Gebruik het uninstall-script dat is opgegeven in de sjabloon om het toepassingsexemplaar te verwijderen uit het cluster en de registratie van het toepassingstype op te heffen. Het duurt enige tijd om het exemplaar te wissen met deze opdracht en de opdracht install‘sh mag niet onmiddellijk na dit script worden uitgevoerd. 

```bash
./uninstall.sh
```

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u de volgende zaken geleerd:
> [!div class="checklist"]
> * Een Linux-containertoepassing implementeren in Azure
> * Failover uitvoeren voor een container in een Service Fabric-cluster
> * Een container in een Service Fabric-cluster schalen

* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-containers-overview.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
* Bekijk de [codevoorbeelden voor Service Fabric-containers](https://github.com/Azure-Samples/service-fabric-dotnet-containers) op GitHub.

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
