---
title: Een punt-naar-site-VPN-gatewayverbinding met een virtueel netwerk configureren met behulp van het Resource Manager-implementatiemodel | Microsoft Docs
description: U maakt een beveiligde verbinding met uw Azure Virtual Network door een punt-naar-site-VPN-gatewayverbinding te maken.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 3fe204c09eebf7d254a1bf2bb130e2d3498b6b45
ms.openlocfilehash: 41bba6660c52d4aa7d10d846ad65e1f6aa5e582c


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Een punt-naar-site-verbinding met een VNet configureren met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. Een P2S-verbinding is nuttig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk. 

Voor punt-naar-site-verbindingen hebt u geen VPN-apparaat of openbaar IP-adres nodig. Er wordt een VPN-verbinding tot stand gebracht door de verbinding te starten vanaf de clientcomputer. Voor meer informatie over punt-naar-site-verbindingen leest u [Veelgestelde vragen over VPN-gateways](vpn-gateway-vpn-faq.md#point-to-site-connections) en [Planning en ontwerp](vpn-gateway-plan-design.md). 

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het Resource Manager-implementatiemodel met behulp van PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en -methoden voor P2S-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Basiswerkstroom
![Punt-naar-site-diagram](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")

In dit scenario maakt u een virtueel netwerk met een punt-naar-site-verbinding. De instructies helpen u ook bij het genereren van certificaten die u nodig hebt voor deze configuratie. Een P2S-verbinding bestaat uit de volgende onderdelen: een VNet met een VPN-gateway, een basiscertificaat in een CER-bestand (openbare sleutel), een clientcertificaat en de VPN-configuratie op de client. 

Voor deze configuratie gebruikt u de volgende waarden. De variabelen worden ingesteld in sectie [1](#declare) van dit artikel. U kunt de stappen gebruiken als een overzicht en de waarden ongewijzigd gebruiken, of u kunt ze wijzigen zodat ze overeenkomen met uw omgeving. 

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Voorbeeldwaarden
* **Naam: VNet1**
* **Adresruimte: 192.168.0.0/16** en **10.254.0.0/16**<br>Voor dit voorbeeld gebruiken we meer dan één adresruimte om te verduidelijken dat deze configuratie met meerdere adresruimten werkt. Meerdere adresruimten zijn echter niet vereist voor deze configuratie.
* **Subnetnaam: FrontEnd**
  * **Subnetadresbereik: 192.168.1.0/24**
* **Subnetnaam: BackEnd**
  * **Subnetadresbereik: 10.254.1.0/24**
* **Subnetnaam: GatewaySubnet**<br>De naam van het subnet *GatewaySubnet* is verplicht voor een goede werking van de VPN-gateway.
  * **Subnetadresbereik: 192.168.200.0/24** 
* **VPN-clientadresgroep: 172.16.201.0/24**<br>VPN-clients die verbinding maken met het VNet via deze punt-naar-site-verbinding, ontvangen een IP-adres van de VPN-clientadresgroep.
* **Abonnement:** controleer of u het juiste abonnement hebt in het geval u er meer dan één hebt.
* **Resourcegroep: TestRG**
* **Locatie: VS - oost**
* **DNS-server: IP-adres** van de DNS-server die u wilt gebruiken voor naamomzetting.
* **Gatewaynaam: Vnet1GW**
* **Openbare IP-naam: VNet1GWPIP**
* **VPNType: op route gebaseerd**

## <a name="before-beginning"></a>Voordat u begint
* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).
* Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets. Als u werkt met PowerShell voor deze configuratie, zorg er dan voor dat u de bewerkingen uitvoert als beheerder. 

## <a name="a-namedeclareapart-1---log-in-and-set-variables"></a><a name="declare"></a>Deel 1: Aanmelden en variabelen instellen
In deze sectie meldt u zich aan en declareert u de waarden die voor deze configuratie worden gebruikt. De opgegeven waarden worden in de voorbeeldscripts gebruikt. Wijzig de waarden zodat ze overeenkomen met uw omgeving. U kunt ook de gedeclareerde waarden gebruiken en de stappen bij wijze van oefening doorlopen.

1. Meld u in de PowerShell-console aan bij uw Azure-account. Deze cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell.
   
        Login-AzureRmAccount 
2. Haal een lijst met uw Azure-abonnementen op.
   
        Get-AzureRmSubscription
3. Geef het abonnement op dat u wilt gebruiken. 
   
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"
4. Declareer de waarden die u wilt gebruiken. Gebruik het volgende voorbeeld, en vervang zo nodig de waarden door uw eigen waarden.
   
        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"

## <a name="a-nameconfigurevnetapart-2---configure-a-vnet"></a><a name="ConfigureVNet"></a>Deel 2: Een VNet configureren
1. Maak een resourcegroep.
   
        New-AzureRmResourceGroup -Name $RG -Location $Location
2. Maak de subnetconfiguraties voor het virtuele netwerk, noem deze *FrontEnd*, *BackEnd* en *GatewaySubnet*. Deze voorvoegsels moeten deel uitmaken van de VNet-adresruimte die u hebt opgegeven.
   
        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
3. Maak het virtuele netwerk. De opgegeven DNS-server moet een DNS-server zijn die de namen kan omzetten voor de resources waarmee u verbinding maakt. In dit voorbeeld hebben we een openbaar IP-adres gebruikt. Zorg ervoor dat u uw eigen waarden gebruikt.
   
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
4. Geef de variabelen op voor het virtuele netwerk dat u hebt gemaakt.
   
        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
5. Vraag een dynamisch toegewezen openbaar IP-adres aan. Dit IP-adres is nodig voor een juiste werking van de gateway. Later verbindt u de gateway met de IP-configuratie van de gateway.
   
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="a-namecertificatesapart-3---certificates"></a><a name="Certificates"></a>Deel 3: SSL-certificaten
Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. U exporteert gegevens van openbare certificaten (niet de persoonlijke sleutel) als een met Base-64 gecodeerd X.509-.cer-bestand van een basiscertificaat dat is gegenereerd door een commerciële certificeringsoplossing, maar u kunt ook een zelfondertekend basiscertificaat gebruiken. Vervolgens importeert u de gegevens van openbare certificaten uit het basiscertificaat naar Azure. Daarnaast moet u een clientcertificaat genereren uit het basiscertificaat voor clients. Elke client die via een P2S-verbinding wil maken met het virtuele netwerk, moet beschikken over een clientcertificaat dat is gegenereerd op basis van het basiscertificaat.

### <a name="a-namecera1-obtain-the-cer-file-for-the-root-certificate"></a><a name="cer"></a>1. Het CER-bestand voor het basiscertificaat verkrijgen
U moet de openbare certificaatgegevens ophalen voor het basiscertificaat dat u wilt gebruiken.

* Als u een commercieel certificeringssysteem gebruikt, haalt u het CER-bestand voor het basiscertificaat op dat u wilt gebruiken. 
* Als u geen commerciële certificeringsoplossing gebruikt, moet u een zelfondertekend basiscertificaat genereren. Voor de stappen in Windows 10 raadpleegt u [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Werken met zelfondertekende basiscertificaten voor punt-naar-site-configuraties).

1. Als u een CER-bestand uit een certificaat wilt ophalen, opent u **certmgr.msc** en zoekt u het basiscertificaat. Klik met de rechtermuisknop op het zelfondertekende basiscertificaat, klik op **Alle taken** en klik vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.
2. Klik in de wizard op **Volgende**, selecteer **Nee, de persoonlijke sleutel niet exporteren** en klik vervolgens op **Volgende**.
3. Selecteer op de pagina **Bestandsindeling voor export** de optie **Met Base64 gecodeerde X.509 (*.CER).** Klik op **Volgende**. 
4. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.
5. Klik op **Voltooien** om het certificaat te exporteren.

### <a name="a-namegeneratea2-generate-the-client-certificate"></a><a name="generate"></a>2. Het clientcertificaat genereren
Nu gaat u de clientcertificaten genereren. U kunt een uniek certificaat genereren voor elke client die verbinding maakt of u kunt hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid tot het intrekken van één certificaat, indien nodig. Anders moet u, als alle clients hetzelfde certificaat gebruiken en u het certificaat voor één van de clients moet intrekken, nieuwe certificaten genereren en installeren voor alle clients die het certificaat gebruiken om te verifiëren. De clientcertificaten worden verderop in deze oefening op elke clientcomputer geïnstalleerd.

* Als u een commerciële certificeringsoplossing gebruikt, genereert u een clientcertificaat met de algemene waarde-indeling 'name@yourdomain.com',, in plaats van de NetBIOS-indeling 'DOMEIN\gebruikersnaam'. 
* Als u een zelfondertekend certificaat gebruikt, raadpleegt u [Werken met zelfondertekende basiscertificaten voor punt-naar-site-configuraties](vpn-gateway-certificates-point-to-site.md) om een clientcertificaat te genereren.

### <a name="a-nameexportclientcerta3-export-the-client-certificate"></a><a name="exportclientcert"></a>3. Het clientcertificaat exporteren
Er is een clientcertificaat vereist voor verificatie. Nadat u het clientcertificaat hebt gegenereerd, exporteert u het. Het clientcertificaat dat u hebt geëxporteerd, wordt later geïnstalleerd op elke clientcomputer.

1. U kunt *certmgr.msc* gebruiken om een clientcertificaat te exporteren. Klik met de rechtermuisknop op het clientcertificaat dat u wilt exporteren, klik op **Alle taken** en vervolgens op **Exporteren**.
2. Exporteer het clientcertificaat met de persoonlijke sleutel. Dit is een *PFX*-bestand. Zorg dat u het wachtwoord (sleutel) dat u voor dit certificaat instelt, ergens noteert of onthoudt.

### <a name="a-nameuploada4-upload-the-root-certificate-cer-file"></a><a name="upload"></a>4. Het CER-bestand van het basiscertificaat uploaden
Declareer de variabele voor uw certificaatnaam waarbij u de waarde vervangt door uw eigen waarde:

        $P2SRootCertName = "Mycertificatename.cer"

Voeg de openbare certificaatgegevens voor het basiscertificaat toe aan Azure. U kunt bestanden voor maximaal twintig basiscertificaten uploaden. U uploadt de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het .cer-bestand is geüpload, gebruikt Azure dit voor het verifiëren van clients die verbinding maken met het virtuele netwerk. 

Vervang het pad naar het bestand door uw eigen pad en voer vervolgens de cmdlets uit.

        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="a-namecreategatewayapart-4---create-the-vpn-gateway"></a><a name="creategateway"></a>Deel 4: De VPN-gateway maken
Configureer en maak de virtuele netwerkgateway voor uw VNet. De *-GatewayType* moet **Vpn** zijn en het *-VpnType* moet **RouteBased** zijn. Dit kan maximaal 45 minuten duren.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="a-nameclientconfigapart-5---download-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>Deel 5: Het configuratiepakket voor de VPN-client downloaden
Clients die met een P2S-verbinding maken met Azure, moeten zowel een certificaat als een VPN-clientconfiguratiepakket hebben geïnstalleerd. Configuratiepakketten voor VPN-clients zijn beschikbaar voor Windows-clients. Het VPN-clientpakket bevat informatie om de VPN-clientsoftware te configureren die in Windows is ingebouwd en specifiek is voor het VPN waarmee u verbinding wilt maken. Met het pakket wordt geen andere software geïnstalleerd. Zie de [Veelgestelde vragen over de VPN Gateway](vpn-gateway-vpn-faq.md#point-to-site-connections) voor meer informatie.

1. Wanneer de gateway is gemaakt, kunt u het clientconfiguratiepakket downloaden. In dit voorbeeld wordt het pakket gedownload voor 64-bits clients. Als u de 32-bits client wilt downloaden, vervangt u 'Amd64' door 'x86'. U kunt de VPN-client ook downloaden via Azure Portal.
   
        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
2. De PowerShell-cmdlet retourneert een URL-koppeling. Dit is een voorbeeld van hoe de geretourneerde URL eruitziet:
   
        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
3. Kopieer en plak de geretourneerde koppeling naar een webbrowser om het pakket te downloaden. Installeer het pakket vervolgens op de clientcomputer. Als u een SmartScreen-melding ontvangt, klikt u op **Meer info** en klikt u vervolgens **Toch uitvoeren** om het pakket te installeren.
4. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De verbinding wordt nu vermeld. U ziet de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. Deze is vergelijkbaar met dit voorbeeld: 
   
    ![VPN-client](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN client")

## <a name="a-nameclientcertificateapart-6---install-the-client-certificate"></a><a name="clientcertificate"></a>Deel 6: Het clientcertificaat installeren
Elke clientcomputer moet een clientcertificaat hebben om te kunnen verifiëren. Wanneer u het clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat.

1. Kopieer het .pfx-bestand naar de clientcomputer.
2. Dubbelklik op het .pfx-bestand om het te installeren. Wijzig de installatielocatie niet.

## <a name="a-nameconnectapart-7---connect-to-azure"></a><a name="connect"></a>Deel 7: Verbinding maken met Azure
1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken. 
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.
   
    ![VPN-clientverbinding](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN client connection")
3. Uw verbinding wordt nu tot stand gebracht.
   
    ![Verbinding tot stand gebracht](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Connection established")

## <a name="a-nameverifyapart-8---verify-your-connection"></a><a name="verify"></a>Deel 8: De verbinding controleren
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

## <a name="a-nameaddremovecertato-add-or-remove-a-trusted-root-certificate"></a><a name="addremovecert"></a>Een vertrouwd basiscertificaat toevoegen of verwijderen
Certificaten worden gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. Via de volgende stappen kunt u basiscertificaten toevoegen en verwijderen. Wanneer u een met Base64 gecodeerd X.509 (.cer)-bestand aan Azure toevoegt, geeft u bij Azure aan dat deze het basiscertificaat dat het bestand vertegenwoordigt, moet vertrouwen. 

U kunt vertrouwde basiscertificaten toevoegen of verwijderen via PowerShell of in Azure Portal. Als u dit wilt doen in Azure Portal, gaat u naar uw **Virtuele netwerkgateway > Instellingen > Punt-naar-site-configuratie > Basiscertificaten**. De volgende stappen helpen u bij het uitvoeren van deze taken met PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat toevoegen
U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Volg de volgende stappen om een basiscertificaat toe te voegen.

1. Maak en bereid het nieuwe basiscertificaat voor dat u aan Azure gaat toevoegen. Exporteer de openbare sleutel als een met Base-64 gecodeerd X.509-certificaat (.CER) en open het bestand met een teksteditor. Kopieer vervolgens alleen de onderstaande sectie. 
   
    Kopieer de waarden, zoals weergegeven in het volgende voorbeeld:
   
    ![certificaat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certificate")
2. Geef de certificaatnaam en sleutelgegevens op als een variabele. Vervang de gegevens door uw eigen gegevens, zoals weergegeven in het volgende voorbeeld:
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
3. Voeg het nieuwe basiscertificaat toe. U kunt slechts één certificaat tegelijk toevoegen.
   
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
4. U kunt controleren of het nieuwe certificaat correct is toegevoegd met behulp van de volgende cmdlet.
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat verwijderen
U kunt vertrouwde basiscertificaat verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, kunnen de clientcertificaten die zijn gegenereerd op basis van het basiscertificaat geen verbinding meer maken met Azure via punt-naar-site. Als u wilt dat clients verbinding kunnen maken, moeten deze een nieuw clientcertificaat installeren dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

1. Als u een vertrouwd basiscertificaat wilt verwijderen, wijzigt u het volgende voorbeeld:
   
    Declareer de variabelen.
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
2. Verwijder het certificaat.
   
        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
3. Gebruik de volgende cmdlet om te controleren of het certificaat is verwijderd. 
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="a-namerevokeato-manage-the-list-of-revoked-client-certificates"></a><a name="revoke"></a>De lijst met ingetrokken clientcertificaten beheren
U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Als u een .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u een bepaald clientcertificaat wilt intrekken, en niet het basiscertificaat, is dat mogelijk. Op die manier zijn andere certificaten die zijn gegenereerd op basis van het basiscertificaat nog steeds geldig.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="revoke-a-client-certificate"></a>Een clientcertificaat intrekken
1. Zoek de vingerafdruk van het clientcertificaat dat u wilt intrekken.
   
        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "?ef2af033d0686820f5a3c74804d167b88b69982f"
2. Voeg de vingerafdruk toe aan de lijst met ingetrokken vingerafdrukken.
   
        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
3. Controleer of de vingerafdruk is toegevoegd aan de certificaatintrekkingslijst. U moet één vingerafdruk tegelijk toevoegen.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Een clientcertificaat reactiveren
U kunt een clientcertificaat opnieuw activeren door de vingerafdruk te verwijderen uit de lijst met ingetrokken clientcertificaten.

1. Verwijder de vingerafdruk uit de lijst met de vingerafdrukken waarvan het clientcertificaat is ingetrokken.
   
       Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
       -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
2. Controleer of de vingerafdruk is verwijderd uit de lijst met ingetrokken vingerafdrukken.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.




<!--HONumber=Dec16_HO1-->


