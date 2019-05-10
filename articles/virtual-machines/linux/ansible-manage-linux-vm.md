---
title: 'Snelstartgids: Linux virtuele machines in Azure met behulp van Ansible beheren | Microsoft Docs'
description: In deze snelstartgids leert u hoe u voor het beheren van een virtuele Linux-machine in Azure met Ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: a7862e95966d7b0e0ab31f242dff0244735fe7a1
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409232"
---
# <a name="quickstart-manage-linux-virtual-machines-in-azure-using-ansible"></a>Quickstart: Beheren van virtuele Linux-machines in Azure met Ansible

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel gebruikt u een Ansible-playbook starten en stoppen van een virtuele Linux-machine. 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="stop-a-virtual-machine"></a>Een virtuele machine stoppen

In deze sectie gebruikt u Ansible toewijzing (stop) een Azure virtuele machine.

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Open [Cloud Shell](/azure/cloud-shell/overview).

1. Maak een bestand met de naam `azure-vm-stop.yml`, en open het in de editor:

    ```azurecli-interactive
    code azure-vm-stop.yml
    ```

1. Plak de volgende voorbeeldcode in de editor:

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Stop virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
            allocated: no
    ```

1. Vervang de `{{ resource_group_name }}` en `{{ vm_name }}` tijdelijke aanduidingen door uw waarden.

1. Sla het bestand op en sluit de editor af.

1. Voer de playbook met behulp de `ansible-playbook` opdracht:

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1. Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="start-a-virtual-machine"></a>Een virtuele machine starten

In deze sectie maakt u Ansible gebruiken om te beginnen een (gestopt) Azure virtuele machine is ongedaan gemaakt.

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Open [Cloud Shell](/azure/cloud-shell/overview).

1. Maak een bestand met de naam `azure-vm-start.yml`, en open het in de editor:

    ```azurecli-interactive
    code azure-vm-start.yml
    ```

1. Plak de volgende voorbeeldcode in de editor:

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Start virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
    ```

1. Vervang de `{{ resource_group_name }}` en `{{ vm_name }}` tijdelijke aanduidingen door uw waarden.

1. Sla het bestand op en sluit de editor af.

1. Voer de playbook met behulp de `ansible-playbook` opdracht:

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1. Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

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
> [Zelfstudie: Beheren Azure dynamische voorraden met Ansible](~/articles/ansible/ansible-manage-azure-dynamic-inventories.md)