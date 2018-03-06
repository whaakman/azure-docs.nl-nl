---
title: Maken van een gebruiker gedefinieerde route-route-netwerkverkeer via een virtueel netwerkapparaat - Azure-portal | Microsoft Docs
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
ms.openlocfilehash: 0319029277091611673f15c94604604850cbfcbe
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="create-a-user-defined-route---azure-portal"></a>Maken van een gebruiker gedefinieerde route - Azure-portal

In deze zelfstudie meer informatie over het maken van de gebruiker gedefinieerde routes naar verkeer leiden tussen twee [virtueel netwerk](virtual-networks-overview.md) subnetten via een virtueel netwerkapparaat. Een virtueel netwerk-apparaat is een virtuele machine die wordt uitgevoerd een netwerktoepassing, zoals een firewall. Zie voor meer informatie over vooraf geconfigureerde virtuele netwerkapparaten die u in een Azure-netwerk implementeren kunt, de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Wanneer u subnetten in een virtueel netwerk maakt, maakt Azure standaard [systeemroutes](virtual-networks-udr-overview.md#system-routes) waarmee bronnen in alle subnetten om te communiceren met elkaar, zoals wordt weergegeven in de volgende afbeelding:

![Standaardroutes](./media/create-user-defined-route/default-routes.png)

In deze zelfstudie maakt u een virtueel netwerk maken met openbare, persoonlijke en DMZ subnetten, zoals wordt weergegeven in de volgende afbeelding. Doorgaans webservers kunnen worden geïmplementeerd met een openbare subnet en een toepassing of de database-server kan worden geïmplementeerd met een persoonlijke subnet. U een virtuele machine om te fungeren als een virtueel netwerkapparaat in het Perimeternetwerk subnet maken en desgewenst een virtuele machine maken in elk subnet die communiceren via het netwerk virtuele apparaat. Al het verkeer tussen de openbare en particuliere subnetten doorgestuurd via het apparaat, zoals wordt weergegeven in de volgende afbeelding:

![Door de gebruiker gedefinieerde routes](./media/create-user-defined-route/user-defined-routes.png)

Dit artikel bevat stappen voor het maken van een gebruiker gedefinieerde route via het Resource Manager-implementatiemodel het implementatiemodel dat wordt u aangeraden is bij het maken van de gebruiker gedefinieerde routes. Als u een door de gebruiker gedefinieerde route (klassiek) maken moet, Zie [een door de gebruiker gedefinieerde route (klassiek) maken](virtual-network-create-udr-classic-ps.md). Als u niet bekend met Azure implementatiemodellen bent, Zie [begrijpen Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Zie voor meer informatie over de gebruiker gedefinieerde routes, [overzicht van de gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined).

## <a name="create-routes-and-network-virtual-appliance"></a>Routes en netwerk virtueel apparaat maken

1. **Vereiste**: een virtueel netwerk maken met twee subnetten via de stappen in [een virtueel netwerk maken](#create-a-virtual-network).
2. Nadat u het virtuele netwerk hebt gemaakt in een internetbrowser, gaat u naar de [Azure-portal](https://portal.azure.com). Meld u aan met uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).
3. Op de **zoeken bronnen** vak aan de bovenkant van de portal *myResourceGroup*. Klik op **myResourceGroup** wanneer deze wordt weergegeven in de zoekresultaten. De resourcegroep is gemaakt als onderdeel van de vereiste.
4. Klik op **myVnet**, zoals wordt weergegeven in de volgende afbeelding:

    ![Netwerkinterface-instellingen](./media/create-user-defined-route/virtual-network.png)

5. Een subnet maken voor het virtuele netwerkapparaat:
 
    - Onder **myVnet**, klikt u op **subnetten** onder **instellingen** aan de linkerkant.
    - Klik op **+ Subnet**, zoals wordt weergegeven in de volgende afbeelding:

        ![Subnetten](./media/create-user-defined-route/subnets.png) 
    - Voer de volgende waarden onder **subnet toevoegen**, klikt u vervolgens op **OK**:

        |Instelling|Waarde|
        |-----|-----|
        |Naam|DMZ|
        |Adresbereik (CIDR-blok)|10.0.2.0/24|

6. Een virtuele machine van de netwerk virtueel apparaat maken:

    - Klik aan de linkerkant van de portal op **+ nieuw**, klikt u vervolgens op **Compute**, en klik vervolgens op **Windows Server 2016 Datacenter** of **Ubuntu Server 16.04 LTS**.
    - Voer de volgende waarden op de **basisbeginselen** blade die wordt weergegeven, klikt u vervolgens op **OK**.

        |Instelling|Waarde|
        |---|---|
        |Naam|myVm Nva|
        |Gebruikersnaam|azureuser|
        |Wachtwoord en wachtwoord bevestigen|Een wachtwoord van uw keuze|
        |Abonnement|Selecteer uw abonnement|
        |Resourcegroep|Klik op **gebruik bestaande**, selecteer daarna **myResourceGroup**|
        |Locatie|VS - oost|
    - Op de **een grootte kiezen** blade die wordt weergegeven, klikt u op **DS1_V2 standaard**, klikt u vervolgens op **Selecteer**.
    - Op de **instellingen** blade die wordt weergegeven, klikt u op **virtueel netwerk**. Klik op **myVnet** in de **virtueel netwerk kiezen** blade die wordt weergegeven.
    - Op de **instellingen** blade, klikt u op **Subnet**. Klik op **DMZ** op de **Kies subnet** blade die wordt weergegeven. 
    - Laat de standaardwaarden voor de overige instellingen en klik op **OK**.
    - Klik op **maken** op de **maken** blade die wordt weergegeven. Implementatie van de virtuele machine duurt een paar minuten.

    > [!NOTE]
    > Naast het maken van de virtuele machine, de Azure portal maakt u een openbaar IP-adres en toegewezen aan de virtuele machine is standaard. Als u de virtuele machine in een productieomgeving zijn implementeert, kunt u niet aan een openbaar IP-adres toewijzen aan de virtuele machine. In plaats daarvan kunt u toegang tot het virtuele netwerk-apparaat bij andere virtuele machines binnen het virtuele netwerk. Zie voor meer informatie over openbare IP-adressen, [beheren van een openbaar IP-adres](virtual-network-public-ip-address.md).

7. Een statisch privé IP-adres toewijzen en schakel doorsturen via IP voor de netwerkinterface de portal in de vorige stap hebt gemaakt. 
    - Op de **zoeken bronnen** vak boven aan de pagina *myVm Nva*.
    - Klik op **myVm Nva** wanneer deze wordt weergegeven in de zoekresultaten.
    - Klik op **Networking** onder **instellingen** aan de linkerkant.
    - Klik op de naam van de netwerkinterface onder **myVm-Nva - netwerkinterfaces**. De naam is **myvm nva***X*, waarbij *X* is een getal dat is toegewezen door de portal.
    - Klik op **IP-configuraties** onder **instellingen** voor de netwerkinterface, zoals wordt weergegeven in de volgende afbeelding:

        ![Netwerkinterface-instellingen](./media/create-user-defined-route/network-interface-settings.png)
        
    - Klik op **ingeschakeld** voor de **doorsturen via IP** instellen, klikt u vervolgens op **opslaan**. Doorsturen via IP moet zijn ingeschakeld voor elke netwerkinterface die niet is geadresseerd aan een IP-adres toegewezen verkeer ontvangt. Hiermee schakelt u doorsturen via IP inschakelen controle van de Azure bron/het doel voor de netwerkinterface.
    - Klik op **ipconfig1** in de lijst met IP-configuraties.
    - Klik op **statische** voor **toewijzing** van de privé IP-adres onder **ipconfig1**, zoals wordt weergegeven in de volgende afbeelding:

        ![IP-configuratie](./media/create-user-defined-route/ip-configuration.png)
    - Zoals u in de afbeelding hierboven, geef *10.0.2.4* onder **IP-adres** in de **particuliere IP-adresinstellingen**. Een statisch IP-adres toewijzen aan de netwerkinterface, zorgt u ervoor dat het adres voor de duur van de netwerkinterface is gekoppeld aan virtuele machine niet wijzigen. Het opgegeven adres is momenteel niet toegewezen aan een andere resource in het subnet DMZ die de netwerkinterface is in. 
    - Om de configuratie op te slaan, klikt u op **opslaan** onder **ipconfig1**. Sluit het vak ipconfig1 niet totdat u een melding ontvangt in de portal die de netwerkinterface is opgeslagen.
 
8. Twee routetabellen maken. Routetabellen bevatten nul of meer routes:

    - Klik aan de linkerkant van de portal op **+ nieuw** > **Networking** > **routetabel**.
    - Voer de volgende waarden onder **een routetabel maken**, en klik vervolgens op **maken**:

        |Instelling|Waarde|
        |---|---|
        |Naam|myRouteTable-openbare|
        |Abonnement|Selecteer uw abonnement|
        |Resourcegroep|Selecteer **gebruik bestaande**, selecteer daarna **myResourceGroup**|
        |Locatie|VS - oost|
    
    - De vorige substappen van stap 8 opnieuw uitvoeren, maar de naam van de routetabel *myRouteTable privé-*.
9. Toevoegen van de routes naar de *myRouteTable-openbare* routetabel en koppel de routetabel voor de *openbare* subnet:

    - Op de **zoeken bronnen** vak aan de bovenkant van de portal *myRouteTable-openbare*. Klik op **myRouteTable-openbare** wanneer deze wordt weergegeven in de zoekresultaten.
    - Onder **myRouteTable-openbare**, klikt u op **Routes** in de lijst met **instellingen**.
    - Klik op **+ toevoegen** onder **myRouteTable-openbare - Routes**.
    -  Standaard Azure routes verkeer tussen alle subnetten in een virtueel netwerk. Maken van een route als u wilt wijzigen van Azure standaard routering zodanig dat verkeer van de *openbare* subnet doorgestuurd via de NVA in plaats van rechtstreeks naar de *persoonlijke* subnet. Voer de volgende waarden op de **Add route** blade die wordt weergegeven en klik vervolgens op **OK**:

        |Instelling|Waarde|Uitleg|
        |---|---|---|
        |De routenaam van de|ToPrivateSubnet|Deze route stuurt het verkeer naar het subnet van de persoonlijke via het netwerk virtuele apparaat.|
        |Adresvoorvoegsel|10.0.1.0/24| Alle verkeer dat wordt verzonden naar adressen binnen deze adresvoorvoegsel (10.0.1.0 - 10.0.1.254) wordt verzonden naar het virtuele netwerk-apparaat.|
        |Volgend hoptype| Selecteer **virtueel apparaat**||
        |Adres van de volgende hop|10.0.2.4| Het statische privé IP-adres van de netwerkinterface die is gekoppeld aan het virtuele netwerk-apparaat. Het enige hoptype kunt u een adres voor **virtueel apparaat**.|

    - Onder **myRouteTable-openbare**, klikt u op **subnetten** onder **instellingen**. 
    - Klik op **+ koppelen** onder **myRouteTable-openbare - subnetten**.
    - Klik op **virtueel netwerk** onder **subnet koppelen**, klikt u vervolgens op **myVnet**.
    - Klik op **Subnet** onder **subnet koppelen**, klikt u vervolgens op **openbare** onder **Subnet kiezen**. 
    - Om de configuratie op te slaan, klikt u op **OK** onder **subnet koppelen**. Een subnet kan nul of één routetabel gekoppeld hebben.
10. Volledige stap 9 opnieuw, zoeken naar **myRouteTable privé-**, een route met de volgende instellingen te maken en koppelen van de routetabel voor de **persoonlijke** subnet van de **myVnet** virtueel netwerk:

    |Instelling|Waarde|Uitleg|
    |---|---|---|
    |De routenaam van de|ToPublicSubnet|Deze route stuurt het verkeer naar het subnet van de openbare via het netwerk virtuele apparaat.|
    |Adresvoorvoegsel|10.0.0.0/24| Alle verkeer dat wordt verzonden naar adressen binnen deze adresvoorvoegsel (10.0.0.0 - 10.0.1.254) wordt verzonden naar het virtuele netwerk-apparaat.|
    |Volgend hoptype| Selecteer **virtueel apparaat**||
    |Adres van de volgende hop|10.0.2.4||

    Het netwerkverkeer tussen bronnen in het persoonlijke en openbare subnetten stroomt via het netwerk virtuele apparaat. 
11. **Optioneel:** maken van een virtuele machine in de openbare en particuliere subnetten en valideren van de communicatie tussen de virtuele machines wordt doorgestuurd via het netwerk virtuele apparaat via de stappen in [routeringvalideren](#validate-routing). 
12. **Optionele**: verwijderen van de resources die u in deze zelfstudie maakt via de stappen in [resources verwijderen](#delete-resources).

## <a name="validate-routing"></a>Routering valideren

1. Als u nog niet gedaan hebt, voer de stappen in [routes en virtueel netwerkapparaat maken](#create-routes-and-network-virtual-appliance).
2. Klik op de **Try it** knop in het vak dat wordt gevolgd door, die de Azure-Cloud-Shell geopend. Als u wordt gevraagd, meldt u zich voor het gebruik van Azure in uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p). De Azure-Cloud-Shell is een gratis bash-shell met de Azure-opdrachtregelinterface vooraf is geïnstalleerd. 

    De volgende scripts maakt twee virtuele machines, één in de *openbare* subnet en één in de *persoonlijke* subnet. Doorsturen via IP voor de netwerkinterface binnen het besturingssysteem van de NVA waarmee het besturingssysteem voor het routeren van verkeer via de netwerkinterface wordt ook inschakelen door de scripts. Een productie NVA doorgaans het verkeer inspecteert voordat deze routering, maar in deze zelfstudie de eenvoudige NVA alleen het verkeer gerouteerd zonder deze te bekijken. 

    Klik op de **kopie** knop in de **Linux** of **Windows** scripts die volgen en de inhoud van het script in een teksteditor plakken. Wijzig het wachtwoord voor de *adminPassword* variabele vervolgens plakken het script in de Azure-Cloud-Shell. Voer het script voor het besturingssysteem die u hebt geselecteerd toen u het virtuele netwerk-apparaat hebt gemaakt in stap 6 van [routes en virtueel netwerkapparaat maken](#create-routes-and-network-virtual-appliance). 

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

Als u deze zelfstudie hebt voltooid, is het raadzaam de resources verwijderen die u hebt gemaakt, zodat u geen gebruik kosten. Verwijderen van een resourcegroep, verwijdert tevens alle bronnen die zich in de resourcegroep. Met de portal geopend, moet u de volgende stappen uitvoeren:

1. Voer in het zoekvak portal **myResourceGroup**. Klik in de zoekresultaten op **myResourceGroup**.
2. Op de **myResourceGroup** blade, klikt u op de **verwijderen** pictogram.
3. Het verwijderen te bevestigen, in de **TYPE de naam van een RESOURCEGROEP** Voer **myResourceGroup**, en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Maak een [maximaal beschikbare virtuele netwerkapparaat](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuele netwerkapparaten hebben vaak meerdere netwerkinterfaces en IP-adressen aan hen toegewezen. Meer informatie over hoe [netwerkinterfaces toevoegen aan een bestaande virtuele machine](virtual-network-network-interface-vm.md#vm-add-nic) en [IP-adressen toevoegen aan een bestaande netwerkinterface](virtual-network-network-interface-addresses.md#add-ip-addresses). Hoewel alle grootten van virtuele machines ten minste twee netwerkinterfaces die zijn gekoppeld aan deze hebben kunnen, de grootte van elke virtuele machine biedt ondersteuning voor het maximale aantal netwerkinterfaces. Zie voor meer informatie over het aantal netwerkinterfaces elke virtuele machine ondersteunt de grootte, [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) en [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) grootten van virtuele machines. 
- Maken van een gebruiker gedefinieerde route om af te dwingen tunnel verkeer lokale via een [site-naar-site VPN-verbinding](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
