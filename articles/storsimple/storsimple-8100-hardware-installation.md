---
title: Microsoft Azure StorSimple 8100-apparaat installeren | Microsoft Docs
description: Beschrijft hoe uitpakken, op rek monteren en bekabelen uw StorSimple 8100-apparaat voordat u implementeert en configureert de software.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 5fbc407a9792d033037fdaa2b14f4055d94c15ab
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Unpack, rack koppelen, en uw StorSimple 8100-apparaat bekabelen
## <a name="overview"></a>Overzicht
Uw Microsoft Azure StorSimple 8100 is een enkel behuizing rek apparaat. Deze zelfstudie wordt uitgelegd hoe u unpack, rack mount en kabel de 8100 StorSimple-apparaat hardware voordat u configureren en implementeren van het StorSimple-apparaat.

## <a name="unpack-your-storsimple-8100-device"></a>Uw StorSimple 8100-apparaat uitpakken
De volgende stappen bevatten duidelijke, gedetailleerde instructies voor het uitpakken van uw StorSimple 8100-opslagapparaat. Dit apparaat wordt geleverd in één vak.

### <a name="prepare-to-unpack-your-device"></a>Uitpakken van het apparaat voorbereiden
Voordat u uw apparaat uitpakken, controleert u de volgende informatie.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

1. Zorg ervoor dat u twee personen beschikbaar hebt voor het beheren van het gewicht van de behuizing als u deze handmatig worden verwerkt. Een volledig geconfigureerde behuizing wegen tot 32 kg (70 lbs.).
2. Het selectievakje in op een plat niveau vlak plaatsen.

Vervolgens wordt de volgende stappen uit te pakken van uw apparaat.

#### <a name="to-unpack-your-device"></a>Uitpakken van het apparaat
1. Controleer het en het schuim verpakking voor crushes, delen, water schade of andere duidelijk schade. Als het vak of verpakking ernstig is beschadigd, opent u het niet. Neem [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om te bepalen of het apparaat zich in goede staat.
2. Pak het vak. De volgende afbeelding ziet u de uitgepakte weergave van uw StorSimple-apparaat.
   
     ![Uitpakken van uw opslagapparaat](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Uitgepakte weergave van uw opslagapparaat**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Verpakking vak |
   |   2 |Onder schuim |
   |   3 |Apparaat |
   |   4 |Bovenste schuim |
   |   5 |Accessoires vak |
3. Zorg ervoor dat er na het uitpakken van het vak:
   
   * 1 apparaat met één behuizing
   * 2 stroomkabels
   * 1 crossover Ethernet-kabel
   * 2 seriële console kabels
   * 1 seriële USB converter voor de seriële toegang
   * 1 fraudebestendig T10 draai
   * 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
   * 1 met het rek monteren kit (2 side rails met koppelen van de hardware)
   * Getting Started-documentatie
     
     Als u een van de hierboven vermelde items niet ontvangen [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

De volgende stap is het rack koppelen van uw apparaat.

## <a name="rack-mount-your-storsimple-8100-device"></a>Uw StorSimple 8100-apparaat rack koppelen
De volgende stappen voor het installeren van uw StorSimple 8100-opslagapparaat in een standaard 19-inch rek met vooraan en achteraan advertenties. Het StorSimple 8100-apparaat heeft één primaire behuizing.

De installatie bestaat uit meerdere stappen, die wordt besproken in de volgende procedures.

> [!IMPORTANT]
> StorSimple-apparaten moet rek wordt gemonteerd voor een juiste werking.
> 
> 

### <a name="prepare-the-site"></a>Voorbereiden van de site
Het apparaat moet worden geïnstalleerd in een standaard 19-inch rack die front- en achterste advertenties. Gebruik de volgende procedure om voor te bereiden voor de rackinstallatie van.

#### <a name="to-prepare-the-site-for-rack-installation"></a>De site voor de rackinstallatie van voorbereiden
1. Zorg ervoor dat het apparaat veilig is gebaseerd op een platte, stabiele en niveau werk water (of vergelijkbaar).
2. Controleer of de site waar u van plan bent om in te stellen van een onafhankelijke bron of een rack power distribution Units (PDU) met een noodvoeding (UPS) standaard Netstroom zijn aangesloten.
3. Zorg ervoor dat één 2U sleuf is beschikbaar op het rek waarin u van plan bent om te koppelen van het apparaat.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

Zorg ervoor dat u twee personen beschikbaar hebt voor het beheren van het gewicht als u de Apparaatinstelling handmatig verwerkt. Een volledig geconfigureerde behuizing wegen tot 32 kg (70 lbs.).

### <a name="rack-prerequisites"></a>Rack-vereisten
De 8100-behuizing is ontworpen voor installatie in een standaard 19-inch rack cab met:

* Minimale diepte van 27.84 inches van rek te boeken.
* Maximale gewicht van 32 kg voor het apparaat
* Maximale back druk van 5 Pascal (0,5 mm water meter).

### <a name="rack-mounting-rail-kit"></a>Rek-montage spoor kit
Een set rails koppelen is beschikbaar voor gebruik met de 19-inch rack CAB-bestand. De spoorstaven zijn getest voor het afhandelen van het gewicht maximale behuizing. Deze rails wordt ook installatie van meerdere insluitingen zonder verlies van ruimte binnen het rack toestaan.

#### <a name="to-install-the-device-on-the-rails"></a>Voor het installeren van het apparaat op de rails
1. Deze stap alleen uitvoeren als interne rails zijn niet geïnstalleerd op uw apparaat. Normaal gesproken zijn de binnenste spoorstaven geïnstalleerd in de fabriek. Als rails niet zijn geïnstalleerd, installeert u de links-spoor en rechts spoor dia aan de kant van het chassis behuizing. Ze koppelen met behulp van zes metrische schroeven voor elke zijde. Om te helpen met de afdrukstand, de dia spoor zijn gemarkeerd **LH – voorzijde** en **RH – voorzijde**, en het einde die is aangebracht vanaf de achterzijde van de behuizing heeft een Tapse end.<br/>
   
    ![Spoor dia koppelen aan behuizing chassis](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Koppelen van interne spoor dia aan welke zijden van de behuizing**
   
    Label | Beschrijving
    ----- | -----------
    1     | M 3 x 4 knop head schroeven
    2     | Chassis dia

2. Het buitenste links spoor en buitenste rechterkant assembly's koppelen aan de rack cab verticale leden. De vierkante haken zijn gemarkeerd **LH**, **RH**, en **deze kant up** om te leiden u door de juiste richting.
3. Zoek de pincodes spoor op de voorgrond en de achterzijde van de assembly spoor. De spoor tussen de berichten rack passen en de pincodes invoegen in de voor- en achterste rack post verticale lid gaten uitbreiden. Zorg ervoor dat de assembly spoor niveau is.
4. Twee van de opgegeven metrische schroeven gebruiken voor het beveiligen van de assembly spoor op het rek verticale leden. Een installatie op de voorgrond en een op de achterzijde gebruiken.
5. Herhaal deze stappen voor de andere spoor-assembly.<br/>
   
     ![Spoor dia rack cab koppelen](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **De buitenste spoor assembly's gekoppeld aan het rek**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Installatie clamping |
   |   2 |Vierkante hole front rack na installatie |
   |   3 |Links spoor front locatie pincodes |
   |   4 |Installatie clamping |
   |   5 |Links spoor achterste locatie pincodes |

### <a name="mounting-the-device-in-the-rack"></a>Koppelen van het apparaat in het rek
Gebruik de rackrails die alleen zijn geïnstalleerd, uit te voeren de volgende stappen uit om te koppelen van het apparaat in het rek.

#### <a name="to-mount-the-device"></a>Koppelen van het apparaat
1. Met een helper lift-de behuizing en met de rackrails uitlijnen.
2. Het apparaat zorgvuldig in de spoorstaven plaatsen en vervolgens push het apparaat volledig in het rack cab.<br/>
   
    ![Apparaat invoegen in het rek](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Koppelen van het apparaat in het rek**
3. Verwijder de linker- en rechts front flens caps door binnen te halen van de gratis caps. De caps flens uitgelijnd gewoon naar de flenzen.
4. De ruimte in het rek door het installeren van een opgegeven kruiskopschroevendraaier head installatie via elke flens, linker- en beveiligen.
5. Installeer de flens caps door ze naar de gewenste positie en uitlijning ze aanwezig.<br/>
   
     ![Flens caps installeren](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **De caps flens installeren**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Behuizing fixeerpunt installatie |

De volgende stap is uw apparaat voor voeding, netwerk- en seriële toegang bekabelen.

## <a name="cable-your-storsimple-8100-device"></a>Uw StorSimple 8100-apparaat bekabelen
De volgende procedures wordt uitgelegd hoe uw StorSimple 8100-apparaat voor voeding, netwerk- en seriële verbindingen bekabelen.

### <a name="prerequisites"></a>Vereisten
Voordat u begint met de bekabeling van uw apparaat, moet u:

* Uw opslagapparaat, volledig uitgepakt en rack gekoppeld.
* 2 power kabels die bij uw apparaat
* Toegang tot 2 Power Distribution Units (aanbevolen).
* Netwerkkabels
* Seriële kabels opgegeven
* Seriële USB converter met het juiste stuurprogramma geïnstalleerd op uw PC (indien nodig)
* Opgegeven 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
* [Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Power bekabeling
Uw apparaat bevat redundante stroom en koeling Modules (PCMs). Zowel PCMs moet worden geïnstalleerd en is verbonden met verschillende energiebeheerschema bronnen om hoge beschikbaarheid te garanderen.

De volgende stappen om uw apparaat voor power bekabelen uitvoeren.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Netwerkkabels
Uw apparaat is een actief/stand-byconfiguratie: op elk gewenst één module van de domeincontroller actief is en verwerking van alle bewerkingen voor de schijf en netwerk terwijl de domeincontroller-module is stand-by. Als een domeincontroller is mislukt, wordt de stand-by-controller onmiddellijk wordt geactiveerd en blijft de schijf en netwerkbewerkingen.

Ter ondersteuning van deze failover redundante domeincontroller, moet u het netwerk van uw apparaat bekabelen zoals beschreven in de volgende stappen uit.

#### <a name="to-cable-for-network-connection"></a>Op de kabel voor netwerkverbinding
1. Uw apparaat heeft zes netwerkinterfaces op elke domeincontroller: vier 1 Gbps en twee 10 Gbps Ethernet-poorten. Identificeer de verschillende gegevenspoorten op het backplane van uw apparaat.
   
    ![Backplane van 8100-apparaat](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Gegevenspoorten terug van het apparaat wordt weergegeven**
   
   | Label | Beschrijving |
   | --- | --- |
   |   0,1,4,5 |1 GbE-netwerkinterfaces |
   |   2,3 |10 GbE-netwerkinterfaces |
   |   6 |Seriële poorten |
2. Zie het volgende diagram voor netwerkkabels. (De minimale configuratie wordt weergegeven door ononderbroken blauwe lijnen. Aanvullende configuratie vereist voor hoge beschikbaarheid en prestaties wordt weergegeven door de stippellijn.)

    ![Uw apparaat 2U voor netwerk bekabelen](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Netwerk bekabeling voor uw apparaat**

   |Label | Beschrijving |
   |----- | ----------- |
   | A    | LAN met internettoegang |
   | B    | Controller 0 |
   | C    | PCM 0 |
   | D    | Controller 1 |
   | E    | PCM 1 |
   | F, G | Hosts |
   | 0-5  | Netwerkinterfaces |



Wanneer het apparaat bekabeling, wordt de minimale configuratie vereist:

* Ten minste twee netwerkinterfaces verbonden op elke domeincontroller met één voor toegang tot de cloud en één voor iSCSI. De DATA 0 poort is automatisch ingeschakeld en geconfigureerd via de seriële console van het apparaat. Naast de DATA 0 moet een andere gegevenspoort ook worden geconfigureerd via de klassieke Azure portal. In dit geval verbinding maken met DATA 0 poort aan de primaire LAN (netwerk met internettoegang). De andere gegevenspoorten kunnen worden verbonden met SAN/iSCSI-LAN (VLAN)-segment van het netwerk, afhankelijk van de beoogde rol.
* Identieke interfaces op elke domeincontroller verbonden met hetzelfde netwerk om beschikbaarheid te garanderen als een domeincontroller failover plaatsvindt. Als u verbinding maken met DATA 0 en 3 van de gegevens voor een van de domeincontrollers wilt, moet u bijvoorbeeld verbinding maken met de bijbehorende DATA 0 en 3 van de gegevens op de andere controller.

Houd er rekening mee voor hoge beschikbaarheid en prestaties:

* Indien mogelijk, configureert u een andere combinatie voor iSCSI (10 GbE aanbevolen) en een combinatie van netwerkinterface voor toegang tot de cloud (1 GbE) op elke domeincontroller.
* Indien mogelijk verbinding met twee verschillende switches om ervoor te zorgen beschikbaarheid op basis van een storing van de switch netwerkinterfaces van elke domeincontroller. De afbeelding ziet u twee 10 GbE netwerkinterfaces, DATA 2 en DATA 3 uit elke domeincontroller die is verbonden met twee verschillende switches.

Raadpleeg voor meer informatie de **netwerkinterfaces** onder de [vereisten voor hoge beschikbaarheid voor uw StorSimple-apparaat](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Als u met uw 10 GbE-netwerkinterfaces SFP + infraroodzenders gebruikt, gebruikt u de opgegeven QSFP-SFP + adapters. Ga voor meer informatie naar [ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seriële poort bekabeling
De volgende stappen uitvoeren om te kabel van de seriële poort.

#### <a name="to-cable-for-serial-connection"></a>Op de kabel voor seriële verbinding
1. Uw apparaat heeft een seriële poort van elke domeincontroller die wordt geïdentificeerd door een moersleutelpictogram. Zie de afbeelding in de [netwerkkabels](#network-cabling) sectie vinden van de seriële poorten op de backplane van uw apparaat.
2. Identificeer de actieve controller op uw apparaat-backplane. Een knipperende blauw LED geeft aan dat de domeincontroller actief is.
3. De opgegeven seriële kabels (indien nodig, het USB-seriële conversieprogramma voor uw laptop), en koppel de console of de computer (met terminal emulatie van het apparaat) naar de seriële poort van de actieve controller.
4. De serie USB-stuurprogramma's (met het apparaat verzonden) installeren op uw computer.
5. De seriële verbinding als volgt instellen: 115.200 baud, 8 gegevensbits, 1 stop bits, geen pariteit en datatransportbesturing ingesteld op None.
6. Controleren of de verbinding werkt met ENTER op de console. Het menu van een seriële console moet worden weergegeven.

> [!NOTE]
> **Lights-Out Management**: wanneer het apparaat is geïnstalleerd in een externe datacenter of in een computerruimte met beperkte toegang, moet u zorgen dat de seriële verbindingen met beide domeincontrollers altijd zijn verbonden met een switch van de seriële console of een vergelijkbare apparatuur. Hierdoor kan de out-of-band beheer op afstand en bewerkingen van de ondersteuning als er netwerkonderbrekingen of onverwachte storingen.
> 
> 

Uw apparaat is nu bekabeld voor voeding, toegang tot het netwerk- en seriële verbinding. De volgende stap is de software configureren en implementeren van uw apparaat.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [implementeren en configureren van uw on-premises StorSimple-apparaat](storsimple-8000-deployment-walkthrough-u2.md).

