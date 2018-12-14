---
title: Simulatie apparaatoverzicht - Azure | Microsoft Docs
description: Een beschrijving van de oplossingsversneller voor Apparaatsimulatie en de mogelijkheden ervan.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 12/03/2018
ms.author: dobett
ms.openlocfilehash: 69e3708ef4c31e2dd91b5f50baecc46ea129cf33
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346194"
---
# <a name="device-simulation-solution-accelerator-overview"></a>Overzicht van apparaat simulatie van de oplossingsversneller

In een cloud-gebaseerde IoT-oplossing verbinden uw apparaten met een cloudeindpunt voor het verzenden van telemetriegegevens zoals temperatuur, de locatie en status. Uw oplossing verbruikt deze telemetrische gegevens, zodat u kunt het uitvoeren van acties of inzichten die zijn afgeleid van het.

Wanneer u een IoT-oplossing ontwikkelt, zijn experimenteren en daarvan te testen essentiële onderdelen van dit proces. Simulatie is een belangrijk hulpmiddel tijdens dit proces. Met Apparaatsimulatie kunt u het volgende doen:

* Snel een prototype ophalen van en worden uitgevoerd en vervolgens door aan te passen herhalen gesimuleerde gedrag van het apparaat onmiddellijk te schalen. Dit proces kunt u uit het idee bewijzen voordat u investeert in dure hardware. U kunt aangepaste apparaten via de webgebruikersinterface voor het genereren van een prototype-apparaat in een paar seconden kunt maken.
* Controleer of de oplossing naar verwachting werkt van het apparaat tot en met de oplossing door realistisch gedrag van het apparaat te simuleren. U kunt complexe apparaatgedrag JavaScript om realistische gesimuleerde telemetrie te genereren met een script.
* Schaal testen van uw oplossing door simulating normaal, piek en daarbuiten piek load voorwaarden. Schaal tests kunnen u ook naar de juiste grootte de Azure-resources die nodig zijn om uit te voeren van uw oplossing.

![Voorbeeld drone simulatie](media/iot-accelerators-device-simulation-overview/dronesimulation.png)

Met Apparaatsimulatie, kunt u modellen voor apparaten om te simuleren van uw fysieke apparaten definiëren. Dit model bevat berichtindelingen, dubbeleigenschappen en methoden. U kunt ook complexe apparaatgedrag met JavaScript simuleren.

U kunt simulaties uitvoeren voor één tot duizenden apparaten die verbinding maken met een IoT-hub. Om u te helpen bij het testen, kunt u eventueel een IoT-hub, samen met Apparaatsimulatie voor een zelfstandige omgeving implementeren.

Apparaatsimulatie is gratis. Echter Apparaatsimulatie implementeert in uw Azure-abonnement in de cloud en Azure-resources verbruiken. Als de simulatie van het apparaat niet voldoet aan uw vereisten, de [broncode is ook beschikbaar op GitHub](https://github.com/Azure/device-simulation-dotnet) voor u het kopiëren en wijzigen.

## <a name="sample-simulations"></a>Voorbeeld-simulaties

Als u Apparaatsimulatie implementeren, krijgt u een voorbeeld-simulaties en proefapparaten. Deze voorbeelden kunt u informatie over het gebruik van Apparaatsimulatie. Uitvoeren om te beginnen, een [voorbeeld simulatie die 10 vrachtwagens simuleert](quickstart-device-simulation-deploy.md). U kunt ook [maken van uw eigen simulatie met behulp van een van de vele proefapparaten opgegeven](iot-accelerators-device-simulation-create-simulation.md).

![Simulatieconfiguratie](media/iot-accelerators-device-simulation-overview/SampleSimulation.png)

## <a name="custom-simulated-devices"></a>Aangepaste gesimuleerde apparaten

Kunt u de Apparaatsimulatie naar [maken van aangepaste apparaatmodellen](iot-accelerators-device-simulation-create-custom-device.md) te gebruiken in uw simulaties. U kunt bijvoorbeeld een nieuw Apparaatmodel voor koelkast dat temperatuur en vochtigheid telemetrie verzendt definiëren. Aangepaste gesimuleerde apparaten zijn ideaal voor eenvoudige apparaatgedrag met willekeurige, verhogen of telemetriewaarden afneemt.

![Apparaatmodel maken](media/iot-accelerators-device-simulation-overview/adddevicemodel.png)

## <a name="advanced-simulated-devices"></a>Geavanceerde gesimuleerde apparaten

Als u meer controle over de telemetriewaarden die een apparaat verzendt, kunt u een geavanceerde Apparaatmodel. Geavanceerde apparaatmodellen inschakelen JavaScript-ondersteuning voor het bewerken van de verzonden telemetriewaarden. Bijvoorbeeld, u kunt de binnenkant temperatuur van een geparkeerde auto kan simuleren op een ' hot ' zonnige dag - als de buitenste temperatuurstijging, de binnenkant temperatuur exponentieel toeneemt.

Geavanceerde apparaatmodellen kunnen u [maken en uploaden van uw eigen apparaatmodellen](iot-accelerators-device-simulation-advanced-device.md) die bestaan uit een definitie van het apparaat JSON-bestand en bijbehorende JavaScript-bestanden.

Geavanceerde apparaatmodellen kunnen u:

* Geef de indeling van het apparaat, samen met de telemetrietypen wordt verzonden.
* Gebruik aangepaste scripts voor het genereren van telemetriewaarden die status van het apparaat gedurende een periode bijhouden.
* Gebruik aangepaste scripts om op te geven hoe het gesimuleerde apparaat reageert op methoden.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de oplossingsversnellers Apparaatsimulatie en de mogelijkheden ervan. Om te beginnen met behulp van de oplossingsversnellers, gaat u verder met de Snelstartgids:

> [!div class="nextstepaction"]
> [Implementeren en uitvoeren van een IoT-apparaat simuleren in Azure](quickstart-device-simulation-deploy.md)
