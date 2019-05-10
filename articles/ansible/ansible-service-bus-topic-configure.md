---
title: Zelfstudie - onderwerpen configureren in Azure Service Bus wanneer u Ansible gebruikt | Microsoft Docs
description: Leer hoe u Ansible gebruikt om een Azure Service Bus-onderwerp te maken
keywords: ansible, azure, devops, bash, playbook, servicebus, onderwerpen en abonnementen
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: ca8d849796520ac260d888d772c064316db68a30
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230871"
---
# <a name="tutorial-configure-topics-in-azure-service-bus-using-ansible"></a>Zelfstudie: Onderwerpen configureren in Azure Service Bus met Ansible

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-servicebus.md](../../includes/open-source-devops-intro-servicebus.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een onderwerp maken
> * Een abonnement maken
> * Een SAS-beleid maken
> * Naamruimtegegevens ophalen
> * Onderwerp-en abonnementsgegevens ophalen
> * Intrekken van een SAS-beleid

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-service-bus-topic"></a>De Service Bus-onderwerp maken

De voorbeeldcode van de playbook wordt de volgende bronnen:
- Azure-resourcegroep
- Service Bus-naamruimte in de resourcegroep bevinden
- Service Bus-onderwerp met de naamruimte

Sla het volgende playbook op als `servicebus_topic.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Ensure resource group exist
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
    - name: Create a namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
    - name: Create a topic
      azure_rm_servicebustopic:
          name: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: topic
    - debug:
          var: topic
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_topic.yml
```

## <a name="create-the-subscription"></a>Het abonnement maken

De voorbeeldcode van de playbook wordt het abonnement onder een Service Bus-onderwerp. Azure Service Bus-onderwerpen kunnen meerdere abonnementen hebt. Een abonnee naar een onderwerp kunt ontvangt een kopie van elk bericht dat wordt verzonden naar het onderwerp. Abonnementen zijn benoemde entiteiten, deze blijvend worden gemaakt, maar kunnen desgewenst laten verlopen.

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      topic: servicebustesttopic
      subscription: servicebustestsubs
  tasks:
    - name: Create a subscription
      azure_rm_servicebustopicsubscription:
          name: "{{ subscription }}"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: subs
    - debug:
          var: subs
```

Sla het volgende playbook op als `servicebus_subscription.yml`:

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_subscription.yml
```

## <a name="create-the-sas-policy"></a>De SAS-beleid maken

Een [Shared Access Signature (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) is een autorisatiemechanisme voor op basis van claims met behulp van tokens. 

De voorbeeldcode playbook maakt twee SAS-beleid voor een Service Bus-wachtrij met verschillende bevoegdheden.

Sla het volgende playbook op als `servicebus_topic_policy.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Create a policy with send and listen privilege
      azure_rm_servicebussaspolicy:
          name: "{{ topic }}-{{ item }}"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          rights: "{{ item }}"
      with_items:
        - send
        - listen
      register: policy
    - debug:
          var: policy
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_topic_policy.yml
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

## <a name="retrieve-topic-and-subscription-information"></a>Onderwerp-en abonnementsgegevens ophalen

De voorbeeldquery playbook code's voor de volgende informatie:
- Informatie over Service Bus
- Lijst met details van abonnement voor het onderwerp
 
Sla het volgende playbook op als `servicebus_list.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Get a topic's information
      azure_rm_servicebus_facts:
          type: topic
          name: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: topic_fact
    - name: "List subscriptions under topic {{ topic }}"
      azure_rm_servicebus_facts:
          type: subscription
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: subs_fact
    - debug:
          var: "{{ item }}"
      with_items:
        - topic_fact.servicebuses[0]
        - subs_fact.servicebuses
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:
- De `show_sas_policies` waarde geeft aan of u wilt weergeven van de SAS-beleid onder de opgegeven wachtrij. Deze waarde is standaard ingesteld op `False` om te voorkomen dat extra netwerkbelasting.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_list.yml
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
      topic: servicebustesttopic
  tasks:
    - name: Delete a policy
      azure_rm_servicebussaspolicy:
          name: "{{ topic }}-policy"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook servicebus_topic_policy_delete.yml
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
      topic: servicebustesttopic
      subscription: servicebustestsubs
  tasks:
    - name: Delete subscription
      azure_rm_servicebustopicsubscription:
          name: "{{ subscription }}"
          topic: "{{ topic }}"
          resource_group: "{{ resource_group }}"
          namespace: "{{ namespace }}"
          state: absent
    - name: Delete topic
      azure_rm_servicebustopic:
          name: "{{ topic }}"
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
> [Ansible in Azure](/azure/ansible/)