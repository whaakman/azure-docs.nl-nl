---
title: Met behulp van de Docker-VM-extensie voor Linux op Azure
description: Beschrijving van Docker en de virtuele Machines van Azure-extensies en laat zien hoe u via een programma maken van virtuele Machines in Azure die docker-hosts vanaf de opdrachtregel met de Azure CLI.
services: virtual-machines-linux
documentationcenter: ''
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: 91f7ea54afce0e94953d4bb01bbb1b33f167fe22
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>De Docker VM-extensie gebruiken met de Azure-opdrachtregelinterface (Azure CLI)
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor meer informatie over het gebruik van de Docker-VM-extensie met het Resource Manager-model [hier](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

In dit onderwerp wordt beschreven hoe een virtuele machine maken met de Docker-VM-extensie van de modus van de service management (asm) in de Azure CLI op elk platform. [Docker](https://www.docker.com/) is een van de meest populaire virtualisatie-methoden die gebruikmaakt van [Linux containers](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines als een manier om gegevens te isoleren en berekeningen op gedeelde bronnen. U kunt de Docker-VM-extensie en de [Azure Linux Agent](../../extensions/agent-linux.md) voor het maken van een Docker-virtuele machine die als host fungeert voor een onbeperkt aantal containers voor uw toepassingen in Azure. Zie voor een hoog niveau bespreking van containers en hun voordelen de [Docker hoog niveau Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a>Het gebruik van de Docker-VM-extensie met Azure
Voor het gebruik van de Docker-VM-extensie met Azure, moet u een versie van de [Azure-opdrachtregelinterface](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) hoger is dan 0.8.6 (zoals de huidige versie van dit artikel 0.10.0 is). U kunt de Azure CLI voor Mac, Linux en Windows installeren.

Het volledige proces Docker gebruiken in Azure is eenvoudig:

* De Azure CLI en de bijbehorende afhankelijkheden installeren op de computer van waaruit u bepalen Azure wilt (op Windows, wordt dit een Linux-distributie uitgevoerd als een virtuele machine)
* De Azure CLI Docker-opdrachten gebruiken om u te maken van een VM-Docker-host in Azure
* Gebruik de lokale Docker-opdrachten voor het beheren van uw Docker-containers in uw Docker-virtuele machine in Azure.

### <a name="install-the-azure-command-line-interface-azure-cli"></a>Installeer de Azure Command-Line Interface (Azure CLI)
Als u wilt installeren en configureren van de Azure CLI, Zie [het installeren van de Azure-opdrachtregelinterface](../../../cli-install-nodejs.md). Typ het volgende om door te gaan met de installatie, `azure` bij de opdrachtprompt en na een korte ogenblik u de Azure CLI-ASCII-illustratie ziet waarin de basisopdrachten voor u beschikbaar. Als de installatie correct werkt, moet u kunnen naar het type `azure help vm` en ziet dat een van de lijst met opdrachten 'docker'.

> [!NOTE]
> Docker bevat hulpprogramma's voor Windows, [Docker-Machine](https://docs.docker.com/installation/windows/), die u kunt ook gebruiken om het maken van een docker-client die u gebruiken kunt om te werken met Azure Virtual machines als docker-hosts te automatiseren.
> 
> 

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>De Azure CLI om te verbinden met uw Azure-Account
Voordat u kunt de Azure CLI gebruiken moet u de referenties van uw Azure-account koppelen aan de Azure CLI op uw platform. De sectie [verbinding maken met uw Azure-abonnement](/cli/azure/authenticate-azure-cli) wordt uitgelegd hoe u downloaden en te importeren uw **.publishsettings** bestand of om uw Azure-CLI koppelen aan een organisatie-id.

> [!NOTE]
> Er zijn bepaalde verschillen in het gedrag wanneer u een of de andere methoden voor verificatie, dus Lees het document hierboven om te begrijpen van de verschillende functionaliteit.
> 
> 

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Docker installeren en gebruiken van de Docker-VM-extensie voor Azure
Ga als volgt de [Docker-installatie-instructies](https://docs.docker.com/installation/#installation) Docker lokaal installeren op uw computer.

Voor het gebruik van Docker met een virtuele Machine van Azure, de installatiekopie van het Linux gebruikt voor de virtuele machine over de [Azure Linux VM-Agent](../../extensions/agent-linux.md) geïnstalleerd. Er zijn momenteel slechts twee typen installatiekopieën die deze:

* Een installatiekopie van een virtuele Ubuntu uit de galerie van Azure-installatiekopie of
* Een aangepaste installatiekopie van Linux die u hebt gemaakt met de Azure Linux VM-Agent geïnstalleerd en geconfigureerd. Zie [Azure Linux VM-Agent](../../extensions/agent-linux.md) voor meer informatie over het bouwen van een aangepaste Linux VM met de Azure VM-Agent.

### <a name="using-the-azure-image-gallery"></a>Met behulp van de installatiekopie-galerie van Azure
Vanuit een Bash of Terminal-sessie, kunt u de volgende Azure CLI-opdracht gebruiken de installatiekopie van het meest recente Ubuntu vinden in de galerie met virtuele machine moet worden gebruikt door op te geven

`azure vm image list | grep Ubuntu-14_04`

en selecteer een van de namen van de installatiekopie, zoals `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, en gebruik de volgende opdracht voor het maken van een nieuwe virtuele machine met behulp van de installatiekopie.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Waarbij:

* *&lt;de naam van de VM-cloudservice&gt;*  is de naam van de virtuele machine die als de hostcomputer van de Docker-container in Azure
* *&lt;gebruikersnaam&gt;*  wordt de gebruikersnaam van de standaardgebruiker voor de hoofdmap van de virtuele machine
* *&lt;wachtwoord&gt;*  is het wachtwoord van de *gebruikersnaam* account dat voldoet aan de normen van complexiteit voor Azure

> [!NOTE]
> Op dit moment een wachtwoord moet ten minste 8 tekens, één kleine letter en één hoofdletter, cijfer en speciaal teken zoals een van de volgende tekens bevatten: `!@#$%^&+=`. Nee, de punt aan het einde van de voorgaande zin is niet een speciaal teken.
> 
> 

Als de opdracht voltooid is, ziet u ongeveer het volgende, afhankelijk van de precieze argumenten en opties die u gebruikt:

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> Maken van een virtuele machine, kan een paar minuten duren, maar nadat deze zijn ingericht (de waarde van de status is `ReadyRole`) de Docker-daemon (de Docker-service) wordt gestart en u kunt verbinding maken met de Docker-container-host.
> 
> 

Als u wilt testen van de Docker-virtuele machine die u hebt gemaakt in Azure, type

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

waar *&lt;vm-naam-u-gebruikt&gt;* is de naam van de virtuele machine die u in de aanroep van gebruikt `azure vm docker create`. U ziet er ongeveer uit als de volgende, waarmee wordt aangegeven dat uw virtuele machine Docker-Host actief is en worden uitgevoerd in Azure en wachten op de opdrachten. 

Nu u proberen kunt verbinding maken via de docker-client om informatie te verkrijgen (in sommige Docker-client-instellingen, zoals die op de Mac, u mogelijk gebruik van `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Net als u zeker weet dat u alle werken, kunt u de virtuele machine voor de Docker-extensie controleren:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker-verificatie voor Webtoepassingshost VM
Naast het maken van de VM Docker de `azure vm docker create` opdracht maakt u ook automatisch de benodigde certificaten zodat de Docker-client-computer verbinding maken met de Azure-containerhost via HTTPS en de certificaten zijn opgeslagen op de client en de host-machines, naar gelang van toepassing. Op daaropvolgende pogingen worden gedaan, worden de bestaande certificaten opnieuw gebruikt en gedeeld met de nieuwe host.

Standaard certificaten zijn geplaatst `~/.docker`, en Docker worden geconfigureerd om te worden uitgevoerd op poort **2376**. Als u wilt gebruiken een andere poort of een map, moet u een van de volgende `azure vm docker create` opdrachtregelopties voor het configureren van uw Docker-container hosten VM voor gebruik van een andere poort of verschillende certificaten voor verbindingen met clients:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

De Docker-daemon op de host is geconfigureerd om te luisteren naar en verifiëren van clientverbindingen op de opgegeven poort met behulp van de certificaten die worden gegenereerd door de `azure vm docker create` opdracht. De clientcomputer moet deze certificaten toegang te krijgen tot de Docker-host hebben.

> [!NOTE]
> Een netwerkhost uitgevoerd zonder deze certificaten worden iedereen die u kunt verbinding maken met de machine. Voordat u de standaardconfiguratie wijzigt, zorg ervoor dat u de risico's voor uw computers en toepassingen begrijpt.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Bent u klaar om te gaan naar de [Docker-gebruikershandleiding] en de Docker-VM te gebruiken. Zie voor informatie over het maken van een VM Docker-functionaliteit in de nieuwe portal [het gebruik van de Docker-VM-extensie met de Portal].
* De virtuele machine in Azure Docker-extensie ondersteunt ook Docker Compose dat gebruikmaakt van een declaratieve YAML-bestand voor een toepassing developer gemodelleerd nemen in een omgeving en het genereren van een consistente implementatie. Zie [aan de slag met Docker en opstellen om te definiëren en een toepassing met meerdere container uitvoert op een virtuele machine van Azure].  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How to use the Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]:../storage-whatis-account.md
[het gebruik van de Docker-VM-extensie met de Portal]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker-gebruikershandleiding]:https://docs.docker.com/userguide/

[aan de slag met Docker en opstellen om te definiëren en een toepassing met meerdere container uitvoert op een virtuele machine van Azure]:../docker-compose-quickstart.md
