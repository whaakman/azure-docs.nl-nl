---
title: Maken van een gebruiker gedefinieerde route-route-netwerkverkeer via een virtueel netwerkapparaat - Azure CLI | Microsoft Docs
description: Informatie over het maken van een gebruiker gedefinieerde route voor het Azure standaard routering door Routering van netwerkverkeer via een virtueel netwerkapparaat onderdrukken.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: jdial
ms.openlocfilehash: adfcf53f9fca0efafb538edfd65b95313dcf1559
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-user-defined-route---azure-cli"></a>Maken van een gebruiker gedefinieerde route - Azure CLI

In deze zelfstudie meer informatie over het maken van de gebruiker gedefinieerde routes naar verkeer leiden tussen twee [virtueel netwerk](virtual-networks-overview.md) subnetten via een virtueel netwerkapparaat. Een virtueel netwerk-apparaat is een virtuele machine die wordt uitgevoerd een netwerktoepassing, zoals een firewall. Zie voor meer informatie over vooraf geconfigureerde virtuele netwerkapparaten die u in een Azure-netwerk implementeren kunt, de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Wanneer u subnetten in een virtueel netwerk maakt, maakt Azure standaard [systeemroutes](virtual-networks-udr-overview.md#system-routes) waarmee bronnen in alle subnetten om te communiceren met elkaar, zoals wordt weergegeven in de volgende afbeelding:

![Standaardroutes](./media/create-user-defined-route/default-routes.png)

In deze zelfstudie maakt u een virtueel netwerk maken met openbare, persoonlijke en DMZ subnetten, zoals wordt weergegeven in de volgende afbeelding. Doorgaans webservers kunnen worden geïmplementeerd met een openbare subnet en een toepassing of de database-server kan worden geïmplementeerd met een persoonlijke subnet. U een virtuele machine om te fungeren als een virtueel netwerkapparaat in het Perimeternetwerk subnet maken en desgewenst een virtuele machine maken in elk subnet die communiceren via het netwerk virtuele apparaat. Al het verkeer tussen de openbare en particuliere subnetten doorgestuurd via het apparaat, zoals wordt weergegeven in de volgende afbeelding:

![Door de gebruiker gedefinieerde routes](./media/create-user-defined-route/user-defined-routes.png)

Dit artikel bevat stappen voor het maken van een gebruiker gedefinieerde route via het Resource Manager-implementatiemodel het implementatiemodel dat wordt u aangeraden is bij het maken van de gebruiker gedefinieerde routes. Als u een door de gebruiker gedefinieerde route (klassiek) maken moet, Zie [een door de gebruiker gedefinieerde route (klassiek) maken](virtual-network-create-udr-classic-cli.md). Als u niet bekend met Azure implementatiemodellen bent, Zie [begrijpen Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Zie voor meer informatie over de gebruiker gedefinieerde routes, [overzicht van de gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined).

## <a name="create-routes-and-network-virtual-appliance"></a>Routes en netwerk virtueel apparaat maken

Azure CLI-opdrachten zijn hetzelfde, ongeacht of u de opdrachten vanaf Windows, Linux of Mac OS uitvoeren. Er zijn echter scripting verschillen tussen de houders van het besturingssysteem. De scripts in de volgende stappen uitvoeren in een installatie en uitvoering van Azure CLI-opdrachten in een Bash-shell vereisen. Kunt u [installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) op uw PC op, of klik op de **Try it** knop in een van de scripts uit te voeren van de scripts in de Azure-Cloud-Shell.
 
1. **Vereiste**: een virtueel netwerk maken met twee subnetten via de stappen in [een virtueel netwerk maken](#create-a-virtual-network).
2. Als de Azure CLI vanaf uw computer uitgevoerd, meld u aan bij Azure met de `az login` opdracht. Als de Cloud-Shell, bent u automatisch aangemeld.
3. Stel een paar variabelen die worden gebruikt in de resterende stappen:

    ```azurecli-interactive
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    ```

4. Maak een *DMZ* subnet in het virtuele netwerk gemaakt in de vereisten:

    ```azurecli-interactive
    az network vnet subnet create \
      --name DMZ \
      --address-prefix 10.0.2.0/24 \
      --vnet-name myVnet \
      --resource-group $rgName
    ```

5. Maak de NVA virtuele machine. Statische openbare en particuliere IP-adressen toewijzen aan de netwerkinterface die de CLI maakt. Statische adressen wijzigen voor de levensduur van de virtuele machine niet. De NVA mag een virtuele machine met Linux of Windows-besturingssysteem. De virtuele machine maken, kopieert u het script voor elk besturingssysteem en plak deze in de CLI. Als het script voor het maken van een Windows-VM in een teksteditor te plakken, wijzig de waarde voor de *AdminPassword* variabele en vervolgens de gewijzigde tekst plakken in uw CLI.

    **Linux**

    ```azurecli-interactive
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image UbuntuLTS \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

    **Windows**

    ```azurecli-interactive
    AdminPassword=ChangeToYourPassword
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image win2016datacenter \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --admin-password $AdminPassword      
    ```

6. Doorsturen via IP voor de netwerkinterface van de NVA inschakelen. IP wordt doorsturen voor een netwerkinterface Azure niet om te controleren van de bron/het doel-IP-adres. Als u deze instelling kan verkeer dat is bestemd voor een IP-adres dan de NIC die het ontvangt, niet inschakelt is door Azure verwijderd.

    ```azurecli-interactive
    az network nic update \
      --name myVm-NvaVMNic \
      --resource-group $rgName \
      --ip-forwarding true
    ```

7. Maken van een routetabel voor de *openbare* subnet.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Public \
      --resource-group $rgName
    ```
    
8. Standaard Azure routes verkeer tussen alle subnetten in een virtueel netwerk. Maak een route om routering zodat het verkeer van het openbare-subnet aan het subnet dat persoonlijke doorgestuurd via de NVA in plaats van rechtstreeks met het subnet voor persoonlijke Azure-standaardinstelling te wijzigen.

    ```azurecli-interactive    
    az network route-table route create \
      --name ToPrivateSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Public \
      --address-prefix 10.0.1.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

9. Koppel de *myRouteTable-openbare* routetabel aan de *openbare* subnet. Koppelen van een routetabel aan een subnet zorgt ervoor dat Azure voor het routeren van al het uitgaande verkeer vanuit het subnet op basis van de routes in de routetabel. Een routetabel kan worden gekoppeld zijn aan nul of meerdere subnetten, terwijl een subnet nul of één routetabel gekoppeld kan hebben.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Public \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Public
    ```

10. Maken van de routetabel voor de *persoonlijke* subnet.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Private \
      --resource-group $rgName
    ```
      
11. Maken van een route-route-verkeer van de *persoonlijke* subnet moet de *openbare* subnet via de NVA virtuele machine.

    ```azurecli-interactive
    az network route-table route create \
      --name ToPublicSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Private \
      --address-prefix 10.0.0.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

12. Koppelen van de routetabel voor de *persoonlijke* subnet.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Private \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Private
    ```
    
13. **Optioneel:** maken van een virtuele machine in de openbare en particuliere subnetten en valideren van de communicatie tussen de virtuele machines wordt doorgestuurd via het netwerk virtuele apparaat via de stappen in [routeringvalideren](#validate-routing).
14. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-resources).

## <a name="validate-routing"></a>Routering valideren

1. Als u nog niet gedaan hebt, voer de stappen in [routes en virtueel netwerkapparaat maken](#create-routes-and-network-virtual-appliance).
2. Klik op de **Try it** knop in het vak dat wordt gevolgd door, die de Azure-Cloud-Shell geopend. Als u wordt gevraagd, meldt u zich voor het gebruik van Azure in uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p). De Azure-Cloud-Shell is een gratis bash-shell met de Azure-opdrachtregelinterface vooraf is geïnstalleerd. 

    De volgende scripts maakt twee virtuele machines, één in de *openbare* subnet en één in de *persoonlijke* subnet. Doorsturen via IP voor de netwerkinterface binnen het besturingssysteem van de NVA waarmee het besturingssysteem voor het routeren van verkeer via de netwerkinterface wordt ook inschakelen door de scripts. Een productie NVA doorgaans het verkeer inspecteert voordat deze routering, maar in deze zelfstudie de eenvoudige NVA alleen het verkeer gerouteerd zonder deze te bekijken. 

    Klik op de **kopie** knop in de **Linux** of **Windows** scripts die volgen en de inhoud van het script in een teksteditor plakken. Wijzig het wachtwoord voor de *adminPassword* variabele vervolgens plakken het script in de Azure-Cloud-Shell. Voer het script voor het besturingssysteem die u hebt geselecteerd toen u het virtuele netwerk-apparaat hebt gemaakt in stap 5 van [routes en virtueel netwerkapparaat maken](#create-routes-and-network-virtual-appliance). 

    **Linux**

    ```azurecli-interactive
    #!/bin/bash

    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.    
    az vm extension set \
      --resource-group $rgName \
      --vm-name myVm-Nva \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
    ```

    **Windows**

    ```azurecli-interactive

    #!/bin/bash
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Public \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Private \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1"}'

    # Restart the NVA virtual machine.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Restart-Computer -ComputerName myVm-Nva -Force"}'
    ```

3. Communicatie tussen de virtuele machines in de openbare en particuliere subnetten valideren. 

    - Open een [SSH](../virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) (Linux) of [extern bureaublad](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) (Windows) verbinding met het openbare IP-adres van de *myVm-openbare* virtuele machine.
    - Vanaf een opdrachtprompt op de *myVm-openbare* virtuele machine, voer `ping myVm-Private`. U ontvangt antwoorden omdat de NVA het verkeer van de openbare naar het persoonlijke subnet stuurt.
    - Van de *myVm-openbare* virtuele machine, voer de opdracht Traceroute tussen de virtuele machines in de openbare en particuliere subnetten. Voer de juiste opdracht die volgt, afhankelijk van welk besturingssysteem die u in de virtuele machines in de openbare en particuliere subnetten hebt geïnstalleerd:
        - **Windows**: uitvoeren vanaf een opdrachtprompt de `tracert myvm-private` opdracht.
        - **Ubuntu**: Voer de `tracepath myvm-private` opdracht.
      Verkeer wordt doorgegeven via 10.0.2.4 (NVA) voordat 10.0.1.4 (de virtuele machine in het persoonlijke subnet) is bereikt. 
    - De vorige stappen voltooien door verbinding te maken met de *myVm privé-* virtuele machine en ping het *myVm-openbare* virtuele machine. De opdracht Traceroute toont communicatie via 10.0.2.4 onderweg alvorens 10.0.0.4 (de virtuele machine in de openbare subnet).
      
      > [!NOTE]
      > De vorige stappen kunnen u bevestigen routering tussen Azure privé IP-adressen. Als u worden doorgestuurd of proxy wilt, adressen verkeer naar openbare IP-adres via een virtueel netwerkapparaat:
      > - Het apparaat moet opgeven voor NAT of proxy-mogelijkheden. Als netwerkadresomzetting, het apparaat de bron-IP vertalen moet-adres in een eigen en vervolgens doorsturen die aanvraag naar het openbare IP-adres. Of het apparaat heeft netwerkadres vertaald adres van de bronserver of proxy, Azure zet de virtuele netwerkapparaat van privé IP-adres voor een openbaar IP-adres. Zie voor meer informatie over de verschillende methoden Azure gebruikt om te vertalen van privé IP-adressen voor openbare IP-adressen, [inzicht in uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
      > - Een extra route in de routetabel zoals voorvoegsel: 0.0.0.0/0, volgend hoptype VirtualAppliance en volgende hop-IP-adres 10.0.2.4 (in het vorige voorbeeldscript).
      >
    - **Eventueel**: Gebruik de volgende hop-mogelijkheden van Azure-netwerk-Watcher voor het valideren van de volgende hop tussen twee virtuele machines in Azure. Voordat u de netwerk-Watcher, moet u eerst [maken van een exemplaar van Azure-netwerk-Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor de regio die u wilt gebruiken in. In deze zelfstudie wordt de regio VS-Oost gebruikt. Nadat u een exemplaar van de netwerk-Watcher voor de regio hebt ingeschakeld, voer de volgende opdracht om te zien van de volgende hop informatie tussen de virtuele machines in de openbare en particuliere subnetten:
     
        ```azurecli-interactive
        az network watcher show-next-hop --resource-group myResourceGroup --vm myVm-Public --source-ip 10.0.0.4 --dest-ip 10.0.1.4
        ```

       Retourneert de uitvoer *10.0.2.4* als de **nextHopIpAddress** en *VirtualAppliance* als de **nextHopType**.

> [!NOTE]
> Ter illustratie van de concepten in deze zelfstudie, openbare IP-adressen zijn toegewezen aan de virtuele machines in de openbare en particuliere subnetten en alle toegang tot het netwerk poort voor zowel virtuele machines in Azure is ingeschakeld. Bij het maken van virtuele machines voor gebruik in productieomgevingen u mogelijk geen openbare IP-adressen toewijzen aan deze en netwerkverkeer naar de persoonlijke subnet mogelijk worden gefilterd door het implementeren van een virtueel netwerkapparaat plaatsen of door een netwerkbeveiligingsgroep toewijzen aan de subnetten netwerkinterface of beide. Zie voor meer informatie over netwerkbeveiligingsgroepen, [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Deze zelfstudie vereist een bestaand virtueel netwerk met twee subnetten. Klik op de **Try it** knop in het vak dat volgt, snel een virtueel netwerk maken. Te klikken op de **Try it** knop wordt geopend de [Azure Cloud Shell](../cloud-shell/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Hoewel de Shell Cloud wordt uitgevoerd van PowerShell- of Bash-shell in deze sectie, wordt de Bash-shell wordt gebruikt voor het maken van het virtuele netwerk. De Bash-shell heeft de Azure-opdrachtregelinterface geïnstalleerd. Als u wordt gevraagd door de Cloud-Shell, meldt u zich voor het gebruik van Azure in uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p). Klik op het virtuele netwerk in deze zelfstudie gebruikt om de **kopie** knop in het onderstaande vak en klik vervolgens het script in de Azure-Cloud-Shell plakken:

```azurecli-interactive
#!/bin/bash

#Set variables used in the script.
rgName="myResourceGroup"
location="eastus"

# Create a resource group.
az group create \
  --name $rgName \
  --location $location

# Create a virtual network with one subnet named Public.
az network vnet create \
  --name myVnet \
  --resource-group $rgName \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24

# Create an additional subnet named Private in the virtual network.
az network vnet subnet create \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --vnet-name myVnet \
  --resource-group $rgName
```

Zie voor meer informatie over het gebruik van de portal, PowerShell of een Azure Resource Manager-sjabloon maken van een virtueel netwerk, [een virtueel netwerk maken](virtual-networks-create-vnet-arm-pportal.md).

## <a name="delete-resources"></a>Resources verwijderen

Als u deze zelfstudie hebt voltooid, is het raadzaam de resources verwijderen die u hebt gemaakt, zodat u geen gebruik kosten. Verwijderen van een resourcegroep, verwijdert tevens alle bronnen die zich in de resourcegroep. Voer de volgende opdracht in een sessie CLI:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

- Maak een [maximaal beschikbare virtuele netwerkapparaat](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuele netwerkapparaten hebben vaak meerdere netwerkinterfaces en IP-adressen aan hen toegewezen. Meer informatie over hoe [netwerkinterfaces toevoegen aan een bestaande virtuele machine](virtual-network-network-interface-vm.md#vm-add-nic) en [IP-adressen toevoegen aan een bestaande netwerkinterface](virtual-network-network-interface-addresses.md#add-ip-addresses). Hoewel alle grootten van virtuele machines ten minste twee netwerkinterfaces die zijn gekoppeld aan deze hebben kunnen, de grootte van elke virtuele machine biedt ondersteuning voor het maximale aantal netwerkinterfaces. Zie voor meer informatie over het aantal netwerkinterfaces elke virtuele machine ondersteunt de grootte, [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) en [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) grootten van virtuele machines. 
- Maken van een gebruiker gedefinieerde route om af te dwingen tunnel verkeer lokale via een [site-naar-site VPN-verbinding](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
