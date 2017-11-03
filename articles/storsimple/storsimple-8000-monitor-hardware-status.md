---
title: StorSimple 8000 series hardwareonderdelen en status | Microsoft Docs
description: Informatie over het bewaken van de hardwareonderdelen van uw StorSimple-apparaat via de service Manager voor StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 90724099842eac513c39dccf113ad1c0a63983f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-hardware-components-and-status"></a>Gebruik van de service Manager voor StorSimple-apparaat aan hardwareonderdelen monitor en status
## <a name="overview"></a>Overzicht
In dit artikel beschrijft de verschillende fysieke en logische onderdelen in uw on-premises StorSimple 8000 series apparaat. Ook wordt uitgelegd hoe de status van het apparaat onderdeel bewaken met behulp van de **Status- en hardware health** blade in de service Manager voor StorSimple-apparaat.

De **Status- en hardware health** blade toont de hardwarestatus van alle onderdelen van het StorSimple-apparaat.

Onder de lijst met onderdelen 8100 zijn er drie secties waarin wordt beschreven:

* **Gedeelde onderdelen** – deze maken geen deel uit van de domeincontrollers, zoals harde schijven, behuizing, PCM-onderdelen en PCM temperatuur, regel spanning en huidige sensoren regel.
* **Onderdelen van de controller 0** – de onderdelen die zich op Controller 0, zoals een domeincontroller, SAS-uitbreidingsmodule en connector temperatuursensors controller en de verschillende netwerkinterfaces bevinden.
* **Onderdelen van de controller 1** – de onderdelen die deel uitmaken van de Controller 1, vergelijkbaar met die gedetailleerde voor Controller 0.

Een 8600-apparaat heeft extra onderdelen die met de behuizing uitgebreid Bunch van schijven (EBOD overeenkomen). Er zijn vijf secties onder de lijst van onderdelen. Er zijn drie secties die de onderdelen in de primaire behuizing bevatten en identiek zijn aan die hierboven worden beschreven voor 8100 van deze. Er zijn twee extra secties vindt u de behuizing EBOD waarin wordt beschreven:

* **EBOD Controller 0 onderdelen** – de onderdelen die zich op EBOD behuizing, 0, zoals de controller EBOD temperatuursensors SAS uitbreidingsmodule en de connector en de domeincontroller bevinden.
* **Onderdelen van EBOD Controller 1** – de onderdelen die deel uitmaken van EBOD behuizing, 1, vergelijkbaar met die gedetailleerde voor EBOD behuizing 0.
* **EBOD behuizing gedeelde onderdelen** – de onderdelen aanwezig zijn op de EBOD behuizing en PCM die geen deel uitmaken van de controller EBOD.

> [!NOTE]
> **De status van de hardware is niet beschikbaar voor een StorSimple-Cloud-apparaat (8010/8020).**


## <a name="monitor-the-hardware-status"></a>De hardwarestatus controleren
Voer de volgende stappen uit om de status van de hardware van een apparaatonderdeel weer te geven:

1. Navigeer naar **apparaten**, selecteert u een specifieke StorSimple-apparaat. Ga naar **Monitor > Hardware health**.

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health1.png)

2. Zoek de **hardwareonderdelen** sectie en kies uit de beschikbare onderdelen. Klik op het onderdeellabel voor de lijst uitvouwen en weergeven van de status van de verschillende Apparaatonderdelen. Zie de [lijst met gedetailleerde onderdelen voor de primaire behuizing](#component-list-for-primary-enclosure-of-storsimple-device) en de [lijst met gedetailleerde onderdelen voor de behuizing EBOD](#component-list-for-ebod-enclosure-of-storsimple-device).

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health2.png)

3. Gebruik de volgende code kleurenschema voor het interpreteren van de onderdeelstatus:
   
   * **Selectievakje groen** – geeft aan een gezonde onderdeel **OK** status.
   * **Gele** – geeft aan een verslechterde onderdeel in **waarschuwing** status.
   * **Rood uitroepteken** – Denotes een mislukte onderdeel dat heeft een **fout** status.
   * **Met de zwarte tekst wit** – geeft aan een onderdeel dat is niet aanwezig.
   
   De volgende schermafbeelding ziet u een apparaat met onderdelen in **OK**, **waarschuwing**, en **fout** status.
       
   ![](./media/storsimple-8000-monitor-hardware-status/hw-health3.png)

   Uitbreiden van de **lijst met gedeelde onderdelen**, ziet u het NVRAM en het cluster zijn gedegradeerd.

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health5.png)

   Uitbreiden van de **Controller 1 onderdelen** lijst ziet u dat het clusterknooppunt is mislukt.  

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health4.png)  

4. Als er een onderdeel dat zich niet in een **goed** status, neem contact op met Microsoft Support. Als u waarschuwingen op het apparaat zijn ingeschakeld, ontvangt u een e-mailmelding. Als u vervangen door een mislukte hardwareonderdeel wilt, Zie [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>Lijst met onderdelen voor primaire behuizing van StorSimple-apparaat
De volgende tabel bevat een overzicht van de fysieke en logische onderdelen die zijn opgenomen in de primaire-behuizing (aanwezig in 8100 en 8600) van uw on-premises StorSimple-apparaat.

| Onderdeel | Module | Type | Locatie | Veld FRU (replaceable unit)? | Beschrijving |
| --- | --- | --- | --- | --- | --- |
| Station in sleuf [0-11] |Harde schijven |Fysieke |Gedeeld |Ja |Een regel is voor elk van de SSD- of de HDD-schijven in de primaire behuizing opgenomen. |
| Ambient-temperatuursensor |Behuizing |Fysieke |Gedeeld |Nee |Meet de temperatuur binnen het chassis. |
| Halverwege vlak-temperatuursensor |Behuizing |Fysieke |Gedeeld |Nee |Meet de temperatuur van het halverwege vlak. |
| Akoestisch alarm |Behuizing |Fysieke |Gedeeld |Nee |Hiermee wordt aangegeven of het subsysteem akoestisch alarm binnen het chassis functioneel. |
| Behuizing |Behuizing |Fysieke |Gedeeld |Ja |De aanwezigheid van een chassis aangeeft. |
| Bijlage-instellingen |Behuizing |Fysieke |Gedeeld |Nee |Verwijst naar het voorpaneel van de behuizing. |
| Regel spanning sensoren |PCM |Fysieke |Gedeeld |Nee |Talrijke regel spanning sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de gemeten spanning binnen de tolerantie. |
| De huidige sensoren regel |PCM |Fysieke |Gedeeld |Nee |Talrijke regel huidige sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de huidige gemeten binnen de tolerantie. |
| Temperatuursensors in PCM |PCM |Fysieke |Gedeeld |Nee |Talrijke temperatuursensors zoals Inlet en Hotspot sensoren hebben hun status wordt weergegeven, die aangeeft of de gemeten temperatuur binnen de tolerantie. |
| Voeding [0-1] |PCM |Fysieke |Gedeeld |Ja |Een regel wordt voor elk van de stroom wordt voorzien in de twee PCMs zich aan de achterzijde van het apparaat weergegeven. |
| Koeling [0-1] |PCM |Fysieke |Gedeeld |Ja |Een regel is voor elk van de vier ventilatoren die zich in de twee PCMs opgenomen. |
| Accu [0-1] |PCM |Fysieke |Gedeeld |Ja |Een regel is voor elk van de back-up batterij-modules die zijn aangebracht in de PCM opgenomen. |
| Metis |N.v.t. |Logische |Gedeeld |N.v.t. |De status van de batterijen: Hiermee wordt aangegeven of ze moeten in rekening gebracht en naderen einde van de levenscyclus. |
| Cluster |N.v.t. |Logische |Gedeeld |N.v.t. |Hier wordt de status van het cluster dat wordt gemaakt tussen de twee geïntegreerde controller-modules. |
| Clusterknooppunt |N.v.t. |Logische |Gedeeld |N.v.t. |Geeft de status van de controller als onderdeel van het cluster. |
| Clusterquorum |N.v.t. |Logische | |N.v.t. |Hiermee geeft u de aanwezigheid van het lidmaatschap van de meeste schijf in de opslaggroep harde schijf. |
| HDD gegevensruimte |N.v.t. |Logische |Gedeeld |N.v.t. |De opslagruimte die wordt gebruikt voor gegevens in de opslaggroep harde schijf (HDD). |
| HDD management ruimte |N.v.t. |Logische |Gedeeld |N.v.t. |De ruimte die is gereserveerd in de opslaggroep HDD voor beheertaken. |
| Harde schijf quorum ruimte |N.v.t. |Logische |Gedeeld |N.v.t. |De ruimte die is gereserveerd in de HDD-opslaggroep voor het clusterquorum. |
| HDD vervanging ruimte |N.v.t. |Logische |Gedeeld |N.v.t. |De ruimte die is gereserveerd in de opslaggroep HDD voor vervanging van de domeincontroller. |
| SSD gegevensruimte |N.v.t. |Logische |Gedeeld |N.v.t. |De opslagruimte die wordt gebruikt voor gegevens in het station Solid-State opslaggroep (SSD). |
| SSD NVRAM ruimte |N.v.t. |Logische |Gedeeld |N.v.t. |De opslagruimte in de SSD-opslaggroep die is toegewezen voor NVRAM logica. |
| HDD-opslaggroep |N.v.t. |Logische |Gedeeld |N.v.t. |Bevat de status van de logische opslaggroep die wordt gemaakt van apparaat HDD's. |
| SSD-opslaggroep |N.v.t. |Logische |Gedeeld |N.v.t. |Bevat de status van de logische opslaggroep die wordt gemaakt van apparaat SSD's. |
| Controller [0-1] [state] |I/O 'S |Fysieke |Domeincontroller |Ja |De status van de domeincontroller en of deze in de actieve of stand-by-modus binnen het chassis. |
| Temperatuursensors in netwerkcontroller |I/O 'S |Fysieke |Domeincontroller |Nee |Talrijke temperatuursensors zoals i/o-module, CPU-temperatuur, DIMM en PCIe sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de temperatuur aangetroffen binnen de tolerantie. |
| SAS uitbreidingsmodule |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van de seriële gekoppelde SCSI (SAS)-uitbreidingsmodule, die wordt gebruikt voor het verbinding maken met de geïntegreerde opslag van de domeincontroller. |
| SAS-connector [0-1] |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van elke SAS-connector, die wordt gebruikt voor de geïntegreerde opslag verbinden met de uitbreidingsmodule SAS. |
| SBB halverwege vlak tussen geheugenonderdelen |I/O 'S |Fysieke |Domeincontroller |Nee |Hiermee geeft u de status van de connector halverwege vlak die verbinding maken met elke domeincontroller het halverwege vlak wordt gebruikt. |
| Processorcore |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van de processor-cores binnen elke domeincontroller. |
| Behuizing electronics power |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van het systeem die wordt gebruikt door de behuizing. |
| Behuizing electronics diagnostische gegevens |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van de diagnostische gegevens subsystemen geleverd door de domeincontroller. |
| Bmc (Baseboard Management Controller) |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van de baseboard management controller (BMC) die een serviceprocessor van gespecialiseerde die het apparaat gecontroleerd door sensoren en communiceert met de systeembeheerder via een onafhankelijke verbinding. |
| Ethernet |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van elk van de netwerkinterfaces, dat wil zeggen, het beheer en de gegevenspoorten op de domeincontroller. |
| NVRAM |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van NVRAM, een niet-vluchtige RAM-geheugen back-up gemaakt door de accu dat dient voor het bewaren van toepassing kritieke gegevens in geval van een stroomstoring. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>Lijst met onderdelen voor EBOD behuizing van StorSimple-apparaat
De volgende tabel bevat een overzicht van de fysieke en logische onderdelen die zijn opgenomen in de behuizing EBOD (alleen aanwezig in 8600 model) van uw on-premises StorSimple-apparaat.

| Onderdeel | Module | Type | Locatie | FRU? | Beschrijving |
| --- | --- | --- | --- | --- | --- |
| Station in sleuf [0-11] |Harde schijven |Fysieke |Gedeeld |Ja |Een regel is voor elk van de harde schijf stations vooraan in de behuizing EBOD opgenomen. |
| Ambient-temperatuursensor |Behuizing |Fysieke |Gedeeld |Nee |Meet de temperatuur binnen het chassis. |
| Halverwege vlak-temperatuursensor |Behuizing |Fysieke |Gedeeld |Nee |Meet de temperatuur van het halverwege vlak. |
| Akoestisch alarm |Behuizing |Fysieke |Gedeeld |Nee |Hiermee wordt aangegeven of het subsysteem akoestisch alarm binnen het chassis functioneel. |
| Behuizing |Behuizing |Fysieke |Gedeeld |Ja |De aanwezigheid van een chassis aangeeft. |
| Bijlage-instellingen |Behuizing |Fysieke |Gedeeld |Nee |Verwijst naar de OPS of het voorpaneel van de behuizing. |
| Regel spanning sensoren |PCM |Fysieke |Gedeeld |Nee |Talrijke regel spanning sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de gemeten spanning binnen de tolerantie. |
| De huidige sensoren regel |PCM |Fysieke |Gedeeld |Nee |Talrijke regel huidige sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de huidige gemeten binnen de tolerantie. |
| Temperatuursensors in PCM |PCM |Fysieke |Gedeeld |Nee |Talrijke temperatuursensors zoals Inlet en Hotspot sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de gemeten temperatuur binnen de tolerantie. |
| Voeding [0-1] |PCM |Fysieke |Gedeeld |Ja |Een regel wordt voor elk van de stroom wordt voorzien in de twee PCMs zich aan de achterzijde van het apparaat weergegeven. |
| Koeling [0-1] |PCM |Fysieke |Gedeeld |Ja |Een regel is voor elk van de vier ventilatoren die zich in de twee PCMs opgenomen. |
| Lokale opslag [HDD] |N.v.t. |Logische |Gedeeld |N.v.t. |Bevat de status van de logische opslaggroep die wordt gemaakt van apparaat HDD's. |
| Controller [0-1] [state] |I/O 'S |Fysieke |Domeincontroller |Ja |Geeft de status van de domeincontrollers in de module EBOD. |
| Temperatuursensors in EBOD |I/O 'S |Fysieke |Domeincontroller |Nee |Talrijke temperatuursensors van elke domeincontroller hebben hun status wordt weergegeven, waarin wordt aangegeven of de temperatuur aangetroffen binnen de tolerantie. |
| SAS uitbreidingsmodule |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van de SAS-uitbreidingsmodule die wordt gebruikt voor het verbinding maken met de geïntegreerde opslag van de domeincontroller. |
| SAS-connector [0-2] |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van elke SAS-connector, die wordt gebruikt voor de geïntegreerde opslag verbinden met de uitbreidingsmodule SAS. |
| SBB halverwege vlak tussen geheugenonderdelen |I/O 'S |Fysieke |Domeincontroller |Nee |Hiermee geeft u de status van de connector halverwege vlak die verbinding maken met elke domeincontroller het halverwege vlak wordt gebruikt. |
| Behuizing electronics power |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van het systeem die wordt gebruikt door de behuizing. |
| Behuizing electronics diagnostische gegevens |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van de diagnostische gegevens subsystemen geleverd door de domeincontroller. |
| Verbinding met apparaat-controller |I/O 'S |Fysieke |Domeincontroller |Nee |Geeft de status van de verbinding tussen de EBOD i/o-module en de apparaat-controller. |

## <a name="next-steps"></a>Volgende stappen
* Voor het gebruik van de service Manager voor StorSimple-apparaat voor het beheren van uw apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).
* Als u een apparaatonderdeel met de gedegradeerde of mislukte status oplossen wilt, raadpleegt u [StorSimple indicatoren](storsimple-monitoring-indicators.md).
* Zie ter vervanging van een mislukte hardwareonderdeel [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).
* Als u doorgaat naar apparaat problemen, [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

