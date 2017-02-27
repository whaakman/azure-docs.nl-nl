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
ms.date: 02/17/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cf72197aba2c6e6c7a51f96d1161cf1fbe88a0c5
ms.openlocfilehash: 61ef9523739ebd8bd105c6faccf42405c7c62567


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Een punt-naar-site-verbinding met een VNet configureren met Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. Een P2S-verbinding is nuttig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk. 

Voor punt-naar-site-verbindingen hebt u geen VPN-apparaat of openbaar IP-adres nodig. Er wordt een VPN-verbinding tot stand gebracht door de verbinding te starten vanaf de clientcomputer. Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt met behulp van Azure Portal. De stappen zijn van toepassing op het Resource Manager-implementatiemodel.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en -methoden voor P2S-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Basiswerkstroom
![Punt-naar-site-diagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Voorbeeldwaarden
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

## <a name="before-beginning"></a>Voordat u begint
* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).

## <a name="a-namecreatevnetapart-1---create-a-virtual-network"></a><a name="createvnet"></a>Deel 1: Een virtueel netwerk maken
Als u deze configuratie bij wijze van oefening maakt, kunt u de [voorbeeldwaarden](#example) gebruiken.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="a-nameaddressapart-2---specify-address-space-and-subnets"></a><a name="address"></a>Deel 2: Adresruimte en subnetten opgeven
Wanneer het VNet is gemaakt, kunt u er extra adresruimte en subnetten aan toevoegen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namegatewaysubnetapart-3---add-a-gateway-subnet"></a><a name="gatewaysubnet"></a>Deel 3: Een gatewaysubnet toevoegen

Voordat u het virtuele netwerk verbindt met een gateway, moet u eerst het gatewaysubnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. De gatewayservices gebruiken de IP-adressen opgegeven in het gatewaysubnet. Maak indien mogelijk een gatewaysubnet met een CIDR-blok van /28 of /27 zodat er voldoende IP-adressen zijn om aan toekomstige aanvullende configuratievereisten te voldoen.

De schermafbeeldingen in deze sectie worden gegeven als voorbeeld. Zorg ervoor dat u het adresbereik GatewaySubnet gebruikt dat overeenkomt met de vereiste waarden voor uw configuratie.

###<a name="to-create-a-gateway-subnet"></a>Een gatewaysubnet maken

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namednsapart-4---specify-a-dns-server-optional"></a><a name="dns"></a>Deel 4: Een DNS-server opgeven (optioneel)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namecreategwapart-5---create-a-virtual-network-gateway"></a><a name="creategw"></a>Deel 5: De gateway voor een virtueel netwerk maken
Punt-naar-site-verbindingen vereisen de volgende instellingen:

* Gatewaytype: VPN
* VPN-type: op route gebaseerd

### <a name="to-create-a-virtual-network-gateway"></a>De gateway van een virtueel netwerk maken
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namegeneratecertapart-6---generate-certificates"></a><a name="generatecert"></a>Deel 6: Certificaten genereren
Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. U exporteert gegevens van openbare certificaten (niet de persoonlijke sleutel) als een met Base-64 gecodeerd X.509-.cer-bestand van een basiscertificaat dat is gegenereerd door een commerciële certificeringsoplossing, maar u kunt ook een zelfondertekend basiscertificaat gebruiken. Vervolgens importeert u de gegevens van openbare certificaten uit het basiscertificaat naar Azure. Daarnaast moet u een clientcertificaat genereren uit het basiscertificaat voor clients. Elke client die via een P2S-verbinding wil maken met het virtuele netwerk, moet beschikken over een clientcertificaat dat is gegenereerd op basis van het basiscertificaat.

### <a name="a-namegetcerastep-1---obtain-the-cer-file-for-the-root-certificate"></a><a name="getcer"></a>Stap 1: Het CER-bestand voor het basiscertificaat verkrijgen

U dient het CER-bestand voor het basiscertificaat te verkrijgen. U kunt een basiscertificaat van een bedrijfsoplossing gebruiken of u kunt [een zelfondertekend basiscertificaat met makecert maken](vpn-gateway-certificates-point-to-site.md). Hoewel u met PowerShell zelfondertekende certificaten kunt maken, bevat het certificaat dat door PowerShell wordt gegenereerd, niet de velden die nodig zijn voor P2S-verbindingen.

1. Als u een CER-bestand uit een certificaat wilt ophalen, opent u **certmgr.msc** en zoekt u het basiscertificaat. Klik met de rechtermuisknop op het zelfondertekende basiscertificaat, klik op **Alle taken** en klik vervolgens op **Exporteren**. Hiermee word de **Wizard Certificaat exporteren** geopend.
2. Klik in de wizard op **Volgende**, selecteer **Nee, de persoonlijke sleutel niet exporteren** en klik vervolgens op **Volgende**.
3. Selecteer op de pagina **Bestandsindeling voor export** de optie **Met Base64 gecodeerde X.509 (*.CER).** Klik op **Volgende**. 
4. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.
5. Klik op **Voltooien** om het certificaat te exporteren.

### <a name="a-namegenerateclientcertastep-2---generate-a-client-certificate"></a><a name="generateclientcert"></a>Stap2: Een clientcertificaat genereren
U kunt een uniek certificaat genereren voor elke client die verbinding maakt met een virtueel netwerk of u kunt hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid tot het intrekken van één certificaat, indien nodig. Anders moet u, als alle clients hetzelfde certificaat gebruiken en u het certificaat voor één van de clients moet intrekken, nieuwe certificaten genereren en installeren voor alle clients die het certificaat voor verificatie gebruiken.

####<a name="enterprise-certificate"></a>Commercieel certificaat
- Als u een commerciële certificeringsoplossing gebruikt, genereert u een clientcertificaat met de algemene waarde-indeling 'name@yourdomain.com',, in plaats van de indeling 'domeinnaam\gebruikersnaam'.
- Zorg ervoor dat het clientcertificaat dat u verleent is gebaseerd op de certificaatsjabloon voor Gebruiker met Clientverificatie als het eerste item in de lijst, in plaats van bijvoorbeeld smartcardaanmelding. U kunt het certificaat controleren door op het clientcertificaat te dubbelklikken en **Details > Uitgebreid sleutelgebruik** weer te geven.

####<a name="self-signed-certificate"></a>Zelfondertekend certificaat 
Als u een zelfondertekend certificaat gebruikt, raadpleegt u [Werken met zelfondertekende basiscertificaten voor punt-naar-site-configuraties](vpn-gateway-certificates-point-to-site.md) om een clientcertificaat te genereren.

### <a name="a-nameexportclientcertastep-3---export-the-client-certificate"></a><a name="exportclientcert"></a>Stap 3: Het clientcertificaat exporteren
Er is een clientcertificaat vereist voor verificatie. Nadat u het clientcertificaat hebt gegenereerd, exporteert u het. Het clientcertificaat dat u hebt geëxporteerd, wordt later geïnstalleerd op elke clientcomputer.

1. U kunt *certmgr.msc* gebruiken om een clientcertificaat te exporteren. Klik met de rechtermuisknop op het clientcertificaat dat u wilt exporteren, klik op **Alle taken** en vervolgens op **Exporteren**.
2. Exporteer het clientcertificaat met de persoonlijke sleutel. Dit is een *PFX*-bestand. Zorg dat u het wachtwoord (sleutel) dat u voor dit certificaat instelt, ergens noteert of onthoudt.

## <a name="a-nameaddresspoolapart-7---add-the-client-address-pool"></a><a name="addresspool"></a>Deel 7: De clientadresgroep toevoegen
1. Nadat de virtuele netwerkgateway is gemaakt, gaat u naar de sectie **Instellingen** van de blade van de virtuele netwerkgateway. Klik in de sectie **Instellingen** op **Punt-naar-site-configuratie** om de blade **Configuratie** te openen.
   
    ![Blade Punt-naar-site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Adresgroep** is de groep met IP-adressen van waaruit clients die verbinding maken, een IP-adres ontvangen. Voeg de adresgroep toe en klik vervolgens op **Opslaan**.
   
    ![Clientadresgroep](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="a-nameuploadfileapart-8---upload-the-root-certificate-cer-file"></a><a name="uploadfile"></a>Deel 8: Het CER-bestand van het basiscertificaat uploaden
Nadat de gateway is gemaakt, kunt u het .cer-bestand voor een vertrouwd basiscertificaat uploaden naar Azure. U kunt bestanden voor maximaal twintig basiscertificaten uploaden. U uploadt de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het .cer-bestand is geüpload, gebruikt Azure dit voor het verifiëren van clients die verbinding maken met het virtuele netwerk.

1. Certificaten worden toegevoegd op de blade **punt-naar-site-configuratie** in de sectie **Basiscertificaat**.  
2. Zorg ervoor dat u het basiscertificaat hebt geëxporteerd als een Base-64 encoded X.509 (.cer)-bestand. U moet het bestand in deze indeling exporteren, zodat u het certificaat met een teksteditor kunt openen.
3. Open het certificaat met een teksteditor zoals Kladblok. Kopieer alleen het volgende gedeelte als één ononderbroken regel:
   
    ![Certificaatgegevens](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)

    > [!NOTE]
    > Zorg er bij het kopiëren van de certificaatgegevens voor dat u de tekst als één ononderbroken regel kopieert zonder regelterugloop of regelinvoer. Mogelijk moet u de weergave in de teksteditor wijzigen naar Symbool weergeven/Alle tekens weergeven om de regelterugloop en regelinvoer te zien.                                                                                                                                                                            
    >
    >

4. Plak de certificaatgegevens in het veld **Gegevens van openbaar certificaat**. Geef een **naam** aan het certificaat en klik vervolgens op **Opslaan**. U kunt maximaal 20 vertrouwde basiscertificaten toevoegen.
   
    ![Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="a-nameclientconfigapart-9---download-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>Deel 9: Het configuratiepakket voor de VPN-client downloaden en installeren
Clients die met een P2S-verbinding maken met Azure, moeten zowel een certificaat als een VPN-clientconfiguratiepakket hebben geïnstalleerd. Configuratiepakketten voor VPN-clients zijn beschikbaar voor Windows-clients. 

Het VPN-clientpakket bevat gegevens voor de configuratie van de VPN-clientsoftware die is ingebouwd in Windows. De configuratie is specifiek voor de VPN waarmee u verbinding wilt maken. Met het pakket wordt geen andere software geïnstalleerd.

1. Klik op de blade **Punt-naar-site-configuratie** op **VPN-client downloaden** om de blade **VPN-client downloaden** te openen.
   
    ![VPN-client downloaden 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Selecteer het juiste pakket voor de client en klik vervolgens op **Downloaden**. Selecteer voor 64-bits clients **AMD64**. Selecteer voor 32-bits clients **x86**.

    ![VPN-client downloaden 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/client.png)
3. Installeer het pakket op de clientcomputer. Als u een SmartScreen-melding ontvangt, klikt u op **Meer info** en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
4. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De verbinding wordt nu vermeld. U ziet de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. Deze is vergelijkbaar met dit voorbeeld: 
   
    ![VPN-client](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png)


## <a name="a-nameinstallclientcertapart-10---install-the-client-certificate"></a><a name="installclientcert"></a>Deel 10: Het clientcertificaat installeren
Elke clientcomputer moet een clientcertificaat hebben om te kunnen verifiëren. Wanneer u het clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat.

1. Kopieer het .pfx-bestand naar de clientcomputer.
2. Dubbelklik op het .pfx-bestand om het te installeren. Wijzig de installatielocatie niet.

## <a name="a-nameconnectapart-11---connect-to-azure"></a><a name="connect"></a>Deel 11: Verbinding maken met Azure
1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken. 
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.
   
    ![VPN-client maakt verbinding met Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)

    
3. Uw verbinding wordt nu tot stand gebracht.
   
    ![VPN-client verbonden met Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)
                                                                                                                                                                           

> [!NOTE]
> Als u een certificaat gebruikt dat is verleend met behulp van een oplossing voor een CA voor ondernemingen en problemen ondervindt bij de verificatie, controleert u de verificatievolgorde op het clientcertificaat. U kunt de volgorde van de verificatielijst controleren door op het clientcertificaat te dubbelklikken en naar **Details > Uitgebreid sleutelgebruik** te gaan. Controleer of Clientverificatie als het eerste item in de lijst wordt weergegeven. Als dat niet het geval is, moet u een clientcertificaat op basis van de gebruikerssjabloon verlenen waarin clientverificatie als het eerste item in de lijst wordt weergegeven. 
>
>

## <a name="a-nameverifyapart-12---verify-your-connection"></a><a name="verify"></a>Deel 12: De verbinding controleren
1. Als u wilt controleren of uw VPN-verbinding actief is, opent u een opdrachtprompt met verhoogde bevoegdheid en voert u *ipconfig/all* in.
2. Bekijk de resultaten. Het IP-adres dat u hebt ontvangen is een van de adressen binnen het adresbereik van de punt-naar-site-VPN-clientadresgroep die u hebt opgegeven in uw configuratie. De resultaten moeten er ongeveer als volgt uitzien:
   
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

## <a name="a-nameaddato-add-or-remove-trusted-root-certificates"></a><a name="add"></a>Vertrouwde basiscertificaten toevoegen of verwijderen
U kunt vertrouwde basiscertificaat verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, kunnen de clientcertificaten die zijn gegenereerd op basis van het basiscertificaat geen verbinding meer maken met Azure via punt-naar-site. Als u wilt dat clients verbinding kunnen maken, moeten deze een nieuw clientcertificaat installeren dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

U kunt de lijst met ingetrokken clientcertificaten beheren op de blade **Punt-naar-site configuratie**. Dit is de blade die u hebt gebruikt voor het [uploaden van een vertrouwd basiscertificaat](#uploadfile).

## <a name="a-namerevokeclientato-manage-the-list-of-revoked-client-certificates"></a><a name="revokeclient"></a>De lijst met ingetrokken clientcertificaten beheren
U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Als u een .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u een bepaald clientcertificaat wilt intrekken, en niet het basiscertificaat, is dat mogelijk. Op die manier zijn andere certificaten die zijn gegenereerd op basis van het basiscertificaat nog steeds geldig. 

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

U kunt de lijst met ingetrokken clientcertificaten beheren op de blade **Punt-naar-site configuratie**. Dit is de blade die u hebt gebruikt voor het [uploaden van een vertrouwd basiscertificaat](#uploadfile). Voeg de naam van het certificaat en de vingerafdruk toe en sla vervolgens op.

## <a name="a-namefaqapoint-to-site-faq"></a><a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.




<!--HONumber=Feb17_HO3-->


