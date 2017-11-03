---
title: Maak een openbare Load Balancer Standard met zone-redundante openbare IP-adres frontend met Azure portal | Microsoft Docs
description: Informatie over het maken van een openbare Load Balancer Standard met zone-redundante frontend van openbare IP-adres met de Azure-portal
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
ms.openlocfilehash: e28983f6015f8dcaa546c20fd9bc8d986a15ab5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Maak een openbare Load Balancer Standard met zone-redundante openbare IP-adres frontend met Azure portal

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zone-redundante frontend met behulp van een openbare standaard voor IP-adres.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Registreren voor beschikbaarheid Zones, Load Balancer Standard en openbare IP-adres standaard Preview
 
>[!NOTE]
[Load Balancer standaard SKU](https://aka.ms/azureloadbalancerstandard) is momenteel in Preview. Tijdens de preview heeft de functie mogelijk niet dezelfde beschikbaarheid en betrouwbaarheid als functies die al algemeen beschikbaar zijn. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie. Gebruik de algemeen beschikbaar [Load Balancer basis-SKU](load-balancer-overview.md) voor uw productie-services. 

> [!NOTE]
> Beschikbaarheid zones zijn Preview-versie en zijn gereed voor het ontwikkelen en testen van scenario's. Ondersteuning is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Voordat u selecteert een zone of zone-redundante optie voor de frontend openbaar IP-adres voor de Load Balancer, moet u eerst de stappen in uitvoeren [registreren voor de beschikbaarheid van zones preview](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Aanmelden bij de Azure portal op https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Een zone redundant load balancer maken

1. Navigeer via een browser naar Azure Portal: [http://portal.azure.com](http://portal.azure.com). Meld u vervolgens aan met uw Azure-account.
2. Selecteer op de bovenste linkerkant van het scherm en **nieuw** > **Networking** > **Load Balancer.**
3. In de ** maken de load balancer, onder **naam** type **myPublicLB**.
4. Selecteer onder **Type** de optie **Openbaar**.
5. Selecteer onder SKU, **Standard (Preview)**.
6. Klik op **openbaar IP-adres**, klikt u op **nieuw**op de **maken van een openbaar IP-adres** pagina onder de naam, type **myPublicIPStandard**, en voor **beschikbaarheid zone (Preview)**, selecteer **Zone-redundante**.
7. Onder **locatie**, selecteer **Oost-US2**, en klik vervolgens op **OK**. De load balancer begint vervolgens met de implementatie. Het duurt een aantal minuten voordat de implementatie is voltooid.

    ![zone-redundante Load Balancer standaard maken met de Azure portal](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [maken van een openbaar IP-adres in een zone beschikbaarheid](../virtual-network/create-public-ip-availability-zone-portal.md)



