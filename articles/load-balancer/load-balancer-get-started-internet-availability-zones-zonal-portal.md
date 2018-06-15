---
title: Maak een openbare Load Balancer Standard met zonal openbare IP-adres frontend met Azure portal | Microsoft Docs
description: Informatie over het maken van een openbare Load Balancer Standard met zonal frontend van openbare IP-adres met de Azure-portal
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
ms.openlocfilehash: 533c48b3a004f85dfbd2970d73dcf7de21811dca
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
ms.locfileid: "30324080"
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-public-ip-address-frontend-using-azure-portal"></a>Maak een openbare Load Balancer Standard met zonal openbare IP-adres frontend met Azure portal

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zonal frontend. Om te begrijpen hoe beschikbaarheid zones werken met standaard Load Balancer, Zie [standaard Load Balancer en beschikbaarheid zones](load-balancer-standard-availability-zones.md). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
> Ondersteuning voor de beschikbaarheid van Zones is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Een load balancer met zonal frontend-IP-adres maken

1. Navigeer vanuit een browser naar de Azure-portal: [ http://portal.azure.com ](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Selecteer op de bovenste linkerkant van het scherm en **maken van een resource** > **Networking** > **Load Balancer.**
3. In de **maken load balancer** pagina onder **naam** type **myLoadBalancer**.
4. Selecteer onder **Type** de optie **Openbaar**.
5. Selecteer onder SKU, **standaard**.
6. Klik op **een openbaar IP-adres kiezen**, klikt u op **nieuw**, en in **openbare IP-adres maken** pagina onder de naam, type **myPublicIPZonal**, Selecteer voor de SKU, **standaard**, voor de zone van de beschikbaarheid, selecteert u **1**.
    
>[!NOTE] 
> De openbare IP gemaakt in deze stap is van het standaard SKU standaard.

7. Voor **resourcegroep**, klikt u op **nieuw**, en typ vervolgens **myResourceGroupZLB** als de naam van de resourcegroep.
8. Voor **locatie**, selecteer **West-Europa**, en klik vervolgens op **OK**. De load balancer begint vervolgens met de implementatie. Het duurt een aantal minuten voordat de implementatie is voltooid.

    ![zone-redundante Load Balancer standaard maken met de Azure portal](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [standaard Load Balancer en beschikbaarheid zones](load-balancer-standard-availability-zones.md).



