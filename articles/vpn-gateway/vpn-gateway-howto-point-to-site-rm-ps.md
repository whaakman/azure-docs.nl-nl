<properties 
   pageTitle="Een punt-naar-site-VPN-verbinding met een virtueel netwerk configureren met behulp van het Resource Manager-implementatiemodel | Microsoft Azure"
   description="U maakt een beveiligde verbinding met uw Azure Virtual Network door een punt-naar-site-VPN-verbinding te maken."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="cherylmc" />

# Een punt-naar-site-verbinding met een virtueel netwerk configureren met behulp van PowerShell

> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Portal - klassiek](vpn-gateway-point-to-site-create.md)

Met een punt-naar-site (P2S)-configuratie kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. Een P2S-verbinding is nuttig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk. 

Voor punt-naar-site-verbindingen hebt u geen VPN-apparaat of openbaar IP-adres nodig. Er wordt een VPN-verbinding tot stand gebracht door de verbinding te starten vanaf de clientcomputer. Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over VPN Gateways](vpn-gateway-vpn-faq.md#point-to-site-connections) en [Planning en ontwerp](vpn-gateway-plan-design.md).

Dit artikel geldt voor punt-naar-site-verbindingen naar een virtueel netwerk dat is gemaakt met behulp van het Resource Manager-implementatiemodel. De stappen in dit artikel gebruiken PowerShell.

**Over Azure-implementatiemodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Implementatiemodellen en hulpmiddelen voor punt-naar-site-verbindingen**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

![Punt-naar-site-diagram](./media/vpn-gateway-point-to-site-create/point2site.png "point-to-site")


## Over deze configuratie

In dit scenario maakt u een virtueel netwerk met een punt-naar-site-verbinding. Een P2S-verbinding bestaat uit de volgende onderdelen: een VNet met een gateway, een basiscertificaat in een .cer-bestand, een clientcertificaat en de VPN-configuratie aan de clientzijde. 

Voor deze configuratie gebruiken we de volgende waarden:

- Naam: **VNet1**, met de adresruimten **192.168.0.0/16** en **10.254.0.0/16**. Opmerking: u kunt meer dan één adresruimte gebruiken voor een VNet.
- Subnetnaam: **FrontEnd**, met **192.168.1.0/24**
- Subnet name: **BackEnd**, met **10.254.1.0/24**
- Subnetnaam: **GatewaySubnet**, met **192.168.200.0/24**. De naam van het subnet *GatewaySubnet* is verplicht voor een goede werking van de gateway. 
- VPN-clientadresgroep: **172.16.201.0/24**. VPN-clients die verbinding maken met de VNet via deze punt-naar-site-verbinding ontvangen een IP-adres van deze groep.
- Abonnement: controleer of u het juiste abonnement hebt in het geval u er meer dan één hebt.
- Resourcegroep: **TestRG**
- Locatie: **VS - oost**
- DNS-server: **IP-adres** van de DNS-server die u wilt gebruiken voor naamomzetting.
- Naam van GW: **GW**
- Openbare IP-naam: **GWIP**
- VpnType: **op route gebaseerd**


## Voordat u begint

- Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
    
- Installeer de Azure Resource Manager PowerShell-cmdlets (1.0.2 of later). Zie [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets.

## Een punt-naar-site-verbinding configureren voor Azure

1. Meld u in de PowerShell-console aan bij uw Azure-account. Deze cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell.

        Login-AzureRmAccount 

2. Haal een lijst met uw Azure-abonnementen op.

        Get-AzureRmSubscription

3. Geef het abonnement op dat u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. In deze configuratie worden de volgende PowerShell-variabelen gedefinieerd met de waarden die u wilt gebruiken. De opgegeven waarden worden in de voorbeeldscripts gebruikt. Geef de waarden op die u wilt gebruiken. Gebruik het volgende voorbeeld, en vervang zo nodig de waarden door uw eigen waarden. 

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
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $P2SRootCertName = "ARMP2SRootCert.cer"

5. Maak een nieuwe resourcegroep.

        New-AzureRmResourceGroup -Name $RG -Location $Location

6. Maak de subnetconfiguraties voor het virtuele netwerk, noem deze *FrontEnd*, *BackEnd* en *GatewaySubnet*. Deze voorvoegsels moeten deel uitmaken van de VNet-adresruimte die hierboven is opgegeven.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

7. Maak het virtuele netwerk. De opgegeven DNS-server moet een DNS-server zijn die de namen kan omzetten voor de resources waarmee u verbinding maakt. In dit voorbeeld hebben we een openbaar IP-adres gebruikt. Zorg ervoor dat u uw eigen waarden gebruikt.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

8. Geef de variabelen op voor het virtuele netwerk dat u hebt gemaakt.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

9. Vraag een dynamisch toegewezen openbaar IP-adres aan. Dit IP-adres is nodig voor een juiste werking van de gateway. Later verbindt u de gateway met de IP-configuratie van de gateway.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
        
10. Voeg een vertrouwd certificaat toe aan Azure. U kunt maximaal 20 certificaten toevoegen. Zie voor instructies voor het maken van een zelfondertekend basiscertificaat met *makecert* het artikel [Werken met zelfondertekende basiscertificaten voor punt-naar-site-configuraties](vpn-gateway-certificates-point-to-site.md). Wanneer u een met Base64 gecodeerd X.509 (.cer)-bestand aan Azure toevoegt, geeft u bij Azure aan dat deze het basiscertificaat dat het bestand vertegenwoordigt, moet vertrouwen.

    Als u de openbare sleutel wilt hebben, kunt u het certificaat exporteren als een met Base64 gecodeerd X.509 (.cer)-bestand. Noteer het pad waarnaar u het .cer-bestand hebt geëxporteerd. Dit is een voorbeeld van het verkrijgen van de Base64-tekenreeksweergave van uw certificaat. U moet het pad naar uw eigen .cer-bestand gebruiken voor deze stap.
    
        $filePathForCert = "pasteYourCerFilePathHere"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64


11. Maak de gateway van het virtuele netwerk voor uw VNet. De *-GatewayType* moet **Vpn** zijn en het *-VpnType* moet **RouteBased** zijn.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## Clientconfiguratie

Elke client die verbinding met Azure maakt met behulp van punt-naar-site moet over twee dingen beschikken: de VPN-client moet worden geconfigureerd om verbinding te maken, en op de client moet een clientcertificaat zijn geïnstalleerd. Configuratiepakketten voor VPN-clients zijn beschikbaar voor Windows-clients. Zie de [Veelgestelde vragen over de VPN Gateway](vpn-gateway-vpn-faq.md#point-to-site-connections) voor meer informatie. 

1. Download het configuratiepakket voor de VPN-client. Gebruik in deze stap het volgende voorbeeld voor het downloaden van het clientconfiguratiepakket.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

    De PowerShell-cmdlet retourneert een URL-koppeling. Kopieer en plak de geretourneerde koppeling naar een webbrowser om het pakket te downloaden op uw computer. Dit is een voorbeeld van hoe de geretourneerde URL eruit ziet.

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
    
2. Genereer en installeer de clientcertificaten (*.pfx) die zijn gemaakt op basis van het basiscertificaat op de clientcomputers. U kunt elke willekeurige installatiemethode gebruiken. Als u een zelfondertekend basiscertificaat gebruikt en niet bekend bent met hoe u dit moet doen, raadpleegt u het artikel [Werken met zelfondertekende basiscertificaten voor punt-naar-site-configuraties](vpn-gateway-certificates-point-to-site.md). 

3. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken. 

4. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.

5. Uw verbinding wordt nu tot stand gebracht. U kunt uw verbinding verifiëren met behulp van de volgende procedure.

## De verbinding controleren

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

## Een vertrouwd basiscertificaat toevoegen of verwijderen

Certificaten worden gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. Via de volgende stappen kunt u basiscertificaten toevoegen en verwijderen. Wanneer u een met Base64 gecodeerd X.509 (.cer)-bestand aan Azure toevoegt, geeft u bij Azure aan dat deze het basiscertificaat dat het bestand vertegenwoordigt, moet vertrouwen.

### Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Volg de volgende stappen om een basiscertificaat toe te voegen.

1. Maak en bereid het nieuwe basiscertificaat voor dat u aan Azure gaat toevoegen.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Voeg het nieuwe basiscertificaat toe. U kunt slechts één certificaat tegelijk toevoegen.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2

3. U kunt controleren of het nieuwe certificaat correct is toegevoegd met behulp van de volgende cmdlet.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName

### Een vertrouwd basiscertificaat verwijderen

U kunt vertrouwde basiscertificaat verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, kunnen de clientcertificaten die zijn gegenereerd op basis van het basiscertificaat geen verbinding meer maken met Azure via punt-naar-site. Als u wilt dat clients verbinding kunnen maken, moeten deze een nieuw clientcertificaat installeren dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

1. Als u een vertrouwd basiscertificaat wilt verwijderen, wijzigt u het volgende voorbeeld.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

 
2. Gebruik de volgende cmdlet om te controleren of het certificaat is verwijderd. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName $RG -VirtualNetworkGatewayName $GWName

## De lijst met ingetrokken clientcertificaten beheren

U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Als u een .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u een bepaald clientcertificaat wilt intrekken, en niet het basiscertificaat, is dat mogelijk. Op die manier zijn andere certificaten die zijn gegenereerd op basis van het basiscertificaat nog steeds geldig. De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### Een clientcertificaat intrekken

1. Zoek de vingerafdruk van het clientcertificaat dat u wilt intrekken.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Voeg de vingerafdruk toe aan de lijst met ingetrokken vingerafdrukken.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Controleer of de vingerafdruk is toegevoegd aan de certificaatintrekkingslijst. U moet één vingerafdruk tegelijk toevoegen.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### Een clientcertificaat reactiveren

U kunt een clientcertificaat opnieuw activeren door de vingerafdruk te verwijderen uit de lijst met ingetrokken clientcertificaten.

1.  Verwijder de vingerafdruk uit de lijst met de vingerafdrukken waarvan het clientcertificaat is ingetrokken.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Controleer of de vingerafdruk is verwijderd uit de lijst met ingetrokken vingerafdrukken.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## Volgende stappen

U kunt een virtuele machine toevoegen aan het virtuele netwerk. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor de stappen.





<!--HONumber=ago16_HO4-->


