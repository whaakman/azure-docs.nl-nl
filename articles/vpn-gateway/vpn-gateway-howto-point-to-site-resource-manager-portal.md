---
title: 'Met een computer verbinding maken met een virtueel Azure-netwerk met behulp van punt-naar-site: Portal | Microsoft Docs'
description: U maakt een beveiligde verbinding met uw Azure Virtual Network door een punt-naar-site-VPN-gatewayverbinding te maken met behulp van Resource Manager en Azure Portal.
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
ms.date: 05/03/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: a8c815326c255833fc7944821199d80e26b735ee
ms.contentlocale: nl-nl
ms.lasthandoff: 05/05/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Een punt-naar-site-verbinding met een VNet configureren met Azure Portal

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het Resource Manager-implementatiemodel met behulp van Azure Portal. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Punt-naar-site-verbindingen zijn handig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk. Voor P2S-verbindingen hebt u geen VPN-apparaat of een openbaar IP-adres nodig. U brengt de VPN-verbinding tot stand vanaf de clientcomputer. Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.

![Punt-naar-site-diagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

### <a name="example"></a>Voorbeeldwaarden

U kunt de volgende waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel:

* **Naam: VNet1**
* **Adresruimte: 192.168.0.0/16**<br>In dit voorbeeld gebruiken we slechts één adresruimte. U kunt meer dan één adresruimte voor uw VNet hebben.
* **Subnetnaam: FrontEnd**
* **Subnetadresbereik: 192.168.1.0/24**
* **Abonnement:** controleer of u het juiste abonnement hebt in het geval u er meer dan één hebt.
* **Resourcegroep: TestRG**
* **Locatie: VS - oost**
* **GatewaySubnet: 192.168.200.0/24**
* **Gatewaynaam van het virtuele netwerk: VNet1GW**
* **Gatewaytype: VPN**
* **VPN-type: op route gebaseerd**
* **Openbaar IP-adres: VNet1GWpip**
* **Verbindingstype: punt-naar-site-verbinding**
* **Clientadresgroep: 172.16.201.0/24**<br>VPN-clients die verbinding maken met het VNet via deze punt-naar-site-verbinding, ontvangen een IP-adres van de clientadresgroep.

## <a name="createvnet"></a>1: Een virtueel netwerk maken

Controleer eerst of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial). Als u deze configuratie bij wijze van oefening maakt, kunt u de [voorbeeldwaarden](#example) gebruiken.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>2: Adresruimte en subnetten opgeven

Wanneer het VNet is gemaakt, kunt u er extra adresruimte en subnetten aan toevoegen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3: Een gatewaysubnet toevoegen

Voordat u het virtuele netwerk verbindt met een gateway, moet u eerst het gatewaysubnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. De gatewayservices gebruiken de IP-adressen opgegeven in het gatewaysubnet. Maak indien mogelijk een gatewaysubnet met een CIDR-blok van /28 of /27 zodat er voldoende IP-adressen zijn om aan toekomstige aanvullende configuratievereisten te voldoen.

De schermafbeeldingen in deze sectie worden gegeven als voorbeeld. Zorg ervoor dat u het adresbereik GatewaySubnet gebruikt dat overeenkomt met de vereiste waarden voor uw configuratie.

### <a name="to-create-a-gateway-subnet"></a>Een gatewaysubnet maken

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4: Een DNS-server opgeven (optioneel)

Nadat u uw virtuele netwerk hebt gemaakt, kunt u het IP-adres van een DNS-server toevoegen om de naamomzetting te verwerken. De opgegeven DNS-server moet een DNS-server zijn die de namen kan omzetten voor de resources waarmee u verbinding maakt. Het VPN-clientconfiguratiepakket dat u in een latere fase maakt, bevat de IP-adressen van de DNS-servers die u in deze instelling opgeeft. Als u de lijst met DNS-servers in de toekomst moet bijwerken, kunt u nieuwe VPN-clientconfiguratiepakketten genereren die overeenkomen met de nieuwe lijst.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>5: De gateway van een virtueel netwerk maken

Punt-naar-site-verbindingen vereisen de volgende instellingen:

* Gatewaytype: VPN
* VPN-type: op route gebaseerd

### <a name="to-create-a-virtual-network-gateway"></a>De gateway van een virtueel netwerk maken

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>6: Certificaten genereren

Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren.

### <a name="getcer"></a>Stap 1: Het CER-bestand voor het basiscertificaat verkrijgen

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>Stap2: Een clientcertificaat genereren

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a> 7: De clientadresgroep toevoegen

1. Nadat de virtuele netwerkgateway is gemaakt, gaat u naar de sectie **Instellingen** van de blade van de virtuele netwerkgateway. Klik in de sectie **Instellingen** op **Punt-naar-site-configuratie** om de blade **Configuratie** te openen.

  ![Blade Punt-naar-site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Adresgroep** is de groep met IP-adressen van waaruit clients die verbinding maken, een IP-adres ontvangen. Voeg de adresgroep toe en klik vervolgens op **Opslaan**.

  ![Clientadresgroep](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>8: Het CER-bestand van het basiscertificaat uploaden

Nadat de gateway is gemaakt, kunt u het .cer-bestand voor een vertrouwd basiscertificaat uploaden naar Azure. U kunt bestanden voor maximaal twintig basiscertificaten uploaden. U uploadt de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het .cer-bestand is geüpload, gebruikt Azure dit voor het verifiëren van clients die verbinding maken met het virtuele netwerk.

1. Certificaten worden toegevoegd op de blade **punt-naar-site-configuratie** in de sectie **Basiscertificaat**.  
2. Zorg ervoor dat u het basiscertificaat hebt geëxporteerd als een Base-64 encoded X.509 (.cer)-bestand. U moet het certificaat in deze indeling exporteren, zodat u het certificaat met een teksteditor kunt openen.
3. Open het certificaat met een teksteditor zoals Kladblok. Zorg er bij het kopiëren van de certificaatgegevens voor dat u de tekst als één ononderbroken regel kopieert zonder regelterugloop of regelinvoer. Mogelijk moet u de weergave in de teksteditor wijzigen naar Symbool weergeven/Alle tekens weergeven om de regelterugloop en regelinvoer te zien. Kopieer alleen het volgende gedeelte als één ononderbroken regel:

  ![Certificaatgegevens](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Plak de certificaatgegevens in het veld **Gegevens van openbaar certificaat**. Geef een **naam** aan het certificaat en klik vervolgens op **Opslaan**. U kunt maximaal 20 vertrouwde basiscertificaten toevoegen.

  ![Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>9: Het configuratiepakket voor de VPN-client installeren

Als u verbinding wilt maken met een VNet met behulp van een punt-naar-site-VPN, moet elke client een configuratiepakket voor de VPN-client installeren. Met het pakket wordt geen VPN-client geïnstalleerd. U kunt hetzelfde configuratiepakket voor de VPN-client gebruiken op elke clientcomputer, mits de versie overeenkomt met de architectuur van de client. Zie voor de lijst met ondersteunde clientbesturingssystemen de [Veelgestelde vragen over punt-naar-site-verbindingen](#faq) onderaan dit artikel.

Het configuratiepakket configureert de systeemeigen Windows VPN-client met de instellingen die nodig zijn om verbinding te maken met het virtuele netwerk, en als u een DNS-server voor uw VNet hebt opgegeven, bevat het pakket het IP-adres van de DNS-server die de client gaat gebruiken voor naamomzetting. Als u de opgegeven DNS-server later wijzigt, na het genereren van het clientconfiguratiepakket, moet u een nieuw clientconfiguratiepakket genereren om op uw clientcomputers te installeren.

### <a name="step-1---download-the-client-configuration-package"></a>Stap 1: Download het configuratiepakket voor de VPN-client

1. Klik op de blade **Punt-naar-site-configuratie** op **VPN-client downloaden** om de blade **VPN-client downloaden** te openen. Het duurt enkele minuten om het pakket te genereren.

  ![VPN-client downloaden 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Selecteer het juiste pakket voor de client en klik vervolgens op **Downloaden**. Sla het configuratiepakket op. U kunt het configuratiepakket voor de VPN-client installeren op elke clientcomputer die verbinding maakt met het virtuele netwerk.

  ![VPN-client downloaden 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-the-client-configuration-package"></a>Stap 2: Het configuratiepakket voor de client installeren

1. Kopieer het configuratiebestand lokaal op de computer die u wilt verbinden met het virtuele netwerk. 
2. Dubbelklik op het EXE-bestand om het pakket te installeren op de clientcomputer. Omdat u het configuratiepakket hebt gemaakt, is het niet ondertekend en ziet u mogelijk een waarschuwing. Als u een Windows SmartScreen-melding ontvangt, klikt u op **Meer info** (links) en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
3. Installeer het pakket op de clientcomputer. Als u een Windows SmartScreen-melding ontvangt, klikt u op **Meer info** (links) en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
4. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt.

## <a name="installclientcert"></a>10: Het clientcertificaat installeren

Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat. Dit is meestal alleen een kwestie van dubbelklikken op het certificaat en het installeren. Zie [Een geëxporteerd clientcertificaat installeren](vpn-gateway-certificates-point-to-site.md#install) voor meer informatie.

## <a name="connect"></a>11: Verbinding maken met Azure

1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik op **Doorgaan** om verhoogde bevoegdheden te gebruiken.

2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.

  ![VPN-client maakt verbinding met Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. De verbinding is tot stand gebracht.

  ![Verbinding tot stand gebracht](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

Als u problemen hebt met de verbinding, controleer dan het volgende:

- Open **Gebruikerscertificaten beheren** en navigeer naar **Vertrouwde basiscertificeringsinstanties\Certificaten**. Controleer of het basiscertificaat wordt weergegeven. Verificatie werkt alleen als het basiscertificaat aanwezig is. Wanneer u een PFX-bestand van het clientcertificaat exporteert met de standaardwaarde 'Indien mogelijk alle certificaten in het certificeringspad opnemen', wordt ook de basiscertificaatinformatie geëxporteerd. Wanneer u het clientcertificaat installeert, wordt vervolgens ook het basiscertificaat geïnstalleerd op de clientcomputer. 

- Als u een certificaat gebruikt dat is verleend met behulp van een oplossing voor een CA voor ondernemingen en problemen ondervindt bij de verificatie, controleert u de verificatievolgorde op het clientcertificaat. U kunt de volgorde van de verificatielijst controleren door op het clientcertificaat te dubbelklikken en naar **Details > Uitgebreid sleutelgebruik** te gaan. Controleer of Clientverificatie als het eerste item in de lijst wordt weergegeven. Als dat niet het geval is, moet u een clientcertificaat op basis van de gebruikerssjabloon verlenen waarin clientverificatie als het eerste item in de lijst wordt weergegeven.


## <a name="verify"></a>12: De verbinding controleren

1. Als u wilt controleren of uw VPN-verbinding actief is, opent u een opdrachtprompt met verhoogde bevoegdheid en voert u *ipconfig/all* in.
2. Bekijk de resultaten. Het IP-adres dat u hebt ontvangen is een van de adressen binnen het adresbereik van de punt-naar-site-VPN-clientadresgroep die u hebt opgegeven in uw configuratie. De resultaten zijn vergelijkbaar met het volgende voorbeeld:
   
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


Als u geen verbinding met een virtuele machine kunt maken via P2S, gebruikt u 'ipconfig' om het IPv4-adres te controleren dat is toegewezen aan de ethernetadapter op de computer waarvanaf u de verbinding maakt. Als het IP-adres zich binnen het adresbereik bevindt van het VNet waarmee u verbinding maakt of binnen het adresbereik van uw VPNClientAddressPool, wordt dit een overlappende adresruimte genoemd. Als uw adresruimte op deze manier overlapt, kan het netwerkverkeer Azure niet bereiken en blijft het in het lokale netwerk. Als uw netwerkadresruimte niet overlapt en u nog steeds geen verbinding kunt maken met de virtuele machine, raadpleegt u [Troubleshoot Remote Desktop connections to a VM](../virtual-machines/windows/troubleshoot-rdp-connection.md) (Problemen met extern-bureaubladverbindingen met virtuele machines oplossen).

## <a name="connectVM"></a>Verbinding maken met een virtuele machine

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Vertrouwde basiscertificaten toevoegen of verwijderen

U kunt vertrouwde basiscertificaat toevoegen in en verwijderen uit Azure. Wanneer u een basiscertificaat verwijdert, kunnen clients met een certificaat dat is gegenereerd op basis van dat basiscertificaat niet worden geverifieerd, en kunnen ze dus geen verbinding maken. Als u wilt dat clients kunnen worden geverifieerd en verbinding kunnen maken, moet u een nieuw clientcertificaat installeren dat is gegenereerd op basis van een basiscertificaat dat wordt vertrouwd (is geüpload) in Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Zie voor instructies de sectie [Een vertrouwd basiscertificaat uploaden](#uploadfile) in dit artikel.

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat verwijderen

1. Als u een vertrouwd basiscertificaat wilt verwijderen, gaat u naar de blade **Punt-naar-site-configuratie** voor uw virtuele netwerkgateway.
2. In het gedeelte **Basiscertificaat** van de blade, zoekt u het certificaat dat u wilt verwijderen.
3. Klik op het weglatingsteken naast het certificaat en vervolgens op Verwijderen.

## <a name="revokeclient"></a>Een clientcertificaat intrekken

U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Dit wijkt af van het verwijderen van een vertrouwd basiscertificaat. Als u een vertrouwd .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u in plaats van een basiscertificaat een clientcertificaat intrekt, kunt u de certificaten die zijn gegenereerd op basis van het basiscertificaat, blijven gebruiken voor verificatie.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="to-revoke-a-client-certificate"></a>Een clientcertificaat intrekken

U kunt een clientcertificaat intrekken door de vingerafdruk toe te voegen aan de intrekkingslijst.

1. Haal de vingerafdruk voor het clientcertificaat op. Zie voor meer informatie [De vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieer de gegevens naar een teksteditor en verwijder alle spaties, zodat u een doorlopende tekenreeks overhoudt.
3. Navigeer naar de blade **Punt-naar-site-configuratie** van de virtuele netwerkgateway. Dit is dezelfde blade die u hebt gebruikt voor het [uploaden van een vertrouwd basiscertificaat](#uploadfile).
4. In het gedeelte **Ingetrokken certificaten** voert u een beschrijvende naam in voor het certificaat (dit hoeft niet de CN van het certificaat te zijn).
5. Kopieer en plak de vingerafdruk naar het veld **Vingerafdruk**.
6. De vingerafdruk wordt gevalideerd en automatisch toegevoegd aan de intrekkingslijst. Er wordt een bericht op het scherm weergegeven met de melding dat de lijst wordt bijgewerkt. 
7. Wanneer het bijwerken is voltooid, kan het certificaat niet langer worden gebruikt om verbinding te maken. Clients die verbinding proberen te maken met het certificaat, ontvangen een bericht waarin wordt gemeld dat het certificaat niet meer geldig is.

## <a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/linux/azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.
