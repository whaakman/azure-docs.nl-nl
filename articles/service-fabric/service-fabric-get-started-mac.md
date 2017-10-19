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
ms.date: 09/26/2017
ms.author: saysa
ms.openlocfilehash: 0fae5fe35c25f97a9eb2c0d648cfb0f66b7f0725
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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
Service Fabric wordt niet systeemeigen op OS X uitgevoerd. Als u een lokaal Service Fabric-cluster wilt uitvoeren, bieden we een vooraf geconfigureerde virtuele Ubuntu-machine met Vagrant en VirtualBox. Voordat u aan de slag gaat, hebt u het volgende nodig:

* [Vagrant (v1.8.4 of hoger)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> U moet wederzijds ondersteunde versies van Vagrant en VirtualBox gebruiken. Vagrant functioneert mogelijk niet goed in combinatie met een niet-ondersteunde versie van VirtualBox.
>

## <a name="create-the-local-vm"></a>De lokale virtuele machine maken
Ga als volgt te werk om de lokale VM te maken met een Service Fabric-cluster met 5-knooppunten:

1. De `Vagrantfile`-opslagplaats klonen

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Met deze stappen downloadt u het bestand `Vagrantfile` dat de VM-configuratie bevat plus de locatie van waar de virtuele machine wordt gedownload.  Het bestand verwijst naar een voorraadinstallatiekopie van Ubuntu.

2. Navigeer naar de lokale kloon van de opslagplaats

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. Wijzig de standaardinstellingen van de virtuele machine (optioneel)

    Standaard wordt de lokale virtuele machine als volgt geconfigureerd:

   * 3 GB toegewezen geheugen
   * Persoonlijk hostnetwerk geconfigureerd op IP 192.168.50.50 dat passthrough van verkeer van de Mac-host inschakelt

     U kunt deze instellingen wijzigen of een andere configuratie toevoegen aan de virtuele machine in de `Vagrantfile`. Zie de [Vagrant-documentatie](http://www.vagrantup.com/docs) voor de volledige lijst met configuratieopties.
4. De virtuele machine maken

    ```bash
    vagrant up
    ```


5. Aanmelden bij de VM en de Service Fabric SDK installeren

    ```bash
    vagrant ssh
    ```

   Installeer de SDK zoals wordt beschreven in [SDK-installatie](service-fabric-get-started-linux.md).  Het onderstaande script is bedoeld om u te helpen bij het installeren van de Service Fabric-runtime en de algemene Service Fabric SDK samen met SFTCL CLI. Als u het script uitvoert, wordt ervan uitgegaan dat u de licenties voor alle software die wordt geïnstalleerd, hebt gelezen en hiermee akkoord gaat.

    ```bash
    sudo curl -s https://raw.githubusercontent.com/Azure/service-fabric-scripts-and-templates/master/scripts/SetupServiceFabric/SetupServiceFabric.sh | sudo bash
    ```

5.  Het Service Fabric-cluster starten

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    >[!TIP]
    > Als het downloaden van de virtuele machine lang duurt, kunt u deze downloaden met behulp van wget of curl, of via een browser door te navigeren naar de koppeling die met **config.vm.box_url** is opgegeven in het bestand `Vagrantfile`. Nadat u deze lokaal hebt gedownload, bewerkt u `Vagrantfile`, zodat dit wijst naar het lokale pad waar u de installatiekopie hebt gedownload. Als u de installatiekopie bijvoorbeeld hebt gedownload naar /home/users/test/azureservicefabric.tp8.box, stelt u **config.vm.box_url** in op dat pad.
    >

5. Test of het cluster correct is ingesteld door naar Service Fabric Explorer te gaan op http://192.168.50.50:19080/Explorer (ervan uitgaande dat u het standaard IP-adres van het privénetwerk hebt gehouden).

    ![Service Fabric Explorer bekeken vanuit de host-Mac][sfx-mac]

## <a name="install-the-necessary-java-artifacts-on-vagrant-to-use-service-fabric-java-programming-model"></a>De benodigde Java-artefacten op Vagrant installeren als u het Service Fabric Java-programmeermodel wilt gebruiken

Als u Service Fabric-services wilt bouwen met behulp van Java, moet JDK 1.8 zijn samen zijn geïnstalleerd met Gradle, dat wordt gebruikt voor het uitvoeren van build-taken. Met het volgende fragment wordt Open JDK 1.8 samen geïnstalleerd met Gradle. De Service Fabric-Java-bibliotheken worden opgehaald uit Maven.

  ```bash
  vagrant ssh
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
```

## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>De Service Fabric-CLI (sfctl) instellen op een Mac

Volg de instructies in [Service Fabric-CLI](service-fabric-cli.md#cli-mac) als u de Service Fabric-CLI (`sfctl`) wilt installeren op een Mac.
De CLI-opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters, toepassingen en services.

## <a name="create-application-on-you-mac-using-yeoman"></a>Toepassingen maken op een Mac met Yeoman

Service Fabric biedt hulpprogramma's waarmee u vanuit de terminal een Service Fabric-toepassing kunt maken met behulp van de Yeoman-sjabloongenerator. Volg de stappen hieronder om te controleren of de Yeoman-sjabloongenerator van Service Fabric werkt op uw computer.

1. Node.js en NPM moeten zijn geïnstalleerd op uw Mac. Als dat niet het geval is, kunt u Node.js en NPM als volgt installeren met behulp van Homebrew. Gebruik de optie ``-v`` om te controleren welke versies van Node.js en NPM zijn geïnstalleerd op uw Mac.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. [Yeoman](http://yeoman.io/)-sjabloongenerator installeren op uw computer vanuit NPM

  ```bash
  npm install -g yo
  ```
3. Installeer de Yeoman generator die u wilt gebruiken. Volg hiervoor de stappen in deze [documentatie](service-fabric-get-started-linux.md). Volg deze stappen om Service Fabric-toepassingen te maken met behulp van Yeoman:

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. Als u een Service Fabric Java-toepassing wilt maken op een Mac, moeten JDK 1.8 en Gradle op de computer zijn geïnstalleerd.

## <a name="set-up-net-core-20-development"></a>.NET Core 2.0-ontwikkeling instellen

Installeer de [.NET Core 2.0 SDK voor Mac](https://www.microsoft.com/net/core#macos) om [C# Service Fabric-toepassingen te maken](service-fabric-create-your-first-linux-application-with-csharp.md). Pakketten voor .NET Core 2.0 Service Fabric-toepassingen worden gehost op NuGet.org, momenteel in preview.


## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>De Service Fabric-invoegtoepassing installeren voor Eclipse Neon

Service Fabric biedt een invoegtoepassing voor de **Eclipse Neon voor Java IDE** die het maken, bouwen en implementeren van Java-services kan vereenvoudigen. U kunt de installatiestappen volgen uit deze algemene [documentatie](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) over het installeren en bijwerken van de Service Fabric Eclipse-invoegtoepassing.

>[!TIP]
> De standaardinstelling is dat het standaard-IP-adres wordt ondersteund dat is opgegeven voor ``Vagrantfile`` in de ``Local.json`` van de gegenereerde toepassing. Als u dat adres wijzigt en Vagrant implementeert met een ander IP-adres, moet u het bijbehorende IP-adres in de ``Local.json`` van uw toepassing ook bijwerken.

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
