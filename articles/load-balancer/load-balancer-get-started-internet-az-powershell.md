---
title: Maak een openbare Load Balancer Standard met zone-redundante frontend met openbare IP-adres met behulp van PowerShell | Microsoft Docs
description: Informatie over het maken van openbare Load Balancer Standard met een zone-redundante openbare IP-adres frontend met behulp van PowerShell
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2017
ms.author: kumud
ms.openlocfilehash: 9987fe1273dc05a2ad10c65325ad1d487d38247e
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-powershell"></a>Maak een openbare Load Balancer Standard met zone-redundante frontend met openbare IP-adres met behulp van PowerShell

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zone-redundante frontend met behulp van een openbare standaard voor IP-adres.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Registreren voor beschikbaarheid Zones, Load Balancer Standard en openbare IP-adres standaard Preview

In dit artikel is vereist dat u beschikt over versie 4.4.0 of hoger van de module AzureRM geïnstalleerd. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u wilt installeren of upgraden, installeert u de nieuwste versie van de module AzureRM op basis van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM).

>[!NOTE]
[Load Balancer standaard SKU](https://aka.ms/azureloadbalancerstandard) is momenteel in Preview. Tijdens de preview heeft de functie mogelijk niet dezelfde beschikbaarheid en betrouwbaarheid als functies die al algemeen beschikbaar zijn. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie. Gebruik de algemeen beschikbaar [Load Balancer basis-SKU](load-balancer-overview.md) voor uw productie-services. 

> [!NOTE]
> Beschikbaarheid zones zijn Preview-versie en zijn gereed voor het ontwikkelen en testen van scenario's. Ondersteuning is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Voordat u selecteert een zone of zone-redundante optie voor de frontend openbaar IP-adres voor de Load Balancer, moet u eerst de stappen in uitvoeren [registreren voor de beschikbaarheid van zones preview](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de volgende opdracht:

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Maak een openbare IP-standaard 
Maak een openbare IP-standaard met de volgende opdracht:

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Name 'myPublicIP' `
  -Location westeurope -AllocationMethod Static -Sku Standard 
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Een front-end-IP-configuratie voor de website maken

Maak een frontend-IP-configuratie met de volgende opdracht:

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
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
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroup -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [maken van een openbaar IP-adres in een zone beschikbaarheid](../virtual-network/create-public-ip-availability-zone-portal.md)



