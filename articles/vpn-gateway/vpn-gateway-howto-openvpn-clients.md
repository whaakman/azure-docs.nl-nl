---
title: OpenVPN Clients configureren voor Azure VPN-Gateway | Microsoft Docs
description: Stappen voor het OpenVPN Clients configureren voor Azure VPN-Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 141f29ea1c7ebdd8208dda6bc39a60f7873dab7d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977839"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway-preview"></a>OpenVPN clients configureren voor Azure VPN-Gateway (Preview)

Dit artikel helpt u bij het configureren van OpenVPN clients.

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder serviceovereenkomst en moet niet worden gebruikt voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voordat u begint

Controleer of dat u de stappen voor het configureren van OpenVPN voor uw VPN-gateway hebt voltooid. Zie voor meer informatie, [OpenVPN configureren voor Azure VPN-Gateway](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Windows-clients

1. Download en installeer de client OpenVPN via de officiële [OpenVPN website](https://openvpn.net/index.php/open-source/downloads.html).
2. Download het VPN-profiel voor de gateway. Dit kan worden uitgevoerd vanaf het tabblad voor punt-naar-site-configuratie in Azure portal, of ' New-AzureRmVpnClientConfiguration' in PowerShell.
3. Pak het profiel. Vervolgens opent u het configuratiebestand vpnconfig.ovpn vanuit de map OpenVPN in Kladblok.
4. Vul in de sectie voor P2S-client met P2S-client certificaat openbare sleutel in base64. In een PEM opgemaakte certificaat, kunt u gewoon het cer-bestand en kopieer de met base64-sleutel tussen de headers van certificaat openen. Hier ziet u hoe u een certificaat om de gecodeerde openbare sleutel te exporteren.
5. Vul in de sectie van de persoonlijke sleutel met P2S-client certificate persoonlijke sleutel in base64. Zie voor meer informatie over het ophalen van de persoonlijke sleutel [de sleutel exporteren](vpn-gateway-certificates-point-to-site.md#clientexport).
6. Alle andere velden niet wijzigen. Gebruik de gevuld in de configuratie in de invoer van de client verbinding maken met de VPN-verbinding.
7. Kopieer het bestand vpnconfig.ovpn naar de map C:\Program Files\OpenVPN\config.
8. Met de rechtermuisknop op het pictogram OpenVPN in de taakbalk en klik op verbinding maken.

## <a name="mac"></a>Mac-clients

1. Download en installeer een OpenVPN-client, zoals [TunnelBlik](https://tunnelblick.net/downloads.html). 
2. Download het VPN-profiel voor de gateway. Dit kan worden uitgevoerd vanaf het tabblad punt-naar-site-configuratie in Azure portal of met behulp van 'New-AzureRmVpnClientConfiguration' in PowerShell.
3. Pak het profiel. Open het configuratiebestand vpnconfig.ovpn vanuit de map OpenVPN in Kladblok.
4. Vul in de sectie voor P2S-client met P2S-client certificaat openbare sleutel in base64. In een PEM opgemaakte certificaat, kunt u gewoon het cer-bestand en kopieer de met base64-sleutel tussen de headers van certificaat openen. Zie [Exporteer de openbare sleutel](vpn-gateway-certificates-point-to-site.md#cer) voor informatie over het exporteren van een certificaat om de gecodeerde openbare sleutel.
5. Vul in de sectie van de persoonlijke sleutel met P2S-client certificate persoonlijke sleutel in base64. Zie [uw persoonlijke sleutel exporteren](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) voor informatie over het ophalen van de persoonlijke sleutel.
6. Alle andere velden niet wijzigen. Gebruik de gevuld in de configuratie in de invoer van de client verbinding maken met de VPN-verbinding.
7. Dubbelklik op het bestand van het profiel voor het maken van het profiel in tunnelblik.
8. Start Tunnelblik uit de toepassingsmap.
9. Klik op het pictogram Tunneblik in het systeemvak en kies verbinding maken.

## <a name="linux"></a>Linux-clients

1. Open een nieuwe sessie. U kunt een nieuwe sessie openen door te drukken 'Ctrl + Alt + t' op hetzelfde moment
2. Voer de volgende opdracht om vereiste onderdelen te installeren:

  ```
  sudo apt-get install openvpn
  sudo apt-get -y install network-manager-openvpn
  sudo service network-manager restart
  ```
3. Download het VPN-profiel voor de gateway. Dit kan worden uitgevoerd vanaf het tabblad configuratie punt-naar-site in de Azure portal of met behulp van 'New-AzureRmVpnClientConfiguration' in PowerShell.
4. Vul in de sectie van de persoonlijke sleutel met P2S-client certificate persoonlijke sleutel in base64. Zie [uw persoonlijke sleutel exporteren](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) voor informatie over het ophalen van de persoonlijke sleutel.
5. Als u wilt verbinding maken met behulp van de opdrachtregel, typt u het volgende:
  
  ```
  Sudo openvpn –config <name and path of your VPN profile file>
  ```
5. Als u wilt verbinding maken met de gebruikersinterface, gaat u naar de systeeminstellingen.
6. Klik op **+** om toe te voegen een nieuwe VPN-verbinding.
7. Onder **toevoegen VPN**, kies **importeren uit bestand...**
8. Blader naar de profielbestand en dubbelklik op of kies **openen**
9. Klik op **toevoegen** op de **toevoegen VPN** venster.
  
  ![Importeren vanuit bestand](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
10. U kunt verbinding maken door het inschakelen van de VPN-verbinding **ON** op de **netwerkinstellingen** pagina of onder het netwerkpictogram in het systeemvak.

## <a name="next-steps"></a>Volgende stappen

Als u wilt dat de VPN-clients kunnen toegang krijgen tot bronnen in een ander vnet (productie), klikt u vervolgens de instructies op de [Vnet-naar-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artikel voor het instellen van een vnet-naar-vnet-verbinding. Houd er rekening dat BGP is ingeschakeld op de gateways en verbindingen, anders niet het verkeer.
