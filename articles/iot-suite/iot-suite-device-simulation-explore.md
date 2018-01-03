---
title: Aan de slag met de oplossing apparaat simulatie - Azure | Microsoft Docs
description: De oplossing van de simulatie IoT Suite is een hulpprogramma dat kan worden gebruikt om te helpen bij het ontwikkelen en testen van een IoT-oplossing. De simulatie-service is een zelfstandige die kan worden gebruikt in combinatie met andere vooraf geconfigureerde oplossingen of gebruikt met uw eigen aangepaste oplossingen bieden.
services: iot device simulation
suite: iot-suite
author: troyhopwood
manager: timlt
ms.author: troyhop
ms.service: iot-suite
ms.date: 12/15/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 25b9d1c7debe0d98a87b7d0b47dea6ab6f36c8af
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="device-simulation-walkthrough"></a>Apparaat simulatie scenario

Simulatie van Azure IoT-apparaat is een hulpprogramma dat kan worden gebruikt om te helpen bij het ontwikkelen en testen van een IoT-oplossing. Apparaat simulatie is een zelfstandige aanbieding die u in combinatie met andere vooraf geconfigureerde oplossingen of met uw eigen aangepaste oplossingen gebruiken kunt.

Deze zelfstudie leert u enkele van de functies van de simulatie van het apparaat. Hier ziet u hoe het werkt en kunt u deze gebruiken voor het testen van uw eigen IoT-oplossingen.

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Een simulatie configureren
> * Starten en stoppen van een simulatie
> * Metrische gegevens weergeven telemetrie

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt voltooid, moet u een geïmplementeerd exemplaar van Azure IoT-apparaat simulatie in uw Azure-abonnement.

Als u de simulatie van het apparaat nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren Azure IoT-apparaat simulatie](iot-suite-device-simulation-explore.md) zelfstudie.

## <a name="configuring-device-simulation"></a>De simulatie apparaat configureren

U kunt configureren en uitvoeren van de simulatie apparaat volledig uit in het dashboard. Open het dashboard van de IoT-Suite [ingerichte oplossingen](https://www.azureiotsuite.com/) pagina. Klik op **starten** onder uw nieuwe apparaat simulatie-implementatie.

### <a name="target-iot-hub"></a>Doel IoT-Hub

U kunt simulatie apparaat gebruiken met een vooraf is ingericht iothub of met andere IoT-hub:

![Doel IoT-Hub](media/iot-suite-device-simulation-explore/targethub.png)

> [!NOTE]
> De optie voor het gebruik van een vooraf is ingericht IoT-Hub is alleen beschikbaar als u hebt gekozen voor het maken van een nieuwe IoT Hub wanneer u simulatie apparaat hebt geïmplementeerd. Als u een IoT-hub hebt, kunt u altijd een nieuwe van maken de [Azure-portal](https://portal.azure.com).

Als u wilt richten op een specifieke IoT-hub, bieden de **iothubowner** verbindingsreeks. U krijgt deze verbindingsreeks uit de [Azure-portal](https://portal.azure.com):

1. Klik op de pagina van de configuratie van IoT-Hub in de Azure portal **gedeeld toegangsbeleid**.
1. Klik op **iothubowner**.
1. Kopieer de primaire of secundaire sleutel.
1. Deze waarde in het tekstvak onder doel IoT Hub plakken.

![Doel IoT-hub](media/iot-suite-device-simulation-explore/connectionstring.png)

### <a name="device-model"></a>Apparaatmodel

Het model kunt u kiezen van het type apparaat om te simuleren. U kunt een van de vooraf geconfigureerde apparaatmodellen kiezen of een lijst met sensoren voor een aangepast Apparaatmodel definiëren:

![Apparaatmodel](media/iot-suite-device-simulation-explore/devicemodel.png)

#### <a name="pre-configured-device-models"></a>Vooraf geconfigureerde apparaatmodellen

Apparaat simulatie biedt drie modellen van de vooraf geconfigureerde apparaten. Apparaatmodellen voor Chillers liften en vrachtwagens zijn beschikbaar.

Vooraf geconfigureerde apparaatmodellen bevatten meerdere sensoren met een vooraf bepaald telemetrie frequentie. U kunt de frequentie telemetrie voor deze apparaten niet aanpassen.

De volgende tabel bevat een overzicht van de configuraties voor elke vooraf geconfigureerde Apparaatmodel:

| Apparaatmodel | Temperatuursensor | Eenheid | Telemetrie-frequentie
| -------------| ------ | -----| --------------------|
| Koelunit | Vochtigheid | % | 5 seconden |
| | pressure | psig | 5 seconden |
| | Temperatuur | F | 5 seconden |
| Lift | Floor | | 5 seconden |
| | trillingen | mm | 5 seconden |
| | Temperatuur | F | 5 seconden |
| Vrachtwagen | Breedtegraad | | 3 seconden |
| | Lengtegraad | | 3 seconden |
| | Snelheid | mph | 5 seconden |
| | cargotemperature | F | 5 seconden |

#### <a name="custom-device-model"></a>Aangepaste Apparaatmodel

Aangepast apparaatmodellen kunnen u model sensoren die uw eigen apparaten beter te vertegenwoordigen. Een aangepast apparaat kan maximaal 10 aangepaste sensoren hebben.

Wanneer u het modeltype aangepast apparaat selecteert, kunt u nieuwe sensoren toevoegen door te klikken op **+ toevoegen sensor**.

![Sensoren toevoegen](media/iot-suite-device-simulation-explore/customsensors.png)

Aangepaste sensoren hebben de volgende eigenschappen:

| Veld | Beschrijving |
| ----- | ----------- |
| De Sensornaam van de | Een beschrijvende naam voor de sensor zoals **temperatuur** of **snelheid**. |
| Gedrag | Gedrag inschakelen telemetriegegevens verschillen van een bericht naar de volgende gegevens echte simuleren. **Verhoging** verhoogt u de waarde met één in elk bericht dat is verzonden vanaf de minimumwaarde. Als de maximale waarde is voldaan, vervolgens wordt gestart via opnieuw bij de minimumwaarde. **Verlagen** gedraagt zich op dezelfde manier als **verhoging** maar telt omlaag. De **willekeurige** gedrag genereert een willekeurige waarde tussen de minimumwaarde en de maximumwaarde. |
| Min-waarde | Het laagste aantal waarmee de aanvaardbare bereik. |
| Maximumwaarde | Het grootste aantal waarmee de aanvaardbare bereik. |
| Eenheid | De maateenheid voor de sensor zoals ° F of MPH. |

### <a name="number-of-devices"></a>Aantal apparaten

Apparaat simulatie kunt op dit moment u maximaal 1000 apparaten te simuleren.

![Aantal apparaten](media/iot-suite-device-simulation-explore/numberofdevices.png)

### <a name="telemetry-frequency"></a>Telemetrie-frequentie

Telemetrie frequentie kunt u opgeven hoe vaak uw gesimuleerde apparaten gegevens moeten verzenden naar de IoT-hub. U kunt gegevens verzenden als vaak als elke 10 seconden of als zelden elke als 99 uur, 59 minuten en 59 seconden.

![Telemetrie-frequentie](media/iot-suite-device-simulation-explore/frequency.png)

> [!NOTE]
> Als u een IoT-hub dan vooraf is ingericht iothub gebruikt, moet u bericht limieten overwegen voor uw doel IoT-hub. Bijvoorbeeld, als u 1000 gesimuleerde apparaten verzenden van telemetrie elke 10 seconden naar een hub S1 hebt limiet u deze telemetrie voor de hub in iets meer dan een uur.

### <a name="simulation-duration"></a>Duur van de simulatie

U kunt kiezen om uit te voeren van uw simulatie gedurende een bepaalde periode of uit te voeren totdat u het expliciet stopt. Wanneer u een specifieke tijdsduur kiest, kunt u een duur tussen 10 minuten tot maximaal 99 uren, 59 minuten en seconden 59.

![Duur van de simulatie](media/iot-suite-device-simulation-explore/duration.png)

### <a name="start-and-stop-the-simulation"></a>Starten en stoppen van de simulatie

Wanneer u de benodigde configuratiegegevens hebt toegevoegd aan het formulier de **simulatie starten** knop ingeschakeld. Klik op deze knop voor het starten van de simulatie.

![Simulatie starten](media/iot-suite-device-simulation-explore/start.png)

Als u een bepaalde duur voor de simulatie hebt opgegeven, wordt automatisch gestopt wanneer de tijd is bereikt. U kunt de simulatie altijd vroeg stoppen door te klikken op **simulatie gestopt.**

Als u wilt uw simulatie onbepaalde tijd worden uitgevoerd, wordt deze wordt uitgevoerd totdat u klikt op **stoppen simulatie**. U kunt uw browser sluiten en keert u terug naar de pagina apparaat simulatie uw simulatie op elk gewenst moment stoppen.

![Simulatie stoppen](media/iot-suite-device-simulation-explore/stop.png)

> [!NOTE]
> Slechts één simulatie kan tegelijkertijd worden uitgevoerd. Voordat u een nieuwe simulatie begint, moet u de simulatie die momenteel worden uitgevoerd stoppen.
