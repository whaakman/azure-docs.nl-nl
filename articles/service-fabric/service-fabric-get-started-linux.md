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
ms.date: 03/23/2017
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 516b8e517a16dd0d87e02189260166696225fbab
ms.lasthandoff: 03/28/2017


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

### <a name="supported-operating-system-versions"></a>Ondersteunde versies van besturingssystemen
De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

* Ubuntu 16.04 (i**'Xenial Xerus'**)

## <a name="update-your-apt-sources"></a>Uw apt-bronnen bijwerken
Voor het installeren van de SDK en het bijbehorende runtimepakket via apt-get, moet u eerst uw apt-bronnen bijwerken.

1. Open een terminal.
2. Voeg de Service Fabric-opslagplaats toe aan uw lijst met bronnen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```
3. Voeg de **dotnet**-opslagplaats toe aan uw lijst met bronnen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```
4. Voeg de nieuwe GPG-sleutel toe aan uw apt-sleutelhanger.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```
    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Vernieuw uw pakketlijsten op basis van de net toegevoegde opslagplaatsen.

    ```bash
    sudo apt-get update
    ```
## <a name="install-and-set-up-the-sdk-for-containers-and-guest-executables"></a>De SDK voor containers en uitvoerbare gastbestanden installeren en instellen
Wanneer uw bronnen zijn bijgewerkt, kunt u de SDK installeren.

1. Installeer het Service Fabric-SDK-pakket. U wordt gevraagd om de installatie te bevestigen en de licentieovereenkomst te accepteren.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```
2. Voer het SDK-installatiescript uit.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Wanneer u de stappen hebt uitgevoerd om het algemene SDK-pakket te installeren, moet u apps kunnen maken met uitvoerbare gastbestanden of containerservices door `yo azuresfguest` uit te voeren. Mogelijk moet u de omgevingsvariabele **$NODE_PATH** instellen op de locatie waarop de knooppuntmodules zich bevinden. 

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

## <a name="set-up-the-azure-cross-platform-cli"></a>De platformoverschrijdende CLI van Azure instellen
De [platformoverschrijdende CLI van Azure][azure-xplat-cli-github] bevat opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters en toepassingen. Deze is gebaseerd op Node.js en daarom [moet u controleren of u Node hebt geïnstalleerd][install-node] voordat u doorgaat met de volgende instructies:

1. Kloon de github-opslagplaats naar uw ontwikkelmachine.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```
2. Schakel over naar de gekloonde opslagplaats en installeer de CLI-afhankelijkheden met behulp van de Node Package Manager (NPM).

    ```bash
    cd azure-xplat-cli
    npm install
    ```
3. Maak een symlink vanuit de map bin/azure van de gekloonde opslagplaats naar /usr/bin/azure, zodat deze wordt toegevoegd aan het pad en opdrachten beschikbaar zijn vanuit elke andere map.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```
4. Schakel ten slotte automatische aanvulling voor Service Fabric-opdrachten in.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

> [!NOTE]
> Service Fabric-opdrachten zijn nog niet beschikbaar in Azure CLI 2.0.


## <a name="set-up-a-local-cluster"></a>Een lokaal cluster instellen
Als alles goed is geïnstalleerd, moet u een lokaal cluster kunnen starten.

1. Voer het installatiescript van het cluster uit.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
2. Open een webbrowser en navigeer naar http://localhost:19080/Explorer. Als het cluster is gestart, ziet u het Service Fabric Explorer-dashboard.

    ![Service Fabric Explorer in Linux][sfx-linux]

Op dit punt kunt u vooraf samengestelde Service Fabric-toepassingspakketten of nieuwe implementeren op basis van gastcontainers of uitvoerbare gastbestanden. Als u nieuwe services wilt maken met behulp van de Java of .NET Core SDK's, voert u de optionele installatiestappen uit die in de volgende secties worden uitgelegd.


> [!NOTE]
> Zelfstandige clusters worden niet ondersteund in Linux - slechts clusters met één vak en clusters met meerdere Azure Linux-machines worden ondersteund in het voorbeeld.
>

## <a name="install-the-java-sdk-optional-if-you-wish-to-use-the-java-programming-models"></a>De Java-SDK installeren (alleen als u de Java-programmeermodellen wilt gebruiken)
De Java-SDK biedt de bibliotheken en sjablonen die vereist zijn voor het bouwen van Service Fabric-services met behulp van Java.

1. Installeer het Java-SDK-pakket.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```
2. Voer het SDK-installatiescript uit.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```
## <a name="install-the-eclipse-neon-plugin-optional"></a>De Eclipse Neon-invoegtoepassing installeren (optioneel)

U kunt de Eclipse-invoegtoepassing voor Service Fabric installeren vanuit de **Eclipse IDE voor Java-ontwikkelaars**. Met Eclipse kunt u behalve Service Fabric Java-toepassingen ook uitvoerbare Fabric Service-gasttoepassingen en containertoepassingen maken.

> [!NOTE]
> De installatie van de Java-SDK is een vereiste voor het gebruik van de Eclipse-invoegtoepassing, zelfs als u deze alleen gebruikt om uitvoerbare gastbestanden en containertoepassingen te maken en te implementeren.
>

1. Zorg er in Eclipse voor dat u de meest recente Eclipse **Neon**- en meest recente Buildship-versie (1.0.17 of hoger) hebt geïnstalleerd. U kunt de versies van geïnstalleerde onderdelen controleren door **Help > Installation Details** te kiezen. U kunt Buildship bijwerken met behulp van [deze instructies][buildship-update].
2. Als u de Service Fabric-invoegtoepassing wilt installeren, kiest u **Help > Install New Software...**
3. Voer in het tekstvak 'Work with' het volgende in: http://dl.windowsazure.com/eclipse/servicefabric
4. Klik op Add.
    ![Eclipse-invoegtoepassing][sf-eclipse-plugin]
5. Kies de Service Fabric-invoegtoepassing en klik op Next.
6. Doorloop de installatie en accepteer de gebruiksrechtovereenkomst.

Als u de Service Fabric Eclipse-invoegtoepassing al hebt geïnstalleerd, zorgt u ervoor dat u de meest recente versie gebruikt. U kunt dit controleren door ``Help => Installation Details`` te selecteren en in de lijst met geïnstalleerde modules naar Fabric Service te zoeken. Selecteer de update als er een nieuwere versie beschikbaar is. 

Zie [Aan de slag met de Eclipse-invoegtoepassing voor de ontwikkeling van Java-toepassingen met Service Fabric](service-fabric-get-started-eclipse.md) voor meer informatie.


## <a name="install-the-net-core-sdk-optional-if-you-wish-to-use-the-net-core-programming-models"></a>De .NET Core-SDK installeren (alleen als u de .NET Core-programmeermodellen wilt gebruiken)
De .NET Core-SDK biedt de bibliotheken en sjablonen die vereist zijn voor het bouwen van Service Fabric-services met behulp van platformoverschrijdende .NET Core.

1. Installeer het .NET Core-SDK-pakket.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Voer het SDK-installatiescript uit.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

## <a name="updating-the-sdk-and-runtime"></a>De SDK en Runtime bijwerken

Als u wilt bijwerken naar de nieuwste versie van de SDK en runtime, voert u de volgende stappen uit (verwijder SDK's uit de lijst die u niet wilt bijwerken of installeren):

   ```bash
   sudo apt-get update
   sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
   ```

Als u de CLI wilt bijwerken, gaat u naar de map waar u CLI hebt gekloond en voert u `git pull` uit om bij te werken.  Als er voor het bijwerken extra stappen nodig zijn, worden deze in de releaseopmerkingen genoemd. 


## <a name="next-steps"></a>Volgende stappen
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Uw eerste CSharp-toepassing in Linux maken](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Uw ontwikkelomgeving voorbereiden in OSX](service-fabric-get-started-mac.md)
* [De Azure CLI gebruiken voor het beheren van uw Service Fabric-toepassingen](service-fabric-azure-cli.md)
* [Verschillen tussen Service Fabric in Windows en Linux](service-fabric-linux-windows-differences.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

