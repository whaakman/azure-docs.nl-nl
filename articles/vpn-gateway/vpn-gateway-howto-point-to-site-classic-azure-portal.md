---
title: 'Met een computer verbinding maken met een virtueel Azure-netwerk met behulp van punt-naar-site: klassieke Azure-portal | Microsoft Docs'
description: U maakt een beveiligde verbinding met uw klassieke Azure Virtual Network door een punt-naar-site-VPN-gatewayverbinding te maken met Azure Portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 92e573d7f3ebfbe41c8012068a8262d6fc324da8
ms.lasthandoff: 03/21/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal-classic"></a>Een punt-naar-site-verbinding met een VNet configureren met Azure Portal (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Punt-naar-site-verbindingen zijn handig als u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk. Voor P2S-verbindingen hebt u geen VPN-apparaat of een openbaar IP-adres nodig. U brengt de VPN-verbinding tot stand vanaf de clientcomputer.

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het klassieke implementatiemodel met behulp van Azure Portal. Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Implementatiemodellen en -methoden voor P2S-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Basiswerkstroom
![Punt-naar-site-diagram](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

De volgende secties helpen u bij het maken van een beveiligde punt-naar-site-verbinding met een virtueel netwerk.

1. Een virtueel netwerk en een VPN-gateway maken
2. Certificaten genereren
3. Het .cer-bestand uploaden
4. Het configuratiepakket voor de VPN-client genereren
5. De clientcomputer configureren
6. Verbinding maken met Azure

### <a name="example-settings"></a>Voorbeeldinstellingen
U kunt de volgende voorbeeldinstellingen gebruiken:

* **Naam: VNet1**
* **Adresruimte: 192.168.0.0/16**<br>In dit voorbeeld gebruiken we slechts één adresruimte. U kunt meer dan één adresruimte voor uw VNet hebben.
* **Subnetnaam: FrontEnd**
* **Subnetadresbereik: 192.168.1.0/24**
* **Abonnement:** controleer of u het juiste abonnement hebt in het geval u er meer dan één hebt.
* **Resourcegroep: TestRG**
* **Locatie: VS - oost**
* **Verbindingstype: punt-naar-site-verbinding**
* **Clientadresruimte: 172.16.201.0/24**. VPN-clients die verbinding maken met het VNet via deze punt-naar-site-verbinding, ontvangen een IP-adres van de opgegeven pool.
* **GatewaySubnet: 192.168.200.0/24**. Het gatewaysubnet moet de naam GatewaySubnet gebruiken.
* **Grootte:** selecteer de gateway-SKU die u wilt gebruiken.
* **Routeringstype: dynamisch**



## <a name="vnetvpn"></a>Sectie 1: Een virtueel netwerk en een VPN-gateway maken

Controleer eerst of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).
### <a name="createvnet"></a>Deel 1: Een virtueel netwerk maken
Als u nog geen virtueel netwerk hebt, maakt u er een. De schermafbeeldingen dienen alleen als voorbeeld. Zorg dat u de waarden vervangt door die van uzelf. Volg de volgende stappen om een VNet te maken met behulp van Azure Portal:

1. Navigeer via een browser naar de [Azure Portal](http://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op **Nieuw**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **virtueel netwerk** in de geretourneerde lijst en klik hierop om de resourceblade **Virtueel netwerk** te openen.

    ![Een virtuele netwerkblade zoeken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Kies uit de lijst **Selecteer een implementatiemodel** (aan de onderzijde van de blade Virtueel netwerk) de optie **Klassiek** en klik vervolgens op **Maken**.

    ![Een implementatiemodel selecteren](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. Configureer de VNet-instellingen op de blade **virtueel netwerk aanmaken**. Voeg uw eerste adresruimte en één adresbereik voor het subnet toe aan deze blade. Nadat u het VNet hebt aangemaakt, kunt u terugkeren en aanvullende subnets en adresruimten toevoegen.

    ![Een virtuele netwerkblade aanmaken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Controleer of het **Abonnement** het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
6. Klik op **Resourcegroep** en selecteer een bestaande resourcegroep of maak een nieuwe aan door een naam in te voeren voor uw nieuwe resourcegroep. Geef de resourcegroep een naam die aansluit bij de geplande configuratiewaarden wanneer u een nieuwe groep aanmaakt. Zie voor meer informatie over resourcegroepen [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Selecteer vervolgens de **Locatie**-instellingen voor uw VNet. De locatie bepaalt waar de resources die u in dit VNet implementeert, worden opgeslagen.
8. Selecteer **Vastmaken aan dashboard** als u uw VNet gemakkelijk wilt terugvinden op het dashboard en klik vervolgens op **Aanmaken**.

    ![Vastmaken aan dashboard](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Nadat u op Aanmaken klikt, ziet u een tegel op uw dashboard die de voortgang van uw VNet aangeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.

    ![Tegel Virtueel netwerk maken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Nadat u uw virtuele netwerk hebt gemaakt, kunt u het IP-adres van een DNS-server toevoegen om de naamomzetting te verwerken. Open de instellingen voor het virtuele netwerk, klik op de DNS-servers en voeg het IP-adres toe van de DNS-server die u wilt gebruiken. Met deze instelling wordt geen nieuwe DNS-server gemaakt. Zorg ervoor dat u een DNS-server toevoegt waarmee uw resources kunnen communiceren.

Wanneer het virtuele netwerk is gemaakt, wordt op de pagina met netwerken in de klassieke Azure Portal **Gemaakt** vermeld onder **Status**.

### <a name="gateway"></a>Deel 2: Een gatewaysubnet en een gateway voor dynamische routering maken
In deze stap maakt u een gatewaysubnet en een gateway voor dynamische routering. Als u in Azure Portal het gatewaysubnet en de gateway maakt voor het klassieke implementatiemodel, kunt u dit doen via dezelfde configuratieblades.

1. Navigeer in de portal naar het virtuele netwerk waarvoor u een gateway wil maken.
2. Ga op de blade voor het virtuele netwerk naar de blade **Overzicht** en klik in de sectie VPN-verbindingen op **Gateway**.

    ![Klik om een gateway te maken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. Selecteer op de blade **Nieuwe VPN-verbinding** de optie **Punt-naar-site**.

    ![Verbindingstype punt-naar-site](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. Voeg voor **Clientadresruimte** het IP-adresbereik toe. Dit is het bereik van waaruit de VPN-clients een IP-adres ontvangen wanneer er verbinding wordt gemaakt. Verwijder het automatisch ingevulde bereik en voeg uw eigen bereik toe.

    ![Clientadresruimte](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Selecteer het selectievakje **Gateway onmiddellijk maken**.

    ![Gateway onmiddellijk maken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Klik op **Optionele gatewayconfiguratie** om de blade **Gatewayconfiguratie** te openen.

    ![Klik op Optionele gatewayconfiguratie](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Klik op **Vereiste subnetinstellingen configureren** om het **Gatewaysubnet** toe te voegen. Het is mogelijk om een klein gatewaysubnet van /29 te maken, maar we raden u aan een groter subnet met meer adressen te maken door ten minste /28 of /27 te selecteren. Hierdoor hebt u genoeg adressen voor mogelijke aanvullende toekomstige configuraties.

   > [!IMPORTANT]
   > Als u met gatewaysubnetten werkt, vermijd dan om een netwerkbeveiligingsgroep (NSG) te koppelen aan het gatewaysubnet. Een koppeling van een netwerkbeveiligingsgroep naar dit subnet zorgt er mogelijk voor dat uw VPN-gateway niet meer werkt zoals verwacht. Raadpleeg voor meer informatie over netwerkbeveiligingsgroepen [Wat is een netwerkbeveiligingsgroep?](../virtual-network/virtual-networks-nsg.md)
   >
   >

    ![GatewaySubnet toevoegen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Selecteer de gateway **Grootte**. Dit is de gateway-SKU die u gaat gebruiken om uw virtuele netwerkgateway te maken. In de portal is de standaard-SKU **Basic**. Zie [Informatie over VPN-gatewayinstellingen](vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor informatie over gateway-SKU's.

    ![grootte van de gateway](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Selecteer het **Routeringstype** voor uw gateway. P2S-configuraties vereisen een **Dynamisch** routeringstype. Klik op **OK** wanneer u klaar bent met het configureren van deze blade.

    ![Routeringstype configureren](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. Klik op de blade **Nieuwe VPN-verbinding** op **OK** (onder aan de blade) om te beginnen met het maken van uw virtuele netwerkgateway. Dit kan maximaal 45 minuten duren.

## <a name="generatecerts"></a>Sectie 2 - Certificaten maken
Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. Nadat u het basiscertificaat hebt gemaakt, exporteert u de gegevens van het openbare certificaat (niet de persoonlijke sleutel) als een X.509 CER-bestand dat is gecodeerd met Base64. Vervolgens kunt u de gegevens van het openbare certificaat uploaden naar Azure.

Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. Het clientcertificaat wordt gegenereerd op basis van het basiscertificaat en wordt op elke clientcomputer geïnstalleerd. Als er geen geldig clientcertificaat is geïnstalleerd en de client probeert verbinding te maken met het VNet, mislukt de verificatie.

### <a name="cer"></a>Deel 1: De openbare sleutel (.cer) voor het basiscertificaat verkrijgen

####<a name="enterprise-certificate"></a>Commercieel certificaat
 
Als u gebruikmaakt van een bedrijfsoplossing kunt u de bestaande certificaatketen gebruiken. Verkrijg het CER-bestand dat u wilt gebruiken voor het basiscertificaat.

####<a name="self-signed-root-certificate"></a>Zelfondertekend basiscertificaat

Als u geen commerciële certificeringsoplossing gebruikt, moet u een zelfondertekend basiscertificaat maken. Als u een zelfondertekend certificaat wilt maken dat de vereiste velden voor P2S-verificatie bevat, kunt u PowerShell gebruiken. Zie [Create a self-signed certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Een zelfondertekend certificaat voor punt-naar-site-verbindingen maken) voor een stapsgewijze procedure om een zelfondertekend basiscertificaat te maken.

> [!NOTE]
> Eerder was makecert de aanbevolen methode voor het maken van zelfondertekende basiscertificaten en het genereren van clientcertificaten voor punt-naar-site-verbindingen. U kunt nu PowerShell gebruiken om deze certificaten te maken. Een van de voordelen van PowerShell is dat u SHA-2-certificaten kunt maken. Zie [Create a self-signed certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Een zelfondertekend certificaat voor punt-naar-site-verbindingen maken met PowerShell) voor de vereiste waarden.
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>De openbare sleutel voor een zelfondertekend basiscertificaat exporteren

Punt-naar-site-verbindingen werken alleen als de openbare sleutel (.cer) is geüpload naar Azure. Voer de volgende stappen uit om het CER-bestand voor het zelfondertekende basiscertificaat te exporteren.

1. Als u een CER-bestand wilt genereren van het certificaat, opent u **certmgr.msc**. Zoek het zelfondertekende basiscertificaat, meestal in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten', en klik er met de rechtermuisknop op. Klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.
2. Klik in de wizard op **Volgende**. Selecteer **Nee, de persoonlijke sleutel niet exporteren** en klik vervolgens op **Volgende**.
3. Selecteer op de pagina **Bestandsindeling voor export** de optie **Met Base64 gecodeerde X.509 (*.CER)** en klik op **Volgende**. 
4. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.
5. Klik op **Voltooien** om het certificaat te exporteren. U ziet een bericht dat****de export is gelukt. Klik op **OK** om de wizard te sluiten.

### <a name="genclientcert"></a>Deel 2: Een clientcertificaat genereren

U kunt een uniek certificaat genereren voor elke client die verbinding maakt of u kunt hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid tot het intrekken van één certificaat, indien nodig. Anders moet u, als alle clients hetzelfde certificaat gebruiken en u het certificaat voor één van de clients moet intrekken, nieuwe certificaten genereren en installeren voor alle clients die het certificaat voor verificatie gebruiken.

####<a name="enterprise-certificate"></a>Commercieel certificaat
- Als u een commerciële certificeringsoplossing gebruikt, genereert u een clientcertificaat met de algemene waarde-indeling 'name@yourdomain.com', in plaats van de indeling 'domeinnaam\gebruikersnaam'.
- Zorg ervoor dat het clientcertificaat dat u verleent is gebaseerd op de certificaatsjabloon voor Gebruiker met Clientverificatie als het eerste item in de lijst, in plaats van bijvoorbeeld smartcardaanmelding. U kunt het certificaat controleren door op het clientcertificaat te dubbelklikken en **Details > Uitgebreid sleutelgebruik** weer te geven.

####<a name="self-signed-root-certificate"></a>Zelfondertekend basiscertificaat 
Als u een zelfondertekend basiscertificaat gebruikt, raadpleegt u [Generate a client certificate](vpn-gateway-certificates-point-to-site.md#clientcert) (Een clientcertificaat genereren) voor de stappen waarmee u een clientcertificaat kunt genereren dat compatibel is met punt-naar-site-verbindingen.

### <a name="exportclientcert"></a>Deel 3: Het clientcertificaat exporteren
Als u aan de hand van de [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert)-instructies een clientcertificaat genereert van een zelfondertekend basiscertificaat, wordt het certificaat automatisch geïnstalleerd op de computer die u hebt gebruikt om het certificaat te genereren. Als u een clientcertificaat op een andere clientcomputer wilt installeren, moet u het certificaat exporteren.

1. Open **certmgr.msc** om een clientcertificaat te exporteren. Klik met de rechtermuisknop op het clientcertificaat dat u wilt exporteren, klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.
2. Klik in de wizard op **Volgende**, selecteer **Ja, de persoonlijke sleutel exporteren** en klik vervolgens op **Volgende**.
3. Laat op de pagina **Bestandsindeling voor export** de standaardinstellingen geselecteerd. Zorg ervoor dat **en mogelijk alle certificaten in het certificeringspad opnemen** is geselecteerd. Klik op **Volgende**.
4. Op de pagina **Beveiliging** moet u de persoonlijke sleutel beveiligen. Als u ervoor kiest om een wachtwoord te gebruiken, is het belangrijk dat u het wachtwoord voor dit certificaat ergens noteert of onthoudt. Klik op **Volgende**.
5. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.
6. Klik op **Voltooien** om het certificaat te exporteren.

## <a name="upload"></a>Sectie 3: Het CER-bestand van het basiscertificaat uploaden
Nadat de gateway is gemaakt, kunt u het .cer-bestand voor een vertrouwd basiscertificaat uploaden naar Azure. U kunt bestanden voor maximaal twintig basiscertificaten uploaden. U uploadt de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het .cer-bestand is geüpload, gebruikt Azure dit voor het verifiëren van clients die verbinding maken met het virtuele netwerk.

1. Klik in het gedeelte **VPN-verbindingen** van de blade voor uw VNet op de afbeelding **clients** om de blade **Punt-naar-site VPN-verbinding** te openen.

    ![Clients](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Klik op de blade **Punt-naar-site-verbinding** op **Certificaten beheren** om de blade **Certificaten** te openen.<br>

    ![Blade Certificaten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Klik op de blade **Certificaten** op **Uploaden** om de blade **Certificaat uploaden** te openen.<br>

    ![Blade Certificaten uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Klik op de afbeelding van een map om het .cer-bestand te zoeken. Selecteer het bestand en klik vervolgens op **OK**. Vernieuw de pagina om het geüploade certificaat weer te geven op de blade **Certificaten**.

    ![Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>Sectie 4: Het configuratiepakket voor de VPN-client genereren
U moet ook een VPN-client configureren om verbinding te maken met het virtuele netwerk. De clientcomputer vereist zowel een clientcertificaat als een correct configuratiepakket van de VPN-client om verbinding te kunnen maken.

Het VPN-clientpakket bevat configuratiegegevens voor de configuratie van de VPN-clientsoftware die is ingebouwd in Windows. Met het pakket wordt geen andere software geïnstalleerd. De instellingen zijn specifiek voor het virtuele netwerk waarmee u verbinding wilt maken. Zie voor de lijst met ondersteunde clientbesturingssystemen de [Veelgestelde vragen over punt-naar-site-verbindingen](#faq) onderaan dit artikel.

### <a name="to-generate-the-vpn-client-configuration-package"></a>Het configuratiepakket voor de VPN-client genereren
1. Klik in Azure Portal op de blade **Overzicht** voor uw VNet in **VPN-verbindingen** op de clientafbeelding om de blade **Punt-naar-site VPN-verbinding** te openen.
2. Klik boven aan de blade **Punt-naar-site VPN-verbinding** op het downloadpakket dat overeenkomt met het besturingssysteem van de client waarop het wordt geïnstalleerd:

   * Selecteer voor 64-bits clients **VPN-clients (64-bits)**.
   * Selecteer voor 32-bits clients **VPN-clients (32-bits)**.

     ![Het configuratiepakket voor de VPN-client downloaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. U ziet een bericht dat meldt dat Azure het configuratiepakket voor de VPN-client genereert voor het virtuele netwerk. Het pakket wordt binnen enkele minuten gegenereerd en er wordt op de lokale computer een bericht weergegeven om te melden dat het pakket is gedownload. Sla het configuratiepakket op. U moet dit installeren u op elke clientcomputer die via P2S verbinding gaat maken met het virtuele netwerk.

## <a name="clientconfiguration"></a>Sectie 5: De clientcomputer configureren
### <a name="part-1-install-an-exported-client-certificate"></a>Deel 1 - Een geëxporteerd clientcertificaat installeren

Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat.

1. Zoek het *PFX*-bestand en kopieer het naar de clientcomputer. Dubbelklik op de clientcomputer op het *PFX*-bestand om het te installeren. Laat de waarde voor **Archieflocatie** staan op **Huidige gebruiker** en klik op **Volgende**.
2. Laat de pagina **Te exporteren bestand** ongewijzigd. Klik op **Volgende**.
3. Voer op de pagina **Beveiliging met persoonlijke sleutel** het wachtwoord in voor het certificaat, als u een wachtwoord hebt ingesteld. Controleer anders of de beveiligings-principal die het certificaat installeert, juist is en klik vervolgens op **Volgende**.
4. Laat op de pagina **Certificaatarchief** de standaardlocatie staan en klik op **Volgende**.
5. Klik op **Voltooien**. Klik in het venster **Beveiligingswaarschuwing** voor de certificaatinstallatie op **Ja**. U kunt zonder problemen op Ja klikken omdat u zelf het certificaat hebt gegenereerd. Het certificaat wordt nu geïmporteerd.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Deel 2: Het configuratiepakket voor de VPN-client installeren
Op voorwaarde dat de versie overeenkomt met de architectuur van de client, kunt u hetzelfde configuratiepakket voor de VPN-client gebruiken op elke clientcomputer.

1. Kopieer het configuratiebestand lokaal op de computer die u wilt verbinden met het virtuele netwerk. 
2. Dubbelklik op het EXE-bestand om het pakket te installeren op de clientcomputer. Omdat u het configuratiepakket hebt gemaakt, is het niet ondertekend. Dit betekent dat er mogelijk een waarschuwing wordt weergegeven. Als u een Windows SmartScreen-melding ontvangt, klikt u op **Meer info** (links) en klikt u vervolgens op **Toch uitvoeren** om het pakket te installeren.
3. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De verbinding wordt nu vermeld. U ziet de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. Die ziet er ongeveer als volgt uit:

    ![VPN-client](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png)

## <a name="connect"></a>Sectie 6: Verbinding maken met Azure
### <a name="connect-to-your-vnet"></a>Verbinding maken met uw VNet
1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken.
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.

    ![VPN-clientverbinding](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Uw verbinding wordt nu tot stand gebracht.

    ![Tot stand gebrachte verbinding](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

> [!NOTE]
> Als u een certificaat gebruikt dat is verleend met behulp van een oplossing voor een CA voor ondernemingen en problemen ondervindt bij de verificatie, controleert u de verificatievolgorde op het clientcertificaat. U kunt de volgorde van de verificatielijst controleren door op het clientcertificaat te dubbelklikken en naar **Details > Uitgebreid sleutelgebruik** te gaan. Controleer of Clientverificatie als het eerste item in de lijst wordt weergegeven. Als dat niet het geval is, moet u een clientcertificaat op basis van de gebruikerssjabloon verlenen waarin clientverificatie als het eerste item in de lijst wordt weergegeven. 
>
>

### <a name="verify-the-vpn-connection"></a>De VPN-verbinding controleren
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

## <a name="add"></a>Vertrouwde basiscertificaten toevoegen of verwijderen

U kunt vertrouwde basiscertificaat toevoegen in en verwijderen uit Azure. Wanneer u een vertrouwd certificaat verwijdert, kunnen de clientcertificaten die zijn gegenereerd op basis van het basiscertificaat geen verbinding meer maken met Azure via punt-naar-site. Als u wilt dat clients verbinding kunnen maken, moeten deze een nieuw clientcertificaat installeren dat is gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Zie [Sectie 3: Het CER-bestand van het basiscertificaat uploaden](#upload) voor instructies.

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat verwijderen


1. Klik in het gedeelte **VPN-verbindingen** van de blade voor uw VNet op de afbeelding **clients** om de blade **Punt-naar-site VPN-verbinding** te openen.

    ![Clients](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Klik op de blade **Punt-naar-site-verbinding** op **Certificaten beheren** om de blade **Certificaten** te openen.<br>

    ![Blade Certificaten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Op de blade **Certificaten** klikt u op de knop met weglatingstekens naast het certificaat dat u wilt verwijderen. Klik vervolgens op **Verwijderen**.

     ![Basiscertificaat verwijderen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>


## <a name="revokeclient"></a>Een clientcertificaat intrekken
U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Dit wijkt af van het verwijderen van een vertrouwd basiscertificaat. Als u een vertrouwd .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u in plaats van een basiscertificaat een clientcertificaat intrekt, kunt u de certificaten die zijn gegenereerd op basis van het basiscertificaat, blijven gebruiken voor verificatie van de punt-naar-site-verbinding.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="to-revoke-a-client-certificate"></a>Een clientcertificaat intrekken

U kunt een clientcertificaat intrekken door de vingerafdruk toe te voegen aan de intrekkingslijst.

1. Haal de vingerafdruk voor het clientcertificaat op. Zie voor meer informatie [De vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieer de gegevens naar een teksteditor en verwijder alle spaties, zodat u een doorlopende tekenreeks overhoudt.
3. Navigeer naar de blade **'klassieke virtuele-netwerknaam' > Punt-naar-site VPN-verbinding > Certificaten** en klik vervolgens op **Intrekkingslijst** om de blade Intrekkingslijst te openen. 
4. Op de blade **Intrekkingslijst** klikt u op **+Certificaat toevoegen** om de blade **Certificaat toevoegen aan intrekkingslijst** te openen.
5. Op de blade **Certificaat toevoegen aan intrekkingslijst** plakt u de vingerafdruk van het certificaat als een doorlopende regel tekst, zonder spaties. Klik op **OK** onder aan de blade.
6. Wanneer het bijwerken is voltooid, kan het certificaat niet langer worden gebruikt om verbinding te maken. Clients die verbinding proberen te maken met het certificaat, ontvangen een bericht waarin wordt gemeld dat het certificaat niet langer geldig is.


## <a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.

