---
title: Microsoft Azure FXT Edge Filer cluster maken
description: Een hybride opslag cache-cluster maken met de Azure FXT Edge Filer
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: 94ec2b088940f4f1f683a4f88ae312879d909bc1
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543532"
---
# <a name="tutorial-create-the-azure-fxt-edge-filer-cluster"></a>Zelfstudie: Maken van het cluster Azure FXT Edge Filer

Nadat u installeert en de Azure FXT Edge Filer hardware-knooppunten voor uw cache initialiseren, moet u de software van de cluster FXT gebruiken om de cachecluster te maken. 

Deze zelfstudie leert u de stappen voor het configureren van uw hardwareknooppunten als een cluster. 

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Welke informatie nodig is voordat u begint met het maken van het cluster
> * Het verschil tussen het beheernetwerk van het cluster, het clusternetwerk en het netwerk clientgerichte
> * Verbinding maken met een clusterknooppunt 
> * Over het maken van een eerste-cluster met één knooppunt van de Azure FXT Edge Filer
> * Hoe u zich aanmeldt bij het cluster van het Configuratiescherm om de clusterinstellingen te configureren

Deze procedure neemt tussen 15 en 45 minuten, afhankelijk van hoeveel onderzoek die u moet doen om u te identificeren van IP-adressen en netwerkbronnen.

## <a name="prerequisites"></a>Vereisten

Voer deze vereisten uit voordat u deze zelfstudie begint:

* Installeren van uw Azure FXT Edge Filer hardware-systemen in uw datacenter 

  U hoeft alleen één knooppunt te maken van het cluster, maar u moet [ten minste twee meer knooppunten toevoegen](fxt-add-nodes.md) voordat u kunt het cluster configureren en u klaar voor gebruik. 

* Juiste kracht en het netwerk kabels verbinden met het systeem  
* Inschakelen op ten minste één Azure FXT Edge Filer knooppunt en [het root-wachtwoord instellen](fxt-node-password.md)

## <a name="gather-information-for-the-cluster"></a>Verzamelen van informatie voor het cluster 

U moet de volgende informatie om de Azure FXT Edge Filer-cluster te maken:

* Clusternaam

* Beheerderswachtwoord in te stellen voor het cluster

* IP-adressen:

  * Één IP-adres voor Clusterbeheer, en het IP-masker en router moet worden gebruikt voor het beheer van netwerk
  * De eerste en laatste IP-adressen in een aaneengesloten reeks IP-adressen voor clustercommunicatie (knooppunten). Zie [IP-adres-distributie](#ip-address-distribution)hieronder voor meer informatie. 
  * (Clientgerichte IP-adressen worden ingesteld nadat de cluster is gemaakt.)

* Informatie over de infrastructuur van netwerk:

  * Het IP-adres van een DNS-server voor het cluster
  * De naam van de DNS-domein voor het cluster
  * De naam of IP-adres voor de cluster NTP-servers (één server of drie of meer) 
  * Of u wilt inschakelen van IEEE 802.1ax-2008 aggregatie op de interfaces van het cluster koppelen
  * Als u koppeling aggregatie, inschakelen of niet gebruik van IEEE 802.3ad (LACP) dynamische aggregatie

Nadat u het cluster maakt, maar is het beter om dat te doen tijdens het maken, kunt u deze items van de infrastructuur netwerk configureren. 

### <a name="ip-address-distribution"></a>IP-adres-distributie

De Azure FXT Edge Filer hybride opslag cache-cluster maakt gebruik van IP-adressen in drie categorieën:

* Beheer van IP: Één IP-adres voor Clusterbeheer

  Dit adres doet dienst als het toegangspunt voor toegang tot de cluster configuratieprogramma's (het web op basis van het Configuratiescherm of de XML-RPC-API). Dit adres wordt automatisch toegewezen aan het primaire knooppunt in het cluster en wordt automatisch verplaatst als het primaire knooppunt wordt gewijzigd.

  Andere IP-adressen kunnen worden gebruikt voor toegang tot het Configuratiescherm, maar het beheer van IP-adres is ontworpen om toegang te bieden, zelfs als failover voor afzonderlijke knooppunten.

* Clusternetwerk: Een bereik van IP-adressen voor communicatie binnen het cluster

  Het clusternetwerk wordt gebruikt voor communicatie tussen clusterknooppunten en bestanden op te halen uit de back endopslag (core filter).

  **Aanbevolen:** Een IP-adres per fysieke poort die wordt gebruikt voor communicatie binnen het cluster op elk knooppunt Azure FXT Edge Filer toewijzen. Het cluster wijst automatisch de adressen in het opgegeven bereik toe aan de afzonderlijke knooppunten.

* Clientgerichte netwerk: Het bereik van IP-adressen die clients moeten gebruiken voor aanvraag- en write-bestanden

  De client-netwerkadressen worden gebruikt door clients toegang krijgen tot de belangrijkste filer gegevens via het cluster. Een NFS-client kan bijvoorbeeld een van deze adressen koppelen.

  **Aanbevolen:** Een IP-adres per fysieke poort die wordt gebruikt voor communicatie van clients op elk knooppunt FXT reeks toewijzen.

  Het cluster wordt clientgerichte IP-adressen zo gelijkmatig mogelijk verspreid over de bijbehorende knooppunten.

  Voor het gemak wordt in veel beheerders één DNS-naam configureren met round robin DNS (RRDNS) configuratie zodat u gemakkelijk om te verdelen van aanvragen van clients over het adresbereik. Met deze instelling kunt ook alle clients de dezelfde mount-opdracht gebruiken voor toegang tot het cluster. Lezen [DNS configureren](fxt-configure-network.md#configure-dns-for-load-balancing) voor meer informatie.

Het beheer van IP-adres en een bereik van netwerkadressen cluster moeten worden opgegeven om een nieuwe cluster te maken. Client-verbonden adressen zijn opgegeven na het maken van het cluster.

## <a name="connect-to-the-first-node"></a>Verbinding maken met het eerste knooppunt

U kunt verbinding maken met een van de geïnstalleerde FXT knooppunten en de OS-software gebruiken voor het instellen van het cluster.

Als u nog niet hebt gedaan, inschakelen op ten minste een van de knooppunten FXT voor uw cluster, en zorg ervoor dat er een netwerkverbinding en een IP-adres. U moet een nieuwe hoofdwachtwoord dus de stappen in het activeren van het knooppunt ingesteld [hardware wachtwoorden instellen](fxt-node-password.md) als u hebt nog niet gedaan.

Om te controleren of de netwerkverbinding, zorg ervoor dat de netwerkverbinding van het knooppunt LED's belicht (en, indien nodig, de indicatoren in het netwerk omzetten die is gekoppeld). -LED's worden beschreven in [Monitor Azure FXT Edge Filer hardwarestatus](fxt-monitor.md).

Wanneer het knooppunt wordt opgestart, wordt er een IP-adres aanvragen. Als deze is verbonden met een DHCP-server, die het IP-adres dat is geleverd door DHCP worden geaccepteerd. (Dit IP-adres is tijdelijk. Dit verandert wanneer u het cluster maakt.)

Als deze niet is verbonden met een DHCP-server of geen reactie ontvangen, wordt het knooppunt Bonjour software gebruiken om in te stellen van een zelf toegewezen IP-adres in het formulier 169.254. \*. \*. U moet echter een tijdelijke statische IP-adres ingesteld op een van de netwerkkaarten van het knooppunt voordat u deze gebruikt om een cluster te maken. Instructies zijn opgenomen in dit document verouderde; Neem contact op met Microsoft-Service en ondersteuning voor bijgewerkte informatie: [Bijlage A: Instellen van een statisch IP-adres op een knooppunt FXT](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/static_ip.html).

### <a name="find-the-ip-address"></a>Het IP-adres zoeken

Verbinding maken met het knooppunt Azure FXT Edge Filer vinden het IP-adres. U kunt een seriële kabel, rechtstreekse verbinding met de USB- en VGA-poorten, gebruiken of verbinding maken via een KVM-switch. (Voor poortverbinding Zie [initiële wachtwoorden instellen](fxt-node-password.md).)

Nadat u verbinding hebt gemaakt, aanmelden met de gebruikersnaam `root` en het wachtwoord dat u hebt ingesteld toen u het knooppunt voor de eerste keer is opgestart.  

Na het aanmelden, moet u bepalen van het knooppunt IP-adres.

Gebruik de opdracht `ifconfig` om te zien van de adressen die zijn toegewezen aan dit systeem.

Bijvoorbeeld, de opdracht `ifconfig | grep -B5 inet` zoekt voor poorten met internet-adressen en biedt vijf regels van context om weer te geven van de poort-id.

Schrijf op elk IP-adres in het rapport ifconfig weergegeven. Adressen die worden weergegeven met de poortnamen van de zoals e0a of e0b zijn goede opties. Gebruik geen IP-adressen worden weergegeven met e7 * namen, omdat deze namen alleen voor servicepoorten iDRAC/IPMI gebruikt worden.  

## <a name="load-the-cluster-configuration-wizard"></a>Laden van de wizard cluster configureren

Gebruik het configuratiehulpprogramma van de cluster op basis van een browser om het cluster te maken. 

Voer het IP-adres voor het knooppunt in een webbrowser. Als de browser een bericht over de site niet wordt vertrouwd geeft, met de site wilt u toch doorgaan. (Afzonderlijke Azure FXT Edge Filer knooppunten geen voor de opgegeven CA beveiligingscertificaten.)

![Besturingselement voor deelvenster aanmeldingspagina in een browservenster](media/fxt-cluster-create/unconfigured-node-gui.png)

Laat de **gebruikersnaam** en **wachtwoord** velden leeg. Klik op **aanmelding** om het cluster maken-pagina te laden.

![Scherm voor het eerste instellen voor een niet-geconfigureerde knooppunt in de browser op basis van GUI-Configuratiescherm. Hier ziet opties voor het bijwerken van software, handmatig configureren van een cluster of configureren van een cluster op basis van een setup-bestand.](media/fxt-cluster-create/setup-first-screen.png)

## <a name="create-the-cluster"></a>Het cluster maken

Het configuratieprogramma cluster leidt u door een reeks schermen om de Azure FXT Edge Filer-cluster te maken. Zorg ervoor dat u hebt de [vereist informatie](#gather-information-for-the-cluster) gereed is voordat u begint. 

### <a name="creation-options"></a>Opties voor het maken

Het eerste scherm biedt drie opties. Gebruik de handmatige configuratie-optie, tenzij er speciale instructies van ondersteunend personeel.

Klik op **ik het cluster handmatig configureert** laden van het scherm Nieuw cluster configuratie-opties. 

De andere opties worden zelden gebruikt:

* 'De systeemkopie bijwerken', vraagt u voor het installeren van nieuwe OS-software voordat het cluster te maken. (De momenteel geïnstalleerde versie wordt weergegeven aan de bovenkant van het scherm.) Moet u een URL en een gebruikersnaam en wachtwoord, ofwel het pakketbestand van de software - opgeven of door het uploaden van een bestand vanaf uw computer. 

* Het hulpprogramma voor het bestand van de cluster-installatieoptie wordt soms gebruikt door de klantenservice van Microsoft. 

## <a name="cluster-options"></a>Clusteropties

Het volgende scherm vraagt u om opties voor het nieuwe cluster te configureren.

De pagina is onderverdeeld in twee gedeelten **basisconfiguratie** en **netwerkconfiguraties**. De configuratiesectie van netwerken heeft ook subsecties: één voor de **Management** netwerk en één voor de **Cluster** netwerk.

### <a name="basic-configuration"></a>Basisconfiguratie

Vul in het bovenste gedeelte basisinformatie voor het nieuwe cluster.

![Details van de sectie "Basisconfiguratie" in browserpagina GUI. Deze drie velden weergegeven (de naam van de cluster, het beheerderswachtwoord, bevestig het wachtwoord)](media/fxt-cluster-create/basic-configuration.png) 

* **Clusternaam** -Voer een unieke naam voor het cluster.

  Naam van het cluster moet voldoen aan deze criteria voldoen:
  
  * Lengte van 1 tot 16 tekens
  * Kan letters, cijfers, en het streepje (-) en onderstrepingstekens (_) bevatten 
  * Mag geen andere interpunctie of speciale tekens bevatten
  
  U kunt deze naam later wijzigen de **Cluster** > **algemene instellingen** configuratiepagina. (Lees voor meer informatie over de instellingen van de [Cluster configuratiehandleiding](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html), dit is geen onderdeel van deze documentatie.)

  > [!NOTE] 
  > De clusternaam van uw wordt gebruikt om systeeminformatie geüpload voor ondersteuning voor bewaking en probleemoplossing, dus is het handig om op te nemen van de naam van uw bedrijf te identificeren.

* **Beheerderswachtwoord** -Stel het wachtwoord voor de gebruiker met beheerdersrechten standaard `admin`.
  
  U moet afzonderlijke gebruikersaccounts instellen voor elke persoon die worden beheerd door het cluster, maar u kunt de gebruiker niet verwijderen `admin`. Meld u als `admin` als u nodig hebt om extra gebruikers te maken.
 
  Kunt u het wachtwoord voor `admin` in de **beheer** > **gebruikers** instellingenpagina in het cluster het Configuratiescherm. Lees voor meer informatie, de **gebruikers** documentatie in de [Cluster configuratiehandleiding](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_users.html).

<!-- to do: update "legacy" URLs when docs are ported to Microsoft site -->

### <a name="network-configuration"></a>Netwerkconfiguratie

De **netwerken** sectie vraagt u om op te geven van de netwerkinfrastructuur die het cluster gebruikt. 

Er zijn twee aparte netwerken te configureren:

* De *beheernetwerk* biedt de beheerderstoegang tot het cluster voor configuratie en bewaking. De hier vermelde IP-adres wordt gebruikt wanneer u verbinding maakt tot het Configuratiescherm of voor SSH-toegang. 

  De meeste clusters slechts één management IP-adres gebruiken, maar als u wilt toevoegen, interfaces kunt u doen na het maken van het cluster.

* De *clusternetwerk* wordt gebruikt voor communicatie tussen clusterknooppunten en tussen clusterknooppunten en back-end-opslag (core filter).

Het netwerk clientgerichte geconfigureerd is later, nadat het cluster is gemaakt.

Deze sectie bevat ook de configuratie van DNS- en NTP-servers die worden gebruikt door beide netwerken.

### <a name="configure-the-management-network"></a>Het beheernetwerk configureren

Instellingen in de **Management** sectie hebben betrekking op het netwerk waarmee de beheerderstoegang tot het cluster.

![Details van de sectie 'Beheer', met velden voor 5 opties en een selectievakje voor 1Gb-beheernetwerk](media/fxt-cluster-create/management-network-filled.png)

* **Beheer van IP** -Geef het IP-adres dat u gebruiken wilt voor toegang tot het cluster het Configuratiescherm. Dit adres wordt door het primaire knooppunt van het cluster worden geclaimd, maar wordt automatisch verplaatst naar een knooppunt in orde als de oorspronkelijke primaire knooppunt niet beschikbaar is.

  De meeste clusters gebruik slechts één IP-adresbeheer. Als u meer dan één wilt, kunt u ze toevoegen nadat u het cluster hebt gemaakt met behulp van de **Cluster** > **beheernetwerk** instellingenpagina. Meer informatie in de [Cluster configuratiehandleiding](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html).

* **IP-masker** -Geef het IP-masker voor het beheernetwerk.

* **Router** -Geef het adres van de standaard-gateway die worden gebruikt door het beheernetwerk.

* **VLAN-label (optioneel)** : als uw cluster maakt gebruik van VLAN-tags, geef de code voor het beheernetwerk.

  Aanvullende VLAN-instellingen zijn geconfigureerd in de **Cluster** > **VLAN** instellingenpagina. Lezen [werken met VLAN's](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html#vlan-overview) en [Cluster > VLAN](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vlan.html) in het Cluster configuratiehandleiding voor meer informatie.

* **MTU** - indien nodig, aanpassen van de maximale verzendeenheid (MTU) voor het beheernetwerk van uw cluster.

* **Gebruik 1Gb mgmt netwerk** -Schakel dit selectievakje in als u wilt de twee 1 GbE-netwerk-poorten op de knooppunten FXT toewijzen met alleen het beheernetwerk. (U kunt 25GbE/10 GbE-poorten die beschikbaar zijn voor al het andere verkeer moet hebben.) Als u dit selectievakje niet inschakelt, gebruikt het beheernetwerk de hoogste snelheid-poort. 

### <a name="configure-the-cluster-network"></a>Configureer het clusternetwerk 

De clusternetwerkinstellingen van toepassing op het verkeer tussen de clusterknooppunten, en tussen clusterknooppunten en core-filter.

![Details van de sectie 'Cluster', met velden zes waarden op te geven](media/fxt-cluster-create/cluster-network-filled.png)

* **Eerste IP-adres** en **laatste IP** -Geef het IP-adressen die het bereik moet worden gebruikt voor communicatie binnen het cluster definiëren. De IP-adressen die hier worden gebruikt, moeten aaneengesloten en niet door DHCP toegewezen.

  U kunt meer IP-adressen toevoegen nadat het cluster is gemaakt. Gebruik de **Cluster** > **clusternetwerken** instellingenpagina ([Cluster configuratiehandleiding documentatie](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cluster_networks.html#gui-cluster-networks)).

  De waarde in **nummer van IP-adressen binnen het bereik** berekend en wordt automatisch weergegeven.

* **Niet-mgmt-netmasker (optioneel)** -Geef het IP-masker voor het clusternetwerk. 

  Het systeem stelt automatisch de waarde van het subnetmasker dat u hebt opgegeven voor het beheernetwerk; Wijzig deze indien nodig.

* **Cluster-router (optioneel)** -Geef het adres van de standaard-gateway wordt gebruikt door de clusternetwerk. 

  Het systeem wordt automatisch het gateway-adres dat u hebt opgegeven voor het beheernetwerk.

* **VLAN-label (optioneel)-cluster** : als uw cluster maakt gebruik van VLAN-tags, geef de code voor het clusternetwerk.

* **Niet-mgmt-MTU (optioneel)** - indien nodig, aanpassen van de maximale verzendeenheid (MTU) voor het clusternetwerk.

### <a name="configure-cluster-dns-and-ntp"></a>Configureren van DNS- en NTP-cluster 

Onder de **Cluster** sectie er zijn velden voor het opgeven van DNS- en NTP-servers en voor het inschakelen van koppeling aggregatie. Deze instellingen gelden voor alle netwerken die gebruikmaakt van het cluster.

![Details van de sectie voor de configuratie van DNS-/ NTP, met drie velden voor DNS-servers, velden voor de DNS-domein en DNS-zoekactie, drie velden voor NTP-servers, en een vervolgkeuzelijst voor aggregatie koppelingsopties](media/fxt-cluster-create/dns-ntp-filled.png)

* **DNS-server (s)** : Geef het IP-adres van een of meer domain name system (DNS)-servers.

  DNS wordt aanbevolen voor alle clusters en vereist als u wilt gebruiken van SMB, AD- of Kerberos. 
  
  Voor optimale prestaties van het cluster DNS-server configureren voor round robin-taakverdeling zoals beschreven in [DNS configureren voor het cluster Azure FXT Edge Filer](fxt-configure-network.md#configure-dns-for-load-balancing).

* **DNS-domein** -Voer de naam van het domein het cluster gebruikt.

* **DNS-zoekactie** : (optioneel), Voer meer domeinnamen die moet worden gezocht om op te lossen van DNS-query's. U kunt maximaal zes domeinnamen, gescheiden door spaties toevoegen.

* **NTP-server (s)** -één of drie netwerk tijd protocol (NTP) servers opgeven in de beschikbare velden. U kunt hostnamen of IP-adressen gebruiken.

* **Koppeling van aggregatie** -Link aggregation kunt u aanpassen hoe het Ethernet-poorten op de clusterknooppunten FXT verschillende soorten verkeer verwerken. Voor meer informatie lezen [Link Aggregation](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cluster_general_setup.html#link-aggregation) in de handleiding voor het Cluster.

### <a name="click-the-create-button"></a>Klik op de knop maken

Na het opgeven van de vereiste instellingen in het formulier, klikt u op de **Cluster maken** knop.

![onderaan ingevuld formulier met de cursor boven de knop maken op rechtsonder](media/fxt-cluster-create/create-cluster.png)

Het systeem wordt een bericht weergegeven tijdens het maken van het cluster.

![statusbericht van cluster-configuratie in de browser: "Het knooppunt FXT is nu het cluster te maken. Dit duurt enkele minuten. Wanneer het cluster is gemaakt, gaat u naar deze koppeling om de configuratie te voltooien." met de hyperlink op "gaat u deze koppeling naar"](media/fxt-cluster-create/creating-message.png)

Na enkele ogenblikken kunt u klikken op de koppeling in het bericht om naar het cluster het Configuratiescherm te gaan. (Deze koppeling gaat u naar het IP-adres dat u hebt opgegeven in **Management IP**.) Het duurt 15 seconden met één minuut voor de koppeling te activeren nadat u op de knop maken. Als de web-interface niet wordt geladen, wacht u enkele seconden en klik vervolgens nogmaals op de koppeling. 

Maken van een cluster duurt een minuut of langer, maar u kunt aanmelden bij het Configuratiescherm tijdens het proces is. Het is normaal voor dashboard-pagina van het Configuratiescherm om weer te geven van waarschuwingen, totdat het proces voor het cluster maken is voltooid. 

## <a name="open-the-settings-pages"></a>Open de instellingen voor pagina 's 

Nadat u het cluster maakt, moet u voor het aanpassen van de configuratie van uw netwerk en de werkstroom. 

Gebruik de webinterface van het Configuratiescherm voor het instellen van het nieuwe cluster. Volg de koppeling van het scherm van uw cluster maken-status, of blader naar het beheer van IP-adres die u hebt ingesteld op het cluster.

Aanmelden bij de webinterface met de gebruikersnaam `admin` en het wachtwoord die u hebt ingesteld bij het maken van het cluster.

![webbrowser met control panel aanmelding velden](media/fxt-cluster-create/admin-login.png)

Het deelvenster Beheer wordt geopend en toont de **Dashboard** pagina. Als het maken van het cluster is voltooid, schakel eventuele waarschuwingsberichten in de weergave.

Klik op de **instellingen** tabblad om de cluster te configureren.

Op de **instellingen** tabblad, in de linkerzijbalk toont een menu van de configuratiepagina's. De pagina's zijn ingedeeld per categorie. Klik op de + of - besturingselement aan de bovenkant van de naam van de categorie te vergroten of te verbergen, de afzonderlijke pagina's.

![Tabblad instellingen van het Configuratiescherm (in browser) met het Cluster > algemene instellingen pagina geladen](media/fxt-cluster-create/settings-tab-populated.png)

## <a name="cluster-setup-steps"></a>Instellingsstappen voor cluster

Op dit moment in het proces, het cluster bestaat, maar er slechts één knooppunt, geen clientgerichte IP-adressen en er is geen back-end-opslag. Extra installatiestappen nodig zijn om te gaan van een nieuw cluster met een cache-systeem dat gereed is voor het verwerken van uw werkstroom.

### <a name="required-configuration"></a>Vereiste configuratie

Deze stappen zijn nodig voor de meeste of alle clusters. 

* Knooppunten toevoegen aan het cluster 

  Drie knooppunten is standaard, maar veel productieclusters hebben meer - tot een maximum van 24-knooppunten.

  Lezen [clusterknooppunten toevoegen](fxt-add-nodes.md) voor meer informatie over andere Azure FXT Edge Filer eenheden toevoegen aan uw cluster en om in te schakelen van hoge beschikbaarheid.

* Back-end opslag opgeven

  Voeg *filer core* definities voor elke back-end-opslagsysteem dat door het cluster wordt gebruikt. Lezen [back-end-opslag toevoegen en configureren van virtuele naamruimte](fxt-add-storage.md#about-back-end-storage) voor meer informatie.

* Instellen van de clienttoegang en de virtuele-naamruimte 

  Maak ten minste één virtuele server (vserver) en wijs het IP-adresbereik voor client-computers te gebruiken. Ook moet u de clusternaamruimte (ook wel de algemene Namespace of GNS), een virtueel bestandssysteem-functie waarmee u uitvoer van de back-end-opslag toewijzen aan virtuele paden. De clusternaamruimte biedt clients de structuur van een bestandssysteem voor consistente en toegankelijk is, zelfs als u back-end opslagmedia. De naamruimte kan ook bieden een gebruiksvriendelijke virtuele opslagapparaat hiërarchie voor Azure Blob-containers of andere ondersteunde cloud-objectopslag.

  Lezen [configureren van de naamruimte](fxt-add-storage.md#configure-the-namespace) voor meer informatie. Deze stap omvat:
  * Het maken van vservers
  * Instellen van koppelingen tussen de client netwerk weergeven en de back-end-opslag 
  * Definiëren welke client-IP-worden adressen bediend door elke vserver

  > [!Note] 
  > Voordat u begint met het instellen van het cluster GNS wordt aanzienlijke planning aanbevolen. Lees de [met behulp van een globale Namespace](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html) en [maken en werken met VServers](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#creating-and-working-with-vservers) secties in het Cluster configuratiehandleiding voor hulp.

* [Netwerkinstellingen aanpassen](fxt-configure-network.md)

  Er zijn verschillende instellingen voor netwerk dat moeten worden geverifieerd of aangepast voor een nieuw cluster. Lezen [netwerkinstellingen aanpassen](fxt-configure-network.md) voor meer informatie over deze items:

  * Configuratie van DNS- en NTP controleren 
  * Configureren van directoryservices, indien nodig 
  * Instellen van VLAN 's
  * Proxy-servers configureren
  * IP-adressen toe te voegen aan het clusternetwerk
  * Opslaan van certificaten voor bestandsversleuteling

* [Ondersteuning voor bewaking hebt ingesteld](#enable-support)

  U moet accepteer het privacybeleid voor het hulpprogramma voor serverconfiguratie en moet u uw instellingen voor de upload ondersteuning configureren op hetzelfde moment.

  Het cluster kan automatisch oplossen van problemen met gegevens over uw cluster, met inbegrip van statistieken en foutopsporing van bestanden uploaden. Deze uploads laat Microsoft Customer Service and Support de best mogelijke service bieden. U kunt aanpassen wat wordt bewaakt, en schakel eventueel de proactieve ondersteuning en de externe service voor het oplossen van problemen.  

### <a name="optional-configuration"></a>Optionele configuratie

Deze stappen zijn niet vereist voor alle clusters. Ze wel nodig zijn voor bepaalde typen werkstromen of voor bepaalde cluster management stijlen. 

* Knooppunt instellingen aanpassen

  U kunt knooppuntnamen en knooppunt IPMI poorten configureren op een cluster-brede niveau of afzonderlijk. Als u deze instellingen configureert voordat u knooppunten toevoegt aan het cluster, kunnen de nieuwe knooppunten kiezen om de instellingen automatisch wanneer ze bij. De opties worden beschreven in het oude cluster maken van het document [knooppunt instellingen aanpassen](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_node.html).

  > [!TIP]
  > Sommige documentatie voor dit product is nog niet beschikbaar op de documentatiesite van Microsoft Azure. Koppelingen naar de [Cluster configuratiehandleiding](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html) en de oudere versie van de [Cluster maken-handleiding](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/create_index.html) gaat u naar een afzonderlijke GitHub gehoste website. 

* SMB configureren

  Als u wilt dat SMB-toegang tot uw cluster, evenals de NFS, moet u SMB configureren en inschakelen. SMB (ook wel CIFS) wordt doorgaans gebruikt ter ondersteuning van Microsoft Windows-clients.

  Plannen en configureren van SMB omvat meer dan een paar knoppen in het Configuratiescherm. Afhankelijk van de vereisten van uw systeem, SMB kan beïnvloeden hoe u core filter, hoeveel vservers die u maakt, definieert hoe u uw koppelingen en naamruimte machtigingen voor toegang en andere instellingen configureren.

  Lees voor meer informatie de handleiding voor Cluster [SMB-toegang configureren](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/smb_overview.html) sectie.

* Aanvullende licenties installeren

  Als u gebruiken van cloud dan Azure Blob-opslag wilt, moet u een extra functie licentie installeren. Neem contact op met uw Microsoft-vertegenwoordiger voor meer informatie over het aanschaffen van een FlashCloud<sup>TM</sup> licentie. Details worden beschreven [back-end-opslag toevoegen en configureren van virtuele naamruimte](fxt-add-storage.md#about-back-end-storage).


### <a name="enable-support"></a>Ondersteuning inschakelen

Het cluster Azure FXT Edge Filer kunt ondersteuningsgegevens over uw cluster automatisch uploaden. Deze uploads, kunnen medewerkers de best mogelijke klantervaring-service.

Volg deze stappen voor het instellen van ondersteuning voor uploads.

1. Navigeer naar de **Cluster** > **ondersteuning** instellingenpagina. Accepteer het privacybeleid. 

   ![Schermopname van het Configuratiescherm en pop-upvenster met bevestigen om het privacybeleid accepteren](media/fxt-cluster-create/fxt-privacy-policy.png)

1. Klik op het driehoekje aan de linkerkant van **klantgegevens** om uit te breiden, de sectie.
1. Klik op de **Revalidate uploadinformatie** knop.
1. Naam van de ondersteuning van het cluster instellen in **unieke clusternaam** -Zorg ervoor dat deze unieke identificatie van uw cluster ter ondersteuning van personeel.
1. Schakel de selectievakjes voor **statistieken bewaking**, **algemene gegevens uploaden**, en **Crash gegevens uploaden**.
1. Klik op **Indienen**.  

   ![Schermafbeelding met voltooide klant info-sectie van de pagina instellingen ondersteuning](media/fxt-cluster-create/fxt-support-info.png)

1. Klik op het driehoekje aan de linkerkant van **Secure proactieve ondersteuning (SP's)** om uit te breiden, de sectie.
1. Schakel het selectievakje voor **inschakelen SP's koppeling**.
1. Klik op **Indienen**.

   ![Schermafbeelding met voltooide proactieve ondersteuning beveiligde sectie op de instellingenpagina voor ondersteuning](media/fxt-cluster-create/fxt-support-sps.png)

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt gemaakt van de basic-cluster en het privacybeleid geaccepteerd, voegt u de rest van de clusterknooppunten toe. 

> [!div class="nextstepaction"]
> [Clusterknooppunten toevoegen](fxt-add-nodes.md)
