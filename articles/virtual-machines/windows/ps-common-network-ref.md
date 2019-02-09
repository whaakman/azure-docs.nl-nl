---
title: Algemene PowerShell-opdrachten voor Azure Virtual Networks | Microsoft Docs
description: Algemene PowerShell-opdrachten om u te helpen aan de slag het maken van een virtueel netwerk en alle bijbehorende resources voor virtuele machines.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: 31a0d486f2540ea75a57b29b8f1da21839783468
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984631"
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Algemene PowerShell-opdrachten voor Azure Virtual Networks

Als u maken van een virtuele machine wilt, moet u maken een [virtueel netwerk](../../virtual-network/virtual-networks-overview.md) of een bestaand virtueel netwerk waarin de virtuele machine kan worden toegevoegd op de hoogte. Normaal gesproken wanneer u een virtuele machine maakt, moet u ook rekening houden met het maken van de resources in dit artikel beschreven.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

Enkele variabelen die kunnen nuttig zijn voor u als meer dan een van de opdrachten in dit artikel uitvoert:

- $location - de locatie van de netwerkbronnen. U kunt [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) vinden een [geografische regio](https://azure.microsoft.com/regions/) die bij u past.
- $myResourceGroup - de naam van de resourcegroep waar de netwerkbronnen zich bevinden.

## <a name="create-network-resources"></a>Netwerkbronnen maken

| Taak | Opdracht |
| ---- | ------- |
| Subnetconfiguraties maken |$subnet1 = [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) -Name "mySubnet1" -AddressPrefix XX.X.X.X/XX<BR>$subnet2 = New-AzVirtualNetworkSubnetConfig -Name "mySubnet2" -AddressPrefix XX.X.X.X/XX<BR><BR>Een typische netwerk mogelijk een subnet voor een [internetgerichte load balancer](../../load-balancer/load-balancer-internet-overview.md) en een apart subnet voor een [interne load balancer](../../load-balancer/load-balancer-internal-overview.md). |
| Een virtueel netwerk maken |$vnet = [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) -Name "myVNet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix XX.X.X.X/XX -Subnet $subnet1, $subnet2 |
| Test voor een unieke domeinnaam op |[Test-AzDnsAvailability](https://docs.microsoft.com/powershell/module/az.network/test-azdnsavailability) -DomainNameLabel "myDNS" -Location $location<BR><BR>U kunt opgeven dat een DNS-domeinnaam voor een [openbare IP-resource](../../virtual-network/virtual-network-ip-addresses-overview-arm.md), waarmee een toewijzing voor domainname.location.cloudapp.azure.com naar het openbare IP-adres wordt gemaakt in de Azure beheerde DNS-servers. De naam mag alleen letters, cijfers en afbreekstreepjes bevatten. Het eerste en laatste teken moet een letter of cijfer en de domeinnaam moet uniek zijn binnen de Azure-locatie. Als **waar** wordt geretourneerd, wordt de voorgestelde naam uniek is. |
| Een openbaar IP-adres maken |$pip = [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) -Name "myPublicIp" -ResourceGroupName $myResourceGroup -DomainNameLabel "myDNS" -Location $location -AllocationMethod Dynamic<BR><BR>Het openbare IP-adres maakt gebruik van de domeinnaam die u eerder hebt getest en wordt gebruikt door de front-end-configuratie van de load balancer. |
| Maak een front-end IP-configuratie |$frontendIP = [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) -Name "myFrontendIP" -PublicIpAddress $pip<BR><BR>De front-end-configuratie bevat de openbare IP-adres dat u eerder hebt gemaakt voor inkomend netwerkverkeer. |
| Een back-endadresgroep maken |$beAddressPool = [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) -Name "myBackendAddressPool"<BR><BR>Biedt de interne adressen van de back-end van de load balancer die toegankelijk zijn via een netwerkinterface. |
| Een test maken |$healthProbe = [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) -Name "myProbe" -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2<BR><BR>Bevat statustests die worden gebruikt om te controleren op beschikbaarheid van instanties van virtuele machines in de back-endadresgroep. |
| Maak een regel voor taakverdeling |$lbRule = [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80<BR><BR>Bevat regels die een openbare poort op de load balancer aan een poort in de back-endadresgroep toewijst. |
| Een binnenkomende NAT-regel maken |$inboundNATRule = [New-AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) -Name "myInboundRule1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389<BR><BR>Bevat regels die een openbare poort op de load balancer toewijzen aan een poort voor een specifieke virtuele machine in de back-endadresgroep. |
| Een load balancer maken |$loadBalancer = [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) - ResourceGroupName $myResourceGroup-naam 'myLoadBalancer'-locatie $location - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-$healthProbe-test |
| Een netwerkinterface maken |$nic1 = [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) - ResourceGroupName $myResourceGroup-naam 'myNIC' - locatie $location - PrivateIpAddress XX. X.X.X-Subnet $subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Maak een netwerkinterface met behulp van de openbare IP-adres en het virtuele netwerksubnet dat u eerder hebt gemaakt. |

## <a name="get-information-about-network-resources"></a>Informatie over netwerkbronnen

| Taak | Opdracht |
| ---- | ------- |
| Lijst met virtuele netwerken |[Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van de virtuele netwerken in de resourcegroep. |
| Informatie ophalen over een virtueel netwerk |Get-AzVirtualNetwork -Name "myVNet" -ResourceGroupName $myResourceGroup |
| Lijst met subnetten in een virtueel netwerk |Get-AzVirtualNetwork -Name "myVNet" -ResourceGroupName $myResourceGroup &#124; Select Subnets |
| Informatie ophalen over een subnet |[Get-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetworksubnetconfig) -Name "mySubnet1" -VirtualNetwork $vnet<BR><BR>Hiermee haalt informatie over het subnet in het opgegeven virtuele netwerk. De waarde $vnet geeft het object dat wordt geretourneerd door Get-AzVirtualNetwork. |
| Lijst met IP-adressen |[Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van de openbare IP-adressen in de resourcegroep. |
| Lijst met load balancers |[Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van de load balancers in de resourcegroep. |
| Lijst met netwerkinterfaces |[Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterface) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van alle netwerkinterfaces in de resourcegroep. |
| Informatie ophalen over een netwerkinterface |Get-AzNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Hiermee haalt u informatie over een specifieke netwerkinterface. |
| De IP-configuratie van een netwerkinterface |[Get-AzNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterfaceipconfig) -naam 'myNICIP' - NetworkInterface $nic<BR><BR>Hiermee haalt u informatie over de IP-adresconfiguratie van de opgegeven netwerkinterface. De waarde $nic geeft het object dat wordt geretourneerd door Get-AzNetworkInterface. |

## <a name="manage-network-resources"></a>Netwerkresources beheren

| Taak | Opdracht |
| ---- | ------- |
| Voeg een subnet toe aan een virtueel netwerk |[Add-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/add-azvirtualnetworksubnetconfig) -AddressPrefix XX.X.X.X/XX -Name "mySubnet1" -VirtualNetwork $vnet<BR><BR>Hiermee voegt u een subnet toe aan een bestaand virtueel netwerk. De waarde $vnet geeft het object dat wordt geretourneerd door Get-AzVirtualNetwork. |
| Een virtueel netwerk verwijderen |[Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork) -Name "myVNet" -ResourceGroupName $myResourceGroup<BR><BR>Hiermee verwijdert u het opgegeven virtuele netwerk van de resourcegroep. |
| Een netwerkinterface verwijderen |[Remove-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/remove-aznetworkinterface) -Name "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Hiermee verwijdert u de opgegeven netwerkinterface van de resourcegroep. |
| Een load balancer verwijderen |[Remove-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer) -Name "myLoadBalancer" -ResourceGroupName $myResourceGroup<BR><BR>Hiermee verwijdert u de opgegeven load balancer uit de resourcegroep. |
| Een openbaar IP-adres verwijderen |[Remove-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress)-Name "myIPAddress" -ResourceGroupName $myResourceGroup<BR><BR>Hiermee verwijdert u de opgegeven openbare IP-adres uit de resourcegroep. |

## <a name="next-steps"></a>Volgende stappen
* Gebruik de netwerkinterface die u zojuist hebt gemaakt wanneer u [maken van een virtuele machine](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Meer informatie over hoe u kunt [een virtuele machine maken met meerdere netwerkinterfaces](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md).

