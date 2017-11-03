---
title: Een Azure VPN-gateway als u wilt herstellen IPsec-tunnels opnieuw instellen | Microsoft Docs
description: Dit artikel begeleidt u bij het opnieuw instellen van uw Azure VPN-Gateway als u wilt herstellen IPsec-tunnels. Het artikel is van toepassing op VPN-gateways in het klassieke en het Resource Manager-implementatiemodel.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: cherylmc
ms.openlocfilehash: 604af54d034b6b51a96ff098827e2352a163be18
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="reset-a-vpn-gateway"></a>Een VPN-gateway opnieuw instellen

Het opnieuw instellen van een Azure VPN-gateway is handig als u cross-premises VPN-connectiviteit verliest in een of meer Site-to-Site VPN-tunnels. In een dergelijke situatie functioneren al uw on-premises VPN-apparaten naar behoren, maar kunnen ze geen IPSec-tunnels tot stand brengen met de Azure VPN-gateways. In dit artikel helpt u bij uw VPN-gateway opnieuw instellen.

### <a name="what-happens-during-a-reset"></a>Wat er gebeurt tijdens het opnieuw instellen?

Een VPN-gateway bestaat uit twee VM-instanties uitgevoerd in een actief/stand-byconfiguratie. Wanneer u de gateway opnieuw instelt, wordt de gateway opnieuw opgestart en vervolgens wordt de cross-premises configuraties opnieuw toegepast. De gateway behoudt hetzelfde openbare IP-adres. Dat betekent dat u de VPN-routerconfiguratie niet hoeft bij te werken met een nieuw openbaar IP-adres voor de Azure VPN-gateway.

Wanneer u de opdracht opnieuw in te stellen van de gateway, de huidige instantie van de Azure VPN-gateway wordt onmiddellijk opnieuw opgestart. Er is een korte onderbreking tijdens de failover van de actieve instantie (opnieuw wordt opgestart), naar de stand-by-instantie. Deze onderbreking zou niet langer dan een minuut moeten duren.

Als de verbinding na de eerste keer opnieuw opstarten niet is hersteld, voert u deze opdracht opnieuw uit om de tweede VM-instantie (de nieuwe actieve gateway) opnieuw op te starten. Als de twee keer opnieuw opstarten direct na elkaar wordt aangevraagd, duurt het iets langer omdat beide VM-exemplaren (actief en stand-by) opnieuw worden opgestart. Dit veroorzaakt een langere onderbreking van de VPN-verbinding, die twee tot vier minuten kan duren, voordat de virtuele machines opnieuw zijn opgestart.

Na twee keer opnieuw opstarten als u steeds cross-premises connectiviteitsproblemen ondervindt nog, opent u een verzoek om ondersteuning van de Azure-portal.

## <a name="before"></a>Voordat u begint

Controleer voordat u de gateway opnieuw instelt de hieronder vermelde belangrijke zaken voor elke S2S-VPN-tunnel (site-naar-site) met IPsec. Als items niet overeenkomen, wordt de verbinding met S2SVPN-tunnels verbroken. Controleren en te corrigeren van de configuraties voor uw on-premises en Azure VPN-gateways slaat u onnodig opnieuw moet opstarten en dat de andere werkende verbindingen op de gateways.

Controleer of u de volgende items voordat het opnieuw instellen van uw gateway:

* Controleer of de internet-IP-adressen (VIP's) voor zowel de Azure VPN-gateway als de on-premises VPN-gateway correct zijn geconfigureerd in zowel het Azure- als het lokale VPN-beleid.
* De vooraf gedeelde sleutel moet hetzelfde zijn in de Azure- en de on-premises VPN-gateway.
* Als u een specifieke IPsec/IKE-configuratie, zoals versleuteling, hash-algoritmen en PFS (Perfect Forward Secrecy) toepast, moet u controleren of de Azure- en on-premises VPN-gateway dezelfde configuratie hebben.

## <a name="portal"></a>Azure-portal

U kunt een Resource Manager-VPN-gateway met behulp van de Azure-portal opnieuw instellen. Als u een klassieke gateway opnieuw instellen wilt, raadpleegt u de [PowerShell](#resetclassic) stappen.

### <a name="resource-manager-deployment-model"></a>Resource Manager-implementatiemodel

1. Open de [Azure-portal](https://portal.azure.com) en navigeer naar de virtuele netwerkgateway van Resource Manager die u wilt instellen.
2. Klik op 'Herstellen' op de blade voor de virtuele netwerkgateway.

  ![Opnieuw instellen van VPN-Gateway-blade](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. Klik op de blade opnieuw instellen op de **opnieuw** knop.

## <a name="ps"></a>PowerShell

### <a name="resource-manager-deployment-model"></a>Resource Manager-implementatiemodel

De cmdlet voor het opnieuw instellen van een gateway is **Reset AzureRmVirtualNetworkGateway**. Voordat u een reset uitvoert, zorg ervoor dat de nieuwste versie van de [Resource Manager PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0). Het volgende voorbeeld wordt een virtuele netwerkgateway VNet1GW met de naam in de resourcegroep TestRG1:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Resultaat:

Wanneer u een resultaat geretourneerd ontvangt, kunt u aannemen opnieuw instellen van de gateway is geslaagd. Maar is er niets in de geretourneerde resultatenset die expliciet aangeeft dat het opnieuw instellen voltooid is. Als u kijken nauw de geschiedenis om te zien precies wanneer het opnieuw instellen van de gateway is opgetreden wilt, kunt u bekijken die informatie in de [Azure-portal](https://portal.azure.com). Navigeer in de portal naar **'GatewayName' -> resourcestatus**.

### <a name="resetclassic"></a>Klassieke implementatiemodel

De cmdlet voor het opnieuw instellen van een gateway is **Reset-AzureVNetGateway**. Voordat u een reset uitvoert, zorg ervoor dat de nieuwste versie van de [Service Management (SM) PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0). Het volgende voorbeeld wordt de gateway voor een virtueel netwerk met de naam 'ContosoVNet':

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

Resultaat:

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <a name="cli"></a>Azure CLI

Om de gateway opnieuw instellen, gebruikt u de [az vnet-netwerkgateway opnieuw](https://docs.microsoft.com/cli/azure/network/vnet-gateway#az_network_vnet_gateway_reset) opdracht. Het volgende voorbeeld wordt een virtuele netwerkgateway VNet5GW met de naam in de resourcegroep TestRG5:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Resultaat:

Wanneer u een resultaat geretourneerd ontvangt, kunt u aannemen opnieuw instellen van de gateway is geslaagd. Maar is er niets in de geretourneerde resultatenset die expliciet aangeeft dat het opnieuw instellen voltooid is. Als u kijken nauw de geschiedenis om te zien precies wanneer het opnieuw instellen van de gateway is opgetreden wilt, kunt u bekijken die informatie in de [Azure-portal](https://portal.azure.com). Navigeer in de portal naar **'GatewayName' -> resourcestatus**.