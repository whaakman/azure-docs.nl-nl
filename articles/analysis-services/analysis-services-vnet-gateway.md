---
title: Gebruik On-premises gegevensgateway voor gegevensbronnen voor Azure Virtual Network | Microsoft Docs
description: Informatie over het configureren van een server voor het gebruik van een gateway voor gegevensbronnen op VNet.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 62f6413854ecdfc257655fd864167d8e24d35e06
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427301"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Gateway gebruiken voor gegevensbronnen op een Azure Virtual Network (VNet)

In dit artikel beschrijft de **AlwaysUseGateway** eigenschap van de server voor gebruik bij gegevensbronnen zijn op een [Azure Virtual Network (VNet)](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>Toegang tot de server met VNet-gegevensbronnen

Uw gegevensbronnen zijn toegankelijk via een VNet, uw Azure Analysis Services-server moet verbinding maken met deze gegevensbronnen alsof ze zich on-premises, in uw eigen omgeving. U kunt configureren dat de **AlwaysUseGateway** eigenschap van de server om op te geven van de server voor toegang tot alle gegevens van de gegevensbron via een [On-premises gateway](analysis-services-gateway.md). 

> [!NOTE]
> Deze eigenschap is effectieve alleen wanneer een [On-premises gegevensgateway](analysis-services-gateway.md) is geïnstalleerd en geconfigureerd. De gateway kan zich op het VNet.

## <a name="configure-alwaysusegateway-property"></a>De eigenschap AlwaysUseGateway configureren

1. In SSMS > server > **eigenschappen** > **algemene**, selecteer **weergeven (alle) geavanceerde eigenschappen**.
2. In de **ASPaaS\AlwaysUseGateway**, selecteer **waar**.

    ![Gebruik altijd de gateway-eigenschap](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Zie ook
[Verbinding maken met on-premises gegevensbronnen](analysis-services-gateway.md)   
[Installeren en configureren van een on-premises gegevensgateway](analysis-services-gateway-install.md)   
[Azure-netwerk (VNET)](../virtual-network/virtual-networks-overview.md)   

