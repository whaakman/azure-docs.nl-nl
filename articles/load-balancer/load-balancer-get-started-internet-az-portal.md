---
title: Een Load Balancer maken met zone-redundante front - Azure portal
titlesuffix: Azure Load Balancer
description: Informatie over het maken van een openbare Standard Load Balancer met zone-redundante openbaar IP-adres-frontend met Azure portal
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: bdd890d85fa72098fecaf86a2782a25ceee783b7
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201122"
---
#  <a name="create-a-standard-load-balancer-with-zone-redundant-frontend-using-azure-portal"></a>Een Standard Load Balancer maken met zone-redundante front met behulp van Azure portal

In dit artikel begeleidt bij het maken van een openbare [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) met een zone-redundante front met behulp van een openbare standaard IP-adres. Er is een één front-end-IP-adres op een Standard Load Balancer zone-redundante standaard.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
 Ondersteuning voor Beschikbaarheidszones is beschikbaar voor geselecteerde Azure-resources en regio's en groottefamilies van de virtuele machine. Zie voor meer informatie over hoe u aan de slag en welke Azure-resources, -regio's en VM-groottefamilies u ze met uitproberen kunt, [overzicht van Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Een zone-redundante load balancer maken

1. Navigeer via een browser naar de Azure-portal: [ http://portal.azure.com ](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Selecteer op de bovenste linkerkant van het scherm, **een resource maken** > **netwerken** > **Load Balancer.**
3. In de **load balancer maken** pagina onder **naam** type **myLoadBalancer**.
4. Selecteer onder **Type** de optie **Openbaar**.
5. Selecteer onder SKU, **Standard**.
6. Klik op **openbaar IP-adres**, klikt u op **nieuw**, en in **openbare IP-adres maken** pagina, onder de naam, type **myPublicIPStandard**.
    >[!NOTE] 
    > De openbare IP die zijn gemaakt in deze stap is van standaard-SKU en zone-redundante standaard. 
8. Onder **locatie**, selecteer **VS-Oost 2**, en klik vervolgens op **OK**. De load balancer begint vervolgens met de implementatie. Het duurt een aantal minuten voordat de implementatie is voltooid.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Standard Load Balancer en Beschikbaarheidsset zones](load-balancer-standard-availability-zones.md).



