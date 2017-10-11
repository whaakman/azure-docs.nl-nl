---
title: Microsoft Azure StorSimple 8600-apparaat installeert | Microsoft Docs
description: Beschrijft hoe uitpakken, op rek monteren en bekabelen uw StorSimple 8600-apparaat voordat u implementeert en configureert de software.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/24/2016
ms.author: alkohli
ms.openlocfilehash: 309ceba2d65c0745ba1acac698acb62526ab8078
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Unpack, rack koppelen, en uw StorSimple 8600-apparaat bekabelen
## <a name="overview"></a>Overzicht
Microsoft Azure StorSimple 8600 is een dubbele behuizing-apparaat en bestaat uit een primaire en een EBOD behuizing. Deze zelfstudie wordt uitgelegd hoe u uitpakken, rek monteren en hardware van de kabel de 8600 StorSimple-apparaten voordat u de software StorSimple configureren.

## <a name="unpack-your-storsimple-8600-device"></a>Uw StorSimple 8600-apparaat uitpakken
De volgende stappen bevatten duidelijke, gedetailleerde instructies voor het uitpakken van uw StorSimple 8600-opslagapparaat. Dit apparaat wordt geleverd in vakken, één voor de primaire behuizing en één voor de EBOD behuizing. Deze twee vakken worden vervolgens in één vak geplaatst.

### <a name="prepare-to-unpack-your-device"></a>Uitpakken van het apparaat voorbereiden
Voordat u uw apparaat uitpakken, controleert u de volgende informatie.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

1. Zorg ervoor dat u twee personen beschikbaar hebt voor het beheren van het gewicht van het apparaat als u deze handmatig worden verwerkt. Een volledig geconfigureerde behuizing wegen tot 32 kg (70 lbs.).
2. Het selectievakje in op een plat niveau vlak plaatsen.

Vervolgens wordt de volgende stappen uit te pakken van uw apparaat.

#### <a name="to-unpack-your-device"></a>Uitpakken van het apparaat
1. Controleer het en het schuim verpakking voor crushes, delen, water schade of andere duidelijk schade. Als het vak of verpakking ernstig is beschadigd, opent u het niet. Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) om te bepalen of het apparaat zich in goede staat.
2. Open het buitenste en los van de twee selectievakjes die overeenkomt met de primaire en EBOD behuizingen. U kunt nu de primaire en behuizingen EBOD uitpakken. De volgende afbeelding ziet de uitgepakte weergave van een van de bijlagen.
   
    ![Uitpakken van uw opslagapparaat](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **Uitgepakte weergave van uw opslagapparaat**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Verpakking vak |
   |   2 |SAS-kabels (in het systeemvak, accessoires en kabels) |
   |   3 |Onder schuim |
   |   4 |Apparaat |
   |   5 |Bovenste schuim |
   |   6 |Accessoires vak |
3. Zorg ervoor dat er na het uitpakken van de twee vakken:
   
   * 1 primaire behuizing (de primaire behuizing en EBOD behuizing zijn in twee afzonderlijke vakken)
   * 1 EBOD behuizing
   * 4 stroomkabels, 2 in elk vak
   * 2 SAS-kabels (met de primaire behuizing EBOD behuizing)
   * 1 crossover Ethernet-kabel
   * 2 seriële console kabels
   * 1 seriële USB converter voor de seriële toegang
   * 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
   * 2 rack mount kits (4 side rails met koppelen van de hardware, 2 voor de primaire behuizing en EBOD behuizing), 1 in elk vak
   * Gestarte documentatie ophalen
     
     Als u een van de hierboven vermelde items niet ontvangen [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).  

De volgende stap is het rack koppelen van uw apparaat.

## <a name="rack-mount-your-storsimple-8600-device"></a>Uw StorSimple 8600-apparaat rack koppelen
De volgende stappen voor het installeren van uw StorSimple 8600-opslagapparaat in een standaard 19-inch rek met vooraan en achteraan advertenties. Dit apparaat wordt geleverd met twee behuizingen: een primaire-behuizing en een EBOD behuizing. Deze moeten rack gekoppeld.

De installatie bestaat uit meerdere stappen, die wordt besproken in de volgende procedures.

> [!IMPORTANT]
> StorSimple-apparaten moet rek wordt gemonteerd voor een juiste werking.
> 
> 

### <a name="site-preparation"></a>Voorbereiding van de site
De bijlagen moeten worden geïnstalleerd in een standaard 19-inch rack die front- en achterste advertenties. Gebruik de volgende procedure om voor te bereiden voor de rackinstallatie van.

#### <a name="to-prepare-the-site-for-rack-installation"></a>De site voor de rackinstallatie van voorbereiden
1. Zorg ervoor dat de primaire en behuizingen EBOD verwerken veilig op een platte, stabiele en niveau werkoppervlak (of vergelijkbaar).
2. Controleer of de site waar u van plan bent om in te stellen van een onafhankelijke bron of een rek Power Distribution Unit (PDU) met een noodvoeding (UPS) standaard Netstroom zijn aangesloten.
3. Zorg ervoor dat één 4U (2 X 2U) sleuf is beschikbaar op het rek waarin u van plan bent de bijlagen koppelen.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

 Zorg ervoor dat u twee personen beschikbaar hebt voor het beheren van het gewicht als u de Apparaatinstelling handmatig verwerkt. Een volledig geconfigureerde behuizing wegen tot 32 kg (70 lbs.).

### <a name="rack-prerequisites"></a>Rack-vereisten
De bijlagen zijn ontworpen voor installatie in een standaard 19-inch rack cab met:

* Minimale diepte van 27.84 inches van rek te boeken
* Maximale gewicht van 32 kg voor het apparaat
* Maximale back druk van 5 Pascal (0,5 mm water meter)

### <a name="rack-mounting-rail-kit"></a>Rek-montage spoor kit
Een set koppelen rails krijgt voor gebruik met de 19-inch rack CAB-bestand. De spoorstaven zijn getest voor het afhandelen van het gewicht maximale behuizing. Deze rails wordt ook installatie van meerdere insluitingen zonder verlies van ruimte binnen het rack toestaan. Installeer eerst de EBOD behuizing.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>De behuizing EBOD installeren op de spoorstaven
1. Deze stap alleen uitvoeren als interne rails zijn niet geïnstalleerd op uw apparaat. Normaal gesproken zijn de binnenste spoorstaven geïnstalleerd in de fabriek. Als rails niet zijn geïnstalleerd, installeert u de links-spoor en rechts spoor dia aan de kant van het chassis behuizing. Ze koppelen met behulp van zes metrische schroeven voor elke zijde. Om te helpen met de afdrukstand, de dia spoor zijn gemarkeerd **LH – voorzijde** en **RH – voorzijde**, en het einde die is aangebracht vanaf de achterzijde van de behuizing heeft een Tapse end.
   
    ![Spoor dia koppelen aan behuizing chassis](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Spoor dia koppelen aan welke zijden van de behuizing**
   
   | Label | Beschrijving |
   | --- | --- |
   |  1 |M 3 x 4 knop head schroeven |
   |  2 |Chassis dia |
2. Het linkerdeelvenster spoor en rechterkant assembly's koppelen aan de rack cab verticale leden. De vierkante haken zijn gemarkeerd **LH**, **RH**, en **deze kant up** om u te begeleiden juiste richting.
3. Zoek de pincodes spoor op de voorgrond en de achterzijde van de assembly spoor. De spoor tussen de berichten rack passen en de pincodes invoegen in de voor- en achterzijde rack post verticale lid gaten uitbreiden. Zorg ervoor dat de assembly spoor niveau is.
4. Beveilig de assembly spoor op het rek verticale leden met behulp van twee van de metrische schroeven opgegeven. Een installatie op de voorgrond en een op de achterzijde gebruiken.
5. Herhaal deze stappen voor de andere spoor-assembly.
   
     ![Spoor dia rack cab koppelen](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Spoor assembly's op het rek koppelen**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Installatie clamping |
   |   2 |Vierkante hole front rack na installatie |
   |   3 |Front-spoor locatie pincodes links |
   |   4 |Installatie clamping |
   |   5 |Links achterste spoor locatie pincodes |

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>Koppelen van de behuizing EBOD in het rek
Gebruik de rackrails die alleen zijn geïnstalleerd, uit te voeren de volgende stappen uit om te koppelen van de behuizing EBOD in het rek.

#### <a name="to-mount-the-ebod-enclosure"></a>De behuizing EBOD koppelen
1. Met een helper lift-de behuizing en met de rackrails uitlijnen.
2. Zorgvuldig de behuizing invoegen in de spoorstaven en vervolgens dit doorgeven volledig in het rack cab.
   
    ![Apparaat invoegen in het rek](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Koppelen van de behuizing in het rek**
3. Verwijder de linker- en rechts front flens caps door binnen te halen van de gratis caps. De caps flens uitgelijnd gewoon naar de flenzen.
4. De behuizing in het rack beveiligen door een opgegeven kruiskopschroevendraaier head installatie via elke flens, linker- en installeren.
5. Installeer de caps flens door ze naar de gewenste positie en uitlijning ze naar de juiste plaats.
   
     ![Flens caps installeren](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **De caps flens installeren**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Behuizing fixeerpunt installatie |

### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Koppelen van de primaire behuizing in het rek
Nadat u klaar bent met het koppelen van de behuizing EBOD, moet u de primaire behuizing dezelfde stappen volgen koppelen.

> [!NOTE]
> * Het is mogelijk enkele leeg sleuven in het rek tussen de primaire behuizing en de behuizing EBOD hebben.
> * Gebruik de SAS-kabel opgegeven 2m verbinding maken met de primaire behuizing de behuizing EBOD.
> * Er zijn geen beperkingen op de relatieve plaatsing van de hoofd-eenheid in de eenheid EBOD. Daarom kan de primaire behuizing worden geplaatst in de bovenste sleuf en de onderstaande EBOD behuizing — of vice versa.
> 
> 

De volgende stap is uw apparaat voor voeding, netwerk- en seriële toegang bekabelen.

## <a name="cable-your-storsimple-8600-device"></a>Uw StorSimple 8600-apparaat bekabelen
De volgende procedures wordt uitgelegd hoe uw StorSimple 8600-apparaat voor voeding, netwerk- en seriële verbindingen bekabelen.

### <a name="prerequisites"></a>Vereisten
Voordat u uw apparaat bekabelen, moet u:

* Uw primaire enclosure en de behuizing EBOD volledig uitgepakt
* 4 power kabels (2 elke voor de primaire en de behuizing EBOD) bij uw apparaat
* 2 SAS-kabels geleverd met het apparaat verbinding maken met de behuizing EBOD de primaire behuizing
* Toegang tot 2 Power Distribution Units (PDU's) (aanbevolen)
* Netwerkkabels
* Seriële kabels opgegeven
* Seriële USB converter met het juiste stuurprogramma geïnstalleerd op uw PC (indien nodig)
* Opgegeven 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
* [Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS en Power bekabeling
Uw apparaat heeft zowel een primaire-behuizing en een EBOD behuizing. Hiervoor moet de eenheden op elkaar worden bekabeld voor seriële gekoppelde SCSI (SAS)-verbinding en geen stroom.

Bij het instellen van dit apparaat voor de eerste keer, voer de stappen voor het SAS-kabels eerst en voltooi de stappen voor het power bekabeling.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Netwerkkabels
Het apparaat zich in een actief/stand-byconfiguratie: op elk gewenst één module van de domeincontroller actief is en verwerking van alle bewerkingen voor de schijf en netwerk terwijl de domeincontroller-module is stand-by. Als een domeincontroller-fout optreedt, wordt de stand-by-controller onmiddellijk wordt geactiveerd en blijft alle bewerkingen van de schijf en netwerken.

Ter ondersteuning van deze failover redundante domeincontroller, moet u het netwerk van uw apparaat bekabelen zoals weergegeven in de volgende stappen uit.

#### <a name="to-cable-for-network-connection"></a>Op de kabel voor netwerkverbinding
1. Uw apparaat heeft zes netwerkinterfaces op elke domeincontroller: vier 1 Gbps en twee 10 Gbps Ethernet-poorten. Raadpleeg de volgende afbeelding om de gegevenspoorten op het backplane van uw apparaat te identificeren.
   
     ![Backplane van 8600-apparaat](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **De gegevenspoorten terug van uw apparaat wordt weergegeven**
   
   | Label | Beschrijving |
   | --- | --- |
   |   0,1,4,5 |1 GbE-netwerkinterfaces |
   |   2,3 |10 GbE-netwerkinterfaces |
   |   6 |Seriële poorten |
2. Zie het volgende diagram voor netwerkkabels. (De minimale configuratie wordt weergegeven door ononderbroken blauwe lijnen. Voor hoge beschikbaarheid en prestaties aanvullende configuratie vereist weergegeven door de stippellijn.)

![Uw apparaat 4U voor netwerk bekabelen](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Netwerk bekabeling voor uw apparaat**

| Label | Beschrijving |
| --- | --- |
| A |LAN met internettoegang |
| B |Controller 0 |
| C |PCM 0 |
| D |Controller 1 |
| E |PCM 1 |
| F |EBOD-controller 0 |
| G |EBOD controller 1 |
| H, IK |Hosts (bijvoorbeeld bestandsservers) |
| 0-5 |Netwerkinterfaces |
| 6 |Primaire behuizing |
| 7 |EBOD behuizing |

Wanneer het apparaat bekabeling, wordt de minimale configuratie vereist:

* Ten minste twee netwerkinterfaces verbonden op elke domeincontroller met één voor toegang tot de cloud en één voor iSCSI. De DATA 0 poort is automatisch ingeschakeld en geconfigureerd via de seriële console van het apparaat. Naast de DATA 0 moet een andere gegevenspoort ook worden geconfigureerd via de klassieke Azure portal. In dit geval verbinding maken met DATA 0 poort aan de primaire LAN (netwerk met internettoegang). De andere gegevenspoorten kunnen worden verbonden met SAN/iSCSI-LAN (VLAN)-segment van het netwerk, afhankelijk van de beoogde rol.
* Identieke interfaces op elke domeincontroller verbonden met hetzelfde netwerk om beschikbaarheid te garanderen als een domeincontroller failover plaatsvindt. Als u verbinding maken met DATA 0 en 3 van de gegevens voor een van de domeincontrollers wilt, moet u bijvoorbeeld verbinding maken met de bijbehorende DATA 0 en 3 van de gegevens op de andere controller.

Houd er rekening mee voor hoge beschikbaarheid en prestaties:

* Indien mogelijk, configureert u een andere combinatie voor iSCSI (10 GbE aanbevolen) en een combinatie van netwerkinterface voor toegang tot de cloud (1 GbE) op elke domeincontroller.
* Indien mogelijk verbinding met twee verschillende switches om ervoor te zorgen beschikbaarheid op basis van een storing van de switch netwerkinterfaces van elke domeincontroller. De afbeelding ziet u twee 10 GbE netwerkinterfaces, DATA 2 en DATA 3 uit elke domeincontroller die is verbonden met twee verschillende switches. Raadpleeg voor meer informatie de **netwerkinterfaces** onder de [vereisten voor hoge beschikbaarheid voor uw StorSimple-apparaat](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Als SFP + infraroodzenders met uw 10 GbE-netwerkinterfaces, gebruikt u de opgegeven QSFP-SFP + adapters. Ga voor meer informatie naar [ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seriële poort bekabeling
De volgende stappen uitvoeren om te kabel van de seriële poort.

#### <a name="to-cable-for-serial-connection"></a>Op de kabel voor seriële verbinding
1. Uw apparaat heeft een seriële poort van elke domeincontroller die wordt geïdentificeerd door een moersleutelpictogram. Als u wilt de seriële poorten vinden, raadpleegt u de afbeelding de gegevens poorten op de achterkant van het apparaat toont.
2. Identificeer de actieve controller op uw apparaat-backplane. Een knipperende blauw LED geeft aan dat de domeincontroller actief is.
3. De opgegeven seriële kabel (indien nodig, het USB-seriële conversieprogramma voor uw laptop), en koppel de console of de computer (met terminal emulatie van het apparaat) naar de seriële poort van de actieve controller.
4. De serie USB-stuurprogramma's (met het apparaat verzonden) installeren op uw computer.
5. Stel de seriële verbinding als volgt in:
   
   * 115.200 baud
   * 8 gegevensbits
   * 1 stop-bits
   * Geen pariteit
   * Datatransportbesturing instellen op **None**
6. Controleren of de verbinding werkt met ENTER op de console. Het menu van een seriële console moet worden weergegeven.

> [!NOTE]
> **Beheer van lights-Out:** wanneer het apparaat is geïnstalleerd in een externe datacenter of in een computerruimte met beperkte toegang, moet u zorgen dat de seriële verbindingen met beide domeincontrollers altijd zijn verbonden met een switch van de seriële console of een vergelijkbare apparatuur. Hierdoor kan out-of-band beheer op afstand en ondersteuning-bewerkingen in geval van een onderbreking van de netwerk- of onverwachte storingen.
> 
> 

U hebt uw apparaat voor voeding, toegang tot het netwerk en seriële verbinding bekabeling. De volgende stap is het configureren van de software op uw apparaat.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om [implementeren en configureren van uw on-premises StorSimple-apparaat](storsimple-deployment-walkthrough-u2.md).

