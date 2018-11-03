---
title: Docker-hosts in Azure met Docker-Machine maken | Microsoft Docs
description: Beschrijving van het gebruik van Docker Machine aan docker-hosts maken in Azure.
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: ''
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 98b2d76f4b8ba9ba8316f3a33fd2ff5ed5899833
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969529"
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Docker-hosts in Azure maken met Docker-Machine
Met [Docker](https://www.docker.com/) containers is vereist voor een virtuele machine met de docker-daemon-host.
In dit onderwerp wordt beschreven hoe u de [docker-machine](https://docs.docker.com/machine/) opdracht voor het maken van nieuwe Linux-VM's, geconfigureerd met de Docker-daemon worden uitgevoerd in Azure. 

**Opmerking:** 

* *In dit artikel is afhankelijk van de op de versie van de docker-machine 0.9.0-rc2 of hoger*
* *Windows-Containers wordt via docker-machine in de nabije toekomst worden ondersteund*

## <a name="create-vms-with-docker-machine"></a>Virtuele machines maken met Docker-Machine
Docker hosten van virtuele machines in Azure maken met de `docker-machine create` opdracht met behulp van de `azure` stuurprogramma. 

De Azure-stuurprogramma is vereist voor uw abonnements-ID. U kunt de [Azure CLI](cli-install-nodejs.md) of de [Azure Portal](https://portal.azure.com) om op te halen van uw Azure-abonnement. 

**Met behulp van de Azure Portal**

* Selecteer **abonnementen** in het linkernavigatiedeelvenster en kopieer de abonnements-id.

**Azure CLI gebruiken**

* Type ```azure account list``` en kopieer de abonnements-id.

Type `docker-machine create --driver azure` om te zien van de opties en hun standaardwaarden.
U ziet ook de [Docker Azure-stuurprogramma documentatie](https://docs.docker.com/machine/drivers/azure/) voor meer informatie. 

Het volgende voorbeeld is afhankelijk van de [standaardwaarden](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), maar deze waarden eventueel ingesteld: 

* Azure-dns voor de naam die is gekoppeld aan het openbare IP-adres en de certificaten die zijn gegenereerd. Dit is de DNS-naam van uw virtuele machine. De virtuele machine kan vervolgens veilig stoppen, de dynamische IP-Adressen vrijgeven en bieden de mogelijkheid om te herstellen nadat de virtuele machine opnieuw is gestart met een nieuwe IP-adres. Voorvoegsel van de naam moet uniek zijn voor die regio UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.
* Open poort 80 op de virtuele machine voor uitgaande toegang tot internet
* grootte van de virtuele machine sneller premium-opslag gebruiken
* premiumopslag die wordt gebruikt voor de vm-schijf

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Kies een docker-host met docker-machine
Als er een vermelding in docker-machine voor de host, kunt u de standaard-host instellen bij het uitvoeren van docker-opdrachten.

## <a name="using-powershell"></a>PowerShell gebruiken
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Met behulp van Bash
```bash
eval $(docker-machine env MyDockerHost)
```

U kunt nu een docker-opdrachten uitvoeren op basis van de opgegeven host

```
docker ps
docker info
```

## <a name="run-a-container"></a>Uitvoeren van een container
U kunt nu een eenvoudige webserver om te testen of uw host correct is geconfigureerd met een host is geconfigureerd, uitvoeren.
Hier wordt een standaard nginx-installatiekopie gebruiken, geeft u het moet luisteren op poort 80, en als de host-VM opnieuw wordt opgestart, wordt de container wordt opnieuw gestart ook (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

De uitvoer moet er ongeveer als volgt uit:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>De container testen
Bekijk actieve containers met behulp van `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

En ziet u de actieve container `docker-machine ip <VM name>` zoeken naar het IP-adres in te voeren in de browser:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Ngnix-container die wordt uitgevoerd](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Samenvatting
Met docker-machine, kunt u docker-hosts in Azure eenvoudig inrichten voor uw validaties afzonderlijke docker-host.
Voor productie hosten van containers, Zie de [Azure Container Service](http://aka.ms/AzureContainerService)

Zie voor het ontwikkelen van .NET Core-toepassingen met Visual Studio, [Docker-hulpprogramma's voor Visual Studio](http://aka.ms/DockerToolsForVS)

