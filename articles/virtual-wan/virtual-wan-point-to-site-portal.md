---
title: Azure Virtual WAN gebruiken om een punt-naar-site-verbinding te maken met Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Virtual WAN kunt gebruiken om een punt-naar-site-VPN-verbinding te maken met Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect remote users to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: e3d1a79dc7dd775cea71df2f36a5f34d85f3e240
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261805"
---
# <a name="tutorial-create-a-point-to-site-connection-using-azure-virtual-wan-preview"></a>Zelfstudie: Een punt-naar-site-verbinding maken met Azure Virtual WAN (Preview)

In deze zelfstudie leert u hoe u Virtual WAN kunt gebruiken om verbinding te maken met uw resources in Azure via een VPN-verbinding met IPsec/IKE (IKEv2)- of OpenVPN. Voor dit soort verbinding moet een client op de clientcomputer worden geconfigureerd. Zie voor meer informatie over Virtual WAN het [Overzicht van Virtual WAN](virtual-wan-about.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een WAN maken
> * Een P2S-configuratie maken
> * Een hub maken
> * P2S-configuratie toepassen op een hub
> * Een VNet verbinden met een hub
> * Een configuratie voor een VPN-client downloaden en toepassen
> * Uw virtuele WAN weergeven
> * Resourcestatus weergeven
> * Een verbinding bewaken

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Deze functie registreren

Klik op **Proberen** om deze functie eenvoudig te registreren met Azure Cloud Shell.

>[!NOTE]
>Als u deze functie niet registreert, kunt u de functie niet gebruiken en wordt deze ook niet weergegeven in de portal.
>
>

Wanneer u op **Proberen** hebt geklikt om Azure Cloud Shell te openen, kopieert en plakt u de volgende opdrachten:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```
 
```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Zodra wordt weergegeven dat de functie is geregistreerd, registreert u het abonnement opnieuw bij de naamruimte Microsoft.Network.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Een virtueel netwerk maken

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Een virtueel WAN maken

Open een browser, ga naar de [Azure-portal (preview)](https://aka.ms/azurevirtualwanpreviewfeatures) en meld u aan met uw Azure-account.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Een hub maken

> [!NOTE]
> Selecteer de instelling ‘Punt-naar-site-gateway’ niet in deze stap.
>

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="site"></a>4. Een P2S-configuratie maken

Een P2S-configuratie definieert de parameters om verbinding te maken met externe clients.

1. Ga naar **Alle resources**.
2. Klik op de virtuele WAN die u hebt gemaakt.
3. Klik onder **Virtual WAN-architectuur** op **Punt-naar-site-configuraties**.
4. Klik op **+ Punt-naar-site-configuratie toevoegen** aan de bovenkant van de pagina om de pagina **Nieuwe punt-naar-site-configuratie maken** te openen.
5. Vul op de pagina **Nieuwe punt-naar-site-configuratie** de volgende velden in:

  *  **Naam van configuratie** - Dit is de naam waarmee u naar uw configuratie wilt verwijzen.
  *  **Type tunnel** -Het protocol dat voor de tunnel moet worden gebruikt.
  *  **Adresgroep** -Dit is de IP-adresgroep op basis waarvan de clients worden toegewezen.
  *  **Certificaatnaam van hoofdmap** - Een beschrijvende naam voor het certificaat.
  *  **Certificaatgegevens van hoofdmap** - Base-64-gecodeerde X.509-certificaatgegevens.

5. Klik op **Maken** om de configuratie te maken.

## <a name="hub"></a>5. Hubtoewijzing bewerken

1. Klik op de pagina voor uw virtuele WAN op **Hubs**.
2. Selecteer de hub waaraan u de punt-naar-site-configuratie wilt toewijzen.
3. Klik op **"..."** en kies **Virtuele hub bewerken**
4. Schakel het selectievakje **Punt-naar-site-gateway opnemen** in.
5. Kies de **gateway-schaaleenheden** en de **punt-naar-site-configuratie**, en een **adresgroep** voor de clients.
6. Klik op **Bevestigen**. 
7. De bewerking kan dertig minuten duren.

## <a name="vnet"></a>6. Uw VNet verbinden met een hub

In deze stap brengt u de peering-verbinding tussen uw hub en een VNet tot stand. Herhaal deze stappen voor elk VNet dat u wilt verbinden.

1. Klik op de pagina voor uw virtuele WAN op **Virtuele netwerkverbinding**.
2. Klik op de pagina Virtuele netwerkverbinding op **+Verbinding toevoegen**.
3. Vul de volgende velden in op de pagina **Verbinding toevoegen**:

    * **Verbindingsnaam** - voer een naam in voor uw verbinding.
    * **Hubs** - selecteer de hub die u wilt koppelen aan deze verbinding.
    * **Abonnement** - controleer of het abonnement klopt.
    * **Virtueel netwerk** - selecteer het virtuele netwerk dat met deze hub wilt verbinden. Het virtuele netwerk mag geen bestaande virtuele netwerkgateway hebben.

## <a name="device"></a>7. VPN-profiel downloaden

Gebruik het VPN-profiel om uw clients te configureren.

1. Klik op de pagina voor uw virtuele WAN op **Hubs**.
2. Selecteer de hub waarvoor u het profiel wilt downloaden.
3. Klik op **"..."** en kies **Profiel downloaden**. 
4. Wanneer het bestand gereed is, klikt u op de koppeling om het te downloaden.
4. Gebruik het profielbestand om de punt-naar-site-clients te configureren.

## <a name="device"></a>8. Punt-naar-site-clients configureren
Gebruik het gedownloade profiel om de clients voor externe toegang te configureren. De procedure verschilt per besturingssysteem. Volg daarom de juiste instructies hieronder:

### <a name="microsoft-windows"></a>Microsoft Windows
#### <a name="openvpn"></a>OpenVPN

1.  Download en installeer de OpenVPN-client op de officiële website.
2.  Download het VPN-profiel voor de gateway. Dit kan via het tabblad Punt-naar-site-configuraties in Azure Portal of via New-AzureRmVpnClientConfiguration in PowerShell.
3.  Pak het profiel uit. Open het configuratiebestand vpnconfig.ovpn uit de map OpenVPN in Kladblok.
4.  Vul het gedeelte P2S client certificate met de openbare P2S-clientcertificatcode in base64. In een certificaat met PEM-indeling kunt u gewoon het .cer-bestand openen en de base64-code tussen de headers van het certificaat kopiëren. Hier ziet u hoe u een certificaat kunt exporteren om de gecodeerde openbare sleutel te verkrijgen.
5.  Vul in het gedeelte voor de persoonlijke sleutel de persoonlijke P2S-clientcertificaatsleutel in Base64 in. Bekijk hier hoe u de persoonlijke sleutel kunt uitpakken.
6.  Wijzig geen andere velden. Gebruik de ingevulde configuratie in de clientinvoer om verbinding te maken met de VPN.
7.  Kopieer het bestand vpnconfig.ovpn naar C:\Program Files\OpenVPN\config folder.
8.  Klik met de rechtermuisknop op het pictogram van OpenVPN in het systeemvak en klik op Verbinding maken.

#### <a name="ikev2"></a>IKEv2

1. Selecteer de VPN-clientconfiguratiebestanden die overeenkomen met de architectuur van de Windows-computer. Kies voor een architectuur met 64-bits processor het installatiepakket ‘VpnClientSetupAmd64’. Kies voor een architectuur met 32-bits processor het installatiepakket ‘VpnClientSetupAmdX86’.
2. Dubbelklik op het pakket om het te installeren. Klik op Meer info en vervolgens op Toch uitvoeren als u een SmartScreen-melding ziet.
3. Navigeer op de clientcomputer naar Netwerkinstellingen en klik op VPN. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt.
4. Controleer of u een clientcertificaat op de clientcomputer hebt geïnstalleerd voordat u verbinding probeert te maken. Een clientcertificaat is vereist voor verificatie als u het systeemeigen verificatietype met Azure-certificaat gebruikt. Raadpleeg Certificaten genereren voor meer informatie over het genereren van certificaten. Raadpleeg Een clientcertificaat installeren voor meer informatie over het installeren van een clientcertificaat.

### <a name="mac-os-x"></a>Mac (OS X)
#### <a name="openvpn"></a>OpenVPN

1.  Download en installeer een OpenVPN-client, zoals TunnelBlik via https://tunnelblick.net/downloads.html 
2.  Download het VPN-profiel voor de gateway. Dit kan via het tabblad Punt-naar-site-configuratie in Azure Portal of via New-AzureRmVpnClientConfiguration in PowerShell.
3.  Pak het profiel uit. Open het configuratiebestand vpnconfig.ovpn uit de map OpenVPN in Kladblok.
4.  Vul het gedeelte P2S client certificate met de openbare P2S-clientcertificatcode in base64. In een certificaat met PEM-indeling kunt u gewoon het .cer-bestand openen en de base64-code tussen de headers van het certificaat kopiëren. Hier ziet u hoe u een certificaat kunt exporteren om de gecodeerde openbare sleutel te verkrijgen.
5.  Vul in het gedeelte voor de persoonlijke sleutel de persoonlijke P2S-clientcertificaatsleutel in Base64 in. Bekijk hier hoe u de persoonlijke sleutel kunt uitpakken.
6.  Wijzig geen andere velden. Gebruik de ingevulde configuratie in de clientinvoer om verbinding te maken met de VPN.
7.  Dubbelklik op het profielbestand om het profiel te maken in TunnelBlik
8.  Open Tunnelblik via de toepassingsmap
9.  Klik op het pictogram van Tunnelblik in het systeemvak en kies Verbinding maken

#### <a name="ikev2"></a>IKEv2

Azure biedt geen mobileconfig-bestand voor verificatie van systeemeigen Azure-certificaten. U moet de systeemeigen IKEv2 VPN-client handmatig configureren op elke Mac die verbinding met Azure zal maken. De map Generic bevat alle informatie die u nodig hebt voor de configuratie. Als u de map Generic niet ziet in uw download, is IKEv2 waarschijnlijk niet geselecteerd als tunneltype. Genereer het zip-bestand opnieuw om de map Generic te verkrijgen nadat IKEv2 is geselecteerd. De map Generic bevat de volgende bestanden:

- VpnSettings.xml, dat belangrijke instellingen bevat, zoals het serveradres en tunneltype.
- VpnServerRoot.cer, dat het basiscertificaat bevat dat vereist is om de Azure VPN-gateway te valideren tijdens het configureren van de P2S-verbinding.

## <a name="viewwan"></a>9. Uw virtuele WAN weergeven

1. Navigeer naar uw virtuele WAN.
2. Op de pagina Overzicht vertegenwoordigt elk punt op de kaart een hub. Houd de muisaanwijzer boven een punt om de statussamenvatting voor de hub weer te geven.
3. In de sectie Hubs en verbindingen vindt u informatie over de hubstatus, site, regio, VPN-verbindingsstatus en verzonden en ontvangen bytes.

## <a name="viewhealth"></a>10. Resourcestatus bekijken

1. Navigeer naar uw WAN.
2. Klik op de pagina van uw WAN, in de sectie **Ondersteuning en probleemoplossing**, op **Status** en bekijk de status van uw resource.

## <a name="connectmon"></a>11. Een verbinding bewaken

Maak een verbinding om de communicatie tussen een Azure-VM en een externe site te bewaken. Zie voor meer informatie over het instellen van een verbindingscontrole de pagina [Netwerkcommunicatie bewaken](~/articles/network-watcher/connection-monitor.md). Het bronveld is het IP-adres van de VM in Azure en het doel-IP-adres is het IP-adres voor de site.

## <a name="cleanup"></a>12. Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, wanneer u deze niet meer nodig hebt. Vervangen 'myResourceGroup' door de naam van uw resourcegroep en voer de volgende PowerShell-opdracht uit:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een WAN maken
> * Een site maken
> * Een hub maken
> * Een hub verbinden met een site
> * Een VNet verbinden met een hub
> * Een configuratie voor een VPN-apparaat downloaden en toepassen
> * Uw virtuele WAN weergeven
> * Resourcestatus weergeven
> * Een verbinding bewaken

Zie voor meer informatie over Virtual WAN de pagina [Overzicht van Virtual WAN](virtual-wan-about.md).
