---
title: StorSimple 8000-serie hardware-onderdelen en de status | Microsoft Docs
description: Informatie over het bewaken van de hardware-onderdelen van uw StorSimple-apparaat via de StorSimple Device Manager-service.
services: storsimple
documentationcenter: ''
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: alkohli
ms.openlocfilehash: 58007eea9ce25423bc3a9c2847de42db04be43eb
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056868"
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-hardware-components-and-status"></a>Gebruik van de service StorSimple Device Manager voor de hardware-onderdelen bewaken en de status

## <a name="overview"></a>Overzicht
Dit artikel beschrijft de verschillende fysieke en logische onderdelen in uw on-premises StorSimple 8000-serie-apparaat. Ook wordt uitgelegd hoe u de status van het apparaat onderdeel bewaken met behulp van de **Status en hardwarestatus** blade in de StorSimple Device Manager-service.

De **Status en hardwarestatus** blade ziet u de status van de hardware van alle onderdelen van de StorSimple-apparaat.

Onder de lijst van onderdelen voor 8100 zijn er drie secties waarin wordt beschreven:

* **Gedeelde onderdelen** – deze maken geen deel uit van de controllers, zoals harde schijven, behuizing, voeding en koeling-Module (PCM)-onderdelen en PCM temperatuur, regel spanning, en huidige sensoren regel.
* **Onderdelen van controller 0** – de onderdelen die zich op Controller 0, zoals controller, SAS-Expander bevinden en -connector, Temperatuursensoren controller en de verschillende netwerkinterfaces bevinden.
* **Onderdelen van controller 1** – de onderdelen die deel uitmaken van Controller 1, vergelijkbaar met die gedetailleerde voor Controller 0.

Een 8600-apparaat heeft extra onderdelen die met de behuizing uitgebreid Bunch van schijven (EBOD overeenkomen). Er zijn vijf secties onder de lijst van onderdelen. Er zijn drie secties die de onderdelen in de primaire behuizing bevatten, en zijn identiek aan die wordt beschreven voor 8100 van deze. Er zijn twee extra secties voor de behuizing EBOD waarin wordt beschreven:

* **Onderdelen van EBOD-Controller 0** – de onderdelen die zich op EBOD behuizing, 0, zoals de EBOD-controller Temperatuursensoren van SAS-Expander bevinden en -connector en domeincontroller bevinden.
* **Onderdelen van EBOD-Controller 1** – de onderdelen die deel uitmaken van EBOD behuizing, 1, vergelijkbaar met die gedetailleerde voor EBOD behuizing 0.
* **Gedeelde onderdelen van EBOD behuizing** – de onderdelen die aanwezig zijn in de EBOD behuizing en PCM die geen deel uitmaken van de EBOD-controller.

> [!NOTE]
> **De status van uw hardware is niet beschikbaar voor een StorSimple Cloud Appliance (8010/8020).**


## <a name="monitor-the-hardware-status"></a>Monitor de status van uw hardware
Voer de volgende stappen uit om de status van de hardware van een apparaatonderdeel weer te geven:

1. Navigeer naar **apparaten**, selecteert u een specifieke StorSimple-apparaat. Ga naar **Monitor > hardwarestatus**.

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health1.png)

2. Zoek de **hardwareonderdelen** sectie en kies uit de beschikbare onderdelen. Klik op het onderdeellabel om de lijst uitvouwen en bekijk de status van de verschillende onderdelen van het apparaat. Zie de [lijst met gedetailleerde onderdelen voor de primaire behuizing](#component-list-for-primary-enclosure-of-storsimple-device) en de [lijst met gedetailleerde onderdelen voor de behuizing EBOD](#component-list-for-ebod-enclosure-of-storsimple-device).

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health2.png)

3. Gebruik de volgende code kleurenschema voor het interpreteren van de onderdeelstatus:
   
   * **Selectievakje groen** – geeft aan een onderdeel in orde **OK** status.
   * **Gele** – geeft aan een gedegradeerde onderdeel in **waarschuwing** staat.
   * **Rood uitroepteken** – Denotes een mislukt onderdeel waarvoor een **fout** status.
   * **Met de zwarte tekst wit** – geeft aan een onderdeel dat niet aanwezig.
   
   De volgende schermafbeelding ziet u een apparaat met componenten in **OK**, **waarschuwing**, en **fout** staat.
       
   ![](./media/storsimple-8000-monitor-hardware-status/hw-health3.png)

   Uitbreiden van de **lijst met gedeelde onderdelen**, zien we dat de NVRAM en het cluster worden verminderd.

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health5.png)

   Uitbreiden van de **onderdelen van Controller 1** lijst ziet u dat knooppunt van het cluster is mislukt.  

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health4.png)  

4. Als er een onderdeel dat zich niet in een **orde** heeft, neem contact op met Microsoft Support. Als er waarschuwingen zijn ingeschakeld op uw apparaat, ontvangt u een e-mailmelding. Als u een mislukte hardwareonderdeel vervangt wilt, Zie [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>Lijst met onderdelen voor primaire behuizing van StorSimple-apparaat
De volgende tabel geeft een overzicht van de fysieke en logische onderdelen die deel uitmaken van de primaire behuizing (aanwezig in 8100- en 8600) van uw on-premises StorSimple-apparaat.

| Onderdeel | Module | Type | Locatie | Veld FRU (replaceable unit)? | Beschrijving |
| --- | --- | --- | --- | --- | --- |
| Station in sleuf [0-11] |Schijfstations |Fysiek |Gedeeld |Ja |Een regel wordt weergegeven voor elk van de SSD of HDD stations op de primaire behuizing. |
| Omgevingstemperatuursensor |Systeembehuizing |Fysiek |Gedeeld |Nee |Hiermee wordt de temperatuur binnen het chassis. |
| Midplane-temperatuursensor |Systeembehuizing |Fysiek |Gedeeld |Nee |Hiermee wordt de temperatuur van het vlak van het midden. |
| Hoorbaar alarm |Systeembehuizing |Fysiek |Gedeeld |Nee |Geeft aan of het subsysteem hoorbaar alarm binnen het chassis functioneel. |
| Systeembehuizing |Systeembehuizing |Fysiek |Gedeeld |Ja |Geeft aan dat de aanwezigheid van een chassis. |
| Insluitingsinstellingen |Systeembehuizing |Fysiek |Gedeeld |Nee |Verwijst naar het voorpaneel van de behuizing. |
| Regel spanningssensors |PCM |Fysiek |Gedeeld |Nee |Talloze line spanningssensors hebben hun status wordt weergegeven, waarin wordt aangegeven of de gemeten spanning binnen de tolerantie. |
| Huidige sensoren |PCM |Fysiek |Gedeeld |Nee |Talloze line huidige sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de huidige gemeten binnen de tolerantie. |
| Temperatuursensoren in PCM |PCM |Fysiek |Gedeeld |Nee |Groot aantal Temperatuursensoren zoals Inlet en hotspots sensoren hebben hun status wordt weergegeven, die aangeeft of de temperatuur gemeten binnen de tolerantie. |
| Voeding [0-1] |PCM |Fysiek |Gedeeld |Ja |Een regel wordt weergegeven voor elk van de stroom wordt voorzien in de twee PCMs zich aan de achterzijde van het apparaat. |
| Koeling [0-1] |PCM |Fysiek |Gedeeld |Ja |Een regel wordt weergegeven voor elk van de vier ventilatoren die zich in de twee PCMs bevinden. |
| Accu [0-1] |PCM |Fysiek |Gedeeld |Ja |Een regel wordt weergegeven voor elk van de back-upbatterij-modules die in de PCM worden geplaatst. |
| Metis |N/A |Logische |Gedeeld |N/A |Geeft de status van de accu's: of kosten in rekening gebracht en ze einde van de levensduur nadert. |
| Cluster |N/A |Logische |Gedeeld |N/A |Geeft de status van het cluster dat is gemaakt tussen de twee geïntegreerde controller-modules. |
| Clusterknooppunt |N/A |Logische |Gedeeld |N/A |Geeft de status van de controller als onderdeel van het cluster. |
| Clusterquorum |N/A |Logische | |N/A |Geeft aan dat de aanwezigheid van het lidmaatschap van de meeste schijfruimte in de HDD-opslaggroep. |
| Gegevensruimte op de harde schijf |N/A |Logische |Gedeeld |N/A |De opslagruimte die wordt gebruikt voor gegevens in de opslaggroep van vasteschijfstations (HDD). |
| Beheerruimte op |N/A |Logische |Gedeeld |N/A |De ruimte die is gereserveerd in de HDD-opslaggroep voor beheertaken. |
| Quorumruimte op |N/A |Logische |Gedeeld |N/A |De ruimte die is gereserveerd in de HDD-opslaggroep voor het clusterquorum. |
| Vervangingsruimte op |N/A |Logische |Gedeeld |N/A |De ruimte die is gereserveerd in de HDD-opslaggroep voor de vervanging van de domeincontroller. |
| Gegevensruimte op de SSD |N/A |Logische |Gedeeld |N/A |De opslagruimte die wordt gebruikt voor gegevens in de opslaggroep Solid-state drive (SSD). |
| SSD NVRAM-ruimte |N/A |Logische |Gedeeld |N/A |De opslagruimte in de SSD-opslaggroep die is toegewezen voor NVRAM-logica. |
| HDD-opslaggroep |N/A |Logische |Gedeeld |N/A |Geeft de status van de logische opslaggroep die is gemaakt op basis van apparaat HDD's. |
| SSD-opslaggroep |N/A |Logische |Gedeeld |N/A |Geeft de status van de logische opslaggroep die is gemaakt op basis van apparaat SSD's. |
| Controller [0-1] [status] |I/O |Fysiek |Controller |Ja |Geeft de status van de domeincontroller, en of deze in de modus actief of stand-by binnen het chassis. |
| Temperatuursensoren in netwerkcontroller |I/O |Fysiek |Controller |Nee |Groot aantal Temperatuursensoren zoals i/o-module, CPU-temperatuur, DIMM en PCIe sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de temperatuur aangetroffen binnen de tolerantie. |
| SAS-expander |I/O |Fysiek |Controller |Nee |Geeft de status van de seriële gekoppelde SCSI (SAS)-expander, dat wordt gebruikt om verbinding maken met de geïntegreerde opslag naar de controller. |
| SAS-connector [0-1] |I/O |Fysiek |Controller |Nee |Geeft de status van elke SAS-connector, die wordt gebruikt om te verbinden met de SAS-expander geïntegreerde-opslag. |
| SBB midplane interconnect |I/O |Fysiek |Controller |Nee |Geeft de status van de midplane-connector, die wordt gebruikt voor elke controller op het vlak van het midden verbinding. |
| Processorcore |I/O |Fysiek |Controller |Nee |Geeft de status van de processor-cores binnen elke domeincontroller. |
| Behuizing van de elektronicavoeding |I/O |Fysiek |Controller |Nee |Geeft de status van het systeem die worden gebruikt door de behuizing. |
| Behuizing van de elektronische diagnose |I/O |Fysiek |Controller |Nee |Geeft de status van de diagnostische gegevens subsystemen weer die is opgegeven door de netwerkcontroller. |
| Bmc (Baseboard Management Controller) |I/O |Fysiek |Controller |Nee |Geeft de status van de baseboard management controller (BMC), dit is een speciale serviceprocessor waarmee het apparaat worden gecontroleerd via sensoren en communiceert met de systeembeheerder via een onafhankelijke-verbinding. |
| Ethernet |I/O |Fysiek |Controller |Nee |Geeft de status van elk van de netwerkinterfaces, dat wil zeggen, het beheer en de gegevenspoorten op de controller. |
| NVRAM |I/O |Fysiek |Controller |Nee |Geeft de status van NVRAM, een niet-vluchtige RAM-geheugen back-up gemaakt door de accu dat dient voor het bewaren van toepassing essentiële gegevens in het geval van een stroomstoring. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>Lijst met onderdelen van EBOD-behuizing van StorSimple-apparaat
De volgende tabel geeft een overzicht van de fysieke en logische onderdelen die deel uitmaken van de behuizing EBOD (alleen aanwezig in het model 8600) van uw on-premises StorSimple-apparaat.

| Onderdeel | Module | Type | Locatie | FRU? | Beschrijving |
| --- | --- | --- | --- | --- | --- |
| Station in sleuf [0-11] |Schijfstations |Fysiek |Gedeeld |Ja |Een regel wordt weergegeven voor elk van de harde schijf stations vooraan in de EBOD-behuizing. |
| Omgevingstemperatuursensor |Systeembehuizing |Fysiek |Gedeeld |Nee |Hiermee wordt de temperatuur binnen het chassis. |
| Midplane-temperatuursensor |Systeembehuizing |Fysiek |Gedeeld |Nee |Hiermee wordt de temperatuur van het vlak van het midden. |
| Hoorbaar alarm |Systeembehuizing |Fysiek |Gedeeld |Nee |Geeft aan of het subsysteem hoorbaar alarm binnen het chassis functioneel. |
| Systeembehuizing |Systeembehuizing |Fysiek |Gedeeld |Ja |Geeft aan dat de aanwezigheid van een chassis. |
| Insluitingsinstellingen |Systeembehuizing |Fysiek |Gedeeld |Nee |Verwijst naar het OPS- of het voorpaneel van de behuizing. |
| Regel spanningssensors |PCM |Fysiek |Gedeeld |Nee |Talloze line spanningssensors hebben hun status wordt weergegeven, waarin wordt aangegeven of de gemeten spanning binnen de tolerantie. |
| Huidige sensoren |PCM |Fysiek |Gedeeld |Nee |Talloze line huidige sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de huidige gemeten binnen de tolerantie. |
| Temperatuursensoren in PCM |PCM |Fysiek |Gedeeld |Nee |Groot aantal Temperatuursensoren zoals Inlet en hotspots sensoren hebben hun status wordt weergegeven, waarin wordt aangegeven of de temperatuur gemeten binnen de tolerantie. |
| Voeding [0-1] |PCM |Fysiek |Gedeeld |Ja |Een regel wordt weergegeven voor elk van de stroom wordt voorzien in de twee PCMs zich aan de achterzijde van het apparaat. |
| Koeling [0-1] |PCM |Fysiek |Gedeeld |Ja |Een regel wordt weergegeven voor elk van de vier ventilatoren die zich in de twee PCMs bevinden. |
| Lokale opslag [HDD] |N/A |Logische |Gedeeld |N/A |Geeft de status van de logische opslaggroep die is gemaakt op basis van apparaat HDD's. |
| Controller [0-1] [status] |I/O |Fysiek |Controller |Ja |Geeft de status van de domeincontrollers in de EBOD-module. |
| Temperatuursensoren in EBOD |I/O |Fysiek |Controller |Nee |Groot aantal Temperatuursensoren van elke domeincontroller hebben hun status wordt weergegeven, waarin wordt aangegeven of de temperatuur aangetroffen binnen de tolerantie. |
| SAS-expander |I/O |Fysiek |Controller |Nee |Geeft de status van de SAS-Expander bevinden, dat wordt gebruikt om verbinding maken met de geïntegreerde opslag naar de controller. |
| SAS-connector [0-2] |I/O |Fysiek |Controller |Nee |Geeft de status van elke SAS-connector, die wordt gebruikt om te verbinden met de SAS-expander geïntegreerde-opslag. |
| SBB midplane interconnect |I/O |Fysiek |Controller |Nee |Geeft de status van de midplane-connector, die wordt gebruikt voor elke controller op het vlak van het midden verbinding. |
| Behuizing van de elektronicavoeding |I/O |Fysiek |Controller |Nee |Geeft de status van het systeem die worden gebruikt door de behuizing. |
| Behuizing van de elektronische diagnose |I/O |Fysiek |Controller |Nee |Geeft de status van de diagnostische gegevens subsystemen weer die is opgegeven door de netwerkcontroller. |
| Verbinding met de apparaatcontroller |I/O |Fysiek |Controller |Nee |Geeft de status van de verbinding tussen de EBOD i/o-module en de apparaatcontroller. |

## <a name="next-steps"></a>Volgende stappen
* Voor het gebruik van de StorSimple Device Manager-service voor het beheren van uw apparaat, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).
* Als u nodig hebt om op te lossen van een apparaatonderdeel dat een gedegradeerde of mislukte status heeft, raadpleeg dan [StorSimple indicatoren](storsimple-monitoring-indicators.md).
* Ter vervanging van een mislukte hardware-onderdeel, Zie [StorSimple onderdeel Hardwarevervanging](storsimple-hardware-component-replacement.md).
* Als zich problemen met het apparaat, blijft [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

