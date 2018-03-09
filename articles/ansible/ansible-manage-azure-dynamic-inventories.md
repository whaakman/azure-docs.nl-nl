---
title: Ansible gebruiken voor het beheren van uw Azure dynamische voorraden
description: Informatie over het gebruik van Ansible voor het beheren van uw Azure dynamische voorraden
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, dynamische inventarisatie
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/14/2018
ms.topic: article
ms.openlocfilehash: 799be6d2bb521de38af952376bf8ee14a18846de
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="use-ansible-to-manage-your-azure-dynamic-inventories"></a>Ansible gebruiken voor het beheren van uw Azure dynamische voorraden
Ansible kan worden gebruikt voor het pull-inventarisatie-informatie uit diverse bronnen (met inbegrip van cloud-bronnen zoals Azure) in een *dynamische inventaris*. In dit artikel, gebruikt u de [Azure Cloud Shell](./ansible-run-playbook-in-cloudshell.md) tag een van deze virtuele machines voor het configureren van een Ansible Azure dynamische inventarisatie waarin u twee virtuele machines maken en Nginx installeren op de gemarkeerde virtuele machine.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement** : als u hebt een Azure-abonnement maken geen een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.

- **Azure-referenties** - [maken Azure-referenties en Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-the-test-virtual-machines"></a>De test-virtuele machines maken

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Open [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

1. Maak een Azure-resourcegroep voor het opslaan van de virtuele machines voor deze zelfstudie.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Twee virtuele Linux-machines in Azure met behulp van een van de volgende technieken maken:

    - **Ansible playbook** -het artikel [een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm) ziet u hoe u een virtuele machine maken vanuit een playbook Ansible. Als u een playbook voor het definiëren van een of beide van de virtuele machines gebruikt, moet u zorgen dat de SSH-verbinding wordt gebruikt in plaats van een wachtwoord.

    - **Azure CLI** -probleem elk van de volgende in de Cloud-Shell opdrachten de twee virtuele machines maken:

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

## <a name="tag-a-virtual-machine"></a>Labels van een virtuele machine
U kunt [codes gebruiken om uw Azure-resources te organiseren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) door gebruiker gedefinieerde categorieën. 

Voer de volgende [az resource tag](/cli/azure/resource?view=azure-cli-latest.md#az_resource_tag) opdracht voor het taggen van de virtuele machine `ansible-inventory-test-vm1` met de sleutel `nginx`:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Genereren van een dynamische inventarisatie
Als u uw virtuele machines hebt gedefinieerd (en niet-gecodeerde), is het tijd om de dynamische voorraad genereren. Ansible bevat een pythonscript aangeroepen [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) die een dynamische inventaris van uw Azure-resources wordt gegenereerd door het maken van API-aanvragen in de Azure Resource Manager. De volgende stappen maakt u met behulp van de `azure_rm.py` script verbinding maken met uw twee virtuele machines in Azure testen:

1. Gebruik de GNU `wget` opdracht voor het ophalen van de `azure_rm.py` script:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Gebruik de `chmod` opdracht om de machtigingen voor toegang tot de `azure_rm.py` script. De volgende opdracht maakt gebruik van de `+x` parameter om toe te staan voor uitvoering (actief) van het opgegeven bestand (`azure_rm.py`):

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Gebruik de [ansible opdracht](https://docs.ansible.com/ansible/2.4/ansible.html) verbinding maken met de resourcegroep: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Eenmaal zijn verbonden, ziet u resultaten die vergelijkbaar is met de volgende uitvoer:

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

## <a name="enable-the-virtual-machine-tag"></a>Het label van de virtuele machine inschakelen
Nadat u de gewenste tag hebt ingesteld, moet u 'inschakelen' van het label. Een manier om een label inschakelen door de tag te exporteren naar een omgevingsvariabele heet `AZURE_TAGS` via de **exporteren** opdracht:

```azurecli-interactive
export AZURE_TAGS=nginx
```

Zodra de tag is geëxporteerd, kunt u proberen de `ansible` opdracht opnieuw:

```azurecli-interactive
ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
```

U ziet nu slechts één virtuele machine (met de tag overeenkomt met de waarde die wordt geëxporteerd naar de **AZURE_TAGS** omgevingsvariabele):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Nginx op de gemarkeerde VM instellen
Het doel van labels is om aan te bieden de mogelijkheid snel en eenvoudig werken met subgroepen van uw virtuele machines. Bijvoorbeeld, Stel dat u wilt installeren Nginx alleen op virtuele machines waarop u een label van hebt toegewezen `nginx`. De volgende stappen laten zien hoe eenvoudig die is om uit te voeren:

1. Maak een bestand (voor uw playbook bevatten) met de naam `nginx.yml` als volgt:

  ```azurecli-interactive
  vi nginx.yml
  ```

1. Voeg de volgende code in de zojuist gemaakte `nginx.yml` bestand:

    ```yml
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

1. Nadat u de playbook uitvoert, ziet u resultaten die vergelijkbaar is met de volgende uitvoer:

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
Deze sectie ziet u een techniek als u wilt testen dat Nginx is geïnstalleerd op de virtuele machine.

1. Gebruik de [az vm lijst-ip-adressen](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az_vm_list_ip_addresses) opdracht voor het ophalen van de IP-adres van de `ansible-inventory-test-vm1` virtuele machine. De geretourneerde waarde (IP-adres van de virtuele machine) wordt vervolgens gebruikt als de parameter voor de SSH-opdracht verbinding maken met de virtuele machine.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. De [nginx - v](https://nginx.org/en/docs/switches.html) opdracht wordt doorgaans gebruikt om de Nginx-versie te drukken. Echter, kan het ook worden gebruikt om te bepalen of Nginx is geïnstalleerd. Voer deze tijdens verbinding met de `ansible-inventory-test-vm1` virtuele machine.

    ```azurecli-interactive
    nginx -v
    ```

1. Zodra u uitvoert het `nginx -v` uitvoert, ziet u het Nginx-versie (tweede regel) die aangeeft dat de Nginx is geïnstalleerd.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)
    
    tom@ansible-inventory-test-vm1:~$
    ```

1. Druk op de  **&lt;Ctrl > D** toetsenbord combinatie om de SSH-sessie te verbreken.

1. Uitvoeren van de voorgaande stappen voor het `ansible-inventory-test-vm2` virtuele machine levert een informatief bericht waarmee wordt aangegeven waar vind u Nginx (Dit betekent dat u niet geïnstalleerd op dit moment hebt):

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
