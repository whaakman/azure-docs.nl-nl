---
title: Ansible gebruiken voor het beheren van uw Azure dynamische voorraden
description: Leer hoe u Ansible gebruikt voor het beheren van uw Azure dynamische voorraden
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, dynamische voorraad
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 08/09/2018
ms.topic: article
ms.openlocfilehash: 1f19d5918d81acb76936edf8989a556335a3c0df
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261253"
---
# <a name="use-ansible-to-manage-your-azure-dynamic-inventories"></a>Ansible gebruiken voor het beheren van uw Azure dynamische voorraden
Ansible kan worden gebruikt voor het ophalen van inventarisatie-informatie uit verschillende bronnen (met inbegrip van cloudbronnen zoals Azure) in een *dynamische voorraad*. In dit artikel gebruikt u de [Azure Cloud Shell](./ansible-run-playbook-in-cloudshell.md) label een van deze virtuele machines voor het configureren van het dynamische voorraad van een Ansible Azure waarin u twee virtuele machines maakt, en installeer Nginx op de gelabelde virtuele machine.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Azure-referenties** - [maken-Azure-referenties en Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-the-test-virtual-machines"></a>De virtuele testmachines maken

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Open [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

1. Maak een Azure-resourcegroep voor het opslaan van de virtuele machines voor deze zelfstudie.

    > [!IMPORTANT]  
    > De Azure-resourcegroep die in deze stap u maakt moet een naam die is volledig kleine hebben. Anders mislukt de generatie van de dynamische voorraad.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Maak twee virtuele Linux-machines op Azure met een van de volgende technieken:

    - **Ansible-playbook** -het artikel [een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm) ziet u hoe u een virtuele machine maken van een Ansible-playbook. Als u een playbook voor het definiëren van een of beide van de virtuele machines gebruikt, zorgt u ervoor dat de SSH-verbinding wordt gebruikt in plaats van een wachtwoord.

    - **Azure CLI** -probleem elk van de volgende in de Cloud Shell opdrachten voor het maken van de twee virtuele machines:

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm1 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm2 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

## <a name="tag-a-virtual-machine"></a>Een virtuele machine taggen
U kunt [tags gebruiken om uw Azure-resources te organiseren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) door gebruiker gedefinieerde categorieën. 

Voer de volgende [az resource tag](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) opdracht voor het taggen van de virtuele machine `ansible-inventory-test-vm1` met de sleutel `nginx`:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Genereren van een dynamische voorraad
Zodra u uw virtuele machines hebt gedefinieerd (en gemarkeerde), is het tijd voor het genereren van de dynamische voorraad. Ansible biedt een Python-script met de naam [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) die een dynamische voorraad van uw Azure-resources worden gegenereerd door API-aanvragen naar de Azure Resource Manager. De volgende stappen helpen u bij met behulp van de `azure_rm.py` script verbinding maken met de twee virtuele machines van Azure testen:

1. Gebruik de GNU `wget` opdracht voor het ophalen van de `azure_rm.py` script:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Gebruik de `chmod` opdracht om de machtigingen voor toegang tot de `azure_rm.py` script. De volgende opdracht maakt gebruik van de `+x` parameter om toe te staan voor de uitvoering (die wordt uitgevoerd) van het opgegeven bestand (`azure_rm.py`):

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Gebruik de [ansible opdracht](https://docs.ansible.com/ansible/2.4/ansible.html) verbinding maken met de resourcegroep: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Zodra de verbinding is gemaakt, ziet u resultaat is vergelijkbaar met de volgende uitvoer:

    ```Output
    ansible-inventory-test-vm1 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ansible-inventory-test-vm2 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ```

## <a name="enable-the-virtual-machine-tag"></a>De code van de virtuele machine inschakelen
Nadat u de gewenste tag hebt ingesteld, moet u de tag 'inschakelen'. Eén manier om in te schakelen van een label wordt door de tag te exporteren naar een omgevingsvariabele genoemd `AZURE_TAGS` via de **exporteren** opdracht:

```azurecli-interactive
export AZURE_TAGS=nginx
```

Zodra de tag is geëxporteerd, kunt u proberen de `ansible` opdracht opnieuw uit:

```azurecli-interactive
ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
```

Nu ziet u slechts één virtuele machine (met de tag overeenkomt met de waarde die wordt geëxporteerd naar de **AZURE_TAGS** omgevingsvariabele):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Nginx op de gelabelde virtuele machine instellen
Het doel van labels is om in te schakelen van de mogelijkheid snel en eenvoudig werken met subgroepen van uw virtuele machines. Bijvoorbeeld: Stel dat u wilt om Nginx te installeren op virtuele machines waarop u een tag van hebt toegewezen `nginx`. De volgende stappen laten zien hoe gemakkelijk dit is om uit te voeren:

1. Maak een bestand (om de playbook) met de naam `nginx.yml` als volgt:

  ```azurecli-interactive
  vi nginx.yml
  ```

1. Voeg de volgende code in het zojuist gemaakte `nginx.yml` bestand:

    ```yml
    ---
    - name: Install and start Nginx on an Azure virtual machine
    hosts: azure
    become: yes
    tasks:
    - name: install nginx
      apt: pkg=nginx state=installed
      notify:
      - start nginx

    handlers:
    - name: start nginx
      service: name=nginx state=started
    ```

1. Voer de `nginx.yml` playbook:

    ```azurecli-interactive
    ansible-playbook -i azure_rm.py nginx.yml
    ```

1. Nadat u de playbook hebt uitgevoerd, ziet u de resultaten die vergelijkbaar is met de volgende uitvoer:

    ```Output
    PLAY [Install and start Nginx on an Azure virtual machine] **********

    TASK [Gathering Facts] **********
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] **********
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] **********
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP **********
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Nginx-installatie testen
In deze sectie ziet u een techniek als u wilt testen dat Nginx is geïnstalleerd op uw virtuele machine.

1. Gebruik de [az vm list-ip-adressen](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) opdracht voor het ophalen van de IP-adres van de `ansible-inventory-test-vm1` virtuele machine. De geretourneerde waarde (IP-adres van de virtuele machine) wordt vervolgens gebruikt als de parameter voor de SSH-opdracht verbinding maken met de virtuele machine.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. Tijdens verbinding met de `ansible-inventory-test-vm1` virtuele machine, voer de [nginx - v](https://nginx.org/en/docs/switches.html) opdracht om te bepalen als Nginx is geïnstalleerd.

    ```azurecli-interactive
    nginx -v
    ```

1. Zodra u uitvoert het `nginx -v` opdracht, ziet u de Nginx-versie (tweede regel) die aangeeft dat Nginx is geïnstalleerd.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)
    
    tom@ansible-inventory-test-vm1:~$
    ```

1. Druk op de  **&lt;Ctrl > D** combinatie om de SSH-sessie te verbreken op het toetsenbord.

1. Uitvoeren van de voorgaande stappen voor het `ansible-inventory-test-vm2` virtuele machine levert een informatief bericht waarmee wordt aangegeven waar vind u Nginx (Dit betekent dat u deze nog niet geïnstalleerd op dit moment):

    ```Output
    tom@ansible-inventory-test-vm2:~$ nginx -v
    The program 'nginx' can be found in the following packages:
    * nginx-core
    * nginx-extras
    * nginx-full
    * nginx-lightTry: sudo apt install <selected package>
    tom@ansible-inventory-test-vm2:~$
    ```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm)
