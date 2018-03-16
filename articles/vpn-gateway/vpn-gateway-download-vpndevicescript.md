---
title: 'Downloaden van de VPN-apparaat configuratiescripts voor S2S-VPN-verbindingen: Azure Resource Manager | Microsoft Docs'
description: Dit artikel begeleidt u bij het downloaden van de VPN-apparaat configuratiescripts voor S2S-VPN-verbindingen met Azure VPN-Gateways met Azure Resource Manager.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2018
ms.author: yushwang
ms.openlocfilehash: 2ec428bc5d2cdfb376db3c27b9899014c7ffa2af
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="download-vpn-device-configuration-scripts-for-s2s-vpn-connections"></a>Downloaden van de VPN-apparaat configuratiescripts voor S2S-VPN-verbindingen

Dit artikel begeleidt u bij het downloaden van de VPN-apparaat configuratiescripts voor S2S-VPN-verbindingen met Azure VPN-Gateways met Azure Resource Manager. Het volgende diagram toont de werkstroom op hoog niveau.

![Download-script](./media/vpn-gateway-download-vpndevicescript/downloaddevicescript.png)

## <a name="about"></a>Over VPN-apparaat-configuratiescripts

Er bestaat een cross-premises VPN-verbinding van een Azure VPN-gateway, een on-premises VPN-apparaat en een verbinding te maken van de twee IPsec S2S VPN-tunnel. De gangbare werkstroom bevat de volgende stappen uit:

1. Maken en configureren van een Azure VPN-gateway (virtuele netwerkgateway)
2. Maken en configureren van een Azure lokale netwerkgateway met uw on-premises netwerk en de VPN-apparaat
3. Een Azure VPN-verbinding tussen de Azure VPN-gateway en de lokale netwerkgateway maken en configureren
4. De on-premises VPN-apparaat dat wordt vertegenwoordigd door de lokale netwerkgateway tot stand brengen van de werkelijke S2S VPN-tunnel met de Azure VPN-gateway configureren

U kunt stappen 1 tot en met 3 met de Azure [portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md), of [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md). De laatste stap omvat het configureren van de on-premises VPN-apparaten buiten Azure. Deze functie kunt u een configuratiescript voor uw VPN-apparaat met de bijbehorende waarden van uw Azure VPN-gateway, virtuele netwerk en adresvoorvoegsels voor lokale netwerk, en eigenschappen van VPN-verbinding, enz. al ingevuld downloaden. U kunt het script als uitgangspunt gebruiken of het script rechtstreeks toepassen op uw on-premises VPN-apparaten via de configuration-console.

> [!IMPORTANT]
> * De syntaxis voor elke configuratie van VPN-apparaatscript is verschillende en sterk afhankelijk van de modellen en firmware-versies. Besteed extra aandacht aan uw apparaat model en de versie-informatie met betrekking tot de beschikbare sjablonen.
> * Sommige parameterwaarden uniek moeten zijn op het apparaat en kunnen niet worden bepaald zonder toegang tot het apparaat. De van Azure-configuratiescripts vooraf vult u deze waarden, maar u moet om te controleren of de opgegeven waarden zijn geldig zijn op uw apparaat. Voor voorbeelden:
>    * Interface-nummers
>    * Access control lijst cijfers
>    * Beleidsnamen of cijfers, enzovoort.
> * Zoekt u naar het sleutelwoord '**vervangen**' ingesloten in het script te vinden van de parameters die u controleren wilt alvorens het script toe te passen.
> * Sommige sjablonen hebben een '**OPSCHONEN**' sectie die u kunt toepassen om de configuraties worden verwijderd. De secties opschonen zijn standaard uitgecommentarieerd.

## <a name="download-the-configuration-script-from-azure-portal"></a>Download het configuratiescript vanuit Azure-portal

Een Azure VPN-gateway, lokale netwerkgateway en een verbinding te maken van de twee verbindingsbron maken. De volgende pagina begeleidt u bij de stappen uit:

* [Maak een Site-naar-Site-verbinding in de Azure portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Zodra de verbindingsbron is gemaakt, volgt u onderstaande instructies voor het downloaden van de VPN-apparaat-configuratiescripts:

1. Navigeer via een browser naar de [Azure-portal](http://portal.azure.com) en, indien nodig, meld u aan met uw Azure-account
2. Ga naar de verbindingsbron die u hebt gemaakt. U kunt de lijst van alle verbindingsbronnen die vinden door te klikken op 'Alle services', en vervolgens 'Netwerken' en "Verbindingen."

    ![lijst met verbindingen](./media/vpn-gateway-download-vpndevicescript/connectionlist.png)

3. Klik op de verbinding die u wilt configureren.

    ![overzicht van de verbinding](./media/vpn-gateway-download-vpndevicescript/connectionoverview.png)

4. Klik op de koppeling 'Downloaden configuration' als gemarkeerd in rood op de overzichtspagina van verbinding; Hiermee opent u de pagina 'Downloaden configuratie'.

    ![download-script-1](./media/vpn-gateway-download-vpndevicescript/downloadscript-1.png)

5. De model-familie en firmware-versie van uw VPN-apparaat selecteren en klik vervolgens op de knop 'Downloaden configuratie'.

    ![download66-script-2](./media/vpn-gateway-download-vpndevicescript/downloadscript-2.PNG)

6. U wordt gevraagd om op te slaan van het gedownloade script (een tekstbestand) vanuit de browser.
7. Zodra u het configuratiescript hebt gedownload, opent u het met een teksteditor en zoekt u het sleutelwoord 'REPLACE' om te bepalen en onderzoek de parameters die mogelijk moeten worden vervangen.

    ![script bewerken](./media/vpn-gateway-download-vpndevicescript/editscript.png)

## <a name="download-the-configuration-script-using-azure-powershell"></a>Download het configuratiescript met Azure PowerShell

U kunt ook het configuratiescript met Azure PowerShell downloaden, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
$Sub         = "<YourSubscriptionName>"
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite5"

Login-AzureRmAccount
Set-AzureRmContext -Subscription $Sub

# List the available VPN device models and versions
Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration script for the connection
Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG -DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="apply-the-configuration-script-to-your-vpn-device"></a>Script voor de configuratie van toepassing op uw VPN-apparaat

Nadat u hebt gedownload en het configuratiescript gevalideerd, wordt de volgende stap is het script toepassen op uw VPN-apparaat. De werkelijke procedure afhankelijk van uw VPN-apparaten maakt en -modellen. Raadpleeg de handleidingen bewerking of de instructie pagina's voor uw VPN-apparaten.

## <a name="next-steps"></a>Volgende stappen

Doorgaan met het configureren van uw [Site-naar-Site-verbinding](vpn-gateway-howto-site-to-site-resource-manager-portal.md).
