---
title: StorSimple indicatoren | Microsoft Docs
description: Beschrijft de Lichtdioden (LED's) en akoestisch alarmen gebruikt voor het bewaken van de status van het StorSimple-apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 9ae0caec211dc1199f0abd2ce9bc0c7ad11c02ec
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>StorSimple indicatoren gebruiken voor het beheren van uw apparaat


## <a name="overview"></a>Overzicht
Uw StorSimple-apparaat bevat Lichtdioden (LED's) en alarmen die u kunt gebruiken voor het bewaken van de modules en de algehele status van het StorSimple-apparaat. De bewaking indicatoren kunnen u vinden op de hardware-onderdelen van de primaire ruimte van het apparaat en de EBOD behuizing. De bewaking indicatoren kunnen LED's of akoestisch alarmen zijn.

Er zijn drie LED statussen die worden gebruikt om de status van een module te geven: groen, groen rood geel of rood oranje knippert.  

* Groen LED's vertegenwoordigen een gezonde operationele status.  
* Knippert groen voor rood oranje LED's vertegenwoordigen de aanwezigheid van niet-kritieke voorwaarden waarvoor u mogelijk tussenkomst van de gebruiker.  
* Rood oranje LED's erop duiden dat er een kritieke fout die aanwezig zijn in de module.  

De rest van dit artikel beschrijft de verschillende bewaking indicatielampjes, de locaties op het StorSimple-apparaat, de status van het apparaat op basis van de Staten LED en eventuele bijbehorende akoestisch alarmen.

## <a name="front-panel-indicator-leds"></a>Voorpaneel LED
Het begin van het deelvenster, ook wel bekend als de *operations Configuratiescherm* of *ops Configuratiescherm*, toont de cumulatieve status van alle modules in het systeem. Het voorpaneel identiek is op de primaire StorSimple en de behuizing EBOD en hieronder wordt weergegeven.  

   ![Apparaat voorpaneel][1]

Het voorpaneel bevat de volgende indicatoren:  

1. Knop Dempen
2. Power indicator LED (rood-groen-geel)
3. Module-fout indicator geleid (in rood-oranje/uit)
4. Logische fout indicator geleid (in rood-oranje/OFF
5. Eenheid-ID weergeven  

Het belangrijkste verschil tussen de voorpaneel LED's voor het apparaat en die voor de behuizing EBOD is de **System eenheid-id-nummer** weergegeven op het beeldscherm LED. De standaardeenheid-ID weergegeven op het apparaat is **00**, terwijl de standaard-eenheid-ID weergegeven op de behuizing EBOD **01**. Hiermee kunt u snel onderscheid maken tussen het apparaat en de behuizing EBOD wanneer het apparaat is ingeschakeld. Als uw apparaat is uitgeschakeld, gebruikt u de informatie in [schakelt u een nieuw apparaat](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) te onderscheiden van het apparaat van de behuizing EBOD.  

## <a name="front-panel-led-status"></a>Voorpaneel LED status
Gebruik de volgende tabel om te identificeren van de status aangegeven door de LED's op het voorpaneel voor het apparaat of de EBOD behuizing.  

| Inschakelen van het systeem | Module-fout | Logische-fout | Waarschuwing | Status |
| --- | --- | --- | --- | --- |
| Rood oranje |UIT |UIT |N.v.t. |Netstroom verloren, uitgevoerd op de back-stroom of Netstroom zijn aangesloten op en de controller modules zijn verwijderd. |
| Groen |AAN |AAN |N.v.t. |OPS Configuratiescherm inschakelen (5s) status testen |
| Groen |UIT |UIT |N.v.t. |Power op alle functies goed |
| Groen |AAN |N.v.t. |Fout met betrekking tot PCM LED's, ventilator veroorzaakt LED 's |Alle PCM-fout ventilator fault, boven of onder temperatuur |
| Groen |AAN |N.v.t. |I/o-module LED 's |Een controller-fout module |
| Groen |AAN |N.v.t. |N.v.t. |Behuizing logica fouttolerantie |
| Groen |Flash |N.v.t. |Status van de module geleid voor module van de domeincontroller. Fout met betrekking tot PCM LED's, ventilator veroorzaakt LED 's |Onbekende controller moduletype geïnstalleerd, I2C bus mislukt, controller-module essentieel product data (VPD)-configuratiefout |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Voeding (PCM) module LED koeling
Energiebeheer koeling module (PCM) LED vindt u op de achterkant van de primaire behuizing of EBOD behuizing op elke PCM-module. In dit onderwerp wordt beschreven hoe u de volgende LED's gebruikt voor het bewaken van de status van uw StorSimple-apparaat.  

* PCM LED's voor de primaire behuizing
* PCM LED's voor de behuizing EBOD

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM LED's voor de primaire behuizing
Het StorSimple-apparaat heeft een 764W PCM-module met een extra batterij. De volgende afbeelding ziet het deelvenster LED voor het apparaat.  

   ![PCM LED's op de primaire behuizing][2]

LED legenda:

1. Fout in Echtheidsvoorwaarde power
2. Ventilator mislukt
3. Fout met betrekking tot accu
4. PCM OK
5. DC-fout
6. Goede accu  

De status van de PCM wordt aangegeven in het deelvenster LED. Het apparaat PCM LED deelvenster heeft zes LED's. De status van de voeding en de ventilator vier van deze LED's worden weergegeven. De resterende twee LED's geven de status van de back-up batterij module in de PCM. U kunt de volgende tabellen gebruiken om te bepalen van de status van de PCM.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM LED voor voeding en -ventilator
| Status | PCM orde (groen) | AC mislukken (geel) | Ventilator (geel) is mislukt | DC is mislukt (geel) |
| --- | --- | --- | --- | --- |
| Er is geen netstroom (met behuizing) |UIT |UIT |UIT |UIT |
| Er is geen netstroom (alleen deze PCM) |UIT |AAN |UIT |AAN |
| AC presenteren aan PCM - OK |AAN |UIT |UIT |UIT |
| PCM mislukken (ventilator mislukken) |UIT |UIT |AAN |N.v.t. |
| PCM-fout (via amp via spanning via huidige) |UIT |AAN |AAN |AAN |
| PCM (ventilator buiten tolerantie) |AAN |UIT |UIT |AAN |
| Standby-modus |Knipperend |UIT |UIT |UIT |
| Download van de PCM-firmware |UIT |Knipperend |Knipperend |Knipperend |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>PCM LED voor de back-up batterij
| Status | Accu goed (groen) | Fout met betrekking tot accu (geel) |
| --- | --- | --- |
| Accu niet aanwezig |UIT |UIT |
| Accu aanwezig zijn en worden in rekening gebracht |AAN |UIT |
| Het laden of onderhoud zuivering accu |Knipperend |UIT |
| Accu 'soft' fout (herstelbare) |UIT |Knipperend |
| Accu "harde" fout (niet-herstelbare) |UIT |AAN |
| Disarmed accu |Knipperend |UIT |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM LED's voor de behuizing EBOD
De behuizing EBOD heeft een PCM 580 w bij en er geen extra accu. De PCM-deelvenster voor de behuizing EBOD heeft indicatielampjes alleen voor de voedingen en de ventilator. De volgende afbeelding ziet deze LED's.

   ![PCM LED's op de behuizing EBOD][3] 

De volgende tabel kunt u de status van de PCM bepaald.  

| Status | PCM orde (groen) | AC mislukken (geel) | Ventilator (geel) is mislukt | DC is mislukt (geel) |
| --- | --- | --- | --- | --- |
| Er is geen netstroom (met behuizing) |UIT |UIT |UIT |UIT |
| Er is geen netstroom (alleen deze PCM) |UIT |AAN |UIT |AAN |
| AC aanwezig PCM ON – OK |AAN |UIT |UIT |UIT |
| PCM mislukken (ventilator mislukken) |UIT |UIT |AAN |X |
| PCM-fout (via amp via spanning via huidige |UIT |AAN |AAN |AAN |
| PCM (ventilator buiten tolerantie) |AAN |UIT |UIT |AAN |
| Stand-by-model |Knipperend |UIT |UIT |UIT |
| Download van de PCM-firmware |UIT |Knipperend |Knipperend |Knipperend |

## <a name="controller-module-indicator-leds"></a>Controller module LED
Het StorSimple-apparaat bevat LED's voor de primaire domeincontroller en de domeincontroller EBOD modules.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Bewaking van LED's voor de primaire domeincontroller
De volgende afbeelding kunt u de LED's op de primaire domeincontroller herkennen. (Alle onderdelen weergegeven om u te helpen bij de afdrukstand.)  

   ![Bewaking LED's - primaire domeincontroller][4]

Gebruik de volgende tabel om te bepalen of de controllermodule correct functioneert.  

### <a name="controller-indicator-leds"></a>Controller LED
| LED | Beschrijving |
| --- | --- |
| ID-LED (blauw) |Hiermee wordt aangegeven dat de module wordt wordt geïdentificeerd. Als de blauwe LED op een actieve domeincontroller knippert, de controller is de actieve controller en de andere is de stand-by-controller. Zie voor meer informatie [identificeren van de actieve controller op uw apparaat](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Fout met betrekking tot LED (geel) |Hiermee geeft u een fout in de controller. |
| OK LED (groen) |Constante groen geeft aan dat de controller OK. Knippert groen geeft aan een configuratiefout VPD-controller. |
| SAS activiteit LED's (groen) |Constante groen geeft aan dat een verbinding zonder huidige activiteit. Groen knippert Hiermee geeft u de verbinding lopende activiteit. |
| Ethernet-status LED 's |Rechts wordt aangegeven koppeling/netwerkactiviteit: (constante groen) koppeling active (knippert groen) netwerkactiviteit. Links geeft Netwerksnelheid: (geel) 1000 Mb/s (groen) 100 Mb/s en (uit) 10 Mb/s. Deze licht mogelijk, afhankelijk van het Onderdeelmodel knipperen zelfs als de netwerkinterface is niet ingeschakeld. |
| POST LED 's |Geeft de voortgang opstarten wanneer de controller is ingeschakeld. Als het StorSimple-apparaat niet kan worden opgestart, helpt dit LED Microsoft Support identificeren punt in het opstartproces waarin de fout is opgetreden. |

> [!IMPORTANT]
> Als de fout LED uit is, is er een probleem met de domeincontroller-module die mogelijk worden opgelost door de domeincontroller opnieuw te starten. Neem contact op met Microsoft Support als dit probleem niet is opgelost wanneer u de domeincontroller opnieuw te starten.  
> 
> 

### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Bewaking van LED's voor de EBOD (EBOD behuizing)
Elk van de 6 Gb/s SAS EBOD controllers heeft LED's die de status aan te geven, zoals wordt weergegeven in de volgende afbeelding.  

  ![Bewaking LED's - EBOD behuizing][5]

Gebruik de volgende tabel om te bepalen of de domeincontroller EBOD module normaal functioneert.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD controller module-LED
| Status | I/o-module orde (groen) | I/o-module-fout (geel) | Host-poortactiviteit (groen) |
| --- | --- | --- | --- |
| Controllermodule OK |AAN |UIT |- |
| Controller module fouttolerantie |UIT |AAN |- |
| Er is geen externe host-poortverbinding |- |- |UIT |
| Externe host-poortverbinding – geen activiteit |- |- |AAN |
| Externe host-poortverbinding - activiteit |- |- |Knipperend |
| Fout in module metagegevens van domeincontroller |Knipperend |- |- |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Schijfstation LED voor de primaire behuizing en EBOD behuizing
Het StorSimple-apparaat heeft schijfstations zich in zowel de primaire behuizing en de EBOD behuizing. Elke harde schijf bevat de bewaking van indicatielampjes, zoals beschreven in deze sectie. 

Voor de schijfstations de stationsstatus wordt aangegeven met een groen LED en een rood oranje LED gekoppeld aan het begin van elke schijf carrier-module. De volgende afbeelding ziet deze LED's.

  ![Schijfstation LED 's][6]

Gebruik de volgende tabel om te bepalen van de status van elke harde schijf, dat op zijn beurt gevolgen heeft voor de algehele voorpaneel LED status.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Schijfstation LED voor de behuizing EBOD
| Status | Activiteit OK LED (groen) | Fout met betrekking tot LED (rood oranje) | Ops Configuratiescherm LED gekoppeld |
| --- | --- | --- | --- |
| Er is geen station is geïnstalleerd |UIT |UIT |Geen |
| Station geïnstalleerd en operationeel |Knipperend in-of uitschakelen met de activiteit |X |Geen |
| SCSI-behuizing Services (SE) apparaat identiteit instellen |AAN |Knippert 1 seconde op/1 seconde uitschakelen |Geen |
| SE apparaat veroorzaakt bit is ingesteld |AAN |AAN |Logische-fout (rood) |
| Stroomstoring besturingselement circuit |UIT |AAN |Module-fout (rood) |

## <a name="audible-alarms"></a>Akoestisch alarmen
Een StorSimple-apparaat bevat akoestisch alarmen die zijn gekoppeld aan zowel de primaire ruimte en de EBOD behuizing. Een akoestisch alarm bevindt zich op het voorpaneel van beide behuizingen (ook wel bekend als in het deelvenster ops). De akoestisch alarm geeft aan wanneer een fouttoestand aanwezig is. De volgende voorwaarden wordt de waarschuwing activeren:  

* Ventilator fout of probleem
* Spanning buiten het bereik
* Boven of onder temperatuur voorwaarde
* Thermische overschrijding
* Systeemfout
* Logische-fout
* Geef stroomstoring
* Verwijderen van een macht koeling van module (PCM)  

De volgende tabel beschrijft de verschillende alarm statussen.  

### <a name="alarm-states"></a>Waarschuwing statussen
| Status van waarschuwing | Actie | Actie knop Dempen ingedrukt |
| --- | --- | --- |
| S0 |Normale modus: achtergrond |Twee keer een piepgeluid laat horen |
| S1 |Modus voor fouten: 1 seconde op/1 seconde uitschakelen |Overgang naar S2 of S3 (Zie Opmerkingen) |
| S2 |Modus herinneren: onregelmatige geluid |Geen |
| S3 |Dempen modus: achtergrond |Geen |
| S4 |Kritieke fout modus: continue alarm |Niet beschikbaar: dempen niet actief |

> [!NOTE]
> * Waarschuwing status S1, als u niet op dempen binnen twee minuten overgezet de status automatisch naar S2 of S3.  
> * Waarschuwing statussen S1 voor S4 terug naar S0 nadat de voorwaarde van de fout is gewist.  
> * Kritieke fout status S4 kan worden ingevoerd vanuit een andere status.  


U kunt de akoestisch alarm dempen door op de knop Dempen in het deelvenster ops te drukken. Automatische dempen zal optreden na twee minuten als de dempen switch niet handmatig wordt beheerd. Wanneer het alarm gedempt is, blijven het geluid met korte onregelmatige pieptonen om aan te geven dat het probleem blijft optreden. Het alarm worden achtergrond wanneer alle problemen zijn uitgeschakeld.

De volgende tabel beschrijft de verschillende voorwaarden van de waarschuwing.

### <a name="alarm-conditions"></a>Waarschuwing-voorwaarden
| Status | Ernst | Waarschuwing | OPS deelvenster LED |
| --- | --- | --- | --- |
| Waarschuwing aan PCM: DC stroomstoring van een enkele PCM |Fout: Er is geen verlies van redundantie |S1 |Module-fout |
| Waarschuwing aan PCM: DC stroomstoring van een enkele PCM |Fout: verlies van redundantie |S1 |Module-fout |
| Mislukken van PCM-ventilator |Fout: verlies van redundantie |S1 |Module-fout |
| SBB module PCM-fout gedetecteerd |Fouttolerantie |S1 |Module-fout |
| PCM verwijderd |Fout in de configuratie |Geen |Module-fout |
| Configuratiefout behuizing |Fout: kritiek |S1 |Module-fout |
| Lage temperatuur waarschuwingsmelding |Waarschuwing |S1 |Module-fout |
| Hoge temperatuur waarschuwingsmelding |Waarschuwing |S1 |Module-fout |
| Via temperatuur |Fout: kritiek |S1 |Module-fout |
| I2C bus mislukt |Fout: verlies van redundantie |S1 |Module-fout |
| OPS deelvenster communicatiefout (I2C) |Fout: kritiek |S1 |Module-fout |
| Fout bij de domeincontroller |Fout: kritiek |S1 |Module-fout |
| SBB interface module fouttolerantie |Fout: kritiek |S1 |Module-fout |
| SBB interface module-fout: geen werkende modules resterende |Fout: kritiek |S4 |Module-fout |
| SBB interfacemodule verwijderd |Waarschuwing |Geen |Module-fout |
| Station besturingselement stroomstoring |Waarschuwing: zonder verlies van station power |S1 |Module-fout |
| Station besturingselement stroomstoring |Fout: essentiële; station stroomstoring |S1 |Module-fout |
| Station verwijderd |Waarschuwing |Geen |Module-fout |
| Onvoldoende stroom beschikbaar |Waarschuwing |Geen |Module-fout |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple hardwareonderdelen en status](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


