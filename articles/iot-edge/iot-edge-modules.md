---
title: Azure IoT rand modules begrijpen | Microsoft Docs
description: Meer informatie over Azure IoT Edge-modules en hoe deze zijn geconfigureerd
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 726bbafa9e4ba35cfa4a9cbf4d89056d52fe7963
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="understand-azure-iot-edge-modules---preview"></a>Azure IoT rand modules begrijpen - voorbeeld

Azure IoT-rand kunt u implementeren en beheren van bedrijfsregels in te schakelen op de rand in de vorm van *modules*. Azure IoT Edge-modules zijn de kleinste rekeneenheid beheerd door IoT rand en kunnen Azure-services (zoals Azure Stream Analytics) of uw eigen oplossings-specifieke code bevatten. Om te begrijpen hoe modules zijn ontwikkeld, geïmplementeerd en beheerd, is het nuttig om denkt vier conceptuele onderdelen die gezamenlijk een module:

* Een **module installatiekopie** is een pakket met de software die een module definieert.
* Een **module-exemplaar** is de specifieke rekeneenheid uitvoering van de module-installatiekopie op een apparaat IoT rand. De module-exemplaar is gestart door de runtime IoT rand.
* Een **module identiteit** is een stukje informatie (inclusief beveiligingsreferenties) opgeslagen in de IoT-Hub die is gekoppeld aan elk exemplaar van de module.
* Een **module twin** is een JSON-document dat is opgeslagen in de IoT-Hub die informatie over de status voor een module-exemplaar bevat, met inbegrip van metagegevens, configuraties en voorwaarden. 

## <a name="module-images-and-instances"></a>Module-installatiekopieën en exemplaren

IoT-rand module installatiekopieën bevatten toepassingen die van het beheer, beveiliging en functies voor communicatie van de rand van de IoT-runtime gebruikmaken. U kunt uw eigen installatiekopieën module ontwikkelen of exporteren van een ondersteunde Azure-service, zoals Azure Stream Analytics.
De afbeeldingen in de cloud bestaat en ze kunnen worden bijgewerkt, gewijzigd en geïmplementeerd in verschillende oplossingen. Bijvoorbeeld, bestaat een module die u machine learning gebruikt voor het voorspellen van de uitvoer van de productie-regel als een afzonderlijke installatiekopie dan een module die gebruikmaakt van de visie van de computer waarmee een drone. 

Telkens wanneer een installatiekopie van een module is geïmplementeerd op een apparaat en gestart door de runtime IoT rand wordt een nieuw exemplaar van deze module gemaakt. Twee apparaten in verschillende onderdelen van de hele wereld gebruiken dezelfde module installatiekopie; evenwel moet elke hun eigen module-exemplaar wanneer de module wordt gestart op het apparaat. 

![Module afbeeldingen in de cloud - exemplaren van de module op apparaten][1]

In de implementatie, modules installatiekopieën beschikbaar zijn als container afbeeldingen in een opslagplaats en module exemplaren zijn containers op apparaten. Naarmate gebruiksvoorbeelden voor Azure IoT rand groeien, kunt u nieuwe typen van installatiekopieën van de module en exemplaren wordt gemaakt. Resource beperkte apparaten uitvoeren niet bijvoorbeeld containers zodat de installatiekopieën van de module die als dll-bestanden en -exemplaren die uitvoerbare bestanden bestaan mogelijk nodig. 

## <a name="module-identities"></a>Module-id 's

Wanneer een nieuw exemplaar van de module wordt gemaakt door de runtime IoT Edge, is het exemplaar gekoppeld aan een overeenkomende module-id. De identiteit van de module wordt opgeslagen in IoT-Hub en wordt gebruikt als de scope adressering en beveiliging voor alle lokale en cloud-communicatie voor die specifieke module-exemplaar.
De identiteit die is gekoppeld aan een module-exemplaar afhankelijk van de identiteit van het apparaat waarop het exemplaar wordt uitgevoerd en de naam die u die module in uw oplossing bieden. Bijvoorbeeld, als u aanroept `insight` een module die gebruikmaakt van een Azure Stream Analytics en u deze implementeren op een apparaat aangeroepen `Hannover01`, de rand van de IoT-runtime maakt een overeenkomende module-id genoemd `/devices/Hannover01/modules/insight`.

Klaarblijkelijk in scenario's kunt wanneer u een module installatiekopie implementeert meerdere keren op hetzelfde apparaat, moet u implementeren dezelfde installatiekopie van meerdere keren met verschillende namen.

![Module identiteiten zijn uniek][2]

## <a name="module-twins"></a>Module horende

Elk exemplaar van de module heeft ook een overeenkomende module-twin die u gebruiken kunt voor het configureren van de module-exemplaar. Het exemplaar en de twin zijn met elkaar verbonden door middel van de identiteit van de module. 

Een module-twin is een JSON-document met een module gegevens en configuratie-eigenschappen worden opgeslagen. Dit concept werkt parallel de [apparaat twin] [ lnk-device-twin] concept uit IoT Hub. De structuur van een module-twin is exact hetzelfde zijn als een apparaat-twin. De API's gebruikt voor interactie met beide typen horende zijn ook hetzelfde. Het enige verschil tussen de twee is de identiteit die wordt gebruikt om te instantiëren van de client-SDK. 

```
// Create a DeviceClient object. This DeviceClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
DeviceClient client = new DeviceClient.CreateFromConnectionString(moduleConnectionString, settings); 
await client.OpenAsync(); 
 
// Get the model twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="next-steps"></a>Volgende stappen
 - [Inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur][lnk-runtime]

<!-- Images -->
[1]: ./media/iot-edge-modules/image_instance.png
[2]: ./media/iot-edge-modules/identity.png

<!-- Links -->
[lnk-device-identity]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-runtime]: iot-edge-runtime.md