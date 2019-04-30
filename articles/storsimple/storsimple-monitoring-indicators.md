---
title: StorSimple indicatoren | Microsoft Docs
description: Beschrijft de Lichtdioden (LED's) en hoorbaar alarm gebruikt voor het bewaken van de status van het StorSimple-apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: ef8acf1c3c9211168ebacc8d62647f6789c745a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60630561"
---
# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>StorSimple indicatoren gebruiken om uw apparaat te beheren


## <a name="overview"></a>Overzicht
Uw StorSimple-apparaat bevat luminescentiedioden (LED's) en waarschuwingen die u gebruiken kunt voor het bewaken van de modules en de algehele status van het StorSimple-apparaat. De controle-indicatoren kunnen u vinden op de hardware-onderdelen van de primaire ruimte van het apparaat en de EBOD-behuizing. De indicatoren controle kunnen-LED's of hoorbaar alarm zijn.

Er zijn drie LED statussen die worden gebruikt om aan te geven van de status van een module: groen, groen naar rood-geel of rood-geel knipperen.  

* Groen-LED's vertegenwoordigen de operationele status in orde.  
* Knipperen groen naar rood-geel-LED's vertegenwoordigen de aanwezigheid van niet-kritieke situaties die mogelijk tussenkomst van de gebruiker.  
* Rood-geel-LED's erop duiden dat er een kritieke fout is aanwezig in de module.  

De rest van dit artikel beschrijft de verschillende bewakings-LED, hun locaties op het StorSimple-apparaat, de status van het apparaat op basis van de Staten LED en alle bijbehorende hoorbaar alarm.

## <a name="front-panel-indicator-leds"></a>Voorpaneel-LED 's
De front-scherm, ook wel bekend als de *operations deelvenster* of *ops deelvenster*, geeft u de cumulatieve status van alle modules in het systeem. Het voorpaneel identiek zijn op de primaire StorSimple en de EBOD-behuizing is en wordt hieronder weergegeven.  

   ![Voorpaneel van apparaat][1]

Het voorpaneel bevat de volgende indicatoren:  

1. Knop Dempen
2. Energiebeheer-indicator LED (rood-groen-geel)
3. Module fouttolerantie indicator geleid (in rood-geel/uitgeschakeld)
4. Logische fout indicator geleid (in rood-geel/uitgeschakeld
5. Eenheid-ID weergeven  

Het belangrijkste verschil tussen het voorpaneel-LED's voor het apparaat en die voor de EBOD-behuizing is de **System eenheid id-nummer** op het LED-scherm weergegeven. De standaardeenheid weergegeven op het apparaat-ID is **00**, terwijl de ID van de standaard-eenheid weergegeven op de behuizing EBOD **01**. Hiermee kunt u snel onderscheid maken tussen het apparaat en de behuizing EBOD wanneer het apparaat is ingeschakeld. Als uw apparaat is uitgeschakeld, gebruikt u de informatie in [inschakelen op een nieuw apparaat](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) om te onderscheiden van het apparaat van de EBOD-behuizing.  

## <a name="front-panel-led-status"></a>Voorpaneel LED-status
Gebruik de volgende tabel om u te identificeren van de status aangegeven door de LED's op het voorpaneel van het apparaat of de EBOD-behuizing.  

| Inschakelen van het systeem | Module-fout | Logische fout | Waarschuwing | Status |
| --- | --- | --- | --- | --- |
| Rood-geel |UIT |UIT |N/A |Netstroomtoevoer verloren, operationele op back-up power, of op Netstroom zijn aangesloten op en de controller modules zijn verwijderd. |
| Groen |AAN |AAN |N/A |OPS deelvenster inschakelen (5s) status testen |
| Groen |UIT |UIT |N/A |Inschakelen van alle functies die goed |
| Groen |AAN |N/A |PCM fouttolerantie LED's, ventilator fouttolerantie LED 's |PCM schuld, ventilator fouttolerantie, boven of onder temperatuur |
| Groen |AAN |N/A |I/o-module LED 's |Een controller module-fout |
| Groen |AAN |N/A |N/A |Behuizing logische fout |
| Groen |Flash |N/A |Status van de module onder leiding van de controllermodule. PCM fouttolerantie LED's, ventilator fouttolerantie LED 's |Onbekende controller moduletype is geïnstalleerd, I2C bus fout, controller module essentieel product data (VPD)-configuratiefout |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Voeding, koeling module (PCM)-LED 's
Energiebeheer koeling module (PCM)-LED's kunt u vinden op de achterkant van het primaire behuizing of EBOD behuizing op elke PCM-module. In dit onderwerp wordt beschreven hoe u de volgende LED's gebruiken voor het bewaken van de status van uw StorSimple-apparaat.  

* PCM-LED's voor de primaire behuizing
* PCM-LED's voor de behuizing EBOD

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM-LED's voor de primaire behuizing
Het StorSimple-apparaat heeft een 764W PCM-module met een extra accu. De volgende afbeelding ziet u het LED-deelvenster voor het apparaat.  

   ![PCM-LED's op de primaire behuizing][2]

LED legenda:

1. Fout in Echtheidsvoorwaarde power
2. Storing in de ventilator
3. Accu fouttolerantie
4. PCM OK
5. DC-fout
6. Goede accu  

De status van de PCM wordt aangegeven in het venster LED. Het apparaat PCM-LED deelvenster heeft zes LED's. Vier van deze LED's weergegeven de status van de voeding en de ventilator. De resterende twee LED's geven de status van de back-upbatterij-module in de PCM. U kunt de volgende tabellen gebruiken om de status van de PCM te bepalen.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM-LED's voor voeding en -ventilator
| Status | PCM orde (groen) | AC-fouten (geel) | Ventilator (geel) is mislukt | DC is mislukt (geel) |
| --- | --- | --- | --- | --- |
| Er is geen netstroom (naar behuizing) |UIT |UIT |UIT |UIT |
| Er is geen netstroom (alleen deze PCM) |UIT |AAN |UIT |AAN |
| AC presenteren aan PCM - OK |AAN |UIT |UIT |UIT |
| PCM mislukt (ventilator mislukken) |UIT |UIT |AAN |N/A |
| PCM-fouten (via amp over spanning, via een huidig) |UIT |AAN |AAN |AAN |
| PCM (ventilator buiten tolerantie) |AAN |UIT |UIT |AAN |
| Standby-modus |Knipperen |UIT |UIT |UIT |
| PCM firmware downloaden |UIT |Knipperen |Knipperen |Knipperen |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>PCM-LED's voor de back-upbatterij
| Status | Accu goed (groen) | Accu fouttolerantie (geel) |
| --- | --- | --- |
| Accu niet aanwezig |UIT |UIT |
| Accu aanwezig zijn en in rekening gebracht |AAN |UIT |
| Accu in rekening gebracht of onderhoud ontslag |Knipperen |UIT |
| Accu 'soft' veroorzaakt (herstelbare) |UIT |Knipperen |
| Accu "harde" fout (niet-herstelbare) |UIT |AAN |
| Accu disarmed |Knipperen |UIT |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM-LED's voor de behuizing EBOD
De behuizing EBOD heeft een PCM 580 w bij en er geen extra accu. De PCM-Configuratiescherm voor de behuizing EBOD heeft-LED's alleen voor de voedingen en de ventilator. De volgende afbeelding ziet u deze LED's.

   ![PCM-LED's op de behuizing EBOD][3] 

U kunt de volgende tabel gebruiken om te bepalen van de status van de PCM.  

| Status | PCM orde (groen) | AC-fouten (geel) | Ventilator (geel) is mislukt | DC is mislukt (geel) |
| --- | --- | --- | --- | --- |
| Er is geen netstroom (naar behuizing) |UIT |UIT |UIT |UIT |
| Er is geen netstroom (alleen deze PCM) |UIT |AAN |UIT |AAN |
| AC presenteren PCM ON – OK |AAN |UIT |UIT |UIT |
| PCM mislukt (ventilator mislukken) |UIT |UIT |AAN |X |
| PCM-fouten (via amp over spanning over huidige |UIT |AAN |AAN |AAN |
| PCM (ventilator buiten tolerantie) |AAN |UIT |UIT |AAN |
| Stand-by-model |Knipperen |UIT |UIT |UIT |
| PCM firmware downloaden |UIT |Knipperen |Knipperen |Knipperen |

## <a name="controller-module-indicator-leds"></a>Controller-module-LED 's
Het StorSimple-apparaat bevat LED's voor de primaire controller en de EBOD-controller-modules.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Controle-LED's voor de primaire controller
De volgende afbeelding kunt u de LED's op de primaire controller identificeren. (Alle onderdelen worden weergegeven om u te helpen in de afdrukstand.)  

   ![Controle-LED's - primaire controller][4]

Gebruik de volgende tabel om te bepalen of de controllermodule correct functioneert.  

### <a name="controller-indicator-leds"></a>Controller-LED 's
| LED | Description |
| --- | --- |
| ID-LED (blauw) |Geeft aan dat de module is die wordt geïdentificeerd. Als de blauwe LED op een actieve domeincontroller knippert, de controller is de actieve controller en de andere is de stand-by-controller. Zie voor meer informatie, [de actieve controller op uw apparaat identificeren](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Fout met betrekking tot LED (geel) |Geeft aan dat een fout in de controller. |
| OK LED (groen) |Onveranderlijk groen geeft aan dat de controller OK is. Knipperen groen geeft aan dat een domeincontroller VPD-configuratiefout. |
| SAS-activiteit LED's (groen) |Onveranderlijk groen geeft aan dat een verbinding met geen huidige activiteit. Knipperen groen geeft aan dat de verbinding is lopende activiteiten. |
| Status van de Ethernet-LED 's |Rechts wijst op een koppeling/netwerk activiteit: (onveranderlijk groen)-koppeling is actief (knipperen groen) netwerkactiviteit. Links geeft aan dat de snelheid van het netwerk: 1000 Mb/s (geel) en (groen) 100 Mb/s (uit) 10 Mb/s. Dit lampje mogelijk, afhankelijk van het Onderdeelmodel knipperen, zelfs als de netwerkinterface is niet ingeschakeld. |
| POST-LED 's |Geeft de voortgang van de opstartinstallatiekopie aan wanneer de controller is ingeschakeld. Als het StorSimple-apparaat mislukt om op te starten, kunnen deze LED Microsoft Support identificeren van het punt in het opstartproces waarop de fout is opgetreden. |

> [!IMPORTANT]
> Als de fout LED wordt belicht, is er een probleem met de netwerkcontroller-module die mogelijk worden opgelost door de controller opnieuw te starten. Neem contact op met Microsoft Support als dit probleem niet is opgelost wanneer u de controller opnieuw te starten.  
> 
> 

### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Controle-LED's voor de EBOD (EBOD behuizing)
Elk van de 6 Gb/s SAS EBOD-controllers heeft LED's die de status aan te geven, zoals wordt weergegeven in de volgende afbeelding.  

  ![Controle-LED's - EBOD behuizing][5]

Gebruik de volgende tabel om te bepalen of de module EBOD-controller normaal functioneert.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD-controller-module-LED 's
| Status | I/o-module orde (groen) | I/o-module fouttolerantie (geel) | Poort hostactiviteit (groen) |
| --- | --- | --- | --- |
| Controllermodule OK |AAN |UIT |- |
| Controller-module fouttolerantie |UIT |AAN |- |
| Er is geen externe host-poortverbinding |- |- |UIT |
| Externe host-poortverbinding: Er is geen activiteit |- |- |AAN |
| Externe host-poortverbinding - activiteit |- |- |Knipperen |
| Fout bij de metagegevens van de controller module |Knipperen |- |- |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Schijf-LED's voor de primaire behuizing en EBOD behuizing
Het StorSimple-apparaat heeft schijven vindt u in zowel de primaire behuizing en de EBOD-behuizing. Elke schijf bevat een controle-LED's, zoals beschreven in deze sectie. 

Voor de schijven, de status van de schijf wordt aangegeven door een groene LED en een rood-geel-LED gekoppeld aan het begin van elke module drive-provider. De volgende afbeelding ziet u deze LED's.

  ![Schijf-LED 's][6]

Gebruik de volgende tabel om de status van elke harde schijf die invloed hebben op zijn beurt op de algehele voorpaneel LED status te bepalen.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Schijf-LED's voor de behuizing EBOD
| Status | Activiteit OK LED (groen) | Fout met betrekking tot LED (rood-geel) | Ops-deelvenster LED die zijn gekoppeld |
| --- | --- | --- | --- |
| Er is geen station is geïnstalleerd |UIT |UIT |Geen |
| Station geïnstalleerd en operationeel |Knipperen in-of uitschakelen met de activiteit |X |Geen |
| Identiteitsset SCSI Enclosure Services (SES)-apparaat |AAN |Knipperen 1 seconde op/1 seconde uitschakelen |Geen |
| SES apparaat fouttolerantie bit is ingesteld |AAN |AAN |Logische fout (rood) |
| Stroomstoring besturingselement circuit |UIT |AAN |Module-fouten (rood) |

## <a name="audible-alarms"></a>Hoorbaar alarm
Een StorSimple-apparaat bevat hoorbaar alarm die zijn gekoppeld aan zowel de primaire behuizing en de EBOD-behuizing. Een hoorbaar alarm bevindt zich op het voorpaneel (ook wel bekend als in het deelvenster ops) van beide behuizingen. Hoorbaar alarm geeft aan wanneer een fouttoestand aanwezig is. De volgende voorwaarden wordt de waarschuwing te activeren:  

* Ventilator schuld of fout
* Spanning buiten het bereik
* Boven of onder de voorwaarde temperatuur
* Warmte overschrijding
* Systeemfout
* Logische fout
* Levering stroomstoring
* Het verwijderen van een macht koeling module (PCM)  

De volgende tabel beschrijft de verschillende statussen van de waarschuwing.  

### <a name="alarm-states"></a>Waarschuwing-statussen
| Status van waarschuwing | Bewerking | Actie met knop Dempen ingedrukt |
| --- | --- | --- |
| S0 |Normale modus: op de achtergrond |Pieptoon twee keer |
| S1 |Fout-modus: 1 seconde op/1 seconde uitschakelen |Overgang naar S2 of S3 (Zie de opmerkingen) |
| S2 |Modus herinneren: onregelmatige geluid |Geen |
| S3 |Zacht modus: op de achtergrond |Geen |
| S4 |Kritieke fout modus: continue alarm |Niet beschikbaar: dempen niet actief |

> [!NOTE]
> * Waarschuwing status S1, als u niet op dempen binnen twee minuten de status automatisch wordt overgezet naar S2 of S3.  
> * Waarschuwing Staten S1 naar S4 terug naar S0 nadat de fouttoestand is uitgeschakeld.  
> * Kritieke fout staat S4 kan worden ingevoerd van een andere staat.  


U kunt de hoorbaar alarm dempen door te drukken dempen ops in het venster. Automatische dempen gebeurt na twee minuten als de dempen niet handmatig wordt beheerd. Wanneer de waarschuwing gedempt is, blijft het een gesprek met korte onregelmatige pieptoon om aan te geven dat het probleem blijft optreden. De waarschuwing niet op de achtergrond wanneer alle problemen zijn uitgeschakeld.

De volgende tabel beschrijft de verschillende voorwaarden voor de waarschuwing.

### <a name="alarm-conditions"></a>Waarschuwing-voorwaarden
| Status | Severity | Waarschuwing | OPS deelvenster LED |
| --- | --- | --- | --- |
| PCM alarm, verlies van DC-stroom vanaf een enkele PCM |Fout: Er is geen verlies van redundantie |S1 |Module-fout |
| PCM alarm, verlies van DC-stroom vanaf een enkele PCM |Fout: verlies van redundantie |S1 |Module-fout |
| PCM-ventilator mislukt |Fout: verlies van redundantie |S1 |Module-fout |
| SBB-module gedetecteerd PCM-fout |Fouttolerantie |S1 |Module-fout |
| PCM verwijderd |Configuratiefout |Geen |Module-fout |
| Behuizing-configuratiefout |Fout: kritiek |S1 |Module-fout |
| Lage temperatuur waarschuwing |Waarschuwing |S1 |Module-fout |
| Hoogste temperatuur waarschuwing |Waarschuwing |S1 |Module-fout |
| Over temperatuur |Fout: kritiek |S1 |Module-fout |
| I2C bus fout |Fout: verlies van redundantie |S1 |Module-fout |
| OPS deelvenster communicatiefout (I2C) |Fout: kritiek |S1 |Module-fout |
| Controllerfout |Fout: kritiek |S1 |Module-fout |
| SBB interface module fouttolerantie |Fout: kritiek |S1 |Module-fout |
| Fout in SBB interface module – geen werkende modules resterend |Fout: kritiek |S4 |Module-fout |
| SBB interfacemodule verwijderd |Waarschuwing |Geen |Module-fout |
| Besturingselement voor station stroomstoring |Waarschuwing: zonder verlies van station power |S1 |Module-fout |
| Besturingselement voor station stroomstoring |Fout: essentiële; stroomstoring station |S1 |Module-fout |
| Station verwijderd |Waarschuwing |Geen |Module-fout |
| Onvoldoende acculading beschikbaar |Waarschuwing |geen |Module-fout |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple hardware-onderdelen en de status](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


