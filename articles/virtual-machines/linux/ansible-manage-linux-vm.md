---
title: Een virtuele Linux-machine beheren in Azure met Ansible
description: Lees hoe u een virtuele Linux-machine beheert in Azure met behulp van Ansible
ms.service: virtual-machines-linux
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 09/27/2018
ms.openlocfilehash: 8f97cf8a4231e9a2144f27c0540de96574e13795
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57789874"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Een virtuele Linux-machine beheren in Azure met Ansible
U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele Azure-machines, net zoals andere resources. In dit artikel leest u hoe u een virtuele Linux-machine start en stopt met een Ansible-playbook. 

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Toewijzing van een virtuele Azure-machine ongedaan maken (stoppen) met Ansible
In deze sectie wordt beschreven hoe u de toewijzing van een virtuele Azure-machine ongedaan maakt (stopt) met Ansible

1.  Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1.  Open [Cloud Shell](/azure/cloud-shell/overview).

1.  Maak een bestand (voor het playbook) met de naam `azure-vm-stop.yml` en open dit in de vi-editor:

    ```azurecli-interactive
    vi azure-vm-stop.yml
    ```

1.  Activeer de invoegmodus door op de toets **I** te drukken.

1.  Plak de volgende voorbeeldcode in de editor:

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
      - name: Deallocate the virtual machine
        azure_rm_virtualmachine:
          resource_group: myResourceGroup
          name: myVM
          allocated: no
    ```

1.  Verlaat de invoegmodus door op **Esc** te drukken.

1.  Sla het bestand op en sluit vi Editor af met de volgende opdracht:

    ```bash
    :wq
    ```

1.  Voer het voorbeeld van het Ansible-playbook uit.

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1.  De uitvoer lijkt op de onderstaande uitvoer, waar u kunt zien dat de toewijzing van de virtuele machine ongedaan is gemaakt (gestopt):

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="use-ansible-to-start-a-deallocated-stopped-azure-virtual-machine"></a>Een virtuele Azure-machine starten waarvan de toewijzing ongedaan is gemaakt (gestopt)
In deze sectie wordt beschreven hoe u een virtuele Azure-machine start waarvan de toewijzing ongedaan is gemaakt (gestopt) met Ansible

1.  Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1.  Open [Cloud Shell](/azure/cloud-shell/overview).

1.  Maak een bestand (voor het playbook) met de naam `azure-vm-start.yml` en open dit in de vi-editor:

    ```azurecli-interactive
    vi azure-vm-start.yml
    ```

1.  Activeer de invoegmodus door op de toets **I** te drukken.

1.  Plak de volgende voorbeeldcode in de editor:

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
      - name: Start the virtual machine
        azure_rm_virtualmachine:
          resource_group: myResourceGroup
          name: myVM
    ```

1.  Verlaat de invoegmodus door op **Esc** te drukken.

1.  Sla het bestand op en sluit vi Editor af met de volgende opdracht:

    ```bash
    :wq
    ```

1.  Voer het voorbeeld van het Ansible-playbook uit.

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1.  De uitvoer lijkt op de onderstaande uitvoer, waar u kunt zien dat de toewijzing van de virtuele machine is gestart:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Inventarisatie van dynamische Azure-resources beheren met Ansible](~/articles/ansible/ansible-manage-azure-dynamic-inventories.md)