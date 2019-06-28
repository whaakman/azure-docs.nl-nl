---
title: Microsoft Azure FXT Edge Filer netwerkverbindingen en -voeding
description: Hoe u de netwerkpoorten bekabelen en acculading Azure FXT Edge Filer hardware toevoegen
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 444835a94559a352bfd749cfa1cb2cd8c3a39373
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450308"
---
# <a name="tutorial-make-network-connections-and-supply-power-to-the-azure-fxt-edge-filer-node"></a>Zelfstudie: Netwerkverbindingen maken en om het knooppunt Azure FXT Edge Filer te leveren

In deze zelfstudie leert u hoe u de netwerkverbindingen voor een Azure FXT Edge Filer hardware-knooppunt-kabel.

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Het type van de netwerkkabel voor uw omgeving kiezen
> * Verbinding maken tussen een Azure FXT Edge Filer-knooppunt en uw netwerk van datacenters
> * Hoe u kabels via de kabel beheertak (CMA) route
> * Hoe u power verbinding met het racks apparaat en inschakelen

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet de Azure FXT Edge Filer worden geïnstalleerd op een rek standaard apparatuur. De CMA moet worden geïnstalleerd op het knooppunt filer. 

## <a name="identify-ports"></a>Identificeren van poorten

Identificeer de verschillende poorten op de achterkant van uw Azure FXT Edge Filer. 
 
![Achterzijde een bekabelde apparaat](media/fxt-back-annotated.png)

## <a name="cable-the-device"></a>Het apparaat bekabelen

* Verbinding maken met de RJ-45-poorten aan uw datacenter netwerkbron zoals beschreven in [netwerkpoorten](#network-ports).  
* Veilig verbinding maken met de [iDRAC poort](#idrac-port) met een afzonderlijk netwerk met een beveiligde DHCP-server. 
* Gebruik de USB-poorten en de VGA-poort verbinding te maken van een toetsenbord en monitor naar het knooppunt voor de eerste installatie. U moet het knooppunt opstarten en [instellen van een eerste wachtwoord](fxt-node-password.md) om het activeren van het knooppunt de van andere poorten. Lezen [initiële wachtwoorden instellen](fxt-node-password.md) voor meer informatie. 

Dit artikel wordt ook beschreven hoe u [verbinding netstroom](#connect-power-cables) voor het knooppunt. 

In dit artikel wordt ook uitgelegd hoe u verbinding maken met het knooppunt [seriële poort](#serial-port-only-when-necessary), maar de seriële poort wordt alleen gebruikt voor geavanceerde probleemoplossing. 

### <a name="network-ports"></a>Netwerkpoorten 

Elk knooppunt Azure FXT Edge Filer bevat de volgende netwerkpoorten: 

* Zes snelle 25GbE/10GbE dubbele snelheid gegevenspoorten: 

  * Vier poorten die worden geleverd door twee tarief voor twee poorten invoegtoepassing-netwerkadapters
  * Twee poorten die worden geleverd door de netwerkadapter van moederbord mezzanine 

* Twee 1 GbE-poorten is geleverd door de netwerkadapter van moederbord mezzanine 

De met hoge snelheid 25GbE/10 GbE-gegevenspoorten zijn standaard SFP28-compatibele kooien. Voor het gebruik van optische kabels, moet u SFP28 optische transceiver modules (niet opgegeven) installeren.

De poorten van 1GbE hebben standaard RJ-45-connectors.

Voor een volledige lijst van ondersteunde kabels en switches infraroodzenders, Raadpleeg de [Cavium FastlinQ 41000 reeks interoperabiliteit Matrix](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).

Het type verbindingen moet worden gebruikt voor uw systeem is afhankelijk van uw data center-omgeving.

* Als u verbinding maakt met een netwerk 25GbE, bekabelen u elk van de gegevenspoorten met hoge snelheid met een van de volgende kabeltypen:

  * Optische kabel en SFP28 optische transceiver met 25GbE of dubbele snelheid 25GbE/10 GbE-mogelijkheid
  * SFP28 type 25GbE kunnen direct twinaxial kabel koppelen

* Als u verbinding maakt met een netwerk 10 GbE, kabel voor elk van de snelle poorten met een van de volgende: 

  * Optische kabel en SFP28 optische transceiver met 10 GbE of dubbele snelheid 25GbE/10 GbE-mogelijkheid.
  * SFP28 type 25GbE kunnen direct twinaxial kabel koppelen
  * SFP28 type 10 GbE-compatibele direct twinaxial kabel koppelen

* De netwerk-poorten van 1GbE worden gebruikt voor cluster beheer van verkeer. Controleer de **1Gb-mgmt-netwerk gebruiken** optie bij het maken van het cluster (beschreven in [configureren van het beheernetwerk](fxt-cluster-create.md#configure-the-management-network)). De poorten met standard CAT 5- of betere kabel zoals beschreven in de lijst met ondersteunde kabels-kabel.

  U kunt de 1 GbE-poorten als u van plan bent de snelle om poorten te gebruiken voor al het verkeer uncabled laten. Standaard worden de netwerk-poorten van 1GbE niet gebruikt als een hogere snelheid de gegevenspoort beschikbaar is.  

### <a name="idrac-port"></a>iDRAC poort  

De poort met het label iDRAC is een verbinding van 1Gb die communicatie mogelijk maakt met een externe toegang-controller die wordt gebruikt voor het hardwarebeheer van en bewaking. FXT software maakt gebruik van Intelligent Platform Management Interface (IPMI) met deze controller voor probleemoplossing en herstel. U kunt de ingebouwde [iDRAC interface](https://www.dell.com/support/manuals/idrac9-lifecycle-controller-v3.30.30.30/idrac_3.30.30.30_ug/) voor het bewaken van hardware via deze poort. iDRAC en IPMI toegang zijn standaard ingeschakeld. 

> [!Note]
> De poort iDRAC kunt overslaan van het besturingssysteem en Communiceer rechtstreeks met de hardware op het knooppunt. 

Gebruik deze beveiligingsstrategieën wanneer verbinding te maken en configureren van de iDRAC-poort:

* Alleen verbinding maken met iDRAC poorten met een netwerk dat fysiek van de gegevensnetwerk dat wordt gebruikt gescheiden is voor toegang tot het cluster.
* Een beveiligde iDRAC administrator-wachtwoord instellen op elk knooppunt. U moet dit wachtwoord voor activeren van de hardware - Volg de instructies in ingesteld [hardware wachtwoorden instellen](fxt-node-password.md).
* De standaardconfiguratie voor de poort van iDRAC maakt gebruik van DHCP- en IPv4-IP-adrestoewijzing. Zorg ervoor dat uw omgeving DHCP goed is beveiligd en dat de verbindingen beperkt tussen DHCP-clients en de DHCP-server zijn. (Het cluster van het Configuratiescherm bevat instellingen als u wilt wijzigen van de knooppunten adres configuratiemethode nadat u het cluster hebt gemaakt.)
* Laat de iDRAC-poort ingesteld op 'toegewezen mode' (de standaardinstelling), die netwerkverkeer iDRAC/IPMI beperkt tot de speciale RJ-45-poort.

De poort iDRAC is niet vereist voor een snelle netwerkverbinding.
  
### <a name="serial-port-only-when-necessary"></a>Seriële poort (alleen indien nodig)

In sommige gevallen Microsoft Service and Support achterhalen mogelijk kunt u een terminal verbinden met de seriële poort van een knooppunt een probleem op te sporen.  

Koppelen van de console:

1. Ga naar de seriële poort (COM1) op de achterkant van het knooppunt FXT Edge Filer.
1. Een null-modem-kabel gebruiken de seriële poort verbinden met een geconfigureerd voor ANSI-115200-8N1 terminal.
1. Meld u aan bij de console en andere maatregelen nemen wijze die door ondersteunend personeel.

## <a name="route-cables-in-the-cable-management-arm-cma"></a>De beheertak kabel (CMA) route-kabels

Elk knooppunt Azure FXT Edge Filer wordt geleverd met een optionele kabel beheertak. De CMA vereenvoudigt kabel Routering en biedt eenvoudiger toegang tot de achterkant van het chassis zonder kabels verbreken. 

Volg deze instructies voor het routeren van de kabels via de CMA: 

1. Met behulp van de tie-wordt opgegeven, bundel de kabels samen als ze openen en sluiten van de opslagplaatsen, zodat ze niet aangrenzende systemen (1 storen).
1. Met de CMA op de positie van de service, route de bundel kabel via de binnenste en buitenste-opslagplaatsen (2).
1. Gebruik de vooraf geïnstalleerde haakje en lus banden aan beide uiteinden van de opslagplaatsen voor het beveiligen van de kabels (3).
1. De CMA swing terug naar hun plaats op het systeemvak (4).
1. Gebruik de vooraf geïnstalleerde status indicator kabel aan het einde van het systeem en de kabel beveiligen door deze via de CMA routering. Het andere uiteinde van de kabel koppelen aan de hoek van de buitenste CMA mandje (5). 

   > [!CAUTION]
   > Om te voorkomen van potentiële schade voorkomen van uitstekende kabels, Beveilig een vertraging in de status indicator kabel na deze kabel via de CMA routering. 

![Afbeelding van CMA met kabels geïnstalleerd](media/cma-cabling-scan-400.png)

> [!NOTE]
>  Als u de CMA niet hebt geïnstalleerd, gebruikt u de twee hook en lus banden opgegeven in het spoor kit voor het routeren van de kabels aan het einde van uw systeem.
> 
>  1. Zoek de buitenste CMA vierkante haken aan de binnenkant zijden van beide rackflenzen.
>  2. De kabels voorzichtig, bundelen binnenhalen ze wissen van de systeem-connectors voor de links en rechts.
>  3. De banden haakje en lus via de tooled sleuven op het buitenste CMA vierkante haken voor elke zijde van het systeem voor het beveiligen van de kabel bundels van thread.
> 
>     ![Kabels doorgestuurd zonder een CMA](media/fxt-route-cables-no-cma-400.png)

## <a name="about-ip-address-requirements"></a>Over de vereisten voor IP-adres

Voor hardwareknooppunten in een cache van Azure FXT Edge Filer hybride opslag, worden IP-adressen beheerd door de clustersoftware.

Elk knooppunt moet ten minste één IP-adres, maar knooppuntadressen worden toegewezen wanneer knooppunten zijn toegevoegd aan of verwijderd uit het cluster. 

Het totale aantal IP-adressen die zijn vereist, is afhankelijk van het aantal knooppunten in waaruit de cache. 

Het IP-adresbereik configureren met behulp van de software van het Configuratiescherm nadat de knooppunten zijn geïnstalleerd. Voor meer informatie lezen [verzamelen van informatie voor het cluster](fxt-cluster-create.md#gather-information-for-the-cluster).  

## <a name="connect-power-cables"></a>Verbinding maken met power kabels

Elk knooppunt Azure FXT Edge Filer maakt gebruik van twee power supply eenheden (PSUs). 

> [!TIP] 
> Als u wilt profiteren van de twee redundante PSUs, elke AC-voedingskabel aan een power distribution Units (PDU) in een onafhankelijke vertakking-circuit te koppelen.  
> 
> Voor de PDU's voor extra beveiliging kunt u een Noodvoeding. 

1. De opgenomen stroomkabels verbinden met de PSUs in het chassis. Zorg ervoor dat de kabels en PSUs volledig worden geplaatst. 
1. De stroomkabels koppelen aan de power distribution-eenheden op het rek apparatuur. Gebruik indien mogelijk, twee afzonderlijke energiebronnen voor de twee kabels. 
 
### <a name="power-on-an-azure-fxt-edge-filer-node"></a>Inschakelen van een Azure FXT Edge Filer-knooppunt

Voor het inschakelen van het knooppunt, drukt u op de knop op de voorgrond van het systeem. De knop is op het rechts van het Configuratiescherm. 

### <a name="power-off-an-azure-fxt-edge-filer-node"></a>Een knooppunt Azure FXT Edge Filer uitschakelen

De / uit-knop kan worden gebruikt om het systeem wordt afgesloten tijdens het testen en voordat u deze toevoegt aan een cluster. Nadat een Filer van Azure FXT Edge-knooppunt gebruikt als onderdeel van een cluster wordt, moet u de clustersoftware besturingselement in het deelvenster gebruiken om de hardware af te sluiten. Lezen [hoe veilig uit Azure FXT Edge Filer hardware](fxt-power-off.md) voor meer informatie. 

## <a name="next-steps"></a>Volgende stappen

Nadat u klaar bent met de hardware, power op elk knooppunt bekabeling en ze worden geïnitialiseerd door in te stellen hun root-wachtwoord. 
> [!div class="nextstepaction"]
> [De eerste wachtwoorden instellen](fxt-node-password.md)
