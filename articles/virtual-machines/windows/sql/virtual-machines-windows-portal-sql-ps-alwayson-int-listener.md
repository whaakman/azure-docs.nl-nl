---
title: Always On Availability Group Listeners – Microsoft Azure configureren | Microsoft Docs
description: Beschikbaarheid van groep Listeners op het Azure Resource Manager-model, met behulp van een interne load balancer met een of meer IP-adressen configureren.
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 76ebdc85db2c65b1ad99c1e7abe5e697f1c1284c
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063995"
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Een of meer Always On availability group listeners - Resource Manager configureren
Dit onderwerp wordt beschreven hoe u:

* Maak een interne load balancer voor SQL Server-beschikbaarheidsgroepen met behulp van PowerShell-cmdlets.
* Voeg extra IP-adressen toe aan een load balancer voor de meer dan één beschikbaarheidsgroep. 

Een beschikbaarheidsgroeplistener is een virtuele-netwerknaam waarmee clients verbinding om toegang tot de database maken. Op Azure virtual machines bevat een load balancer het IP-adres voor de listener. De load balancer routeert verkeer naar het exemplaar van SQL Server die op de testpoort luistert. Meestal een interne load balancer maakt gebruik van een beschikbaarheidsgroep. Een interne Azure load balancer kan een of meer IP-adressen te hosten. Elk IP-adres maakt gebruik van een specifieke testpoort. Dit document laat zien hoe u PowerShell gebruikt voor het maken van een load balancer of IP-adressen toevoegen aan een bestaande load balancer voor SQL Server-beschikbaarheidsgroepen. 

De mogelijkheid meerdere IP-adressen toewijzen aan een interne load balancer is bekend bent met Azure en is alleen beschikbaar in Resource Manager-model. Als u wilt deze taak hebt voltooid, moet u een SQL Server-beschikbaarheidsgroep geïmplementeerd op Azure virtual machines in Resource Manager-model heeft. Beide virtuele machines met SQL Server moet behoren tot dezelfde beschikbaarheidsset. U kunt de [Microsoft sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) om automatisch te maken de beschikbaarheidsgroep in Azure Resource Manager. Deze sjabloon maakt automatisch de van beschikbaarheidsgroep, met inbegrip van de interne load balancer voor u. Als u liever, kunt u [handmatig configureren van een AlwaysOn-beschikbaarheidsgroep](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

Dit onderwerp gebruikmaken van de beschikbaarheidsgroepen zijn al geconfigureerd.  

Verwante onderwerpen zijn onder andere:

* [AlwaysOn-beschikbaarheidsgroepen in Azure VM (GUI) configureren](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Een VNet-naar-VNet-verbinding configureren met behulp van Azure Resource Manager en PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>De Windows Firewall configureren
De Windows Firewall voor SQL Server-toegang configureren. De firewall-regels toestaan TCP-verbindingen met de poorten gebruikt door de SQL Server-exemplaar en de listener-test. Zie voor gedetailleerde instructies [configureren van een Windows-Firewall for Database Engine Access](https://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Maak een inkomende regel voor de SQL Server-poort en de testpoort.

Als u beperken van toegang met een Azure Network Security Group, zorg ervoor dat de regels voor toestaan de back-end SQL Server VM-IP-adressen bevatten, en de load balancer zwevend IP-adressen voor de AG-listener en core IP-adres van het cluster, indien van toepassing.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Voorbeeldscript: Een interne load balancer maken met PowerShell
> [!NOTE]
> Als u hebt gemaakt met de beschikbaarheidsgroep met de [Microsoft sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), de interne load balancer is al gemaakt. 
> 
> 

De volgende PowerShell-script maakt u een interne load balancer, configureert u de load balancer-regels en stelt een IP-adres voor de load balancer. Voer het script, opent u Windows PowerShell ISE en plak het script in het scriptvenster. Gebruik `Connect-AzureRmAccount` aanmelden bij PowerShell. Als u meerdere Azure-abonnementen hebt, gebruikt u `Select-AzureRmSubscription ` om in te stellen van het abonnement. 

```powershell
# Connect-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a> Voorbeeldscript: Een IP-adres toevoegen aan een bestaande load balancer met PowerShell
Toevoegen voor het gebruik van meer dan één beschikbaarheidsgroep, een extra IP-adres aan de load balancer. Elk IP-adres vereist een eigen regel testpoort en front-poort te verdelen.

De front-endpoort is de poort die toepassingen gebruiken om verbinding maken met de SQL Server-exemplaar. IP-adressen voor verschillende beschikbaarheidsgroepen kunnen gebruiken dezelfde front-poort.

> [!NOTE]
> Elk IP-adres vereist voor SQL Server-beschikbaarheidsgroepen, een specifieke testpoort. Als u één IP-adres op een load balancer testpoort 59999 gebruikt, kunnen geen andere IP-adressen op die load balancer testpoort 59999 gebruiken.

* Zie voor meer informatie over de load balancer limieten **privé-front-end-IP per load balancer** onder [Netwerklimieten - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Zie voor meer informatie over de beschikbaarheid van groep limieten [beperkingen (Availability Groups)](https://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Het volgende script wordt een nieuw IP-adres toegevoegd aan een bestaande load balancer. De ILB maakt gebruik van de listener-poort voor de load balancer-front-endpoort. Deze poort kan niet de poort op die SQL Server luistert. Voor standaardexemplaren van SQL Server is de poort 1433. De load balancer-regel voor een beschikbaarheidsgroep vereist een zwevend IP (direct server return) zodat de back-end-poort hetzelfde als de front-endpoort is. Werk de variabelen voor uw omgeving. 

```powershell
# Connect-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```

## <a name="configure-the-listener"></a>Configureer de listener

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>Stel de listener-poort in SQL Server Management Studio

1. Start SQL Server Management Studio en maak verbinding met de primaire replica.

1. Navigeer naar **AlwaysOn hoge beschikbaarheid** | **beschikbaarheidsgroepen** | **beschikbaarheidsgroep Listeners**. 

1. U ziet nu de listener met de naam die u hebt gemaakt in Failoverclusterbeheer. Met de rechtermuisknop op de listenernaam van de en klikt u op **eigenschappen**.

1. In de **poort** vak, geef het poortnummer voor de beschikbaarheidsgroep-listener met behulp van de $EndpointPort u eerder hebt gebruikt (1433 is de standaardinstelling), klikt u vervolgens op **OK**.

## <a name="test-the-connection-to-the-listener"></a>Test de verbinding met de listener

De verbinding wilt testen:

1. RDP naar een SQL-Server die zich in hetzelfde virtuele netwerk, maar is geen eigenaar van de replica. Dit kan zijn dat de andere SQL-Server in het cluster.

1. Gebruik **sqlcmd** hulpprogramma om de verbinding te testen. Bijvoorbeeld, het volgende script maakt een **sqlcmd** verbinding met de primaire replica met de listener met de Windows-verificatie:
   
    ```
    sqlcmd -S <listenerName> -E
    ```
   
    Als de listener met behulp van een andere poort dan de standaardwaarde standaardpoort (1433), de poort in de verbindingsreeks opgeven. Bijvoorbeeld, de volgende sqlcmd-opdracht is verbonden met een listener op poort 1435: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

De SQLCMD-verbinding wordt automatisch verbinding met elk exemplaar van SQL Server als host fungeert voor de primaire replica. 

> [!NOTE]
> Zorg ervoor dat de poort die u opgeeft geopend op de firewall van beide SQL-Servers is. Beide servers vereist een inkomende regel voor de TCP-poort die u gebruikt. Zie [toevoegen of bewerken firewallregel](https://technet.microsoft.com/library/cc753558.aspx) voor meer informatie. 
> 
> 

## <a name="guidelines-and-limitations"></a>Richtlijnen en beperkingen
Houd rekening met de volgende richtlijnen op beschikbaarheidsgroep-listener in Azure met behulp van interne load balancer:

* Met een interne load balancer, u alleen toegang tot de listener uit binnen hetzelfde virtuele netwerk.

* Als u beperken van toegang met een Azure Network Security Group, zorg ervoor dat de regels voor toestaan de back-end SQL Server VM-IP-adressen bevatten, en de load balancer zwevend IP-adressen voor de AG-listener en core IP-adres van het cluster, indien van toepassing.

## <a name="for-more-information"></a>Voor meer informatie
Zie voor meer informatie, [configureren altijd op beschikbaarheidsgroep in Azure VM handmatig](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets
De volgende PowerShell-cmdlets gebruiken om te maken van een interne load balancer voor virtuele machines van Azure.

* [Nieuwe-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) wordt een load balancer. 
* [Nieuwe AzureRMLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) maakt een front-end-IP-configuratie voor een load balancer. 
* [Nieuwe-AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) maakt de regelconfiguratie van een voor een load balancer. 
* [Nieuwe-AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) maakt u een back-end-pool-adresconfiguratie voor een load balancer. 
* [Nieuwe-AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) maakt u een Testconfiguratie voor een load balancer.
* [Remove-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) Hiermee verwijdert u een load balancer uit een Azure-resourcegroep.
