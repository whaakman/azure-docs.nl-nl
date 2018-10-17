---
title: Toepassingen implementeren in schaalsets voor virtuele machines in Azure met Ansible
description: Meer informatie over het gebruik van Ansible voor het configureren van een schaalset voor virtuele machines en het implementeren van een toepassing in de schaalset voor virtuele machines in Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, virtuele machine, schaalset voor virtuele machines, vmss
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/11/2018
ms.openlocfilehash: 3512cb7eda9f9e5a6e18dc83f6523029b17a9de2
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391467"
---
# <a name="deploy-applications-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Toepassingen implementeren in schaalsets voor virtuele machines in Azure met Ansible
U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. U kunt Ansible gebruiken om uw toepassingen te implementeren in Azure. In dit artikel leest u hoe u een Java-toepassing implementeert in een schaalset voor virtuele Azure-machines (VMSS).  

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Schaalset voor virtuele machines**: als u nog geen schaalset voor virtuele machines hebt, kunt u [een schaalset voor virtuele machines maken met Ansible](ansible-create-configure-vmss.md). 
- Met **git** - [git](https://git-scm.com) wordt een Java-voorbeeld gedownload dat in deze zelfstudie wordt gebruikt.
- **Java SE Development Kit (JDK)**: de JDK wordt gebruikt om het Java-voorbeeldproject te maken.
- **Hulpprogramma's voor maken van Apache Maven**: de [hulpprogramma's voor maken van Apache Maven](https://maven.apache.org/download.cgi) worden gebruikt om het Java-voorbeeldproject te maken.

> [!Note]
> Ansible 2.6 is vereist voor het uitvoeren van de volgende playbooks-voorbeelden in deze zelfstudie. 

## <a name="get-host-information"></a>Hostgegevens ophalen

In deze sectie ziet u hoe u met Ansible hostgegevens kunt ophalen voor een groep virtuele Azure-machines. Hieronder ziet u een voorbeeld van een Ansible-playbook. Met de code worden de openbare IP-adressen en load balancer binnen de opgegeven resourcegroup opgehaald en wordt een hostgroep met de naam **saclesethosts** gemaakt in de inventaris. 

Sla het volgende voorbeeld-playbook op als `get-hosts-tasks.yml`: 

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

## <a name="prepare-an-application-for-deployment"></a>Een toepassing voorbereiden voor implementatie  

In deze sectie gebruikt u git om een Java-voorbeeldproject van GitHub te klonen en het project te maken. Sla het volgende playbook op als `app.yml`:

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

Voer het voorbeeld-Ansible-playbook uit met de volgende opdracht:

  ```bash
  ansible-playbook app.yml
  ```

De uitvoer van de opdracht ansible-playbook lijkt op het volgende, waarbij u kunt zien dat de voorbeeld-app is gemaakt die is gekloond vanuit GitHub:

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

## <a name="deploy-the-application-to-vmss"></a>De toepassing implementeren in VMSS

Met de volgende sectie in een Ansible-playbook wordt de JRE (Java Runtime Environment) geïnstalleerd op een hostgroep met de naam **saclesethosts** en wordt de Java-toepassing geïmplementeerd in een hostgroep met de naam **saclesethosts**: 

(Wijzig de `admin_password` in uw eigen wachtwoord.)

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

U kunt het voorgaande voorbeeld-Ansible-playbook opslaan als `vmss-setup-deploy.yml` of [het volledige voorbeeld-playbook downloaden](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss). 

Als u het ssh-verbindingstype wilt gebruiken met wachtwoorden, moet u het sshpass-programma installeren. 
  - Voor Ubunto 16.04 voert u de opdracht `apt-get install sshpass` uit.
  - Voor CentOS 7.4 voert u de opdracht `yum install sshpass` uit.

U ziet mogelijk een foutbericht zoals **Het gebruik van een SSH-wachtwoord in plaats van een sleutel is niet mogelijk omdat de controle van de hostsleutel is ingeschakeld en dit niet wordt ondersteund door sshpass. Voeg de vingerafdruk van deze host toe aan het bestand known_hosts om deze host te beheren.** Als u deze fout ziet, kunt u de controle van de hostsleutel uitschakelen door de volgende regel toe te voegen aan het bestand `/etc/ansible/ansible.cfg` of het bestand `~/.ansible.cfg`:
  ```bash
  [defaults]
  host_key_checking = False
  ```

Voer het playbook uit met de volgende opdracht:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

In de uitvoer van het uitvoeren van de opdracht ansible-playbook wordt aangegeven dat de Java-voorbeeldtoepassing is geïnstalleerd in de hostgroep van de schaalset voor virtuele machines:

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

Gefeliciteerd! Uw toepassing wordt nu uitgevoerd in Azure. U kunt nu navigeren naar de URL van de load balancer voor uw schaalset voor virtuele machines:

![Java-app die wordt uitgevoerd in een schaalset voor virtuele machines in Azure.](media/ansible-deploy-app-vmss/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Ansible-voorbeeldplaybook voor VMSS](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)