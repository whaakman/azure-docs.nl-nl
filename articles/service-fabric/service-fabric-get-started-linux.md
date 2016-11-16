---
title: Uw ontwikkelomgeving instellen in Linux | Microsoft Docs
description: Installeer de runtime en SDK en maak een lokaal ontwikkelcluster in Linux. Zodra u dit hebt gedaan, kunt u toepassingen bouwen.
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 567a998102558626df73878865b317b830ba1faa


---
# <a name="prepare-your-development-environment-on-linux"></a>Uw ontwikkelomgeving voorbereiden in Linux
> [!div class="op_single_selector"]
> -[ Windows](service-fabric-get-started.md)
> 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Als u [Azure Service Fabric-toepassingen](service-fabric-application-model.md) op uw Linux-ontwikkelmachine wilt implementeren en uitvoeren, moet u de runtime en algemene SDK installeren. U kunt ook optionele SDK's voor Java en .NET Core installeren.

## <a name="prerequisites"></a>Vereisten
### <a name="supported-operating-system-versions"></a>Ondersteunde versies van besturingssystemen
De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

* Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Uw apt-bronnen bijwerken
Voor het installeren van de SDK en het bijbehorende runtimepakket via apt-get, moet u eerst uw apt-bronnen bijwerken.

1. Open een terminal.
2. Voeg de Service Fabric-opslagplaats toe aan uw lijst met bronnen.
   
    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```
3. Voeg de nieuwe GPG-sleutel toe aan uw apt-sleutelhanger.
   
    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```
4. Vernieuw uw pakketlijsten op basis van de net toegevoegde opslagplaatsen.
   
    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>De SDK installeren en instellen
Wanneer uw bronnen zijn bijgewerkt, kunt u de SDK installeren.

1. Installeer het Service Fabric-SDK-pakket. U wordt gevraagd om de installatie te bevestigen en de licentieovereenkomst te accepteren.
   
    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```
2. Voer het SDK-installatiescript uit.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-crossplatform-cli"></a>De platformoverschrijdende CLI van Azure instellen
De [platformoverschrijdende CLI van Azure][azure-xplat-cli-github] bevat opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters en toepassingen. Deze is gebaseerd op Node.js en daarom [moet u controleren of u Node][installatieknooppunt] hebt geïnstalleerd voordat u doorgaat met de onderstaande instructies.

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

## <a name="set-up-a-local-cluster"></a>Een lokaal cluster instellen
Als alles goed is geïnstalleerd, moet u een lokaal cluster kunnen starten.

1. Voer het installatiescript van het cluster uit.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
2. Open een webbrowser en navigeer naar http://localhost:19080/Explorer. Als het cluster is gestart, ziet u het Service Fabric Explorer-dashboard.
   
    ![Service Fabric Explorer in Linux][sfx-linux]

Op dit punt kunt u vooraf samengestelde Service Fabric-toepassingspakketten of nieuwe implementeren op basis van gastcontainers of uitvoerbare gastbestanden. Als u nieuwe services wilt maken met behulp van de Java of .NET Core SDK's, voert u de volgende optionele installatiestappen uit.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>De Java-SDK en Eclipse Neon-invoegtoepassing installeren (optioneel)
De Java-SDK biedt de bibliotheken en sjablonen die vereist zijn voor het bouwen van Service Fabric-services met behulp van Java.

1. Installeer het Java-SDK-pakket.
   
    ```bash
    sudo apt-get install servicefabricsdkjava
    ```
2. Voer het SDK-installatiescript uit.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

U kunt de Eclipse-invoegtoepassing voor Service Fabric installeren vanuit de Eclipse Neon IDE.

1. Zorg ervoor dat u in Eclips Buildship versie 1.0.17 of hoger hebt geïnstalleerd. U kunt de versies van geïnstalleerde onderdelen controleren door **Help > Installation Details** te kiezen. U kunt Buildship bijwerken met behulp van [deze instructies][buildship-bijwerken].
2. Als u de Service Fabric-invoegtoepassing wilt installeren, kiest u **Help > Install New Software...**
3. Voer in het tekstvak 'Work with' het volgende in: http://dl.windowsazure.com/eclipse/servicefabric
4. Klik op Add.
   
    ![Eclipse-invoegtoepassing][sf-eclipse-plugin]
5. Kies de Service Fabric-invoegtoepassing en klik op Next.
6. Doorloop de installatie en accepteer de gebruiksrechtovereenkomst.

## <a name="install-the-net-core-sdk-optional"></a>De .NET Core-SDK installeren (optioneel)
De .NET Core-SDK biedt de bibliotheken en sjablonen die vereist zijn voor het bouwen van Service Fabric-services met behulp van platformoverschrijdende .NET Core.

1. Installeer het .NET Core-SDK-pakket.
   
    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```
2. Voer het SDK-installatiescript uit.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Volgende stappen
* [Uw eerste Java-toepassing in Linux maken](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw ontwikkelomgeving voorbereiden in OSX](service-fabric-get-started-mac.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[installatieknooppunt]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-bijwerken]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png



<!--HONumber=Nov16_HO2-->


