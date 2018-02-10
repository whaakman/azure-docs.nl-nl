---
title: Voer Ansible met Bash in de Azure-Cloud-Shell
description: Meer informatie over het uitvoeren van verschillende Ansible taken met Bash in de Azure-Cloud-Shell
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 02/01/2018
ms.topic: article
ms.openlocfilehash: 0cd0390a381e85c8f047960ce06c581a433d4a2c
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Voer Ansible met Bash in de Azure-Cloud-Shell

In deze zelfstudie leert u hoe u verschillende Ansible taken uitvoeren via Bash in de Cloud-Shell. Deze taken omvatten verbinding te maken met een virtuele machine en het maken van Ansible hulpmiddelen marketing en verkoop voor het maken en verwijderen van een Azure-resourcegroep.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement** : als u hebt een Azure-abonnement maken geen een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.

- **Azure-referenties** - [maken Azure-referenties en Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

- **Configureren van Azure Cloud Shell** : als u niet bekend met Azure Cloud-Shell, het artikel bent [Quick Start voor Bash in de Azure-Cloud-Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/quickstart) ziet u hoe u starten en configureren van Cloud-Shell. Een speciale website voor Cloud-Shell Hier starten:

[![Cloud-Shell starten](https://shell.azure.com/images/launchcloudshell.png "Cloud Shell starten")](https://shell.azure.com)

## <a name="use-ansible-to-connect-to-a-vm"></a>Ansible gebruiken voor verbinding met een virtuele machine
Ansible heeft een pythonscript aangeroepen gemaakt [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) die een dynamische inventaris van uw Azure-resources wordt gegenereerd door het maken van API-aanvragen in de Azure Resource Manager. De volgende stappen maakt u met behulp van de `azure_rm.py` script verbinding maken met een virtuele machine van Azure:

1. Open Bash in de Cloud-Shell. Shell-type wordt aangegeven aan de linkerkant van het Cloud-Shell-venster.

1. Als u niet een virtuele machine te gebruiken, voert u de volgende opdrachten in de Cloud-Shell voor het maken van een virtuele machine waarmee u wilt testen:

  ```azurecli-interactive
  az group create --resource-group ansible-test-rg --location eastus
  ```

  ```azurecli-interactive
  az vm create --resource-group ansible-test-rg --name ansible-test-vm --image UbuntuLTS --generate-ssh-keys
  ```

1. Gebruik de GNU `wget` opdracht voor het ophalen van de `azure_rm.py` script:

  ```azurecli-interactive
  wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
  ```

1. Gebruik de `chmod` opdracht om de machtigingen voor toegang tot de `azure_rm.py` script. De volgende opdracht maakt gebruik van de `+x` parameter om toe te staan voor uitvoering (actief) van het opgegeven bestand (`azure_rm.py`):

  ```azurecli-interactive
  chmod +x azure_rm.py
  ```

1. Gebruik de [ansible opdracht](https://docs.ansible.com/ansible/2.4/ansible.html) verbinding maken met uw virtuele machine: 

  ```azurecli-interactive
  ansible -i azure_rm.py ansible-test-vm -m ping
  ```

  Eenmaal zijn verbonden, kunt u resultaten die vergelijkbaar is met de uitvoer ziet:

  ```Output
  The authenticity of host 'nn.nnn.nn.nn (nn.nnn.nn.nn)' can't be established.
  ECDSA key fingerprint is SHA256:&lt;some value>.
  Are you sure you want to continue connecting (yes/no)? yes
  test-ansible-vm | SUCCESS => {
      "changed": false,
      "failed": false,
      "ping": "pong"
  }
  ```

1. Als u een resourcegroep en de virtuele machine hebt gemaakt in deze sectie

  ```azurecli-interactive
  az group delete -n <resourceGroup>
  ```

## <a name="run-a-playbook-in-cloud-shell"></a>Een playbook uitvoeren in Cloud Shell
De [ansible playbook](https://docs.ansible.com/ansible/2.4/ansible-playbook.html) opdracht Ansible hulpmiddelen marketing en verkoop, de taken uitgevoerd op de betreffende host (s) wordt uitgevoerd. In deze sectie helpt u bij het gebruik van de Cloud-Shell kunnen maken en uitvoeren van de twee hulpmiddelen marketing en verkoop - Eén voor het maken van een resourcegroep en een tweede verwijderen van de resourcegroep. 

1. Maak een bestand met de naam `rg.yml` als volgt:

  ```azurecli-interactive
  vi rg.yml
  ```

1. Kopieer de volgende inhoud in de Cloud Shell-venster (nu de host van een exemplaar van de editor VI):

  ```yml
  - name: My first Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
          name: demoresourcegroup
          location: eastus
  ```

1. Sla het bestand en de VI-editor te sluiten door te voeren `:wq` en drukt u op &lt;Enter >.

1. Gebruik de `ansible-playbook` uit te voeren opdracht de `rg.yml` playbook:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Hier ziet u resultaten die vergelijkbaar is met de volgende uitvoer:

  ```Output
  PLAY [My first Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Create a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

1. Controleer of de implementatie:

  ```azurecli-interactive
  az group show -n demoresourcegroup
  ```

1. Nu dat u de resourcegroep hebt gemaakt, maakt u een tweede Ansible playbook voor het verwijderen van de resourcegroep:

  ```azurecli-interactive
  vi rg2.yml
  ```

1. Kopieer de volgende inhoud in de Cloud Shell-venster (nu de host van een exemplaar van de editor VI):

  ```yml
  - name: My second Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
          name: demoresourcegroup
          state: absent
  ```

1. Sla het bestand en de VI-editor te sluiten door te voeren `:wq` en drukt u op &lt;Enter >.

1. Gebruik de `ansible-playbook` uit te voeren opdracht de `rg2.yml` playbook:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Hier ziet u resultaten die vergelijkbaar is met de volgende uitvoer:

  ```Output
  The output is as following. 
  PLAY [My second Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Delete a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm)
