---
title: 'Configuratiescripts voor VPN-apparaat voor S2S-VPN-verbindingen downloaden: Azure Resource Manager | Microsoft Docs'
description: Dit artikel begeleidt u bij het downloaden van configuratiescripts voor VPN-apparaat voor S2S-VPN-verbindingen met Azure VPN-Gateways met behulp van Azure Resource Manager.
services: vpn-gateway
author: yushwang
manager: rossort
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/09/2019
ms.author: yushwang
ms.openlocfilehash: f7ee53c10c6597dbf98f8f85fc31fe789137471e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57996318"
---
# <a name="download-vpn-device-configuration-scripts-for-s2s-vpn-connections"></a>Configuratiescripts voor VPN-apparaat voor S2S-VPN-verbindingen downloaden

Dit artikel begeleidt u bij het downloaden van configuratiescripts voor VPN-apparaat voor S2S-VPN-verbindingen met Azure VPN-Gateways met behulp van Azure Resource Manager. Het volgende diagram toont de werkstroom op hoog niveau.

![Download-script](./media/vpn-gateway-download-vpndevicescript/downloaddevicescript.png)

De volgende apparaten hebt beschikbaar scripts:

[!INCLUDE [scripts](../../includes/vpn-gateway-device-configuration-scripts.md)]

## <a name="about"></a>Over configuratiescripts voor VPN-apparaat

Een cross-premises VPN-verbinding bestaat uit een Azure VPN-gateway, een on-premises VPN-apparaat en een verbinding te maken van de twee IPSec-S2S VPN-tunnel. De gebruikelijke werkstroom bevat de volgende stappen uit:

1. Maken en configureren van een Azure VPN-gateway (virtuele netwerkgateway)
2. Maken en configureren van een Azure lokale netwerkgateway met uw on-premises netwerk en de VPN-apparaat
3. Een Azure VPN-verbinding tussen de Azure VPN-gateway en de lokale netwerkgateway maken en configureren
4. De on-premises VPN-apparaat dat wordt vertegenwoordigd door de lokale netwerkgateway tot stand brengen van de werkelijke S2S VPN-tunnel met de Azure VPN-gateway configureren

U kunt stappen 1 t/m 3 met behulp van Azure [portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md), of [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md). De laatste stap bestaat uit het configureren van de on-premises VPN-apparaten buiten Azure. Deze functie kunt u voor het downloaden van een configuratiescript voor uw VPN-apparaat met de bijbehorende waarden van uw Azure VPN-gateway, virtueel netwerk, en adresvoorvoegsels voor on-premises netwerk, en eigenschappen van de VPN-verbinding, enz. al ingevuld. U kunt het script als uitgangspunt te gebruiken, of het script toepassen rechtstreeks naar uw on-premises VPN-apparaten via de configuration-console.

> [!IMPORTANT]
> * De syntaxis voor elke VPN-apparaatscript configuratie is anders en sterk afhankelijk van de modellen en firmware-versies. Speciale aandacht besteden aan uw apparaat model en versie-informatie met betrekking tot de beschikbare sjablonen.
> * Sommige parameterwaarden uniek moeten zijn op het apparaat en kunnen niet worden bepaald zonder toegang tot het apparaat. De van Azure-configuratiescripts vooraf vult u deze waarden, maar u wilt controleren of de opgegeven waarden zijn geldig is op uw apparaat. Voor voorbeelden:
>    * Interface-nummers
>    * De lijst met controlenummers toegang
>    * Voor beleidsnamen of nummers, enzovoort.
> * Zoek naar het sleutelwoord '**vervangen**' ingesloten in het script te vinden van de parameters die u controleren wilt voordat u het script.
> * Sommige sjablonen bevatten een "**OPSCHONEN**" sectie die u kunt toepassen om de configuraties worden verwijderd. De secties opschonen zijn standaard opgenomen als opmerkingen.

## <a name="download-the-configuration-script-from-azure-portal"></a>Het configuratiescript downloaden vanuit Azure portal

Maak een Azure VPN-gateway, lokale netwerkgateway en de resource van een verbinding verbinding te maken van de twee. De volgende pagina leidt u door de stappen uit:

* [Een Site-naar-Site-verbinding maken in Azure portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Zodra de resource van de verbinding is gemaakt, volg de onderstaande instructies voor het downloaden van de configuratiescripts voor VPN-apparaten:

1. Vanuit een browser, Ga naar de [Azure-portal](https://portal.azure.com) en, indien nodig, meld u aan met uw Azure-account
2. Ga naar de resource van de verbinding die u hebt gemaakt. U kunt de lijst met alle verbindingsresources vinden door te klikken op 'Alle services', en vervolgens "Netwerken" en "Verbindingen".

    ![lijst met verbindingen](./media/vpn-gateway-download-vpndevicescript/connectionlist.png)

3. Klik op de verbinding die u wilt configureren.

    ![overzicht van verbindingen](./media/vpn-gateway-download-vpndevicescript/connectionoverview.png)

4. Klik op de koppeling 'Downloaden configuration' als in de overzichtspagina; rood gemarkeerde Hiermee opent u de pagina ' downloaden '.

    ![download-script-1](./media/vpn-gateway-download-vpndevicescript/downloadscript-1.png)

5. Selecteer de model-familie en firmware-versie voor uw VPN-apparaat, en klik vervolgens op de knop 'Downloaden-configuratie'.

    ![download66-script-2](./media/vpn-gateway-download-vpndevicescript/downloadscript-2.PNG)

6. U wordt gevraagd om op te slaan van het gedownloade script (een tekstbestand) vanuit uw browser.
7. Nadat u het configuratiescript gedownload, opent u het met een teksteditor en zoekt u het sleutelwoord 'REPLACE' om te bepalen en controleren van de parameters die moeten mogelijk worden vervangen.

    ![edit-script](./media/vpn-gateway-download-vpndevicescript/editscript.png)

## <a name="download-the-configuration-script-using-azure-powershell"></a>Download het configuratiescript met behulp van Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt ook het configuratiescript met behulp van Azure PowerShell, downloaden, zoals wordt weergegeven in het volgende voorbeeld:

```azurepowershell-interactive
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite1"

# List the available VPN device models and versions
Get-AzVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration script for the connection
Get-AzVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG -DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="apply-the-configuration-script-to-your-vpn-device"></a>Het configuratiescript toepassen op uw VPN-apparaat

Nadat u hebt gedownload en het configuratiescript gevalideerd, wordt de volgende stap is het script toepassen op uw VPN-apparaat. De werkelijke procedure is afhankelijk van uw VPN-apparaat merken en modellen. Raadpleeg de handleidingen bewerking of de instructie pagina's voor uw VPN-apparaten.

## <a name="next-steps"></a>Volgende stappen

Doorgaan met het configureren van uw [Site-naar-Site-verbinding](vpn-gateway-howto-site-to-site-resource-manager-portal.md).
