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
ms.date: 8/23/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 8d902ba9e2077b4b70762c76cfb3ebf12752fb11
ms.contentlocale: nl-nl
ms.lasthandoff: 09/14/2017

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
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
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
    sudo apt-get install curl
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

## <a name="install-and-set-up-the-sdk-for-local-cluster-setup"></a>De SDK installeren en instellen voor gebruik van een lokaal cluster

Wanneer u uw bronnen hebt bijgewerkt, kunt u de SDK installeren. Installeer het Service Fabric SDK-pakket, bevestig de installatie en ga akkoord met de gebruiksrechtovereenkomst.

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   Met de volgende opdrachten kunt u het accepteren van de licentie voor Service Fabric-pakketten automatiseren:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

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

## <a name="set-up-the-service-fabric-cli"></a>De Service Fabric-CLI instellen

De [Service Fabric-CLI](service-fabric-cli.md) bevat opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters en toepassingen.
Volg de instructies in [Service Fabric CLI](service-fabric-cli.md) voor het installeren van de CLI.


## <a name="install-and-set-up-the-generators-for-containers-and-guest-executables"></a>De generatoren voor containers en uitvoerbare gastbestanden installeren en instellen
Service Fabric biedt hulpprogramma's waarmee u vanuit de terminal een Service Fabric-toepassing kunt maken met behulp van de Yeoman-sjabloongenerator. Volg de stappen hieronder om te controleren of de Yeoman-sjabloongenerator van Service Fabric werkt op uw computer.

1. nodejs en NPM installeren op uw computer

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. [Yeoman](http://yeoman.io/)-sjabloongenerator installeren op uw computer vanuit NPM

  ```bash
  sudo npm install -g yo
  ```
3. Yeo-containergenerator voor Service Fabric en de generator voor uitvoerbare gastbestanden installeren vanuit NPM

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

Nadat u de bovenstaande generatoren hebt geïnstalleerd, moet u apps kunnen maken met uitvoerbare gastbestanden of containerservices door respectievelijk `yo azuresfguest` of `yo azuresfcontainer` uit te voeren.

## <a name="install-the-necessary-java-artifacts-optional-if-you-want-to-use-the-java-programming-models"></a>De benodigde Java-artefacten installeren (alleen als u de Java-programmeermodellen wilt gebruiken)

Als u Service Fabric-services wilt bouwen met behulp van Java, moet JDK 1.8 zijn samen zijn geïnstalleerd met Gradle, dat wordt gebruikt voor het uitvoeren van build-taken. Met het volgende fragment wordt Open JDK 1.8 samen geïnstalleerd met Gradle. De Service Fabric-Java-bibliotheken worden opgehaald uit Maven.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a>De Eclipse Neon-invoegtoepassing installeren (optioneel)

U kunt de Eclipse-invoegtoepassing voor Service Fabric installeren vanuit de **Eclipse IDE voor Java-ontwikkelaars**. Met Eclipse kunt u behalve Service Fabric Java-toepassingen ook uitvoerbare Fabric Service-gasttoepassingen en containertoepassingen maken.

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
De .NET Core-SDK bevat de bibliotheken en sjablonen die vereist zijn voor het maken van Service Fabric-services met .NET Core. Installeer het .NET Core SDK-pakket door het volgende uit te voeren:

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-the-sdk-and-runtime"></a>De SDK en runtime bijwerken

Als u wilt bijwerken naar de nieuwste versie van de SDK en runtime, voert u de volgende opdrachten uit (hef de selectie op van ongewenste SDK's):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
Als u de binaire bestanden uit de Java SDK wilt bijwerken vanaf Maven, moet u de versiegegevens van het bijbehorende binaire bestand in het bestand ``build.gradle`` aanpassen, zodat deze verwijzen naar de nieuwste versie. Als u precies wilt weten waar u de versie moet bijwerken, kunt u kijken naar een bestand ``build.gradle`` in de [voorbeelden](https://github.com/Azure-Samples/service-fabric-java-getting-started) om snel aan de slag te gaan met Service Fabric.

> [!NOTE]
> Als de pakketten worden bijgewerkt, kan dit ertoe leiden dat uw lokale ontwikkelcluster wordt stopgezet. Start uw lokale cluster opnieuw op na een upgrade door de instructies op deze pagina te volgen.

## <a name="next-steps"></a>Volgende stappen

* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Uw eerste CSharp-toepassing in Linux maken](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Uw ontwikkelomgeving voorbereiden in OSX](service-fabric-get-started-mac.md)
* [De Service Fabric-CLI gebruiken voor het beheren van uw toepassingen](service-fabric-application-lifecycle-sfctl.md)
* [Verschillen tussen Service Fabric in Windows en Linux](service-fabric-linux-windows-differences.md)
* [Aan de slag met Service Fabric-CLI](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

