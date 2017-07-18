---
title: Uw ontwikkelomgeving instellen in Linux | Microsoft Docs
description: Installeer de runtime en SDK en maak een lokaal ontwikkelcluster in Linux. Zodra u dit hebt gedaan, kunt u toepassingen bouwen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: da6a8b4824d7215eb1db131680856ac04003f5aa
ms.contentlocale: nl-nl
ms.lasthandoff: 07/13/2017

---
# <a name="prepare-your-development-environment-on-linux"></a>Uw ontwikkelomgeving voorbereiden in Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Als u [Azure Service Fabric-toepassingen](service-fabric-application-model.md) op uw Linux-ontwikkelmachine wilt implementeren en uitvoeren, moet u de runtime en algemene SDK installeren. U kunt ook optionele SDK's voor Java en .NET Core installeren.

## <a name="prerequisites"></a>Vereisten

De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="update-your-apt-sources"></a>Uw APT-bronnen bijwerken
Voor het installeren van de SDK en het bijbehorende runtimepakket via het apt-get-opdrachtregelprogramma, moet u eerst uw APT-bronnen (Advanced Packaging Tool) bijwerken.

1. Open een terminal.
2. Voeg de Service Fabric-opslagplaats toe aan uw lijst met bronnen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Voeg de `dotnet`-opslagplaats toe aan uw lijst met bronnen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Voeg de nieuwe Gnu Privacy Guard-code (GnuPG of GPG) toe aan uw APT-sleutelring.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Voeg de officiële GPG-sleutel van Docker toe aan uw APT-sleutelring.

    ```bash
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Stel de Docker-opslagplaats in.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Vernieuw uw pakketlijsten op basis van de net toegevoegde opslagplaatsen.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk-for-containers-and-guest-executables"></a>De SDK voor containers en uitvoerbare gastbestanden installeren en instellen

Wanneer u uw bronnen hebt bijgewerkt, kunt u de SDK installeren.

1. Installeer het Service Fabric SDK-pakket, bevestig de installatie en ga akkoord met de gebruiksrechtovereenkomst.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

    >   [!TIP]
    >   Met de volgende opdrachten kunt u het accepteren van de licentie voor Service Fabric-pakketten automatiseren:
    >   ```bash
    >   echo "servicefabric servicefabric/accepted-eula-v1 select true" | debconf-set-selections
    >   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | debconf-set-selections
    >   ```
    
2. Voer het SDK-installatiescript uit.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Wanneer u het algemene SDK-pakket hebt geïnstalleerd, moet u apps kunnen maken met uitvoerbare gastbestanden of containerservices door `yo azuresfguest` of `yo azuresfcontainer` uit te voeren. Mogelijk moet u de omgevingsvariabele $NODE_PATH instellen op de locatie van de knooppuntmodules. 


```bash
    export NODE_PATH=$NODE_PATH:$HOME/.node/lib/node_modules 
```

Als u de omgeving als hoofdmap gebruikt, moet u de variabele mogelijk instellen met de volgende opdracht:

```bash
    export NODE_PATH=$NODE_PATH:/root/.node/lib/node_modules 
```


> [!TIP]
> U kunt deze opdrachten toevoegen aan het bestand ~/.bashrc, zodat u de omgevingsvariabele niet bij elke aanmelding hoeft in te stellen.
>

## <a name="set-up-the-xplat-service-fabric-cli"></a>XPlat Service Fabric CLI instellen
[XPlat CLI][azure-xplat-cli-github] bevat opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters en toepassingen. Deze is gebaseerd op Node.js en daarom [moet u controleren of u Node hebt geïnstalleerd][install-node] voordat u doorgaat met de volgende instructies:

1. Kloon de GitHub-opslagplaats naar uw ontwikkelmachine.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Schakel over naar de gekloonde opslagplaats en installeer de CLI-afhankelijkheden met behulp van de NPM (Node Package Manager).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Maak een symlink van de `bin/azure`-map van de gekloonde opslagplaats naar `/usr/bin/azure`.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Schakel ten slotte automatische aanvulling voor Service Fabric-opdrachten in.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

### <a name="set-up-azure-cli-20"></a>Azure CLI 2.0 instellen

Als alternatief voor XPlat CLI is er nu een Service Fabric-opdrachtmodule opgenomen in Azure CLI.

Zie [Aan de slag met Service Fabric en Azure CLI 2.0](service-fabric-azure-cli-2-0.md) voor meer informatie over het installeren van Azure CLI 2.0 en het gebruik van de Service Fabric-opdrachten.

## <a name="set-up-a-local-cluster"></a>Een lokaal cluster instellen
Als de installatie is voltooid, moet u een lokaal cluster kunnen starten.

1. Voer het installatiescript van het cluster uit.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Open een webbrowser en ga naar [Service Fabric Explorer](http://localhost:19080/Explorer). Als het cluster is gestart, ziet u het Service Fabric Explorer-dashboard.

    ![Service Fabric Explorer in Linux][sfx-linux]

Op dit punt kunt u vooraf samengestelde Service Fabric-toepassingspakketten of nieuwe implementeren op basis van gastcontainers of uitvoerbare gastbestanden. Als u nieuwe services wilt maken met behulp van de Java of .NET Core SDK's, voert u de optionele installatiestappen uit die in de volgende secties worden uitgelegd.


> [!NOTE]
> Zelfstandige clusters worden niet ondersteund in Linux. De preview ondersteunt slechts één selectievakje en meerdere Azure Linux-machineclusters.
>

## <a name="install-the-java-sdk-optional-if-you-want-to-use-the-java-programming-models"></a>De Java-SDK installeren (alleen als u de Java-programmeermodellen wilt gebruiken)
De Java-SDK biedt de bibliotheken en sjablonen die vereist zijn voor het bouwen van Service Fabric-services met behulp van Java.

1. Installeer het Java-SDK-pakket.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Voer het SDK-installatiescript uit.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a>De Eclipse Neon-invoegtoepassing installeren (optioneel)

U kunt de Eclipse-invoegtoepassing voor Service Fabric installeren vanuit de **Eclipse IDE voor Java-ontwikkelaars**. Met Eclipse kunt u behalve Service Fabric Java-toepassingen ook uitvoerbare Fabric Service-gasttoepassingen en containertoepassingen maken.

> [!NOTE]
> De Java-SDK is een vereiste voor het gebruik van de Eclipse-invoegtoepassing, zelfs als u deze alleen gebruikt voor uitvoerbare gastbestanden en containertoepassingen.
>

1. Zorg er in Eclipse voor dat u de meest recente Eclipse Neon en de meest recente Buildship-versie (1.0.17 of hoger) hebt geïnstalleerd. U kunt de versies van geïnstalleerde onderdelen controleren door **Help** > **Installation Details** te selecteren. U kunt Buildship bijwerken met behulp van de instructies in [Eclipse Buildship: Eclipse-invoegtoepassingen voor Gradle][buildship-update].

2. Als u de Service Fabric-invoegtoepassing wilt installeren, selecteert u **Help** > **Install New Software**.

3. Geef in het vak **Work with** **http://dl.microsoft.com/eclipse** op.

4. Klik op **Add**.

    ![De pagina met beschikbare software][sf-eclipse-plugin]

5. Selecteer de **Fabric Service**-invoegtoepassing en klik op **Next**.

6. Voer de installatiestappen uit en accepteer de gebruiksrechtovereenkomst.

Als u de Eclipse-invoegtoepassing voor Service Fabric al hebt geïnstalleerd, controleert u of u de meest recente versie gebruikt. U kunt dit controleren door **Help** > **Installation Details** te selecteren en in de lijst met geïnstalleerde invoegtoepassingen naar Service Fabric te zoeken. Selecteer **Update** als er een nieuwere versie beschikbaar is.

Zie [Service Fabric-invoegtoepassing voor de ontwikkeling van Eclipse Java-toepassingen](service-fabric-get-started-eclipse.md) voor meer informatie.


## <a name="install-the-net-core-sdk-optional-if-you-want-to-use-the-net-core-programming-models"></a>De .NET Core-SDK installeren (alleen als u de .NET Core-programmeermodellen wilt gebruiken)
De .NET Core-SDK bevat de bibliotheken en sjablonen die vereist zijn voor het maken van Service Fabric-services met .NET Core.

1. Installeer het .NET Core-SDK-pakket.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Voer het SDK-installatiescript uit.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

## <a name="update-the-sdk-and-runtime"></a>De SDK en runtime bijwerken

Als u wilt bijwerken naar de nieuwste versie van de SDK en runtime, voert u de volgende opdrachten uit (hef de selectie op van ongewenste SDK's):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
```


> [!NOTE]
> Als de pakketten worden bijgewerkt, kan dit ertoe leiden dat uw lokale ontwikkelcluster wordt stopgezet. Start uw lokale cluster opnieuw op na een upgrade door de instructies op deze pagina te volgen.

## <a name="next-steps"></a>Volgende stappen
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Uw eerste CSharp-toepassing in Linux maken](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Uw ontwikkelomgeving voorbereiden in OSX](service-fabric-get-started-mac.md)
* [Azure CLI gebruiken voor het beheren van uw Service Fabric-toepassingen](service-fabric-azure-cli.md)
* [Verschillen tussen Service Fabric in Windows en Linux](service-fabric-linux-windows-differences.md)

## <a name="related-articles"></a>Verwante artikelen:

* [Aan de slag met Service Fabric en Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Aan de slag met Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

