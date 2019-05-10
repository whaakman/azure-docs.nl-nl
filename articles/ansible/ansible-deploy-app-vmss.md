---
title: Zelfstudie - apps implementeren op virtuele-machineschaalsets in Azure met behulp van Ansible | Microsoft Docs
description: Leer hoe u Ansible gebruikt virtuele Azure-machine-schaalsets configureren en implementeren van toepassing op de schaalset
keywords: ansible, azure, devops, bash, playbook, virtuele machine, schaalset voor virtuele machines, vmss
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: a44fd06ace9b21122f5f4253ac7d9601b54e6b62
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231038"
---
# <a name="tutorial-deploy-apps-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Zelfstudie: Apps implementeren op virtuele-machineschaalsets in Azure met Ansible

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Ophalen van informatie over de host voor een groep van virtuele Azure-machines
> * Klonen en de voorbeeld-app bouwen
> * Java Runtime Environment (Java Runtime Environment) installeren op een schaalset
> * De Java-toepassing in een schaalset implementeren

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)] 
[!INCLUDE [ansible-prereqs-vm-scale-set.md](../../includes/ansible-prereqs-vm-scale-set.md)]
- Met **git** - [git](https://git-scm.com) wordt een Java-voorbeeld gedownload dat in deze zelfstudie wordt gebruikt.
- **JDK (Java SE Development Kit)**: de [JDK](https://aka.ms/azure-jdks) wordt gebruikt om het Java-voorbeeldproject te maken.
- **Apache Maven** - [Apache Maven](https://maven.apache.org/download.cgi) wordt gebruikt om het voorbeeld van Java-project te bouwen.

## <a name="get-host-information"></a>Hostgegevens ophalen

De playbook-code in deze sectie wordt informatie over de host voor een groep van virtuele machines opgehaald. De code opgehaald van de openbare IP-adressen en load balancer in een opgegeven resourcegroep gemaakt en maakt u een hostgroep met de naam `scalesethosts` in voorraad.

Sla het volgende voorbeeld-playbook op als `get-hosts-tasks.yml`:

  ```yml
  - name: Get facts for all Public IPs within a resource groups
    azure_rm_publicipaddress_facts:
      resource_group: "{{ resource_group }}"
    register: output_ip_address

  - name: Get loadbalancer info
    azure_rm_loadbalancer_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ loadbalancer_name }}"
    register: output

  - name: Add all hosts
    add_host:
      groups: scalesethosts
      hostname: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}_{{ item.properties.frontendPort }}"
      ansible_host: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}"
      ansible_port: "{{ item.properties.frontendPort }}"
      ansible_ssh_user: "{{ admin_username }}"
      ansible_ssh_pass: "{{ admin_password }}"
    with_items:
      - "{{ output.ansible_facts.azure_loadbalancers[0].properties.inboundNatRules }}"
  ```

## <a name="prepare-an-application-for-deployment"></a>Een toepassing voorbereiden voor implementatie

De code van de playbook in deze sectie wordt gebruikgemaakt van `git` voor het klonen van een Java-voorbeeldproject van GitHub en het project bouwt. 

Sla het volgende playbook op als `app.yml`:

  ```yml
  - hosts: localhost
    vars:
      repo_url: https://github.com/spring-guides/gs-spring-boot.git
      workspace: ~/src/helloworld

    tasks:
    - name: Git Clone sample app
      git:
        repo: "{{ repo_url }}"
        dest: "{{ workspace }}"

    - name: Build sample app
      shell: mvn package chdir="{{ workspace }}/complete"
  ```

Voer het voorbeeld-Ansible-playbook uit met de volgende opdracht:

  ```bash
  ansible-playbook app.yml
  ```

Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

  ```Output
  PLAY [localhost] 

  TASK [Gathering Facts] 
  ok: [localhost]

  TASK [Git Clone sample app] 
  changed: [localhost]

  TASK [Build sample app] 
  changed: [localhost]

  PLAY RECAP 
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-a-scale-set"></a>De toepassing implementeren in een schaalset

De code playbook in deze sectie wordt gebruikt om:

* Installeer de JRE wordt geselecteerd voor een hostgroep met de naam `saclesethosts`
* De Java-toepassing aan een hostgroep met de naam implementeren `saclesethosts`

Er zijn twee manieren om op te halen van de voorbeeld-playbook:

* [Download de playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-setup-deploy.yml) en sla deze op `vmss-setup-deploy.yml`.
* Maak een nieuw bestand met de naam `vmss-setup-deploy.yml` en kopieer naar het de volgende inhoud:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    scaleset_name: myScaleSet
    loadbalancer_name: myScaleSetLb
    admin_username: azureuser
    admin_password: "{{ admin_password }}"
  tasks:
  - include: get-hosts-tasks.yml

- name: Install JRE on a scale set
  hosts: scalesethosts
  become: yes
  vars:
    workspace: ~/src/helloworld
    admin_username: azureuser

  tasks:
  - name: Install JRE
    apt:
      name: default-jre
      update_cache: yes

  - name: Copy app to Azure VM
    copy:
      src: "{{ workspace }}/complete/target/gs-spring-boot-0.1.0.jar"
      dest: "/home/{{ admin_username }}/helloworld.jar"
      force: yes
      mode: 0755

  - name: Start the application
    shell: java -jar "/home/{{ admin_username }}/helloworld.jar" >/dev/null 2>&1 &
    async: 5000
    poll: 0
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:

* In de `vars` sectie, vervangt de `{{ admin_password }}` tijdelijke aanduiding door uw eigen wachtwoord.
* Gebruik de ssh-verbindingstype met wachtwoorden, het sshpass programma installeren:

    Ubuntu:

    ```bash
    apt-get install sshpass
    ```

    CentOS:

    ```bash
    yum install sshpass
    ```

* In sommige omgevingen ziet u mogelijk een fout over het gebruik van een SSH-wachtwoord in plaats van een sleutel te gebruiken. Als u dit foutbericht ontvangt, kunt u controleren door de volgende regel toe te voegen-hostsleutel uitschakelen `/etc/ansible/ansible.cfg` of `~/.ansible.cfg`:

    ```bash
    [defaults]
    host_key_checking = False
    ```

Voer het playbook uit met de volgende opdracht:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

De uitvoer van het uitvoeren van de opdracht ansible-playbook geeft aan dat de Java-voorbeeldtoepassing is geïnstalleerd aan de hostgroep van de schaalset:

  ```Output
  PLAY [localhost]

  TASK [Gathering Facts]
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups]
  ok: [localhost]

  TASK [Get loadbalancer info]
  ok: [localhost]

  TASK [Add all hosts]
  changed: [localhost] ...

  PLAY [Install JRE on scale set]

  TASK [Gathering Facts]
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM]
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application]
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

## <a name="verify-the-results"></a>De resultaten controleren

Controleer of de resultaten van uw werk door te navigeren naar de URL van de load balancer voor de schaalset:

![Java-app die wordt uitgevoerd in een schaalset in Azure.](media/ansible-vmss-deploy/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Automatisch schalen virtuele-machineschaalsets in Azure met behulp van Ansible](./ansible-auto-scale-vmss.md)