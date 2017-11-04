---
title: Maken en beheren van een Windows-VM in Azure met behulp van Python | Microsoft Docs
description: Leer hoe u Python maken en beheren van een Windows-VM in Azure.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: davidmu
ms.openlocfilehash: bb777d41570d7b1dc97402d532519488912948e3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a>Maken en beheren van Windows-machines in Azure met Python

Een [Azure virtuele Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) moet meerdere ondersteunende Azure-resources. In dit artikel bevat informatie over het maken, beheren en het verwijderen van de VM-netwerkbronnen met behulp van Python. Procedures voor:

> [!div class="checklist"]
> * Een Visual Studio-project maken
> * Pakketten installeren
> * Referenties maken
> * Resources maken
> * Beheertaken uitvoeren
> * Resources verwijderen
> * De toepassing uitvoeren

Het duurt ongeveer 20 minuten aan deze stappen uit te voeren.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Als u nog niet gedaan hebt, installeert u [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Selecteer **ontwikkeling van een Python** op de pagina werkbelastingen en klik vervolgens op **installeren**. In de samenvatting, kunt u zien dat **Python 3 64-bits (3.6.0)** automatisch voor u is geselecteerd. Als u Visual Studio al hebt geïnstalleerd, kunt u de Python-werkbelasting met de Visual Studio starten toevoegen.
2. Klik na het installeren en starten van de Visual Studio, op **bestand** > **nieuw** > **Project**.
3. Klik op **sjablonen** > **Python** > **Python toepassing**, voer *myPythonProject* voor de naam van het project, selecteer de locatie van het project en klik vervolgens op **OK**.

## <a name="install-packages"></a>Pakketten installeren

1. Klik in Solution Explorer onder *myPythonProject*, met de rechtermuisknop op **Python-omgevingen**, en selecteer vervolgens **virtuele omgeving toevoegen**.
2. Accepteer de standaardnaam van op het scherm voor het toevoegen van virtuele omgeving *env*, zorg ervoor dat *Python 3.6 (64-bits)* is ingeschakeld voor de basisinterpreter en klik vervolgens op **maken**.
3. Met de rechtermuisknop op de *env* omgeving die u hebt gemaakt, klikt u op **Install Python Package**, voer *azure* in het zoekvak en druk op Enter.

U ziet in het windows-uitvoer dat de azure-pakketten zijn geïnstalleerd. 

## <a name="create-credentials"></a>Referenties maken

Voordat u deze stap, zorg ervoor dat u hebt een [Active Directory-service-principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md). U moet ook de toepassings-ID, de verificatiesleutel en de tenant-ID die u nodig hebt in een later stadium vastleggen.

1. Open *myPythonProject.py* -bestand dat is gemaakt en vervolgens deze code toevoegen om te zorgen dat uw toepassing om uit te voeren:

    ```python
    if __name__ == "__main__":
    ```

2. Toevoegen voor het importeren van de code die nodig is, deze instructies boven aan het bestand .py:

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. Vervolgens in het bestand .py variabelen toevoegen na de importinstructies algemene waarden die worden gebruikt in de code opgeven:
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    Vervang **abonnement-id** met uw abonnements-id.

4. Toevoegen voor het maken van de Active Directory-referenties die u wilt aanvragen, deze functie na de variabelen in het bestand .py:

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    Vervang **toepassing-id**, **verificatiesleutel**, en **tenant-id** met de waarden die u eerder hebt verzameld wanneer u uw Azure Active Directory-service-principal gemaakt.

5. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a>Resources maken
 
### <a name="initialize-management-clients"></a>Van beheerclients te initialiseren

Van beheerclients zijn nodig voor het maken en beheren van resources met behulp van de SDK voor Python in Azure. Voor het maken van de management-clients, voeg deze code onder de **als** instructie op vervolgens einde van het bestand .py:

```python
resource_group_client = ResourceManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
network_client = NetworkManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
compute_client = ComputeManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
```

### <a name="create-the-vm-and-supporting-resources"></a>De virtuele machine en de ondersteunende resources maken

Alle resources moeten zijn opgenomen in een [resourcegroep](../../azure-resource-manager/resource-group-overview.md).

1. Voeg deze functie na de variabelen in het bestand .py voor het maken van een resourcegroep:

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter to continue...')
    ```

[Beschikbaarheidssets](tutorial-availability-sets.md) het eenvoudiger voor u de virtuele machines die worden gebruikt door uw toepassing te onderhouden.

1. Voeg deze functie na de variabelen in het bestand .py voor het maken van een beschikbaarheidsset:
   
    ```python
    def create_availability_set(compute_client):
        avset_params = {
            'location': LOCATION,
            'sku': { 'name': 'Aligned' },
            'platform_fault_domain_count': 3
        }
        availability_set_result = compute_client.availability_sets.create_or_update(
            GROUP_NAME,
            'myAVSet',
            avset_params
        )
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter to continue...')
    ```

Een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) nodig is om te communiceren met de virtuele machine.

1. Voeg deze functie na de variabelen in het bestand .py voor het maken van een openbaar IP-adres voor de virtuele machine:

    ```python
    def create_public_ip_address(network_client):
        public_ip_addess_params = {
            'location': LOCATION,
            'public_ip_allocation_method': 'Dynamic'
        }
        creation_result = network_client.public_ip_addresses.create_or_update(
            GROUP_NAME,
            'myIPAddress',
            public_ip_addess_params
        )

        return creation_result.result()
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

Een virtuele machine moet worden in een subnet van een [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

1. Voeg deze functie na de variabelen in het bestand .py voor het maken van een virtueel netwerk:

    ```python
    def create_vnet(network_client):
        vnet_params = {
            'location': LOCATION,
            'address_space': {
                'address_prefixes': ['10.0.0.0/16']
            }
        }
        creation_result = network_client.virtual_networks.create_or_update(
            GROUP_NAME,
            'myVNet',
            vnet_params
        )
        return creation_result.result()
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

3. Toevoegen als een subnet toevoegen aan het virtuele netwerk, deze functie na de variabelen in het bestand .py:
    
    ```python
    def create_subnet(network_client):
        subnet_params = {
            'address_prefix': '10.0.0.0/24'
        }
        creation_result = network_client.subnets.create_or_update(
            GROUP_NAME,
            'myVNet',
            'mySubnet',
            subnet_params
        )

        return creation_result.result()
    ```
        
4. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

Een virtuele machine moet een netwerkinterface om te communiceren met het virtuele netwerk.

1. Voeg deze functie na de variabelen in het bestand .py voor het maken van een netwerkinterface:

    ```python
    def create_nic(network_client):
        subnet_info = network_client.subnets.get(
            GROUP_NAME, 
            'myVNet', 
            'mySubnet'
        )
        publicIPAddress = network_client.public_ip_addresses.get(
            GROUP_NAME,
            'myIPAddress'
        )
        nic_params = {
            'location': LOCATION,
            'ip_configurations': [{
                'name': 'myIPConfig',
                'public_ip_address': publicIPAddress,
                'subnet': {
                    'id': subnet_info.id
                }
            }]
        }
        creation_result = network_client.network_interfaces.create_or_update(
            GROUP_NAME,
            'myNic',
            nic_params
        )

        return creation_result.result()
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

Nu u de ondersteunende resources hebt gemaakt, kunt u een virtuele machine maken.

1. Voeg deze functie na de variabelen in het bestand .py voor het maken van de virtuele machine:
   
    ```python
    def create_vm(network_client, compute_client):  
        nic = network_client.network_interfaces.get(
            GROUP_NAME, 
            'myNic'
        )
        avset = compute_client.availability_sets.get(
            GROUP_NAME,
            'myAVSet'
        )
        vm_parameters = {
            'location': LOCATION,
            'os_profile': {
                'computer_name': VM_NAME,
                'admin_username': 'azureuser',
                'admin_password': 'Azure12345678'
            },
            'hardware_profile': {
                'vm_size': 'Standard_DS1'
            },
            'storage_profile': {
                'image_reference': {
                    'publisher': 'MicrosoftWindowsServer',
                    'offer': 'WindowsServer',
                    'sku': '2012-R2-Datacenter',
                    'version': 'latest'
                }
            },
            'network_profile': {
                'network_interfaces': [{
                    'id': nic.id
                }]
            },
            'availability_set': {
                'id': avset.id
            }
        }
        creation_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm_parameters
        )
    
        return creation_result.result()
    ```

    > [!NOTE]
    > Deze zelfstudie maakt een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van de andere afbeeldingen, [navigeren door en selecteren van installatiekopieën van virtuele machine van Azure met Windows PowerShell en Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

## <a name="perform-management-tasks"></a>Beheertaken uitvoeren

Tijdens de levenscyclus van een virtuele machine, kan u wilt uitvoeren van beheertaken, zoals starten, stoppen of een virtuele machine wordt verwijderd. Bovendien wilt u maken code om herhaalde of complexe taken te automatiseren.

### <a name="get-information-about-the-vm"></a>Informatie ophalen over de virtuele machine

1. Voor informatie over de virtuele machine, voeg deze functie na de variabelen in het bestand .py:

    ```python
    def get_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME, expand='instanceView')
        print("hardwareProfile")
        print("   vmSize: ", vm.hardware_profile.vm_size)
        print("\nstorageProfile")
        print("  imageReference")
        print("    publisher: ", vm.storage_profile.image_reference.publisher)
        print("    offer: ", vm.storage_profile.image_reference.offer)
        print("    sku: ", vm.storage_profile.image_reference.sku)
        print("    version: ", vm.storage_profile.image_reference.version)
        print("  osDisk")
        print("    osType: ", vm.storage_profile.os_disk.os_type.value)
        print("    name: ", vm.storage_profile.os_disk.name)
        print("    createOption: ", vm.storage_profile.os_disk.create_option.value)
        print("    caching: ", vm.storage_profile.os_disk.caching.value)
        print("\nosProfile")
        print("  computerName: ", vm.os_profile.computer_name)
        print("  adminUsername: ", vm.os_profile.admin_username)
        print("  provisionVMAgent: {0}".format(vm.os_profile.windows_configuration.provision_vm_agent))
        print("  enableAutomaticUpdates: {0}".format(vm.os_profile.windows_configuration.enable_automatic_updates))
        print("\nnetworkProfile")
        for nic in vm.network_profile.network_interfaces:
            print("  networkInterface id: ", nic.id)
        print("\nvmAgent")
        print("  vmAgentVersion", vm.instance_view.vm_agent.vm_agent_version)
        print("    statuses")
        for stat in vm_result.instance_view.vm_agent.statuses:
            print("    code: ", stat.code)
            print("    displayStatus: ", stat.display_status)
            print("    message: ", stat.message)
            print("    time: ", stat.time)
        print("\ndisks");
        for disk in vm.instance_view.disks:
            print("  name: ", disk.name)
            print("  statuses")
            for stat in disk.statuses:
                print("    code: ", stat.code)
                print("    displayStatus: ", stat.display_status)
                print("    time: ", stat.time)
        print("\nVM general status")
        print("  provisioningStatus: ", vm.provisioning_state)
        print("  id: ", vm.id)
        print("  name: ", vm.name)
        print("  type: ", vm.type)
        print("  location: ", vm.location)
        print("\nVM instance status")
        for stat in vm.instance_view.statuses:
            print("  code: ", stat.code)
            print("  displayStatus: ", stat.display_status)
    ```
2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter to continue...')
    ```

### <a name="stop-the-vm"></a>De virtuele machine stoppen

U kunt een virtuele machine stoppen en alle bijbehorende instellingen behouden, maar blijven in rekening gebracht voor of u kunt een virtuele machine stoppen en deze ongedaan gemaakt. Wanneer een virtuele machine in de toewijzing is opgeheven, zijn alle resources die zijn gekoppeld ook toewijzing ongedaan is gemaakt en facturering eindigt voor.

1. Toevoegen om te stoppen van de virtuele machine zonder deze toewijzing, deze functie na de variabelen in het bestand .py:

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    Als u wilt dat de virtuele machine ongedaan, wijzigt u de aanroep power_off deze code:

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    stop_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="start-the-vm"></a>Start de virtuele machine

1. Voeg deze functie na de variabelen in het bestand .py voor het starten van de virtuele machine:

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    start_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="resize-the-vm"></a>De virtuele machine vergroten of verkleinen

Veel aspecten van de implementatie moeten worden beschouwd bij het kiezen van een grootte voor de virtuele machine. Zie voor meer informatie [VM-grootten](sizes.md).

1. Als de grootte van de virtuele machine wijzigen, voeg deze functie na de variabelen in het bestand .py:

    ```python
    def update_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        vm.hardware_profile.vm_size = 'Standard_DS3'
        update_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm
        )

    return update_result.result()
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter to continue...')
    ```

### <a name="add-a-data-disk-to-the-vm"></a>Een gegevensschijf toevoegen aan de virtuele machine

Virtuele machines hebben een of meer [gegevensschijven](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) die als virtuele harde schijven zijn opgeslagen.

1. Toevoegen als een gegevensschijf toevoegen aan de virtuele machine, deze functie na de variabelen in het bestand .py: 

    ```python
    def add_datadisk(compute_client):
        disk_creation = compute_client.disks.create_or_update(
            GROUP_NAME,
            'myDataDisk1',
            {
                'location': LOCATION,
                'disk_size_gb': 1,
                'creation_data': {
                    'create_option': DiskCreateOption.empty
                }
            }
        )
        data_disk = disk_creation.result()
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        add_result = vm.storage_profile.data_disks.append({
            'lun': 1,
            'name': 'myDataDisk1',
            'create_option': DiskCreateOption.attach,
            'managed_disk': {
                'id': data_disk.id
            }
        })
        add_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME,
            VM_NAME,
            vm)

        return add_result.result()
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter to continue...')
    ```

## <a name="delete-resources"></a>Resources verwijderen

Omdat u in rekening voor resources die worden gebruikt in Azure gebracht, maar het is altijd een goede gewoonte resources verwijderen die niet langer nodig zijn. Als u wilt verwijderen van de virtuele machines en de ondersteunende resources, is hoeft u de resourcegroep verwijderen.

1. Voeg deze functie na de variabelen in het bestand .py voor het verwijderen van de resourcegroep en alle resources:
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. Als u de functie die u eerder hebt toegevoegd, voeg deze code onder de **als** instructie aan het einde van het bestand .py:
   
    ```python
    delete_resources(resource_group_client)
    ```

3. Sla *myPythonProject.py*.

## <a name="run-the-application"></a>De toepassing uitvoeren

1. Als u wilt de consoletoepassing uitvoeren, klikt u op **Start** in Visual Studio.

2. Druk op **Enter** nadat de status van elke bron wordt geretourneerd. In de statusinformatie ziet u een **geslaagd** Inrichtingsstatus. Nadat de virtuele machine is gemaakt, hebt u de mogelijkheid om te verwijderen van alle resources die u maakt. Voordat u op **Enter** om te starten als u resources verwijdert, u kan even duren om te controleren of het maken in de Azure portal. Als u de Azure portal openen, moet u wellicht de blade als u nieuwe resources wilt vernieuwen.  

    Het moet over vijf minuten duren voordat deze consoletoepassing volledig uitvoeren vanaf het begin te voltooien. Duurt enkele minuten nadat de toepassing is voltooid voordat alle resources en de resourcegroep worden verwijderd.


## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de implementatie zijn, raadpleegt u als volgende stap [Problemen met resourcegroepimplementaties in Azure Portal oplossen](../../resource-manager-troubleshoot-deployments-portal.md).
- Meer informatie over de [Azure Python-bibliotheek](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)

