---
title: "Configureer altijd op beschikbaarheidsgroep-Listeners – Microsoft Azure | Microsoft Docs"
description: Beschikbaarheid van groep Listeners configureren op het Azure Resource Manager-model, met behulp van een interne load balancer met een of meer IP-adressen.
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 74fa1e4c9cfa608a9a385f3dd82a0599fbcc421c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Een of meer altijd op beschikbaarheid beschikbaarheidsgroeplisteners - Resource Manager configureren
Dit onderwerp wordt beschreven hoe:

* Maak een interne load balancer voor SQL Server-beschikbaarheidsgroepen met PowerShell-cmdlets.
* Extra IP-adressen toevoegen aan een load balancer voor meer dan één beschikbaarheidsgroep. 

Een beschikbaarheidsgroep-listener is de naam van een virtueel netwerk waarmee clients zijn verbonden voor toegang tot de database. Op virtuele machines in Azure bevat een load balancer het IP-adres voor de listener. De load balancer wordt verkeer gerouteerd naar het exemplaar van SQL Server die op de testpoort luistert. Een beschikbaarheidsgroep maakt meestal gebruik van een interne load balancer. Een Azure interne load balancer kan één of meerdere IP-adressen te hosten. Elk IP-adres gebruikt een specifieke testpoort. Dit document wordt beschreven hoe PowerShell gebruiken voor het maken van een load balancer, of IP-adressen toevoegen aan een bestaande load balancer voor SQL Server-beschikbaarheidsgroepen. 

De mogelijkheid meerdere IP-adressen toewijzen aan een interne load balancer is nieuw in Azure en is alleen beschikbaar in het Resource Manager-model. Voor het voltooien van deze taak moet u een SQL Server-beschikbaarheidsgroep geïmplementeerd op Azure virtuele machines in de Resource Manager-model hebben. Beide virtuele machines van SQL Server moet behoren tot dezelfde beschikbaarheidsset. U kunt de [Microsoft sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) automatisch maken van de beschikbaarheidsgroep in Azure Resource Manager. Deze sjabloon wordt automatisch de beschikbaarheidsgroep, met inbegrip van de interne load balancer voor u gemaakt. Als u liever, kunt u [handmatig configureren van een AlwaysOn-beschikbaarheidsgroep](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

In dit onderwerp is vereist dat uw beschikbaarheidsgroepen al zijn geconfigureerd.  

Verwante onderwerpen zijn:

* [AlwaysOn-beschikbaarheidsgroepen configureren in Azure virtuele machine (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Een VNet-naar-VNet-verbinding configureren met behulp van Azure Resource Manager en PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>De Windows Firewall configureren
De Windows Firewall configureren voor toegang tot SQL Server. De firewall-regels toestaan TCP-verbindingen met de poorten gebruikt door SQL Server-exemplaar en de listener-test. Zie voor gedetailleerde instructies [Windows Firewall configureren voor toegang tot de Database-Engine](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Maak een inkomende regel voor de SQL Server-poort en voor de testpoort.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Voorbeeldscript: Een interne load balancer maken met PowerShell
> [!NOTE]
> Als u de beschikbaarheidsgroep met gemaakt de [Microsoft sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), de interne load balancer al is gemaakt. 
> 
> 

De volgende PowerShell-script maakt een interne load balancer, configureert u de load-balancingregels en stelt een IP-adres voor de load balancer. Het script wordt uitgevoerd, opent u Windows PowerShell ISE en plak het script in de Script-veld. Gebruik `Login-AzureRMAccount` aan te melden PowerShell. Als u meerdere Azure-abonnementen hebt, gebruikt u `Select-AzureRmSubscription ` instellen van het abonnement. 

```powershell
# Login-AzureRmAccount
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

## <a name="Add-IP"></a>Voorbeeldscript: een IP-adres toevoegen aan een bestaande load balancer met PowerShell
Toevoegen voor het gebruik van meer dan één beschikbaarheidsgroep een extra IP-adres aan de load balancer. Elk IP-adres vereist een eigen taakverdeling regel, testpoort en front-poort.

De front-endpoort is de poort die toepassingen gebruiken voor verbinding met de SQL Server-exemplaar. IP-adressen voor verschillende beschikbaarheidsgroepen kunnen gebruiken dezelfde front-poort.

> [!NOTE]
> Elk IP-adres vereist voor beschikbaarheidsgroepen van SQL Server, een specifieke testpoort. Als u één IP-adres voor een load balancer testpoort 59999 gebruikt, kunnen geen andere IP-adressen op die load balancer testpoort 59999 gebruiken.

* Zie voor meer informatie over de load balancer limieten **persoonlijke front-end-IP per load balancer** onder [Networking limieten - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Zie voor meer informatie over de beschikbaarheid van groep limieten [beperkingen (beschikbaarheidsgroepen)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Het volgende script voegt een nieuwe IP-adres toe aan een bestaande load balancer. De ILB maakt gebruik van de listener-poort voor de front-endpoort voor taakverdeling. Deze poort kan de poort die SQL Server naar luistert zijn. Standaard-exemplaren van SQL Server is de poort 1433. De taakverdelingsregel voor een beschikbaarheidsgroep vereist een zwevend IP (direct server return) zodat de back-end-poort hetzelfde als de front-endpoort is. De variabelen voor uw omgeving bijwerken. 

```powershell
# Login-AzureRmAccount
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

## <a name="configure-the-listener"></a>De listener configureren

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>Stel de listener-poort in SQL Server Management Studio

1. Start SQL Server Management Studio en maak verbinding met de primaire replica.

1. Navigeer naar **AlwaysOn hoge beschikbaarheid** | **beschikbaarheidsgroepen** | **beschikbaarheidsgroep-Listeners**. 

1. U ziet nu de naam van de listener die u hebt gemaakt in Failoverclusterbeheer. Met de rechtermuisknop op de naam van de listener en klik op **eigenschappen**.

1. In de **poort** vak het poortnummer opgeven voor de beschikbaarheidsgroep-listener met behulp van de $EndpointPort u eerder hebt gebruikt (1433 is de standaardinstelling), klikt u vervolgens op **OK**.

## <a name="test-the-connection-to-the-listener"></a>Test de verbinding met de listener

De verbinding testen:

1. RDP naar een SQL-Server die zich in hetzelfde virtuele netwerk, maar is geen eigenaar van de replica. Dit kan zijn dat de andere SQL-Server in het cluster.

1. Gebruik **sqlcmd** hulpprogramma om de verbinding te testen. Bijvoorbeeld, het volgende script stelt een **sqlcmd** verbinding met de primaire replica via de listener met de Windows-verificatie:
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    Als de listener met behulp van een andere poort dan de standaard poort (1433), de poort in de verbindingsreeks opgeven. Bijvoorbeeld de volgende sqlcmd-opdracht is verbonden met een listener op poort 1435: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

De SQLCMD-verbinding wordt automatisch verbinding met elk ander exemplaar van SQL Server als host fungeert voor de primaire replica. 

> [!NOTE]
> Zorg ervoor dat de poort die u opgeeft geopend op de firewall van beide SQL-Servers is. Beide servers vereisen een inkomende regel voor de TCP-poort die u gebruikt. Zie [toevoegen of bewerken firewallregel](http://technet.microsoft.com/library/cc753558.aspx) voor meer informatie. 
> 
> 

## <a name="guidelines-and-limitations"></a>Richtlijnen en beperkingen
Houd rekening met de volgende richtlijnen op beschikbaarheidsgroep-listener in Azure met interne load balancer:

* Met een interne load balancer u alleen toegang tot de listener uit binnen hetzelfde virtuele netwerk.


## <a name="for-more-information"></a>Voor meer informatie
Zie voor meer informatie [configureren altijd op de beschikbaarheidsgroep in Azure VM handmatig](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets
Gebruik de volgende PowerShell-cmdlets voor het maken van een interne load balancer voor virtuele machines in Azure.

* [Nieuwe AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) maakt u een load balancer. 
* [Nieuwe AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) maakt een front-end-IP-configuratie voor een load balancer. 
* [Nieuwe AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) maakt u de regelconfiguratie van een voor een load balancer. 
* [Nieuwe AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) maakt een back-end-pool-adresconfiguratie voor een load balancer. 
* [Nieuwe AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) maakt u een test-configuratie voor een load balancer.
* [Verwijder AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) Hiermee verwijdert u een load balancer van een Azure-resourcegroep.
