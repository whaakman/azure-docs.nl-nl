---
title: Informatie over hoe modules logische uitvoeren op uw apparaten - Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge-modules zijn containers eenheden van de logica die kan worden geïmplementeerd en extern worden beheerd, zodat u bedrijfslogica op IoT Edge apparaten uitvoeren kunt
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 976b46a26d95b5e252b0df2383ea94b4dd280d24
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54229622"
---
# <a name="understand-azure-iot-edge-modules"></a>Informatie over Azure IoT Edge-modules

Azure IoT Edge kunt u implementeren en beheren van zakelijke logica aan de rand in de vorm van *modules*. Azure IoT Edge-modules zijn de kleinste rekeneenheid beheerd door IoT Edge en Azure-services (zoals Azure Stream Analytics) of uw eigen oplossingsspecifieke code kunnen bevatten. Om te begrijpen hoe modules zijn ontwikkeld, geïmplementeerd en beheerd, helpt het om te zien van de vier algemene onderdelen van een module:

* Een **installatiekopie module** is een pakket met de software die een module definieert.
* Een **module-exemplaar** is de specifieke rekeneenheid waarop u de installatiekopie van de module wordt uitgevoerd op een IoT Edge-apparaat. De module-exemplaar wordt gestart door de IoT Edge-runtime.
* Een **module identiteit** is een stukje informatie (met inbegrip van beveiligingsreferenties) die zijn opgeslagen in IoT-Hub die is gekoppeld aan elk exemplaar van de module.
* Een **moduledubbel** is een JSON-document dat is opgeslagen in IoT Hub, met informatie over de status voor een module-exemplaar, met inbegrip van metagegevens, configuraties en voorwaarden. 

## <a name="module-images-and-instances"></a>Module-afbeeldingen en instanties

IoT Edge module afbeeldingen bevatten toepassingen die van het beheer, beveiliging en functies voor communicatie van de IoT Edge-runtime profiteren. U kunt uw eigen installatiekopieën module ontwikkelen of exporteren van een ondersteunde Azure-service, zoals Azure Stream Analytics.
De afbeeldingen aanwezig zijn in de cloud en ze kunnen worden bijgewerkt, gewijzigd en geïmplementeerd in verschillende oplossingen. Bijvoorbeeld, bestaat een module die maakt gebruik van machine learning om te voorspellen van de uitvoer van de productielijn als een afzonderlijke installatiekopie dan een module die gebruikmaakt van computer vision voor het beheren van een drone. 

Telkens wanneer de installatiekopie van een module is geïmplementeerd op een apparaat en aan de slag door de IoT Edge-runtime, is een nieuw exemplaar van die module gemaakt. Twee apparaten in verschillende onderdelen van de hele wereld kunnen dezelfde installatiekopie van een module gebruiken. Elk apparaat moet echter een eigen instantie van de module wanneer de module wordt gestart op het apparaat. 

![Diagram - Module afbeeldingen in de cloud, module-exemplaren op apparaten](./media/iot-edge-modules/image_instance.png)

In uitvoering, modules installatiekopieën bestaan als containerinstallatiekopieën in een opslagplaats en module-exemplaren zijn containers op apparaten. 

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Module-id 's

Wanneer een nieuw exemplaar van de module wordt gemaakt door de IoT Edge-runtime, wordt het exemplaar is gekoppeld aan een overeenkomstige module-id. De identiteit van de module is opgeslagen in IoT Hub, en wordt gebruikt als bereik voor adressen en beveiliging voor alle lokale en cloud-communicatie voor die specifieke module-exemplaar.
De identiteit die is gekoppeld aan een module-exemplaar afhankelijk is van de identiteit van het apparaat op waarop het exemplaar wordt uitgevoerd en de naam die u voor die module in uw oplossing opgeeft. Bijvoorbeeld, als u aanroepen `insight` een module die gebruikmaakt van een Azure Stream Analytics, en u deze implementeren op een apparaat met de naam `Hannover01`, IoT Edge-runtime maakt u een overeenkomstige module-id met de naam `/devices/Hannover01/modules/insight`.

Duidelijk in scenario's kunt als u wilt implementeren een installatiekopie van de module meerdere keren op hetzelfde apparaat, u implementeren dezelfde installatiekopie van meerdere keren met verschillende namen.

![Diagram - Module-id's zijn uniek binnen apparaten en alle apparaten](./media/iot-edge-modules/identity.png)

## <a name="module-twins"></a>Moduledubbels

Elk exemplaar van de module heeft ook een bijbehorende moduledubbel die u gebruiken kunt voor het configureren van de module-exemplaar. Het exemplaar en het dubbele zijn gekoppeld met elkaar via de identiteit van de module. 

Een moduledubbel is een JSON-document waarin de eigenschappen van de informatie en configuratie van de module. Dit concept werkt parallel de [apparaatdubbel](../iot-hub/iot-hub-devguide-device-twins.md) concept van IoT Hub. De structuur van een moduledubbel is hetzelfde als een apparaatdubbel. De API's gebruikt om te communiceren met beide typen dubbels zijn ook hetzelfde. Het enige verschil tussen de twee is de identiteit die wordt gebruikt voor het starten van de client-SDK. 

```csharp
// Create a ModuleClient object. This ModuleClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(settings); 
await client.OpenAsync(); 
 
// Get the module twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="offline-capabilities"></a>Functionaliteiten offline

Azure IoT Edge biedt ondersteuning voor offline-bewerkingen op uw IoT Edge-apparaten. Deze mogelijkheden zijn beperkt voor nu. 

IoT Edge-modules zijn offline gedurende langere perioden, zolang de volgende vereisten wordt voldaan: 

* **Bericht time-to-live (TTL) is niet verlopen**. De standaardwaarde voor TTL bericht is twee uur, kan maar worden gewijzigd hoger of lager in de Store en doorsturen configuratie in de IoT Edge hub-instellingen. 
* **Modules hoeft te verifiëren bij de IoT Edge hub wanneer u offline bent**. Modules kunnen alleen worden geverifieerd met IoT Edge-hubs die u een actieve verbinding met een IoT-hub hebt. Modules moeten verifiëren als ze opnieuw om welke reden dan ook gestart zijn. Modules kunnen nog steeds berichten verzenden naar de IoT Edge hub nadat de SAS-token is verlopen. Wanneer verbinding wordt hervat, wordt de IoT Edge hub een nieuw token aanvragen van de module en wordt deze gevalideerd met de IoT-hub. Als dit lukt, IoT Edge hub stuurt de module die wordt opgeslagen, zelfs de berichten die zijn verzonden terwijl van de module-token is verlopen. 
* **De module die de berichten verzonden offline nog steeds functioneel is wanneer verbinding wordt hervat**. Na het opnieuw verbinding maakt met IoT Hub, moet IoT Edge hub voor het valideren van een nieuwe module-token (als de voorgaande build is verlopen) voordat de module-berichten kunnen worden doorgestuurd. Als de module niet beschikbaar is, voor een nieuw token, wordt de IoT Edge hub kan niet reageren op opgeslagen berichten van de module. 
* **De IoT Edge hub schijfruimte voor het opslaan van de berichten heeft**. Berichten worden standaard opgeslagen in de IoT Edge hub-container-bestandssysteem. Er is een configuratieoptie om op te geven van een gekoppeld volume voor het opslaan van de berichten in plaats daarvan. In beide gevallen moet er ruimte die beschikbaar is voor het opslaan van de berichten voor uitgestelde levering aan IoT-Hub.  

Aanvullende offline-mogelijkheden zijn beschikbaar in openbare preview. Zie voor meer informatie, [begrijpen uitgebreid offline-mogelijkheden voor IoT Edge-apparaten, modules en onderliggende apparaten](offline-capabilities.md).

## <a name="next-steps"></a>Volgende stappen
 - [Informatie over de vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules](module-development.md)
 - [De Azure IoT Edge-runtime en de bijbehorende architectuur begrijpen](iot-edge-runtime.md)

<!-- Images -->
[2]: ./media/iot-edge-modules/identity.png
