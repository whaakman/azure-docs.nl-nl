---
title: Algemene PowerShell-opdrachten voor Azure Virtual Network | Microsoft Docs
description: Algemene PowerShell-opdrachten om aan te beginnen met het opstellen van een virtueel netwerk en de bijbehorende bronnen voor virtuele machines.
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
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
ms.author: davidmu
ms.openlocfilehash: 31c0e558ca87d918d8e662d7aa4c12502961288a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Algemene PowerShell-opdrachten voor virtuele netwerken van Azure

Als u maken van een virtuele machine wilt, moet u maken een [virtueel netwerk](../../virtual-network/virtual-networks-overview.md) of weten over een bestaand virtueel netwerk waarin de virtuele machine kan worden toegevoegd. Wanneer u een virtuele machine maakt, moet u doorgaans ook rekening houden met het maken van de resources die worden beschreven in dit artikel.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

Sommige variabelen kunnen handig zijn voor u als meer dan één van de opdrachten in dit artikel wordt uitgevoerd:

- $location - de locatie van de netwerkbronnen. U kunt [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) vinden een [geografische regio](https://azure.microsoft.com/regions/) die voor u geschikt.
- $myResourceGroup - de naam van de resourcegroep waar de netwerkbronnen zich bevinden.

## <a name="create-network-resources"></a>Netwerkbronnen maken

| Taak | Opdracht |
| ---- | ------- |
| Subnetconfiguraties maken |$subnet1 = [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" -AddressPrefix XX.X.X.X/XX<BR>$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet2" -AddressPrefix XX.X.X.X/XX<BR><BR>Een typische netwerk wellicht een subnet voor een [internet gerichte load balancer](../../load-balancer/load-balancer-internet-overview.md) en een apart subnet voor een [interne load balancer](../../load-balancer/load-balancer-internal-overview.md). |
| Een virtueel netwerk maken |$vnet = [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) -Name "myVNet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix XX.X.X.X/XX -Subnet $subnet1, $subnet2 |
| Test voor een unieke domeinnaam |[Test-AzureRmDnsAvailability](https://docs.microsoft.com/powershell/module/azurerm.network/test-azurermdnsavailability) -DomainNameLabel "myDNS" -Location $location<BR><BR>U kunt opgeven dat een DNS-domeinnaam voor een [openbare IP-resource](../../virtual-network/virtual-network-ip-addresses-overview-arm.md), waarbij automatisch een toewijzing voor domainname.location.cloudapp.azure.com in het openbare IP-adres in de Azure-beheerde DNS-servers. De naam mag alleen letters, cijfers en afbreekstreepjes. Het eerste en laatste teken moet een letter of getal en de domeinnaam moet uniek zijn binnen de Azure-locatie. Als **waar** wordt geretourneerd, is de voorgestelde naam uniek. |
| Een openbaar IP-adres maken |$pip = [nieuw AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) -naam 'myPublicIp' - ResourceGroupName $myResourceGroup - DomainNameLabel 'myDNS'-locatie $location - AllocationMethod dynamische<BR><BR>Het openbare IP-adres gebruikt de domeinnaam die u eerder hebt getest en wordt gebruikt door de frontend-configuratie van de load balancer. |
| Maken van een frontend-IP-configuratie |$frontendIP = [nieuw AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) -naam 'myFrontendIP' - PublicIpAddress $pip<BR><BR>De frontend-configuratie omvat het openbare IP-adres dat u eerder hebt gemaakt voor binnenkomend netwerkverkeer. |
| Een back-endadresgroep maken |$beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) -Name "myBackendAddressPool"<BR><BR>Interne adressen biedt voor de back-end van de load balancer die toegankelijk zijn via een netwerkinterface. |
| Een test maken |$healthProbe = [nieuw AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) -naam 'myProbe' - RequestPath 'HealthProbe.aspx'-Protocol http-poort 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Bevat statuscontroles die worden gebruikt om te controleren van de beschikbaarheid van exemplaren van virtuele machines in de back-end-adresgroep. |
| Een taakverdelingsregel maken |$lbRule = [nieuw AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) -naam HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-$healthProbe Probe-Protocol Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Bevat regels die een openbare poort op de load balancer aan een poort in de back-end-adresgroep toewijst. |
| Een binnenkomende NAT-regel maken |$inboundNATRule = [nieuw AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) -naam 'myInboundRule1' - FrontendIpConfiguration $frontendIP-Protocol TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Bevat de regels voor toewijzing van een openbare poort op de load balancer voor een poort voor een specifieke virtuele machine in de back-end-adresgroep. |
| Een load balancer maken |$loadBalancer = [nieuw AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancer) - ResourceGroupName $myResourceGroup-naam 'myLoadBalancer'-locatie $location - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-$healthProbe-test |
| Een netwerkinterface maken |$nic1= [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) -ResourceGroupName $myResourceGroup -Name "myNIC" -Location $location -PrivateIpAddress XX.X.X.X -Subnet $subnet2 -LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] -LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Maak een netwerkinterface met de openbare IP-adres en subnet van het virtuele netwerk dat u eerder hebt gemaakt. |

## <a name="get-information-about-network-resources"></a>Informatie over netwerkbronnen ophalen

| Taak | Opdracht |
| ---- | ------- |
| Lijst met virtuele netwerken |[Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van alle virtuele netwerken in de resourcegroep. |
| Informatie ophalen over een virtueel netwerk |Get-AzureRmVirtualNetwork -Name "myVNet" -ResourceGroupName $myResourceGroup |
| Lijst met subnetten in een virtueel netwerk |Get-AzureRmVirtualNetwork -Name "myVNet" -ResourceGroupName $myResourceGroup &#124; Select Subnets |
| Informatie ophalen over een subnet |[Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" -VirtualNetwork $vnet<BR><BR>Hiermee haalt informatie over het subnet in het opgegeven virtuele netwerk. De waarde $vnet geeft het object dat wordt geretourneerd door Get-AzureRmVirtualNetwork. |
| Lijst met IP-adressen |[Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van de openbare IP-adressen in de resourcegroep. |
| Lijst met load balancers |[Get-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermloadbalancer) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van alle load balancers in de resourcegroep. |
| Lijst met netwerkinterfaces |[Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterface) -ResourceGroupName $myResourceGroup<BR><BR>Geeft een lijst van alle netwerkinterfaces in de resourcegroep. |
| Informatie ophalen over een netwerkinterface |Get-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Hiermee haalt u informatie over een specifieke netwerkinterface. |
| De IP-configuratie van een netwerkinterface ophalen |[Get-AzureRmNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterfaceipconfig) -Name "myNICIP" -NetworkInterface $nic<BR><BR>Hiermee haalt u informatie over de IP-adresconfiguratie van de opgegeven netwerkinterface. De waarde $nic geeft het object dat wordt geretourneerd door Get-AzureRmNetworkInterface. |

## <a name="manage-network-resources"></a>Netwerkresources beheren

| Taak | Opdracht |
| ---- | ------- |
| Voeg een subnet toe aan een virtueel netwerk |[Add-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) -AddressPrefix XX.X.X.X/XX -Name "mySubnet1" -VirtualNetwork $vnet<BR><BR>Voegt een subnet toe aan een bestaand virtueel netwerk. De waarde $vnet geeft het object dat wordt geretourneerd door Get-AzureRmVirtualNetwork. |
| Een virtueel netwerk verwijderen |[Remove-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetwork) -Name "myVNet" -ResourceGroupName $myResourceGroup<BR><BR>Het opgegeven virtuele netwerk verwijdert uit de resourcegroep. |
| Verwijderen van een netwerkinterface |[Remove-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermnetworkinterface) -Name "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Verwijdert de opgegeven netwerkinterface van de resourcegroep. |
| Een load balancer verwijderen |[Remove-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermloadbalancer) -Name "myLoadBalancer" -ResourceGroupName $myResourceGroup<BR><BR>De opgegeven load balancer verwijdert uit de resourcegroep. |
| Een openbaar IP-adres verwijderen |[Remove-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermpublicipaddress)-Name "myIPAddress" -ResourceGroupName $myResourceGroup<BR><BR>Verwijdert de opgegeven openbare IP-adres van de resourcegroep. |

## <a name="next-steps"></a>Volgende stappen
* Gebruik de netwerkinterface die u zojuist hebt gemaakt wanneer u [een virtuele machine maken](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Meer informatie over hoe u kunt [een virtuele machine maken met meerdere netwerkinterfaces](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md).

