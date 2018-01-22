---
title: 'Via een computer verbinding maken met een virtueel netwerk met behulp van een punt-naar-site-verbinding en verificatie via een certificaat: klassieke Azure Portal | Microsoft Docs'
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
ms.date: 01/17/2018
ms.author: cherylmc
ms.openlocfilehash: 150b6fcc80a57c0cded110e19cf81f5a2883e583
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-certificate-authentication-classic-azure-portal"></a>Een punt-naar-site-verbinding naar een VNet configureren met behulp van verificatie via een certificaat (klassiek): Azure Portal

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het klassieke implementatiemodel met behulp van Azure Portal. Deze configuratie maakt gebruik van certificaten om de client te verifiëren waarmee verbinding wordt gemaakt. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Met een punt-naar-site-VPN-gateway (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele netwerk. P2S-verbindingen zijn nuttig als u verbinding wilt maken met uw VNet vanaf een externe locatie, bijvoorbeeld als u ook thuis werkt of op een congres verbinding wilt maken. Een P2S-VPN is ook een uitstekende oplossing in plaats van een site-naar-site-VPN wanneer u maar een paar clients hebt die verbinding moeten maken met een VNet. Een P2S-verbinding wordt tot stand gebracht door deze te starten vanaf de clientcomputer.

> [!IMPORTANT]
> Het klassieke implementatiemodel ondersteunt alleen Windows VPN-clients en maakt gebruik van het SSTP-protocol (Secure Socket Tunneling Protocol), een VPN-protocol op basis van SSL. Voor de ondersteuning van niet-Windows VPN-clients, moet uw VNet worden gemaakt met het implementatiemodel van Resource Manager. Het Resource Manager-implementatiemodel biedt naast ondersteuning voor SSTP ook ondersteuning voor het VPN-type IKEv2. Zie [Informatie over P2S-verbindingen](point-to-site-about.md) voor meer informatie.
>
>

![Punt-naar-site-diagram](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)


Voor punt-naar-site-verbindingen met verificatie via een certificaat is het volgende vereist:

* Een dynamische VPN-gateway.
* De openbare sleutel (CER-bestand) voor een basiscertificaat dat is geüpload naar Azure. Dit wordt beschouwd als een vertrouwd certificaat en wordt gebruikt voor verificatie.
* Op basis van het basiscertificaat wordt een clientcertificaat gegenereerd voor installatie op elke clientcomputer die wordt verbonden. Dit certificaat wordt gebruikt voor clientverificatie.
* Op elke clientcomputer die wordt verbonden, moet bovendien een VPN-clientconfiguratiepakket worden gegenereerd en geïnstalleerd. Het clientconfiguratiepakket configureert de systeemeigen VPN-client die zich al op het systeem bevindt, met de benodigde informatie om verbinding te maken met het VNet.

Punt-naar-site-verbindingen hebben geen VPN-apparaat of on-premises openbaar IP-adres nodig. De VPN-verbinding is gemaakt via SSTP (Secure Socket Tunneling Protocol). Wij ondersteunen SSTP versies 1.0, 1.1 en 1.2 aan de serverzijde. De client besluit welke versie moet worden gebruikt. Voor Windows 8.1 en hoger, gebruikt SSTP standaard 1.2. 

Voor meer informatie over punt-naar-site-verbindingen leest u de [Veelgestelde vragen over punt-naar-site](#faq) onder aan dit artikel.

### <a name="example-settings"></a>Voorbeeldinstellingen

U kunt de volgende waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel:

* **Naam: VNet1**
* **Adresruimte: 192.168.0.0/16**<br>In dit voorbeeld gebruiken we slechts één adresruimte. Zoals u in het diagram ziet, kunt u meer dan één adresruimte voor uw VNet hebben.
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

## <a name="vnetvpn"></a>1. Een virtueel netwerk en een VPN-gateway maken

Controleer eerst of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).

### <a name="createvnet"></a>Deel 1: Een virtueel netwerk maken

Als u nog geen virtueel netwerk hebt, maakt u er een. De schermafbeeldingen dienen alleen als voorbeeld. Zorg dat u de waarden vervangt door die van uzelf. Volg de volgende stappen om een VNet te maken met behulp van Azure Portal:

1. Navigeer via een browser naar de [Azure Portal](http://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op **Nieuw**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **Virtual Network** in de lijst met resultaten en klik erop om de pagina **Virtual Network** te openen.

  ![Pagina voor zoeken van virtueel netwerk](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Kies onder aan de pagina Virtual Network de optie **Klassiek** in de lijst **Selecteer een implementatiemodel** en klik vervolgens op **Maken**.

  ![Een implementatiemodel selecteren](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken**. Voeg op deze pagina de eerste adresruimte en één adresbereik voor het subnet toe. Nadat u het VNet hebt aangemaakt, kunt u terugkeren en aanvullende subnets en adresruimten toevoegen.

  ![De pagina Virtueel netwerk maken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Controleer of het **Abonnement** het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
6. Klik op **Resourcegroep** en selecteer een bestaande resourcegroep of maak een nieuwe aan door een naam in te voeren voor uw nieuwe resourcegroep. Geef de resourcegroep een naam die aansluit bij de geplande configuratiewaarden wanneer u een nieuwe resourcegroep aanmaakt. Zie [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
7. Selecteer vervolgens de **Locatie**-instellingen voor uw VNet. De locatie bepaalt waar de resources die u naar dit VNet implementeert, worden opgeslagen.
8. Selecteer **Vastmaken aan dashboard** als u uw VNet gemakkelijk wilt terugvinden op het dashboard en klik vervolgens op **Aanmaken**.

  ![Vastmaken aan dashboard](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Nadat u op Aanmaken hebt geklikt, verschijnt er een tegel op uw dashboard die de voortgang van uw VNet aangeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.

  ![Tegel Virtueel netwerk maken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Nadat het virtuele netwerk is gemaakt, wordt **Gemaakt** weergegeven.
11. Voeg een DNS-server toe (optioneel). Nadat u uw virtuele netwerk hebt gemaakt, kunt u het IP-adres van een DNS-server voor naamomzetting toevoegen. Het IP-adres dat u opgeeft voor de DNS-server, moet het adres zijn van een DNS-server die de namen van de resources in uw VNet kan omzetten.<br>Open de instellingen voor het virtuele netwerk, klik op de DNS-servers en voeg het IP-adres toe van de DNS-server die u wilt gebruiken om een DNS-server toe te voegen.

### <a name="gateway"></a>Deel 2: Een gatewaysubnet en een gateway voor dynamische routering maken

In deze stap maakt u een gatewaysubnet en een gateway voor dynamische routering. Als u in Azure Portal het gatewaysubnet en de gateway maakt voor het klassieke implementatiemodel, kunt u dit doen via dezelfde configuratiepagina's. Het gatewaysubnet wordt alleen gebruikt voor de gatewayservices. Daarom moet u nooit zaken rechtstreeks implementeren in het gatewaysubnet, zoals virtuele machines of andere services.

1. Navigeer in de portal naar het virtuele netwerk waarvoor u een gateway wil maken.
2. Ga op de pagina voor het virtuele netwerk naar de pagina **Overzicht** en klik in de sectie VPN-verbindingen op **Gateway**.

  ![Klik om een gateway te maken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. Selecteer op de pagina **Nieuwe VPN-verbinding** de optie **Punt-naar-site**.

  ![Verbindingstype punt-naar-site](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. Voeg voor **Clientadresruimte** het IP-adresbereik toe. Dit is het bereik van waaruit de VPN-clients een IP-adres ontvangen wanneer er verbinding wordt gemaakt. Gebruik een privé-IP-adresbereik dat niet overlapt met de on-premises locatie waarvanaf u verbinding maakt of met het VNet waarmee u verbinding wilt maken. U kunt het automatisch ingevulde bereik verwijderen en daarna het privé-IP-adresbereik toevoegen dat u wilt gebruiken. In dit voorbeeld ziet u de automatisch ingevulde variabele. Verwijderen deze om de gewenste waarde toe te voegen.

  ![Clientadresruimte](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Selecteer het selectievakje **Gateway onmiddellijk maken**. Klik op **Optionele gatewayconfiguratie** om de pagina **Gatewayconfiguratie** te openen.

  ![Klik op Optionele gatewayconfiguratie](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
6. Klik op **Vereiste subnetinstellingen configureren** om het **Gatewaysubnet** toe te voegen. Het is mogelijk om een klein gatewaysubnet van /29 te maken, maar we raden u aan een groter subnet met meer adressen te maken door ten minste /28 of /27 te selecteren. Hierdoor hebt u genoeg adressen voor mogelijke aanvullende toekomstige configuraties. Als u met gatewaysubnetten werkt, vermijd dan om een netwerkbeveiligingsgroep (NSG) te koppelen aan het gatewaysubnet. Een koppeling van een netwerkbeveiligingsgroep naar dit subnet zorgt er mogelijk voor dat uw VPN-gateway niet meer werkt zoals verwacht.

  ![GatewaySubnet toevoegen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
7. Selecteer de gateway **Grootte**. De grootte is de gateway-SKU voor de gateway van uw virtuele netwerk. In de portal is de standaard-SKU **Basic**. Zie [Informatie over VPN-gatewayinstellingen](vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor informatie over gateway-SKU's.

  ![Grootte van de gateway](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
8. Selecteer het **Routeringstype** voor uw gateway. P2S-configuraties vereisen een **Dynamisch** routeringstype. Klik op **OK** wanneer u klaar bent met het configureren van deze pagina.

  ![Routeringstype configureren](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
9. Klik onder aan de pagina **Nieuwe VPN-verbinding** op **OK** om de virtuele netwerkgateway te gaan maken. Een VPN-gateway wordt binnen maximaal 45 minuten voltooid. De daadwerkelijke instelduur hangt af van de gateway-SKU die u selecteert.

## <a name="generatecerts"></a>2. Certificaten maken

Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. U uploadt de informatie van de openbare sleutel over het basiscertificaat naar Azure. De openbare sleutel wordt dan beschouwd als 'vertrouwd'. Clientcertificaten moeten worden gegenereerd op basis van het vertrouwde basiscertificaat en geïnstalleerd op elke clientcomputer. Dit gebeurt in het certificaatarchief Certificates-Current user/Personal. Het certificaat wordt gebruikt om de client te verifiëren bij het maken van verbinding met het VNet. 

Als u zelfondertekende certificaten gebruikt, moeten ze worden gemaakt met behulp van specifieke parameters. U kunt een zelfondertekend certificaat maken met behulp van de instructies voor [PowerShell en Windows 10](vpn-gateway-certificates-point-to-site.md) of [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Het is belangrijk dat u de stappen volgt in deze instructies bij het werken met zelfondertekende basiscertificaten en het genereren van clientcertificaten uit het zelfondertekende basiscertificaat. Anders zijn de certificaten die u maakt niet compatibel met P2S-verbindingen en zal er een verbindingsfout optreden.

### <a name="cer"></a>Deel 1: De openbare sleutel (.cer) voor het basiscertificaat verkrijgen

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="genclientcert"></a>Deel 2: Een clientcertificaat genereren

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>3. Het CER-bestand van het basiscertificaat uploaden

Wanneer de gateway is gemaakt, uploadt u het CER-bestand (dat de informatie over de openbare sleutel bevat) voor een vertrouwd basiscertificaat naar Azure. U uploadt de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Nadat het CER-bestand is geüpload, kan Azure daarmee clients met een geïnstalleerd clientcertificaat (gemaakt op basis van het vertrouwde basiscertificaat) verifiëren. Indien nodig kunt u later aanvullende vertrouwde basiscertificaatbestanden uploaden (maximaal 20).  

1. Klik in de sectie **VPN-verbindingen** van de pagina voor uw VNet op de afbeelding **clients** om de pagina **Punt-naar-site VPN-verbinding** te openen.

  ![Clients](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Klik op de pagina **Punt-naar-site-verbinding** op **Certificaten beheren** om de pagina **Certificaten** te openen.<br>

  ![De pagina Certificaten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Klik op de pagina **Certificaten** op **Uploaden** om de pagina **Certificaat uploaden** te openen.<br>

    ![De pagina Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Klik op de afbeelding van een map om het .cer-bestand te zoeken. Selecteer het bestand en klik vervolgens op **OK**. Vernieuw de pagina om het geüploade certificaat weer te geven op de pagina **Certificaten**.

  ![Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>4. De client configureren

Als u verbinding wilt maken met een VNet met behulp van een punt-naar-site-VPN, moet op elke client een pakket worden geïnstalleerd om de systeemeigen Windows VPN-client te configureren. Met het configuratiepakket wordt de systeemeigen Windows VPN-client geconfigureerd met de instellingen die nodig zijn om verbinding te maken met het virtuele netwerk.

U kunt hetzelfde configuratiepakket voor de VPN-client gebruiken op elke clientcomputer, mits de versie overeenkomt met de architectuur van de client. Zie voor de lijst met ondersteunde clientbesturingssystemen de [Veelgestelde vragen over punt-naar-site-verbindingen](#faq) onderaan dit artikel.

### <a name="generateconfigpackage"></a>Deel 1: Het configuratiepakket voor de VPN-client genereren en installeren

1. Ga in Azure Portal naar de pagina **Overzicht** voor uw VNet en klik bij **VPN-verbindingen** op de clientafbeelding om de pagina **Punt-naar-site VPN-verbinding** te openen.
2. Klik boven aan de pagina **Punt-naar-site VPN-verbinding** op het downloadpakket dat overeenkomt met het besturingssysteem van de client waarop het wordt geïnstalleerd:

  * Selecteer voor 64-bits clients **VPN-clients (64-bits)**.
  * Selecteer voor 32-bits clients **VPN-clients (32-bits)**.

  ![Het configuratiepakket voor de VPN-client downloaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Nadat het pakket is gegenereerd, downloadt en installeert u dit op de clientcomputer. Als u een SmartScreen-melding ziet, klikt u op **Meer info** en vervolgens op **Toch uitvoeren**. U kunt het pakket ook opslaan en op andere clientcomputers installeren.

### <a name="installclientcert"></a>Deel 2: Het clientcertificaat installeren

Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat. Dit is meestal alleen een kwestie van dubbelklikken op het certificaat en het installeren. Zie [Een geëxporteerd clientcertificaat installeren](vpn-gateway-certificates-point-to-site.md#install) voor meer informatie.

## <a name="connect"></a>5. Verbinding maken met Azure

### <a name="connect-to-your-vnet"></a>Verbinding maken met uw VNet

1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik in dat geval op **Doorgaan** om verhoogde bevoegdheden te gebruiken.
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.

  ![VPN-clientverbinding](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. De verbinding is tot stand gebracht.

  ![Tot stand gebrachte verbinding](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Problemen met P2S-verbindingen oplossen

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="verifyvpnconnect"></a>De VPN-verbinding controleren

1. Als u wilt controleren of uw VPN-verbinding actief is, opent u een opdrachtprompt met verhoogde bevoegdheid vanaf uw client-computer en voert u *ipconfig/all* uit.
2. Bekijk de resultaten. Het IP-adres dat u hebt ontvangen, is een van de adressen binnen het adresbereik van de punt-naar-site-verbinding dat u hebt opgegeven tijdens het maken van het VNet. De resultaten moeten er als volgt uitzien:

  ```
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
  ```

## <a name="connectVM"></a>Verbinding maken met een virtuele machine

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add"></a>Vertrouwde basiscertificaten toevoegen of verwijderen

U kunt vertrouwde basiscertificaat toevoegen in en verwijderen uit Azure. Wanneer u een basiscertificaat verwijdert, kunnen clients met een certificaat dat is gegenereerd op basis van dat basiscertificaat niet worden geverifieerd, en kunnen ze dus geen verbinding maken. Als u wilt dat clients kunnen worden geverifieerd en verbinding kunnen maken, moet u een nieuw clientcertificaat installeren dat is gegenereerd op basis van een basiscertificaat dat wordt vertrouwd (is geüpload) in Azure.

### <a name="addtrustedroot"></a>Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 vertrouwde .cer-basiscertificaatbestanden toevoegen aan Azure. Zie [Sectie 3: Het CER-bestand van het basiscertificaat uploaden](#upload) voor instructies.

### <a name="removetrustedroot"></a>Een vertrouwd basiscertificaat verwijderen

1. Klik in de sectie **VPN-verbindingen** van de pagina voor uw VNet op de afbeelding **clients** om de pagina **Punt-naar-site VPN-verbinding** te openen.

  ![Clients](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Klik op de pagina **Punt-naar-site-verbinding** op **Certificaten beheren** om de pagina **Certificaten** te openen.<br>

  ![De pagina Certificaten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Klik op de pagina **Certificaten** op de weglatingstekens naast het certificaat dat u wilt verwijderen en klik vervolgens op **Verwijderen**.

  ![Basiscertificaat verwijderen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>

## <a name="revokeclient"></a>Een clientcertificaat intrekken

U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Dit wijkt af van het verwijderen van een vertrouwd basiscertificaat. Als u een vertrouwd .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u in plaats van een basiscertificaat een clientcertificaat intrekt, kunt u de certificaten die zijn gegenereerd op basis van het basiscertificaat, blijven gebruiken voor verificatie van de punt-naar-site-verbinding.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="revokeclientcert"></a>Een clientcertificaat intrekken

U kunt een clientcertificaat intrekken door de vingerafdruk toe te voegen aan de intrekkingslijst.

1. Haal de vingerafdruk voor het clientcertificaat op. Zie voor meer informatie [De vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieer de gegevens naar een teksteditor en verwijder alle spaties, zodat u een doorlopende tekenreeks overhoudt.
3. Navigeer naar de pagina **'naam van klassieke virtuele netwerk' > Punt-naar-site VPN-verbinding > Certificaten** en klik vervolgens op **Intrekkingslijst** om de pagina Intrekkingslijst te openen. 
4. Klik op de pagina **Intrekkingslijst** op **+Certificaat toevoegen** om de pagina **Certificaat toevoegen aan intrekkingslijst** te openen.
5. Plak op de pagina **Certificaat toevoegen aan intrekkingslijst** de vingerafdruk van het certificaat als een doorlopende regel tekst, zonder spaties. Klik onder aan de pagina op **OK**.
6. Wanneer het bijwerken is voltooid, kan het certificaat niet langer worden gebruikt om verbinding te maken. Clients die verbinding proberen te maken met het certificaat, ontvangen een bericht waarin wordt gemeld dat het certificaat niet meer geldig is.

## <a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-point-to-site-classic-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/linux/azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.
