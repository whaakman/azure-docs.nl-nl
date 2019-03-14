---
title: 'Een virtuele netwerkgateway verwijderen: Azure Portal: Resource Manager | Microsoft Docs'
description: De gateway van een virtueel netwerk met behulp van de Azure-portal in het Resource Manager-implementatiemodel verwijderen.
services: vpn-gateway
documentationcenter: na
author: cherylmc
ms.service: vpn-gateway
ms.date: 10/23/2018
ms.author: cherylmc
ms.topic: conceptual
ms.openlocfilehash: 387b4e982772f22453876e1ea8b9e7c4039601c4
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57790574"
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a>De gateway van een virtueel netwerk met behulp van de portal verwijderen

> [!div class="op_single_selector"]
> * [Azure-portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klassiek)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

In dit artikel bevat instructies voor het verwijderen van een Azure VPN-gateways geïmplementeerd met behulp van de Resource Manager-implementatiemodel. Er zijn een aantal verschillende methoden die u kunt uitvoeren wanneer u wilt verwijderen van een virtuele netwerkgateway voor de configuratie van een VPN-gateway.

- Als u wilt Alles verwijderen en opnieuw beginnen, zoals in het geval van een testomgeving, kunt u de resourcegroep verwijderen. Wanneer u een resourcegroep verwijdert, worden alle resources binnen de groep verwijderd. Deze methode wordt alleen aanbevolen als u niet wilt dat een van de resources in de resourcegroep. U kunt slechts een paar resources met behulp van deze benadering selectief niet verwijderen.

- Als u wilt dat sommige van de resources in de resourcegroep, wordt het verwijderen van een virtuele netwerkgateway iets gecompliceerdere. Voordat u de virtuele netwerkgateway verwijderen kunt, moet u eerst alle resources die afhankelijk van de gateway zijn verwijderen. De stappen die u volgt, is afhankelijk van het type van de verbindingen die u hebt gemaakt en de afhankelijke resources voor elke verbinding.

> [!IMPORTANT]
> De onderstaande instructies wordt beschreven hoe u Azure VPN-gateways die zijn geïmplementeerd met behulp van de Resource Manager-implementatiemodel te verwijderen. Als u wilt verwijderen van een VPN-gateway geïmplementeerd met behulp van het klassieke implementatiemodel,. Gebruik Azure PowerShell beschreven [hier](vpn-gateway-delete-vnet-gateway-classic-powershell.md).


## <a name="delete-a-vpn-gateway"></a>Een VPN-gateway verwijderen

Als u wilt verwijderen van een virtuele netwerkgateway, moet u eerst elke resource die betrekking hebben op de virtuele netwerkgateway verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden.

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

Op dit moment wordt de virtuele netwerkgateway verwijderd. De volgende stappen te verwijderen van alle resources die niet meer worden gebruikt.

### <a name="to-delete-the-local-network-gateway"></a>De lokale netwerkgateway verwijderen

1. In **alle resources**, Ga naar de lokale netwerkgateways die gekoppeld aan elke verbinding zijn.
2. Op de **overzicht** blade voor de lokale netwerkgateway, klikt u op **verwijderen**.

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a>De openbare IP-adresresource voor de gateway verwijderen

1. In **alle resources**, zoek de resource van het openbare IP-adres dat gekoppeld aan de gateway is. Als de virtuele netwerkgateway actief / actief is, ziet u twee openbare IP-adressen. 
2. Op de **overzicht** pagina voor het openbare IP-adres, klikt u op **verwijderen**, klikt u vervolgens **Ja** om te bevestigen.

### <a name="to-delete-the-gateway-subnet"></a>Het gatewaysubnet verwijderen

1. In **alle resources**, zoek het virtuele netwerk. 
2. Op de **subnetten** blade, klikt u op de **GatewaySubnet**, klikt u vervolgens op **verwijderen**. 
3. Klik op **Ja** om te bevestigen dat u wilt verwijderen van het gatewaysubnet.

## <a name="deleterg"></a>Een VPN-gateway verwijderen door de resourcegroep te verwijderen

Als u zich geen zorgen over een van uw resources in de resourcegroep te blijven en u alleen wilt beginnen, kunt u een hele resourcegroep verwijderen. Dit is een snelle manier om alles te verwijderen. De volgende stappen gelden alleen voor de Resource Manager-implementatiemodel.

1. In **alle resources**, zoek de resourcegroep en klik op om de blade te openen.
2. Klik op **Verwijderen**. Bekijk de betrokken resources op de blade verwijderen. Zorg ervoor dat u wilt verwijderen van al deze resources. Als dit niet het geval is, gebruik dan de stappen in verwijderen een VPN-gateway aan de bovenkant van dit artikel.
3. Als u wilt doorgaan, typ de naam van de resourcegroep die u wilt verwijderen en klik vervolgens op **verwijderen**.
