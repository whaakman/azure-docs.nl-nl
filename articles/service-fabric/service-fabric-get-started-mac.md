---
title: Uw ontwikkelomgeving in Mac OS X instellen voor gebruik met Azure Service Fabric | Microsoft Docs
description: Installeer de runtime, SDK en hulpprogramma's en maak een lokaal ontwikkelcluster. Zodra u dit hebt gedaan, kunt u toepassingen bouwen in Mac OS X.
services: service-fabric
documentationcenter: linux
author: suhuruli
manager: chackdan
editor: ''
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: linux
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/17/2017
ms.author: suhuruli
ms.openlocfilehash: 84d1f52b5fb8f18d3578bad28930f74534b1409f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60947586"
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Uw ontwikkelomgeving instellen in Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

U kunt Azure Service Fabric-toepassingen bouwen voor uitvoering in Linux-clusters met behulp van Mac OS X. In dit document wordt uitgelegd hoe u uw Mac kunt instellen voor ontwikkeling.

## <a name="prerequisites"></a>Vereisten
Azure Service Fabric wordt niet systeemeigen op OS X uitgevoerd. Als u een lokaal Service Fabric-cluster wilt uitvoeren, wordt er een vooraf geconfigureerde Docker-containerinstallatiekopie aangeboden. Voordat u aan de slag gaat, hebt u het volgende nodig:

* Ten minste 4 GB RAM-geheugen.
* Nieuwste versie van [Docker](https://www.docker.com/).

>[!TIP]
>
>Volg de stappen in de officiële [Docker-documentatie](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install) om Docker te installeren op uw Mac. Na de installatie dient u [uw installatie te controleren](https://docs.docker.com/docker-for-mac/#check-versions-of-docker-engine-compose-and-machine).
>

## <a name="create-a-local-container-and-set-up-service-fabric"></a>Een lokale container maken en Service Fabric configureren
Als u een lokale Docker-container wilt instellen en daarop een Service Fabric-cluster wilt uitvoeren, voert u de volgende stappen uit:

1. Werk de configuratie van de Docker-daemon op uw host bij met de volgende instellingen en start de Docker-daemon opnieuw op: 

    ```json
    {
        "ipv6": true,
        "fixed-cidr-v6": "fd00::/64"
    }
    ```
    U kunt deze instellingen rechtstreeks in het bestand daemon.json in uw Docker-installatiepad bijwerken. U kunt de daemon-configuratie-instellingen in Docker rechtstreeks wijzigen. Selecteer de **Docker-pictogram**, en selecteer vervolgens **voorkeuren** > **Daemon** > **Geavanceerd**.
    
    >[!NOTE]
    >
    >Wijzigen van de rechtstreeks in Docker-daemon is aanbevolen omdat de locatie van het bestand daemon.json per machine verschillen variëren kan. Bijvoorbeeld, ~/Library/Containers/com.docker.docker/Data/database/com.docker.driver.amd64-linux/etc/docker/daemon.json.
    >

    >[!TIP]
    >U wordt aangeraden tijdens het testen van grote toepassingen de resources te verhogen die aan Docker zijn toegewezen. U kunt dit doen door het **Docker-pictogram** te selecteren en vervolgens **Geavanceerd** om het aantal kernen en het geheugen aan te passen.

2. Maak in een nieuwe map een bestand met de naam `Dockerfile` om uw Service Fabric Image te maken:

    ```Dockerfile
    FROM microsoft/service-fabric-onebox
    WORKDIR /home/ClusterDeployer
    RUN ./setup.sh
    #Generate the local
    RUN locale-gen en_US.UTF-8
    #Set environment variables
    ENV LANG=en_US.UTF-8
    ENV LANGUAGE=en_US:en
    ENV LC_ALL=en_US.UTF-8
    EXPOSE 19080 19000 80 443
    #Start SSH before running the cluster
    CMD /etc/init.d/ssh start && ./run.sh
    ```

    >[!NOTE]
    >U kunt het bestand aanpassen zodat aanvullende programma's of afhankelijkheden aan uw container kunnen worden toegevoegd.
    >Bijvoorbeeld: het toevoegen van `RUN apt-get install nodejs -y` biedt ondersteuning voor `nodejs`-toepassingen als uitvoerbare gastbestanden.
    
    >[!TIP]
    > Standaard wordt hierdoor de installatiekopie met de nieuwste versie van Service Fabric opgehaald. Ga naar de [Docker Hub](https://hub.docker.com/r/microsoft/service-fabric-onebox/)-pagina als u een bepaalde revisie wilt ophalen

3. Als u uw herbruikbare installatiekopie wilt bouwen vanaf `Dockerfile`, opent u een terminal en gaat u met de opdracht `cd` naar de map met uw `Dockerfile`. Voer vervolgende de volgende opdracht uit:

    ```bash 
    docker build -t mysfcluster .
    ```
    
    >[!NOTE]
    >Deze bewerking kan enige tijd duren, maar hoeft slechts eenmaal te worden uitgevoerd.

4. Nu kunt u snel een lokale kopie van Service Fabric starten zodra u deze nodig hebt. Voer hiertoe de volgende opdracht uit:

    ```bash 
    docker run --name sftestcluster -d -v /var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mysfcluster
    ```

    >[!TIP]
    >Door een naam op te geven voor uw instantie van de container, kunt u deze op een beter leesbare manier verwerken. 
    >
    >Als uw toepassing op bepaalde poorten luistert, moeten de poorten worden opgegeven met behulp van aanvullende `-p` labels. Bijvoorbeeld, als uw toepassing op poort 8080 luistert, voeg dan de volgende `-p` tag toe:
    >
    >`docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox microsoft/service-fabric-onebox`
    >

5. Het cluster duurt even om te starten. Wanneer deze wordt uitgevoerd, u kunt logboeken bekijken via de volgende opdracht of naar het dashboard gaan om de status van het cluster weer te geven [ http://localhost:19080 ](http://localhost:19080):

    ```bash 
    docker logs sftestcluster
    ```



6. Om te stoppen en opschonen de container, gebruikt u de volgende opdracht. Echter, we gebruiken deze container in de volgende stap.

    ```bash 
    docker rm -f sftestcluster
    ```

### <a name="known-limitations"></a>Bekende beperkingen 
 
 Hier volgen bekende beperkingen van het uitvoeren van het lokale cluster in een container voor Mac-computers: 
 
 * DNS-service kan niet worden uitgevoerd en wordt niet ondersteund [Probleem #132](https://github.com/Microsoft/service-fabric/issues/132)

## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>De Service Fabric-CLI (sfctl) instellen op een Mac

Volg de instructies in [Service Fabric-CLI](service-fabric-cli.md#cli-mac) als u de Service Fabric-CLI (`sfctl`) wilt installeren op een Mac.
De CLI-opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters, toepassingen en services.

1. Als u verbinding wilt maken met het cluster voordat u toepassingen wilt implementeren, voert u de onderstaande opdracht uit. 

```bash
sfctl cluster select --endpoint http://localhost:19080
```

## <a name="create-your-application-on-your-mac-by-using-yeoman"></a>Een toepassingen maken op een Mac met Yeoman

Service Fabric biedt hulpprogramma's waarmee u vanuit de terminal een Service Fabric-toepassing kunt maken met behulp van de Yeoman-sjabloongenerator. Volg de volgende stappen om te controleren of de Yeoman-sjabloongenerator van Service Fabric werkt op uw computer:

1. Node.js en knooppunt Package Manager (NPM) moeten worden geïnstalleerd op uw Mac. De software kan worden geïnstalleerd met behulp van [HomeBrew](https://brew.sh/), als volgt:

    ```bash
    brew install node
    node -v
    npm -v
    ```
2. Installeer de [Yeoman](https://yeoman.io/)-sjabloongenerator op uw computer vanuit NPM:

    ```bash
    npm install -g yo
    ```
3. Installeer de Yeoman generator die u wilt gebruiken. Volg hiervoor de stappen in deze [startdocumentatie](service-fabric-get-started-linux.md#set-up-yeoman-generators-for-containers-and-guest-executables). Volg deze stappen om Service Fabric-toepassingen te maken met behulp van Yeoman:

    ```bash
    npm install -g generator-azuresfjava       # for Service Fabric Java Applications
    npm install -g generator-azuresfguest      # for Service Fabric Guest executables
    npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
    ```
4. Nadat u de generatoren hebt geïnstalleerd, kunt u uitvoerbare gastbestanden of containerservices maken door respectievelijk `yo azuresfguest` of `yo azuresfcontainer` uit te voeren.

5. Als u een Service Fabric Java-toepassing wilt maken op een Mac, moeten JDK 1.8 en Gradle op de hostcomputer zijn geïnstalleerd. De software kan worden geïnstalleerd met behulp van [HomeBrew](https://brew.sh/), als volgt: 

    ```bash
    brew update
    brew cask install java
    brew install gradle
    ```

    >[!TIP]
    > Moet u controleren of dat u hebt de juiste versie van de JDK is geïnstalleerd. 

## <a name="deploy-your-application-on-your-mac-from-the-terminal"></a>Toepassingen implementeren op uw Mac vanuit de terminal

Nadat u de Service Fabric-toepassing hebt gemaakt en gebouwd, kunt u de toepassing als volgt implementeren met [Service Fabric CLI](service-fabric-cli.md#cli-mac):

1. Maak verbinding met het Service Fabric-cluster dat wordt uitgevoerd binnen de containerinstantie op uw Mac:

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Ga naar de directory van uw project en voer het installatiescript uit:

    ```bash
    cd MyProject
    bash install.sh
    ```

## <a name="set-up-net-core-20-development"></a>.NET Core 2.0-ontwikkeling instellen

Installeer de [.NET Core 2.0 SDK voor Mac](https://www.microsoft.com/net/core#macos) om [C# Service Fabric-toepassingen te maken](service-fabric-create-your-first-linux-application-with-csharp.md). Pakketten voor .NET Core 2.0 Service Fabric-toepassingen worden gehost op NuGet.org, momenteel in preview.

## <a name="install-the-service-fabric-plug-in-for-eclipse-on-your-mac"></a>De Service Fabric-invoegtoepassing installeren voor Eclipse op uw Mac

Azure Service Fabric biedt een invoegtoepassing voor Eclipse Neon (of later) voor de Java IDE. De invoegtoepassing vereenvoudigt het proces voor het maken en implementeren van Java-services. Als u de Service Fabric-invoegtoepassing voor Eclipse wilt installeren of bijwerken naar de nieuwste versie, volg [deze stappen](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse). De andere stappen in de [Service Fabric voor Eclipse documentatie](service-fabric-get-started-eclipse.md) zijn ook van toepassing: maken van een toepassing, een service toevoegen aan een toepassing, een toepassing verwijderen, enzovoort.

De laatste stap is het instantiëren van de container met een pad dat wordt gedeeld met de host. De invoegtoepassing vereist dit soort instantiëring om te werken met de Docker-container op uw Mac. Bijvoorbeeld:

```bash
docker run -itd -p 19080:19080 -v /Users/sayantan/work/workspaces/mySFWorkspace:/tmp/mySFWorkspace --name sfonebox microsoft/service-fabric-onebox
```

De kenmerken zijn als volgt gedefinieerd:
* `/Users/sayantan/work/workspaces/mySFWorkspace`is het volledig gekwalificeerde pad van de werkruimte op uw Mac.
* `/tmp/mySFWorkspace`is het pad in de container waarin de werkruimte moet worden toegewezen.

>[!NOTE]
> 
>Als u een andere naam-/ padgedeelte voor uw werkruimte hebt, pas deze waarden dan aan in de `docker run` opdracht.
> 
>Als u de container met een andere naam start dan `sfonebox`, werkt u de naam bij in het bestand testclient.sh in de Java-toepassing van uw Service Fabric-actor.
>

## <a name="next-steps"></a>Volgende stappen
<!-- Links -->
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing op Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Een Service Fabric-cluster maken in Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Een Service Fabric-cluster maken met behulp van de Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Inzicht krijgen in het Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [De Service Fabric-CLI gebruiken voor het beheren van uw toepassingen](service-fabric-application-lifecycle-sfctl.md)
* [Een Linux-ontwikkelomgeving voorbereiden in Windows](service-fabric-local-linux-cluster-windows.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
