---
title: De StorSimple-apparaat-configuratie wijzigen | Microsoft Docs
description: "Hierin wordt beschreven hoe u de StorSimple Manager-service opnieuw configureren van een virtueel StorSimple-apparaat dat al is geïmplementeerd."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: bb679264-8d46-429c-9ef7-630dc3b4a423
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: v-sharos
ms.openlocfilehash: 5bfbeb038dbedae2bf77016abbc19458c3dc22c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>De StorSimple Manager-service gebruiken voor het wijzigen van de configuratie van uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
De klassieke Azure portal **configureren** pagina bevat alle apparaatparameters die u kunt opnieuw configureren op een StorSimple-apparaat dat wordt beheerd door een StorSimple Manager-service. Deze zelfstudie wordt uitgelegd hoe u kunt de **configureren** pagina de volgende op apparaatniveau taken uitvoeren:

* Apparaatinstellingen wijzigen 
* Tijdinstellingen wijzigen 
* DNS-instellingen wijzigen 
* Netwerkinterfaces wijzigen
* Wisselen of opnieuw toewijzen van IP-adressen

## <a name="modify-device-settings"></a>Apparaatinstellingen wijzigen
De instellingen voor apparaten bevatten de beschrijvende naam van het apparaat en de beschrijving van het apparaat.

> [!NOTE] 
> U kunt de naam van het apparaat in de klassieke Azure portal niet wijzigen. Naam van het apparaat wordt niet ondersteund.

Een StorSimple-apparaat dat is verbonden met de StorSimple Manager-service wordt een standaardnaam toegewezen. De standaardnaam geeft doorgaans het serienummer van het apparaat. Een standaardnaam voor het apparaat dat is 15 tekens lang zijn, zoals 8600-SHX0991003G44HT geeft bijvoorbeeld het volgende:

* **8600** – geeft aan het model.
* **SHX** – geeft aan van de productie-site.
* **0991003** -geeft aan een specifiek product.
* **G44HT**-de laatste 5 cijfers voor het maken van unieke serienummers worden verhoogd. Dit wordt mogelijk niet een sequentiële set.

U kunt een beschrijving van het apparaat opgeven. Een beschrijving van het apparaat wordt meestal helpt de eigenaar en de fysieke locatie van het apparaat te identificeren. Het beschrijvingsveld moet minder dan 256 tekens bevatten.

## <a name="modify-time-settings"></a>Tijdinstellingen wijzigen
Uw apparaat, wordt de tijd moet synchroniseren om te verifiëren met uw serviceprovider voor cloud-opslag. Selecteer de tijdzone in de vervolgkeuzelijst en maximaal twee Network Time Protocol (NTP)-servers opgeven. De primaire NTP-server is vereist en wordt opgegeven wanneer u Windows PowerShell voor StorSimple gebruiken om uw apparaat te configureren. U kunt de standaard Windows-Server opgeven **time.windows.com** als uw NTP-server. U kunt de primaire NTP-serverconfiguratie via de klassieke Azure portal bekijken, maar u moet de Windows PowerShell-interface gebruiken om dit te wijzigen.

De configuratie van de secundaire NTP-server is optioneel. U kunt de klassieke portal gebruiken om een secundaire NTP-server te configureren. 

Wanneer u de NTP-server configureert, moet u ervoor zorgen dat uw netwerk NTP-verkeer door te geven van uw datacenter met het Internet toestaat. Wanneer u een openbare NTP-server opgeeft, moet u ervoor zorgen dat uw netwerkfirewalls en andere beveiligingsapparaten zijn geconfigureerd om NTP-verkeer worden verzonden naar en van het externe netwerk. Als bidirectionele NTP-verkeer niet wordt toegestaan, moet u een interne NTP-server (een Windows-domeincontroller biedt deze functie). Als uw apparaat kan geen tijd synchroniseren, is deze mogelijk niet communiceren met de provider van uw cloud-opslag.

Een lijst met openbare NTP-servers, Ga naar de [NTP-Servers Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Wat gebeurt er als het apparaat is geïmplementeerd in een andere tijdzone?
Als het apparaat is geïmplementeerd in een andere tijdzone, wijzigt u de tijdzone van het apparaat. Gezien het feit dat het back-upbeleid de tijdzone van het apparaat gebruiken, wordt het back-upbeleid automatisch aanpassen in overeenstemming met de nieuwe tijdzone. Er is geen tussenkomst van de gebruiker is vereist.

## <a name="modify-dns-settings"></a>DNS-instellingen wijzigen
Een DNS-server wordt gebruikt wanneer uw apparaat probeert te communiceren met uw serviceprovider voor cloud-opslag. Voor hoge beschikbaarheid moet u zowel de primaire als de secundaire DNS-servers configureren tijdens de initiële implementatie. Als u wilt configureren op de primaire DNS-server, moet u de Windows PowerShell-interface op uw StorSimple-apparaat gebruiken.

U kunt de klassieke Azure portal gebruiken voor het wijzigen van de secundaire DNS-server.

## <a name="modify-network-interfaces"></a>Netwerkinterfaces wijzigen
Uw apparaat heeft zes apparaat netwerkinterfaces, waarvan vier 1 GbE zijn en dat de twee 10 GbE zijn. Deze interfaces zijn gelabeld als DATA 0: DATA 5. DATA 0, gegevens-1, gegevens 4 en 5 van de gegevens zijn 1 GbE DATA 2 en DATA 3 10 GbE-netwerkinterfaces zijn.

Configureer **netwerkinterface-instellingen** voor elk van de interfaces die moet worden gebruikt. Hoge beschikbaarheid, zodat we raden u aan ten minste twee interfaces voor iSCSI- en twee interfaces zijn ingeschakeld voor de cloud op uw apparaat. We raden aan maar hoeven niet dat ongebruikte interfaces worden uitgeschakeld.

Wanneer u een van de netwerkinterfaces configureert, moet u een virtueel IP-adres (VIP) configureren.

DATA 0 is standaard ingeschakeld cloud. Bij het configureren van DATA 0, moet u ook twee vaste IP-adressen, één voor elke domeincontroller configureren. Deze vaste IP-adressen kunnen worden gebruikt voor het rechtstreeks toegang hebben tot de apparaatcontrollers en zijn handig wanneer u updates installeert op het apparaat, of wanneer u toegang de domeincontrollers voor het oplossen van problemen tot.

In StorSimple 8000 Series Update 1, de route-metric van DATA 0 is ingesteld op de laagste; dus als uw apparaat wordt uitgevoerd StorSimple 8000 Series Update 1, wordt alle cloud-verkeer gerouteerd via DATA 0. Noteer dit als u meer dan één ingeschakeld voor de cloud netwerkinterface op uw StorSimple-apparaat hebt.

> [!NOTE]
> De vaste IP-adressen voor de controller worden gebruikt voor het uitvoeren van de updates op het apparaat. Daarom moeten de vaste IP-adressen routeerbaar zijn en verbinding kunnen maken met internet.
> 
> 

Voor elke netwerkinterface, worden de volgende parameters weergegeven:

* **Snelheid** – geen gebruiker configureerbare parameter. DATA 0, gegevens-1, gegevens 4 en 5 van de gegevens zijn altijd 1 GbE terwijl DATA 2 en DATA 3 10 GbE-interfaces zijn.
  
  > [!NOTE]
  > Snelheid en duplex wordt altijd automatisch-onderhandeld. Jumboframes worden niet ondersteund.
  > 
  > 
* **Interface-status** : een interface kan worden ingeschakeld of uitgeschakeld. Indien ingeschakeld, wordt het apparaat probeert te gebruiken van de interface. We raden aan dat alleen de interfaces die zijn verbonden met het netwerk en gebruikt worden ingeschakeld. Schakel alle interfaces die u niet gebruikt.
* **Type interface** – met deze parameter kunt u iSCSI-verkeer van cloud-opslagverkeer isoleren. Deze parameter zijn:
  
  * **Cloud ingeschakeld** – wanneer ingeschakeld, het apparaat deze interface gebruikt om te communiceren met de cloud.
  * **iSCSI ingeschakeld** – wanneer ingeschakeld, het apparaat deze interface wordt gebruikt voor communicatie met de iSCSI-host.
    
    Het is raadzaam dat u iSCSI-verkeer van cloud-opslagverkeer isoleren. Let ook op dat als de host zich binnen hetzelfde subnet als het apparaat bevindt, hoeft u niet toewijzen van een gateway; echter, als de host zich in een ander subnet dan uw apparaat, u moet een gateway toewijzen.
* **IP-adres** – dit IPv4 of IPv6- of beide kan zijn. De IPv4- als IPv6-adresfamilies worden ondersteund voor de netwerkinterfaces van het apparaat. Wanneer IPv4 wordt gebruikt, geeft u een 32-bits IP-adres (*xxx.xxx.xxx.xxx*) in punt decimale notatie met punten. Wanneer u IPv6 gebruikt, zijn simpelweg een 4-cijferige voorvoegsel en een 128-bits-adres wordt automatisch gegenereerd voor de netwerkinterface van uw apparaat op basis van het voorvoegsel.
* **Subnet** – dit verwijst naar het subnetmasker en via de Windows PowerShell-interface is geconfigureerd.
* **Gateway** – dit is de standaard-gateway die door deze interface moet worden gebruikt wanneer er wordt geprobeerd om te communiceren met knooppunten die zich niet binnen dezelfde IP-adresruimte (subnet). De standaard-gateway moet in dezelfde adresruimte (subnet) als de interface IP-adres, zoals wordt bepaald door het subnetmasker.
* **Vaste IP-adres** – dit veld is alleen beschikbaar als u de DATA 0 configureren interface. Voor bewerkingen zoals updates of het oplossen van het apparaat, moet u wellicht rechtstreeks verbinding maken met de apparaat-controller. Het vaste IP-adres kan worden gebruikt voor toegang tot zowel de actieve en passieve-controller op uw apparaat.

U kunt opnieuw Controller 0 en Controller 1 configureren via de klassieke Azure portal.

> [!NOTE]
> * Controleer of de interfacesnelheid en de duplex op de switch die de interface van elk apparaat is verbonden met zodat de juiste werking. Switch netwerkinterfaces moet ofwel worden onderhandeld met of worden geconfigureerd voor Gigabit Ethernet (1000 Mbps) en full-duplex. Interfaces besturingssysteem trager of half-duplex leidt tot prestatieproblemen.
> * Als u wilt onderbrekingen en downtime minimaliseren, wordt u aangeraden portfast op elk van de switchpoorten die door de iSCSI-netwerkinterface van uw apparaat verbinding met maken in te schakelen. Dit zorgt ervoor dat verbinding met het netwerk kan snel worden opgezet in het geval van een failover.
> 
> 

## <a name="swap-or-reassign-ips"></a>Wisselen of opnieuw toewijzen van IP-adressen
Op dit moment wordt als een netwerkinterface op de domeincontroller een VIP-adres dat wordt gebruikt (door hetzelfde apparaat of een ander apparaat in het netwerk) toegewezen is, is klikt u vervolgens de controller failover. U moet daarom, volgt u de juiste procedure als u bij het wisselen van VIP's voor de netwerkinterface van het apparaat, omdat u een situatie met een dubbele IP maakt.

De volgende stappen uitvoeren om te wisselen of opnieuw toewijzen van de VIP's voor een van de netwerkinterfaces:

#### <a name="to-reassign-ips"></a>Opnieuw toewijzen van IP-adressen
1. Hiermee schakelt u het IP-adres voor beide interfaces.
2. Nadat de IP-adressen zijn uitgeschakeld, moet u het nieuwe IP-adressen toewijzen aan de betreffende interfaces.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [MPIO configureren voor uw StorSimple-apparaat](storsimple-configure-mpio-windows-server.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

