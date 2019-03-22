---
title: Systeemvereisten voor StorSimple 8000-reeks | Microsoft Docs
description: Beschrijving van software, netwerken, en vereisten voor hoge beschikbaarheid en aanbevolen procedures voor een Microsoft Azure StorSimple-oplossing.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: f05e3e85d36ffc23a193a6771a0271c71b2f8544
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013638"
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>StorSimple 8000-serie software, hoge beschikbaarheid en netwerkvereisten

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple. Dit artikel beschrijft de vereisten voor belangrijke en aanbevolen procedures voor uw StorSimple-apparaat en voor de opslagclients toegang tot het apparaat. Het is raadzaam dat u de informatie voordat u uw StorSimple-systeem implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en latere bewerking verwijzen zorgvuldig te controleren.

De systeemvereisten zijn onder andere:

* **Softwarevereisten voor opslagclients** -beschrijving van de ondersteunde besturingssystemen en eventuele bijkomende vereisten voor deze besturingssystemen.
* **Netwerkvereisten voor de StorSimple-apparaat** -vindt u informatie over de poorten die moeten worden geopend in uw firewall om toe te staan voor iSCSI-, cloud of beheer van verkeer.
* **Vereisten voor hoge beschikbaarheid voor StorSimple** : beschrijft de vereisten voor hoge beschikbaarheid en aanbevolen procedures voor uw StorSimple-apparaat en de host-computer.

## <a name="software-requirements-for-storage-clients"></a>Softwarevereisten voor opslagclients

De volgende vereisten zijn voor de opslagclients die toegang hebben tot uw StorSimple-apparaat.

| Ondersteunde besturingssystemen | Vereiste versie | Aanvullende vereisten/opmerkingen |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2, 2016 |StorSimple iSCSI-volumes worden voor gebruik op alleen de volgende Windows-schijftypen ondersteund:<ul><li>Eenvoudig volume op de standaardschijf</li><li>Eenvoudige en gespiegelde volume op de dynamische schijf</li></ul>Alleen de software iSCSI-initiators aanwezig in het besturingssysteem worden ondersteund. ISCSI-initiators hardware worden niet ondersteund.<br></br>Windows Server 2012 en 2016 thin provisioning en ODX functies worden ondersteund als u een StorSimple-iSCSI-volume.<br><br>StorSimple kunt maken dun ingerichte en volledig ingerichte volumes. Er kan geen gedeeltelijk ingerichte volumes maken.<br><br>Een dun ingerichte volume formatteren, kan lange tijd duren. U wordt aangeraden verwijderen van het volume en maak vervolgens een nieuwe in plaats van opnieuw formatteren. Echter, als u nog steeds wil een volume formatteren:<ul><li>Voer de volgende opdracht uit voordat u de herformatteren om ruimte vrijmaken vertragingen te voorkomen: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Nadat de opmaak voltooid is, gebruikt u de volgende opdracht opnieuw inschakelen vrijmaken van ruimte:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>De Windows Server 2012-hotfix toepassen, zoals beschreven in [KB 2878635](https://support.microsoft.com/kb/2870270) op uw Windows Server-computer.</li></ul></li></ul></ul> Als u StorSimple Snapshot Manager of een StorSimple-Adapter voor SharePoint configureert, gaat u naar [softwarevereisten beschreven voor het optionele componenten](#software-requirements-for-optional-components). |
| VMware ESX |5.5 en 6.0 |Ondersteund met VMware vSphere als iSCSI-client. VAAI blok-functie wordt ondersteund met VMware vSphere op StorSimple-apparaten. |
| Linux RHEL/CentOS |5, 6 en 7 |Ondersteuning voor Linux-iSCSI-clients met open met iSCSI-initiator versie 5, 6 en 7. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX wordt momenteel niet ondersteund met StorSimple.


## <a name="software-requirements-for-optional-components"></a>Softwarevereisten voor optionele onderdelen

De volgende vereisten zijn voor de optionele componenten voor StorSimple (StorSimple Snapshot Manager en StorSimple Adapter voor SharePoint).

| Onderdeel | Hostplatform | Aanvullende vereisten/opmerkingen |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |Het gebruik van de StorSimple Snapshot Manager op Windows Server is vereist voor back-up of herstellen van gespiegelde dynamische schijven en voor eventuele toepassingsconsistente back-ups.<br> StorSimple Snapshot Manager wordt alleen ondersteund op Windows Server 2008 R2 SP1 (64-bits), Windows Server 2012 R2 en Windows Server 2012.<ul><li>Als u van venster Server 2012 gebruikmaakt, moet u .NET 3.5 – 4.5 installeren voordat u StorSimple Snapshot Manager installeert.</li><li>Als u van Windows Server 2008 R2 SP1 gebruikmaakt, moet u Windows Management Framework 3.0 installeren voordat u StorSimple Snapshot Manager installeert.</li></ul> |
| StorSimple Adapter voor SharePoint |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>StorSimple Adapter voor SharePoint wordt alleen ondersteund op SharePoint 2010 en SharePoint 2013.</li><li>Resourcestructuur vereist SQL Server Enterprise Edition, versie 2008 R2 of 2012.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>Netwerkvereisten voor uw StorSimple-apparaat

Uw StorSimple-apparaat is een apparaat vergrendeld. Echter, moeten de poorten kunnen worden geopend in uw firewall om toe te staan voor de iSCSI-, cloud en beheer van verkeer. De volgende tabel staan de poorten die moeten worden geopend in uw firewall. In deze tabel *in* of *inkomende* verwijst naar de richting waarin binnenkomende aanvragen van clients toegang uw apparaat tot. *Uit* of *uitgaande* verwijst naar de richting waarin uw StorSimple-apparaat gegevens extern, na de implementatie verzendt: bijvoorbeeld uitgaand naar het Internet.

| Nee. de poort<sup>1,2</sup> | In- of uitschalen | Poort-bereik | Vereist | Opmerkingen |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP)<sup>3</sup> |Uit |WAN |Nee |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot Internet om op te halen van updates.</li><li>De webproxy uitgaande is gebruiker worden geconfigureerd.</li><li>Als u wilt toestaan dat systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li></ul> |
| TCP 443 (HTTPS)<sup>3</sup> |Uit |WAN |Ja |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud.</li><li>De webproxy uitgaande is gebruiker worden geconfigureerd.</li><li>Als u wilt toestaan dat systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li><li>Deze poort wordt ook gebruikt op beide controllers voor garbagecollection.</li></ul> |
| UDP 53 (DNS) |Uit |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u van een op basis van een Internet-DNS-server gebruikmaakt. |
| UDP 123 (NTP) |Uit |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u een Internet-gebaseerde NTP-server gebruikt. |
| TCP 9354 |Uit |WAN |Ja |De uitgaande poort wordt gebruikt door het StorSimple-apparaat om te communiceren met de StorSimple Device Manager-service. |
| 3260 (iSCSI) |In |LAN |Nee |Deze poort wordt gebruikt voor toegang tot gegevens via iSCSI. |
| 5985 |In |LAN |Nee |Binnenkomende poort wordt door StorSimple Snapshot Manager gebruikt om te communiceren met het StorSimple-apparaat.<br>Deze poort wordt ook gebruikt wanneer u op afstand verbinding met Windows PowerShell voor StorSimple via HTTP maken. |
| 5986 |In |LAN |Nee |Deze poort wordt gebruikt wanneer u op afstand verbinding met Windows PowerShell voor StorSimple via HTTPS maken. |

<sup>1</sup> geen inkomende poorten moeten worden geopend op het openbare Internet.

<sup>2</sup> als meerdere poorten een gatewayconfiguratie voeren, de volgorde gerouteerde het uitgaande verkeer wordt bepaald op basis van de routering poortnummer dat wordt beschreven in [poort routering](#routing-metric)hieronder.

<sup>3</sup> de controller vaste IP-adressen op uw StorSimple-apparaat moet routeerbaar zijn en geen verbinding maken met Internet rechtstreeks of via de geconfigureerde webproxy. De vaste IP-adressen worden gebruikt voor het uitvoeren van de updates op het apparaat en voor garbagecollection. Als de apparaatcontrollers kunnen geen verbinding met Internet via de vaste IP-adressen maken, kunt u zich niet bijwerken van uw StorSimple-apparaat en garbagecollection werkt niet goed.

> [!IMPORTANT]
> Zorg ervoor dat de firewall niet wijzigen of ontsleutelen van een SSL-verkeer tussen de StorSimple-apparaat en Azure.


### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewall-regels

Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van de inkomende en uitgaande verkeer configureren. Uw StorSimple-apparaat en de StorSimple Device Manager-service, is afhankelijk van andere Microsoft-toepassingen, zoals Azure Service Bus, toegangsbeheer van Azure Active Directory, opslagaccounts en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt om de firewall-regels configureren. Het is belangrijk om te begrijpen dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Op zijn beurt hiervoor moet de netwerkbeheerder om te controleren en firewallregels bijwerken voor uw StorSimple als en wanneer dat nodig is.

U wordt aangeraden dat u uw firewall-regels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple instellen. Echter, kunt u de onderstaande informatie om in te stellen van geavanceerde firewallregels die nodig zijn om beveiligde omgevingen te maken.

> [!NOTE]
> Het apparaat (bron) IP-adressen moet altijd worden ingesteld op de ingeschakelde netwerkinterfaces. De IP-adressen moet worden ingesteld op bestemming [Azure datacenter IP-adresbereiken](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


#### <a name="url-patterns-for-azure-portal"></a>URL-patronen voor Azure-portal

| URL-patroon | Onderdeel/functionaliteit | Apparaat-IP-adressen |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |StorSimple-apparaatbeheerservice<br>Access Control Service<br>Azure Service Bus<br>Verificatieservice |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.backup.windowsazure.com` |Apparaatregistratie |Alleen DATA 0 |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Het intrekken van certificaten |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-accounts en bewaking |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update-servers<br> |Vaste IP-adressen alleen voor de controllers |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Vaste IP-adressen alleen voor de controllers |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Ondersteuningspakket |Netwerkinterfaces die zijn ingeschakeld voor de cloud |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-patronen voor Azure Government-portal

| URL-patroon | Onderdeel/functionaliteit | Apparaat-IP-adressen |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login.microsoftonline.us` |StorSimple-apparaatbeheerservice<br>Access Control Service<br>Azure Service Bus<br>Verificatieservice |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.backup.windowsazure.us` |Apparaatregistratie |Alleen DATA 0 |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Het intrekken van certificaten |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-accounts en bewaking |Netwerkinterfaces die zijn ingeschakeld voor de cloud |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update-servers<br> |Vaste IP-adressen alleen voor de controllers |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Vaste IP-adressen alleen voor de controllers |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Ondersteuningspakket |Netwerkinterfaces die zijn ingeschakeld voor de cloud |

### <a name="routing-metric"></a>Routering metrische gegevens

Een routering meetwaarde is gekoppeld aan de interfaces en de gateway die de gegevens naar de opgegeven netwerken te routeren. Routering wordt gebruikt door het protocol voor het berekenen van het beste pad naar een bepaalde bestemming als er meerdere paden naar dezelfde bestemming bestaan leert. Des te lager de routering metrische gegevens, hoe hoger de voorkeur.

In de context van StorSimple, als er meerdere netwerkinterfaces en gateways zijn geconfigureerd voor kanaalverkeer van, komen de routering metrische gegevens aan de orde om te bepalen van de relatieve volgorde waarin de interfaces wordt ophalen gebruikt. De routering metrische gegevens kan niet worden gewijzigd door de gebruiker. U kunt echter de `Get-HcsRoutingTable` cmdlet om af te drukken de routering tabel (en metrische gegevens) op uw StorSimple-apparaat. Meer informatie over de cmdlet Get-HcsRoutingTable in [implementatie voor het oplossen van StorSimple](storsimple-troubleshoot-deployment.md).

De routering metrische algoritme dat wordt gebruikt voor Update 2 en latere versies kunt als volgt worden uitgelegd.

* Een reeks vooraf bepaalde waarden zijn toegewezen aan netwerkinterfaces.
* U kunt een van de voorbeeldtabel hieronder wordt weergegeven met de waarden die zijn toegewezen aan de verschillende netwerkinterfaces wanneer ze zijn ingeschakeld voor de cloud of cloud-uitgeschakeld, maar met een geconfigureerde gateway. Houd er rekening mee de waarden die hier worden toegewezen, zijn alleen voorbeelden van waarden.

    | Netwerkinterface | Cloud-enabled | Cloud-uitgeschakeld met gateway |
    |-----|---------------|---------------------------|
    | Data 0  | 1            | -                        |
    | Gegevens 1  | 2            | 20                       |
    | De Data 2  | 3            | 30                       |
    | Gegevens 3  | 4            | 40                       |
    | Data 4  | 5            | 50                       |
    | Gegevens 5  | 6            | 60                       |


* De volgorde waarin het cloudverkeer wordt gerouteerd via de netwerkinterfaces is:
  
    *Data 0 > gegevens 1 > datum 2 > gegevens 3 > gegevens 4 > Data 5*
  
    Dit kan worden verklaard door het volgende voorbeeld.
  
    Houd rekening met een StorSimple-apparaat met twee ingeschakeld voor de cloud netwerkinterfaces, Data 0 en 5 van de gegevens. Gegevens 1 tot en met 4 van de gegevens zijn cloud-uitgeschakeld, maar een geconfigureerde gateway hebben. De volgorde waarin verkeer wordt doorgestuurd voor dit apparaat zijn:
  
    *Data 0 (1) > Data 5 (6) > Data 1 (20) > Data 2 (30) > Data 3 (40) > Data 4 (50)*
  
    *De getallen tussen haakjes geven aan de respectieve routering metrische gegevens.*
  
    Als de Data 0 is mislukt, wordt verkeer van de t/m Data 5 ophalen gerouteerd. Gezien het feit dat een gateway is geconfigureerd op alle andere netwerk, als zowel de Data 0 als de gegevens 5 zijn mislukt, wordt verkeer van de Data 1 doorlopen.
* Als een ingeschakeld voor de cloud-netwerkinterface is mislukt, worden vervolgens 3 nieuwe pogingen met een vertraging van 30 tweede verbinding maken met de interface. Als de nieuwe pogingen mislukken, wordt het verkeer wordt doorgestuurd naar de volgende beschikbare cloud geschikte interface zoals wordt bepaald door de routeringstabel. Als alle de cloud geschikte netwerkinterfaces is mislukt, wordt klikt u vervolgens het apparaat failover naar de andere controller (opnieuw opstarten is niet in dit geval).
* Als er een fout VIP voor een netwerkinterface met iSCSI-zijn ingeschakeld, worden er 3 nieuwe pogingen met een vertraging van 2 seconden. Dit gedrag is dezelfde uit de vorige versies zijn gebleven. Als alle netwerkinterfaces van de iSCSI-zijn mislukt, wordt een failover van de controller (vergezeld van een opnieuw opstarten) optreden.
* Een waarschuwing wordt ook gegeven op uw StorSimple-apparaat wanneer er een VIP-fout. Ga voor meer informatie naar [Naslaggids waarschuwen](storsimple-8000-manage-alerts.md).
* In termen van nieuwe pogingen voorrang iSCSI krijgt boven cloud.
  
    Kijk een naar het volgende voorbeeld: Een StorSimple-apparaat twee netwerkinterfaces die zijn ingeschakeld heeft, Data 0 en 1 van de gegevens. Data 0 is ingeschakeld voor de cloud terwijl Data 1 zowel in de cloud is en iSCSI-zijn ingeschakeld. Er zijn geen andere netwerkinterfaces op dit apparaat zijn voor cloud- of iSCSI-ingeschakeld.
  
    Als gegevens 1 mislukt, krijgt deze de laatste iSCSI-netwerkinterface is, resulteert dit in een failover van de controller 1 van de gegevens op de andere controller.

### <a name="networking-best-practices"></a>Aanbevolen procedures voor netwerken

Naast de bovenstaande netwerkvereisten voor optimale prestaties van uw StorSimple-oplossing zich houden aan de volgende aanbevolen procedures:

* Zorg ervoor dat uw StorSimple-apparaat heeft een speciale 40 Mbps bandbreedte (of meer) beschikbaar te allen tijde. Deze bandbreedte niet mogen worden gedeeld (of toewijzing moet worden gegarandeerd door het gebruik van QoS-beleid) met andere toepassingen.
* Zorg ervoor dat de netwerkverbinding met het Internet te allen tijde beschikbaar is. Er kunnen sporadisch of onbetrouwbare Internet-verbindingen op de apparaten, met inbegrip van welke aard dan ook, zonder internetverbinding resulteert in een niet-ondersteunde configuratie.
* Het iSCSI- en cloud-verkeer te isoleren door speciale netwerkinterfaces op uw apparaat voor toegang tot iSCSI- en cloud. Zie voor meer informatie over het [netwerkinterfaces wijzigen](storsimple-8000-modify-device-config.md#modify-network-interfaces) op uw StorSimple-apparaat.
* Gebruik niet een LACP Link Aggregation Control Protocol ()-configuratie voor de netwerkinterfaces. Dit is een niet-ondersteunde configuratie.

## <a name="high-availability-requirements-for-storsimple"></a>Vereisten voor hoge beschikbaarheid voor StorSimple

De hardwareplatform die is opgenomen in de StorSimple-oplossing beschikt over beschikbaarheid en betrouwbaarheid van functies die voorzien in een basis voor een maximaal beschikbare, fouttolerante opslaginfrastructuur in uw datacenter. Er zijn echter vereisten en best practices die u voldoen moet om te zorgen voor de beschikbaarheid van uw StorSimple-oplossing. Voordat u StorSimple implementeren, zorgvuldig door de volgende vereisten en aanbevolen procedures voor de StorSimple-apparaat en de verbonden host-computers.

Voor meer informatie over het controleren en onderhouden van de hardware-onderdelen van uw StorSimple-apparaat, gaat u naar [gebruik van de service StorSimple Device Manager voor de hardware-onderdelen bewaken en de status](storsimple-8000-monitor-hardware-status.md) en [StorSimple Hardwarevervanging onderdeel](storsimple-8000-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Vereisten voor hoge beschikbaarheid en -procedures voor uw StorSimple-apparaat

Lees de volgende informatie zorgvuldig om ervoor te zorgen de hoge beschikbaarheid van uw StorSimple-apparaat.

#### <a name="pcms"></a>PCMs

StorSimple-apparaten zijn redundante, hot swappable voeding en koeling modules (PCMs). Elke PCM heeft onvoldoende capaciteit om te voorzien van de service voor het hele chassis. Voor hoge beschikbaarheid, moeten beide PCMs worden geïnstalleerd.

* Verbinding maken met uw PCMs met verschillende energiebronnen voor beschikbaarheid als een power-bron is mislukt.
* Als een PCM mislukt, vragen om een vervangende onmiddellijk.
* Verwijder PCM defect is alleen wanneer u de verwijzing wordt vervangen en klaar bent om te installeren.
* Beide PCMs niet gelijktijdig verwijderen. De PCM-module bevat de back-upbatterij-module. Afsluiten zonder accu beveiliging wanneer beide van de PCMs wordt verwijderd en de status van het apparaat wordt niet opgeslagen. Voor meer informatie over de batterij, gaat u naar [onderhouden van de module back-upbatterij](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Controller-modules

StorSimple-apparaten zijn redundante, hot swappable controller modules. De controller-modules werken in een actief/passief-wijze. Op elk moment een module van de domeincontroller actief is en biedt service, terwijl de andere controllermodule is een passieve. De passieve controller-module is ingeschakeld, en operationele wordt als de actieve controller-module is mislukt of is verwijderd. Elke controllermodule heeft onvoldoende capaciteit om te voorzien van de service voor het hele chassis. Beide modules domeincontroller moeten worden geïnstalleerd om hoge beschikbaarheid te garanderen.

* Zorg ervoor dat beide modules controller te allen tijde worden geïnstalleerd.
* Als een controllermodule is mislukt, vragen om een vervangende onmiddellijk.
* Een module controller defect is alleen verwijderd wanneer u de vervanging hebt en gereed is om deze te installeren. Verwijderen van een module voor langere is van invloed op de luchtstroom en wordt daarmee het koelen van het systeem.
* Zorg ervoor dat de netwerkverbindingen met beide modules controller identiek zijn en een identieke netwerkconfiguratie van de verbonden netwerkinterfaces hebben.
* Als een controllermodule is mislukt of als vervanging moet, zorg ervoor dat de andere controllermodule een actieve status is voordat de module controller defect is vervangen. Om te controleren of een domeincontroller actief is, gaat u naar [de actieve controller op uw apparaat identificeren](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Verwijder beide modules netwerkcontroller niet op hetzelfde moment. Als u een failover van de controller wordt uitgevoerd, niet de stand-by-controllermodule afgesloten of verwijderen uit het chassis.
* Wacht ten minste vijf minuten voordat het verwijderen van een controllermodule na een failover van de controller.

#### <a name="network-interfaces"></a>Netwerkinterfaces

StorSimple-apparaat controller modules elke hebt vier 1 Gigabit en twee 10 Gigabit Ethernet-netwerkinterfaces.

* Zorg ervoor dat de netwerkverbindingen met beide modules controller identiek zijn en dat de controller module interfaces zijn verbonden met een identieke netwerkconfiguratie netwerkinterfaces.
* Indien mogelijk, implementeert u Netwerkverbindingen in verschillende switches om te controleren of de beschikbaarheid van de service in het geval van een netwerkfout van het apparaat.
* Bij het ontkoppelen alleen of de laatste resterende ingeschakeld voor iSCSI-interface (met IP-adressen toegewezen), de interface eerst uitschakelen en vervolgens de kabels los te koppelen. Als de interface eerst losgekoppeld is, zal dit leiden tot de actieve controller om een failover uitvoeren naar de passieve controller. Als de passieve controller ook de bijbehorende interfaces losgekoppeld heeft, kunnen beide controllers wordt meerdere keren opnieuw voordat op een domeincontroller.
* Ten minste twee interfaces voor gegevens verbinden met het netwerk van elke domeincontroller-module.
* Als u de twee hebt ingeschakeld 10 GbE-interfaces implementeren voor verschillende switches.
* Gebruik zo mogelijk MPIO op servers om ervoor te zorgen dat de servers een koppeling-, netwerk- of interfacefout kunnen tolereren.

Voor meer informatie over het netwerk van uw apparaat voor hoge beschikbaarheid en prestaties, gaat u naar [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD's en HDD 's

StorSimple-apparaten zijn Solid-State-schijven (SSD's) en harde schijven (HDD's) die zijn beveiligd met behulp van gespiegelde opslagruimten. Gebruik van gespiegelde opslagruimten zorgt ervoor dat het apparaat kunnen tolereren het uitvallen van één of meer SSD of HDD's.

* Zorg ervoor dat alle SSD en HDD-modules zijn geïnstalleerd.
* Als een SSD of HDD mislukt, vragen om een vervangende onmiddellijk.
* Als een SSD of HDD mislukt of moet worden vervangen, zorg er dan voor dat u verwijdert alleen de SSD of HDD die moet worden vervangen.
* Verwijder niet meer dan één SSD of HDD uit het systeem op elk gewenst moment in-time.
  Een storing van 2 of meer schijven van een bepaald type (HDD, SSD) of opeenvolgende mislukte binnen een korte periode kan leiden tot systeem storing en potentieel gegevensverlies. Als dit gebeurt, [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor hulp.
* Tijdens de vervanging bewaken de **gedeelde onderdelen** in de **hardwarestatus** blade voor de stations in de SSD's en HDD's. De status van een groen vinkje geeft aan dat de schijven zijn in orde of OK, terwijl een rood uitroepteken wijst geeft aan dat een mislukte SSD of HDD.
* Het is raadzaam dat u configureert cloudmomentopnamen voor alle volumes die u nodig hebt om te beveiligen in geval van een systeemfout is opgetreden.

#### <a name="ebod-enclosure"></a>EBOD behuizing

StorSimple-Apparaatmodel 8600 bevat een behuizing uitgebreid Bunch van schijven (EBOD) naast de primaire behuizing. Een EBOD bevat EBOD-controllers en harde schijven (HDD's) die zijn beveiligd met behulp van gespiegelde opslagruimten. Gebruik van gespiegelde opslagruimten zorgt ervoor dat het apparaat kunnen tolereren het uitvallen van een of meer HDD's. De EBOD-behuizing is verbonden met de primaire behuizing via redundante SAS-kabels.

* Zorg ervoor dat beide modules EBOD behuizing-controller, zowel SAS-kabels en alle de harde schijven te allen tijde zijn geïnstalleerd.
* Als een EBOD behuizing-controller-module is mislukt, vragen om een vervangende onmiddellijk.
* Als een EBOD behuizing-controller-module is mislukt, zorg ervoor dat de andere module van de domeincontroller actief is voordat u de mislukte module vervangt. Om te controleren of een domeincontroller actief is, gaat u naar [de actieve controller op uw apparaat identificeren](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Tijdens een vervanging van EBOD-controller-module, continu de status controleren van het onderdeel in de StorSimple Device Manager-service door het openen van **Monitor** > **hardwarestatus**.
* Als een SAS-kabel mislukt of moet worden vervangen (Microsoft Support moet worden uitgevoerd om te maken van dergelijke vaststelling), zorg er dan voor dat u de SAS-kabel die moet worden vervangen.
* Niet gelijktijdig verwijderen beide SAS-kabels uit het systeem op elk gewenst moment in-time.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Aanbevelingen voor hoge beschikbaarheid voor uw hostcomputers

Zorgvuldig door deze aanbevolen procedures voor de hoge beschikbaarheid van de hosts die zijn verbonden met uw StorSimple-apparaat.

* StorSimple met configureren [twee knooppunten cluster bestandsserverconfiguraties][1]. Door het verwijderen van single point of fout en te bouwen in redundantie aan de host, wordt de gehele oplossing maximaal beschikbaar.
* Continu beschikbare bestandsshares voor (CA) beschikbaar met Windows Server 2012 (SMB 3.0) gebruiken voor hoge beschikbaarheid tijdens failover van de opslagcontrollers. Voor aanvullende informatie voor het configureren van bestandsserverclusters en continu beschikbare bestandsshares met Windows Server 2012 verwijzen naar dit [videodemo](https://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over limieten voor StorSimple-systeem](storsimple-8000-limits.md).
* [Informatie over het implementeren van uw StorSimple-oplossing](storsimple-8000-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
