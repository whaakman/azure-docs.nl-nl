---
title: Ansible (preview) gebruiken om webverkeer te beheren met Azure Application Gateway
description: Meer informatie over hoe u Ansible gebruikt om een Azure Application Gateway te maken en te configureren voor het beheren van webverkeer
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, azure application gateway, load balancer, webverkeer
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 02b98cb22d897fc9599f6e44ddc57ef4211b0893
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410814"
---
# <a name="manage-web-traffic-with-azure-application-gateway-using-ansible-preview"></a>Ansible (preview) gebruiken om webverkeer te beheren met Azure Application Gateway
[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) is een load balancer voor webverkeer waarmee u het verkeer naar uw webapps kunt beheren. 

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel leest u hoe u Ansible gebruikt om een Azure Application Gateway te maken en deze te gebruiken om het verkeer van twee webservers te beheren die worden uitgevoerd in Azure-containerinstanties. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het netwerk instellen
> * Twee Azure-containerinstanties maken met een httpd-installatiekopie
> * Een toepassingsgateway maken met bovenstaande Azure-containerinstanties in de back-endpool


## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 is vereist om de volgende playbooks-voorbeelden in deze zelfstudie uit te voeren. U kunt de RC-versie van Ansible 2.7 installeren door `sudo pip install ansible[azure]==2.7.0rc2` uit te voeren. Ansible 2.7 wordt uitgebracht in oktober 2018. Daarna hoeft u hier geen versie op te geven omdat de 2.7 de standaardversie is. 

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.  

In het volgende voorbeeld wordt een resourcegroep met de naam **myResourceGroup** gemaakt op de locatie **VS - oost**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Sla bovenstaand playbook op als *rg.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Netwerkbronnen maken 
U moet een virtueel netwerk maken om de toepassingsgateway in staat te stellen te communiceren met andere resources. 

In het volgende voorbeeld worden een virtueel netwerk met de naam **myVNet**, een subnet met de naam **myAGSubnet** en een openbaar IP-adres met de naam **myAGPublicIPAddress** met domein met de naam **mydomain** gemaakt. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    vnet_name: myVNet 
    subnet_name: myAGSubnet 
    publicip_name: myAGPublicIPAddress
    publicip_domain: mydomain
  tasks:
    - name: Create a virtual network
      azure_rm_virtualnetwork:
        name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
            - 10.1.0.0/16
            - 172.100.0.0/16
        dns_servers:
            - 127.0.0.1
            - 127.0.0.2

    - name: Create a subnet
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: 10.1.0.0/24

    - name: Create a public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}" 
        allocation_method: Dynamic
        name: "{{ publicip_name }}"
        domain_name_label: "{{ publicip_domain }}"
```

Sla bovenstaand playbook op als *vnet_create.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook vnet_create.yml
```

## <a name="create-backend-servers"></a>Back-endservers maken
In dit voorbeeld maakt u twee Azure-containerinstanties met httpd-installatiekopieën die worden gebruikt als back-endservers voor de toepassingsgateway.  

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Create a container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_1_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80

    - name: Create another container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_2_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80
```

Sla bovenstaand playbook op als *aci_create.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

We gaan nu een toepassingsgateway maken. In het volgende voorbeeld wordt een toepassingsgateway met de naam **myAppGateway** gemaakt, met back-end-, front-end- en http-configuratie.  

> [!div class="checklist"]
> * **appGatewayIP**, gedefinieerd in het **gateway_ip_configurations**-blok: subnetverwijzing is vereist voor de IP-configuratie van de gateway. 
> * **appGatewayBackendPool**, gedefinieerd in het **backend_address_pools**-blok: een toepassingsgateway moet minimaal één back-endadresgroep hebben. 
> * **appGatewayBackendHttpSettings**, gedefinieerd in het **backend_http_settings_collection**-blok: hiermee wordt aangegeven dat voor de communicatie poort 80 en een HTTP-protocol worden gebruikt. 
> * **appGatewayHttpListener**, gedefinieerd in het **backend_http_settings_collection**-blok: de standaard-listener voor appGatewayBackendPool. 
> * **appGatewayFrontendIP**, gedefinieerd in het **frontend_ip_configurations**-blok: wijst myAGPublicIPAddress toe aan appGatewayHttpListener. 
> * **rule1**, gedefinieerd in het **request_routing_rules**-blok: de standaardregel voor doorsturen voor appGatewayHttpListener. 

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    vnet_name: myVNet
    subnet_name: myAGSubnet
    location: eastus
    publicip_name: myAGPublicIPAddress
    appgw_name: myAppGateway
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Get info of Subnet
      azure_rm_resource_facts:
        api_version: '2018-08-01'
        resource_group: "{{ resource_group }}"
        provider: network
        resource_type: virtualnetworks
        resource_name: "{{ vnet_name }}"
        subresource:
          - type: subnets
            name: "{{ subnet_name }}"
      register: subnet

    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_1_name }}"
      register: aci_1_output
    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_2_name }}"
      register: aci_2_output

    - name: Create instance of Application Gateway
      azure_rm_appgateway:
        resource_group: "{{ resource_group }}"
        name: "{{ appgw_name }}"
        sku:
          name: standard_small
          tier: standard
          capacity: 2
        gateway_ip_configurations:
          - subnet:
              id: "{{ subnet.response[0].id }}"
            name: appGatewayIP
        frontend_ip_configurations:
          - public_ip_address: "{{ publicip_name }}"
            name: appGatewayFrontendIP
        frontend_ports:
          - port: 80
            name: appGatewayFrontendPort
        backend_address_pools:
          - backend_addresses:
              - ip_address: "{{ aci_1_output.response[0].properties.ipAddress.ip }}"
              - ip_address: "{{ aci_2_output.response[0].properties.ipAddress.ip }}"
            name: appGatewayBackendPool
        backend_http_settings_collection:
          - port: 80
            protocol: http
            cookie_based_affinity: enabled
            name: appGatewayBackendHttpSettings
        http_listeners:
          - frontend_ip_configuration: appGatewayFrontendIP
            frontend_port: appGatewayFrontendPort
            name: appGatewayHttpListener
        request_routing_rules:
          - rule_type: Basic
            backend_address_pool: appGatewayBackendPool
            backend_http_settings: appGatewayBackendHttpSettings
            http_listener: appGatewayHttpListener
            name: rule1
```

Sla bovenstaand playbook op als *appgw_create.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook appgw_create.yml
```

Het kan enkele minuten duren voordat de toepassingsgateway is gemaakt. 

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

In bovenstaand voorbeeld-playbook voor netwerkresources is het domein met de naam **mydomain** gemaakt in **eastus**. Nu kunt u naar de browser navigeren en `http://mydomain.eastus.cloudapp.azure.com` invoeren, waarna de volgende pagina wordt weergegeven waarop wordt bevestigd dat de Application Gateway werkt zoals verwacht.

![Application Gateway openen](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet nodig hebt, kunt u ze verwijderen door onderstaand voorbeeld uit te voeren. Hiermee verwijdert u een resourcegroep met de naam **myResourceGroup**. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Sla bovenstaand playbook op als *rg_delete*.yml. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Ansible in Azure](https://docs.microsoft.com/azure/ansible/)
