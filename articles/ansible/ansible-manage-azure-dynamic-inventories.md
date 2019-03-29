---
title: Dynamische voorraden in Azure beheren met Ansible
description: Informatie over het beheren van dynamische voorraden in Azure met Ansible
ms.service: azure
keywords: ansible, azure, devops, bash, cloudshell, dynamische voorraad
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/09/2018
ms.topic: tutorial
ms.openlocfilehash: 0ef754b792654281f2a12b8eee613434896d5476
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58092205"
---
# <a name="use-ansible-to-manage-your-azure-dynamic-inventories"></a>Dynamische voorraden in Azure beheren met Ansible
Ansible kan worden gebruikt om voorraadinformatie uit verschillende bronnen ((waaronder cloudbronnen zoals Azure) in een *dynamische voorraad* op te halen. In dit artikel gebruikt u de [Azure Cloud Shell](./ansible-run-playbook-in-cloudshell.md) om een dynamische virtuele machine voor Ansible Azure te configureren waarin u twee virtuele machines maakt, één van deze virtuele machines tagt en Nginx op de getagde virtuele machine installeert.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Azure-referenties** - [Azure-referenties maken en Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-the-test-virtual-machines"></a>De virtuele testmachines maken

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Open [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

1. Maak een Azure-resourcegroep om de virtuele machines voor deze zelfstudie in op te slaan.

    > [!IMPORTANT]  
    > De Azure-resourcegroep die u in deze stap u maakt, moet een naam hebben die volledig uit kleine letters bestaat. Anders mislukt het genereren van de dynamische voorraad.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Maak twee virtuele Linux-machines in Azure met een van de volgende technieken:

    - **Ansible-playbook**: in het artikel [Een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm) wordt geïllustreerd hoe u een virtuele machine maakt op basis van een Ansible-playbook. Als u een playbook gebruikt voor het definiëren van een van de of beide virtuele machines, moet u ervoor zorgen dat de SSH-verbinding wordt gebruikt in plaats van een wachtwoord.

    - **Azure CLI**: voer elk van de volgende opdrachten in de Cloud Shell uit om de twee virtuele machines te maken:

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
U kunt [tags gebruiken om uw Azure-resources te organiseren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) met behulp van door de gebruiker gedefinieerde categorieën. 

Voer de volgende opdracht [az resource tag](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) in om de virtuele machine `ansible-inventory-test-vm1` met de sleutel `nginx` te taggen:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Een dynamische voorraad genereren
Zodra u uw virtuele machines hebt gedefinieerd (en getagd), is het tijd om de dynamische voorraad te genereren. Ansible bevat een Python-script met de naam [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) dat een dynamische voorraad van uw Azure-resources genereert door API-aanvragen naar de Azure Resource Manager te verzenden. In de volgende stappen wordt uitgelegd hoe u het script `azure_rm.py` gebruikt om verbinding te maken met de twee virtuele testmachines van Azure:

1. Gebruik de GNU-opdracht `wget` om het script `azure_rm.py` op te halen:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Gebruik de opdracht `chmod` om de machtigingen voor toegang tot het script `azure_rm.py` te wijzigen. De volgende opdracht maakt gebruik van de parameter `+x` om uitvoering van het opgegeven bestand (`azure_rm.py`) toe te staan:

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Gebruik de [ansible-opdracht](https://docs.ansible.com/ansible/2.4/ansible.html) om verbinding te maken met de resourcegroep: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Als u verbinding hebt, ziet u resultaten die vergelijkbaar zijn met de volgende uitvoer:

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

## <a name="enable-the-virtual-machine-tag"></a>De tag voor de virtuele machine inschakelen
Nadat u de gewenste tag hebt ingesteld, moet u de tag 'inschakelen'. Eén manier om een tag in te schakelen, is de tag te exporteren naar een omgevingsvariabele genaamd `AZURE_TAGS` via de opdracht **export**:

```azurecli-interactive
export AZURE_TAGS=nginx
```

Zodra de tag is geëxporteerd, kunt u proberen de opdracht `ansible` opnieuw uit te voeren:

```azurecli-interactive
ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
```

Nu ziet u slechts één virtuele machine (de machine waarvan de tag overeenkomt met de waarde die wordt geëxporteerd naar de omgevingsvariabele **AZURE_TAGS**):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Nginx instellen op de getagde virtuele machine
Het doel van tags is om de mogelijkheid in te schakelen snel en eenvoudig met subgroepen van uw virtuele machines te werken. Stel dat u Nginx bijvoorbeeld wilt installeren op virtuele machines waarop u een tag van `nginx` hebt toegewezen. In de volgende stappen ziet u hoe u dit gemakkelijk kunt doen:

1. Maak als volgt een bestand (om uw playbook te bevatten) met de naam `nginx.yml`:

   ```azurecli-interactive
   vi nginx.yml
   ```

1. Voeg de volgende code in het zojuist gemaakte bestand `nginx.yml` in:

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

1. Voer het playbook `nginx.yml` uit:

    ```azurecli-interactive
    ansible-playbook -i azure_rm.py nginx.yml
    ```

1. Zodra u het playbook uitvoert, ziet u resultaten die vergelijkbaar zijn met de volgende uitvoer:

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
In deze sectie wordt één techniek geïllustreerd om te testen of Nginx op uw virtuele machine is geïnstalleerd.

1. Gebruik de opdracht [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) om het IP-adres van de virtuele machine `ansible-inventory-test-vm1` op te halen. De geretourneerde waarde (het IP-adres van de virtuele machine) wordt vervolgens als de parameter gebruikt voor de SSH-opdracht om verbinding te maken met de virtuele machine.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. Terwijl u verbinding hebt met de virtuele machine `ansible-inventory-test-vm1`, voert u de opdracht [nginx - v](https://nginx.org/en/docs/switches.html) uit om te bepalen of Nginx is geïnstalleerd.

    ```azurecli-interactive
    nginx -v
    ```

1. Zodra u de opdracht `nginx -v` uitvoert, ziet u de Nginx-versie (tweede regel) die aangeeft dat Nginx is geïnstalleerd.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)
    
    tom@ansible-inventory-test-vm1:~$
    ```

1. Druk op de toetscombinatie  **&lt;Ctrl > D** om de SSH-sessie te verbreken.

1. Als u de voorgaande stappen voor de virtuele machine `ansible-inventory-test-vm2` uitvoert, verschijnt er een informatief bericht waarin wordt aangegeven waar u Nginx kunt ophalen (dit betekent dat u Nginx momenteel nog niet hebt geïnstalleerd):

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
