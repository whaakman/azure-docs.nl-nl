---
title: On-premises gegevens-gateway gebruiken voor gegevensbronnen voor Azure Virtual Network | Microsoft Docs
description: Informatie over het configureren van een server voor het gebruik van een gateway voor gegevensbronnen op VNet.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7e792114c61c6257f4f5be5bfa65474d595f0d36
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Gateway gebruiken voor gegevensbronnen op een Azure-netwerk (VNet)

In dit artikel beschrijft de **AlwaysUseGateway** eigenschap van de server voor gebruik als gegevensbronnen zijn aangesloten op een [Azure Virtual Network (VNet)](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>Servertoegang tot gegevensbronnen VNet

Als uw gegevensbronnen toegankelijk zijn via een VNet, moet uw Azure Analysis Services-server verbinden met deze gegevensbronnen alsof het on-premises in uw eigen omgeving. U kunt de **AlwaysUseGateway** servereigenschap om op te geven van de server voor toegang tot alle gegevens van de gegevensbron via een [On-premises gateway](analysis-services-gateway.md). 

> [!NOTE]
> Deze eigenschap is effectieve alleen als een [On-premises gegevensgateway](analysis-services-gateway.md) is geïnstalleerd en geconfigureerd. De gateway kan zich op het VNet.

## <a name="configure-alwaysusegateway-property"></a>De eigenschap AlwaysUseGateway configureren

1. In SSMS > server > **eigenschappen** > **algemene**, selecteer **weergeven (alle) geavanceerde eigenschappen**.
2. In de **ASPaaS\AlwaysUseGateway**, selecteer **true**.

    ![Gebruik altijd de gateway-eigenschap](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Zie ook
[Verbinding maken met on-premises gegevensbronnen](analysis-services-gateway.md)   
[Installeren en configureren van een lokale gegevensgateway](analysis-services-gateway-install.md)   
[Azure-netwerk (VNET)](../virtual-network/virtual-networks-overview.md)   

