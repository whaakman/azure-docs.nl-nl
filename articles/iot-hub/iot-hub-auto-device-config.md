---
title: Configureren en controleren van de IoT-apparaten op schaal met Azure IoT Hub | Microsoft Docs
description: Azure IoT Hub automatische inrichting configuraties gebruiken voor het toewijzen van een configuratie aan meerdere apparaten
services: iot-hub
documentationcenter: ''
author: ChrisGMsft
manager: timlt
editor: ''
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: 7146fba69857c3a612ce1b3dbb83387c1f3068d6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="configure-and-monitor-iot-devices-at-scale---preview"></a>Configureren en bewaken van IoT-apparaten op schaal - voorbeeld

Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel van de herhalende en complexe taken van het beheer van grote apparaat vloten via het geheel van hun levenscycli. Met automatische device management, kunt u gericht op een reeks apparaten op basis van hun eigenschappen, een gewenste configuratie definiëren en IoT-Hub apparaten bijwerken wanneer ze scope binnenkomen te laten.  Dit wordt uitgevoerd met behulp van de configuratie van een automatische inrichting, die ook u samenvatten voltooiing en naleving kunt, het samenvoegen van de greep en conflicten en implementeer configuraties in een gefaseerde benadering.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Automatische inrichting configuraties werk door een reeks apparaat horende bijwerken met de gewenste eigenschappen en rapportage een overzicht op basis van het apparaat twin gerapporteerd eigenschappen.  Dit introduceert een nieuwe klasse en een JSON-document aangeroepen een _configuratie_ die bestaat uit drie delen:

* De **doel voorwaarde** definieert het bereik van apparaat horende worden bijgewerkt. De doel-voorwaarde is opgegeven als een query op het apparaat twin tags en/of eigenschappen gerapporteerd.

* De **inhoud** bepaalt de gewenste eigenschappen moeten worden toegevoegd of bijgewerkt in het betreffende apparaat horende. De inhoud bevat een pad naar het gedeelte van de gewenste eigenschappen die moeten worden gewijzigd.

* De **metrische gegevens** definiëren van de samenvatting tellingen van verschillende configuratiestatussen zoals **geslaagd**, **In uitvoering**, en **fout**. Aangepaste metrische gegevens zijn opgegeven als de query's op apparaat twin eigenschappen gerapporteerd.  Systeem metrische gegevens zijn standaard metrische gegevens die het meten van de updatestatus twin, zoals het aantal horende apparaten die zijn gericht en het aantal horende die zijn bijgewerkt. 

## <a name="implement-device-twins-to-configure-devices"></a>Implementeer apparaat horende apparaten configureren

Automatische inrichting configuraties vereisen het gebruik van horende apparaten synchroniseren tussen de cloud en apparaten.  Raadpleeg [Understand and gebruik apparaat horende in IoT-Hub] [ lnk-device-twin] voor instructies over het gebruik van horende apparaten.

## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met tags

Voordat u een configuratie maken kunt, moet u opgeven welke apparaten die u wilt beïnvloeden. Azure IoT Hub identificeert-apparaten met labels in de apparaat-twin. Elk apparaat kan meerdere labels hebben, en kunt u ze een manier die zinvol is voor uw oplossing. Als u apparaten in verschillende locaties beheren, mag u de volgende codes toevoegen aan een apparaat-twin:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="create-a-configuration"></a>Een configuratie maken

1. In de [Azure-portal][lnk-portal], gaat u naar uw IoT-hub. 
1. Selecteer **apparaatconfiguratie (preview)**.
1. Selecteer **configuratie toevoegen**.

Er zijn vijf stappen voor het maken van een configuratie. De volgende secties helpt bij elkaar. 

### <a name="step-1-name-and-label"></a>Stap 1: Naam en een Label

1. Geef de configuratie van een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.
1. Labels voor het bijhouden van uw configuraties toevoegen. Labels zijn **naam**, **waarde** paren met een beschrijving van uw configuratie. Bijvoorbeeld: `HostPlatform, Linux` of `Version, 3.0.1`.
1. Selecteer **volgende** verplaatsen naar stap 2. 

### <a name="step-2-specify-settings"></a>Stap 2: Instellingen opgeven

Deze sectie bevat de doel-inhoud moet worden ingesteld in het betreffende apparaat horende. Er zijn twee invoeren voor elke set instellingen. De eerste is het apparaat twin-pad, dit is het pad naar de sectie JSON binnen de gewenste twin-eigenschappen die worden ingesteld.  De tweede is de JSON-inhoud moet worden ingevoegd in die sectie. Bijvoorbeeld, het apparaat Twin pad en de inhoud instellen met de volgende:

![Stel het apparaat Twin pad en de inhoud](./media/iot-hub-auto-device-config/create-configuration-full-browser.png)

U kunt ook afzonderlijke instellingen instellen door te geven van het gehele pad in het pad naar de Twin en de waarde in de inhoud niets haakjes. Het pad naar de Twin bijvoorbeeld ingesteld op `properties.desired.chiller-water.temperature` en stelt u de inhoud op: `66`

Als er twee of meer configuraties gericht op hetzelfde apparaat Twin pad, de inhoud van de configuratie van de hoogste prioriteit toepassing (prioriteit wordt gedefinieerd in stap 4).

Als u verwijderen van een eigenschap wilt, geeft u de waarde van de eigenschap naar `null`.

U kunt extra instellingen toevoegen door te selecteren **Apparaatinstelling Twin toevoegen**

### <a name="step-3-specify-metrics-optional"></a>Stap 3: Geef de metrische gegevens (optioneel)

Metrische gegevens bieden een samenvatting van de aantallen voor de verschillende statussen die een apparaat rapporteren kan als gevolg van het toepassen van configuratie-inhoud. U kunt bijvoorbeeld een metriek voor in behandeling zijnde instellingen wijzigingen, een metriek op fouten en gegevens voor wijzigingen in de geslaagde maken.

1. Voer een naam voor **metriek naam**
1. Voer een query voor **metriek Criteria**.  De query is gebaseerd op apparaat twin eigenschappen gerapporteerd.  De metriek geeft het aantal rijen dat wordt geretourneerd door de query.

Bijvoorbeeld: `SELECT deviceId FROM devices WHERE properties.reported.chillerWaterSettings.status='pending'`

U kunt een component dat de configuratie is toegepast, bijvoorbeeld opnemen: `SELECT deviceId FROM devices WHERE configurations.yourconfigname.status='Applied'`


### <a name="step-4-target-devices"></a>Stap 4: Doelapparaten

Gebruik de eigenschap labels van uw apparaat horende toe te passen van de specifieke apparaten dat deze configuratie moeten ontvangen.  U kunt ook doelapparaten door apparaat twin eigenschappen gerapporteerd.

Aangezien configuraties met meerdere zijn op hetzelfde apparaat gericht kunnen, moet u elke configuratie een getal prioriteit geven. Als er een conflict optreedt is, wordt de configuratie met de hoogste prioriteit wins. 

1. Voer een positief geheel getal voor de configuratie van de **prioriteit**. Hoogste numerieke waarde wordt beschouwd als de hoogste prioriteit. Als twee configuraties de dezelfde prioriteitsnummer hebt, wordt het account waarmee de meeste is gemaakt onlangs wins. 
1. Voer een **doel voorwaarde** om te bepalen welke apparaten met deze configuratie moeten worden toegepast. De voorwaarde is gebaseerd op het apparaat twin tags of apparaat twin eigenschappen gerapporteerd en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld: `tags.environment='test'` of `properties.reported.chillerProperties.model='4000x'`. 
1. Selecteer **volgende** om door te gaan naar de laatste stap.

### <a name="step-5-review-configuration"></a>Stap 5: Configuratie controleren

Controleer uw configuratie-informatie en selecteer vervolgens **indienen**.

## <a name="monitor-a-configuration"></a>Een configuratie controleren

Bekijk de details van een configuratie en controleren van de apparaten waarop deze wordt uitgevoerd, gebruik de volgende stappen:

1. In de [Azure-portal][lnk-portal], gaat u naar uw IoT-hub. 
1. Selecteer **apparaatconfiguratie (preview)**.
1. Inspecteer de configuratielijst. U kunt de volgende gegevens bekijken voor elke configuratie:
   * **ID** -de naam van de configuratie.
   * **Doel voorwaarde** -de query gebruikt voor het definiëren van de betreffende apparaten.
   * **Prioriteit** -het getal prioriteit is toegewezen aan de configuratie.
   * **Aanmaaktijd** -de tijdstempel van wanneer de configuratie is gemaakt. Dit tijdstempel wordt gebruikt voor ties verbroken wanneer twee configuraties dezelfde prioriteit hebben. 
   * **Systeem metrische gegevens** -metrische gegevens die worden berekend door de IoT Hub en kan niet worden aangepast door ontwikkelaars. Gericht geeft het aantal apparaten horende die overeenkomen met de doel-voorwaarde. Van toepassing is het aantal horende apparaten die zijn gewijzigd door de configuratie, waaronder gedeeltelijke wijzigingen in het geval dat een hogere prioriteit afzonderlijke configuratie ook wijzigingen hebt aangebracht kunt opgegeven. 
   * **Aangepaste metrische gegevens** -metrische gegevens die zijn opgegeven door de ontwikkelaar als een query uitgevoerd naar apparaat twin eigenschappen gerapporteerd.  Per configuratie kan maximaal vijf aangepaste metrische gegevens worden gedefinieerd. 
   
1. Selecteer de configuratie die u wilt bewaken.  
1. Inspecteer de configuratie-informatie. U kunt tabbladen gebruiken om specifieke details over de apparaten die worden ontvangen van de configuratie weer te geven: 
   * **Doel voorwaarde** -de apparaten die overeenkomen met de doel-voorwaarde. 
   * **Metrische gegevens** -een lijst met systeem metrische gegevens en aangepaste metrische gegevens.  U kunt een lijst met apparaten die worden geteld voor elke metriek weergeven door te selecteren van de metrische gegevens in de vervolgkeuzelijst **apparaten weergeven**.
   * **Apparaatinstellingen Twin** -de twin-instellingen voor apparaten die zijn ingesteld door de configuratie. 
   * **Configuratie Labels** -sleutel-waardeparen die wordt gebruikt om een configuratie te beschrijven.  Labels hebben geen invloed op de functionaliteit. 

## <a name="modify-a-configuration"></a>Een configuratie wijzigen

Wanneer u een configuratie wijzigt, worden de wijzigingen direct repliceren naar alle apparaten. 

Als u de doelvoorwaarden bijwerkt, gebeuren de volgende updates:
* Als een apparaat-twin niet voldoen aan de oude doelvoorwaarden, maar voldoet aan de nieuwe doel-voorwaarde en deze configuratie de hoogste prioriteit voor dat apparaat is, wordt deze configuratie toegepast op de apparaat-twin. 
* Als een apparaat-twin voldoet niet aan de doelvoorwaarden, de instellingen van de configuratie wordt verwijderd en de apparaat-twin wordt gewijzigd door de configuratie van de volgende hoogste prioriteit. 
* Als een apparaat twin momenteel met deze configuratie niet meer voldoet aan de voorwaarde doel en voldoet niet aan de voorwaarde van het doel van alle andere configuraties, de instellingen van de configuratie wordt verwijderd en geen andere wijzigingen worden aangebracht op de twin. 

Gebruik de volgende stappen voor het wijzigen van een configuratie: 

1. In de [Azure-portal][lnk-portal], gaat u naar uw IoT-hub. 
1. Selecteer **apparaatconfiguratie (preview)**. 
1. Selecteer de configuratie die u wilt wijzigen. 
1. Updates aanbrengen in de volgende velden: 
   * Doelvoorwaarden 
   * Labels 
   * Prioriteit 
   * Metrische gegevens
1. Selecteer **Opslaan**.
1. Volg de stappen in [Monitor een configuratie] [anker-monitor] bekijkt u de wijzigingen implementeert. 

## <a name="delete-a-configuration"></a>Een configuratie verwijderen

Wanneer u een configuratie verwijdert, nemen alle horende apparaten op hun volgende hoogste prioriteit-configuratie. Als horende apparaten niet voldoen aan de voorwaarde van het doel van een andere configuratie, wordt er geen andere instellingen toegepast. 

1. In de [Azure-portal][lnk-portal], gaat u naar uw IoT-hub. 
1. Selecteer **apparaatconfiguratie (preview)**. 
1. Gebruik het selectievakje in om de configuratie die u wilt verwijderen. 
1. Selecteer **Verwijderen**.
1. Een prompt wordt u gevraagd te bevestigen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe configureren en controleren van de IoT-apparaten op schaal. Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Beheren van uw IoT Hub apparaat-id's in bulk][lnk-bulkIDs]
* [IoT Hub metrische gegevens][lnk-metrics]
* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

Als u wilt verkennen met behulp van de IoT Hub apparaat inrichtingsservice om in te schakelen zonder tussenkomst, Zie just-in-time-inrichting: 

* [Azure IoT Hub apparaat-Service inricht][lnk-dps]

[lnk-device-twin]: iot-hub-devguide-device-twins.md
[lnk-bulkIDs]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
[lnk-portal]: https://portal.azure.com
