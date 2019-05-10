---
title: Zelfstudie - wachtrijen in Azure Service Bus met Ansible configureren | Microsoft Docs
description: Leer hoe u Ansible gebruikt om te maken van een Azure Service Bus-wachtrij
keywords: ansible, azure, devops, bash, playbook, servicebus, wachtrij
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 6efc11106fae18beac43ab1896733ab6bfc64dad
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230765"
---
# <a name="tutorial-configure-queues-in-azure-service-bus-using-ansible"></a>Zelfstudie: Configureren van wachtrijen in Azure Service Bus met Ansible

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-servicebus.md](../../includes/open-source-devops-intro-servicebus.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een wachtrij maken
> * Een SAS-plicy maken
> * Naamruimtegegevens ophalen
> * Wachtrijgegevens ophalen
> * Intrekken van de wachtrij SAS-beleid

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-service-bus-queue"></a>De Service Bus-wachtrij maken

De voorbeeldcode van de playbook wordt de volgende bronnen:
- Azure-resourcegroep
- Service Bus-naamruimte in de resourcegroep bevinden
- Service Bus-wachtrij met de naamruimte

Sla het volgende playbook op als `servicebus_queue.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Ensure resource group exist
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
    - name: Create a namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
    - name: Create a queue
      azure_rm_servicebusqueue:
          name: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: queue
    - debug:
          var: queue
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_queue.yml
```

## <a name="create-the-sas-policy"></a>De SAS-beleid maken

Een [Shared Access Signature (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) is een autorisatiemechanisme voor op basis van claims met behulp van tokens. 

De voorbeeldcode playbook maakt twee SAS-beleid voor een Service Bus-wachtrij met verschillende bevoegdheden.

Sla het volgende playbook op als `servicebus_queue_policy.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Create a policy with send and listen priviledge
      azure_rm_servicebussaspolicy:
          name: "{{ queue }}-policy"
          queue: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          rights: listen_send
      register: policy
    - debug:
          var: policy
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:
- De `rights` waarde vertegenwoordigt de bevoegdheid die een gebruiker aan de wachtrij heeft. Geef een van de volgende waarden: `manage`, `listen`, `send`, of `listen_send`.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_queue_policy.yml
```

## <a name="retrieve-namespace-information"></a>Naamruimtegegevens ophalen

De voorbeeldcode playbook query uitgevoerd voor de naamruimte-informatie.

Sla het volgende playbook op als `servicebus_namespace_info.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
  tasks:
    - name: Get a namespace's information
      azure_rm_servicebus_facts:
          type: namespace
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: ns
    - debug:
          var: ns
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:
- De `show_sas_policies` waarde geeft aan of u wilt weergeven van de SAS-beleid onder de opgegeven naamruimte. De waarde is standaard `False` om te voorkomen dat extra netwerkbelasting.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_namespace_info.yml
```

## <a name="retrieve-queue-information"></a>Wachtrijgegevens ophalen

De voorbeeld-playbook code query's in een wachtrij informatie. 

Sla het volgende playbook op als `servicebus_queue_info.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Get a queue's information
      azure_rm_servicebus_facts:
          type: queue
          name: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: queue
    - debug:
          var: queue
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:
- De `show_sas_policies` waarde geeft aan of u wilt weergeven van de SAS-beleid onder de opgegeven wachtrij. Deze waarde is standaard ingesteld op `False` om te voorkomen dat extra netwerkbelasting.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_queue_info.yml
```

## <a name="revoke-the-queue-sas-policy"></a>Intrekken van de wachtrij SAS-beleid

De playbook-voorbeeldcode wordt een wachtrij SAS-beleid verwijderd.

Sla het volgende playbook op als `servicebus_queue_policy_delete.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Create a policy with send and listen priviledge
      azure_rm_servicebussaspolicy:
          name: "{{ queue }}-policy"
          queue: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_queue_policy_delete.yml
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer het niet meer nodig hebt, verwijdert u de resources die in dit artikel is gemaakt. 

Sla de volgende code als `cleanup.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Delete queue
      azure_rm_servicebusqueue:
          name: "{{ queue }}"
          resource_group: "{{ resource_group }}"
          namespace: "{{ namespace }}"
          state: absent
    - name: Delete namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
    - name: Delete resource group
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          state: absent
          force_delete_nonempty: yes
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Zelfstudie: Configureren van een onderwerp in Azure Service Bus met Ansible](ansible-service-bus-topic-configure.md)