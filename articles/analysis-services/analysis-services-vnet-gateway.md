---
title: On-premises gegevens-gateway gebruiken voor gegevensbronnen voor Azure Virtual Network | Microsoft Docs
description: Informatie over het configureren van een server voor het gebruik van een gateway voor gegevensbronnen op VNet.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7cb1fb7bcd3a50532d5ff994afac56e226fb2018
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596903"
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

