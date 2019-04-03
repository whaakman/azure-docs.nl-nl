---
title: De configuratie van een SAP-multi-SID in Azure maken | Microsoft Docs
description: Handleiding voor multi-SID-configuratie van hoge beschikbaarheid SAP NetWeaver op Windows virtuele machines
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fe9b70d74e326166afae366becc47fbcc8b2ea56
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58846679"
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a>Maken van de configuratie van een SAP NetWeaver-multi-SID

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

Microsoft uitgebracht in September 2016, een functie waar u meerdere virtuele IP-adressen met behulp van een interne load balancer van Azure kunt beheren. Deze functionaliteit bestaat al in de externe load balancer van Azure.

Als u een SAP-implementatie hebt, kunt u een interne load balancer gebruiken voor het maken van een Windows cluster-configuratie voor SAP ASCS/SCS, zoals beschreven in de [handleiding voor hoge beschikbaarheid SAP NetWeaver op Windows-VM's] [ sap-ha-guide].

In dit artikel richt zich op het verplaatsen van een enkele ASCS/SCS-installatie aan de configuratie van een SAP-multi-SID door het installeren van extra SAP ASCS/SCS geclusterde exemplaren in een bestaand Windows Server Failover Clustering (WSFC)-cluster. Als dit proces is voltooid, beschikt u hebt geconfigureerd een SAP-multi-SID-cluster.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten
U hebt al een WSFC-cluster dat wordt gebruikt voor een SAP ASCS/SCS-exemplaar, geconfigureerd zoals beschreven in de [handleiding voor hoge beschikbaarheid SAP NetWeaver op Windows-VM's] [ sap-ha-guide] en zoals wordt weergegeven in het volgende diagram.

![Hoge beschikbaarheid SAP ASCS/SCS-exemplaar][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a>Doel-architectuur

Het doel is het installeren van meerdere SAP ABAP ASCS of SAP Java SCS geclusterde exemplaren in de dezelfde WSFC-cluster, als geïllustreerde hier:

![Meerdere SAP ASCS/SCS geclusterde exemplaren in Azure][sap-ha-guide-figure-6002]

> [!NOTE]
>Er is een limiet aan het aantal privé front-end-IP-adressen voor elke Azure interne load balancer.
>
>Het maximum aantal SAP ASCS/SCS-exemplaren in een WSFC-cluster is gelijk aan het maximum aantal privé front-end-IP-adressen voor elke Azure interne load balancer.
>

Zie voor meer informatie over de limieten van de load balancer "privé-front-end-IP per load balancer in [netwerklimieten: Azure Resource Manager][networking-limits-azure-resource-manager].

De volledige landschap met twee hoge beschikbaarheid van SAP-systemen zou er als volgt:

![SAP hoge beschikbaarheid multi-SID-installatie met behulp van twee SAP-systeem beveiligings-id 's][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> De instelling moet voldoen aan de volgende voorwaarden:
> - De SAP ASCS/SCS-instanties moeten delen hetzelfde WSFC-cluster.
> - Elke SID DBMS beschikken over een eigen toegewezen WSFC-cluster.
> - SAP-toepassingsservers die deel uitmaken van een SAP-systeem SID moeten hebben hun eigen toegewezen virtuele machines.


## <a name="prepare-the-infrastructure"></a>De infrastructuur voorbereiden
Om voor te bereiden op uw infrastructuur, kunt u een extra SAP ASCS/SCS-exemplaar installeren met de volgende parameters:

| Parameternaam | Value |
| --- | --- |
| SAP ASCS/SCS BEVEILIGINGS-ID |pr1-lb-ascs |
| SAP DBMS interne load balancer | PR5 |
| Naam van de virtuele host SAP | pr5-sap-cl |
| SAP ASCS/SCS virtuele host IP-adres (extra Azure-load balancer IP-adres) | 10.0.0.50 |
| Exemplaarnummer van de SAP ASCS/SCS | 50 |
| De testpoort ILB voor extra SAP ASCS/SCS-exemplaar | 62350 |

> [!NOTE]
> Voor instanties van SAP ASCS/SCS-cluster, is een unieke testpoort met elk IP-adres vereist. Als u één IP-adres op een interne Azure load balancer testpoort 62300 gebruikt, kunt geen andere IP-adres op dat load balancer testpoort 62300 gebruiken.
>
>Voor onze testdoeleinden, omdat de testpoort 62300 is al gereserveerd, gebruiken we testpoort 62350.

U kunt extra exemplaren van de SAP ASCS/SCS installeren in de bestaande WSFC-cluster met twee knooppunten:

| Virtuele-machinefunctie | Hostnaam van de virtuele machine | Statisch IP-adres |
| --- | --- | --- |
| 1e clusterknooppunt voor ASCS/SCS-exemplaar |pr1-ascs-0 |10.0.0.10 |
| 2e clusterknooppunt voor ASCS/SCS-exemplaar |pr1-ascs-1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a>De naam van een virtuele host voor de geclusterde SAP ASCS/SCS-exemplaar op de DNS-server maken

U kunt een DNS-vermelding voor de virtuele host-naam van de ASCS/SCS-exemplaar maken met behulp van de volgende parameters:

| Nieuwe SAP ASCS/SCS virtuele host-naam | Bijbehorende IP-adres |
| --- | --- |
|pr5-sap-cl |10.0.0.50 |

De nieuwe hostnaam en IP-adres zijn in de DNS-beheer weergegeven zoals in de volgende schermafbeelding:

![Lijst met DNS-beheer markeren de gedefinieerde DNS-vermelding voor de nieuwe SAP ASCS/SCS cluster virtuele naam en het TCP/IP-adres][sap-ha-guide-figure-6004]

De procedure voor het maken van een DNS-vermelding wordt ook beschreven in het hoofdvenster in detail [handleiding voor hoge beschikbaarheid SAP NetWeaver op Windows-VM's][sap-ha-guide-9.1.1].

> [!NOTE]
> Het nieuwe IP-adres dat u aan de virtuele host-naam van de extra ASCS/SCS-exemplaar toewijst moet hetzelfde zijn als het nieuwe IP-adres dat u hebt toegewezen aan de SAP-Azure load balancer.
>
>In ons scenario is het IP-adres 10.0.0.50.

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a>Een IP-adres toevoegen aan een bestaande interne Azure load balancer met behulp van PowerShell

Voor het maken van meer dan één SAP ASCS/SCS-exemplaar in de dezelfde WSFC-cluster, moet u PowerShell gebruiken om toe te voegen een IP-adres aan een bestaande interne Azure load balancer. Elk IP-adres vereist een eigen regels voor taakverdeling, testpoort, front-end-IP-adresgroep en back-end-pool.

Het volgende script wordt een nieuw IP-adres toegevoegd aan een bestaande load balancer. De PowerShell-variabelen voor uw omgeving bijwerken. Het script maakt u alle benodigde regels voor taakverdeling voor alle SAP ASCS/SCS-poorten.

```powershell

# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get the Azure VNet and subnet
$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzLoadBalancer

# Get new updated configuration
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzLoadBalancer

# Get new updated config
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs to backend pool
$BEPool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzNetworkInterface -NetworkInterface $NIC
        #start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzLoadBalancer

Write-Host "Successfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
Nadat het script is uitgevoerd, worden de resultaten weergegeven in de Azure-portal, zoals wordt weergegeven in de volgende schermafbeelding:

![Nieuwe front-end-IP-groep in Azure portal][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a>Voeg schijven toe om machines te clusteren, en de SIOS share clusterschijf configureren

U kunt het delen van een nieuwe clusterschijf voor elke extra SAP ASCS/SCS-exemplaar moet toevoegen. Voor Windows Server 2012 R2 is de WSFC-cluster share schijf die momenteel in gebruik de SIOS DataKeeper-software-oplossing.

Ga als volgt te werk:
1. Een extra schijf of schijven met dezelfde grootte (die u wilt stripe) toevoegen aan elk van de clusterknooppunten, en ze te formatteren.
2. Storage-replicatie configureren met SIOS DataKeeper.

Deze procedure wordt ervan uitgegaan dat u al SIOS DataKeeper hebt geïnstalleerd op de WSFC-cluster virtuele machines. Als u deze hebt geïnstalleerd, moet u replicatie tussen de machines nu configureren. Het proces wordt beschreven in het hoofdvenster [handleiding voor hoge beschikbaarheid SAP NetWeaver op Windows-VM's][sap-ha-guide-8.12.3.3].  

![DataKeeper synchroon spiegelen voor de nieuwe SAP ASCS/SCS schijf delen][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a>Virtuele machines voor SAP-toepassingsservers en DBMS-cluster implementeren

Voor het voltooien van de voorbereiding van de infrastructuur voor het tweede SAP-systeem, het volgende doen:

1. Implementeren van de toegewezen virtuele machines voor SAP-toepassingsservers en plaats u ze in hun eigen toegewezen beschikbaarheidsgroep.
2. Toegewezen virtuele machines voor DBMS-cluster implementeren en plaats u ze in hun eigen toegewezen beschikbaarheidsgroep.


## <a name="install-the-second-sap-sid2-netweaver-system"></a>Het tweede SAP SID2 NetWeaver-systeem installeert

Het volledige proces van het installeren van een tweede SID2 SAP-systeem wordt beschreven in het hoofdvenster [handleiding voor hoge beschikbaarheid SAP NetWeaver op Windows-VM's][sap-ha-guide-9].

De procedure op hoog niveau is als volgt:

1. [Installeren van het eerste clusterknooppunt van SAP][sap-ha-guide-9.1.2].  
 In deze stap maakt u installeert SAP met een hoge beschikbaarheid ASCS/SCS-exemplaar op de **bestaande WSFC-cluster-knooppunt 1**.

2. [Wijzig het SAP-profiel van de ASCS/SCS-exemplaar][sap-ha-guide-9.1.3].

3. [Configureren van een testpoort][sap-ha-guide-9.1.4].  
 In deze stap maakt configureert u een SAP-clusterbron SAP-SID2-IP-testpoort met behulp van PowerShell. Deze configuratie worden uitgevoerd op een van de knooppunten van het SAP ASCS/SCS.

4. [Het database-exemplaar installeren] [sap-ha-guide-9.2].  
 In deze stap maakt installeert u DBMS op een specifieke WSFC-cluster.

5. [Het tweede clusterknooppunt installeren] [sap-ha-guide-9.3].  
 In deze stap maakt installeert u SAP met een hoge beschikbaarheid ASCS/SCS-exemplaar op het bestaande cluster WSFC-knooppunt 2.

6. Open Windows Firewall-poorten voor de SAP ASCS/SCS-exemplaar en ProbePort.  
 Op beide clusterknooppunten die worden gebruikt voor SAP ASCS/SCS-exemplaren, opent u alle Windows Firewall-poorten die worden gebruikt door SAP ASCS/SCS. Deze poorten worden vermeld in de [handleiding voor hoge beschikbaarheid SAP NetWeaver op Windows-VM's][sap-ha-guide-8.8].  
 De testpoort interne Azure load balancer, die 62350 in ons scenario is ook openen.

7. [Wijzig het starttype van de service-exemplaar van SAP INGEN Windows][sap-ha-guide-9.4].

8. [Installeren van de primaire SAP-toepassingsserver] [ sap-ha-guide-9.5] toegewezen op de nieuwe virtuele machine.

9. [De aanvullende SAP-toepassingsserver installeren] [ sap-ha-guide-9.6] toegewezen op de nieuwe virtuele machine.

10. [Testen van de failover-SAP ASCS/SCS-exemplaar en SIOS replicatie][sap-ha-guide-10].

## <a name="next-steps"></a>Volgende stappen

- [Netwerklimieten: Azure Resource Manager][networking-limits-azure-resource-manager]
- [Meerdere VIP's voor Azure Load Balancer][load-balancer-multivip-overview]
- [Handleiding voor hoge beschikbaarheid SAP NetWeaver op Windows-VM 's][sap-ha-guide]
