---
title: Microsoft Azure StorSimple 8600-apparaat installeren | Microsoft Docs
description: Beschrijft hoe u uitpakken, rek monteren en bekabelen van uw StorSimple 8600-apparaat voordat u implementeert en de software configureert.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 5a8b460441323cb668a3d9939cce434636afc44d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38719002"
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Unpack, rack koppelen, en uw StorSimple 8600-apparaat bekabelen
## <a name="overview"></a>Overzicht
Uw Microsoft Azure StorSimple 8600 is een dubbele behuizing-apparaat en bestaat uit een primaire en een EBOD-behuizing. In deze zelfstudie wordt uitgelegd hoe u uitpakken, rek monteren en kabel de StorSimple 8600-apparaathardware voordat u het configureren van de StorSimple-software.

## <a name="unpack-your-storsimple-8600-device"></a>Uw StorSimple 8600-apparaat uitpakken
De volgende stappen bevatten wissen, gedetailleerde instructies voor het uitpakken van uw StorSimple 8600-opslagapparaat. Dit apparaat is geleverd met twee velden, een voor de primaire behuizing en een voor de EBOD-behuizing. Deze twee vakken worden vervolgens in een vak geplaatst.

### <a name="prepare-to-unpack-your-device"></a>Voorbereidingen voor het uitpakken van het apparaat
Voordat u uw apparaat uitpakken, controleert u de volgende informatie.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

1. Zorg ervoor dat u twee personen beschikbaar voor het beheren van het gewicht van het apparaat hebt als u deze handmatig worden verwerkt. Een volledig geconfigureerde behuizing wegen maximaal 32 kg (70 lbs.).
2. Plaats het vak op een vast, niveau voor aanvallen.

Vervolgens de volgende stappen uit te pakken van uw apparaat.

#### <a name="to-unpack-your-device"></a>Uw apparaat uitpakken
1. Controleer het vak en het schuim verpakking voor crushes, delen, water schade of andere voor de hand liggende schade. Als het selectievakje of de verpakking ernstig is beschadigd, opent u het selectievakje niet. Neem [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om te beoordelen of het apparaat zich in goede staat.
2. Open het buitenste vak en vervolgens neemt u de twee vakken overeenkomt met de primaire en EBOD-behuizingen. U kunt nu de primaire en EBOD-behuizingen uitpakken. De volgende afbeelding toont de uitgepakte weergave van een van de bijlagen.
   
    ![Uw apparaat uitpakken](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **Uitgepakte weergave van uw opslagapparaat**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Verpakking-vak |
   |   2 |SAS-kabels (in de lade accessoires en kabels) |
   |   3 |Onder schuim |
   |   4 |Apparaat |
   |   5 |Bovenste schuim |
   |   6 |Accessoires vak |
3. Zorg ervoor dat u na het uitpakken van de twee velden:
   
   * 1 primaire behuizing (de primaire behuizing en EBOD behuizing zijn in twee afzonderlijke vakken)
   * 1 EBOD behuizing
   * 4 stroomkabels, 2 in elk vak
   * 2 SAS-kabels (voor koppelingen met de primaire behuizing enclosure EBOD)
   * 1 crossover Ethernet-kabel
   * 2 seriële console-kabels
   * 1 serie USB-conversieprogramma voor seriële toegang
   * 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
   * 2 rack mount kits (4 kant rails met koppelen van hardware, 2 voor de primaire behuizing en behuizing EBOD), 1 in elke vak
   * Gestarte documentatie ophalen
     
     Als u een van de items worden weergegeven, niet hebt ontvangen [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).  

De volgende stap is het rack koppelen uw apparaat.

## <a name="rack-mount-your-storsimple-8600-device"></a>Uw StorSimple 8600-apparaat rack koppelen
De volgende stappen voor het installeren van uw StorSimple 8600-apparaat voor gegevensopslag in een standard 19-inch rek met front-end- en achterkant berichten. Dit apparaat wordt geleverd met twee bijlagen: een primaire behuizing en een EBOD-behuizing. Deze moeten rack gekoppeld.

De installatie bestaat uit meerdere stappen, die wordt besproken in de volgende procedures.

> [!IMPORTANT]
> StorSimple-apparaten moet rek gemonteerd voor een goede werking.
> 
> 

### <a name="site-preparation"></a>Site-voorbereiding
De bijlagen moeten worden geïnstalleerd op een standaard 19-inch rek met zowel de front-end en de achterzijde berichten. Gebruik de volgende procedure om voor te bereiden voor rackinstallatie.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Voorbereiden van de site voor de rackinstallatie van een
1. Zorg ervoor dat de primaire en EBOD-behuizingen zijn veilig op een vast, stabiel en niveau werken het oppervlak verwerken (of vergelijkbaar).
2. Controleer of de site waar u van plan bent om in te stellen standard netstroom van een onafhankelijke bron of een rek Power Distribution Unit (PDU) met een noodvoeding (UPS).
3. Zorg ervoor dat één 4U (2 X 2U) de sleuf is beschikbaar op het rek waarin u van plan bent om te koppelen van de bijlagen.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![zware gewicht pictogram](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **waarschuwing!**

 Zorg ervoor dat u twee personen beschikbaar voor het beheren van het gewicht hebt als u omgaan met de installatie van het apparaat handmatig. Een volledig geconfigureerde behuizing wegen maximaal 32 kg (70 lbs.).

### <a name="rack-prerequisites"></a>Rack-vereisten
De bijlagen zijn ontworpen voor de installatie in een standard 19-inch rack cab met:

* Minimale diepte van 27.84 inches van rek te boeken
* Maximale gewicht van 32 kg voor het apparaat
* Maximale zwaar wordt belast van 5 Pascal (0,5 mm water meter)

### <a name="rack-mounting-rail-kit"></a>Rack koppelen spoor kit
Een set rails koppelen wordt geleverd voor gebruik met het 19-inch rack CAB-bestand. De rails getest voor het afhandelen van het gewicht maximale behuizing. Deze rails kunnen ook installatie van meerdere insluitingen hebben zonder verlies van ruimte in het rek te plaatsen. Installeer eerst de EBOD-behuizing.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>De behuizing EBOD installeren op de rails
1. Voer deze stap alleen als interne rails zijn niet geïnstalleerd op uw apparaat. Normaal gesproken zijn de binnenste rails geïnstalleerd in de fabriek. Als de rails niet zijn geïnstalleerd, installeert u de dia's links-spoor en rechts spoor zijden van de behuizing enclosure. Ze koppelen met behulp van zes metrische schroeven aan elke zijde. Om te helpen bij een stand, spoor dia's zijn gemarkeerd **LH – Front** en **RH – Front**, en het einde die vanaf de achterzijde van de behuizing is aangebracht heeft een Tapse end.
   
    ![Spoor dia's koppelen aan behuizing-chassis](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Spoor dia's koppelen aan de zijde van de behuizing**
   
   | Label | Beschrijving |
   | --- | --- |
   |  1 |M 3 x 4 knop head schroeven |
   |  2 |Chassis dia 's |
2. Het linker spoor en rechterkolom assembly's koppelen aan de rack cab verticale leden. De vierkante haken zijn gemarkeerd **LH**, **RH**, en **deze kant van** om u te begeleiden u bij de juiste richting is geplaatst.
3. Ga naar de pincodes spoor op de front-end en de achterzijde van de trein-assembly. Breid het spoor tussen de berichten rack passen en de pincodes invoegen in de front-end- en achterkant rack post verticale lid gaten. Zorg ervoor dat de assembly spoor niveau is.
4. Beveilig de assembly spoor op het rek verticale leden met behulp van twee van de metrische schroeven opgegeven. Een installatie op de front-end en een op de achterkant gebruiken.
5. Herhaal deze stappen voor de andere spoor-assembly.
   
     ![Spoor dia's koppelen met rack cab](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Spoor assembly's koppelen aan het rek**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |MSS schroeven |
   |   2 |Vierkante hole front rack na installatie |
   |   3 |Front-spoor locatie pincodes links |
   |   4 |MSS schroeven |
   |   5 |Links achterzijde spoor locatie pincodes |

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>De behuizing EBOD in het rek monteren
De volgende stappen uit voor het koppelen van de behuizing EBOD in het rek met behulp van het rackrails die alleen zijn geïnstalleerd, worden uitgevoerd.

#### <a name="to-mount-the-ebod-enclosure"></a>De behuizing EBOD koppelen
1. Met een helper de behuizing lift- en het afgestemd op de rackrails.
2. Zorgvuldig de behuizing invoegen in de rails en vervolgens pusht u deze volledig in het rack cab.
   
    ![Apparaat invoegen in het rek](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **De ruimte in het rek monteren**
3. De limieten links en rechts front flens door binnen de gratis limieten te verwijderen. De limieten flens uitgelijnd gewoon naar de flenzen.
4. Beveilig de behuizing in het rack door het installeren van een opgegeven Phillips head schroeven via elke flens, links en rechts.
5. De limieten flens installeren door te drukken naar de gewenste positie en ze naar de gewenste positie uitlijnen.
   
     ![Flens caps installeren](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **De limieten flens installeren**
   
   | Label | Beschrijving |
   | --- | --- |
   |   1 |Behuizing ervan schroeven |

### <a name="mounting-the-primary-enclosure-in-the-rack"></a>De primaire behuizing in het rek monteren
Wanneer u klaar bent met het koppelen van de behuizing EBOD, moet u koppelen van de primaire behuizing dezelfde stappen te volgen.

> [!NOTE]
> * Het is mogelijk dat een paar lege sleuven in het rek tussen de primaire behuizing en de EBOD-behuizing.
> * Gebruik de SAS-kabel opgegeven 2m verbinding maken met de primaire behuizing van de behuizing EBOD.
> * Er zijn geen beperkingen met betrekking tot de relatieve positie van de hoofd-eenheid aan de EBOD-eenheid. Daarom kan de primaire behuizing worden geplaatst in de bovenste site en de onderstaande EBOD-behuizing, of vice versa.
> 
> 

De volgende stap is het uw apparaat voor voeding, netwerk- en seriële toegang bekabelen.

## <a name="cable-your-storsimple-8600-device"></a>Uw StorSimple 8600-apparaat bekabelen
De volgende procedures wordt uitgelegd hoe u uw StorSimple 8600-apparaat voor voeding, netwerk- en seriële verbindingen bekabelen.

### <a name="prerequisites"></a>Vereisten
Voordat u begint met uw apparaat bekabelen, gaat u te werk:

* Uw primaire behuizing en de behuizing EBOD volledig uitgepakt
* 4 power kabels (2 elk voor de primaire en de behuizing EBOD) die voor uw apparaat
* 2 SAS-kabels meegeleverd met het apparaat de behuizing EBOD verbinden met de primaire behuizing
* Toegang tot 2 Power Distribution Units (PDU's) (aanbevolen)
* Netwerkkabels
* Seriële kabels opgegeven
* Serieel-USB-converter met de juiste stuurprogramma geïnstalleerd op uw PC (indien nodig)
* Opgegeven 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
* [Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS- en Power-bekabeling
Uw apparaat heeft zowel een primaire behuizing en een EBOD-behuizing. Hiervoor moet de eenheden op elkaar worden bekabeld voor SAS Serial Attached SCSI ()-connectiviteit en power.

Bij het instellen van dit apparaat voor de eerste keer, voer de stappen voor het SAS-bekabeling eerst en voltooit u de stappen voor het power-bekabeling.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Netwerkbekabeling
Uw apparaat zich in een actief / stand-by-configuratie: op elk moment een module van de domeincontroller actief is en verwerking van alle bewerkingen voor de schijf en netwerk terwijl de andere controllermodule is in stand-by. Als een controller-fout optreedt, wordt de stand-by-controller onmiddellijk wordt geactiveerd en blijft alle bewerkingen voor de schijf en netwerken.

Ter ondersteuning van deze redundante controller-failover, moet u het netwerk van uw apparaat bekabelen zoals wordt weergegeven in de volgende stappen uit.

#### <a name="to-cable-for-network-connection"></a>Op de kabel voor netwerkverbinding
1. Uw apparaat heeft zes netwerkinterfaces op elke domeincontroller: vier 1 Gbps en twee 10 Gbps Ethernet-poorten. Raadpleeg de volgende afbeelding om de gegevenspoorten op de backplane van uw apparaat te identificeren.
   
     ![Backplane van 8600-apparaat](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **De gegevenspoorten terug van uw apparaat worden weergegeven**
   
   | Label | Beschrijving |
   | --- | --- |
   |   0,1,4,5 |1 GbE-netwerkinterfaces |
   |   2,3 |10 GbE-netwerkinterfaces |
   |   6 |Seriële poorten |
2. Zie het volgende diagram voor netwerkbekabeling. (De minimale configuratie wordt weergegeven door ononderbroken blauwe lijnen. Voor hoge beschikbaarheid en prestaties, aanvullende configuratie vereist weergegeven met stippellijnen.)

![Uw apparaat 4U voor netwerk-kabel](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Netwerk-bekabeling voor uw apparaat**

| Label | Beschrijving |
| --- | --- |
| A |LAN met toegang tot Internet |
| B |Controller 0 |
| C |PCM 0 |
| D |Controller 1 |
| E |PCM 1 |
| F |EBOD-controller 0 |
| G |EBOD-controller 1 |
| H, IK |Hosts (bijvoorbeeld bestandsservers) |
| 0-5 |Netwerkinterfaces |
| 6 |Primaire behuizing |
| 7 |EBOD behuizing |

Wanneer het apparaat bekabeling, wordt de minimale configuratie vereist:

* Ten minste twee netwerkinterfaces verbonden op elke domeincontroller met één voor toegang tot de cloud en één voor iSCSI. De DATA 0 poort is automatisch ingeschakeld en geconfigureerd via de seriële console van het apparaat. Naast de DATA 0 moet een andere gegevenspoort ook worden geconfigureerd via de klassieke Azure portal. In dit geval verbinding maken met DATA 0 poortnummer op de primaire LAN (netwerk met toegang tot Internet). De andere gegevenspoorten kunnen worden verbonden met SAN/iSCSI LAN (VLAN)-segment van het netwerk, afhankelijk van de beoogde rol.
* Identieke interfaces op elke domeincontroller verbonden met hetzelfde netwerk om ervoor te zorgen van beschikbaarheid als er een failover van de controller optreedt. Als u ervoor kiest om DATA 0- en DATA 3 voor een van de controllers verbinding te maken, moet u bijvoorbeeld verbinding maken met de bijbehorende DATA 0- en DATA 3 op de andere controller.

Houd er rekening mee voor hoge beschikbaarheid en prestaties:

* Indien mogelijk, configureert u een combinatie van een netwerkinterface voor cloudtoegang (1 GbE) en een ander paar voor iSCSI (10 GbE aanbevolen) op elke domeincontroller.
* Indien mogelijk, verbinding maken met twee verschillende switches om ervoor te zorgen beschikbaarheid op basis van een switch-fout van elke domeincontroller met netwerkinterfaces. De afbeelding ziet u de twee 10 GbE netwerkinterfaces, DATA 2 en DATA 3, van elke domeincontroller die is verbonden met twee verschillende switches. Raadpleeg voor meer informatie de **netwerkinterfaces** onder de [vereisten voor hoge beschikbaarheid voor uw StorSimple-apparaat](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Als u SFP + infraroodzenders met uw 10 GbE-netwerkinterfaces, gebruikt u de opgegeven QSFP-SFP + adapters. Ga voor meer informatie naar [ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw StorSimple-apparaat](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seriële poort bekabeling
De volgende stappen uitvoeren om de seriële poort-kabel.

#### <a name="to-cable-for-serial-connection"></a>Op de kabel voor seriële verbinding
1. Uw apparaat heeft een seriële poort op elke domeincontroller die wordt geïdentificeerd door een moersleutelpictogram. Als u wilt de seriële poorten vinden, raadpleegt u de afbeelding die poorten van de gegevens op de achterkant van het apparaat weergegeven.
2. Identificeer de actieve controller op uw apparaat-backplane. Een knipperende blauw LED geeft aan dat de domeincontroller actief is.
3. De opgegeven seriële kabel (indien nodig, het USB-seriële conversieprogramma voor uw laptop), en koppel de-console of de computer (met terminal emulatie van het apparaat) naar de seriële poort van de actieve controller.
4. De serie USB-stuurprogramma's (geleverd met het apparaat) installeren op uw computer.
5. Stel de seriële verbinding als volgt in:
   
   * 115.200 baudrate
   * 8 databits
   * 1 stop-bits
   * Er is geen pariteit
   * Datatransportbesturing instellen op **None**
6. Controleren of de verbinding werkt door te drukken op Enter in de console. Het menu van een seriële console moet worden weergegeven.

> [!NOTE]
> **Beheer van lights-Out:** wanneer het apparaat is geïnstalleerd in een extern datacenter of in een kamer van de computer met beperkte toegang, zorg ervoor dat de seriële verbindingen met beide controllers altijd met een seriële console-switch of een vergelijkbare apparatuur verbonden bent. Hierdoor kan out-of-band-beheer op afstand en ondersteuning voor bewerkingen in geval van een onderbreking van de netwerk- of onverwachte storingen.
> 
> 

U hebt uw apparaat voor voeding, toegang tot het netwerk en seriële verbinding-bekabeling. De volgende stap is het configureren van de software op uw apparaat.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om [implementeren en configureren uw on-premises StorSimple-apparaat](storsimple-8000-deployment-walkthrough-u2.md).

