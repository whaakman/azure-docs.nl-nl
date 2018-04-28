---
title: Uw ontwikkelomgeving instellen in Linux | Microsoft Docs
description: Installeer de runtime en SDK en maak een lokaal ontwikkelcluster in Linux. Zodra u dit hebt gedaan, kunt u toepassingen bouwen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: bf88e4c702321a7810ec6a3e50eb6cd47a788734
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="prepare-your-development-environment-on-linux"></a>Uw ontwikkelomgeving voorbereiden in Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Als u [Azure Service Fabric-toepassingen](service-fabric-application-model.md) op uw Linux-ontwikkelmachine wilt implementeren en uitvoeren, moet u de runtime en algemene SDK installeren. U kunt ook optionele SDK's voor Java en .NET Core-ontwikkeling installeren. 

Bij de stappen in dit artikel wordt ervan uitgegaan dat u in het eigen systeem installeert op Linux of gebruikmaakt van de Service Fabric OneBox-containerinstallatiekopie, `microsoft/service-fabric-onebox`. 

Het installeren van de Service Fabric-runtime en -SDK op Windows Subsystem voor Linux wordt niet ondersteund. De Azure Service Fabric-opdrachtregelinterface (CLI), waarmee u Service Fabric-entiteiten kunt beheren die elders in de cloud of on-premises worden gehost, wordt echter wel ondersteund. Zie [De Service Fabric-CLI instellen](./service-fabric-cli.md) voor meer informatie over het installeren van de CLI.


## <a name="prerequisites"></a>Vereisten

* De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

    * Ubuntu 16.04 (`Xenial Xerus`)

      * Zorg ervoor dat het pakket `apt-transport-https` is geïnstalleerd:
         
         ```bash
         sudo apt-get install apt-transport-https
         ```
    * Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)


## <a name="installation-methods"></a>Installatiemethoden

### <a name="1-script-installation-ubuntu"></a>1. Installatie van script (Ubuntu)

Er is een script beschikbaar om u te helpen bij het installeren van de Service Fabric-runtime en de algemene Service Fabric SDK samen met **SFCTL** CLI. Voer de handmatige installatiestappen in de volgende sectie uit om te bepalen wat moet worden geïnstalleerd en welke licenties moeten worden geaccepteerd. Als u het script uitvoert, wordt ervan uitgegaan dat u akkoord gaat met de licenties voor alle software die wordt geïnstalleerd. 

Nadat het script is uitgevoerd, kunt u meteen verdergaan met [Een lokaal cluster instellen](#set-up-a-local-cluster).

```bash
sudo curl -s https://raw.githubusercontent.com/Azure/service-fabric-scripts-and-templates/master/scripts/SetupServiceFabric/SetupServiceFabric.sh | sudo bash
```

### <a name="2-manual-installation"></a>2. Handmatige installatie
Volg de rest van deze handleiding voor handmatige installatie van Service Fabric Runtime.

## <a name="update-your-apt-sourcesyum-repositories"></a>APT-sources/Yum-opslagplaatsen bijwerken
Voor het installeren van de SDK en het bijbehorende runtimepakket via het apt-get-opdrachtregelprogramma, moet u eerst uw APT-bronnen (Advanced Packaging Tool) bijwerken.

### <a name="ubuntu"></a>Ubuntu

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


### <a name="red-hat-enterprise-linux-74-service-fabric-preview-support"></a>Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)

1. Open een terminal.
2. Download en installeer extra pakketten voor Enterprise Linux (EPEL).

    ```bash
    wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    sudo yum install epel-release-latest-7.noarch.rpm
    ```
3. Voeg de EfficiOS RHEL7-pakketopslagplaats toe aan het systeem.

    ```bash
    sudo wget -P /etc/yum.repos.d/ https://packages.efficios.com/repo.files/EfficiOS-RHEL7-x86-64.repo
    ```

4. Importeer de aanmeldingssleutel voor het EfficiOS-pakket naar de lokale GPG-sleutelhanger.

    ```bash
    sudo rpmkeys --import https://packages.efficios.com/rhel/repo.key
    ```

5. Voeg de Microsoft RHEL-opslagplaats toe aan het systeem.

    ```bash
    curl https://packages.microsoft.com/config/rhel/7.4/prod.repo > ./microsoft-prod.repo
    sudo cp ./microsoft-prod.repo /etc/yum.repos.d/
    ```

6. Installeer dotnet SDK.

    ```bash
    yum install rh-dotnet20 -y
    ```

## <a name="install-and-set-up-the-service-fabric-sdk-for-local-cluster-setup"></a>De Service Fabric SDK installeren en instellen voor de installatie van een lokaal cluster

Wanneer u uw bronnen hebt bijgewerkt, kunt u de SDK installeren. Installeer het Service Fabric SDK-pakket, bevestig de installatie en ga akkoord met de gebruiksrechtovereenkomst.

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   Met de volgende opdrachten kunt u het accepteren van de licentie voor Service Fabric-pakketten automatiseren:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-ga select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-ga select true" | sudo debconf-set-selections
>   ```

### <a name="red-hat-enterprise-linux-74-service-fabric-preview-support"></a>Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)

```bash
sudo yum install servicefabricsdkcommon
```

De Service Fabric-runtime die wordt geleverd met de bovenstaande installatie omvat de pakketten in de onderstaande tabel. 

 | | DotNetCore | Java | Python | Node.js | 
--- | --- | --- | --- |---
Ubuntu | 2.0.0 | OpenJDK 1.8 | Implicit van npm | meest recente |
RHEL | - | OpenJDK 1.8 | Implicit van npm | meest recente |

## <a name="set-up-a-local-cluster"></a>Een lokaal cluster instellen
  Zodra de installatie is voltooid, kunt u een lokaal cluster starten.

  1. Voer het installatiescript van het cluster uit.

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. Open een webbrowser en ga naar [Service Fabric Explorer](http://localhost:19080/Explorer) (`http://localhost:19080/Explorer`). Als het cluster is gestart, ziet u het Service Fabric Explorer-dashboard. Het kan enkele minuten duren voordat het cluster volledig is ingesteld. Als het openen van de browser is mislukt of als in Service Fabric Explorer niet wordt aangegeven dat het systeem gereed is, wacht u enkele minuten en probeert u het opnieuw.

      ![Service Fabric Explorer in Linux][sfx-linux]

  Op dit punt kunt u vooraf samengestelde Service Fabric-toepassingspakketten of nieuwe implementeren op basis van gastcontainers of uitvoerbare gastbestanden. Als u nieuwe services wilt maken met behulp van de Java of .NET Core SDK's, voert u de optionele installatiestappen uit die in de volgende secties worden uitgelegd.


  > [!NOTE]
  > Zelfstandige clusters worden niet ondersteund in Linux.
  >


>   [!TIP]
    Als u een SSD-schijf beschikbaar hebt, is het raadzaam om met behulp van `--clusterdataroot` met devclustersetup.sh een SSD-mappad door te geven voor hogere prestaties.

## <a name="set-up-the-service-fabric-cli"></a>De Service Fabric-CLI instellen

De [Service Fabric-CLI](service-fabric-cli.md) bevat opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters en toepassingen.
Volg de instructies in [Service Fabric CLI](service-fabric-cli.md) voor het installeren van de CLI.


## <a name="set-up-yeoman-generators-for-containers-and-guest-executables"></a>Yeoman-generatoren instellen voor containers en uitvoerbare gastbestanden
Service Fabric biedt hulpprogramma's waarmee u vanuit een terminal Service Fabric-toepassingen kunt maken met behulp van Yeoman-sjabloongeneratoren. Volg deze stappen om de Service Fabric Yeoman-sjabloongeneratoren in te stellen:

1. nodejs en NPM installeren op uw computer

Ubuntu
  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```

Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)
  ```bash
  sudo yum install nodejs
  sudo yum install npm
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

Nadat u de generatoren hebt geïnstalleerd, kunt u uitvoerbare gastbestanden of containerservices maken door respectievelijk `yo azuresfguest` of `yo azuresfcontainer` uit te voeren.

## <a name="set-up-net-core-20-development"></a>.NET Core 2.0-ontwikkeling instellen

Installeer de [.NET Core 2.0 SDK voor Ubuntu](https://www.microsoft.com/net/core#linuxubuntu) om [C# Service Fabric-toepassingen te maken](service-fabric-create-your-first-linux-application-with-csharp.md). Pakketten voor .NET Core 2.0 Service Fabric-toepassingen worden gehost op NuGet.org, momenteel in preview.

## <a name="set-up-java-development"></a>Java-ontwikkeling instellen

Als u Service Fabric-services wilt bouwen met Java, installeert u JDK 1.8 en Gradle om bouwtaken uit te voeren. Met het volgende fragment wordt Open JDK 1.8 samen geïnstalleerd met Gradle. De Service Fabric-Java-bibliotheken worden opgehaald uit Maven.


Ubuntu 
 ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)
  ```bash
  sudo yum install java-1.8.0-openjdk-devel
  curl -s https://get.sdkman.io | bash
  sdk install gradle
  ```
 
## <a name="install-the-eclipse-plug-in-optional"></a>De Eclipse-invoegtoepassing installeren (optioneel)

U kunt de Eclipse-invoegtoepassing voor Service Fabric installeren vanuit de Eclipse IDE voor Java-ontwikkelaars of Java EE-ontwikkelaars. Met Eclipse kunt u behalve Service Fabric Java-toepassingen ook uitvoerbare Fabric Service-gasttoepassingen en containertoepassingen maken.

> [!IMPORTANT]
> Voor de Service Fabric-invoegtoepassing is Eclipse Neon of een hogere versie vereist. Zie de instructies na deze opmerking voor het controleren van uw Eclipse-versie. Als er een oudere versie van Eclipse is geïnstalleerd, kunt u een nieuwere versie downloaden van de [site van Eclipse](https://www.eclipse.org). Het wordt afgeraden een oudere versie van Eclipse te overschrijven met een nieuwere versie. Verwijder de oudere versie voordat u het installatieprogramma uitvoert of installeer de nieuwe versie in een andere map. 
> 
> Voor Ubuntu wordt u aangeraden de installatie rechtstreeks vanaf de site van Eclipse uit te voeren en niet door middel van een installatieprogramma voor pakketten (`apt` of `apt-get`). Daardoor weet u zeker dat u de meest recente versie van Eclipse hebt. U kunt de Eclipse IDE installeren voor Java-ontwikkelaars of voor Java EE-ontwikkelaars.

1. Controleer in Eclipse of u Eclipse Neon of later en Buildship versie 2.2.1 of later hebt geïnstalleerd. U kunt de versies van geïnstalleerde onderdelen controleren door **Help** > **About Eclipse** > **Installation Details** te selecteren. U kunt Buildship bijwerken met behulp van de instructies in [Eclipse Buildship: Eclipse-invoegtoepassingen voor Gradle][buildship-update].

2. Als u de Service Fabric-invoegtoepassing wilt installeren, selecteert u **Help** > **Install New Software**.

3. In het vak **Work with** typt u **http://dl.microsoft.com/eclipse**.

4. Klik op **Add**.

    ![De pagina met beschikbare software][sf-eclipse-plugin]

5. Selecteer de **Fabric Service**-invoegtoepassing en klik op **Next**.

6. Voer de installatiestappen uit en accepteer de gebruiksrechtovereenkomst.

Als u de Eclipse-invoegtoepassing voor Service Fabric al hebt geïnstalleerd, controleert u of u de meest recente versie gebruikt. U kunt dit controleren door **Help** > **About Eclipse** > **Installation Details** te selecteren en in de lijst met geïnstalleerde invoegtoepassingen naar Service Fabric te zoeken. Selecteer **Update** als er een nieuwere versie beschikbaar is.

Zie [Service Fabric-invoegtoepassing voor de ontwikkeling van Eclipse Java-toepassingen](service-fabric-get-started-eclipse.md) voor meer informatie.

## <a name="update-the-sdk-and-runtime"></a>De SDK en runtime bijwerken

Als u wilt bijwerken naar de nieuwste versie van de SDK en runtime, voert u de volgende opdrachten uit:

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon
```
Als u de binaire bestanden uit de Java SDK wilt bijwerken vanaf Maven, moet u de versiegegevens van het bijbehorende binaire bestand in het bestand ``build.gradle`` aanpassen, zodat deze verwijzen naar de nieuwste versie. Als u precies wilt weten waar u de versie moet bijwerken, kunt u kijken naar een bestand ``build.gradle`` in de [voorbeelden](https://github.com/Azure-Samples/service-fabric-java-getting-started) om snel aan de slag te gaan met Service Fabric.

> [!NOTE]
> Als de pakketten worden bijgewerkt, kan dit ertoe leiden dat uw lokale ontwikkelcluster wordt stopgezet. Start uw lokale cluster opnieuw op na een upgrade door de instructies op deze pagina te volgen.

## <a name="remove-the-sdk"></a>De SDK verwijderen
Voer het volgende uit om de Service Fabric SDK's te verwijderen:

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get remove servicefabric servicefabicsdkcommon
sudo npm uninstall generator-azuresfcontainer
sudo npm uninstall generator-azuresfguest
sudo apt-get install -f
```


### <a name="red-hat-enterprise-linux-74-service-fabric-preview-support"></a>Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)

```bash
sudo yum remote servicefabric servicefabicsdkcommon
sudo npm uninstall generator-azuresfcontainer
sudo npm uninstall generator-azuresfguest
```

## <a name="next-steps"></a>Volgende stappen

* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Uw eerste CSharp-toepassing in Linux maken](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Uw ontwikkelomgeving voorbereiden in OSX](service-fabric-get-started-mac.md)
* [Een Linux-ontwikkelomgeving voorbereiden in Windows](service-fabric-local-linux-cluster-windows.md)
* [De Service Fabric-CLI gebruiken voor het beheren van uw toepassingen](service-fabric-application-lifecycle-sfctl.md)
* [Verschillen tussen Service Fabric in Windows en Linux](service-fabric-linux-windows-differences.md)
* [Besturingssysteempatches automatiseren in uw Linux-cluster](service-fabric-patch-orchestration-application-linux.md)
* [Aan de slag met Service Fabric-CLI](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
