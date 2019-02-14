---
title: Ansible installeren op virtuele Azure-machines
description: Meer informatie over het installeren en configureren van Ansible voor het beheren van Azure-resources onder Ubuntu, CentOS en SLES
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: 3c57e46ebb8828cbf2e44ced95d2d37afe5671c5
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893913"
---
# <a name="install-ansible-on-azure-virtual-machines"></a>Ansible installeren op virtuele Azure-machines

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele machines (VM's) in Azure, net zoals andere resources. In dit artikel wordt in detail beschreven hoe u Ansible en de vereiste modules van Azure SDK voor Python installeert voor veelvoorkomende Linux-versies. U kunt Ansible installeren in andere versies door de geïnstalleerde pakketten aan te passen aan uw specifieke platform. Om Azure-resources op veilige wijze te maken, leert u ook hoe u referenties kunt maken en gebruiken voor Ansible. Zie [Features and tools for Bash in the Azure Cloud Shell](../../cloud-shell/features.md#tools) (functies en hulpprogramma's voor Bash in Azure Cloud Shell) voor een lijst met aanvullende beschikbare hulpprogramma's in Cloud Shell.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan.

- **Toegang tot Linux of een virtuele Linux-machine**: als u geen Linux-machine hebt, maakt u een [virtuele Linux-machine](https://docs.microsoft.com/azure/virtual-network/quick-create-cli).

- **Azure-service-principal**: volg de aanwijzingen in de sectie **De service-principal maken** in het artikel [Een Azure-service-principal maken met de Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Noteer de waarden voor **appId**, **displayName**, **password** en **tenant**.

## <a name="install-ansible-on-an-azure-linux-virtual-machine"></a>Ansible installeren op een virtuele Linux-machine in Azure

Meld u aan bij uw Linux-machine en selecteer een van de volgende versies voor instructies voor het installeren van Ansible:

- [CentOS 7.4](#centos-74)
- Ubuntu 16.04 LTS
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Installeer de vereiste pakketten voor de modules van Azure SDK voor Python en Ansible door de volgende opdrachten uit te voeren in een terminal- of Bash-venster:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Volg de instructies in de sectie [Azure-referenties maken](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Installeer de vereiste pakketten voor de modules van Azure SDK voor Python en Ansible door de volgende opdrachten uit te voeren in een terminal- of Bash-venster:


```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Volg de instructies in de sectie [Azure-referenties maken](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Installeer de vereiste pakketten voor de modules van Azure SDK voor Python en Ansible door de volgende opdrachten uit te voeren in een terminal- of Bash-venster:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Volg de instructies in de sectie [Azure-referenties maken](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Azure-referenties maken

De Ansible-referenties worden op een van de volgende twee manieren geconfigureerd op basis van de abonnements-id en de informatie die bij het maken van de service-principal wordt geretourneerd:

- [Een Ansible-referentiebestand maken](#file-credentials)
- [Ansible-omgevingsvariabelen gebruiken](#env-credentials)

Als u hulpprogramma's zoals Ansible Tower of Jenkins gaat gebruiken, moet u de waarden voor de service-principal declareren als omgevingsvariabelen.

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Ansible-referentiebestand maken

In deze sectie wordt uitgelegd hoe u een lokaal referentiebestand maakt om referenties op te geven voor Ansible. Zie [Referenties opgeven voor Azure-modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules) voor meer informatie over het definiëren van Ansible-referenties.

Voor een ontwikkelomgeving maakt u als volgt een *referentiebestand* voor Ansible op uw virtuele hostmachine:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Voeg de volgende regels in het *referentiebestand* in, waarbij u de tijdelijke aanduidingen vervangt door de informatie die u hebt verkregen bij het maken van de service-principal.

```bash
[default]
subscription_id=<your-subscription_id>
client_id=<security-principal-appid>
secret=<security-principal-password>
tenant=<security-principal-tenant>
```

Sla het bestand op en sluit het.

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>Ansible-omgevingsvariabelen gebruiken

In deze sectie wordt uitgelegd hoe u de Ansible-referenties configureert door deze te exporteren als omgevingsvariabelen.

Voer de volgende opdrachten uit in een terminal- of Bash-venster:

```bash
export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
export AZURE_CLIENT_ID=<security-principal-appid>
export AZURE_SECRET=<security-principal-password>
export AZURE_TENANT=<security-principal-tenant>
```

## <a name="verify-the-configuration"></a>De configuratie controleren
Als u wilt controleren of het configureren geslaagd is, kunt u nu een resourcegroep maken met Ansible.

[!INCLUDE [create-resource-group-with-ansible.md](../../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Een virtuele Linux-machine maken in Azure met Ansible](./ansible-create-vm.md)
