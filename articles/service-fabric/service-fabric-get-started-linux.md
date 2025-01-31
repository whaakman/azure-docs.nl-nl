---
title: Uw ontwikkelomgeving instellen in Linux | Microsoft Docs
description: Installeer de runtime en SDK en maak een lokaal ontwikkelcluster in Linux. Zodra u dit hebt gedaan, kunt u toepassingen bouwen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 49e80c3fc8935064aceef8ef4e2bd3257c41e5e7
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514194"
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

Het installeren van de Service Fabric-runtime en -SDK op Windows Subsystem voor Linux wordt niet ondersteund. U kunt Service Fabric-entiteiten beheren die elders in de cloud of on-premises worden gehost met de Azure Service Fabric-opdrachtregelinterface (CLI), die wordt ondersteund. Zie [De Service Fabric-CLI instellen](./service-fabric-cli.md) voor meer informatie over het installeren van de CLI.


## <a name="prerequisites"></a>Vereisten

Deze besturingssysteemversies worden ondersteund voor ontwikkeling.

* Ubuntu 16.04 (`Xenial Xerus`)

    Zorg ervoor dat het pakket `apt-transport-https` is geïnstalleerd.
         
    ```bash
    sudo apt-get install apt-transport-https
    ```
* Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)


## <a name="installation-methods"></a>Installatiemethoden

### <a name="script-installation-ubuntu"></a>Installatie van script (Ubuntu)

Voor uw gemak is er een script beschikbaar om u te helpen bij het installeren van de Service Fabric-runtime en de algemene Service Fabric SDK samen met de **SFCTL** CLI. Voer de handmatige installatiestappen in de volgende sectie uit. U ziet wat er wordt geïnstalleerd en de bijbehorende licenties. Als u het script uitvoert, wordt ervan uitgegaan dat u akkoord gaat met de licenties voor alle software die wordt geïnstalleerd.

Nadat het script is uitgevoerd, kunt u verdergaan met [Een lokaal cluster instellen](#set-up-a-local-cluster).

```bash
sudo curl -s https://raw.githubusercontent.com/Azure/service-fabric-scripts-and-templates/master/scripts/SetupServiceFabric/SetupServiceFabric.sh | sudo bash
```

### <a name="manual-installation"></a>Handmatige installatie
Volg de rest van deze handleiding voor handmatige installatie van de Service Fabric-runtime en de algemene SDK.

## <a name="update-your-apt-sources-or-yum-repositories"></a>APT-bronnen of Yum-opslagplaatsen bijwerken
Voor het installeren van de SDK en het bijbehorende runtimepakket via het apt-get-opdrachtregelprogramma, moet u eerst uw APT-bronnen (Advanced Packaging Tool) bijwerken.

### <a name="ubuntu"></a>Ubuntu

1. Open een terminal.
2. Voeg de Service Fabric-opslagplaats toe aan uw lijst met bronnen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Voeg de `dotnet`-opslagplaats toe aan uw lijst met bronnen.

    ```bash
    wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    ```

4. Voeg de nieuwe Gnu Privacy Guard-code (GnuPG of GPG) toe aan uw APT-sleutelring.

    ```bash
    curl -fsSL https://packages.microsoft.com/keys/msopentech.asc | sudo apt-key add -
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

7. Azul JDK sleutel toevoegen aan uw APT-sleutelring en stelt u de opslagplaats.

    ```bash
    curl -fsSL https://repos.azul.com/azul-repo.key | sudo apt-key add -
    sudo add-apt-repository 'deb http://repos.azulsystems.com/ubuntu stable main'
    ```

8. Vernieuw uw pakketlijsten op basis van de net toegevoegde opslagplaatsen.

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

6. Installeer de .NET SDK.

    ```bash
    yum install rh-dotnet20 -y
    ```

## <a name="install-and-set-up-the-service-fabric-sdk-for-a-local-cluster"></a>De Service Fabric SDK installeren en instellen voor een lokaal cluster

Wanneer u uw bronnen hebt bijgewerkt, kunt u de SDK installeren. Installeer het Service Fabric SDK-pakket, bevestig de installatie en accepteer de gebruiksrechtovereenkomst.

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get install servicefabricsdkcommon
```

> [!TIP]
>   Met de volgende opdrachten kunt u het accepteren van de licentie voor Service Fabric-pakketten automatiseren:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-ga select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-ga select true" | sudo debconf-set-selections
>   ```

### <a name="red-hat-enterprise-linux-74-service-fabric-preview-support"></a>Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)

```bash
sudo yum install servicefabricsdkcommon
```

De Service Fabric-runtime die wordt geleverd met de installatie omvat de pakketten in de volgende tabel. 

 | | DotNetCore | Java | Python | Node.js | 
--- | --- | --- | --- |---
Ubuntu | 2.0.0 | AzulJDK 1.8 | Implicit van npm | latest |
RHEL | - | OpenJDK 1.8 | Implicit van npm | latest |

## <a name="set-up-a-local-cluster"></a>Een lokaal cluster instellen
Start een lokaal cluster nadat de installatie is voltooid.

1. Voer het installatiescript van het cluster uit.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Open een webbrowser en ga naar [Service Fabric Explorer](http://localhost:19080/Explorer) (`http://localhost:19080/Explorer`). Wanneer het cluster is gestart, ziet u het Service Fabric Explorer-dashboard. Het kan enkele minuten duren voordat het cluster volledig is ingesteld. Als het openen van de browser is mislukt of als in Service Fabric Explorer niet wordt aangegeven dat het systeem gereed is, wacht u enkele minuten en probeert u het opnieuw.

    ![Service Fabric Explorer in Linux][sfx-linux]

    U kunt nu vooraf samengestelde Service Fabric-toepassingspakketten of nieuwe implementeren op basis van gastcontainers of uitvoerbare gastbestanden. Als u nieuwe services wilt maken met behulp van de Java of .NET Core SDK's, voert u de optionele installatiestappen uit die in de volgende secties worden uitgelegd.


> [!NOTE]
> Zelfstandige clusters worden niet ondersteund in Linux.


> [!TIP]
> Als u een SSD-schijf beschikbaar hebt, is het raadzaam om met behulp van `--clusterdataroot` met devclustersetup.sh een SSD-mappad door te geven voor hogere prestaties.

## <a name="set-up-the-service-fabric-cli"></a>De Service Fabric-CLI instellen

De [Service Fabric-CLI](service-fabric-cli.md) bevat opdrachten voor interactie met Service Fabric-entiteiten, inclusief clusters en toepassingen. Volg de instructies in [Service Fabric-CLI](service-fabric-cli.md) voor het installeren van de CLI.


## <a name="set-up-yeoman-generators-for-containers-and-guest-executables"></a>Yeoman-generatoren instellen voor containers en uitvoerbare gastbestanden
Service Fabric biedt hulpprogramma's waarmee u vanuit een terminal Service Fabric-toepassingen kunt maken met behulp van Yeoman-sjabloongeneratoren. Volg deze stappen om de Service Fabric Yeoman-sjabloongeneratoren in te stellen:

1. Installeer nodejs en NPM op uw computer.

    ```bash
    sudo add-apt-repository "deb https://deb.nodesource.com/node_8.x $(lsb_release -s -c) main"
    sudo apt-get update
    sudo apt-get install nodejs
    ```
2. Installeer de [Yeoman](https://yeoman.io/)-sjabloongenerator op uw computer vanuit NPM.

    ```bash
    sudo npm install -g yo
    ```
3. Installeer de Yeo-containergenerator voor Service Fabric en de generator voor uitvoerbare gastbestanden vanuit NPM.

    ```bash
    sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
    sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
    ```

Nadat u de generatoren hebt geïnstalleerd, kunt u uitvoerbare gastbestanden of containerservices maken door respectievelijk `yo azuresfguest` of `yo azuresfcontainer` uit te voeren.

## <a name="set-up-net-core-20-development"></a>.NET Core 2.0-ontwikkeling instellen

Installeer de [.NET Core 2.0 SDK voor Ubuntu](https://www.microsoft.com/net/core#linuxubuntu) om [C# Service Fabric-toepassingen te maken](service-fabric-create-your-first-linux-application-with-csharp.md). NuGet.org host pakketten voor .NET Core 2.0 Service Fabric-toepassingen, momenteel in preview.

## <a name="set-up-java-development"></a>Java-ontwikkeling instellen

Als u wilt maken met behulp van Java Service Fabric-services, installeert u Gradle om uit te voeren build-taken. Voer de onderstaande opdracht Gradle te installeren. De Service Fabric-Java-bibliotheken worden opgehaald uit Maven.


* Ubuntu

    ```bash
    curl -s https://get.sdkman.io | bash
    sdk install gradle 5.1
    ```

* Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)

  ```bash
  sudo yum install java-1.8.0-openjdk-devel
  curl -s https://get.sdkman.io | bash
  sdk install gradle
  ```

U moet ook de Yeo-containergenerator voor Service Fabric installeren voor uitvoerbare Java-bestanden. Zorg ervoor dat u [Yeoman hebt geïnstalleerd](#set-up-yeoman-generators-for-containers-and-guest-executables) en voer de volgende opdracht uit:

  ```bash
  npm install -g generator-azuresfjava
  ```
 
## <a name="install-the-eclipse-plug-in-optional"></a>De Eclipse-invoegtoepassing installeren (optioneel)

U kunt de Eclipse-invoegtoepassing voor Service Fabric installeren vanuit de Eclipse IDE voor Java-ontwikkelaars of Java EE-ontwikkelaars. Met Eclipse kunt u behalve Service Fabric Java-toepassingen ook uitvoerbare Fabric Service-gasttoepassingen en containertoepassingen maken.

> [!IMPORTANT]
> Voor de Service Fabric-invoegtoepassing is Eclipse Neon of een hogere versie vereist. Zie de instructies na deze opmerking voor het controleren van uw Eclipse-versie. Als er een oudere versie van Eclipse is geïnstalleerd, kunt u een nieuwere versie downloaden van de [site van Eclipse](https://www.eclipse.org). We raden u af een oudere versie van Eclipse te overschrijven met een nieuwere versie. Verwijder de oudere versie voordat u het installatieprogramma uitvoert of installeer de nieuwe versie in een andere map.
> 
> Voor Ubuntu wordt u aangeraden de installatie rechtstreeks vanaf de site van Eclipse uit te voeren en niet door middel van een installatieprogramma voor pakketten (`apt` of `apt-get`). Daardoor weet u zeker dat u de meest recente versie van Eclipse hebt. U kunt de Eclipse IDE installeren voor Java-ontwikkelaars of voor Java EE-ontwikkelaars.

1. Controleer in Eclipse of u Eclipse Neon of later en Buildship versie 2.2.1 of later hebt geïnstalleerd. Controleer de versies van geïnstalleerde onderdelen door **Help** > **About Eclipse** > **Installation Details** te selecteren. U kunt Buildship bijwerken met behulp van de instructies op de [Eclipse Buildship: Eclipse-invoegtoepassingen voor Gradle][buildship-update].

2. Als u de Service Fabric-invoegtoepassing wilt installeren, selecteert u **Help** > **Install New Software**.

3. In het vak **Work with** voert u **https://dl.microsoft.com/eclipse** in.

4. Selecteer **Toevoegen**.

    ![Pagina met beschikbare software][sf-eclipse-plugin]

5. Selecteer de **Service Fabric**-invoegtoepassing en klik op **Next**.

6. Voer de installatiestappen uit. Ga vervolgens akkoord met de gebruiksrechtovereenkomst.

Als u de Eclipse-invoegtoepassing voor Service Fabric al hebt geïnstalleerd, controleert u of u de meest recente versie gebruikt. Dit controleert u door **Help** > **About Eclipse** > **Installation Details** te selecteren. Zoek vervolgens naar Service Fabric in de lijst met geïnstalleerde invoegtoepassingen. Selecteer **Update** als er een nieuwere versie beschikbaar is.

Zie [Service Fabric-invoegtoepassing voor de ontwikkeling van Eclipse Java-toepassingen](service-fabric-get-started-eclipse.md) voor meer informatie.

## <a name="update-the-sdk-and-runtime"></a>De SDK en runtime bijwerken

Als u wilt bijwerken naar de nieuwste versie van de SDK en runtime, voert u de volgende opdrachten uit.

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon
```
Als u de binaire bestanden uit de Java SDK wilt bijwerken vanaf Maven, moet u de versiegegevens van het bijbehorende binaire bestand in het bestand ``build.gradle`` aanpassen, zodat deze verwijzen naar de nieuwste versie. Als u precies wilt weten waar u de versie moet bijwerken, kunt u kijken naar een ``build.gradle``-bestand in de [voorbeelden om snel aan de slag te gaan met Service Fabric](https://github.com/Azure-Samples/service-fabric-java-getting-started).

> [!NOTE]
> Als de pakketten worden bijgewerkt, kan dit ertoe leiden dat uw lokale ontwikkelcluster wordt stopgezet. Start uw lokale cluster opnieuw op na een upgrade door de instructies in dit artikel te volgen.

## <a name="remove-the-sdk"></a>De SDK verwijderen
Voer de volgende opdrachten uit om de Service Fabric SDK's te verwijderen.

* Ubuntu

    ```bash
    sudo apt-get remove servicefabric servicefabicsdkcommon
    npm uninstall -g generator-azuresfcontainer
    npm uninstall -g generator-azuresfguest
    sudo apt-get install -f
    ```


* Red Hat Enterprise Linux 7.4 (ondersteuning voor Service Fabric-preview)

    ```bash
    sudo yum remove servicefabric servicefabicsdkcommon
    npm uninstall -g generator-azuresfcontainer
    npm uninstall -g generator-azuresfguest
    ```

## <a name="next-steps"></a>Volgende stappen

* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Uw eerste CSharp-toepassing in Linux maken](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Uw ontwikkelomgeving voorbereiden in OSX](service-fabric-get-started-mac.md)
* [Een Linux-ontwikkelomgeving voorbereiden in Windows](service-fabric-local-linux-cluster-windows.md)
* [Uw toepassingen beheren met behulp van de Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md)
* [Verschillen tussen Service Fabric in Windows en Linux](service-fabric-linux-windows-differences.md)
* [Aan de slag met Service Fabric-CLI](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
