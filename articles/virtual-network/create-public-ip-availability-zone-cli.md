---
title: Maak een zoned openbare IP-adres met de Azure CLI | Microsoft Docs
description: Maak een openbare IP-adres in een zone beschikbaarheid met de Azure CLI.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ef93d43bbd0c58950027810c8c335d9076574326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-cli"></a>Maken van een openbaar IP-adres in een zone beschikbaarheid met de Azure CLI

U kunt een openbare IP-adres in een zone in Azure beschikbaarheid (preview) implementeren. Een zone beschikbaarheid is een fysiek afgescheiden zone in een Azure-regio. Leer hoe u het volgende doet:

> * Maken van een openbaar IP-adres in een zone beschikbaarheid
> * Verwante bronnen die zijn gemaakt in de zone beschikbaarheid identificeren

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Als u wilt installeren en gebruiken van de CLI lokaal, wordt in deze zelfstudie vereist dat u altijd een versie van de Azure CLI groter is dan versie 2.0.17. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Beschikbaarheid zones zijn Preview-versie en zijn gereed voor het ontwikkelen en testen van scenario's. Ondersteuning is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md).

## <a name="create-a-zonal-public-ip-address"></a>Een zonal openbaar IP-adres maken

Maak een openbaar IP-adres in een beschikbaarheidsgroep zone met de volgende opdracht:


```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIp --zone 2 --location westeurope
```
> [!NOTE]
> Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups). Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.

## <a name="get-zone-information-about-a-public-ip-address"></a>Zone-informatie over een openbaar IP-adres ophalen

Haal de gegevens van de zone van een openbaar IP-adres met de volgende opdracht:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIp
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beschikbaarheid zones](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Meer informatie over [openbare IP-adressen](../virtual-network/virtual-network-public-ip-address.md) 
