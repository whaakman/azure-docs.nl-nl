---
title: Maak een openbare Load Balancer Standard met zone-redundante openbare IP-adres frontend met Azure CLI | Microsoft Docs
description: Informatie over het maken van een openbare Load Balancer Standard met zone-redundante openbare IP-adres frontend met Azure CLI
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
ms.openlocfilehash: 769eb86af3e0506ddf03d1ec616d5a17b7e5f714
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-frontend-using-azure-cli"></a>Maak een openbare Load Balancer Standard met zone-redundante frontend met Azure CLI

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zone-redundante frontend met behulp van een openbare standaard voor IP-adres.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Registreren voor beschikbaarheid Zones, Load Balancer Standard en openbare IP-adres standaard Preview

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u Azure CLI versie 2.0.17 zijn uitgevoerd of hoger.  Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

>[!NOTE]
[Load Balancer standaard SKU](https://aka.ms/azureloadbalancerstandard) is momenteel in Preview. Tijdens de preview heeft de functie mogelijk niet dezelfde beschikbaarheid en betrouwbaarheid als functies die al algemeen beschikbaar zijn. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie. Gebruik de algemeen beschikbaar [Load Balancer basis-SKU](load-balancer-overview.md) voor uw productie-services. 

> [!NOTE]
> Beschikbaarheid zones zijn Preview-versie en zijn gereed voor het ontwikkelen en testen van scenario's. Ondersteuning is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Voordat u selecteert een zone of zone-redundante optie voor de frontend openbaar IP-adres voor de Load Balancer, moet u eerst de stappen in uitvoeren [registreren voor de beschikbaarheid van zones preview](https://docs.microsoft.com/azure/availability-zones/az-overview).

Zorg ervoor dat de meest recente geïnstalleerd [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)en u bent aangemeld bij een Azure-account met [az aanmelding](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest#login).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de volgende opdracht:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-public-ip-standard"></a>Maak een openbare IP-standaard

Maak een openbare IP-standaard met de volgende opdracht:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIP --sku Standard
```

## <a name="create-a-load-balancer"></a>Een load balancer maken

Maak een openbare Load Balancer Standard met de standaard openbare IP-adres die u hebt gemaakt in de vorige stap met de volgende opdracht:

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Maken van een Load Balancer-test op poort 80

Maak een load balancer health test met de volgende opdracht:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Maken van een regel voor Load Balancer voor poort 80

Maak een regel voor load balancer met de volgende opdracht:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [maken van een openbaar IP-adres in een zone beschikbaarheid](../virtual-network/create-public-ip-availability-zone-cli.md)



