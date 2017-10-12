---
title: 'Een on-premises netwerk verbinden met een virtueel Azure-netwerk: site-naar-site-VPN (klassiek): Portal | Microsoft Docs'
description: Stappen voor het maken van een IPSec-verbinding van uw on-premises netwerk met een virtueel Azure-netwerk via het openbare internet. Deze stappen helpen u een cross-premises site-naar-site-VPN-gatewayverbinding te maken met de portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/010/2017
ms.author: cherylmc
ms.openlocfilehash: 0be8dd6d90edb7b32b6777c76c9778cda0dcd5ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-site-to-site-connection-using-the-azure-portal-classic"></a>Een site-naar-site-verbinding maken met behulp van Azure Portal (klassiek)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

In dit artikel leest u hoe u Azure Portal gebruikt om een site-naar-site-VPN-gatewayverbinding te maken vanaf uw on-premises netwerk naar het VNet. De stappen in dit artikel zijn van toepassing op het klassieke implementatiemodel. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Zie [Overzicht van VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over VPN-gateways.

![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint, of aan de volgende criteria is voldaan:

* U hebt vastgesteld dat u wilt werken met het klassieke implementatiemodel. Als u wilt werken met het Resource Manager-implementatiemodel, gaat u naar [Een site-naar-site-verbinding maken (Resource Manager)](vpn-gateway-howto-site-to-site-resource-manager-portal.md). We raden u aan indien mogelijk het Resource Manager-implementatiemodel te gebruiken.
* U hebt een compatibel VPN-apparaat nodig en iemand die dit kan configureren. Zie [Over VPN-apparaten](vpn-gateway-about-vpn-devices.md) voor meer informatie over compatibele VPN-apparaten en -apparaatconfiguratie.
* Controleer of u een extern gericht openbaar IPv4-adres voor het VPN-apparaat hebt. Dit IP-adres kan zich niet achter een NAT bevinden.
* Als u de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken. Wanneer u deze configuratie maakt, moet u de IP-adresbereikvoorvoegsels opgeven die Azure naar uw on-premises locatie doorstuurt. Geen van de subnetten van uw on-premises netwerk kan overlappen met de virtuele subnetten waarmee u verbinding wilt maken.
* Momenteel is het in PowerShell vereist dat de gedeelde sleutel wordt opgegeven en de VPN-gatewayverbinding wordt gemaakt. Installeer de nieuwste versie van de Azure SM (Service Management) PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. Als u werkt met PowerShell voor deze configuratie, zorg er dan voor dat u de bewerkingen uitvoert als beheerder. 

### <a name="values"></a>Voorbeeld van configuratiewaarden voor deze oefening

In de voorbeelden in dit artikel worden de volgende waarden gebruikt. U kunt deze waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel.

* **VNet-naam:** TestVNet1
* **Adresruimte:** 
  * 10.11.0.0/16
  * 10.12.0.0/16 (optioneel voor deze oefening)
* **Subnetten:**
  * FrontEnd: 10.11.0.0/24
  * Back-end: 10.12.0.0/24 (optioneel voor deze oefening)
* **Gatewaysubnet**: 10.11.255.0/27
* **Resourcegroep:** TestRG1
* **Locatie:** VS - oost
* **DNS-server:** 10.11.0.3 (optioneel voor deze oefening)
* **Lokale sitenaam:** Site2
* **Clientadresruimte:** de adresruimte op uw on-premises site.

## <a name="CreatVNet"></a>1. Een virtueel netwerk maken

Als u een virtueel netwerk maakt om te gebruiken met een S2S-verbinding, moet u ervoor zorgen dat de opgegeven adresruimten niet overlappen met een of meer van de clientadresruimten voor de lokale sites waarmee u verbinding wilt maken. Als u overlappende subnetten hebt, werkt de verbinding mogelijk niet goed.

* Als u al beschikt over een VNet, controleert u of de instellingen compatibel zijn met het ontwerp van de VPN-gateway. Let vooral op eventuele subnetten die met andere netwerken overlappen. 

* Als u nog geen virtueel netwerk hebt, maakt u er een. De schermafbeeldingen dienen alleen als voorbeeld. Zorg dat u de waarden vervangt door die van uzelf.

### <a name="to-create-a-virtual-network"></a>Een virtueel netwerk maken

1. Navigeer via een browser naar de [Azure Portal](http://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op **+**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **Virtual Network** in de lijst met resultaten en klik erop om de pagina **Virtual Network** te openen.

  ![Pagina voor zoeken van virtueel netwerk](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Kies onder aan de pagina Virtueel netwerk in de vervolgkeuzelijst **Een implementatiemodel selecteren** de optie **Klassiek** en klik vervolgens op **Maken**.

  ![Een implementatiemodel selecteren](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken (klassiek)**. Voeg op deze pagina de eerste adresruimte en één adresbereik voor het subnet toe. Nadat u het VNet hebt aangemaakt, kunt u terugkeren en aanvullende subnets en adresruimten toevoegen.

  ![Pagina Virtueel netwerk maken](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Pagina Virtueel netwerk maken")
5. Controleer of het **Abonnement** het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
6. Klik op **Resourcegroep** en selecteer een bestaande resourcegroep of maak een nieuwe resourcegroep door een naam in te voeren. Zie voor meer informatie over resourcegroepen [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Selecteer vervolgens de **Locatie**-instellingen voor uw VNet. De locatie bepaalt waar de resources die u naar dit VNet implementeert, worden opgeslagen.
8. Selecteer **Vastmaken aan dashboard** als u uw VNet gemakkelijk wilt terugvinden op het dashboard. Klik op **Maken** om het VNet te maken.

  ![Vastmaken aan dashboard](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "Vastmaken aan dashboard")
9. Wanneer u op Maken klikt, wordt er op het dashboard een tegel weergegeven die de voortgang van uw VNet weergeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.

  ![Tegel Virtueel netwerk maken](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Tegel Virtueel netwerk maken")

Nadat het virtuele netwerk is gemaakt, wordt op de pagina met netwerken in de klassieke Azure-portal **Gemaakt** vermeld onder **Status**.

## <a name="additionaladdress"></a>2. Extra adresruimte toevoegen

Nadat u het virtuele netwerk hebt gemaakt, kunt u extra adresruimte toevoegen. Het toevoegen van extra adresruimte geen vereist onderdeel van een S2S-configuratie, maar als u meerdere adresruimten nodig hebt, voert u de volgende stappen uit:

1. Ga naar de virtuele netwerken in de portal.
2. Ga op de pagina van uw virtuele netwerk naar de sectie **Instellingen** en klik op **Adresruimte**.
3. Klik op de pagina Adresruimte op **+Toevoegen** en voer extra adresruimte in.

## <a name="dns"></a>3. Een DNS-server opgeven

DNS-instellingen zijn geen vereist onderdeel van een S2S-configuratie, maar een DNS is nodig voor naamomzetting. Het opgeven van een waarde betekent niet dat er een nieuwe DNS-server wordt gemaakt. Het IP-adres van de DNS-server dat u opgeeft, moet het adres zijn van een DNS-server zijn die de namen kan omzetten voor de resources waarmee u verbinding maakt. Voor dit voorbeeld gebruiken we een privé-IP-adres. De kans is zeer klein dat het IP-adres uit het voorbeeld het IP-adres van uw DNS-server is. Zorg ervoor dat u uw eigen waarden gebruikt.

Nadat u uw virtuele netwerk hebt gemaakt, kunt u het IP-adres van een DNS-server toevoegen om de naamomzetting te verwerken. Open de instellingen voor het virtuele netwerk, klik op de DNS-servers en voeg het IP-adres toe van de DNS-server die u wilt gebruiken voor naamomzetting.

1. Ga naar de virtuele netwerken in de portal.
2. Ga op de pagina voor uw virtuele netwerk naar de sectie **Instellingen** en klik op **DNS-servers**.
3. Voeg een DNS-server toe.
4. Klik boven aan de pagina op **Opslaan** om uw instellingen op te slaan.

## <a name="localsite"></a>4. De lokale site configureren

De lokale site verwijst doorgaans naar uw on-premises locatie. Het bevat het IP-adres van het VPN-apparaat waarmee u verbinding gaat maken en de IP-adresbereiken die via de VPN-gateway naar het VPN-apparaat worden gerouteerd.

1. Navigeer in de portal naar het virtuele netwerk waarvoor u een gateway wil maken.
2. Ga op de pagina voor uw virtuele netwerk naar **Overzicht** en klik in de sectie VPN-verbindingen op **Gateway** om de pagina **Nieuwe VPN-verbinding** te openen.

  ![Klik om gatewayinstellingen te configureren](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "Klik om gatewayinstellingen te configureren")
3. Selecteer **Site-naar-site** op de pagina **Nieuwe VPN-verbinding**.
4. Klik op **Lokale site - vereiste instellingen configureren** om de pagina **Lokale site** te openen. Configureer de instellingen en klik vervolgens op **OK** om de instellingen op te slaan.
  - **Naam:** geef de lokale site een naam waarmee u deze eenvoudig kunt identificeren.
  - **IP-adres voor de VPN-gateway:** dit is het openbare IP-adres van het VPN-apparaat voor uw on-premises netwerk. Voor het VPN-apparaat is een openbaar IPv4-adres vereist. Geef een geldig openbaar IP-adres op voor het VPN-apparaat waarmee u verbinding wilt maken. Het mag zich niet achter NAT bevinden en moet bereikbaar zijn voor Azure. Als u het IP-adres van het VPN-apparaat niet kent, kunt u altijd een tijdelijke aanduiding invoegen (zolang deze maar de indeling van een geldig openbaar IP-adres heeft) en dit later wijzigen.
  - **Clientadresruimte:** vermeld de IP-adresbereiken die u via deze gateway naar het lokale on-premises netwerk wilt routeren. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de hier opgegeven bereiken niet overlappen met bereiken van andere netwerken waarmee uw virtuele netwerk is verbonden of met de adresbereiken van het virtuele netwerk zelf.

  ![Lokale site](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Lokale site configureren")

## <a name="gatewaysubnet"></a>5. Het gatewaysubnet configureren

U moet een gatewaysubnet maken voor de VPN-gateway. Het gatewaysubnet bevat de IP-adressen waarvan de VPN-gatewayservices gebruikmaken.

1. Schakel op de pagina **Nieuwe VPN-verbinding** het selectievakje **Gateway onmiddellijk maken** in. De pagina Optionele gatewayconfiguratie wordt weergegeven. Als u het selectievakje niet inschakelt, wordt de pagina voor het configureren van het gatewaysubnet niet weergegeven.

  ![Gatewayconfiguratie - subnet, grootte, routingtype](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Gatewayconfiguratie - subnet, grootte, routingtype")
2. Als u de pagina **Gatewayconfiguratie** wilt openen, klikt u op **Optionele gatewayconfiguratie - subnet, grootte en routingtype**.
3. Klik op de pagina **Gatewayconfiguratie** op **Subnet - vereiste instellingen configureren** om de pagina **Subnet toevoegen** te openen.

  ![Gatewayconfiguratie - gatewaysubnet](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "gatewayconfiguratie - gatewaysubnet")
4. Voeg op de pagina **Subnet toevoegen** het gatewaysubnet toe. De grootte van het gatewaysubnet dat u opgeeft, is afhankelijk van de VPN-gatewayconfiguratie die u wilt maken. Hoewel het mogelijk is om een gatewaysubnet van slechts /29 te maken, wordt minimaal /27 of /28 aanbevolen. U maakt dan een groter subnet dat meer adressen bevat. Met een groter gatewaysubnet beschikt u over voldoende IP-adressen voor mogelijke toekomstige configuraties.

  ![Gatewaysubnet toevoegen](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Gatewaysubnet toevoegen")

## <a name="sku"></a>6. Het SKU- en VPN-type opgeven

1. Selecteer de gateway **Grootte**. Dit is de gateway-SKU die u gaat gebruiken om uw virtuele netwerkgateway te maken. In de portal is de standaard-SKU = **Basic**. Klassieke VPN-gateways gebruiken de oude (verouderde) gateway-SKU's. Zie [Working with virtual network gateway SKUs (old SKUs)](vpn-gateway-about-skus-legacy.md) (Werken met virtuele netwerkgateway-SKU's (oude SKU's)) voor meer informatie over de oude gateway-SKU's.

  ![SKUL- en VPN-type selecteren](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "SKU- en VPN-type selecteren")
2. Selecteer het **Routeringstype** voor uw gateway. Dit staat ook bekend als het VPN-type. Het is belangrijk dat u het juiste gatewaytype selecteert, omdat u de gateway niet van het ene type naar een ander type kunt converteren. Het VPN-apparaat moet compatibel zijn met het routingtype dat u selecteert. Zie [Over VPN-gatewayinstellingen](vpn-gateway-about-vpn-gateway-settings.md#vpntype) voor meer informatie over VPN-typen. Mogelijk ziet u artikelen die verwijzen naar de VPN-typen RouteBased en PolicyBased. Het type Dynamisch komt overeen met RouteBased, en het type Vast komt overeen met PolicyBased.
3. Klik op **OK** om de instellingen op te slaan.
4. Klik onder aan de pagina **Nieuwe VPN-verbinding** op **OK** om de virtuele netwerkgateway te gaan maken. Afhankelijk van de SKU die u selecteert, duurt dit maximaal 45 minuten.

## <a name="vpndevice"></a>7. Uw VPN-apparaat configureren

Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. In deze stap configureert u het VPN-apparaat. Bij de configuratie van uw VPN-apparaat hebt u het volgende nodig:

- Een gedeelde sleutel. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site-VPN-verbinding maakt. In onze voorbeelden gebruiken we een eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te genereren.
- Het openbare IP-adres van de gateway van uw virtuele netwerk. U kunt het openbare IP-adres weergeven met behulp van Azure Portal, PowerShell of de CLI.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. De verbinding maken
In deze stap stelt u de gedeelde sleutel in en maakt u de verbinding. De sleutel die u instelt, moet dezelfde sleutel zijn die is gebruikt bij de configuratie van het VPN-apparaat.

> [!NOTE]
> Deze stap is momenteel niet beschikbaar in Azure Portal. Gebruik de SM-versie (Service Management) van de Azure PowerShell-cmdlets.
>

### <a name="step-1-connect-to-your-azure-account"></a>Step 1. Verbinding maken met uw Azure-account

1. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

  ```powershell
  Add-AzureAccount
  ```
2. Controleer de abonnementen voor het account.

  ```powershell
  Get-AzureSubscription
  ```
3. Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken.

  ```powershell
  Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_ID"
  ```

### <a name="step-2-set-the-shared-key-and-create-the-connection"></a>Stap 2. De gedeelde sleutel instellen en de verbinding maken

Als u werkt met PowerShell en het klassieke implementatiemodel, komen de namen van resources in de portal soms niet overeen met wat verwacht wordt in Azure bij het gebruik van PowerShell. In de volgende stappen leert u hoe u het netwerkconfiguratiebestand exporteert om de exacte namen te verkrijgen.

1. Maak een map op de computer en exporteer vervolgens het netwerkconfiguratiebestand naar de map. In dit voorbeeld wordt het netwerkconfiguratiebestand geëxporteerd naar C:\AzureNet.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Open het netwerkconfiguratiebestand met een xml-editor en controleer de waarden voor LocalNetworkSite name en VirtualNetworkSite name. Pas het voorbeeld aan uw omgeving aan. Gebruik voor een naam die spaties bevat, enkele aanhalingstekens rond de waarde.

3. Stel de gedeelde sleutel in en maak de verbinding. -SharedKey is een waarde die u genereert en opgeeft. In het voorbeeld wordt abc123 gebruikt, maar u kunt een ingewikkeldere waarde gebruiken. (aanbevolen) Het is van belang dat de waarde die u hier opgeeft, dezelfde waarde is die u hebt opgegeven bij het configureren van het VPN-apparaat.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
  -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
  ```
Wanneer de verbinding is gemaakt, is het resultaat: **Status: geslaagd**.

## <a name="verify"></a>9. De verbinding controleren

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

Als u verbindingsproblemen ondervindt, raadpleegt u de sectie **Problemen oplossen** van de inhoudsopgave in het linkerdeelvenster.

## <a name="reset"></a>Een VPN-gateway opnieuw instellen

Het opnieuw instellen van een Azure VPN-gateway is handig als u cross-premises VPN-connectiviteit verliest in een of meer Site-to-Site VPN-tunnels. In een dergelijke situatie functioneren al uw on-premises VPN-apparaten naar behoren, maar kunnen ze geen IPSec-tunnels tot stand brengen met de Azure VPN-gateways. Zie [Een VPN-gateway opnieuw instellen](vpn-gateway-resetgw-classic.md) voor de stappen.

## <a name="changesku"></a>Een gateway-SKU wijzigen

Zie [Formaat van een gateway-SKU wijzigen](vpn-gateway-about-SKUS-legacy.md) voor de stappen voor het wijzigen van een gateway-SKU.

## <a name="next-steps"></a>Volgende stappen

* Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.
* Zie [Informatie over geforceerde tunneling](vpn-gateway-about-forced-tunneling.md) voor meer informatie over geforceerde tunneling.