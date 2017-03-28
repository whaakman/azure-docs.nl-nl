---
title: 'Met een computer verbinding maken met een virtueel Azure-netwerk met behulp van punt-naar-site: PowerShell | Microsoft Docs'
description: U maakt met uw computer een beveiligde verbinding met uw Azure Virtual Network door een punt-naar-site-VPN-gatewayverbinding te maken.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 33646ad42b9209b2970c0745acec899aed3ff88c
ms.lasthandoff: 03/21/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Een punt-naar-site-verbinding met een VNet configureren met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Punt-naar-site-verbindingen zijn handig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk. Voor P2S-verbindingen hebt u geen VPN-apparaat of een openbaar IP-adres nodig. U brengt de VPN-verbinding tot stand vanaf de clientcomputer.

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het Resource Manager-implementatiemodel met behulp van PowerShell. Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en -methoden voor P2S-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Basiswerkstroom
![Diagram: een computer verbinden met een Azure VNet-punt-naar-site-verbinding](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

In dit scenario maakt u een virtueel netwerk met een punt-naar-site-verbinding. De instructies helpen u ook bij het genereren van certificaten die u nodig hebt voor deze configuratie. Een P2S-verbinding bestaat uit de volgende onderdelen: een VNet met een VPN-gateway, een basiscertificaat in een CER-bestand (openbare sleutel), een clientcertificaat en de VPN-configuratie op de client. 

Voor deze configuratie gebruikt u de volgende waarden. De variabelen worden ingesteld in sectie [1](#declare) van dit artikel. U kunt de stappen gebruiken als een overzicht en de waarden ongewijzigd gebruiken, of u kunt ze wijzigen zodat ze overeenkomen met uw omgeving. 

### <a name="example"></a>Voorbeeldwaarden
* **Naam: VNet1**
* **Adresruimte: 192.168.0.0/16** en **10.254.0.0/16**<br>Voor dit voorbeeld gebruiken we meer dan één adresruimte om te verduidelijken dat deze configuratie met meerdere adresruimten werkt. Meerdere adresruimten zijn echter niet vereist voor deze configuratie.
* **Subnetnaam: FrontEnd**
  * **Subnetadresbereik: 192.168.1.0/24**
* **Subnetnaam: BackEnd**
  * **Subnetadresbereik: 10.254.1.0/24**
* **Subnetnaam: GatewaySubnet**<br>De naam van het subnet *GatewaySubnet* is verplicht voor een goede werking van de VPN-gateway.
  * **Adresbereik GatewaySubnet: 192.168.200.0/24** 
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

## <a name="declare"></a>Deel 1: Aanmelden en variabelen instellen
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

## <a name="ConfigureVNet"></a>Deel 2: Een VNet configureren
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


## <a name="Certificates"></a>Deel 3: SSL-certificaten
Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. Nadat u het basiscertificaat hebt gemaakt, exporteert u de gegevens van het openbare certificaat (niet de persoonlijke sleutel) als een X.509 CER-bestand dat is gecodeerd met Base64. Vervolgens kunt u de gegevens van het openbare certificaat uploaden naar Azure.

Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. Het clientcertificaat wordt gegenereerd op basis van het basiscertificaat en wordt op elke clientcomputer geïnstalleerd. Als er geen geldig clientcertificaat is geïnstalleerd en de client probeert verbinding te maken met het VNet, mislukt de verificatie.

### <a name="cer"></a>Stap 1: Het CER-bestand voor het basiscertificaat verkrijgen

####<a name="enterprise-certificate"></a>Commercieel certificaat
 
Als u gebruikmaakt van een bedrijfsoplossing kunt u de bestaande certificaatketen gebruiken. Verkrijg het CER-bestand dat u wilt gebruiken voor het basiscertificaat.

####<a name="self-signed-root-certificate"></a>Zelfondertekend basiscertificaat

Als u geen commerciële certificeringsoplossing gebruikt, moet u een zelfondertekend basiscertificaat maken. Als u een zelfondertekend basiscertificaat wilt maken dat de vereiste velden voor P2S-verificatie bevat, kunt u PowerShell gebruiken. Zie [Create a self-signed root certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Een zelfondertekend basiscertificaat voor punt-naar-site-verbindingen maken) voor een stapsgewijze procedure om een zelfondertekend basiscertificaat te maken.

> [!NOTE]
> Eerder was makecert de aanbevolen methode voor het maken van zelfondertekende basiscertificaten en het genereren van clientcertificaten voor punt-naar-site-verbindingen. U kunt nu PowerShell gebruiken om deze certificaten te maken. Een van de voordelen van PowerShell is dat u SHA-2-certificaten kunt maken. Zie [Create a self-signed root certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Een zelfondertekend basiscertificaat voor punt-naar-site-verbindingen maken met PowerShell) voor de vereiste waarden.
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>De openbare sleutel voor een zelfondertekend basiscertificaat exporteren

Punt-naar-site-verbindingen werken alleen als de openbare sleutel (.cer) is geüpload naar Azure. Voer de volgende stappen uit om het CER-bestand voor het zelfondertekende basiscertificaat te exporteren.

1. Als u een CER-bestand wilt genereren van het certificaat, opent u **certmgr.msc**. Zoek het zelfondertekende basiscertificaat, meestal in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten', en klik er met de rechtermuisknop op. Klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.
2. Klik in de wizard op **Volgende**. Selecteer **Nee, de persoonlijke sleutel niet exporteren** en klik vervolgens op **Volgende**.
3. Selecteer op de pagina **Bestandsindeling voor export** de optie **Met Base64 gecodeerde X.509 (*.CER)** en klik op **Volgende**. 
4. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.
5. Klik op **Voltooien** om het certificaat te exporteren. U ziet een bericht dat****de export is gelukt. Klik op **OK** om de wizard te sluiten.

### <a name="generate"></a>Stap2: Een clientcertificaat genereren
U kunt een uniek certificaat genereren voor elke client die verbinding maakt of u kunt hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid tot het intrekken van één certificaat, indien nodig. Anders moet u, als alle clients hetzelfde certificaat gebruiken en u het certificaat voor één van de clients moet intrekken, nieuwe certificaten genereren en installeren voor alle clients die het certificaat voor verificatie gebruiken.

####<a name="enterprise-certificate"></a>Commercieel certificaat
- Als u een commerciële certificeringsoplossing gebruikt, genereert u een clientcertificaat met de algemene waarde-indeling 'name@yourdomain.com', in plaats van de indeling 'domeinnaam\gebruikersnaam'.
- Zorg ervoor dat het clientcertificaat dat u verleent is gebaseerd op de certificaatsjabloon voor Gebruiker met Clientverificatie als het eerste item in de lijst, in plaats van bijvoorbeeld smartcardaanmelding. U kunt het certificaat controleren door op het clientcertificaat te dubbelklikken en **Details > Uitgebreid sleutelgebruik** weer te geven.

####<a name="self-signed-root-certificate"></a>Zelfondertekend basiscertificaat 
Als u een zelfondertekend basiscertificaat gebruikt, raadpleegt u [Generate a client certificate](vpn-gateway-certificates-point-to-site.md#clientcert) (Een clientcertificaat genereren) voor de stappen waarmee u een clientcertificaat kunt genereren dat compatibel is met punt-naar-site-verbindingen.


### <a name="exportclientcert"></a>Stap 3: Het clientcertificaat exporteren

Als u aan de hand van de [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert)-instructies een clientcertificaat genereert van een zelfondertekend basiscertificaat, wordt het certificaat automatisch geïnstalleerd op de computer die u hebt gebruikt om het certificaat te genereren. Als u een clientcertificaat op een andere clientcomputer wilt installeren, moet u het certificaat exporteren.
 
1. Open **certmgr.msc** om een clientcertificaat te exporteren. Klik met de rechtermuisknop op het clientcertificaat dat u wilt exporteren, klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.
2. Klik in de wizard op **Volgende**, selecteer **Ja, de persoonlijke sleutel exporteren** en klik vervolgens op **Volgende**.
3. Laat op de pagina **Bestandsindeling voor export** de standaardinstellingen geselecteerd. Zorg ervoor dat **en mogelijk alle certificaten in het certificeringspad opnemen** is geselecteerd. Klik op **Volgende**.
4. Op de pagina **Beveiliging** moet u de persoonlijke sleutel beveiligen. Als u ervoor kiest om een wachtwoord te gebruiken, is het belangrijk dat u het wachtwoord voor dit certificaat ergens noteert of onthoudt. Klik op **Volgende**.
5. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.
6. Klik op **Voltooien** om het certificaat te exporteren.

### <a name="upload"></a>Stap 4: Het CER-bestand van het basiscertificaat uploaden

Nadat de gateway is gemaakt, kunt u het .cer-bestand voor een vertrouwd basiscertificaat uploaden naar Azure. U kunt bestanden voor maximaal twintig basiscertificaten uploaden. U uploadt de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het .cer-bestand is geüpload, gebruikt Azure dit voor het verifiëren van clients die verbinding maken met het virtuele netwerk.

Declareer de variabele voor uw certificaatnaam waarbij u de waarde vervangt door uw eigen waarde:

        $P2SRootCertName = "Mycertificatename.cer"

Vervang het pad naar het bestand door uw eigen pad en voer vervolgens de cmdlets uit.

        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Deel 4: De VPN-gateway maken
Configureer en maak de virtuele netwerkgateway voor uw VNet. De *-GatewayType* moet **Vpn** zijn en het *-VpnType* moet **RouteBased** zijn. Dit kan maximaal 45 minuten duren.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Deel 5: Het configuratiepakket voor de VPN-client downloaden
Clients die met een P2S-verbinding maken met Azure, moeten zowel een certificaat als een VPN-clientconfiguratiepakket hebben geïnstalleerd. Configuratiepakketten voor VPN-clients zijn beschikbaar voor Windows-clients.

Het VPN-clientpakket bevat configuratiegegevens voor de configuratie van de VPN-clientsoftware die is ingebouwd in Windows. Met het pakket wordt geen andere software geïnstalleerd. De instellingen zijn specifiek voor het virtuele netwerk waarmee u verbinding wilt maken. Zie voor de lijst met ondersteunde clientbesturingssystemen de [Veelgestelde vragen over punt-naar-site-verbindingen](#faq) onderaan dit artikel.

1. Wanneer de gateway is gemaakt, kunt u het clientconfiguratiepakket downloaden. In dit voorbeeld wordt het pakket gedownload voor 64-bits clients. Als u de 32-bits client wilt downloaden, vervangt u 'Amd64' door 'x86'. U kunt de VPN-client ook downloaden via Azure Portal.
   
        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
2. De PowerShell-cmdlet retourneert een URL-koppeling. Dit is een voorbeeld van hoe de geretourneerde URL eruitziet:
   
        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
3. Kopieer en plak de geretourneerde koppeling naar een webbrowser om het pakket te downloaden. Installeer het pakket vervolgens op de clientcomputer. Als u een SmartScreen-melding ontvangt, klikt u op **Meer info** en klikt u vervolgens **Toch uitvoeren** om het pakket te installeren.
4. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De verbinding wordt nu vermeld. U ziet de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. Deze is vergelijkbaar met dit voorbeeld: 
   
    ![VPN-client](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png)

## <a name="clientcertificate"></a>Deel 6 - Een geëxporteerd clientcertificaat installeren

Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat.

1. Zoek het *PFX*-bestand en kopieer het naar de clientcomputer. Dubbelklik op de clientcomputer op het *PFX*-bestand om het te installeren. Laat de waarde voor **Archieflocatie** staan op **Huidige gebruiker** en klik op **Volgende**.
2. Laat de pagina **Te exporteren bestand** ongewijzigd. Klik op **Volgende**.
3. Voer op de pagina **Beveiliging met persoonlijke sleutel** het wachtwoord in voor het certificaat, als u een wachtwoord hebt ingesteld. Controleer anders of de beveiligings-principal die het certificaat installeert, juist is en klik vervolgens op **Volgende**.
4. Laat op de pagina **Certificaatarchief** de standaardlocatie staan en klik op **Volgende**.
5. Klik op **Voltooien**. Klik in het venster **Beveiligingswaarschuwing** voor de certificaatinstallatie op **Ja**. U kunt zonder problemen op Ja klikken omdat u zelf het certificaat hebt gegenereerd. Het certificaat wordt nu geïmporteerd.

## <a name="connect"></a>Deel 7: Verbinding maken met Azure
1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken. 
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.
   
    ![VPN-client verbinden met Azure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Uw verbinding wordt nu tot stand gebracht.
   
    ![Verbinding tot stand gebracht](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

> [!NOTE]
> Als u een certificaat gebruikt dat is verleend met behulp van een oplossing voor een CA voor ondernemingen en problemen ondervindt bij de verificatie, controleert u de verificatievolgorde op het clientcertificaat. U kunt de volgorde van de verificatielijst controleren door op het clientcertificaat te dubbelklikken en naar **Details > Uitgebreid sleutelgebruik** te gaan. Controleer of Clientverificatie als het eerste item in de lijst wordt weergegeven. Als dat niet het geval is, moet u een clientcertificaat op basis van de gebruikerssjabloon verlenen waarin clientverificatie als het eerste item in de lijst wordt weergegeven. 
>
>

## <a name="verify"></a>Deel 8: De verbinding controleren
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

## <a name="addremovecert"></a>Een vertrouwd basiscertificaat toevoegen of verwijderen

U kunt vertrouwde basiscertificaat toevoegen in en verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, kunnen de clientcertificaten die zijn gegenereerd op basis van het basiscertificaat geen verbinding meer maken met Azure via punt-naar-site. Als u wilt dat clients verbinding kunnen maken, moeten deze een nieuw clientcertificaat installeren dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.


### <a name="to-add-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat toevoegen
U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Volg de volgende stappen om een basiscertificaat toe te voegen.

1. Maak en bereid het nieuwe basiscertificaat voor dat u aan Azure gaat toevoegen. Exporteer de openbare sleutel als een met Base-64 gecodeerd X.509-certificaat (.CER) en open het bestand met een teksteditor. Kopieer vervolgens alleen de onderstaande sectie. 
   
    Kopieer de waarden, zoals weergegeven in het volgende voorbeeld:
   
    ![certificaat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

    > [!NOTE]
    > Zorg er bij het kopiëren van de certificaatgegevens voor dat u de tekst als één ononderbroken regel kopieert zonder regelterugloop of regelinvoer. Mogelijk moet u de weergave in de teksteditor wijzigen naar Symbool weergeven/Alle tekens weergeven om de regelterugloop en regelinvoer te zien.                                                                                                                                                                            
    >


2. Geef de certificaatnaam en sleutelgegevens op als een variabele. Vervang de gegevens door uw eigen gegevens, zoals weergegeven in het volgende voorbeeld:
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
3. Voeg het nieuwe basiscertificaat toe. U kunt slechts één certificaat tegelijk toevoegen.
   
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
4. U kunt controleren of het nieuwe certificaat correct is toegevoegd met behulp van het volgende voorbeeld:
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat verwijderen
U kunt vertrouwde basiscertificaat verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, kunnen de clientcertificaten die zijn gegenereerd op basis van het basiscertificaat geen verbinding meer maken met Azure via punt-naar-site. Als u wilt dat clients verbinding kunnen maken, moeten deze een nieuw clientcertificaat installeren dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

1. Declareer de variabelen.
   
        $GWName = "Name_of_virtual_network_gateway"
        $RG = "Name_of_resource_group"
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
2. Verwijder het certificaat.
   
        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
3. Gebruik het volgende voorbeeld om te controleren of het certificaat is verwijderd. 
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Een clientcertificaat intrekken
U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Dit wijkt af van het verwijderen van een vertrouwd basiscertificaat. Als u een vertrouwd .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u in plaats van een basiscertificaat een clientcertificaat intrekt, kunt u de certificaten die zijn gegenereerd op basis van het basiscertificaat, blijven gebruiken voor verificatie van de punt-naar-site-verbinding.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="to-revoke-a-client-certificate"></a>Een clientcertificaat intrekken

1. Haal de vingerafdruk voor het clientcertificaat op. Zie voor meer informatie [De vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieer de gegevens naar een teksteditor en verwijder alle spaties, zodat u een doorlopende tekenreeks overhoudt. U declareert dit als een variabele.
3. Declareer de variabelen. Zorg ervoor dat u de vingerafdruk declareert die u in de vorige stap hebt opgehaald.
   
        $RevokedClientCert1 = "NameofCertificate"
        $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
        $GWName = "Name_of_virtual_network_gateway"
        $RG = "Name_of_resource_group"
4. Voeg de vingerafdruk toe aan de lijst met ingetrokken certificaten. U ziet 'Geslaagd' als de vingerafdruk is toegevoegd.
   
        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
        -Thumbprint $RevokedThumbprint1
5. Controleer of de vingerafdruk is toegevoegd aan de certificaatintrekkingslijst.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
6. Nadat de vingerafdruk is toegevoegd, kan het certificaat niet langer worden gebruikt om te verbinden. Clients die verbinding proberen te maken met het certificaat, ontvangen een bericht waarin wordt gemeld dat het certificaat niet langer geldig is.

### <a name="to-reinstate-a-client-certificate"></a>Een clientcertificaat reactiveren
U kunt een clientcertificaat opnieuw activeren door de vingerafdruk te verwijderen uit de lijst met ingetrokken clientcertificaten.

1. Declareer de variabelen. Zorg ervoor dat u de juiste vingerafdruk declareert voor het certificaat dat u opnieuw wilt activeren.
 
        $RevokedClientCert1 = "NameofCertificate"
        $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
        $GWName = "Name_of_virtual_network_gateway"
        $RG = "Name_of_resource_group"

2. Verwijder de vingerafdruk van het certificaat uit de lijst met ingetrokken certificaten.
   
       Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
       -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
3. Controleer of de vingerafdruk is verwijderd uit de lijst met ingetrokken vingerafdrukken.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
## <a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.



