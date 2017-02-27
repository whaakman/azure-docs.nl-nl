---
title: Een punt-naar-site-VPN-gatewayverbinding met een Azure-netwerk configureren met behulp van de klassieke portal | Microsoft Docs
description: U maakt een beveiligde verbinding met uw Azure Virtual Network door een punt-naar-site-VPN-gatewayverbinding te maken.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 4f5668a5-9b3d-4d60-88bb-5d16524068e0
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cf72197aba2c6e6c7a51f96d1161cf1fbe88a0c5
ms.openlocfilehash: 149f3daf1f61f459b0a0834c0f112574510d5259


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Een punt-naar-site-verbinding met een VNet configureren met de klassieke portal
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> * [Klassiek - Klassieke portal](vpn-gateway-point-to-site-create.md)
> 
> 

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. Een P2S-verbinding is nuttig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk.

Voor punt-naar-site-verbindingen hebt u geen VPN-apparaat of openbaar IP-adres nodig. Er wordt een VPN-verbinding tot stand gebracht door de verbinding te starten vanaf de clientcomputer. Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het klassieke implementatiemodel met behulp van de klassieke portal.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en -methoden voor P2S-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Basiswerkstroom
![Punt-naar-site-schema](./media/vpn-gateway-point-to-site-create/p2sclassic.png "punt-naar-site")

De volgende stappen helpen u bij het maken van een beveiligde punt-naar-site-verbinding met een virtueel netwerk. 

De configuratie van een punt-naar-site-verbinding is onderverdeeld in vier secties. De volgorde waarin u elk van deze secties configureert, is belangrijk. Sla geen stappen of instructies over.

* **Sectie 1**: maak een virtueel netwerk en een VPN-gateway.
* **Sectie 2**: maak de certificaten die worden gebruikt voor verificatie en bij het uploaden ervan.
* **Sectie 3**: exporteer en installeer de clientcertificaten.
* **Sectie 4**: configureer de VPN-client.

## <a name="a-namevnetvpnasection-1---create-a-virtual-network-and-a-vpn-gateway"></a><a name="vnetvpn"></a>Sectie 1: Een virtueel netwerk en een VPN-gateway maken
### <a name="part-1-create-a-virtual-network"></a>Stap 1: Een virtueel netwerk maken
1. Meld u aan bij de [klassieke Azure Portal](https://manage.windowsazure.com). In deze stappen wordt gebruik gemaakt van de klassieke portal, niet Azure Portal. U kunt op dit moment geen P2S-verbinding maken met behulp van Azure Portal.
2. Klik linksonder in het scherm op **Nieuw**. Klik in het navigatievenster op **Netwerkservices** en vervolgens op **Virtueel netwerk**. Klik op **Aangepast item maken** om de configuratiewizard te starten.
3. Op de pagina **Details van virtueel netwerk** voert u de volgende informatie in en klikt u op de pijl Volgende in de rechterbenedenhoek.
   
   * **Naam**: geef het virtuele netwerk een naam. Bijvoorbeeld 'VNet1'. Dit is de naam die u gebruikt wanneer u virtuele machines in dit VNet implementeert.
   * **Locatie**: de locatie is direct gerelateerd aan de fysieke locatie (regio) waar u wilt dat de resources (VM's) zich bevinden. Selecteer bijvoorbeeld 'VS - oost' als u wilt dat de virtuele machines die u in dit virtuele netwerk implementeert, zich fysiek bevinden in 'VS - oost'. U kunt de regio die aan het virtuele netwerk is gekoppeld, niet meer wijzigen wanneer het netwerk is gemaakt.
4. Op de pagina **DNS-servers en VPN-connectiviteit** voert u de volgende informatie in en klikt u op de pijl Volgende in de rechterbenedenhoek.
   
   * **DNS-servers**: geef de naam en het IP-adres van de DNS-server op of selecteer een eerder geregistreerde DNS-server in het snelmenu. Met deze instelling wordt geen DNS-server gemaakt. U kunt hiermee de DNS-servers opgeven die u wilt gebruiken voor de naamomzetting voor dit virtuele netwerk. Laat deze sectie leeg als u de standaardservice voor naamomzetting van Azure wilt gebruiken.
   * **Punt-naar-site-VPN configureren**: schakel het selectievakje in.
5. Geef op de pagina **Punt-naar-site-connectiviteit** op uit welk IP-adresbereik uw VPN-clients een IP-adres ontvangen wanneer ze worden verbonden. Er zijn een paar regels met betrekking tot de adresbereiken die u kunt opgeven. Het is belangrijk dat u controleert of het bereik dat u opgeeft geen bereiken in het on-premises netwerk overlapt.
6. Voer de volgende informatie in en klik op de pijl Volgende.
   
   * **Adresruimte**: voeg het beginadres voor het IP-bereik en de CIDR (aantal adressen) toe.
   * **Adresruimte toevoegen**: voeg alleen een adresruimte toe als dit is vereist voor uw netwerkontwerp.
7. Op de pagina **Adresruimten voor virtueel netwerk** geeft u het adresbereik op dat u voor het virtuele netwerk wilt gebruiken. Dit zijn de dynamische IP-adressen (DIPS) die worden toegewezen aan de virtuele machines en andere rolinstanties die u in dit virtuele netwerk implementeert.<br><br>Het is vooral van belang dat u een bereik selecteert dat niet overlapt met een van de bereiken die voor uw on-premises netwerk worden gebruikt. U moet hiervoor afstemmen met de netwerkbeheerder, die mogelijk een bereik van IP-adressen in de adresruimte van uw on-premises netwerk moet reserveren voor het virtuele netwerk.
8. Voer de volgende informatie in en klik op het vinkje om het virtuele netwerk te gaan maken.
   
   * **Adresruimte**: Voeg het interne IP-adresbereik toe dat u voor dit virtuele netwerk wilt gebruiken, inclusief IP-beginadres en Aantal. Het is van belang dat u een bereik selecteert dat niet overlapt met een van de bereiken die voor uw on-premises netwerk worden gebruikt. 
   * **Subnet toevoegen**: U hebt geen aanvullende subnetten nodig, maar misschien wilt u een apart subnet maken voor virtuele machines die statische DIPS krijgen. Of misschien wilt u uw virtuele machines in een subnet plaatsen dat is gescheiden van uw andere rolinstanties.
   * **Gatewaysubnet toevoegen**: Het gatewaysubnet is vereist voor een punt-naar-site-VPN-verbinding. Klik om het gatewaysubnet toe te voegen. Het gatewaysubnet wordt alleen gebruikt voor de gateway van het virtueel netwerk.
9. Wanneer het virtuele netwerk is gemaakt, wordt op de pagina met netwerken in de klassieke Azure Portal **Gemaakt** vermeld onder **Status**. Wanneer het virtuele netwerk is gemaakt, kunt u de gateway voor dynamische routering maken.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Deel 2: Een gateway voor dynamische routering maken
Het gatewaytype moet worden geconfigureerd als Dynamisch. Statische routeringsgateways werken niet met deze functie.

1. Ga in de klassieke Azure Portal naar de pagina **Netwerken** en klik op het virtuele netwerk dat u hebt gemaakt. Navigeer dan naar de **Dashboard**-pagina.
2. Klik onderaan de **Dashboard**-pagina op **Gateway maken**. Er verschijnt een bericht met de vraag **Wilt u een gateway maken voor virtueel netwerk 'VNet1'**. Klik op **Ja** om de gateway te gaan maken. Het kan tot 45 minuten duren voordat de gateway is gemaakt.

## <a name="a-namegenerateasection-2---generate-and-upload-certificates"></a><a name="generate"></a>Sectie 2: Certificaten genereren en uploaden
Certificaten worden gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. U kunt een basiscertificaat gebruiken dat is gegenereerd door een commerciële certificeringsoplossing, of u kunt een zelfondertekend certificaat gebruiken. U kunt maximaal twintig basiscertificaten uploaden naar Azure. Zodra het CER-bestand is geüpload, kan Azure de informatie daarin gebruiken voor het verifiëren van clients met een geïnstalleerd clientcertificaat. Het clientcertificaat moet worden gegenereerd vanuit hetzelfde certificaat dat het CER-bestand vertegenwoordigt.

In deze sectie gaat u het volgende doen:

* Het CER-bestand voor een basiscertificaat verkrijgen. Dit kan een zelfondertekend certificaat zijn, of u kunt uw certificeringssysteem op ondernemingsniveau gebruiken.
* Het CER-bestand uploaden naar Azure.
* Clientcertificaten genereren.

### <a name="a-namerootapart-1-obtain-the-cer-file-for-the-root-certificate"></a><a name="root"></a>Deel 1: het CER-bestand voor het basiscertificaat verkrijgen
Als u een commercieel certificeringssysteem gebruikt, haalt u het CER-bestand voor het basiscertificaat op dat u wilt gebruiken. In [deel 3](#createclientcert) genereert u de clientcertificaten op basis van het basiscertificaat.

Als u geen commerciële certificeringsoplossing gebruikt, moet u een zelfondertekend basiscertificaat genereren. Voor de stappen in Windows 10 raadpleegt u [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Werken met zelfondertekende basiscertificaten voor punt-naar-site-configuraties). Het artikel helpt u makecert te gebruiken om een zelfondertekend certificaat te genereren. Hierna kunt u het CER-bestand exporteren.

### <a name="a-nameuploadapart-2-upload-the-root-certificate-cer-file-to-the-azure-classic-portal"></a><a name="upload"></a>Deel 2: Het CER-bestand van het basiscertificaat uploaden naar de klassieke Azure Portal
Voeg een vertrouwd certificaat toe aan Azure. Wanneer u een met Base64 gecodeerd X.509 (.cer)-bestand aan Azure toevoegt, geeft u bij Azure aan dat deze het basiscertificaat dat het bestand vertegenwoordigt, moet vertrouwen.

1. Ga in de klassieke Azure Portal naar de pagina **Certificaten** voor het virtuele netwerk en klik op **Een basiscertificaat uploaden**.
2. Blader op de pagina **Certificaat uploaden** naar het CER-basiscertificaat en klik op het vinkje.

### <a name="a-namecreateclientcertapart-3-generate-a-client-certificate"></a><a name="createclientcert"></a>Deel 3: Een clientcertificaat genereren
Nu gaat u de clientcertificaten genereren. U kunt een uniek certificaat genereren voor elke client die verbinding maakt of u kunt hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid tot het intrekken van één certificaat, indien nodig. Anders moet u, als alle clients hetzelfde certificaat gebruiken en u het certificaat voor één van de clients moet intrekken, nieuwe certificaten genereren en installeren voor alle clients die het certificaat gebruiken om te verifiëren.

####<a name="enterprise-certificate"></a>Commercieel certificaat
- Als u een commerciële certificeringsoplossing gebruikt, genereert u een clientcertificaat met de algemene waarde-indeling 'name@yourdomain.com',, in plaats van de indeling 'domeinnaam\gebruikersnaam'.
- Zorg ervoor dat het clientcertificaat dat u verleent is gebaseerd op de certificaatsjabloon voor Gebruiker met Clientverificatie als het eerste item in de lijst, in plaats van bijvoorbeeld smartcardaanmelding. U kunt het certificaat controleren door op het clientcertificaat te dubbelklikken en **Details > Uitgebreid sleutelgebruik** weer te geven.

####<a name="self-signed-certificate"></a>Zelfondertekend certificaat 
Als u een zelfondertekend certificaat gebruikt, raadpleegt u [Werken met zelfondertekende basiscertificaten voor punt-naar-site-configuraties](vpn-gateway-certificates-point-to-site.md) om een clientcertificaat te genereren.

## <a name="a-nameinstallclientcertasection-3---export-and-install-the-client-certificate"></a><a name="installclientcert"></a>Sectie 3: Het clientcertificaat exporteren en installeren
Installeer een clientcertificaat op elke computer die u met het virtueel netwerk wilt verbinden. Er is een clientcertificaat vereist voor verificatie. U kunt het installeren van het clientcertificaat automatiseren of u kunt het handmatig installeren. De volgende stappen leiden u door het handmatig exporteren en installeren van het clientcertificaat.

1. U kunt *certmgr.msc* gebruiken om een clientcertificaat te exporteren. Klik met de rechtermuisknop op het clientcertificaat dat u wilt exporteren, klik op **Alle taken** en vervolgens op **Exporteren**.
2. Exporteer het clientcertificaat met de persoonlijke sleutel. Dit is een *PFX*-bestand. Zorg dat u het wachtwoord (sleutel) dat u voor dit certificaat instelt, ergens noteert of onthoudt.
3. Kopieer het *PFX*-bestand naar de clientcomputer. Dubbelklik op de clientcomputer op het *PFX*-bestand om het te installeren. Voer het wachtwoord in wanneer hierom wordt gevraagd. Wijzig de installatielocatie niet.

## <a name="a-namevpnclientconfigasection-4---configure-your-vpn-client"></a><a name="vpnclientconfig"></a>Sectie 4: De VPN-client configureren
U moet ook een VPN-client configureren om verbinding te maken met het virtuele netwerk. De client vereist zowel een clientcertificaat als een correcte configuratie van de VPN-client om verbinding te kunnen maken. Voer de volgende stappen in de gegeven volgorde uit om een VPN-client te configureren.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Deel 1: Het configuratiepakket voor de VPN-client maken
1. Navigeer in de klassieke Azure Portal op de **Dashboard**-pagina voor het virtuele netwerk naar het menu snelle weergave in de rechterhoek. Het VPN-clientpakket bevat configuratiegegevens voor de configuratie van de VPN-clientsoftware die is ingebouwd in Windows. Met het pakket wordt geen andere software geïnstalleerd. De instellingen zijn specifiek voor het virtuele netwerk waarmee u verbinding wilt maken. Zie voor de lijst met ondersteunde clientbesturingssystemen de [Veelgestelde vragen over punt-naar-site-verbindingen](#faq) onderaan dit artikel.<br><br>Selecteer het downloadpakket dat overeenkomt met het besturingssysteem van de client waarop het wordt geïnstalleerd:
   
   * Voor 32-bits clients selecteert u **Het 32-bits VPN-clientpakket downloaden**.
   * Voor 64-bits clients selecteert u **Het 64-bits VPN-clientpakket downloaden**.
2. Het duurt enkele minuten voordat het clientpakket is gemaakt. Wanneer het pakket is voltooid, kunt u het bestand downloaden. Het *EXE*-bestand dat u hebt gedownload, kan veilig worden opgeslagen op de lokale computer.
3. Nadat u het VPN-clientpakket via de klassieke Azure Portal hebt gegenereerd en geïnstalleerd, kunt u het installeren op de clientcomputer van waaruit u verbinding wilt maken met het virtuele netwerk. Als u het VPN-clientpakket op meerdere clientcomputers wilt installeren, moet op elke computer een clientcertificaat worden geïnstalleerd.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Deel 2: Het VPN-configuratiepakket installeren op de client
1. Kopieer het configuratiebestand lokaal op de computer die u wilt verbinden met het virtuele netwerk en dubbelklik op het EXE-bestand. 
2. Wanneer het pakket is geïnstalleerd, kunt u de VPN-verbinding starten. Het configuratiepakket is niet ondertekend door Microsoft. U kunt het pakket desgewenst laten ondertekenen door de ondertekeningsservice van uw organisatie of het zelf ondertekenen met behulp van [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327). U kunt het pakket echter gebruiken zonder het te ondertekenen. Als het pakket niet is ondertekend, wordt echter een waarschuwing weergegeven wanneer u het pakket installeert.
3. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De verbinding wordt nu vermeld. U ziet de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. Die ziet er ongeveer als volgt uit: 
   
    ![VPN-client](./media/vpn-gateway-point-to-site-create/vpn.png "VPN-client")

### <a name="part-3-connect-to-azure"></a>Deel 3: Verbinding maken met Azure
1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken. 
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.
   
    ![VPN-client 2](./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN-clientverbinding")
3. Uw verbinding wordt nu tot stand gebracht.
   
    ![VPN-client 3](./media/vpn-gateway-point-to-site-create/connected.png "VPN-clientverbinding 2")

> [!NOTE]
> Als u een certificaat gebruikt dat is verleend met behulp van een oplossing voor een CA voor ondernemingen en problemen ondervindt bij de verificatie, controleert u de verificatievolgorde op het clientcertificaat. U kunt de volgorde van de verificatielijst controleren door op het clientcertificaat te dubbelklikken en naar **Details > Uitgebreid sleutelgebruik** te gaan. Controleer of Clientverificatie als het eerste item in de lijst wordt weergegeven. Als dat niet het geval is, moet u een clientcertificaat op basis van de gebruikerssjabloon verlenen waarin clientverificatie als het eerste item in de lijst wordt weergegeven. 
>
>

### <a name="part-4-verify-the-vpn-connection"></a>Deel 4: De VPN-verbinding controleren
1. Als u wilt controleren of uw VPN-verbinding actief is, opent u een opdrachtprompt met verhoogde bevoegdheid en voert u *ipconfig/all* in.
2. Bekijk de resultaten. Het IP-adres dat u hebt ontvangen, is een van de adressen binnen het adresbereik van de punt-naar-site-verbinding dat u hebt opgegeven tijdens het maken van het VNet. De resultaten moeten er ongeveer als volgt uitzien:

Voorbeeld:

    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="a-namefaqapoint-to-site-faq"></a><a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.

Bekijk de pagina [Virtual Network Documentation](/azure/virtual-network) (Virtual Network-documentatie) voor meer informatie over Virtual Networks.




<!--HONumber=Feb17_HO3-->


