---
title: De StorSimple 8000-serie-apparaatconfiguratie wijzigen | Microsoft Docs
description: Beschrijft hoe u de service StorSimple Device Manager gebruiken voor het configureren van een StorSimple-apparaat dat al is geïmplementeerd.
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
ms.openlocfilehash: 774f5a73a5fc30352698c0af0c279fbbe488c480
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58008373"
---
# <a name="use-the-storsimple-device-manager-service-to-modify-your-storsimple-device-configuration"></a>De StorSimple Device Manager-service gebruiken voor het wijzigen van de configuratie van uw StorSimple-apparaat

## <a name="overview"></a>Overzicht

De Azure-portal **apparaatinstellingen** sectie de **instellingen** blade bevat alle apparaatparameters die u kunt opnieuw configureren op een StorSimple-apparaat dat wordt beheerd door een StorSimple Device Manager-service . Deze zelfstudie wordt uitgelegd hoe u kunt de **instellingen** blade aan de volgende op apparaatniveau taken uitvoeren:

* Beschrijvende naam van apparaat wijzigen
* Apparaatinstellingen tijd wijzigen
* Toewijzen van een secundaire DNS-server
* Netwerkinterfaces wijzigen
* Wisselen of opnieuw toewijzen van IP-adressen

## <a name="modify-device-friendly-name"></a>Beschrijvende naam van apparaat wijzigen

De Azure-portal kunt u wijzigen van de naam van het apparaat en hieraan een unieke beschrijvende naam van uw keuze. Gebruik de **algemene instellingen** blade op uw apparaat te wijzigen van de beschrijvende naam van het apparaat. De beschrijvende naam mag geen tekens en mag maximaal 64 tekens lang zijn.

> [!NOTE] 
> U kunt de naam van het apparaat in Azure portal alleen wijzigen voordat de installatie van het apparaat is voltooid. Als de minimale Apparaatinstelling voltooid is, kunt u de naam van het apparaat niet wijzigen.

![Apparaatnaam in algemene instellingen](./media/storsimple-8000-modify-device-config/modify-general-settings3.png)

Een StorSimple-apparaat dat is verbonden met de StorSimple Device Manager-service wordt een standaardnaam toegewezen. De standaardnaam geeft doorgaans het serienummer van het apparaat. Apparaat met een standaardnaam die is 15 tekens lang zijn, zoals 8600-SHX0991003G44HT, geeft bijvoorbeeld het volgende:

* **8600** – geeft aan dat het Apparaatmodel.
* **VX** – geeft aan dat de productie-site.
* **0991003** -geeft aan dat een specifiek product.
* **G44HT**-de laatste 5 cijfers voor het maken van de unieke serienummers worden verhoogd. Dit kan een opeenvolgende reeks zijn.

## <a name="modify-device-description"></a>Beschrijving van het apparaat wijzigen

Gebruik de **algemene instellingen** blade op uw apparaat te wijzigen van de beschrijving van het apparaat.

![Beschrijving van het apparaat in de algemene instellingen](./media/storsimple-8000-modify-device-config/modify-general-settings4.png)

Een beschrijving van het apparaat kan de eigenaar en de fysieke locatie van het apparaat te identificeren. Het beschrijvingsveld moet minder dan 256 tekens bevatten.

## <a name="modify-time-settings"></a>Tijdinstellingen wijzigen

Tijd moet om te kunnen verifiëren met uw serviceprovider voor cloud-opslag synchroniseren voor uw apparaat. Gebruik de **algemene instellingen** blade op uw apparaat te wijzigen van de tijd apparaatinstellingen.

![Beschrijving van het apparaat in de algemene instellingen](./media/storsimple-8000-modify-device-config/modify-general-settings2.png)

 Selecteer uw tijdzone in de vervolgkeuzelijst. U kunt maximaal twee Network Time Protocol (NTP)-servers opgeven:

 - **Primaire NTP-server** -de configuratie is vereist en wordt opgegeven wanneer u Windows PowerShell voor StorSimple gebruiken om uw apparaat te configureren. U kunt de standaard Windows-Server opgeven **time.windows.com** als uw NTP-server. U kunt de configuratie van de primaire NTP server via de Azure-portal weergeven, maar moet u de Windows PowerShell-interface gebruiken om deze te wijzigen. Gebruik de `Set-HcsNTPClientServerAddress` cmdlet om te wijzigen van de primaire NTP-server van uw apparaat. Ga voor meer informatie naar de syntaxis voor [Set HcsNTPClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet.

- **Secundaire NTP-server** -de configuratie is optioneel. De portal kunt u een secundaire NTP-server configureren.

Bij het configureren van de NTP-server, zorg ervoor dat uw netwerk de NTP-verkeer kan worden doorgegeven van uw datacenter met het Internet toestaat. Wanneer u een openbare NTP-server opgeeft, moet u ervoor zorgen dat uw netwerkfirewalls en andere beveiligingsapparaten zijn geconfigureerd om toe te staan van NTP verkeer naar en van het externe netwerk. Als in twee richtingen NTP verkeer is niet toegestaan, moet u een interne NTP-server (een Windows-domeincontroller biedt deze functie). Als uw apparaat kan geen tijd synchroniseren, is het mogelijk niet kan communiceren met de cloudopslagprovider.

Een lijst van openbare NTP-servers wilt bekijken, gaat u naar de [NTP-Servers Web](https://support.ntp.org/bin/view/Servers/WebHome).

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Wat gebeurt er als het apparaat is geïmplementeerd in een andere tijdzone?

Als het apparaat is geïmplementeerd in een andere tijdzone, wijzigt u de tijdzone van het apparaat. Gezien het feit dat het back-upbeleid de tijdzone van het apparaat gebruiken, wordt de back-upbeleid wordt automatisch aangepast in overeenstemming met de nieuwe tijdzone. Er is geen tussenkomst van de gebruiker is vereist.

## <a name="modify-dns-settings"></a>DNS-instellingen wijzigen

Een DNS-server wordt gebruikt wanneer uw apparaat probeert te communiceren met uw serviceprovider voor cloud-opslag. Gebruik de **netwerkinstellingen** blade op uw apparaat weergeven en wijzigen van de geconfigureerde DNS-instellingen. 

![DNS-instellingen in netwerkinstellingen](./media/storsimple-8000-modify-device-config/modify-network-settings1.png)

Voor hoge beschikbaarheid moet u zowel de primaire en secundaire DNS-servers configureren tijdens de initiële implementatie.

**Primaire DNS-server** -u de Windows PowerShell voor StorSimple gebruiken om eerst de primaire DNS-server tijdens de eerste installatie. U kunt de primaire DNS-server alleen via de Windows PowerShell-interface opnieuw configureren. Gebruik de `Set-HcsDNSClientServerAddress` cmdlet om te wijzigen van de primaire DNS-server van uw apparaat. Ga voor meer informatie naar de syntaxis voor [Set HcsDNSClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet.

**Secundaire DNS-server** : als u wilt wijzigen van de secundaire DNS-server, gebruikt u de `Set-HcsDNSClientServerAddress` cmdlet in de Windows PowerShell-interface van het apparaat of **netwerkinstellingen** blade van uw StorSimple-apparaat in Azure portal.

Als u wilt wijzigen van de secundaire DNS-server in Azure portal, moet u de volgende stappen uitvoeren.

1. Ga naar uw StorSimple-apparaatbeheerservice. Selecteer in de lijst met apparaten en klikt u op uw apparaat.

2. In de **instellingen** blade, Ga naar **apparaatinstellingen > netwerk**. Hiermee opent u de **netwerkinstellingen** blade. Klik op **DNS-instellingen** tegel. Wijzigen van de secundaire IP-adres van de DNS-server.

    ![Secundaire DNS-server IP-adres wijzigen](./media/storsimple-8000-modify-device-config/modify-secondary-dns1.png)

4. Klik in de opdrachtbalk op **opslaan** wanneer u hierom wordt gevraagd om bevestiging, klikt u op **OK**.

    ![Opslaan en wijzigingen bevestigen](./media/storsimple-8000-modify-device-config/modify-secondary-dns-2.png)



## <a name="modify-network-interfaces"></a>Netwerkinterfaces wijzigen

Uw apparaat heeft zes apparaat netwerkinterfaces, waarvan vier 1 GbE zijn en twee 10 GbE zijn. Deze interfaces zijn gelabeld als DATA 0-5 van de gegevens. DATA 0, 1 van de gegevens, gegevens 4 en 5 van de gegevens zijn 1 GbE zijn 10 GbE-netwerkinterfaces DATA 2 en DATA 3.

Gebruik de **netwerkinstellingen** blade het configureren van elk van de interfaces op die moet worden gebruikt.

![Netwerkinterfaces via netwerkinstellingen configureren](./media/storsimple-8000-modify-device-config/modify-network-settings3.png)

Voor hoge beschikbaarheid, wordt u aangeraden dat u ten minste twee interfaces voor iSCSI- en twee interfaces voor cloud-functionaliteit op uw apparaat hebt. We raden aan maar hoeven niet dat niet-gebruikte interfaces worden uitgeschakeld.

Voor elke netwerkinterface, worden de volgende parameters weergegeven:

* **Snelheid** – geen gebruiker configureerbare parameter. DATA 0, 1 van de gegevens, gegevens 4 en 5 van de gegevens zijn altijd 1 GbE dat DATA 2 en DATA 3 10 GbE-netwerkinterfaces.
  
  > [!NOTE]
  > Snelheid en duplex worden altijd automatisch-onderhandeld. Jumboframes worden niet ondersteund.
  
* **Interface-status** – een interface kan worden ingeschakeld of uitgeschakeld. Indien ingeschakeld, wordt het apparaat probeert het gebruik van de interface. Het is raadzaam dat alleen de interfaces die zijn verbonden met het netwerk en gebruikt wordt ingeschakeld. Schakel alle interfaces die u niet gebruikt.
* **Type interface** : deze parameter kunt u voor het isoleren van iSCSI-verkeer van cloud-opslagverkeer. Deze parameter kan een van de volgende zijn:
  
  * **Cloud ingeschakeld** – wanneer ingeschakeld, het apparaat deze interface gebruikt om te communiceren met de cloud.
  * **iSCSI ingeschakeld** – wanneer ingeschakeld, het apparaat deze interface gebruikt om te communiceren met de iSCSI-host.
    
    Het is raadzaam dat u iSCSI-verkeer van cloud storage verkeer isoleren. Let ook op dat als de host zich in hetzelfde subnet bevindt als uw apparaat, hoeft u niet om toe te wijzen een gateway. echter, als de host zich in een ander subnet dan uw apparaat, u moet toewijzen van een gateway.
* **IP-adres** : bij het configureren van een van de netwerkinterfaces, moet u een virtueel IP (VIP) configureren. Dit kan zijn IPv4 of IPv6- of beide. Zowel de IPv4 en IPv6-adres-families worden ondersteund voor de netwerkinterfaces van het apparaat. Wanneer u IPv4 gebruikt, geeft u de IP-adres van een 32-bits (*xxx.xxx.xxx.xxx*) in een decimale-puntindeling. Wanneer u IPv6 gebruikt, simpelweg een 4-cijferige voorvoegsel en een 128-bits-adres wordt automatisch gegenereerd voor de netwerkinterface van uw apparaat op basis van dit voorvoegsel.
* **Subnet** – dit verwijst naar het subnetmasker en via de Windows PowerShell-interface is geconfigureerd.
* **Gateway** : dit is de standaardgateway die door deze interface moet worden gebruikt wanneer wordt geprobeerd om te communiceren met knooppunten die zich niet binnen de dezelfde IP-adresruimte (subnet). De standaard-gateway moet zich in dezelfde adresruimte (subnet) als de interface IP-adres, zoals wordt bepaald door het subnetmasker.
* **Vaste IP-adres** : dit veld is alleen beschikbaar als u de DATA 0 configureren interface. Voor bewerkingen zoals updates of het oplossen van het apparaat, moet u mogelijk rechtstreeks verbinding maken met de apparaatcontroller. Het vaste IP-adres kan worden gebruikt voor toegang tot zowel de actieve als de passieve controller op uw apparaat.

> [!NOTE]
> * Om ervoor te zorgen goede werking, Controleer of de interfacesnelheid en de duplex op de switch die elke apparaatinterface is verbonden met. Interfaces van de switch moet ofwel worden onderhandeld met of worden geconfigureerd voor Gigabit Ethernet (1000 Mbps) en full-duplex. Interfaces operationele trager of half-duplex leidt tot prestatieproblemen.
> * Om te beperken onderbrekingen en uitvaltijd, wordt u aangeraden dat u portfast op elk van de switchpoorten die door de iSCSI-netwerkinterface van uw apparaat verbinden inschakelt. Dit zorgt ervoor dat de netwerkverbinding snel kan worden gemaakt in het geval van een failover.

### <a name="configure-data-0"></a>DATA 0 configureren

DATA 0 is standaard ingeschakeld cloud. Bij het configureren van DATA 0, moet u ook twee vaste IP-adressen, één voor elke domeincontroller configureren. Deze vaste IP-adressen kunnen worden gebruikt om rechtstreeks toegang hebben tot de apparaatcontrollers en zijn nuttig wanneer u updates installeert op het apparaat voor garbagecollection goed te laten werken of als u toegang hebben tot de domeincontrollers voor het oplossen van.

U kunt de vaste IP-controllers via de blade DATA 0 instellingen opnieuw configureren.

![Netwerkinterface - DATA 0 configureren](./media/storsimple-8000-modify-device-config/modify-network-settings2.png)

> [!NOTE]
> De vaste IP-adressen voor de controller worden gebruikt voor het uitvoeren van de updates op het apparaat en ruimte vrijmaken algoritmen (garbagecollection) goed te laten werken. Daarom moeten de vaste IP-adressen routeerbaar zijn en verbinding kunnen maken met internet.

### <a name="configure-data-1---data-5"></a>GEGEVENS-1 - 5-gegevens configureren

Voor gegevens-1 - 5-netwerkinterfaces van gegevens, kunt u de netwerkinstellingen configureren zoals wordt weergegeven in de volgende schermafbeelding:

![Netwerkinterfaces DATA 1 - 5 gegevens configureren](./media/storsimple-8000-modify-device-config/modify-network-settings4.png)


## <a name="swap-or-reassign-ips"></a>Wisselen of opnieuw toewijzen van IP-adressen

Op dit moment als een netwerkinterface op de controller een VIP-adres dat wordt gebruikt (door hetzelfde apparaat of een ander apparaat in het netwerk) toegewezen wordt, wordt klikt u vervolgens de controller failover. Als u wisselen of opnieuw toewijzen van VIP's voor de netwerkinterface van een apparaat, moet u een juiste procedure volgen als u een dubbel IP-situatie kunt maken.

Voer de volgende stappen uit om te wisselen of opnieuw toewijzen van de VIP's voor het gebruik van de netwerkinterfaces:

#### <a name="to-reassign-ips"></a>Opnieuw toewijzen van IP-adressen

1. Hiermee schakelt u het IP-adres voor beide interfaces.
2. Nadat de IP-adressen zijn uitgeschakeld, moet u het nieuwe IP-adressen toewijzen aan de betreffende interfaces.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [MPIO configureren voor uw StorSimple-apparaat](storsimple-8000-configure-mpio-windows-server.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

