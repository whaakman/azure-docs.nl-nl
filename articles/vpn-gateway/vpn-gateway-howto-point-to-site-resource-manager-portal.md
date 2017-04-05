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
ms.date: 03/20/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 6703df0f64534ed638e570342eef7fbda2a74d2e
ms.lasthandoff: 04/03/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Een punt-naar-site-verbinding met een VNet configureren met Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Punt-naar-site-verbindingen zijn handig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk. Voor P2S-verbindingen hebt u geen VPN-apparaat of een openbaar IP-adres nodig. U brengt de VPN-verbinding tot stand vanaf de clientcomputer.

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt met behulp van Azure Portal. Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en -methoden voor P2S-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Basiswerkstroom
![Punt-naar-site-diagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

### <a name="example"></a>Voorbeeldwaarden
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

## <a name="createvnet"></a>Deel 1: Een virtueel netwerk maken
Controleer eerst of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial). Als u deze configuratie bij wijze van oefening maakt, kunt u de [voorbeeldwaarden](#example) gebruiken.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>Deel 2: Adresruimte en subnetten opgeven
Wanneer het VNet is gemaakt, kunt u er extra adresruimte en subnetten aan toevoegen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>Deel 3: Een gatewaysubnet toevoegen

Voordat u het virtuele netwerk verbindt met een gateway, moet u eerst het gatewaysubnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. De gatewayservices gebruiken de IP-adressen opgegeven in het gatewaysubnet. Maak indien mogelijk een gatewaysubnet met een CIDR-blok van /28 of /27 zodat er voldoende IP-adressen zijn om aan toekomstige aanvullende configuratievereisten te voldoen.

De schermafbeeldingen in deze sectie worden gegeven als voorbeeld. Zorg ervoor dat u het adresbereik GatewaySubnet gebruikt dat overeenkomt met de vereiste waarden voor uw configuratie.

###<a name="to-create-a-gateway-subnet"></a>Een gatewaysubnet maken

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>Deel 4: Een DNS-server opgeven (optioneel)

DNS is niet vereist voor punt-naar-site-verbindingen. Als u echter naamomzetting wilt instellen voor resources die in uw virtuele netwerk worden geïmplementeerd, moet u een DNS-server opgeven. Met deze instelling kunt u de DNS-server opgeven die u wilt gebruiken voor de naamomzetting voor dit virtuele netwerk. Hierdoor wordt geen DNS-server aangemaakt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Deel 5: De gateway voor een virtueel netwerk maken
Punt-naar-site-verbindingen vereisen de volgende instellingen:

* Gatewaytype: VPN
* VPN-type: op route gebaseerd

### <a name="to-create-a-virtual-network-gateway"></a>De gateway van een virtueel netwerk maken
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Deel 6: Certificaten genereren

Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. Nadat u het basiscertificaat hebt gemaakt, exporteert u de gegevens van het openbare certificaat (niet de persoonlijke sleutel) als een X.509 CER-bestand dat is gecodeerd met Base64. Vervolgens kunt u de gegevens van het openbare certificaat uploaden naar Azure.

Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. Het clientcertificaat wordt gegenereerd op basis van het basiscertificaat en wordt op elke clientcomputer geïnstalleerd. Als er geen geldig clientcertificaat is geïnstalleerd en de client probeert verbinding te maken met het VNet, mislukt de verificatie.

### <a name="getcer"></a>Stap 1: Het CER-bestand voor het basiscertificaat verkrijgen

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

### <a name="generateclientcert"></a>Stap2: Een clientcertificaat genereren
U kunt een uniek certificaat genereren voor elke client die verbinding maakt met een virtueel netwerk of u kunt hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid tot het intrekken van één certificaat, indien nodig. Anders moet u, als alle clients hetzelfde certificaat gebruiken en u het certificaat voor één van de clients moet intrekken, nieuwe certificaten genereren en installeren voor alle clients die het certificaat voor verificatie gebruiken.

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

## <a name="addresspool"></a>Deel 7: De clientadresgroep toevoegen
1. Nadat de virtuele netwerkgateway is gemaakt, gaat u naar de sectie **Instellingen** van de blade van de virtuele netwerkgateway. Klik in de sectie **Instellingen** op **Punt-naar-site-configuratie** om de blade **Configuratie** te openen.
   
    ![Blade Punt-naar-site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Adresgroep** is de groep met IP-adressen van waaruit clients die verbinding maken, een IP-adres ontvangen. Voeg de adresgroep toe en klik vervolgens op **Opslaan**.
   
    ![Clientadresgroep](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>Deel 8: Het CER-bestand van het basiscertificaat uploaden
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

## <a name="clientconfig"></a>Deel 9: Het configuratiepakket voor de VPN-client downloaden en installeren
Clients die met een P2S-verbinding maken met Azure, moeten zowel een certificaat als een VPN-clientconfiguratiepakket hebben geïnstalleerd. Configuratiepakketten voor VPN-clients zijn beschikbaar voor Windows-clients. 

Het VPN-clientpakket bevat gegevens voor de configuratie van de VPN-clientsoftware die is ingebouwd in Windows. De configuratie is specifiek voor de VPN waarmee u verbinding wilt maken. Met het pakket wordt geen andere software geïnstalleerd.

Op voorwaarde dat de versie overeenkomt met de architectuur van de client, kunt u hetzelfde configuratiepakket voor de VPN-client gebruiken op elke clientcomputer.

### <a name="step-1---download-the-client-configuration-package"></a>Stap 1: Download het configuratiepakket voor de VPN-client

1. Klik op de blade **Punt-naar-site-configuratie** op **VPN-client downloaden** om de blade **VPN-client downloaden** te openen. Het duurt enkele minuten om het pakket te genereren.
   
    ![VPN-client downloaden 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Selecteer het juiste pakket voor de client en klik vervolgens op **Downloaden**. Sla het configuratiepakket op. U moet dit installeren op elke clientcomputer die verbinding gaat maken met het virtuele netwerk.

    ![VPN-client downloaden 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-the-client-configuration-package"></a>Stap 2: Het configuratiepakket voor de client installeren

1. Kopieer het configuratiebestand lokaal op de computer die u wilt verbinden met het virtuele netwerk. 
2. Dubbelklik op het EXE-bestand om het pakket te installeren op de clientcomputer. Omdat u het configuratiepakket hebt gemaakt, is het niet ondertekend. Dit betekent dat er mogelijk een waarschuwing wordt weergegeven. Als u een Windows SmartScreen-melding ontvangt, klikt u op **Meer info** (links) en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
3. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De verbinding wordt nu vermeld. U ziet de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. Die ziet er ongeveer als volgt uit:
3. Installeer het pakket op de clientcomputer. Als u een Windows SmartScreen-melding ontvangt, klikt u op **Meer info** (links) en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
4. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De verbinding wordt nu vermeld. U ziet de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. Deze is vergelijkbaar met dit voorbeeld: 
   
    ![VPN-client](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png)


## <a name="installclientcert"></a>Deel 10 - Een geëxporteerd clientcertificaat installeren

Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat. 

1. Zoek het *PFX*-bestand en kopieer het naar de clientcomputer. Dubbelklik op de clientcomputer op het *PFX*-bestand om het te installeren. Laat de waarde voor **Archieflocatie** staan op **Huidige gebruiker** en klik op **Volgende**.
2. Laat de pagina **Te exporteren bestand** ongewijzigd. Klik op **Volgende**.
3. Voer op de pagina **Beveiliging met persoonlijke sleutel** het wachtwoord in voor het certificaat, als u een wachtwoord hebt ingesteld. Controleer anders of de beveiligings-principal die het certificaat installeert, juist is en klik vervolgens op **Volgende**.
4. Laat op de pagina **Certificaatarchief** de standaardlocatie staan en klik op **Volgende**.
5. Klik op **Voltooien**. Klik in het venster **Beveiligingswaarschuwing** voor de certificaatinstallatie op **Ja**. U kunt zonder problemen op Ja klikken omdat u zelf het certificaat hebt gegenereerd. Het certificaat wordt nu geïmporteerd.

## <a name="connect"></a>Deel 11: Verbinding maken met Azure
1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken. 
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.
   
    ![VPN-client maakt verbinding met Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)

    
3. Uw verbinding wordt nu tot stand gebracht.
   
    ![VPN-client verbonden met Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)
                                                                                                                                                                           

> [!NOTE]
> Als u een certificaat gebruikt dat is verleend met behulp van een oplossing voor een CA voor ondernemingen en problemen ondervindt bij de verificatie, controleert u de verificatievolgorde op het clientcertificaat. U kunt de volgorde van de verificatielijst controleren door op het clientcertificaat te dubbelklikken en naar **Details > Uitgebreid sleutelgebruik** te gaan. Controleer of Clientverificatie als het eerste item in de lijst wordt weergegeven. Als dat niet het geval is, moet u een clientcertificaat op basis van de gebruikerssjabloon verlenen waarin clientverificatie als het eerste item in de lijst wordt weergegeven. 
>
>

## <a name="verify"></a>Deel 12: De verbinding controleren
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

## <a name="add"></a>Vertrouwde basiscertificaten toevoegen of verwijderen
U kunt vertrouwde basiscertificaat toevoegen in en verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, kunnen de clientcertificaten die zijn gegenereerd op basis van het basiscertificaat geen verbinding meer maken met Azure via punt-naar-site. Als u wilt dat clients verbinding kunnen maken, moeten deze een nieuw clientcertificaat installeren dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Zie voor instructies de sectie [Een vertrouwd basiscertificaat uploaden](#uploadfile) in dit artikel.

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat verwijderen

1. Als u een vertrouwd basiscertificaat wilt verwijderen, gaat u naar de blade **Punt-naar-site-configuratie** voor uw virtuele netwerkgateway.
2. In het gedeelte **Basiscertificaat** van de blade, zoekt u het certificaat dat u wilt verwijderen.
3. Klik op het weglatingsteken naast het certificaat en vervolgens op Verwijderen.

## <a name="revokeclient"></a>Een clientcertificaat intrekken
U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Dit wijkt af van het verwijderen van een vertrouwd basiscertificaat. Als u een vertrouwd .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u in plaats van een basiscertificaat een clientcertificaat intrekt, kunt u de certificaten die zijn gegenereerd op basis van het basiscertificaat, blijven gebruiken voor verificatie van de punt-naar-site-verbinding.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="to-revoke-a-client-certificate"></a>Een clientcertificaat intrekken

U kunt een clientcertificaat intrekken door de vingerafdruk toe te voegen aan de intrekkingslijst.

1. Haal de vingerafdruk voor het clientcertificaat op. Zie voor meer informatie [De vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieer de gegevens naar een teksteditor en verwijder alle spaties, zodat u een doorlopende tekenreeks overhoudt.
3. Navigeer naar de blade **Punt-naar-site-configuratie** van de virtuele netwerkgateway. Dit is de blade die u hebt gebruikt voor het [uploaden van een vertrouwd basiscertificaat](#uploadfile).
4. In het gedeelte **Ingetrokken certificaten** voert u een beschrijvende naam in voor het certificaat (dit hoeft niet de CN van het certificaat te zijn).
5. Kopieer en plak de vingerafdruk naar het veld **Vingerafdruk**.
6. De vingerafdruk wordt gevalideerd en automatisch toegevoegd aan de intrekkingslijst. U ziet een bericht op het scherm met de melding dat de lijst wordt bijgewerkt. 
7. Wanneer het bijwerken is voltooid, kan het certificaat niet langer worden gebruikt om verbinding te maken. Clients die verbinding proberen te maken met het certificaat, ontvangen een bericht waarin wordt gemeld dat het certificaat niet langer geldig is.

## <a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/linux/azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.



