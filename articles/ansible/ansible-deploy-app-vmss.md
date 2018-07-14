---
title: Toepassingen implementeren op virtuele-machineschaalsets in Azure met Ansible
description: Leer hoe u Ansible gebruikt voor het configureren van een virtuele-machineschaalset en -toepassing implementeren op de virtuele-machineschaalset in Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, virtuele machines, virtuele-machineschaalset, vmss
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: b9c8058606e13c0db4908530e98cddb69d2caf50
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008851"
---
# <a name="deploy-applications-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Toepassingen implementeren op virtuele-machineschaalsets in Azure met Ansible
Ansible kunt u de implementatie en configuratie van bronnen in uw omgeving automatiseren. U kunt Ansible gebruiken om uw toepassingen naar Azure te implementeren. Dit artikel ziet u hoe u een Java-toepassing in een schaalset voor virtuele Azure-machines (VMSS) implementeert.  

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement** : als u geen Azure-abonnement, maak een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
- **Ansible configureren** - [maken-Azure-referenties en Ansible configureren](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **Ansible en de Azure Python SDK-modules** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)
- **Virtuele-machineschaalset** : als u een virtuele-machineschaalset nog niet hebt ingesteld, kunt u [maken van een virtuele-machineschaalset met Ansible](ansible-create-configure-vmss.md). 
- **GIT** - [git](https://git-scm.com) wordt gebruikt om een Java-voorbeeld in deze zelfstudie gebruikt te downloaden.
- **Java SE Development Kit (JDK)** -de JDK wordt gebruikt om het voorbeeld van Java-project te bouwen.
- **Hulpprogramma's van Apache Maven build** : de [hulpprogramma's van Apache Maven build](https://maven.apache.org/download.cgi) worden gebruikt om het voorbeeld van Java-project te bouwen.

> [!Note]
> Ansible 2.6 is vereist voor het uitvoeren van de volgende de playbooks voorbeeld in deze zelfstudie. 

## <a name="get-host-information"></a>Informatie over ophalen

In deze sectie ziet u hoe u Ansible gebruikt om op te halen van informatie over de host voor een groep virtuele machines van Azure. Hieronder volgt een voorbeeld van Ansible-playbook. De code wordt de openbare IP-adressen en load balancer in resourcegroep opgegeven en maakt u een hostgroep met de naam **saclesethosts** in voorraad. 

Sla het volgende voorbeeld-playbook als `get-hosts-tasks.yml`: 

  ```yaml
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

## <a name="prepare-an-application-for-deployment"></a>Een toepassing voor implementatie voorbereiden  

In deze sectie maakt u git gebruiken om te klonen van een Java-voorbeeldproject van GitHub en bouw het project. Opslaan van de volgende playbook als `app.yml`:

  ```yaml
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

De voorbeeld-Ansible-playbook uitvoeren met de volgende opdracht:

  ```bash
  ansible-playbook app.yml
  ```

Geeft het de uitvoer van de opdracht ansible-playbook uit die vergelijkbaar is met het volgende waar u zien dat deze de voorbeeld-app klonen vanuit GitHub gebouwd:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Git Clone sample app] ***************************************************************************
  changed: [localhost]

  TASK [Build sample app] ***************************************************
  changed: [localhost]

  PLAY RECAP ***************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-vmss"></a>De toepassing implementeren naar VMSS

Java Runtime Environment (Java Runtime Environment) op een hostgroep met de naam wordt geïnstalleerd door de volgende sectie in een Ansible-playbook **saclesethosts**, en implementeert de Java-toepassing aan een hostgroep met de naam **saclesethosts**: 

(Wijziging de `admin_password` op uw eigen wachtwoord.)

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      scaleset_name: myVMSS
      loadbalancer_name: myVMSSlb
      admin_username: azureuser
      admin_password: "your_password"
    tasks:   
    - include: get-hosts-tasks.yml

  - name: Install JRE on VMSS
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

U kunt het voorgaande voorbeeld Ansible-playbook als opslaan `vmss-setup-deploy.yml`, of [downloaden van het volledige voorbeeld playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss). 

Gebruik de ssh verbindingstype met wachtwoorden, moet u het programma sshpass installeren. 
  - Voer de opdracht uit voor Ubunto 16.04, `apt-get install sshpass`.
  - Voer de opdracht uit voor CentOS 7.4, `yum install sshpass`.

U ziet mogelijk een foutbericht wordt weergegeven als **met een SSH-wachtwoord in plaats van een sleutel is niet mogelijk omdat de Hostsleutel controleren is ingeschakeld en deze biedt geen ondersteuning voor sshpass.  Toevoegen van deze host vingerafdruk toe aan het bestand known_hosts voor het beheren van deze host.** Als u deze fout ziet, kunt u uitschakelen hostsleutel controleren door de volgende regel toe te voegen aan de `/etc/ansible/ansible.cfg` bestand of de `~/.ansible.cfg` bestand:
  ```bash
  [defaults]
  host_key_checking = False
  ```

De playbook uitvoeren met de volgende opdracht:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

De uitvoer van het uitvoeren van de opdracht ansible-playbook geeft aan dat de Java-voorbeeldtoepassing aan de hostgroep van de virtuele-machineschaalset is geïnstalleerd:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups] **********************************************
  ok: [localhost]

  TASK [Get loadbalancer info] ****************************************************************************
  ok: [localhost]

  TASK [Add all hosts] *****************************************************************************
  changed: [localhost] ...

  PLAY [Install JRE on VMSS] *****************************************************************************

  TASK [Gathering Facts] *****************************************************************************
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM] *****************************************************************************
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application] ********************************************************************
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP ************************************************************************************************
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

Gefeliciteerd! Uw toepassing wordt nu uitgevoerd in Azure. U kunt nu navigeren naar de URL van de load balancer voor uw virtuele-machineschaalset:

![Java-app die wordt uitgevoerd in een virtuele-machineschaalset in Azure.](media/ansible-deploy-app-vmss/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Voorbeeld-Ansible-playbook voor VMSS](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)