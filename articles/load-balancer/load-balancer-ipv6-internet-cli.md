---
title: Maken van een openbare load balancer met IPv6 - Azure CLI | Microsoft Docs
description: Informatie over het maken van een openbare load balancer met IPv6 in Azure Resource Manager met behulp van Azure CLI.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
keywords: IPv6-, azure load balancer, dual-stack, openbare IP-adres, systeemeigen ipv6, mobiele, iot
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 3abd47460999f7b059469a58a59a3e297e88effb
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-public-load-balancer-with-ipv6-in-azure-resource-manager-by-using-azure-cli"></a>Maken van een openbare load balancer met IPv6 in Azure Resource Manager met behulp van Azure CLI

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Sjabloon](load-balancer-ipv6-internet-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer is een Layer-4 (TCP, UDP) load balancer. Netwerktaakverdelers bieden hoge beschikbaarheid door het distribueren van inkomend verkeer tussen orde service-exemplaren in cloudservices of virtuele machines in een load balancer-set. Load balancers kunnen ook opleveren voor deze services op meerdere poorten of meerdere IP-adressen of beide.

## <a name="example-deployment-scenario"></a>Voorbeeldscenario voor implementatie

Het volgende diagram illustreert de oplossing die wordt geïmplementeerd met behulp van de voorbeeldsjabloon die wordt beschreven in dit artikel voor taakverdeling.

![Load balancer-scenario](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

In dit scenario moet u de volgende Azure-resources maken:

* twee virtuele machines (VM's)
* de virtuele netwerkinterface voor elke virtuele machine met zowel IPv4 als IPv6-adressen die zijn toegewezen
* Een openbare load balancer met een IPv4- en een IPv6-openbare IP-adres
* Een beschikbaarheidsset met de twee virtuele machines
* twee taakverdelingsregels het openbare VIP's worden toegewezen aan de persoonlijke eindpunten

## <a name="deploy-the-solution-by-using-azure-cli"></a>De oplossing implementeren met behulp van Azure CLI

De volgende stappen laten zien hoe een openbare load balancer maken met behulp van Azure Resource Manager met Azure CLI. Met Azure Resource Manager gemaakt en geconfigureerd elk object afzonderlijk, en deze vervolgens samen om te maken van een resource.

Voor het implementeren van een load balancer maken en configureren van de volgende objecten:

* **Front-end-IP-configuratie**: openbare IP-adressen voor binnenkomend netwerkverkeer bevat.
* **Back-end-adresgroep**: netwerkinterfaces (NIC's) voor de virtuele machines voor het ontvangen van netwerkverkeer van de load balancer bevat.
* **Taakverdelingsregels**: bevat regels die een openbare poort op de load balancer aan een poort in de back-end-adresgroep toewijzen.
* **NAT-regels voor binnenkomende verbindingen**: bevat network address translation (NAT) regels dat een openbare poort op de load balancer worden toegewezen aan een poort voor een specifieke virtuele machine in de back-end-adresgroep.
* **Tests**: statuscontroles die worden gebruikt voor het controleren van de beschikbaarheid van virtuele machine-exemplaren in de back-end-adresgroep bevat.

Zie [Azure Resource Manager-ondersteuning voor load balancer](load-balancer-arm.md) voor meer informatie.

## <a name="set-up-your-azure-cli-environment-to-use-azure-resource-manager"></a>Instellen van uw Azure CLI-omgeving op Azure Resource Manager gebruiken

In dit voorbeeld kunt u de Azure CLI-hulpprogramma's uitvoeren in een PowerShell-opdrachtvenster. Ter verbetering van de leesbaarheid en opnieuw kunnen worden gebruikt, moet u scriptmogelijkheden van PowerShell, niet de Azure PowerShell-cmdlets gebruiken.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [installeren en configureren van Azure CLI](../cli-install-nodejs.md) en volg de instructies tot het punt waar u uw Azure-account en abonnement selecteren.

2. Uitvoeren als u wilt overschakelen naar de modus Resource Manager, de **azure config mode** opdracht:

    ```azurecli
    azure config mode arm
    ```

    Verwachte uitvoer:

        info:    New mode is arm

3. Aanmelden bij Azure en een lijst met abonnementen:

    ```azurecli
    azure login
    ```

4. Voer uw Azure-referenties bij de opdrachtprompt:

    ```azurecli
    azure account list
    ```

5. Selecteer het abonnement dat u wilt gebruiken, en noteer de abonnements-ID moet worden gebruikt in de volgende stap.

6. Stel PowerShell variabelen voor gebruik met de Azure CLI-opdrachten:

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Een resourcegroep, een load balancer, een virtueel netwerk en subnetten maken

1. Een resourcegroep maken:

    ```azurecli
    azure group create $rgName $location
    ```

2. Maak een load balancer:

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. Een virtueel netwerk maken:

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

4. Maak twee subnetten in dit virtuele netwerk:

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Openbare IP-adressen voor de front-pool maken

1. De PowerShell-variabelen instellen:

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. Een openbaar IP-adres voor de front-end-IP-adresgroep maken:

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > De load balancer gebruikmaakt van het domeinlabel van het openbare IP-adres als de volledig gekwalificeerde domeinnaam (FQDN). Dit een wijziging van de klassieke implementatie dat gebruikmaakt van de cloudservice naam als de load balancer FQDN-naam.
    >
    > In dit voorbeeld wordt de FQDN-naam is *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Front-end en back-end-adresgroepen maken

In deze sectie kunt u de volgende IP-adresgroepen maken:
* De front-end-IP-adresgroep die het inkomende netwerkverkeer op de load balancer ontvangt.
* De backend-IP-groep waar de front-groep het netwerkverkeer taakverdeling verzendt.

1. De PowerShell-variabelen instellen:

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. Een front-end-IP-adresgroep maken en deze koppelen aan het openbare IP-adres dat u hebt gemaakt in de vorige stap en de load balancer.

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-load-balancer-rules"></a>De test, NAT-regels maken en load-balancerregels

In dit voorbeeld worden de volgende items gemaakt:

* Een test-regel om te controleren op connectiviteit met TCP-poort 80.
* Een NAT-regel voor alle binnenkomend verkeer op poort 3389 voor poort 3389 voor RDP vertalen.\*
* Een NAT-regel om te vertalen alle binnenkomend verkeer op poort 3391 tot poort 3389 voor remote desktop protocol (RDP).\*
* een load balancer-regel voor een evenwicht tussen alle binnenkomend verkeer op poort 80 op poort 80 van de adressen in de back-end-pool.

\*NAT-regels zijn gekoppeld aan een specifiek exemplaar van de virtuele machine achter de load balancer. Het netwerkverkeer dat op poort 3389 binnenkomt is verzonden naar de specifieke virtuele machine en de poort die is gekoppeld aan de NAT-regel. U moet een protocol (UDP of TCP) voor een NAT-regel opgeven. U kunt beide protocollen toewijzen aan dezelfde poort.

1. De PowerShell-variabelen instellen:

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. Maken van de test.

    Het volgende voorbeeld wordt elke 15 seconden gemaakt TCP voor een test waarmee wordt gecontroleerd of de verbinding met de back-end-TCP-poort 80. Na twee achtereenvolgende mislukte pogingen, worden de back-end-bron niet beschikbaar is gemarkeerd.

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. Binnenkomende NAT-regels waarmee de RDP-verbindingen met de back-end-bronnen maken:

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. Load balancerregels maken waarmee verkeer naar de andere back-end-poorten, afhankelijk van de front-endserver die de aanvraag verzenden.

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. Controleer uw instellingen:

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    Verwachte uitvoer:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a>NIC's maken

NIC's maken en koppelen aan de NAT-regels, load balancer-regels en -tests.

1. De PowerShell-variabelen instellen:

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. Maken van een NIC voor elke back-end en toevoegen van een IPv6-configuratie:

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Maken van de back-end-VM-netwerkbronnen en elke NIC koppelen

Voor het maken van virtuele machines, moet u een opslagaccount hebben. De virtuele machines moeten lid zijn van een beschikbaarheidsset voor load balancing. Zie voor meer informatie over het maken van virtuele machines [maken van een virtuele machine in Azure met behulp van PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

1. De PowerShell-variabelen instellen:

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > Dit voorbeeld wordt de gebruikersnaam en het wachtwoord voor de virtuele machines in leesbare tekst. Nodige zorgvuldigheid u gebruikt deze referenties in leesbare tekst. Zie voor een veiligere methode afhandelen van referenties in PowerShell de [ `Get-Credential` ](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

2. De storage-account en beschikbaarheid set maken.

    U kunt een bestaand opslagaccount gebruiken bij het maken van de virtuele machines. U kunt een nieuw opslagaccount maken met behulp van de volgende opdracht:

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

3. De beschikbaarheidsset maken:

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

4. Maak de virtuele machines met de gekoppelde NIC's:

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-cli.md)  
[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)  
[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
