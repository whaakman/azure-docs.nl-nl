---
title: 'Verwijderen van een virtuele netwerkgateway: Azure-portal: Resource Manager | Microsoft Docs'
description: Verwijder de gateway van een virtueel netwerk met behulp van de Azure-portal in het Resource Manager-implementatiemodel.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: b014d43ab25124d3e08e19ca190b320fc8456593
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a>Verwijder de gateway van een virtueel netwerk met behulp van de portal

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klassiek)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

Dit artikel bevat de instructies voor het verwijderen van een Azure VPN-gateways geïmplementeerd met het implementatiemodel van Resource Manager. Er zijn verschillende manieren die kunt u uw als u wilt verwijderen van een virtuele netwerkgateway voor de configuratie van een VPN-gateway.

- Als u wilt Alles verwijderen en opnieuw beginnen, zoals in het geval van een testomgeving kunt u de resourcegroep verwijderen. Wanneer u een resourcegroep verwijdert, worden alle resources binnen de groep verwijderd. Deze methode wordt alleen aanbevolen als u niet wilt dat de bronnen in de resourcegroep. U kunt alleen een aantal bronnen met deze benadering selectief niet verwijderen.

- Als u wilt dat een aantal van de resources in de resourcegroep, verwijderen van een virtuele netwerkgateway enigszins iets gecompliceerder. Voordat u de virtuele netwerkgateway verwijderen kunt, moet u eerst alle bronnen die afhankelijk van de gateway zijn te verwijderen. De stappen die u volgt, is afhankelijk van het type van de verbindingen die u hebt gemaakt en de afhankelijke resources voor elke verbinding.

> [!IMPORTANT]
> De onderstaande instructies beschreven hoe u Azure VPN-gateways geïmplementeerd met het implementatiemodel van Resource Manager verwijderen. Als u wilt verwijderen van een VPN-gateway geïmplementeerd met behulp van het klassieke implementatiemodel, gebruik Azure PowerShell zoals beschreven [hier](vpn-gateway-delete-vnet-gateway-classic-powershell.md).


## <a name="delete-a-vpn-gateway"></a>Een VPN-gateway verwijderen

Als u wilt verwijderen van een virtuele netwerkgateway, moet u eerst elke bron die betrekking op de virtuele netwerkgateway hebben verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden.

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

Op dit punt wordt wordt de virtuele netwerkgateway verwijderd. De volgende stappen kunt u alle resources verwijderen die niet langer worden gebruikt.

### <a name="to-delete-the-local-network-gateway"></a>De lokale netwerkgateway verwijderen

1. In **alle resources**, Ga naar de lokale netwerkgateways die gekoppeld aan elke verbinding zijn.
2. Op de **overzicht** blade voor de lokale netwerkgateway, klikt u op **verwijderen**.

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a>De bron van de openbare IP-adres voor de gateway verwijderen

1. In **alle resources**, de bron van openbare IP-adres dat gekoppeld aan de gateway is gevonden. Als de virtuele netwerkgateway actief-actief is, ziet u twee openbare IP-adressen. 
2. Op de **overzicht** pagina voor het openbare IP-adres, klikt u op **verwijderen**, klikt u vervolgens **Ja** om te bevestigen.

### <a name="to-delete-the-gateway-subnet"></a>Het gatewaysubnet verwijderen

1. In **alle resources**, zoek het virtuele netwerk. 
2. Op de **subnetten** blade, klikt u op de **GatewaySubnet**, klikt u vervolgens op **verwijderen**. 
3. Klik op **Ja** om te bevestigen dat u wilt verwijderen van het gatewaysubnet.

## <a name="deleterg"></a>Een VPN-gateway verwijderen door het verwijderen van de resourcegroep

Als u zich geen zorgen over het houden van uw resources in de resourcegroep en u wilt beginnen, kunt u een hele resourcegroep verwijderen. Dit is een snelle manier om alles verwijderen. De volgende stappen gelden alleen voor het Resource Manager-implementatiemodel.

1. In **alle resources**, zoek de resourcegroep en klik op om de blade te openen.
2. Klik op **Verwijderen**. Bekijk de betrokken bronnen op de blade verwijderen. Zorg ervoor dat u wilt verwijderen van al deze resources. Als dit niet het geval is, gebruik de stappen in [verwijderen van een VPN-gateway](#deletegw) boven aan dit artikel.
3. Typ de naam van de resourcegroep die u wilt verwijderen en klik vervolgens op om door te gaan **verwijderen**.
