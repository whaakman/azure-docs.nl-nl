---
title: Maak een openbare Load Balancer Standard met zonal openbare IP-adres frontend met Azure PowerShell | Microsoft Docs
description: Informatie over het maken van openbare Load Balancer Standard met een zonal openbare IP-adres frontend met Azure PowerShell
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: 6fb783b79f0e4fd1c453695c1ebd360b91962f41
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-frontend-using-azure-powershell"></a>Maak een openbare Load Balancer Standard met zonal frontend met Azure PowerShell

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zonal frontend met behulp van een openbare standaard voor IP-adres. Om te begrijpen hoe beschikbaarheid zones werken met standaard Load Balancer, Zie [standaard Load Balancer en beschikbaarheid zones](load-balancer-standard-availability-zones.md). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
> Ondersteuning voor de beschikbaarheid van Zones is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de volgende opdracht:

```powershell
New-AzureRmResourceGroup -Name myResourceGroupZLB -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Maak een openbare IP-standaard 
Maak een openbare IP-standaard met de volgende opdracht:

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupZLB -Name 'myPublicIPZonal' `
  -Location westeurope -AllocationMethod Static -Sku Standard -zone 1
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Een front-end-IP-configuratie voor de website maken

Maak een frontend-IP-configuratie met de volgende opdracht:

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEnd' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>De back-end-adresgroep maken

Maak een back-end-adresgroep op met de volgende opdracht:

```powershell
$bepool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Maken van een load balancer-test op poort 80

Maak een health test op poort 80 voor de load balancer met de volgende opdracht:

```powershell
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken
 Maak een regel voor load balancer met de volgende opdracht:

```powershell
   $rule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Een load balancer maken
Maak een standaard Load-Balancer is met de volgende opdracht:

```powershell
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroupZLB -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [standaard Load Balancer en beschikbaarheid zones](load-balancer-standard-availability-zones.md).



