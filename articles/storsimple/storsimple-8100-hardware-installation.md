---
title: Microsoft Azure StorSimple 8100-apparaat installeren | Microsoft Docs
description: Beschrijft hoe u uitpakken, rek monteren en bekabelen van uw StorSimple 8100-apparaat voordat u implementeert en de software configureert.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 5fbc407a9792d033037fdaa2b14f4055d94c15ab
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38701939"
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Unpack, rack koppelen, en uw StorSimple 8100-apparaat bekabelen
## <a name="overview"></a>Overzicht
Uw Microsoft Azure StorSimple 8100 is een één behuizing rek apparaat. In deze zelfstudie wordt uitgelegd hoe u uitpakken, rek monteren en kabel de StorSimple 8100-apparaathardware voordat u configureren en implementeren van het StorSimple-apparaat.

## <a name="unpack-your-storsimple-8100-device"></a>Uw StorSimple 8100-apparaat uitpakken
De volgende stappen bevatten duidelijke, gedetailleerde instructies over het uitpakken van uw StorSimple 8100-opslagapparaat. Dit apparaat wordt verzonden in een vak.

### <a name="prepare-to-unpack-your-device"></a>Voorbereidingen voor het uitpakken van het apparaat
Voordat u uw apparaat uitpakken, controleert u de volgende informatie.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

1. Zorg ervoor dat u twee personen beschikbaar voor het beheren van het gewicht van de behuizing hebt als u deze handmatig worden verwerkt. Een volledig geconfigureerde behuizing wegen maximaal 32 kg (70 lbs.).
2. Plaats het vak op een vast, niveau voor aanvallen.

Vervolgens de volgende stappen uit te pakken van uw apparaat.

#### <a name="to-unpack-your-device"></a>Uw apparaat uitpakken
1. Controleer het vak en het schuim verpakking voor crushes, delen, water schade of andere voor de hand liggende schade. Als het selectievakje of de verpakking ernstig is beschadigd, opent u het selectievakje niet. Neem [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om te beoordelen of het apparaat zich in goede staat.
2. Pak het vak. De volgende afbeelding toont de uitgepakte weergave van uw StorSimple-apparaat.
   
     ![Uw apparaat uitpakken](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Uitgepakte weergave van uw opslagapparaat**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Verpakking-vak |
   |   2 |Onder schuim |
   |   3 |Apparaat |
   |   4 |Bovenste schuim |
   |   5 |Accessoires vak |
3. Na het uitpakken van het vak, zorg ervoor dat u hebt:
   
   * 1 apparaat met één behuizing
   * 2 stroomkabels
   * 1 crossover Ethernet-kabel
   * 2 seriële console-kabels
   * 1 serie USB-conversieprogramma voor seriële toegang
   * 1 fraudebestendig T10 draai
   * 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
   * 1 met het rek monteren kit (2 kant rails met koppelen van hardware)
   * Aan de slag-documentatie
     
     Als u een van de items worden weergegeven, niet hebt ontvangen [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

De volgende stap is het rack koppelen uw apparaat.

## <a name="rack-mount-your-storsimple-8100-device"></a>Uw StorSimple 8100-apparaat rack koppelen
De volgende stappen voor het installeren van uw StorSimple 8100-apparaat voor gegevensopslag in een standard 19-inch rek met front-end- en achterkant berichten. Het StorSimple 8100-apparaat heeft één primaire behuizing.

De installatie bestaat uit meerdere stappen, die wordt besproken in de volgende procedures.

> [!IMPORTANT]
> StorSimple-apparaten moet rek gemonteerd voor een goede werking.
> 
> 

### <a name="prepare-the-site"></a>Voorbereiden van de site
Het apparaat moet worden geïnstalleerd op een standaard 19-inch rek met zowel de front-end en de achterzijde berichten. Gebruik de volgende procedure om voor te bereiden voor rackinstallatie.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Voorbereiden van de site voor de rackinstallatie van een
1. Zorg ervoor dat het apparaat veilig is gebaseerd op een vast, stabiel en niveau werk surface (of vergelijkbaar).
2. Controleer of de site waar u van plan bent om in te stellen standard netstroom van een onafhankelijke bron of een rack power distribution Units (PDU) met een noodvoeding (UPS).
3. Zorg ervoor dat één 2U de sleuf is beschikbaar op het rek waarin u van plan bent om te koppelen van het apparaat.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

Zorg ervoor dat u twee personen beschikbaar voor het beheren van het gewicht hebt als u omgaan met de installatie van het apparaat handmatig. Een volledig geconfigureerde behuizing wegen maximaal 32 kg (70 lbs.).

### <a name="rack-prerequisites"></a>Rack-vereisten
De 8100-behuizing is ontworpen voor de installatie in een standard 19-inch rack cab met:

* Minimale diepte van 27.84 inches van rek te boeken.
* Maximale gewicht van 32 kg voor het apparaat
* Maximale zwaar wordt belast van 5 Pascal (0,5 mm water meter).

### <a name="rack-mounting-rail-kit"></a>Rack koppelen spoor kit
Een set rails koppelen is opgegeven voor gebruik met het 19-inch rack CAB-bestand. De rails getest voor het afhandelen van het gewicht maximale behuizing. Deze rails kunnen ook installatie van meerdere insluitingen hebben zonder verlies van ruimte in het rek te plaatsen.

#### <a name="to-install-the-device-on-the-rails"></a>Voor het installeren van het apparaat op de rails
1. Voer deze stap alleen als interne rails zijn niet geïnstalleerd op uw apparaat. Normaal gesproken zijn de binnenste rails geïnstalleerd in de fabriek. Als de rails niet zijn geïnstalleerd, installeert u de dia's links-spoor en rechts spoor zijden van de behuizing enclosure. Ze koppelen met behulp van zes metrische schroeven aan elke zijde. Om te helpen bij een stand, spoor dia's zijn gemarkeerd **LH – Front** en **RH – Front**, en het einde die vanaf de achterzijde van de behuizing is aangebracht heeft een Tapse end.<br/>
   
    ![Spoor dia's koppelen aan behuizing-chassis](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Binnenste spoor dia's koppelen aan de zijde van de behuizing**
   
    Label | Beschrijving
    ----- | -----------
    1     | M 3 x 4 knop head schroeven
    2     | Chassis dia 's

2. Het buitenste links spoor en buitenste rechterkolom assembly's koppelen aan de rack cab verticale leden. De vierkante haken zijn gemarkeerd **LH**, **RH**, en **deze kant van** om u te begeleiden u bij de juiste richting is geplaatst.
3. Ga naar de pincodes spoor op de front-end en de achterzijde van de trein-assembly. Breid het spoor tussen de berichten rack passen en de pincodes invoegen in de front-end- en achterkant rack post verticale lid gaten. Zorg ervoor dat de assembly spoor niveau is.
4. Twee van de opgegeven metrische schroeven gebruiken voor het beveiligen van de trein assembly op het rek verticale leden. Een installatie op de front-end en een op de achterkant gebruiken.
5. Herhaal deze stappen voor de andere spoor-assembly.<br/>
   
     ![Spoor dia's koppelen met rack cab](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Buitenste spoor assembly's koppelen aan het rek**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |MSS schroeven |
   |   2 |Vierkante hole front rack na installatie |
   |   3 |Links spoor front locatie pincodes |
   |   4 |MSS schroeven |
   |   5 |Links spoor achter locatie pincodes |

### <a name="mounting-the-device-in-the-rack"></a>Het apparaat in het rek monteren
De volgende stappen uit voor het koppelen van het apparaat in het rek met behulp van het rackrails die alleen zijn geïnstalleerd, worden uitgevoerd.

#### <a name="to-mount-the-device"></a>Koppelen van het apparaat
1. Met een helper de behuizing lift- en het afgestemd op de rackrails.
2. Zorgvuldig invoegen van het apparaat in de rails en push vervolgens het apparaat volledig in het rack cab.<br/>
   
    ![Apparaat invoegen in het rek](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Het apparaat in het rek monteren**
3. De limieten links en rechts front flens door binnen de gratis limieten te verwijderen. De limieten flens uitgelijnd gewoon naar de flenzen.
4. De ruimte in het rek door het installeren van een opgegeven Phillips head schroeven via elke flens, linker- en beveiligen.
5. Installeer de limieten flens door ze naar de gewenste positie te drukken en ze uitlijning in plaats.<br/>
   
     ![Flens caps installeren](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **De limieten flens installeren**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Behuizing ervan schroeven |

De volgende stap is het uw apparaat voor voeding, netwerk- en seriële toegang bekabelen.

## <a name="cable-your-storsimple-8100-device"></a>Uw StorSimple 8100-apparaat bekabelen
De volgende procedures wordt uitgelegd hoe u uw StorSimple 8100-apparaat voor voeding, netwerk- en seriële verbindingen bekabelen.

### <a name="prerequisites"></a>Vereisten
Voordat u de bekabeling van uw apparaat, moet u:

* Uw apparaat, volledig uitgepakt en rack gekoppeld.
* 2 power-kabels die voor uw apparaat
* Toegang tot 2 Power Distribution Units (aanbevolen).
* Netwerkkabels
* Seriële kabels opgegeven
* Seriële USB converter met de juiste stuurprogramma geïnstalleerd op uw PC (indien nodig)
* Opgegeven 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
* [Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Energiebeheer-bekabeling
Uw apparaat bevat redundante voeding en koeling Modules (PCMs). Zowel PCMs moet worden geïnstalleerd en die zijn verbonden met verschillende energiebronnen om hoge beschikbaarheid te garanderen.

De volgende stappen uitvoeren om uw apparaat voor power bekabelen.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Netwerkbekabeling
Uw apparaat is een actief / stand-by-configuratie: op elk moment een module van de domeincontroller actief is en verwerking van alle bewerkingen voor de schijf en netwerk terwijl de andere controllermodule is in stand-by. Als een domeincontroller is mislukt, wordt de stand-by-controller onmiddellijk wordt geactiveerd en blijft de schijf en netwerken bewerkingen.

Ter ondersteuning van deze redundante controller-failover, moet u het netwerk van uw apparaat bekabelen zoals beschreven in de volgende stappen uit.

#### <a name="to-cable-for-network-connection"></a>Op de kabel voor netwerkverbinding
1. Uw apparaat heeft zes netwerkinterfaces op elke domeincontroller: vier 1 Gbps en twee 10 Gbps Ethernet-poorten. Identificeer de verschillende gegevenspoorten op de backplane van uw apparaat.
   
    ![Backplane van 8100-apparaat](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Gegevenspoorten terug van het apparaat worden weergegeven**
   
   | Label | Beschrijving |
   | --- | --- |
   |   0,1,4,5 |1 GbE-netwerkinterfaces |
   |   2,3 |10 GbE-netwerkinterfaces |
   |   6 |Seriële poorten |
2. Zie het volgende diagram voor netwerkbekabeling. (De minimale configuratie wordt weergegeven door ononderbroken blauwe lijnen. Aanvullende configuratie vereist voor hoge beschikbaarheid en prestaties wordt weergegeven als stippellijnen.)

    ![Uw apparaat 2U voor netwerk-kabel](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Netwerk-bekabeling voor uw apparaat**

   |Label | Beschrijving |
   |----- | ----------- |
   | A    | LAN met toegang tot Internet |
   | B    | Controller 0 |
   | C    | PCM 0 |
   | D    | Controller 1 |
   | E    | PCM 1 |
   | F, G | Hosts |
   | 0-5  | Netwerkinterfaces |



Wanneer het apparaat bekabeling, wordt de minimale configuratie vereist:

* Ten minste twee netwerkinterfaces verbonden op elke domeincontroller met één voor toegang tot de cloud en één voor iSCSI. De DATA 0 poort is automatisch ingeschakeld en geconfigureerd via de seriële console van het apparaat. Naast de DATA 0 moet een andere gegevenspoort ook worden geconfigureerd via de klassieke Azure portal. In dit geval verbinding maken met DATA 0 poortnummer op de primaire LAN (netwerk met toegang tot Internet). De andere gegevenspoorten kunnen worden verbonden met SAN/iSCSI LAN (VLAN)-segment van het netwerk, afhankelijk van de beoogde rol.
* Identieke interfaces op elke domeincontroller verbonden met hetzelfde netwerk om ervoor te zorgen van beschikbaarheid als er een failover van de controller optreedt. Als u ervoor kiest om DATA 0- en DATA 3 voor een van de controllers verbinding te maken, moet u bijvoorbeeld verbinding maken met de bijbehorende DATA 0- en DATA 3 op de andere controller.

Houd er rekening mee voor hoge beschikbaarheid en prestaties:

* Indien mogelijk, configureert u een combinatie van een netwerkinterface voor cloudtoegang (1 GbE) en een ander paar voor iSCSI (10 GbE aanbevolen) op elke domeincontroller.
* Indien mogelijk, verbinding maken met twee verschillende switches om ervoor te zorgen beschikbaarheid op basis van een switch-fout van elke domeincontroller met netwerkinterfaces. De afbeelding ziet u de twee 10 GbE netwerkinterfaces, DATA 2 en DATA 3, van elke domeincontroller die is verbonden met twee verschillende switches.

Raadpleeg voor meer informatie de **netwerkinterfaces** onder de [vereisten voor hoge beschikbaarheid voor uw StorSimple-apparaat](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Als u van SFP + infraroodzenders met uw 10 GbE-netwerkinterfaces gebruikmaakt, gebruikt u de opgegeven QSFP-SFP + adapters. Ga voor meer informatie naar [ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seriële poort bekabeling
De volgende stappen uitvoeren om de seriële poort-kabel.

#### <a name="to-cable-for-serial-connection"></a>Op de kabel voor seriële verbinding
1. Uw apparaat heeft een seriële poort op elke domeincontroller die wordt geïdentificeerd door een moersleutelpictogram. Zie de afbeelding in de [netwerkkabels](#network-cabling) sectie te vinden van de seriële poorten op de backplane van uw apparaat.
2. Identificeer de actieve controller op uw apparaat-backplane. Een knipperende blauw LED geeft aan dat de domeincontroller actief is.
3. De opgegeven seriële kabels (indien nodig, het USB-seriële conversieprogramma voor uw laptop), en koppel de-console of de computer (met terminal emulatie van het apparaat) naar de seriële poort van de actieve controller.
4. De serie USB-stuurprogramma's (geleverd met het apparaat) installeren op uw computer.
5. Instellen van de seriële verbinding als volgt: 115.200 baudrate, 8 gegevensbits 1 stop bits, geen pariteit en datatransportbesturing ingesteld op None.
6. Controleren of de verbinding werkt door te drukken op Enter in de console. Het menu van een seriële console moet worden weergegeven.

> [!NOTE]
> **Lights-Out Management**: wanneer het apparaat is geïnstalleerd in een extern datacenter of in een kamer van de computer met beperkte toegang, zorg ervoor dat de seriële verbindingen met beide controllers altijd met een seriële console-switch of een vergelijkbare apparatuur verbonden bent. Hierdoor kan de out-of-band-beheer op afstand en ondersteuning voor bewerkingen als er netwerkonderbrekingen of onverwachte fouten.
> 
> 

Uw apparaat is nu bekabeld voor voeding, toegang tot het netwerk en seriële verbinding. De volgende stap is het configureren van de software en implementeren van uw apparaat.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [implementeren en configureren uw on-premises StorSimple-apparaat](storsimple-8000-deployment-walkthrough-u2.md).

