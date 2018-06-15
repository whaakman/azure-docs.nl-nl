---
title: Maak een openbare Load Balancer Standard met zonal frontend met Azure CLI | Microsoft Docs
description: Informatie over het maken van een openbare Load Balancer Standard met zonal frontend met Azure CLI
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
ms.openlocfilehash: da18693f090a256bf69ea27e46e0ac010eb293b0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
ms.locfileid: "30324059"
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-frontend-using-azure-cli"></a>Maak een openbare Load Balancer Standard met zonal frontend met Azure CLI

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zonal frontend. Is een zonal frontend betekent dat elk binnenkomend of uitgaand stroom wordt geleverd door een enkele zone in een regio. U kunt een load balancer met een zonal frontend maken met behulp van een zonal standaard openbare IP-adres in de frontend-configuratie. Om te begrijpen hoe beschikbaarheid zones werken met standaard Load Balancer, Zie [standaard Load Balancer en beschikbaarheid zones](load-balancer-standard-availability-zones.md). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, zorg ervoor dat de meest recente geïnstalleerd [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en u bent aangemeld bij een Azure-account met [az aanmelding](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login).

> [!NOTE]
> Ondersteuning voor de beschikbaarheid van Zones is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de volgende opdracht:

```azurecli-interactive
az group create --name myResourceGroupZLB --location westeurope
```

## <a name="create-a-public-standard-ip-address"></a>Een openbare standaard IP-adres maken

Maak een zonal standaard openbare IP-adres met de volgende opdracht:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroupZLB --name myPublicIPZonal --sku Standard --zone 1
```

## <a name="create-a-load-balancer"></a>Een load balancer maken

Maak een openbare Load Balancer Standard met de standaard openbare IP-adres die u hebt gemaakt in de vorige stap met de volgende opdracht:

```azurecli-interactive
az network lb create --resource-group myResourceGroupZLB --name myLoadBalancer --public-ip-address myPublicIPZonal --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Maken van een Load Balancer-test op poort 80

Maak een load balancer health test met de volgende opdracht:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroupZLB --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Maken van een regel voor Load Balancer voor poort 80

Maak een regel voor load balancer met de volgende opdracht:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroupZLB --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [standaard Load Balancer en beschikbaarheid zones](load-balancer-standard-availability-zones.md).



