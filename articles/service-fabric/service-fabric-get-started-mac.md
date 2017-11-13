---
title: Uw ontwikkelomgeving in Mac OS X instellen voor gebruik met Azure Service Fabric | Microsoft Docs
description: Installeer de runtime, SDK en hulpprogramma's en maak een lokaal ontwikkelcluster. Zodra u dit hebt gedaan, kunt u toepassingen bouwen in Mac OS X.
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: saysa
ms.openlocfilehash: cf67c377cd5c17f7b540fb23af48a333a9cddf69
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Uw ontwikkelomgeving instellen in Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

U kunt Service Fabric-toepassingen bouwen voor uitvoering op Linux-clusters met behulp van Mac OS X. In dit artikel wordt uitgelegd hoe u uw Mac kunt instellen voor ontwikkeling.

## <a name="prerequisites"></a>Vereisten
Service Fabric wordt niet systeemeigen op OS X uitgevoerd. Als u een lokaal Service Fabric-cluster wilt uitvoeren, bieden we een vooraf geconfigureerde Docker-containerinstallatiekopie. Voordat u aan de slag gaat, hebt u het volgende nodig:

* Ten minste 4 GB RAM-geheugen
* Nieuwste versie van [Docker](https://www.docker.com/)
* Toegang tot Service Fabric One-box Docker-[containerinstallatiekopie](https://hub.docker.com/r/servicefabricoss/service-fabric-onebox/)

>[!TIP]
> * Volg de stappen in de officiële Docker-[documentatie](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install) om Docker te installeren op uw Mac. 
> * Wanneer u klaar bent met de installatie, controleert u of de installatie goed is verlopen door [deze stappen](https://docs.docker.com/docker-for-mac/#check-versions-of-docker-engine-compose-and-machine) uit te voeren


## <a name="create-a-local-container-and-setup-service-fabric"></a>Een lokale container maken en Service Fabric configureren
Als u een lokale Docker-container wilt instellen en daarop een Service Fabric-cluster wilt uitvoeren, voert u de volgende stappen uit:

1. Haal de installatiekopie op uit de opslagplaats op de Docker-hub:

    ```bash
    docker pull servicefabricoss/service-fabric-onebox
    ```

2. Start een instantie van een Service Fabric-One-box-container met de installatiekopie:

    ```bash
    docker run -itd -p 19080:19080 --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    >[!TIP]
    >Door een naam op te geven voor uw instantie van de container, kunt u deze op een beter leesbare manier verwerken. 

3. Meld u aan bij de Docker-container in interactieve ssh-modus:

    ```bash
    docker exec -it sfonebox bash
    ```

4. Voer het installatiescript uit om de vereiste afhankelijkheden op te halen en start daarna het cluster op de container.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

5. Nadat stap 4 is voltooid, gaat u op uw Mac naar ``http://localhoist:19080`` en zou u de verkenner van Service Fabric moeten zien.


## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>De Service Fabric-CLI (sfctl) instellen op een Mac

Volg de instructies in [Service Fabric-CLI](service-fabric-cli.md#cli-mac) als u de Service Fabric-CLI (`sfctl`) wilt installeren op een Mac.
De CLI-opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters, toepassingen en services.

## <a name="create-application-on-your-mac-using-yeoman"></a>Toepassingen maken op een Mac met Yeoman

Service Fabric biedt hulpprogramma's waarmee u vanuit de terminal een Service Fabric-toepassing kunt maken met behulp van de Yeoman-sjabloongenerator. Volg de stappen hieronder om te controleren of de Yeoman-sjabloongenerator van Service Fabric werkt op uw computer.

1. Node.js en NPM moeten zijn geïnstalleerd op uw Mac. Als dat niet het geval is, kunt u Node.js en NPM als volgt installeren met behulp van Homebrew. Gebruik de optie ``-v`` om te controleren welke versies van Node.js en NPM zijn geïnstalleerd op uw Mac.

    ```bash
    brew install node
    node -v
    npm -v
    ```
2. Installeer de [Yeoman](http://yeoman.io/)-sjabloongenerator op uw computer vanuit NPM.

    ```bash
    npm install -g yo
    ```
3. Installeer de Yeoman generator die u wilt gebruiken. Volg hiervoor de stappen in deze [documentatie](service-fabric-get-started-linux.md). Volg deze stappen om Service Fabric-toepassingen te maken met behulp van Yeoman:

    ```bash
    npm install -g generator-azuresfjava       # for Service Fabric Java Applications
    npm install -g generator-azuresfguest      # for Service Fabric Guest executables
    npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
    ```
4. Als u een Service Fabric Java-toepassing wilt maken op een Mac, moeten JDK 1.8 en Gradle op de hostcomputer zijn geïnstalleerd. Als dat niet het geval is, kunt u ze installeren met behulp van [HomeBrew](https://brew.sh/). 

    ```bash
    brew update
    brew cask install java
    brew install gradle
    ```

## <a name="deploy-application-on-your-mac-from-terminal"></a>Toepassingen implementeren op uw Mac vanuit terminal

Nadat u de Service Fabric-toepassing hebt gemaakt en gebouwd, kunt u de toepassing als volgt implementeren met [Service Fabric CLI](service-fabric-cli.md#cli-mac):

1. Maak verbinding met het Service Fabric-cluster dat wordt uitgevoerd binnen de containerinstantie op uw Mac.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Ga naar de directory van uw project en voer het installatiescript uit.

    ```bash
    cd MyProject
    bash install.sh
    ```

## <a name="set-up-net-core-20-development"></a>.NET Core 2.0-ontwikkeling instellen

Installeer de [.NET Core 2.0 SDK voor Mac](https://www.microsoft.com/net/core#macos) om [C# Service Fabric-toepassingen te maken](service-fabric-create-your-first-linux-application-with-csharp.md). Pakketten voor .NET Core 2.0 Service Fabric-toepassingen worden gehost op NuGet.org, momenteel in preview.

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-on-your-mac"></a>De Service Fabric-invoegtoepassing installeren voor Eclipse Neon op uw Mac

Service Fabric biedt een invoegtoepassing voor de **Eclipse Neon voor Java IDE** die het maken, bouwen en implementeren van Java-services kan vereenvoudigen. U kunt de installatiestappen volgen uit deze algemene [documentatie](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) over het installeren en bijwerken van de Service Fabric Eclipse-invoegtoepassing naar de nieuwste versie.

Alle andere stappen die worden genoemd in de [Service Fabric-documentatie voor Eclipse](service-fabric-get-started-eclipse.md) voor het maken van een toepassing, het toevoegen van een service aan de toepassing, het installeren/verwijderen van de toepassing, enzovoort, zijn ook hier van toepassing.

Om de Service Fabric Eclipse-invoegtoepassing werkend te krijgen met de Docker-container op uw Mac, moet u naast de bovenstaande stappen een instantie van de container maken met een pad dat wordt gedeeld met de host. Dit kan als volgt:
```bash
docker run -itd -p 19080:19080 -v /Users/sayantan/work/workspaces/mySFWorkspace:/tmp/mySFWorkspace --name sfonebox servicefabricoss/service-fabric-onebox
```
waarbij ``/Users/sayantan/work/workspaces/mySFWorkspace`` het volledig gekwalificeerde pad van de werkruimte op de Mac is en ``/tmp/mySFWorkspace`` het pad binnen de container, waaraan het wordt toegewezen.

> [!NOTE]
>1. Als de naam of het pad van uw werkruimte anders is, moet u in de opdracht ``docker run`` hierboven deze namen gebruiken.
>2. Als u de container met een andere naam start dan ``sfonebox``, moet u de naam bijwerken in het bestand ``testclient.sh`` in de Java-toepassing van uw Service Fabric-actor.

## <a name="next-steps"></a>Volgende stappen
<!-- Links -->
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Een Service Fabric-cluster maken in Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Een Service Fabric-cluster maken met behulp van de Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Inzicht krijgen in het Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [De Service Fabric-CLI gebruiken voor het beheren van uw toepassingen](service-fabric-application-lifecycle-sfctl.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
