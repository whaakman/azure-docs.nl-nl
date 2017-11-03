---
title: Docker-hosts maken in Azure met Docker-Machine | Microsoft Docs
description: Beschrijving van de Machine Docker docker-hosts maken in Azure.
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 766d327a87ed13e04166d71c3d9ae0a1e7a66d19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Docker-hosts in Azure maken met Docker-Machine
Met [Docker](https://www.docker.com/) containers vereist een host met de docker-daemon VM.
In dit onderwerp wordt beschreven hoe u de [docker-machine](https://docs.docker.com/machine/) opdracht voor het maken van nieuwe virtuele Linux-machines, geconfigureerd met de Docker-daemon worden uitgevoerd in Azure. 

**Opmerking:** 

* *In dit artikel hangt samen met docker-machine versie 0.9.0-rc2 of hoger*
* *Windows-Containers worden ondersteund via de docker-machine in de nabije toekomst*

## <a name="create-vms-with-docker-machine"></a>Virtuele machines met Docker-Machine maken
Het maken van de docker host virtuele machines in Azure met de `docker-machine create` opdracht met behulp van de `azure` stuurprogramma. 

De Azure-stuurprogramma is vereist voor uw abonnement-ID. U kunt de [Azure CLI](cli-install-nodejs.md) of de [Azure Portal](https://portal.azure.com) uw Azure-abonnement ophalen. 

**Met behulp van de Azure-Portal**

* Selecteer **abonnementen** uit het linkernavigatiedeelvenster en kopieer de abonnements-id.

**Azure CLI gebruiken**

* Type ```azure account list``` en kopieer de abonnements-id.

Type `docker-machine create --driver azure` om te zien van de opties en hun standaardwaarden.
U ziet ook de [Docker Azure stuurprogramma documentatie](https://docs.docker.com/machine/drivers/azure/) voor meer informatie. 

Het volgende voorbeeld is afhankelijk van de [standaardwaarden](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), maar eventueel deze waarden ingesteld: 

* Azure dns voor de naam die is gekoppeld aan het openbare IP-adres en de certificaten die zijn gegenereerd. Dit is de DNS-naam van uw virtuele machine. De virtuele machine kan vervolgens veilig stoppen, release van het dynamische IP-adres en bieden de mogelijkheid nadat de virtuele machine opnieuw is gestart met een nieuwe IP-verbinding te maken. Voorvoegsel van de naam moet uniek zijn voor deze regio UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.
* Open poort 80 op de virtuele machine voor uitgaande internetverbinding
* grootte van de virtuele machine te kunnen gebruiken sneller premium-opslag
* Premium-opslag gebruikt voor de vm-schijf

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Kies een host docker met docker-machine
Zodra u een vermelding in de docker-machine voor de host hebt, kunt u de standaardhost kunt instellen wanneer docker opdrachten uit te voeren.

## <a name="using-powershell"></a>PowerShell gebruiken
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Met behulp van Bash
```bash
eval $(docker-machine env MyDockerHost)
```

U kunt nu docker-opdrachten uitvoeren op de opgegeven host

```
docker ps
docker info
```

## <a name="run-a-container"></a>Een container worden uitgevoerd
U kunt nu een eenvoudige webserver om te controleren of uw host correct is geconfigureerd met een host die is geconfigureerd, uitvoeren.
Hier wordt een installatiekopie van het standaard nginx gebruiken, geeft u het moet luisteren op poort 80, en als de host-VM opnieuw wordt opgestart, wordt de container wordt opnieuw gestart ook (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

De uitvoer ziet er ongeveer als volgt:

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
Bekijk actieve containers met `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

En ziet u de actieve container `docker-machine ip <VM name>` zoeken naar het IP-adres invoeren in de browser:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Actieve ngnix container](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Samenvatting
U kunt eenvoudig docker-hosts in Azure voor uw afzonderlijke docker host validaties met docker-machine inrichten.
Voor productie hosting van containers, Zie de [Azure Container Service](http://aka.ms/AzureContainerService)

Om te zetten met Visual Studio .NET Core-toepassingen, Zie [Docker-Tools voor Visual Studio](http://aka.ms/DockerToolsForVS)

