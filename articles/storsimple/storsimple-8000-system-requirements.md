---
title: Systeemvereisten voor StorSimple 8000 series | Microsoft Docs
description: Beschrijving van software, netwerken, en hoge beschikbaarheidsvereisten en best practices voor een Microsoft Azure StorSimple-oplossing.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: 1a9cdf31c5924d22d968cd99383417ba371cd1c3
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>StorSimple 8000 series software, hoge beschikbaarheid en netwerkvereisten

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple. Dit artikel worden de systeemvereisten voor belangrijke en aanbevolen procedures voor uw StorSimple-apparaat en voor de opslag-clients toegang tot het apparaat. Het is raadzaam dat u de informatie voordat u uw StorSimple-systeem te implementeren en vervolgens naar deze zo nodig tijdens de implementatie en de volgende bewerking terugverwijzen zorgvuldig te controleren.

De systeemvereisten zijn onder andere:

* **Softwarevereisten voor opslag clients** -beschrijving van de ondersteunde besturingssystemen en eventuele bijkomende vereisten voor deze besturingssystemen.
* **Netwerkvereisten voor de StorSimple-apparaat** -bevat informatie over de poorten die moeten worden geopend in uw firewall om toe te staan voor iSCSI-, cloud of management-verkeer.
* **Vereisten voor hoge beschikbaarheid voor StorSimple** : beschrijft de vereisten voor hoge beschikbaarheid en aanbevolen procedures voor uw StorSimple-apparaat en host-computer.

## <a name="software-requirements-for-storage-clients"></a>Softwarevereisten voor clients van opslag

De volgende softwarevereisten zijn voor de opslag-clients die toegang hebben tot uw StorSimple-apparaat.

| Ondersteunde besturingssystemen | Vereiste versie | Aanvullende vereisten/opmerkingen |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2, 2016 |ISCSI-StorSimple-volumes worden ondersteund voor gebruik op alleen de volgende Windows schijftypen:<ul><li>Eenvoudig volume op een standaardschijf</li><li>Eenvoudige en mirrored volume op een dynamische schijf</li></ul>Alleen de software iSCSI-initiators aanwezig in het besturingssysteem ingebouwd worden ondersteund. Hardware iSCSI-initiators worden niet ondersteund.<br></br>Windows Server 2012 en 2016 thin provisioning en ODX functies worden ondersteund als u een iSCSI-StorSimple-volume.<br><br>StorSimple kunt maken voor thin provisioning en volledig ingerichte volumes. Er kan geen gedeeltelijk ingerichte volumes maken.<br><br>Een thin provisioning volume formatteren kan lang duren. Het is raadzaam om het verwijderen van het volume en maak vervolgens een nieuwe in plaats van het formatteren. Echter, als u liever een volume formatteren:<ul><li>Voer de volgende opdracht voordat de herformatteren om ruimte vrijmaken vertragingen te voorkomen: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Nadat de opmaak voltooid is, moet u de volgende opdracht gebruiken voor het vrijmaken van ruimte opnieuw in te schakelen:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>De Windows Server 2012-hotfix toepassen, zoals beschreven in [KB 2878635](https://support.microsoft.com/kb/2870270) op uw Windows Server-computer.</li></ul></li></ul></ul> Als u voor SharePoint StorSimple Snapshot Manager of een StorSimple-Adapter configureert, gaat u naar [softwarevereisten voor de optionele componenten](#software-requirements-for-optional-components). |
| VMware ESX |5.5 en 6.0 |Ondersteund met VMware vSphere als iSCSI-client. VAAI-block-functie wordt ondersteund met VMware vSphere op StorSimple-apparaten. |
| Linux RHEL/CentOS |5, 6 en 7 |Ondersteuning voor Linux iSCSI-clients met open-iSCSI-initiator versie 5, 6 en 7. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX wordt momenteel niet ondersteund met StorSimple.


## <a name="software-requirements-for-optional-components"></a>Softwarevereisten voor optionele onderdelen

De volgende softwarevereisten zijn voor de optionele componenten StorSimple (StorSimple Snapshot Manager en StorSimple-Adapter voor SharePoint).

| Onderdeel | Hostplatform | Aanvullende vereisten/opmerkingen |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |Gebruik van StorSimple Snapshot Manager op Windows Server is vereist voor back-up/herstel van gespiegelde dynamische schijven en voor eventuele toepassingsconsistente back-ups.<br> StorSimple Snapshot Manager wordt alleen ondersteund op Windows Server 2008 R2 SP1 (64-bits), Windows Server 2012 R2 en Windows Server 2012.<ul><li>Als u van venster Server 2012 gebruikmaakt, moet u .NET 3.5 – 4.5 installeren voordat u StorSimple Snapshot Manager installeert.</li><li>Als u van Windows Server 2008 R2 SP1 gebruikmaakt, moet u Windows Management Framework 3.0 installeren voordat u StorSimple Snapshot Manager installeert.</li></ul> |
| StorSimple Adapter voor SharePoint |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>StorSimple-Adapter voor SharePoint wordt alleen ondersteund in SharePoint 2010 en SharePoint 2013.</li><li>Resourcestructuur vereist SQL Server Enterprise Edition van versie 2008 R2 of 2012.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>Netwerkvereisten voor uw StorSimple-apparaat

Uw StorSimple-apparaat is een apparaat vergrendeld. Poorten moeten echter worden geopend in uw firewall om toe te staan voor de iSCSI-, cloud en beheer van verkeer. De volgende tabel bevat de poorten die moeten worden geopend in uw firewall. In deze tabel *in* of *inkomende* verwijst naar de richting waarin binnenkomende aanvragen van clients toegang uw apparaat tot. *Uitgaand* of *uitgaande* verwijst naar de richting waarin uw StorSimple-apparaat gegevens extern, afgezien van de implementatie verzendt: bijvoorbeeld uitgaand naar het Internet.

| Poort nr.<sup>1,2</sup> | In- of uitzoomen | Poort-bereik | Vereist | Opmerkingen |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP)<sup>3</sup> |out |WAN |Nee |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot Internet voor het ophalen van updates.</li><li>De webproxy uitgaande is gebruiker worden geconfigureerd.</li><li>Als u wilt toestaan systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li></ul> |
| TCP 443 (HTTPS)<sup>3</sup> |out |WAN |Ja |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud.</li><li>De webproxy uitgaande is gebruiker worden geconfigureerd.</li><li>Als u wilt toestaan systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li><li>Deze poort wordt ook gebruikt op beide domeincontrollers voor garbagecollection.</li></ul> |
| UDP 53 (DNS) |out |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u een Internet-gebaseerd DNS-server gebruikt. |
| UDP 123 (NTP) |out |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u een Internet-gebaseerd NTP-server gebruikt. |
| TCP 9354 |out |WAN |Ja |De uitgaande poort wordt gebruikt door het StorSimple-apparaat om te communiceren met de service Manager voor StorSimple-apparaat. |
| 3260 (iSCSI) |in |LAN |Nee |Deze poort wordt gebruikt voor toegang tot gegevens via iSCSI. |
| 5985 |in |LAN |Nee |Binnenkomende poort wordt door StorSimple Snapshot Manager gebruikt om te communiceren met de StorSimple-apparaat.<br>Deze poort wordt ook gebruikt wanneer u extern verbinding met Windows PowerShell voor StorSimple via HTTP maken. |
| 5986 |in |LAN |Nee |Deze poort wordt gebruikt wanneer u extern verbinding met Windows PowerShell voor StorSimple via HTTPS maken. |

<sup>1</sup> geen poorten voor inkomend verkeer moeten worden geopend op het openbare Internet.

<sup>2</sup> als meerdere poorten uitvoeren voor de configuratie van een gateway, de volgorde gerouteerde het uitgaande verkeer wordt bepaald op basis van de poort routering volgorde zoals beschreven in [poort routering](#routing-metric)onderstaande.

<sup>3</sup> de controller vaste IP-adressen op uw StorSimple-apparaat moet routeerbaar zijn en verbinding maken met het Internet rechtstreeks of via de geconfigureerde webproxy. De vaste IP-adressen worden gebruikt voor het onderhoud van de updates op het apparaat en garbagecollection. Als de apparaatcontrollers kunnen geen verbinding met Internet via de vaste IP-adressen, u zich niet uw StorSimple-apparaat bijwerken en garbagecollection werkt niet goed.

> [!IMPORTANT]
> Zorg ervoor dat de firewall niet wijzigen of ontsleutelen van een SSL-verkeer tussen de StorSimple-apparaat en Azure.


### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewallregels

Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van het inkomende en uitgaande verkeer configureren. Uw StorSimple-apparaat en de service Manager voor StorSimple-apparaat, is afhankelijk van andere Microsoft-toepassingen zoals Azure Service Bus, Azure Active Directory-toegangsbeheer, storage-accounts en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt voor het configureren van firewallregels. Het is belangrijk te weten dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Op zijn beurt hiervoor moet de netwerkbeheerder om te controleren en firewallregels bijwerken voor uw StorSimple als en indien nodig.

Het is raadzaam dat u de firewallregels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple ingesteld. U kunt echter de onderstaande informatie geavanceerde firewall-regels die nodig zijn voor het maken van beveiligde omgevingen in te stellen.

> [!NOTE]
> Het apparaat (bron) IP-adressen moet altijd worden ingesteld op de ingeschakelde netwerkinterfaces. De bestemming IP-adressen moet worden ingesteld op [Azure datacenter IP-adresbereiken](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


#### <a name="url-patterns-for-azure-portal"></a>URL-patronen voor Azure-portal

| URL-patroon | Onderdeel/functionaliteit | Apparaat-IP-adressen |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |StorSimple-apparaatbeheerservice<br>Access Control Service<br>Azure Service Bus<br>Authentication-Service |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.backup.windowsazure.com` |Apparaatregistratie |Alleen DATA 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Intrekken van certificaten |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-accounts en controle |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update-servers<br> |Controller vaste IP-adressen alleen |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Controller vaste IP-adressen alleen |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Ondersteuningspakket |Netwerkinterfaces die zijn ingeschakeld voor de cloud |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-patronen voor Azure Government portal

| URL-patroon | Onderdeel/functionaliteit | Apparaat-IP-adressen |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login.microsoftonline.us` |StorSimple-apparaatbeheerservice<br>Access Control Service<br>Azure Service Bus<br>Authentication-Service |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.backup.windowsazure.us` |Apparaatregistratie |Alleen DATA 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Intrekken van certificaten |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-accounts en controle |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update-servers<br> |Controller vaste IP-adressen alleen |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Controller vaste IP-adressen alleen |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Ondersteuningspakket |Netwerkinterfaces die zijn ingeschakeld voor de cloud |

### <a name="routing-metric"></a>Routering

Een metriek routering is gekoppeld aan de interfaces en de gateway die voor de gegevens naar de opgegeven netwerken te routeren. Routering wordt gebruikt door het routeringsprotocol voor het berekenen van het beste pad naar een bepaalde bestemming als deze achterhaalt met dat meerdere paden bestaan met hetzelfde doel. Hoe lager de route-metric, hoe hoger de voorkeur.

In de context van StorSimple, als er meerdere netwerkinterfaces en gateways zijn geconfigureerd voor het kanaalverkeer, de routering metrische gegevens wordt een rol spelen om te bepalen van de relatieve volgorde waarin de interfaces wordt ophalen gebruikt. De routering metrische gegevens kan niet worden gewijzigd door de gebruiker. U kunt echter de `Get-HcsRoutingTable` cmdlet om af te drukken de routering tabel (en metrische gegevens) op uw StorSimple-apparaat. Meer informatie over de cmdlet Get-HcsRoutingTable in [StorSimple probleemoplossing implementatie](storsimple-troubleshoot-deployment.md).

De routering metrische algoritme gebruikt voor de Update 2 en hoger kan als volgt worden verklaard.

* Een reeks vooraf bepaalde waarden zijn toegewezen aan netwerkinterfaces.
* U kunt een van de voorbeeldtabel hieronder wordt weergegeven met waarden die zijn toegewezen aan de verschillende netwerkinterfaces wanneer ze zijn ingeschakeld voor de cloud of cloud-uitgeschakeld, maar met een geconfigureerde gateway. Noteer de waarden die hier worden toegewezen, zijn alleen voorbeeldwaarden.

    | Netwerkinterface | Ingeschakeld voor de cloud | Cloud-uitgeschakeld met gateway |
    |-----|---------------|---------------------------|
    | Data 0  | 1            | -                        |
    | Gegevens-1  | 2            | 20                       |
    | Data 2  | 3            | 30                       |
    | Gegevens 3  | 4            | 40                       |
    | Data 4  | 5            | 50                       |
    | Gegevens 5  | 6            | 60                       |


* De volgorde waarin het cloud-verkeer wordt gerouteerd via de netwerkinterfaces is:
  
    *Data 0 > gegevens 1 > datum 2 > gegevens 3 > gegevens 4 > gegevens 5*
  
    Dit kan worden verklaard door het volgende voorbeeld.
  
    U kunt een virtueel StorSimple-apparaat met twee ingeschakeld voor de cloud netwerkinterfaces, Data 0 en 5 van de gegevens. Gegevens van 1 tot en met 4 van de gegevens zijn cloud-uitgeschakeld, maar een geconfigureerde gateway hebben. De volgorde waarin verkeer wordt gerouteerd voor dit apparaat zijn:
  
    *Data 0 (1) > gegevens 5 (6) > gegevens 1 (20) > gegevens (30) 2 > gegevens 3 (40) > gegevens 4 (50)*
  
    *De getallen tussen haakjes staan de respectieve routering metrische gegevens.*
  
    Als de Data 0 is mislukt, wordt het verkeer van de cloud via Data 5 ophalen gerouteerd. Gezien het feit dat een gateway is geconfigureerd op alle andere netwerken, als zowel Data 0 en 5 van de gegevens is mislukt, gaat het cloud-verkeer via Data 1.
* Als een netwerkinterface met ingeschakeld voor de cloud is mislukt, waarna u 3 nieuwe pogingen met een 30 tweede vertraging verbinding maken met de interface zijn. Als de nieuwe pogingen mislukken, wordt het verkeer wordt doorgestuurd naar de volgende beschikbare ingeschakeld voor de cloud interface zoals wordt bepaald door de routeringstabel. Als alle de ingeschakeld voor de cloud netwerkinterfaces mislukken, wordt klikt u vervolgens het apparaat failover naar de andere controller (opnieuw opstarten is niet in dit geval).
* Als er een VIP-fout voor een netwerkinterface met iSCSI-is ingeschakeld, zal er 3 nieuwe pogingen met een vertraging 2 seconden. Dit gedrag is gebleven hetzelfde uit de vorige versies. Als alle netwerkinterfaces van iSCSI-mislukken, wordt een failover van de domeincontroller (vergezeld van de computer opnieuw is opgestart) optreden.
* Een waarschuwing wordt ook gegeven op uw StorSimple-apparaat wanneer er een VIP-fout. Ga voor meer informatie naar [Naslaggids waarschuwing](storsimple-8000-manage-alerts.md).
* In termen van nieuwe pogingen voorrang iSCSI boven cloud.
  
    Bekijk het volgende voorbeeld: A StorSimple apparaat twee netwerkinterfaces die zijn ingeschakeld heeft, Data 0 en 1 van de gegevens. Data 0 is ingeschakeld voor de cloud terwijl Data 1 zowel cloud- en iSCSI-functionaliteit. Er zijn geen andere netwerkinterfaces op dit apparaat zijn ingeschakeld voor cloud- of iSCSI.
  
    Als Data 1 mislukt, krijgt deze de laatste iSCSI-netwerkinterface is, wordt hierdoor een failover van de domeincontroller op gegevens-1 op de andere controller.

### <a name="networking-best-practices"></a>Aanbevolen procedures voor netwerken

Voldoen aan de volgende best practices naast de bovenstaande netwerkvereisten voor de optimale prestaties van uw StorSimple-oplossing:

* Zorg ervoor dat uw StorSimple-apparaat heeft een speciale 40 Mbps bandbreedte (of meer) beschikbaar te allen tijde. Deze bandbreedte mag niet worden gedeeld (of toewijzing moet worden gegarandeerd door het gebruik van QoS-beleid) met andere toepassingen.
* Controleer de netwerkverbinding met het Internet altijd beschikbaar is. Sporadisch of onbetrouwbare Internet-verbindingen naar de apparaten, met inbegrip van welke aard dan ook, zonder internetverbinding resulteert in een niet-ondersteunde configuratie.
* Het iSCSI- en cloud-verkeer te isoleren door een speciale netwerkinterfaces op uw apparaat voor toegang tot iSCSI- en cloud. Zie voor meer informatie hoe [netwerkinterfaces wijzigen](storsimple-8000-modify-device-config.md#modify-network-interfaces) op uw StorSimple-apparaat.
* Gebruik geen een Link Aggregation Control Protocol (LACP)-configuratie voor uw netwerkinterfaces. Dit is een niet-ondersteunde configuratie.

## <a name="high-availability-requirements-for-storsimple"></a>Vereisten voor hoge beschikbaarheid voor StorSimple

De hardwareplatform die is opgenomen in de StorSimple-oplossing heeft beschikbaarheid en betrouwbaarheid van functies die kunnen worden gebruikt voor een maximaal beschikbare en fouttolerantie opslaginfrastructuur in uw datacenter. Er zijn echter vereisten en aanbevolen procedures die u voldoen moet om ervoor te zorgen, de beschikbaarheid van uw StorSimple-oplossing. Voordat u StorSimple implementeert, zorgvuldig door de volgende vereisten en aanbevolen procedures voor de StorSimple-apparaat en de verbonden hostcomputers.

Voor meer informatie over het controleren en onderhouden van de hardwareonderdelen van uw StorSimple-apparaat, gaat u naar [gebruik van de service Manager voor StorSimple-apparaat voor hardware-onderdelen bewaken en de status](storsimple-8000-monitor-hardware-status.md) en [StorSimple onderdeel Hardwarevervanging](storsimple-8000-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Vereisten voor hoge beschikbaarheid en -procedures voor uw StorSimple-apparaat

Controleer de volgende informatie zorgvuldig om ervoor te zorgen de hoge beschikbaarheid van uw StorSimple-apparaat.

#### <a name="pcms"></a>PCMs

StorSimple-apparaten zijn redundante, hot verwisselbare stroom en koeling modules (PCMs). Elke PCM heeft onvoldoende capaciteit om te voorzien van de service voor het hele chassis. Hoge beschikbaarheid, zodat moeten beide PCMs worden geïnstalleerd.

* Verbinding maken met uw PCMs verschillende energiebeheerschema bronnen voor beschikbaarheid als een power-bron is mislukt.
* Als een PCM mislukt, vragen u een vervangende onmiddellijk.
* Een mislukte PCM alleen verwijderd wanneer u de vervangende hebt en klaar bent om te installeren.
* Beide PCMs niet gelijktijdig verwijderen. De PCM-module bevat de back-up batterij-module. Wanneer zowel de PCMs afsluiten zonder accu beveiliging wordt verwijderd en de apparaatstatus wordt niet opgeslagen. Voor meer informatie over de batterij, gaat u naar [onderhouden van de back-up batterij module](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Controller modules

StorSimple-apparaten zijn redundante, hot verwisselbare controller modules. De domeincontroller-modules werken in een actief/passief-wijze. Op elk gewenst één domeincontroller module actief is en verleent service, terwijl de domeincontroller-module passief is. De controllermodule passieve is ingeschakeld en werkt als de actieve controller-module is mislukt of is verwijderd. Elke domeincontroller-module heeft onvoldoende capaciteit om te voorzien van de service voor het hele chassis. Beide controller-modules moeten worden geïnstalleerd om ervoor te zorgen hoge beschikbaarheid.

* Zorg ervoor dat beide modules controller te allen tijde worden geïnstalleerd.
* Als een domeincontroller-module is mislukt, vragen u een vervangende onmiddellijk.
* Een mislukte controllermodule alleen verwijderd wanneer u de vervangende hebt en klaar bent om te installeren. Verwijderen van een module voor langere perioden van invloed zijn op de luchtstroom en wordt daarmee het koelen van het systeem.
* Zorg ervoor dat de netwerkverbindingen op beide modules controller identiek zijn, en een identieke netwerkconfiguratie van de verbonden netwerkinterfaces hebben.
* Als een module van de domeincontroller mislukt of vervanging moet, zorg er dan voor dat de domeincontroller-module is een actieve status heeft voordat de mislukte controllermodule worden vervangen. Om te bevestigen dat een domeincontroller actief is, gaat u naar [identificeren van de actieve controller op uw apparaat](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Verwijder zowel modules van de domeincontroller niet op hetzelfde moment. Als een domeincontroller failover uitgevoerd wordt, geen afsluiten van de controllermodule stand-by-of verwijderen uit het chassis.
* Na een failover van de domeincontroller, wacht ten minste vijf minuten voordat u een controllermodule verwijdert.

#### <a name="network-interfaces"></a>Netwerkinterfaces

StorSimple-apparaat controller modules elke hebben vier 1 Gigabit en twee 10 Gigabit Ethernet-netwerkinterfaces.

* Zorg ervoor dat de netwerkverbindingen op beide modules controller identiek zijn en dat de domeincontroller module interfaces zijn verbonden met een identieke netwerkconfiguratie netwerkinterfaces.
* Indien mogelijk, implementeert u netwerkverbindingen tussen verschillende switches om te controleren of de beschikbaarheid van de service in geval van een apparaat netwerkstoring.
* Wanneer ontkoppelen de enige of de laatste resterende ingeschakeld voor iSCSI-interface (met IP-adressen toegewezen), de interface eerst uitschakelen en haal de kabels. Als de interface eerst losgekoppeld is, wordt deze de actieve controller failover naar de passieve controller. Als de passieve controller ook de bijbehorende interfaces losgekoppeld heeft, kunnen beide domeincontrollers wordt meerdere keren opnieuw voordat op een domeincontroller.
* Ten minste twee interfaces zijn gegevens verbinding met het netwerk van elke domeincontroller-module.
* Als u de twee hebt ingeschakeld 10 GbE-interfaces, de servers in verschillende switches implementeren.
* Gebruik zo mogelijk MPIO op servers om ervoor te zorgen dat de servers een koppeling-, netwerk- of interfacefout kunnen tolereren.

Voor meer informatie over uw apparaat voor hoge beschikbaarheid en prestaties van netwerken, gaat u naar [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD en HDD 's

StorSimple-apparaten zijn Solid-State-schijven (SSD's) en harde schijven (HDD's) die zijn beveiligd met behulp van gespiegelde opslagruimten. Gebruik van gespiegelde ruimten zorgt ervoor dat het apparaat kunnen tolereren het uitvallen van een of meer SSD's of harde schijven.

* Zorg ervoor dat alle SSD en HDD-modules zijn geïnstalleerd.
* Als een SSD of harde schijf is mislukt, vragen u een vervangende onmiddellijk.
* Als een SSD of harde schijf uitvalt of moet worden vervangen, zorg er dan voor dat u verwijdert alleen de SSD- of harde schijf die moet worden vervangen.
* Verwijder niet meer dan één SSD of harde schijf uit het systeem op elk punt in tijd.
  Een storing van 2 of meer schijven van een bepaald type (HDD, SSD) of opeenvolgende mislukt binnen een korte periode kan leiden tot gegevensverlies storing en potentiële system. Als dit gebeurt, [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor hulp.
* Tijdens de vervanging, controleren de **gedeelde onderdelen** in de **Hardware health** blade voor de stations in de SSD's en HDD's. De status van een groen vinkje geeft aan dat de schijven zijn in orde of OK, terwijl een rood uitroepteken wijst wijst op een mislukte SSD of harde schijf.
* Het is raadzaam dat u configureert cloudmomentopnamen voor alle volumes die u wilt beveiligen in geval van een systeemfout is opgetreden.

#### <a name="ebod-enclosure"></a>EBOD behuizing

StorSimple-Apparaatmodel 8600 bevat een uitgebreide Bunch van schijven (EBOD) behuizing naast de primaire behuizing. Een EBOD bevat EBOD domeincontrollers en harde schijven (HDD's) die zijn beveiligd met behulp van gespiegelde opslagruimten. Gebruik van gespiegelde ruimten zorgt ervoor dat het apparaat kunnen het uitvallen van een of meer HDD tolereren. De behuizing EBOD is verbonden met de primaire behuizing door middel van redundante SAS-kabels.

* Zorg ervoor dat beide EBOD behuizing controller modules, zowel SAS-kabels en alle stations op de harde schijf te allen tijde worden geïnstalleerd.
* Als een EBOD behuizing controller-module is mislukt, vragen u een vervangende onmiddellijk.
* Als een EBOD behuizing controller-module is mislukt, zorg ervoor dat de domeincontroller-module actief is voordat u de mislukte module vervangt. Om te bevestigen dat een domeincontroller actief is, gaat u naar [identificeren van de actieve controller op uw apparaat](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Tijdens een EBOD controller module-vervanging voortdurend de status controleren van het onderdeel in de service Manager voor StorSimple-apparaat met toegang tot **Monitor** > **Hardware health**.
* Als een SAS-kabel mislukt of moet worden vervangen (Microsoft Support moet worden gebruikt om dergelijke bepalen), zorg er dan voor dat u de SAS-kabel die moet worden vervangen.
* Gelijktijdig Verwijder niet beide SAS-kabels uit het systeem op elk punt in tijd.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Aanbevelingen voor hoge beschikbaarheid voor uw hostcomputers

Zorgvuldig door deze aanbevolen procedures om te controleren of de hoge beschikbaarheid van de hosts die zijn verbonden met uw StorSimple-apparaat.

* Configureren van StorSimple met [twee knooppunten cluster bestandsserverconfiguraties][1]. Individuele foutpunten fout en opbouw in redundantie aan de kant van de host wordt verwijderd, wordt de hele oplossing maximaal beschikbaar.
* Voortdurend beschikbare shares voor (CA) beschikbaar in Windows Server 2012 (SMB 3.0) gebruiken voor maximale beschikbaarheid tijdens de failover van de opslagcontrollers. Raadpleeg voor aanvullende informatie voor het configureren van bestandsserverclusters en voortdurend beschikbare shares met Windows Server 2012 naar deze [video demo](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over limieten voor StorSimple-systeem](storsimple-8000-limits.md).
* [Informatie over het implementeren van uw StorSimple-oplossing](storsimple-8000-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
