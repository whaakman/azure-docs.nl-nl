---
title: Snelstartgids - Ansible installeren op virtuele Linux-machines in Azure | Microsoft Docs
description: In deze snelstartgids leert u hoe u Ansible installeren en configureren voor het beheren van Azure-resources op Ubuntu-, CentOS- en SLES
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: e7a664be48d1e26e09faf4f330fd1267ec003315
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64685601"
---
# <a name="quickstart-install-ansible-on-linux-virtual-machines-in-azure"></a>Quickstart: Ansible installeren op virtuele Linux-machines in Azure

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel laat zien hoe het configureren van Ansible voor enkele van de meest voorkomende distributies van Linux. Pas de geïnstalleerde pakketten voor uw bepaald platform wilt Ansible installeren op andere distributies. 

## <a name="prerequisites"></a>Vereisten

- [!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [open-source-devops-prereqs-create-sp.md](../../../includes/open-source-devops-prereqs-create-service-principal.md)]
- **Toegang tot Linux of een virtuele Linux-machine**: als u geen Linux-machine hebt, maakt u een [virtuele Linux-machine](/azure/virtual-network/quick-create-cli).

## <a name="install-ansible-on-an-azure-linux-virtual-machine"></a>Ansible installeren op een virtuele Linux-machine in Azure

Meld u aan bij uw Linux-machine en selecteer een van de volgende versies voor instructies voor het installeren van Ansible:

- [CentOS 7.4](#centos-74)
- Ubuntu 16.04 LTS
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

In deze sectie configureert u CentOS voor het gebruik van Ansible.

1. Open een terminalvenster.

1. Voer de volgende opdracht voor het installeren van de vereiste pakketten voor de Azure Python SDK-modules:

    ```bash
    sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
    sudo yum install -y python-pip python-wheel
    ```

1. Voer de volgende opdracht om de vereiste Ansible-pakketten te installeren:

    ```bash
    sudo pip install ansible[azure]
    ```

1. [Maken van de Azure-referenties](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

In deze sectie configureert u Ubuntu voor het gebruik van Ansible.

1. Open een terminalvenster.

1. Voer de volgende opdracht voor het installeren van de vereiste pakketten voor de Azure Python SDK-modules:

    ```bash
    sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip
    ```

1. Voer de volgende opdracht om de vereiste Ansible-pakketten te installeren:

    ```bash
    sudo pip install ansible[azure]
    ```

1. [Maken van de Azure-referenties](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

In deze sectie configureert u SLES voor het gebruik van Ansible.

1. Open een terminalvenster.

1. Voer de volgende opdracht voor het installeren van de vereiste pakketten voor de Azure Python SDK-modules:

    ```bash
    sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
        python-devel libopenssl-devel libtool python-pip python-setuptools
    ```

1. Voer de volgende opdracht om de vereiste Ansible-pakketten te installeren:

    ```bash
    sudo pip install ansible[azure]
    ```

1. Voer de volgende opdracht om conflicterende Python cryptografie-pakket te verwijderen:

    ```bash
    sudo pip uninstall -y cryptography
    ```

1. [Maken van de Azure-referenties](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Azure-referenties maken

Als u wilt de Ansible-referenties configureren, moet u de volgende informatie:

* Uw Azure-abonnement-ID 
* De waarden van service-principal

Als u Ansible Tower of Jenkins gebruikt, moet u de waarden van service-principal declareren als omgevingsvariabelen.

Configureer de Ansible-referenties met behulp van een van de volgende technieken:

- [Een Ansible-referentiebestand maken](#file-credentials)
- [Ansible-omgevingsvariabelen gebruiken](#env-credentials)

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Ansible-referentiebestand maken

In deze sectie maakt maken u een bestand lokale referenties om referenties te verstrekken aan Ansible. 

Zie voor meer informatie over het definiëren van Ansible referenties [geven referenties aan Azure-Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

1. Maak een bestand met de naam voor een ontwikkelomgeving `credentials` op de host virtuele machine:

    ```bash
    mkdir ~/.azure
    vi ~/.azure/credentials
    ```

1. Voeg de volgende regels in het bestand. Vervang de tijdelijke aanduidingen door de waarden van service-principal.

    ```bash
    [default]
    subscription_id=<your-subscription_id>
    client_id=<security-principal-appid>
    secret=<security-principal-password>
    tenant=<security-principal-tenant>
    ```

1. Sla het bestand op en sluit het.

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>Ansible-omgevingsvariabelen gebruiken

In deze sectie maakt exporteren u de service principal waarden voor het configureren van uw referenties Ansible.

1. Open een terminalvenster.

1. De waarden van service-principal exporteren:

    ```bash
    export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
    export AZURE_CLIENT_ID=<security-principal-appid>
    export AZURE_SECRET=<security-principal-password>
    export AZURE_TENANT=<security-principal-tenant>
    ```

## <a name="verify-the-configuration"></a>De configuratie controleren

Om te controleren of de juiste configuratie, Ansible te gebruiken om een Azure-resourcegroep te maken.

[!INCLUDE [create-resource-group-with-ansible.md](../../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Snelstart: Een virtuele Linux-machine in Azure met behulp van Ansible configureren](./ansible-create-vm.md)