---
title: Een Load Balancer maken met zonegebonden frontend - Azure CLI
titlesuffix: Azure Load Balancer
description: Informatie over het maken van een Standard Load Balancer met zonegebonden frontend met behulp van Azure CLI
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: 466fa37b8ed7a182eb8bd6bc4fe68e0715056398
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200867"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-cli"></a>Een Standard Load Balancer maken met zonegebonden frontend met behulp van Azure CLI

In dit artikel begeleidt bij het maken van een openbare [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) met een zonegebonden frontend. Een zonegebonden frontend betekent dat binnenkomende of uitgaande stromen worden geleverd door een enkele zone in een regio hebben. U kunt een load balancer maken met een zonegebonden frontend met behulp van een zonegebonden standaard, openbaar IP-adres in de front-end-configuratie. Zie voor meer informatie over de werking van beschikbaarheidszones met Standard Load Balancer, [Standard Load Balancer en Beschikbaarheidsset zones](load-balancer-standard-availability-zones.md). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, zorg ervoor dat u de meest recente hebt geïnstalleerd [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en bent aangemeld bij een Azure-account met [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login).

> [!NOTE]
> Ondersteuning voor Beschikbaarheidszones is beschikbaar voor geselecteerde Azure-resources en regio's en groottefamilies van de virtuele machine. Zie voor meer informatie over hoe u aan de slag en welke Azure-resources, -regio's en VM-groottefamilies u ze met uitproberen kunt, [overzicht van Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de volgende opdracht:

```azurecli-interactive
az group create --name myResourceGroupZLB --location westeurope
```

## <a name="create-a-public-standard-ip-address"></a>Openbaar, standaard IP-adres maken

Maak een zonegebonden standaard, openbaar IP-adres met de volgende opdracht:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroupZLB --name myPublicIPZonal --sku Standard --zone 1
```

## <a name="create-a-load-balancer"></a>Een load balancer maken

Een openbare Standard Load Balancer maken met de standaard openbare IP-adres dat u hebt gemaakt in de vorige stap met de volgende opdracht:

```azurecli-interactive
az network lb create --resource-group myResourceGroupZLB --name myLoadBalancer --public-ip-address myPublicIPZonal --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Maken van een LB probe op poort 80

Maak een load balancer-statustest met behulp van de volgende opdracht uit:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroupZLB --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Maak een LB-regel voor poort 80

Maak een load balancer-regel met de volgende opdracht:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroupZLB --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Standard Load Balancer en Beschikbaarheidsset zones](load-balancer-standard-availability-zones.md).



