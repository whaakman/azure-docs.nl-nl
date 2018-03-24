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
ms.openlocfilehash: 10a264609469245d4743886b58730304da3df7bb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Maak een openbare Load Balancer Standard met zone-redundante openbare IP-adres frontend met Azure portal

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zone-redundante frontend met behulp van een openbare standaard voor IP-adres.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="register-for-availability-zones-preview"></a>Registreren voor beschikbaarheid Zones Preview
 
Beschikbaarheid zones zijn Preview-versie en zijn gereed voor het ontwikkelen en testen van scenario's. Ondersteuning is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Aanmelden bij de Azure portal op https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Een zone redundant load balancer maken

1. Navigeer vanuit een browser naar de Azure-portal: [ http://portal.azure.com ](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Selecteer op de bovenste linkerkant van het scherm en **maken van een resource** > **Networking** > **Load Balancer.**
3. In de ** maken de load balancer, onder **naam** type **myPublicLB**.
4. Selecteer onder **Type** de optie **Openbaar**.
5. Selecteer onder SKU, **Standard (Preview)**.
6. Klik op **openbaar IP-adres**, klikt u op **nieuw**op de **maken van een openbaar IP-adres** pagina onder de naam, type **myPublicIPStandard**, en voor **beschikbaarheid zone (Preview)**, selecteer **Zone-redundante**.
7. Onder **locatie**, selecteer **Oost-US2**, en klik vervolgens op **OK**. De load balancer begint vervolgens met de implementatie. Het duurt een aantal minuten voordat de implementatie is voltooid.

    ![zone-redundante Load Balancer standaard maken met de Azure portal](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [maken van een openbaar IP-adres in een zone beschikbaarheid](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



