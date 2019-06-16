---
title: Een privécloud maken op Azure VMware-oplossing door CloudSimple Quickstart-
description: Meer informatie over het maken en configureren van een privécloud met de VMware-oplossing Azure door CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e1fc7809ad94d589483b87c638d027a39098164e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209545"
---
# <a name="quickstart---configure-a-private-cloud-environment"></a>Snelstart: een privécloud-omgeving configureren

In dit artikel leert u hoe u een privécloud CloudSimple maken en uw persoonlijke cloudomgeving instellen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-private-cloud"></a>Een privécloud maken

1. Selecteer **Alle services**.
2. Zoeken naar **CloudSimple Services**.
3. Selecteer de CloudSimple-service op die u wilt maken van uw Privécloud.
4. Overzicht, klikt u op **persoonlijke Cloud maken** openen van een nieuw browsertabblad voor CloudSimple-portal.  Als u hierom wordt gevraagd, aanmelden met uw Azure-referenties aanmelden.  

    ![Private Cloud maken van Azure](media/create-private-cloud-from-azure.png)

5. Geef een naam voor uw Privécloud in CloudSimple-portal
6. Selecteer de **locatie** van uw Privécloud
7. Selecteer de **knooppunttype** u hebt gekocht op Azure.  U kunt ervoor kiezen de [optie CS28 of CS36](cloudsimple-node.md#vmware-solution-by-cloudsimple-nodes-sku). De laatste optie bevat de maximale capaciteit voor Computing en geheugen.
8. Geef de **aantal knooppunten**.  Ten minste drie knooppunten zijn vereist voor het maken van een Private Cloud

    ![Private Cloud - de basisgegevens van maken](media/create-private-cloud-basic-info.png)

9. Klik op **Next: Geavanceerde opties**.
10. Voer de CIDR-bereik voor vSphere/vSAN subnetten. Zorg ervoor dat de CIDR-bereik met een van uw on-premises of andere Azure subnetten niet overlappen.

    ![Private Cloud - geavanceerde opties maken](media/create-private-cloud-advanced-options.png)

11. Selecteer **Volgende: Controleren en maken**.
12. Controleer de instellingen. Als u geen instellingen te wijzigen, klikt u op **vorige**.
13. Klik op **Create**.

Private Cloud inrichtingsproces wordt gestart.  Het duurt maximaal twee uur voor de Privécloud in te richten.

## <a name="launch-cloudsimple-portal"></a>Starten van CloudSimple-portal

U kunt CloudSimple portal openen vanuit Azure portal.  CloudSimple portal wordt gestart met uw Azure aanmeldreferenties met eenmalige aanmelding (SSO).  Toegang tot de portal CloudSimple, moet u toestaan dat de **CloudSimple Service autorisatie** toepassing.  Zie voor meer informatie over het verlenen van machtigingen, [instemmen met toepassing CloudSimple Service autorisatie](https://docs.azure.cloudsimple.com/access-cloudsimple-portal/#consent-to-cloudsimple-service-authorization-application)

1. Selecteer **Alle services**.
2. Zoeken naar **CloudSimple Services**.
3. Selecteer de CloudSimple-service op die u wilt maken van uw Privécloud.
4. Overzicht, klikt u op **gaat u naar de portal CloudSimple** openen van een nieuw browsertabblad voor CloudSimple-portal.  Als u hierom wordt gevraagd, aanmelden met uw Azure-referenties aanmelden.  

    ![Starten van CloudSimple-Portal](media/launch-cloudsimple-portal.png)

## <a name="create-point-to-site-vpn"></a>Punt-naar-site VPN maken

Een punt-naar-site VPN-verbinding is de eenvoudigste manier om verbinding maken met uw Privécloud van uw computer. Punt-naar-site VPN-verbinding gebruiken als u verbinding in de Privécloud op afstand maakt.  Voor snelle toegang tot uw Privécloud de volgende stappen uit te voeren.  Toegang tot CloudSimple regio van uw on-premises netwerk kan worden gedaan met [Site-naar-Site VPN](https://docs.azure.cloudsimple.com/vpn-gateway/) of [Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/).

### <a name="create-gateway"></a>Gateway maken

1. CloudSimple portal en selecteer starten **netwerk**.
2. Selecteer **VPN-Gateway**.
3. Klik op **nieuwe VPN-Gateway**.

    ![Een VPN-gateway maken](media/create-vpn-gateway.png)

4. Voor **gatewayconfiguratie**, geef de volgende instellingen en klik op **volgende**.

    * Selecteer **punt-naar-Site VPN** als het Gatewaytype.
    * Voer een unieke naam voor de gateway.
    * Selecteer de Azure-locatie waar de CloudSimple-service is geïmplementeerd.
    * Geef het clientsubnet voor de punt-naar-site-gateway.  Wanneer u verbinding maakt, zal DHCP-adressen van dit subnet worden gegeven.

5. Voor **verbinding/gebruiker**, geef de volgende instellingen en klik op **volgende**.

    * Selecteren om toe te staan automatisch alle huidige en toekomstige gebruikers toegang krijgen tot de Privécloud via deze punt-naar-site-gateway, **alle gebruikers automatisch toevoegen**. Wanneer u deze optie selecteert, worden alle gebruikers in de lijst met gebruikers automatisch geselecteerd. U kunt de automatische optie negeren door afzonderlijke gebruikers in de lijst uit te schakelen.
    * Klik op de selectievakjes in de lijst met gebruikers alleen afzonderlijke gebruikers.

6. De sectie VLAN's / subnetten kunt u om op te geven en VLAN's / subnetten voor de gateway en verbindingen.

    * De **automatisch toevoegen** opties voor het globale beleid voor deze gateway ingesteld. De instellingen gelden voor de huidige-gateway. De instellingen kunnen worden overschreven in de **Selecteer** gebied.
    * Selecteer **toevoegen management VLAN's / subnetten van Privéclouds**. 
    * Als u wilt alle door de gebruiker gedefinieerde VLAN's / subnetten toevoegen, klikt u op **toevoegen van de gebruiker gedefinieerde VLAN's / subnetten**. 
    * De **Selecteer** instellingen overschrijven de globale instellingen onder **automatisch toevoegen**. 

7. Klik op **volgende** om de instellingen te controleren. Klik op de pictogrammen bewerken als u wilt wijzigen.
8. Klik op **maken** te maken van de VPN-gateway.

### <a name="connect-to-cloudsimple-using-point-to-site-vpn"></a>Verbinding maken met CloudSimple met behulp van punt-naar-site-VPN

VPN-client is nodig voor het verbinden met CloudSimple vanaf uw computer.  Download [OpenVPN client](https://openvpn.net/community-downloads/) voor Windows of [viscositeit](https://www.sparklabs.com/viscosity/download/) voor macOS- en OS X.

1. CloudSimple portal en selecteer starten **netwerk**.
2. Selecteer **VPN-Gateway**.
3. Klik op de punt-naar-site VPN-gateway in de lijst van de VPN-gateways.
4. Selecteer **gebruikers**.
5. Klik op **downloaden van Mijn VPN-configuratie**

    ![VPN-configuratie downloaden](media/download-p2s-vpn-configuration.png)

6. Importeren van de configuratie van uw VPN-client

    * Instructies voor het [importeren van de configuratie op Windows-client](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program)
    * Instructies voor het [importeren van de configuratie op Mac OS- of OS X](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection)

7. Verbinding maken met CloudSimple

## <a name="create-a-vlan-for-your-workload-vms"></a>Een VLAN op voor uw workload-VM's maken

Na het maken van een privécloud, maakt u een VLAN waar u uw werkbelasting/toepassing VM's implementeert.

1. Selecteer in de CloudSimple Portal **netwerk**.
2. Klik op **VLAN/subnetten**.
3. Klik op **VLAN/Subnet maken**

    ![VLAN/Subnet maken](media/create-new-vlan-subnet.png)

4. Selecteer de **Private Cloud** voor het nieuwe VLAN/subnet.
5. Selecteer een VLAN-ID in de lijst.  
6. Voer de subnetnaam van een voor het subnet.
7. Geef het subnet CIDR-bereik en masker.  Dit bereik mag niet overlappen met bestaande subnetten.
8. Klik op **Indienen**.

    ![VLAN-/ subnetgegevens maken](media/create-new-vlan-subnet-details.png)

De VLAN/subnet wordt gemaakt.  U kunt nu deze VLAN-ID gebruiken om te maken van een gedistribueerde poortgroep op uw vCenter Private Cloud. 

## <a name="connect-your-environment-to-an-azure-virtual-network"></a>Verbinding maken met uw omgeving met een Azure-netwerk

CloudSimple biedt u een ExpressRoute-circuit voor uw privécloud. U kunt uw virtuele netwerk in Azure verbinden met het ExpressRoute-circuit. Volg de stappen in voor de volledige informatie over het instellen van de verbinding [Azure virtuele netwerkverbinding met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/cloudsimple-azure-network-connection/)

## <a name="sign-in-to-vcenter"></a>Meld u aan met vCenter

U kunt nu aanmelden bij vCenter voor het instellen van virtuele machines en het beleid.

1. Voor toegang tot vCenter, start u vanuit de portal CloudSimple. Op de startpagina onder **algemene taken**, klikt u op **vSphere-Client starten**.  Selecteer de Privécloud en klik vervolgens op **vSphere-Client starten** op de Privécloud.

    ![VSphere-Client starten](media/launch-vcenter-from-cloudsimple-portal.png)

2. Selecteer de gewenste vSphere-client voor toegang tot vCenter en meld u aan met uw gebruikersnaam en wachtwoord.  De standaardwaarden zijn:
    * Gebruikersnaam: **CloudOwner@cloudsimple.local**
    * Wachtwoord: **CloudSimple123!**  

De vCenter-schermen in de volgende procedures zijn van de client vSphere (HTML5).

## <a name="change-your-vcenter-password"></a>De vCenter-wachtwoord wijzigen

CloudSimple wordt aanbevolen dat u uw wachtwoord de eerste keer dat u zich wijzigt aanmeldt met vCenter.  
Het wachtwoord die u instelt, moet voldoen aan de volgende vereisten:

* Maximum levensduur: Wachtwoord moet worden gewijzigd om 365 dagen
* Hergebruik: Gebruikers niet opnieuw gebruiken van de vorige vijf wachtwoorden
* Duur: 8: 20 tekens bevatten
* Speciaal teken: Ten minste één speciaal teken
* Alfabetische tekens bevatten: Ten minste één hoofdletter, A-Z en ten minste één kleine letter, a-z
* Nummers: Ten minste één numeriek teken, 0-9
* Maximale identieke tekens: Drie

    Voorbeeld: CC of CC is aanvaardbaar als onderdeel van het wachtwoord, maar CCCC niet.

Als u een wachtwoord dat niet voldoet aan de vereisten instellen:

* Als u de vSphere-Flash-Client gebruikt, wordt een fout
* Als u de client HTML5, heeft niet deze een fout gemeld. De wijziging niet geaccepteerd door de client en het oude wachtwoord blijft werken.

## <a name="change-nsx-administrator-password"></a>NSX administrator-wachtwoord wijzigen

NSX manager wordt geïmplementeerd met een standaardwachtwoord.  We raden dat u het wachtwoord te wijzigen nadat u uw Privécloud hebt gemaakt.

   * Gebruikersnaam: **admin**
   * Wachtwoord: **CloudSimple123!**

U vindt de volledig gekwalificeerde domeinnaam (FQDN) en IP-adres van manager NSX op CloudSimple-portal.

1. CloudSimple portal en selecteer starten **Resources**.
2. Klik op de Privécloud die u wilt gebruiken.
3. Selecteer **vSphere-beheernetwerk**
4. Gebruik de FQDN-naam of IP-adres van **NSX Manager** en maak verbinding met een webbrowser. 

    ![NSX Manager FQDN zoeken](media/private-cloud-nsx-manager-fqdn.png)

Volg de instructies in het wachtwoord wilt wijzigen, [NSX Manager installatie](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.2/com.vmware.nsxt.install.doc/GUID-A65FE3DD-C4F1-47EC-B952-DEDF1A3DD0CF.html).

## <a name="create-a-port-group"></a>Een poortgroep maken

Een gedistribueerde poortgroep in vSphere maken:

1. Volg de instructies in 'Een poortgroep gedistribueerde toevoegen' de [vSphere Networking Guide](https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf).
2. Bij het instellen van de poortgroep gedistribueerde, bieden de VLAN-ID gemaakt in [een VLAN maken voor uw Workload-VM's](#create-a-vlan-for-your-workload-vms).

## <a name="next-steps"></a>Volgende stappen

* [Virtuele VMware-machines op Azure gebruiken](https://docs.azure.cloudsimple.com/quickstart-create-vmware-virtual-machine)
* [Virtuele VMware-machines op Azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* [Verbinding maken met on-premises netwerk met behulp van Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [Site-naar-Site-VPN van on-premises instellen](https://docs.azure.cloudsimple.com/vpn-gateway/)
