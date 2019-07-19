---
title: Een Load Balancer met een zonegebonden-frontend maken-Azure Portal
titlesuffix: Azure Load Balancer
description: Meer informatie over het maken van een Standard Load Balancer met zonegebonden frontend met de Azure Portal
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: allensu
ms.openlocfilehash: 9a260c267febb3dc851fe0a8ca8a8e351124116b
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275142"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-portal"></a>Een Standard Load Balancer met zonegebonden-frontend maken met behulp van Azure Portal

In dit artikel wordt beschreven hoe u een open bare [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) maakt met een zonegebonden front-end-IP-configuratie. Zie voor meer informatie over de werking van beschikbaarheidszones met Standard Load Balancer, [Standard Load Balancer en Beschikbaarheidsset zones](load-balancer-standard-availability-zones.md). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
> Ondersteuning voor Beschikbaarheidszones is beschikbaar voor geselecteerde Azure-resources en regio's en groottefamilies van de virtuele machine. Zie voor meer informatie over hoe u aan de slag en welke Azure-resources, -regio's en VM-groottefamilies u ze met uitproberen kunt, [overzicht van Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Een load balancer maken met zonegebonden frontend-IP-adres

1. Navigeer via een browser naar de Azure-portal: [ https://portal.azure.com ](https://portal.azure.com) en meld u aan met uw Azure-account.
2. Selecteer op de bovenste linkerkant van het scherm, **een resource maken** > **netwerken** > **Load Balancer.**
3. In de **load balancer maken** pagina onder **naam** type **myLoadBalancer**.
4. Selecteer onder **Type** de optie **Openbaar**.
5. Selecteer onder SKU, **Standard**.
6. Klik op **kiest u een openbaar IP-adres**, klikt u op **nieuw**, en in **openbare IP-adres maken** pagina, onder de naam, type **myPublicIPZonal**, Selecteer voor de SKU, **Standard**, beschikbaarheidszone, selecteert u **1**.
    
>[!NOTE] 
> De openbare IP die zijn gemaakt in deze stap is van de standaard-SKU standaard.

1. Voor **resourcegroep**, klikt u op **nieuw**, en typ vervolgens **myResourceGroupZLB** als de naam van de resourcegroep.
1. Voor **locatie**, selecteer **West-Europa**, en klik vervolgens op **OK**. De load balancer begint vervolgens met de implementatie. Het duurt een aantal minuten voordat de implementatie is voltooid.

    ![Maak zone-redundante Standard Load Balancer met de Azure Portal](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Standard Load Balancer en Beschikbaarheidsset zones](load-balancer-standard-availability-zones.md).



