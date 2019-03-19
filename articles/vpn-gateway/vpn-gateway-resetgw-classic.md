---
title: Een Azure VPN-gateway als u wilt herstellen van IPsec-tunnels opnieuw instellen | Microsoft Docs
description: Dit artikel helpt u bij het opnieuw instellen van uw Azure VPN-Gateway als u wilt herstellen van IPsec-tunnels. Het artikel is van toepassing op VPN-gateways in zowel het klassieke als het Resource Manager-implementatiemodel.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/14/2019
ms.author: cherylmc
ms.openlocfilehash: 54b89b74017b8d5d6e4bd1b52c6b3986d2802702
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58118796"
---
# <a name="reset-a-vpn-gateway"></a>Een VPN-gateway opnieuw instellen

Het opnieuw instellen van een Azure VPN-gateway is handig als u cross-premises VPN-connectiviteit verliest in een of meer Site-to-Site VPN-tunnels. In een dergelijke situatie functioneren al uw on-premises VPN-apparaten naar behoren, maar kunnen ze geen IPSec-tunnels tot stand brengen met de Azure VPN-gateways. Dit artikel helpt u uw VPN-gateway opnieuw instellen.

### <a name="what-happens-during-a-reset"></a>Wat er gebeurt tijdens het opnieuw instellen?

Een VPN-gateway bestaat uit twee VM-exemplaren die worden uitgevoerd in een configuratie actief / stand-by. Wanneer u de gateway opnieuw instelt, wordt de gateway opnieuw opgestart en de cross-premises configuraties hiervoor vervolgens opnieuw. De gateway behoudt hetzelfde openbare IP-adres. Dat betekent dat u de VPN-routerconfiguratie niet hoeft bij te werken met een nieuw openbaar IP-adres voor de Azure VPN-gateway.

Wanneer u de opdracht opnieuw in te stellen van de gateway, de huidige instantie van de Azure VPN-gateway wordt onmiddellijk opnieuw opgestart. Er is een korte onderbreking tijdens de failover van de actieve instantie (opnieuw wordt opgestart), naar de stand-by-instantie. Deze onderbreking zou niet langer dan een minuut moeten duren.

Als de verbinding na de eerste keer opnieuw opstarten niet is hersteld, voert u deze opdracht opnieuw uit om de tweede VM-instantie (de nieuwe actieve gateway) opnieuw op te starten. Als de twee keer opnieuw opstarten direct na elkaar wordt aangevraagd, duurt het iets langer omdat beide VM-exemplaren (actief en stand-by) opnieuw worden opgestart. Dit veroorzaakt een langere onderbreking van de VPN-verbinding, die twee tot vier minuten kan duren, voordat de virtuele machines opnieuw zijn opgestart.

Na twee keer opnieuw opstarten, als u steeds cross-premises connectiviteitsproblemen ondervindt nog, opent u een ondersteuningsaanvraag via Azure portal.

## <a name="before"></a>Voordat u begint

Controleer voordat u de gateway opnieuw instelt de hieronder vermelde belangrijke zaken voor elke S2S-VPN-tunnel (site-naar-site) met IPsec. Als items niet overeenkomen, wordt de verbinding met S2SVPN-tunnels verbroken. Controleren en corrigeren van de configuraties voor uw on-premises en Azure VPN-gateways worden opgeslagen u onnodig opnieuw moet opstarten en dat de andere werkende verbindingen via de gateways.

Controleer of u de volgende items voordat u de gateway opnieuw instelt:

* Controleer of de internet-IP-adressen (VIP's) voor zowel de Azure VPN-gateway als de on-premises VPN-gateway correct zijn geconfigureerd in zowel het Azure- als het lokale VPN-beleid.
* De vooraf gedeelde sleutel moet hetzelfde zijn in de Azure- en de on-premises VPN-gateway.
* Als u een specifieke IPsec/IKE-configuratie, zoals versleuteling, hash-algoritmen en PFS (Perfect Forward Secrecy) toepast, moet u controleren of de Azure- en on-premises VPN-gateway dezelfde configuratie hebben.

## <a name="portal"></a>Azure-portal

U kunt een Resource Manager-VPN-gateway met behulp van de Azure portal opnieuw. Als u een klassieke gateway opnieuw instellen wilt, raadpleegt u de [PowerShell](#resetclassic) stappen.

### <a name="resource-manager-deployment-model"></a>Resource Manager-implementatiemodel

1. Open de [Azure-portal](https://portal.azure.com) en navigeer naar de virtuele netwerkgateway van Resource Manager die u wilt instellen.
2. Klik op 'Herstellen' op de blade voor de virtuele netwerkgateway.

   ![VPN-Gateway-blade opnieuw instellen](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. Klik op de blade opnieuw instellen op de **opnieuw** knop.

## <a name="ps"></a>PowerShell

### <a name="resource-manager-deployment-model"></a>Resource Manager-implementatiemodel

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De cmdlet voor het opnieuw instellen van een gateway is **Reset-AzVirtualNetworkGateway**. Voordat u een opnieuw instellen, zorg ervoor dat u hebt de nieuwste versie van de [Az PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/az.network). Het volgende voorbeeld wordt een virtuele netwerkgateway met de naam VNet1GW in de resourcegroep TestRG1:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Resultaat:

Wanneer u een geretourneerd resultaat ontvangt, kunt u ervan uitgaan dat de gateway opnieuw instellen is voltooid. Er is echter niets in de geretourneerde resultaten die expliciet aangeeft dat het opnieuw instellen voltooid is. Als u zoeken nauw samen met de geschiedenis om te zien precies wanneer de gateway opnieuw instellen is opgetreden wilt, kunt u bekijken die informatie in de [Azure-portal](https://portal.azure.com). Navigeer in de portal naar **'GatewayName' -> Resource Health**.

### <a name="resetclassic"></a>Klassieke implementatiemodel

De cmdlet voor het opnieuw instellen van een gateway is **Reset-AzureVNetGateway**. Voordat u een opnieuw instellen, zorg ervoor dat u hebt de nieuwste versie van de [SM (Service Management) PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets). Het volgende voorbeeld wordt de gateway voor een virtueel netwerk met de naam 'ContosoVNet':

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

Als u wilt de gateway opnieuw instellen, gebruikt u de [az network vnet-gateway opnieuw instellen](https://docs.microsoft.com/cli/azure/network/vnet-gateway) opdracht. Het volgende voorbeeld wordt een virtuele netwerkgateway VNet5GW met de naam in de resourcegroep TestRG5:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Resultaat:

Wanneer u een geretourneerd resultaat ontvangt, kunt u ervan uitgaan dat de gateway opnieuw instellen is voltooid. Er is echter niets in de geretourneerde resultaten die expliciet aangeeft dat het opnieuw instellen voltooid is. Als u zoeken nauw samen met de geschiedenis om te zien precies wanneer de gateway opnieuw instellen is opgetreden wilt, kunt u bekijken die informatie in de [Azure-portal](https://portal.azure.com). Navigeer in de portal naar **'GatewayName' -> Resource Health**.
