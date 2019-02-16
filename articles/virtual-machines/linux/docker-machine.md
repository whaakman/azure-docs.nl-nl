---
title: Docker Machine gebruiken op Linux-hosts in Azure maken | Microsoft Docs
description: Beschrijft hoe u Docker Machine gebruiken om te maken van Docker-hosts in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: da0e393dc2ae0d93ecc49745a42ffac4669ed74b
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326303"
---
# <a name="how-to-use-docker-machine-to-create-hosts-in-azure"></a>Docker Machine gebruiken om hosts te maken in Azure
Dit artikel wordt uitgelegd hoe u [Docker Machine](https://docs.docker.com/machine/) om hosts te maken in Azure. De `docker-machine` opdracht maakt u een Linux virtuele machine (VM) in Azure en Docker installeert. Vervolgens kunt u uw Docker-hosts in Azure met behulp van de dezelfde lokale hulpprogramma's en werkstromen beheren. Voor het gebruik van docker-machine in Windows 10, moet u de Linux-bash gebruiken.

## <a name="create-vms-with-docker-machine"></a>Virtuele machines maken met Docker-Machine
Eerst moet u uw Azure-abonnement-ID met [az account show](/cli/azure/account) als volgt:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

U Docker-host-VM's maken in Azure met `docker-machine create` door op te geven *azure* als het stuurprogramma. Zie voor meer informatie de [Docker Azure-stuurprogramma-documentatie](https://docs.docker.com/machine/drivers/azure/)

Het volgende voorbeeld wordt een virtuele machine met de naam *myVM*, op basis van 'Standard D2 v2' plan, maakt u een gebruikersaccount met de naam *azureuser*, en opent u poort *80* op de host-VM. Volg de prompts Meld u aan bij uw Azure-account en het verlenen van machtigingen voor Docker-Machine maken en beheren van resources.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-size "Standard_DS2_v2" \
    myvm
```

De uitvoer lijkt op het volgende voorbeeld:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvm) Completed machine pre-create checks.
Creating machine...
(myvm) Querying existing resource group.  name="docker-machine"
(myvm) Creating resource group.  name="docker-machine" location="westus"
(myvm) Configuring availability set.  name="docker-machine"
(myvm) Configuring network security group.  name="myvm-firewall" location="westus"
(myvm) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvm) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvm) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvm) Creating public IP address.  name="myvm-ip" static=false
(myvm) Creating network interface.  name="myvm-nic"
(myvm) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvm) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvm
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env myvm
```

## <a name="configure-your-docker-shell"></a>Configureren van uw Docker-shell
Voor verbinding met uw Docker-host in Azure, de relevante verbinding-instellingen te definiëren. Als aan het einde van de uitvoer die u hebt genoteerd, als volgt de verbindingsgegevens voor uw Docker-host weergeven: 

```bash
docker-machine env myvm
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env myvm)
```

Voor het definiëren van de instellingen van de verbinding, kunt u de van de voorgestelde-configuratieopdracht uitvoeren (`eval $(docker-machine env myvm)`), of kunt u handmatig de omgevingsvariabelen instellen. 

## <a name="run-a-container"></a>Uitvoeren van een container
Om te zien van een container in actie, kunt een eenvoudige NGINX-webserver worden uitgevoerd. Maak een container met `docker run` en poort 80 voor webverkeer te genereren als volgt weer te geven:

```bash
docker run -d -p 80:80 --restart=always nginx
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

Weergave actieve containers met `docker ps`. De volgende voorbeelduitvoer ziet u de NGINX-container die wordt uitgevoerd met poort 80 weergegeven:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-the-container"></a>De container testen
Het openbare IP-adres van de Docker-host als volgt verkrijgen:


```bash
docker-machine ip myvm
```

Als u wilt zien van de container in actie, open een webbrowser en voer het openbare IP-adres die u hebt genoteerd in de uitvoer van de voorgaande opdracht:

![Ngnix-container die wordt uitgevoerd](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden over het gebruik van Docker Compose [aan de slag met Docker en Compose in Azure](docker-compose-quickstart.md).
