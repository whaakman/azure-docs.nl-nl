---
title: Docker Machine gebruiken om u te maken van Linux-hosts in Azure | Microsoft Docs
description: Beschrijft hoe u Machine Docker Docker-hosts maken in Azure.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: iainfou
ms.openlocfilehash: 7772381e9796ddc2e0db215bab4f230473eaa462
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-docker-machine-to-create-hosts-in-azure"></a>Het gebruik van Docker-Machine maken van hosts in Azure
Dit artikel wordt uitgelegd hoe u [Docker-Machine](https://docs.docker.com/machine/) hosts maken in Azure. De `docker-machine` opdracht maakt u een Linux-virtuele machine (VM) in Azure vervolgens Docker geïnstalleerd. U kunt uw Docker-hosts in Azure met behulp van de dezelfde lokale hulpprogramma's en werkstromen beheren. U kunt docker-machine in Windows 10, moet u Linux-bash gebruiken.

## <a name="create-vms-with-docker-machine"></a>Virtuele machines met Docker-Machine maken
Eerst moet u uw Azure-abonnement-ID met [az account weergeven](/cli/azure/account#show) als volgt:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Maken van virtuele machines Docker-host in Azure met `docker-machine create` door te geven *azure* als het stuurprogramma. Zie voor meer informatie de [Docker Azure stuurprogramma-documentatie](https://docs.docker.com/machine/drivers/azure/)

Het volgende voorbeeld wordt een virtuele machine met de naam *myVM*, op basis van 'V2 standaard D2' plan, maakt u een gebruikersaccount met de naam *azureuser*, en poort geopend *80* op de VM-host. Volg de prompts aanmelden bij uw Azure-account en het verlenen van machtigingen voor Docker-Machine maken en beheren van resources.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-size "Standard_D2_v2 \
    myvm
```

De uitvoer lijkt op het volgende voorbeeld:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvmdocker) Completed machine pre-create checks.
Creating machine...
(myvmdocker) Querying existing resource group.  name="docker-machine"
(myvmdocker) Creating resource group.  name="docker-machine" location="westus"
(myvmdocker) Configuring availability set.  name="docker-machine"
(myvmdocker) Configuring network security group.  name="myvmdocker-firewall" location="westus"
(myvmdocker) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvmdocker) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvmdocker) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvmdocker) Creating public IP address.  name="myvmdocker-ip" static=false
(myvmdocker) Creating network interface.  name="myvmdocker-nic"
(myvmdocker) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvmdocker) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvmdocker"
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
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env myvmdocker
```

## <a name="configure-your-docker-shell"></a>De Docker-shell configureren
Voor verbinding met de Docker-host in Azure, de juiste verbinding-instellingen te definiëren. Zoals vermeld aan het einde van de uitvoer, moet weergeven de verbindingsgegevens voor de Docker-host als volgt: 

```bash
docker-machine env myvmdocker
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env myvmdocker)
```

De verbinding wilt definiëren instellingen kunt u de voorgestelde configuratieopdracht uitvoeren (`eval $(docker-machine env myvmdocker)`), of kunt u handmatig de omgevingsvariabelen instellen. 

## <a name="run-a-container"></a>Een container worden uitgevoerd
Overzicht van een container in actie, kunt een eenvoudige NGINX-webserver uitgevoerd. Maken van een container met `docker run` en weer te geven van poort 80 voor internetverkeer als volgt:

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

Weergave actieve containers met `docker ps`. De volgende voorbeelduitvoer wordt weergegeven de NGINX-container met met poort 80 weergegeven:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-the-container"></a>De container testen
Het openbare IP-adres van Docker-host als volgt verkrijgen:


```bash
docker-machine ip myvmdocker
```

Open een webbrowser om de container in actie kunt zien, en geef het openbare IP-adres hebt genoteerd in de uitvoer van de voorgaande opdracht:

![Actieve ngnix container](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Volgende stappen
U kunt ook hosts met maken de [Docker VM-extensie](dockerextension.md). Zie voor voorbeelden op met behulp van Docker Compose [aan de slag met Docker en opstellen in Azure](docker-compose-quickstart.md).
