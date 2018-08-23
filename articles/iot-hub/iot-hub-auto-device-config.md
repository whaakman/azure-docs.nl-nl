---
title: Configureren en controleren van IoT-apparaten op schaal met Azure IoT Hub | Microsoft Docs
description: Azure IoT Hub automatische inrichting configuraties gebruiken voor het toewijzen van een configuratie op meerdere apparaten
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: 2edde122b109779794bb86752d69a5318edb9235
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42058673"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-portal"></a>Configureren en controleren van IoT-apparaten op schaal met Azure portal

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel herhaalde en complexe taken van grote apparaat vloten via het geheel van de levenscycli hiervan te beheren. Met automatische device management, kunt u richten op een set met apparaten op basis van hun eigenschappen, definieert een gewenste configuratie en IoT Hub-apparaten bijgewerkt wanneer ze scope binnenkomen te laten.  Dit wordt uitgevoerd met behulp van de configuratie van een automatische inrichting, die ook u samenvatten is voltooid en naleving kunt, ingang samenvoegen en conflicten en configuraties in een gefaseerde benadering worden uitgerold.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Automatische inrichting configuraties werk door een set dubbele apparaten bijwerken met de gewenste eigenschappen en rapportage een overzicht op basis van de apparaatdubbel gerapporteerde eigenschappen.  Dit introduceert een nieuwe klasse en JSON-document met de naam een *configuratie* die bestaat uit drie delen:

* De **voorwaarde als doel** definieert het bereik van apparaatdubbels om te worden bijgewerkt. De doelvoorwaarde is opgegeven als een query op apparaat apparaatdubbel-tags en/of gerapporteerde eigenschappen.

* De **inhoud richten** bepaalt de gewenste eigenschappen om te worden toegevoegd of bijgewerkt in de betreffende apparaatdubbels. De inhoud bevat een pad naar de sectie van de gewenste eigenschappen die moeten worden gewijzigd.

* De **metrische gegevens** definiëren de samenvatting tellingen van verschillende configuratiestatussen zoals **succes**, **In voortgang**, en **fout**. Aangepaste metrische gegevens zijn opgegeven als query's op apparaat gerapporteerde eigenschappen.  Metrische systeemmeetgegevens zijn standaard metrische gegevens die het meten van de status van het dubbele bijwerken, zoals het aantal dubbele apparaten die zijn gericht en het aantal dubbele die zijn bijgewerkt. 

## <a name="implement-device-twins-to-configure-devices"></a>Apparaatdubbels implementeren om apparaten te configureren

Automatische inrichting configuraties vereisen het gebruik van apparaatdubbels om te synchroniseren tussen de cloud en apparaten. Raadpleeg [apparaatdubbels begrijpen en gebruiken in IoT Hub](iot-hub-devguide-device-twins.md) voor informatie over het gebruik van apparaatdubbels.

## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met behulp van tags

Voordat u een configuratie maken kunt, moet u opgeven welke apparaten die u wilt toepassen. Azure IoT Hub identificeert-apparaten met behulp van tags in de apparaatdubbel. Elk apparaat kan meerdere labels, en u ze een manier die zinvol is voor uw oplossing kunt definiëren. Als u apparaten in verschillende locaties beheert, kan u bijvoorbeeld de volgende codes toevoegen aan een apparaatdubbel:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="create-a-configuration"></a>Een configuratie maken

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub. 

2. Selecteer **IoT apparaatconfiguratie**.

3. Selecteer **configuratie toevoegen**.

Er zijn vijf stappen voor het maken van een configuratie. De volgende secties helpen bij elkaar. 

### <a name="name-and-label"></a>Naam en label

1. Geef uw configuratie van een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.

2. Labels voor het bijhouden van uw configuraties toevoegen. Labels zijn **naam**, **waarde** paren, die uw configuratie wordt beschreven. Bijvoorbeeld, `HostPlatform, Linux` of `Version, 3.0.1`.

3. Selecteer **volgende** te verplaatsen naar de volgende stap. 

### <a name="specify-settings"></a>Instellingen opgeven

In deze sectie bevat de doel-inhoud moet worden ingesteld in de betreffende apparaatdubbels. Er zijn twee invoeren voor elke set instellingen. De eerste is het pad van de dubbele apparaat, dat het pad naar de JSON-sectie in de apparaatdubbel-gewenste eigenschappen die worden ingesteld.  De tweede is het JSON-inhoud moet worden ingevoegd in deze sectie. Het Apparaatdubbel pad en de inhoud bijvoorbeeld ingesteld op het volgende:

![Instellen van het Apparaatdubbel pad en de inhoud](./media/iot-hub-auto-device-config/create-configuration-full-browser.png)

U kunt ook afzonderlijke instellingen instellen door het volledige pad op te geven in het pad van de dubbele apparaat en de waarde in de inhoud met geen vierkante haken. Het pad van de dubbel apparaat bijvoorbeeld ingesteld op `properties.desired.chiller-water.temperature` en stelt u de inhoud op `66`.

Als twee of meer configuraties gericht op hetzelfde Apparaatdubbel pad, de inhoud van de configuratie van de hoogste prioriteit wordt toegepast (prioriteit is gedefinieerd in stap 4).

Als u verwijderen van een eigenschap wilt, geeft u de waarde van de eigenschap te `null`.

U kunt aanvullende instellingen toevoegen door te selecteren **Device Twin-instelling toevoegen**.

### <a name="specify-metrics-optional"></a>Geef de metrische gegevens (optioneel)

Metrische gegevens bieden samenvatting tellingen van de verschillende statussen aan die een apparaat rapporteren kan als gevolg van het toepassen van configuratie-inhoud. U kunt bijvoorbeeld een metrische waarde voor in behandeling zijnde wijzigingen, een metrische waarde voor de fouten en metrische gegevens voor wijzigingen in de geslaagde maken.

1. Voer een naam in voor **metriek naam**.

2. Voer een query voor **metriek Criteria**.  De query is gebaseerd op het apparaat gerapporteerde eigenschappen.  De metrische waarde geeft het aantal rijen dat wordt geretourneerd door de query.

Bijvoorbeeld:

```sql
SELECT deviceId FROM devices 
  WHERE properties.reported.chillerWaterSettings.status='pending'
```

U kunt een component of de configuratie is toegepast, bijvoorbeeld opnemen: 

```sql
/* Include the double brackets. */
SELECT deviceId FROM devices 
  WHERE configurations.[[yourconfigname]].status='Applied'
```

### <a name="target-devices"></a>Doelapparaten

Gebruik de eigenschap tags uit uw apparaatdubbels gericht op de specifieke apparaten die deze configuratie moeten ontvangen.  U kunt ook doelapparaten door apparaat gerapporteerde eigenschappen.

Omdat meerdere configuraties zijn op hetzelfde apparaat gericht kunnen, moet u elke configuratie geven een prioriteitsnummer. Als er een conflict optreedt, wordt de configuratie met de hoogste prioriteit WINS-server is. 

1. Voer een positief geheel getal voor de configuratie van **prioriteit**. De hoogste numerieke waarde wordt beschouwd als de hoogste prioriteit. Als twee configuraties de dezelfde prioriteitsnummer hebt, wordt het account waarmee de meeste is gemaakt onlangs wins. 

2. Voer een **voorwaarde als doel** om te bepalen welke apparaten wordt met deze configuratie worden toegepast. De voorwaarde is gebaseerd op het apparaat apparaatdubbel-tags of apparaatdubbel gerapporteerde eigenschappen en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld, `tags.environment='test'` of `properties.reported.chillerProperties.model='4000x'`. U kunt opgeven `*` om u te richten op alle apparaten.

3. Selecteer **volgende** om door te gaan naar de laatste stap.

### <a name="review-configuration"></a>Configuratie controleren

Uw configuratie-informatie, en selecteer vervolgens **indienen**.

## <a name="monitor-a-configuration"></a>Een configuratie controleren

Bekijk de details van een configuratie en controleren van de apparaten waarop deze wordt uitgevoerd, gebruikt u de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub. 

2. Selecteer **IoT apparaatconfiguratie**.

3. Inspecteer de configuratielijst. Voor elke configuratie kunt u de volgende gegevens bekijken:

   * **ID** -de naam van de configuratie.

   * **Doel van de voorwaarde** -de query die wordt gebruikt voor het definiëren van apparaten uit de doelgroep.

   * **Prioriteit** -het getal prioriteit is toegewezen aan de configuratie.

   * **Aanmaaktijd** -de timestamp van wanneer de configuratie is gemaakt. Dit tijdstempel wordt gebruikt om ties afbreken wanneer twee configuraties dezelfde prioriteit hebben. 

   * **Systeemmeetgegevens** -metrische gegevens die worden berekend door de IoT Hub en kunnen niet worden aangepast door ontwikkelaars. Gericht geeft het aantal dubbele apparaten die voldoen aan de doelvoorwaarde. Van toepassing is opgegeven, het aantal dubbele apparaten die zijn gewijzigd door de configuratie, waaronder gedeeltelijke wijzigingen in het geval dat een hogere prioriteit afzonderlijke configuratie ook wijzigingen aangebracht. 

   * **Aangepaste metrische gegevens** -metrische gegevens die zijn opgegeven door de ontwikkelaar als query's voor apparaatdubbel-gerapporteerde eigenschappen.  Maximaal vijf aangepaste metrische gegevens kunnen worden gedefinieerd per configuratie. 
   
4. Selecteer de configuratie die u wilt bewaken.  

5. Controleer de informatie over de configuratie. U kunt de tabbladen gebruiken om specifieke details over de apparaten die worden ontvangen van de configuratie weer te geven.

   * **Doel van de voorwaarde** -de apparaten die voldoen aan de doelvoorwaarde. 

   * **Metrische gegevens** -een lijst van systeemmeetgegevens en aangepaste metrische gegevens.  U kunt een lijst met apparaten die worden geteld voor elke metrische gegevens weergeven door het selecteren van de metrische gegevens in de vervolgkeuzelijst en selecteer vervolgens **apparaten weergeven**.

   * **Dubbele apparaatinstellingen** -de dubbele-instellingen voor apparaten die zijn ingesteld door de configuratie. 

   * **Configuratie van Labels** -sleutel-waardeparen gebruikt om te beschrijven van een configuratie.  Labels hebben geen invloed op de functionaliteit. 

## <a name="modify-a-configuration"></a>Een configuratie wijzigen

Wanneer u een configuratie wijzigt, worden de wijzigingen onmiddellijk gerepliceerd naar alle apparaten uit de doelgroep. 

Als u de doelvoorwaarde bijwerkt, gebeuren de volgende updates:

* Als een apparaatdubbel is niet voldoen aan de oude doelvoorwaarde, maar voldoet aan de nieuwe doelvoorwaarde en deze configuratie de hoogste prioriteit voor die apparaatdubbel is, wordt deze configuratie toegepast op het dubbele apparaat. 

* Als een apparaatdubbel is niet meer voldoet aan de doelvoorwaarde, de instellingen van de configuratie wordt verwijderd en het dubbele apparaat wordt gewijzigd door de configuratie van de volgende hoogste prioriteit. 

* Als een apparaatdubbel momenteel met deze configuratie niet meer voldoet aan de doelvoorwaarde en niet voldoet aan de doelvoorwaarde van alle andere configuraties, klikt u vervolgens de instellingen van de configuratie wordt verwijderd en geen andere wijzigingen worden aangebracht op het dubbele. 

Voor het wijzigen van een configuratie, gebruikt u de volgende stappen uit: 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub. 

2. Selecteer **IoT apparaatconfiguratie**. 

3. Selecteer de configuratie die u wilt wijzigen. 

4. Updates aanbrengen in de volgende velden: 

   * Doelvoorwaarde 
   * Labels 
   * Prioriteit 
   * Metrische gegevens

4. Selecteer **Opslaan**.

5. Volg de stappen in [bewaken van een configuratie](#monitor-a-configuration) om te bekijken van de wijzigingen worden uitgerold. 

## <a name="delete-a-configuration"></a>Een configuratie verwijderen

Wanneer u een configuratie verwijdert, worden alle dubbele apparaten op de volgende configuratie met hoogste prioriteit. Als u dubbele apparaten niet voldoen aan de doelvoorwaarde van een andere configuratie, wordt er geen andere instellingen toegepast. 

1. In de [Azure-portal](https://portal.azure.com) Ga naar uw IoT-hub. 

2. Selecteer **IoT apparaatconfiguratie**. 

3. Gebruik het selectievakje in om de configuratie die u wilt verwijderen. 

4. Selecteer **Verwijderen**.

5. Een prompt wordt u gevraagd om te bevestigen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe configureren en controleren van IoT-apparaten op schaal. Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Uw IoT-Hub apparaatidentiteiten bulksgewijs beheren](iot-hub-bulk-identity-mgmt.md)
* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)
* [Controle van bewerkingen](iot-hub-operations-monitoring.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)
* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

Om te verkennen met behulp van de IoT Hub Device Provisioning Service inschakelen zero-touch, just-in-time inrichting, Zie: 

* [Azure IoT Hub Device Provisioning Service](/azure/iot-dps)
