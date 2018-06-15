---
title: Maak een openbare Load Balancer Standard met zone-redundante openbare IP-adres frontend met Azure portal | Microsoft Docs
description: Informatie over het maken van een openbare Load Balancer Standard met zone-redundante frontend van openbare IP-adres met de Azure-portal
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
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 9a51638ea6d85178e6631ac278c116e4c7e05d61
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34200255"
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Maak een openbare Load Balancer Standard met zone-redundante openbare IP-adres frontend met Azure portal

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zone-redundante frontend met behulp van een openbare standaard voor IP-adres. Er is een enkel frontend-IP-adres op een standaard Load Balancer zone-redundante standaard.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
 Ondersteuning voor de beschikbaarheid van Zones is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Een zone redundant load balancer maken

1. Navigeer vanuit een browser naar de Azure-portal: [ http://portal.azure.com ](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Selecteer op de bovenste linkerkant van het scherm en **maken van een resource** > **Networking** > **Load Balancer.**
3. In de **maken load balancer** pagina onder **naam** type **myLoadBalancer**.
4. Selecteer onder **Type** de optie **Openbaar**.
5. Selecteer onder SKU, **standaard**.
6. Klik op **openbaar IP-adres**, klikt u op **nieuw**, en in **openbare IP-adres maken** pagina onder de naam, type **myPublicIPStandard**.
    >[!NOTE] 
    > De openbare IP gemaakt in deze stap is van de standaard SKU en zone-redundante standaard. 
8. Onder **locatie**, selecteer **Oost-US2**, en klik vervolgens op **OK**. De load balancer begint vervolgens met de implementatie. Het duurt een aantal minuten voordat de implementatie is voltooid.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [standaard Load Balancer en beschikbaarheid zones](load-balancer-standard-availability-zones.md).



