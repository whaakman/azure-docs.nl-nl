---
title: Configureren van een aangepaste Apparaatmodel - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een aangepast Apparaatmodel in de oplossingsverbetering voor Apparaatsimulatie configureren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: 61a7c5314df541b4b44a286efe982f4bf93256e3
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967528"
---
# <a name="device-models"></a>Apparaatmodellen

Bij het configureren van de simulatie te gebruiken kunt u een bestaand Apparaatmodel met een vooraf gedefinieerde set van sensoren of een aangepaste Apparaatmodel maken met uw eigen keuze aan gesimuleerde sensoren. Aangepaste sensoren kunnen u ze een nauwgezet model uw echte apparaten.

## <a name="pre-configured-device-models"></a>Vooraf geconfigureerde apparaatmodellen

De oplossingsversnellers Apparaatsimulatie biedt drie vooraf geconfigureerde apparaatmodellen: Chillers liften en Trucks.

Vooraf geconfigureerde apparaatmodellen hebben meerdere sensoren met geavanceerde gedrag gedefinieerd in een JavaScript-bestand. U kunt deze problemen niet configureren in de web-UI.

De volgende tabel ziet u de configuraties voor elke vooraf geconfigureerde Apparaatmodel:

| Apparaatmodel  | Sensor           | Eenheid  |
| ------------- | ---------------- | ----- |
| Koelunit       | luchtvochtigheid         | %     |
|               | pressure         | psig  |
|               | temperatuur      | F     |
| Lift      | Floor            |       |
|               | Trillingen        | mm    |
|               | Temperatuur      | F     |
| Vrachtwagen         | Breedtegraad         |       |
|               | Lengtegraad        |       |
|               | snelheid            | mph   |
|               | cargotemperature | F     |

## <a name="custom-device-models"></a>Aangepaste apparaatmodellen

Aangepast apparaatmodellen kunnen u model sensoren die meer ze een nauwgezet model van uw eigen apparaten. Een aangepast apparaat kan maximaal 10 aangepaste sensoren hebben.

Wanneer u het modeltype aangepast apparaat selecteert, u nieuwe sensoren toevoegen door te klikken op **+ toevoegen sensor**.

[![Configureren van de sensoren](./media/iot-accelerators-device-simulation-custom-model/configuresensors-inline.png)](./media/iot-accelerators-device-simulation-custom-model/configuresensors-expanded.png#lightbox)

Aangepaste sensoren hebben de volgende eigenschappen:

| Veld     | Beschrijving |
| --------- | ----------- |
| De Sensornaam van de | Een beschrijvende naam voor de sensor zoals **temperatuur** of **snelheid**.  |
| Gedrag  | Gedrag inschakelen telemetrische gegevens om te variëren van één bericht naar de volgende echte gegevens simuleren. **Verhoging** wordt de waarde verhoogd met één in elk bericht dat wordt verzonden vanaf de minimumwaarde. Als de maximumwaarde is voldaan, klikt u vervolgens het opnieuw wordt gestart opnieuw aan de minimale waarde. **Verlagen** gedraagt zich op dezelfde manier als **verhoging** maar telt omlaag. De **willekeurige** gedrag genereert een willekeurige waarde tussen de minimumwaarde en de maximumwaarde. |
| Minimumwaarde | Het laagste getal in het geaccepteerde bereik. |
| Maximumwaarde | Het grootste aantal in het geaccepteerde bereik. |
| Eenheid      | De maateenheid voor de sensor zoals ° F of MPH. |

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u geleerd hoe u een aangepast Apparaatmodel voor een simulatie configureert. Vervolgens kunt u enkele van de andere verkennen [IoT-oplossingsversnellers](about-iot-accelerators.md).