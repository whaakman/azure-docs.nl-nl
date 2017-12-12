---
title: Met behulp van Docker VM-extensie voor Linux | Microsoft Docs
description: Beschrijft Docker en de virtuele Machines van Azure-extensies en het maken van Azure Virtual Machines die zijn docker-hosts met de Azure CLI in het klassieke implementatiemodel.
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
ms.openlocfilehash: f5d6bb066f9cee0d072ddb3129867cefefe3e66f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="using-the-docker-vm-extension-with-the-azure-portal"></a>Met behulp van de Docker-VM-extensie met de Azure-portal
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

[Docker](https://www.docker.com/) is een van de meest populaire virtualisatie-methoden die gebruikmaakt van [Linux containers](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines als een manier om gegevens te isoleren en berekeningen op gedeelde bronnen. U kunt de Docker-VM-extensie die worden beheerd door [Azure Linux Agent] voor het maken van een Docker-virtuele machine die als host fungeert voor een onbeperkt aantal containers voor uw toepassingen in Azure.

> [!NOTE]
> In dit onderwerp wordt beschreven hoe een Docker-virtuele machine maken vanuit de Azure-portal. Zie voor het maken van een Docker-virtuele machine op de opdrachtregel [het gebruik van de Docker-VM-extensie met de Azure-opdrachtregelinterface (Azure CLI)]. Zie voor een hoog niveau bespreking van containers en hun voordelen de [Docker hoog niveau Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).
> 
> 

## <a name="create-a-new-vm-from-the-image-gallery"></a>Een nieuwe virtuele machine uit de galerie installatiekopie maken
De eerste stap is vereist voor een Azure-VM uit een Linux-installatiekopie die ondersteuning biedt voor de Docker VM-extensie met een installatiekopie van een virtuele Ubuntu 14.04 TNS uit de galerie installatiekopie als een serverinstallatiekopie voorbeeld en Ubuntu 14.04 bureaublad als een client. Klik in de portal op **+ nieuw** voor het maken van een nieuw exemplaar van de virtuele machine en selecteer een installatiekopie van een virtuele Ubuntu 14.04 TNS van de beschikbare selecties of van de volledige installatiekopie-galerie, zoals hieronder wordt weergegeven.

> [!NOTE]
> Op dit moment ondersteuning alleen Ubuntu 14.04 TNS afbeeldingen recenter dan juli 2014 voor de Docker-VM-extensie.
> 
> 

![Maak een nieuwe Ubuntu-afbeelding](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Docker-certificaten maken
Nadat de virtuele machine is gemaakt, zorg ervoor dat de Docker is geïnstalleerd op de clientcomputer. (Zie voor meer informatie [Docker-installatie-instructies](https://docs.docker.com/installation/#installation).)

Maken van de bestanden van het certificaat en de sleutel voor Docker-communicatie volgens [Docker uitgevoerd met https] en plaats deze in de  **`~/.docker`**  map op de clientcomputer.

> [!NOTE]
> De Docker-VM-extensie in de portal op dit moment zijn referenties nodig met base64-gecodeerd zijn.
> 
> 

Op de opdrachtregel gebruiken  **`base64`**  of een ander favoriete codering hulpprogramma voor het maken van onderwerpen base64-gecodeerd. Dit uit te voeren met een eenvoudige reeks certificaat en de sleutel bestanden kan er ongeveer als volgt:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>De Docker-VM-extensie toevoegen
De Docker-VM-extensie toevoegen: zoek het VM-exemplaar dat u hebt gemaakt en schuif omlaag naar **extensies** en klikt u op het online zetten van de VM-extensies, zoals hieronder wordt weergegeven.

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a>Toevoegen van een uitbreiding
Klik op de **+ toevoegen** om mogelijke VM-extensies die u aan deze virtuele machine toevoegen kunt weer te geven.

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-the-docker-vm-extension"></a>Selecteer de Docker-VM-extensie
Kies de Docker VM-extensie, die ervoor zorgt dat de Docker-beschrijving en belangrijke koppelingen, en klik vervolgens op **maken** onderaan om te beginnen met de installatieprocedure.

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a>Het certificaat en de sleutelbestanden toevoegen:
Voer in de velden van het formulier de base64-gecodeerd versies van uw CA-certificaat, een servercertificaat en de sleutel van uw Server, zoals wordt weergegeven in de volgende afbeelding.

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> Houd er rekening mee dat (zoals in de voorgaande afbeelding) de 2376 wordt gevuld standaard. U kunt een willekeurig eindpunt hier invoeren, maar worden de volgende stap om het overeenkomende eindpunt te openen. Als u de standaardwaarde wijzigt, zorg ervoor dat u opent u de overeenkomende eindpunt in de volgende stap.
> 
> 

## <a name="add-the-docker-communication-endpoint"></a>De Docker-communicatie-eindpunt toevoegen
Wanneer weergeven van de resourcegroep die u hebt gemaakt, selecteert u de Netwerkbeveiligingsgroep die zijn gekoppeld aan uw virtuele machine en klik op **regels voor binnenkomende beveiliging** om weer te geven van de regels, zoals hier wordt weergegeven.

![](media/portal-use-docker/AddingEndpoint.png)

Klik op **+ toevoegen** voor het toevoegen van een andere regel en geef een naam voor het eindpunt in de standaard-case (in dit voorbeeld **Docker**), en 2376 'Doelpoortbereik'. Stel de waarde protocol weergeeft **TCP**, en klik op **OK** om de regel te maken.

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a>Test uw Docker-Client en de Azure Docker-Host
Zoek en kopieer de naam van uw VM-domein en op de opdrachtregel van de clientcomputer, het type `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (waarbij *dockerextension* is vervangen door het subdomein voor uw virtuele machine).

Het resultaat ziet er ongeveer als volgt:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Nadat u de bovenstaande stappen hebt voltooid, hebt u nu een volledig functionele Docker-host waarop een virtuele machine van Azure, geconfigureerd om te worden verbonden met op afstand van andere clients.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
Bent u klaar om te gaan naar de [Docker-gebruikershandleiding] en de Docker-VM te gebruiken. Als u automatiseren maken Docker-hosts op Azure VM's via de opdrachtregelinterface wilt, Zie [het gebruik van de Docker-VM-extensie met de Azure-opdrachtregelinterface (Azure CLI)]

<!--Anchors-->
[Create a new VM from the Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add the Docker VM Extension]:#adddockerextension
[Test Docker Client and Azure Docker Host]:#testclientandserver
[Next steps]:#next-steps

<!--Image references-->
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[6]:./media/markdown-template-for-new-articles/pretty49.png
[7]:./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[het gebruik van de Docker-VM-extensie met de Azure-opdrachtregelinterface (Azure CLI)]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Agent]:../agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]:../storage-whatis-account.md

[Docker uitgevoerd met https]:http://docs.docker.com/articles/https/
[Docker-gebruikershandleiding]:https://docs.docker.com/userguide/
