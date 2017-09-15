---
title: 'Via een computer verbinding maken met een virtueel netwerk met behulp van een punt-naar-site-verbinding en verificatie via een certificaat: Azure Portal | Microsoft Docs'
description: Maakt een beveiligde verbinding met een computer via Azure Virtual Network door een punt-naar-site-VPN-gatewayverbinding te maken met behulp van verificatie via een certificaat. Dit artikel is van toepassing op het Resource Manager-implementatiemodel en maakt gebruik van Azure Portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: cc9018d95ffce3b5b4a5ee20d5c78a2122e0223e
ms.contentlocale: nl-nl
ms.lasthandoff: 08/31/2017

---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-certificate-authentication-azure-portal"></a>Een punt-naar-site-verbinding naar een VNet configureren met behulp van verificatie via een certificaat: Azure Portal

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het Resource Manager-implementatiemodel met behulp van Azure Portal. Deze configuratie maakt gebruik van certificaten om de client te verifiëren waarmee verbinding wordt gemaakt. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Met een punt-naar-site-VPN-gateway (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele netwerk. P2S-verbindingen zijn nuttig als u verbinding wilt maken met uw VNet vanaf een externe locatie, bijvoorbeeld als u ook thuis werkt of op een congres verbinding wilt maken. Een P2S-VPN is ook een uitstekende oplossing in plaats van een site-naar-site-VPN wanneer u maar een paar clients hebt die verbinding moeten maken met een VNet. 

P2S maakt gebruik van SSTP (Secure Socket Tunneling Protocol), wat een op SSL gebaseerd VPN-protocol is. Een P2S-verbinding wordt tot stand gebracht door deze te starten vanaf de clientcomputer.

![Punt-naar-site-diagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Voor punt-naar-site-verbindingen met verificatie via een certificaat is het volgende vereist:

* Een RouteBased VPN-gateway.
* De openbare sleutel (CER-bestand) voor een basiscertificaat dat is geüpload naar Azure. Nadat het certificaat is geüpload, wordt het beschouwd als een vertrouwd certificaat en wordt het gebruikt voor verificatie.
* Een clientcertificaat dat wordt gegenereerd op basis van het basiscertificaat en dat wordt geïnstalleerd op elke clientcomputer die verbinding gaat maken met het VNet. Dit certificaat wordt gebruikt voor clientverificatie.
* Een configuratiepakket voor de VPN-client. Het configuratiepakket voor de VPN-client bevat de gegevens die nodig zijn om de client te verbinden met het VNet. Het pakket configureert de bestaande VPN-client die is ingebouwd in het Windows-besturingssysteem. Elke client die verbinding maakt, moet worden geconfigureerd met behulp van het configuratiepakket.

Punt-naar-site-verbindingen hebben geen VPN-apparaat of on-premises openbaar IP-adres nodig. De VPN-verbinding is gemaakt via SSTP (Secure Socket Tunneling Protocol). Wij ondersteunen SSTP versies 1.0, 1.1 en 1.2 aan de serverzijde. De client besluit welke versie moet worden gebruikt. Voor Windows 8.1 en hoger, gebruikt SSTP standaard 1.2.

Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.

#### <a name="example"></a>Voorbeeldwaarden

U kunt de volgende waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel:

* **VNet-naam:** VNet1
* **Adresruimte:** 192.168.0.0/16<br>In dit voorbeeld gebruiken we slechts één adresruimte. U kunt meer dan één adresruimte voor uw VNet hebben.
* **Subnetnaam:** FrontEnd
* **Subnetadresbereik:** 192.168.1.0/24
* **Abonnement:** controleer of u het juiste abonnement hebt in het geval u er meer dan één hebt.
* **Resourcegroep:** TestRG
* **Locatie:** VS - oost
* **GatewaySubnet:** 192.168.200.0/24<br>
* **DNS-server:** (optioneel) IP-adres van de DNS-server die u wilt gebruiken voor naamomzetting.
* **Gatewaynaam van het virtuele netwerk:** VNet1GW
* **Gatewaytype:** VPN
* **VPN-type:** op route gebaseerd
* **Openbare IP-adresnaam:** VNet1GWpip
* **Verbindingstype:** punt-naar-site-verbinding
* **Clientadresgroep:** 172.16.201.0/24<br>VPN-clients die verbinding maken met het VNet via deze punt-naar-site-verbinding, ontvangen een IP-adres van de clientadresgroep.

## <a name="createvnet"></a>1. Een virtueel netwerk maken

Controleer eerst of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Een gatewaysubnet toevoegen

Voordat u het virtuele netwerk verbindt met een gateway, moet u eerst het gatewaysubnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. De gatewayservices gebruiken de IP-adressen opgegeven in het gatewaysubnet. Maak indien mogelijk een gatewaysubnet met een CIDR-blok van /28 of /27 zodat er voldoende IP-adressen zijn om aan toekomstige aanvullende configuratievereisten te voldoen.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. Een DNS-server opgeven (optioneel)

Nadat u uw virtuele netwerk hebt gemaakt, kunt u het IP-adres van een DNS-server toevoegen om de naamomzetting te verwerken. De DNS-server is optioneel voor deze configuratie, maar vereist als u naamomzetting nodig hebt. Het opgeven van een waarde betekent niet dat er een nieuwe DNS-server wordt gemaakt. Het IP-adres van de DNS-server dat u opgeeft, moet het adres zijn van een DNS-server zijn die de namen kan omzetten voor de resources waarmee u verbinding maakt. Voor dit voorbeeld hebben we een privé-IP-adres gebruikt, maar het is zeer onwaarschijnlijk dat dit het IP-adres van uw DNS-server is. Zorg ervoor dat u uw eigen waarden gebruikt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. De gateway van een virtueel netwerk maken

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Certificaten genereren

Certificaten worden in Azure gebruikt om clients te verifiëren die verbinding willen maken met een VNet met behulp van een punt-naar-site-VPN-verbinding. Als u beschikt over het basiscertificaat, [uploadt](#uploadfile) u de gegevens van de openbare sleutel naar Azure. Het basiscertificaat wordt vervolgens als 'vertrouwd' beschouwd door Azure voor verbinding met het virtuele netwerk via P2S. U kunt ook clientcertificaten genereren op basis van het vertrouwde basiscertificaat en deze vervolgens op elke clientcomputer installeren. Het clientcertificaat wordt gebruikt om de client te verifiëren bij het maken van verbinding met het VNet. 

### <a name="getcer"></a>1. Het CER-bestand voor het basiscertificaat verkrijgen

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Een clientcertificaat genereren

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. De clientadrespool toevoegen

De clientadrespool bestaat uit een privé-IP-adresbereik dat u opgeeft. De clients die verbinding maken via een punt-naar-site-VPN-verbinding krijgen een IP-adres uit dit bereik. Gebruik een privé-IP-adresbereik dat niet overlapt met de on-premises locatie waarvanaf u verbinding maakt of met het VNet waarmee u verbinding wilt maken.

1. Nadat de virtuele netwerkgateway is gemaakt, gaat u naar de sectie **Instellingen** van de pagina van de virtuele netwerkgateway. Klik in de sectie **Instellingen** op **Punt-naar-site-configuratie** om de gelijknamige**** pagina te openen.

  ![De pagina Punt-naar-site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. Op de pagina **Punt-naar-site-configuratie** kunt u het automatisch ingevulde bereik verwijderen en daarna het bereik met privé-IP-adressen toevoegen dat u wilt gebruiken. Klik op **Opslaan** om de instelling te valideren en op te slaan.

  ![Clientadresgroep](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Gegevens van openbare sleutel van basiscertificaat uploaden

Nadat de gateway is gemaakt, uploadt u de gegevens van de openbare sleutel voor het basiscertificaat naar Azure. Als het uploaden is voltooid, kan Azure daarmee clients met een geïnstalleerd clientcertificaat (gemaakt op basis van het vertrouwde basiscertificaat) verifiëren. U kunt aanvullende vertrouwde basiscertificaatbestanden uploaden (maximaal 20).

1. Certificaten worden toegevoegd aan de pagina **Punt-naar-site-configuratie**, in de sectie **Basiscertificaat**.  
2. Zorg ervoor dat u het basiscertificaat hebt geëxporteerd als een Base-64 encoded X.509 (.cer)-bestand. U moet het certificaat in deze indeling exporteren, zodat u het certificaat met een teksteditor kunt openen.
3. Open het certificaat met een teksteditor zoals Kladblok. Zorg er bij het kopiëren van de certificaatgegevens voor dat u de tekst als één ononderbroken regel kopieert zonder regelterugloop of regelinvoer. Mogelijk moet u de weergave in de teksteditor wijzigen naar Symbool weergeven/Alle tekens weergeven om de regelterugloop en regelinvoer te zien. Kopieer alleen het volgende gedeelte als één ononderbroken regel:

  ![Certificaatgegevens](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Plak de certificaatgegevens in het veld **Gegevens van openbaar certificaat**. Geef een **naam** aan het certificaat en klik vervolgens op **Opslaan**. U kunt maximaal 20 vertrouwde basiscertificaten toevoegen.

  ![Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>8. Het configuratiepakket voor VPN-clients genereren en installeren

Als u via een punt-naar-site-VPN verbinding wilt maken met een VNet, moet op elke client een clientconfiguratiepakket worden geïnstalleerd waarmee de ingebouwde VPN-client wordt geconfigureerd met de instellingen en bestanden die nodig zijn om verbinding te maken met het virtuele netwerk. Het configuratiepakket voor VPN-clients configureert de ingebouwde Windows VPN-client. Er wordt geen nieuwe of andere VPN-client geïnstalleerd.

U kunt hetzelfde configuratiepakket voor de VPN-client gebruiken op elke clientcomputer, mits de versie overeenkomt met de architectuur van de client. Zie voor de lijst met ondersteunde clientbesturingssystemen de [Veelgestelde vragen over punt-naar-site-verbindingen](#faq) onderaan dit artikel.

### <a name="1-generate-and-download-the-client-configuration-package"></a>1. Het configuratiepakket voor de client genereren en downloaden

1. Klik op de pagina **Punt-naar-site-configuratie** op **VPN-client downloaden** om de pagina **VPN-client downloaden** te openen. Het duurt enkele minuten om het pakket te genereren.

  ![VPN-client downloaden 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Selecteer het juiste pakket voor de client en klik vervolgens op **Downloaden**. Sla het configuratiepakket op. U kunt het configuratiepakket voor de VPN-client installeren op elke clientcomputer die verbinding maakt met het virtuele netwerk.

  ![VPN-client downloaden 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="2-install-the-client-configuration-package"></a>2. Het configuratiepakket voor de client installeren

1. Kopieer het configuratiebestand lokaal op de computer die u wilt verbinden met het virtuele netwerk. 
2. Dubbelklik op het EXE-bestand om het pakket te installeren op de clientcomputer. Omdat u het configuratiepakket hebt gemaakt, is het niet ondertekend en ziet u mogelijk een waarschuwing. Als u een Windows SmartScreen-melding ontvangt, klikt u op **Meer info** (links) en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
3. Installeer het pakket op de clientcomputer. Als u een Windows SmartScreen-melding ontvangt, klikt u op **Meer info** (links) en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
4. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt.

## <a name="installclientcert"></a>9. Een geëxporteerd clientcertificaat installeren

Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat. Dit is meestal alleen een kwestie van dubbelklikken op het certificaat en het installeren.

Zorg ervoor dat het certificaat is geëxporteerd als een PFX-bestand, samen met de volledige certificaatketen (dit is de standaardinstelling). Anders zijn de gegevens van het basiscertificaat niet aanwezig op de clientcomputer en kan de client niet worden geverifieerd. Zie [Een geëxporteerd clientcertificaat installeren](vpn-gateway-certificates-point-to-site.md#install) voor meer informatie.

## <a name="connect"></a>10. Verbinding maken met Azure

1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik op **Doorgaan** om verhoogde bevoegdheden te gebruiken.

2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.

  ![VPN-client maakt verbinding met Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. De verbinding is tot stand gebracht.

  ![Verbinding tot stand gebracht](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Problemen met P2S-verbindingen oplossen

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>11. De verbinding controleren

1. Als u wilt controleren of uw VPN-verbinding actief is, opent u een opdrachtprompt met verhoogde bevoegdheid en voert u *ipconfig/all* in.
2. Bekijk de resultaten. Het IP-adres dat u hebt ontvangen is een van de adressen binnen het adresbereik van de punt-naar-site-VPN-clientadresgroep die u hebt opgegeven in uw configuratie. De resultaten zijn vergelijkbaar met het volgende voorbeeld:

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Verbinding maken met een virtuele machine

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Vertrouwde basiscertificaten toevoegen of verwijderen

U kunt vertrouwde basiscertificaat toevoegen in en verwijderen uit Azure. Wanneer u een basiscertificaat verwijdert, kunnen clients met een certificaat dat is gegenereerd op basis van dat basiscertificaat niet worden geverifieerd, en kunnen ze dus geen verbinding maken. Als u wilt dat clients kunnen worden geverifieerd en verbinding kunnen maken, moet u een nieuw clientcertificaat installeren dat is gegenereerd op basis van een basiscertificaat dat wordt vertrouwd (is geüpload) in Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Zie voor instructies de sectie [Een vertrouwd basiscertificaat uploaden](#uploadfile) in dit artikel.

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat verwijderen

1. Als u een vertrouwd basiscertificaat wilt verwijderen, gaat u naar de pagina **Punt-naar-site-configuratie** voor uw virtuele netwerkgateway.
2. Zoek in de sectie **Basiscertificaat** van de pagina het certificaat dat u wilt verwijderen.
3. Klik op het weglatingsteken naast het certificaat en vervolgens op Verwijderen.

## <a name="revokeclient"></a>Een clientcertificaat intrekken

U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Dit is anders van het verwijderen van een vertrouwd basiscertificaat. Als u een vertrouwd .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u in plaats van een basiscertificaat een clientcertificaat intrekt, kunt u de certificaten die zijn gegenereerd op basis van het basiscertificaat, blijven gebruiken voor verificatie.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="to-revoke-a-client-certificate"></a>Een clientcertificaat intrekken

U kunt een clientcertificaat intrekken door de vingerafdruk toe te voegen aan de intrekkingslijst.

1. Haal de vingerafdruk voor het clientcertificaat op. Zie voor meer informatie [De vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieer de gegevens naar een teksteditor en verwijder alle spaties, zodat u een doorlopende tekenreeks overhoudt.
3. Navigeer naar de pagina **Punt-naar-site-configuratie** van de virtuele netwerkgateway. Dit is de pagina die u ook hebt gebruikt voor het [uploaden van een vertrouwd basiscertificaat](#uploadfile).
4. In het gedeelte **Ingetrokken certificaten** voert u een beschrijvende naam in voor het certificaat (dit hoeft niet de CN van het certificaat te zijn).
5. Kopieer en plak de vingerafdruk naar het veld **Vingerafdruk**.
6. De vingerafdruk wordt gevalideerd en automatisch toegevoegd aan de intrekkingslijst. Er wordt een bericht op het scherm weergegeven met de melding dat de lijst wordt bijgewerkt. 
7. Wanneer het bijwerken is voltooid, kan het certificaat niet langer worden gebruikt om verbinding te maken. Clients die verbinding proberen te maken met het certificaat, ontvangen een bericht waarin wordt gemeld dat het certificaat niet meer geldig is.

## <a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-point-to-site-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/linux/azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.
