---
title: 'Zelfstudie: configureren van netwerken in Azure Kubernetes Service (AKS) met behulp van Ansible kubenet | Microsoft Docs'
description: Meer informatie over het gebruik van Ansible voor het configureren van kubenet netwerken in Azure Kubernetes Service (AKS)-cluster
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, container, aks, kubernetes
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 96c280f43aec32fe3d6631108dfd7e0a3f789224
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767290"
---
# <a name="tutorial-configure-kubenet-networking-in-azure-kubernetes-service-aks-using-ansible"></a>Zelfstudie: Configureren van netwerken in Azure Kubernetes Service (AKS) met behulp van Ansible kubenet

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

Met behulp van AKS, kunt u een cluster met behulp van de volgende netwerk-modellen implementeren:

- [Kubenet netwerken](/azure/aks/configure-kubenet) -netwerkbronnen gewoonlijk worden gemaakt en geconfigureerd, zoals het AKS-cluster is geïmplementeerd.
- [Azure Container netwerken Interface (CNI)-netwerken](/azure/aks/configure-azure-cni) -AKS-cluster is verbonden met een bestaande virtuele-netwerkbronnen en configuraties.

Zie voor meer informatie over netwerken voor uw toepassingen in AKS [concepten voor toepassingen in AKS netwerk](/azure/aks/concepts-network).

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een AKS-cluster maken
> * Configureren van netwerken van Azure kubenet

## <a name="prerequisites"></a>Vereisten

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en een subnet maken

De code playbook in deze sectie maakt u de volgende Azure-resources:

- Virtueel netwerk
- Subnet binnen het virtuele netwerk

Sla het volgende playbook op als `vnet.yml`:

```yml
- name: Create vnet
  azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefixes_cidr:
          - 10.0.0.0/8

- name: Create subnet
  azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefix_cidr: 10.240.0.0/16
      virtual_network_name: "{{ name }}"
  register: subnet
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Een AKS-cluster maken in het virtuele netwerk

De code playbook in deze sectie maakt u een AKS-cluster in een virtueel netwerk. 

Sla het volgende playbook op als `aks.yml`:

```yml
- name: List supported kubernetes version from Azure
  azure_rm_aks_version:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster with vnet
  azure_rm_aks:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      dns_prefix: "{{ name }}"
      kubernetes_version: "{{ versions.azure_aks_versions[-1] }}"
      agent_pool_profiles:
        - count: 3
          name: nodepool1
          vm_size: Standard_D2_v2
          vnet_subnet_id: "{{ vnet_subnet_id }}"
      linux_profile:
          admin_username: azureuser
          ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      service_principal:
          client_id: "{{ lookup('ini', 'client_id section=default file=~/.azure/credentials') }}"
          client_secret: "{{ lookup('ini', 'secret section=default file=~/.azure/credentials') }}"
      network_profile:
          network_plugin: kubenet
          pod_cidr: 192.168.0.0/16
          docker_bridge_cidr: 172.17.0.1/16
          dns_service_ip: 10.0.0.10
          service_cidr: 10.0.0.0/16
  register: aks
```

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- Gebruik `azure_rm_aks_version` module de ondersteunde versie te vinden.
- De `vnet_subnet_id` is van het subnet in de vorige sectie hebt gemaakt.
- De `network_profile` definieert de eigenschappen voor de invoegtoepassing kubenet-netwerk.
- De `service_cidr` interne services in de AKS-cluster toewijzen aan een IP-adres wordt gebruikt. Deze IP-adresbereik moet een adresruimte die ergens anders niet wordt gebruikt in uw netwerk. 
- De `dns_service_ip` -adres moet de '. 10 '-adres van uw service IP-adresbereik.
- De `pod_cidr` moet een grote adresruimte die zich niet in gebruik ergens anders in uw netwerkomgeving. Het adresbereik moet groot genoeg is voor het aantal knooppunten die u verwacht te schalen tot. U kunt dit adresbereik niet wijzigen nadat het cluster is geïmplementeerd.
- De schil IP-adresbereik wordt gebruikt om toe te wijzen een/24-adresruimte aan elk knooppunt in het cluster. In het volgende voorbeeld wordt de `pod_cidr` van 192.168.0.0/16 het eerste knooppunt 192.168.0.0/24, het tweede knooppunt 192.168.1.0/24 en de derde knooppunt 192.168.2.0/24 toegewezen.
- Als het cluster schalen of upgrades worden uitgevoerd blijft Azure een pod IP-adresbereik toewijzen aan elke nieuwe knooppunt.
- De playbook wordt geladen `ssh_key` van `~/.ssh/id_rsa.pub`. Als u deze wijzigen, gebruikt u de indeling met één regel - beginnen met 'ssh-rsa' (zonder de aanhalingstekens).
- De `client_id` en `client_secret` waarden die worden geladen uit `~/.azure/credentials`, dit is de standaard-referentie-bestand. U kunt deze waarden ingesteld op uw service principal of deze waarden van omgevingsvariabelen laden:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

## <a name="associate-the-network-resources"></a>De netwerkresources koppelen

Wanneer u een AKS-cluster maakt, wordt een groep en route-tabel voor de beveiliging voor netwerk worden gemaakt. Deze resources worden beheerd door AKS en bijgewerkt wanneer u maken en beschikbaar maken van services. De groep en de route-tabel voor de beveiliging voor netwerk als volgt aan uw virtuele netwerksubnet koppelen. 

Opslaan van de volgende playbook als `associate.yml`.

```yml
- name: Get route table
  azure_rm_routetable_facts:
      resource_group: "{{ node_resource_group }}"
  register: routetable

- name: Get network security group
  azure_rm_securitygroup_facts:
      resource_group: "{{ node_resource_group }}"
  register: nsg

- name: Parse subnet id
  set_fact:
      subnet_name: "{{ vnet_subnet_id | regex_search(subnet_regex, '\\1') }}"
      subnet_rg: "{{ vnet_subnet_id | regex_search(rg_regex, '\\1') }}"
      subnet_vn: "{{ vnet_subnet_id | regex_search(vn_regex, '\\1') }}"
  vars:
      subnet_regex: '/subnets/(.+)'
      rg_regex: '/resourceGroups/(.+?)/'
      vn_regex: '/virtualNetworks/(.+?)/'

- name: Associate network resources with the node subnet
  azure_rm_subnet:
      name: "{{ subnet_name[0] }}"
      resource_group: "{{  subnet_rg[0] }}"
      virtual_network_name: "{{ subnet_vn[0] }}"
      security_group: "{{ nsg.ansible_facts.azure_securitygroups[0].id }}"
      route_table: "{{ routetable.route_tables[0].id }}"
```

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- De `node_resource_group` is de naam van de resourcegroep waarin de AKS-knooppunten worden gemaakt.
- De `vnet_subnet_id` is van het subnet in de vorige sectie hebt gemaakt.


## <a name="run-the-sample-playbook"></a>De voorbeeld-playbook uitvoeren

Deze sectie vindt u het volledige voorbeeld playbook die de taken maken in dit artikel worden aangeroepen. 

Sla het volgende playbook op als `aks-kubenet.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
      name: aksansibletest
      location: eastus
  tasks:
     - name: Ensure resource group exist
       azure_rm_resourcegroup:
           name: "{{ resource_group }}"
           location: "{{ location }}"

     - name: Create vnet
       include_tasks: vnet.yml

     - name: Create AKS
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
       include_tasks: aks.yml

     - name: Associate network resources with the node subnet
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
           node_resource_group: "{{ aks.node_resource_group }}"
       include_tasks: associate.yml

     - name: Get details of the AKS
       azure_rm_aks_facts:
           name: "{{ name }}"
           resource_group: "{{ resource_group }}"
           show_kubeconfig: user
       register: output

     - name: Show AKS cluster detail
       debug:
           var: output.aks[0]
```

In de `vars` sectie, moet u de volgende wijzigingen:

- Voor de `resource_group` sleutel, wijzigen de `aksansibletest` waarde die u wilt de naam van uw resourcegroep.
- Voor de `name` sleutel, wijzigen de `aksansibletest` waarde naar uw AKS-naam.
- Voor de `Location` sleutel, wijzigen de `eastus` waarde naar de locatie van uw resourcegroep.

Voer de volledige playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook aks-kubenet.yml
```

Uitvoeren van de playbook toont resultaten die lijkt op de volgende uitvoer:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Ensure resource group exist] 
ok: [localhost]

TASK [Create vnet] 
included: /home/devops/aks-kubenet/vnet.yml for localhost

TASK [Create vnet] 
ok: [localhost]

TASK [Create subnet] 
ok: [localhost]

TASK [Create AKS] 
included: /home/devops/aks-kubenet/aks.yml for localhost

TASK [List supported kubernetes version from Azure] 
 [WARNING]: Azure API profile latest does not define an entry for
ContainerServiceClient

ok: [localhost]

TASK [Create AKS cluster with vnet] 
changed: [localhost]

TASK [Associate network resources with the node subnet] 
included: /home/devops/aks-kubenet/associate.yml for localhost

TASK [Get route table] 
ok: [localhost]

TASK [Get network security group] 
ok: [localhost]

TASK [Parse subnet id] 
ok: [localhost]

TASK [Associate network resources with the node subnet] 
changed: [localhost]

TASK [Get details of the AKS] 
ok: [localhost]

TASK [Show AKS cluster detail] 
ok: [localhost] => {
    "output.aks[0]": {
        "id": /subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourcegroups/aksansibletest/providers/Microsoft.ContainerService/managedClusters/aksansibletest",
        "kube_config": "apiVersion: ...",
        "location": "eastus",
        "name": "aksansibletest",
        "properties": {
            "agentPoolProfiles": [
                {
                    "count": 3,
                    "maxPods": 110,
                    "name": "nodepool1",
                    "osDiskSizeGB": 100,
                    "osType": "Linux",
                    "storageProfile": "ManagedDisks",
                    "vmSize": "Standard_D2_v2",
                    "vnetSubnetID": "/subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourceGroups/aksansibletest/providers/Microsoft.Network/virtualNetworks/aksansibletest/subnets/aksansibletest"
                }
            ],
            "dnsPrefix": "aksansibletest",
            "enableRBAC": false,
            "fqdn": "aksansibletest-cda2b56c.hcp.eastus.azmk8s.io",
            "kubernetesVersion": "1.12.6",
            "linuxProfile": {
                "adminUsername": "azureuser",
                "ssh": {
                    "publicKeys": [
                        {
                            "keyData": "ssh-rsa ..."
                        }
                    ]
                }
            },
            "networkProfile": {
                "dnsServiceIP": "10.0.0.10",
                "dockerBridgeCidr": "172.17.0.1/16",
                "networkPlugin": "kubenet",
                "podCidr": "192.168.0.0/16",
                "serviceCidr": "10.0.0.0/16"
            },
            "nodeResourceGroup": "MC_aksansibletest_pcaksansibletest_eastus",
            "provisioningState": "Succeeded",
            "servicePrincipalProfile": {
                "clientId": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA"
            }
        },
        "type": "Microsoft.ContainerService/ManagedClusters"
    }
}

PLAY RECAP 
localhost                  : ok=15   changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer het niet meer nodig hebt, verwijdert u de resources die in dit artikel is gemaakt. 

Sla de volgende code als `cleanup.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
```

In de `vars` sectie, vervangt de `{{ resource_group_name }}` tijdelijke aanduiding door de naam van uw resourcegroep.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie - Azure Container netwerken Interface (CNI)-netwerken configureren in AKS met Ansible](./ansible-aks-configure-cni-networking.md)