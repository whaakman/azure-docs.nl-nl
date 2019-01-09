---
title: Azure digitale dubbels veelvoorkomende querypatronen | Microsoft Docs
description: Meer informatie over algemene patronen van query's op de beheer-API's van de digitale dubbels van Azure.
author: dsk-2015
manager: philmea
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 1/7/2019
ms.author: dkshir
ms.openlocfilehash: 0112853bf36c6b7b594400d303234d204b2ea24a
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108291"
---
# <a name="how-to-query-azure-digital-twins-apis-for-common-tasks"></a>Query's uitvoeren Azure digitale Twins-API's voor algemene taken

Dit artikel laat querypatronen kunt u algemene scenario's voor uw exemplaar van Azure digitale dubbels uitvoeren. Hierbij wordt ervan uitgegaan dat uw digitale Twins-exemplaar al wordt uitgevoerd. U kunt een REST-client, zoals Postman gebruiken. 

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]


## <a name="queries-for-spaces-and-types"></a>Query's voor opslagruimten en typen

In deze sectie bevat voorbeeldquery's voor meer informatie over uw ingerichte spaties. Controleer geverifieerde GET HTTP-aanvragen met de voorbeeldquery's, vervang de aanduidingen door waarden van uw installatie. 

- Get-hoofdknooppunten.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?$filter=ParentSpaceId eq null
    ```

- Een ruimte krijgen met de naam en apparaten, sensoren, berekende waarden en sensorwaarden bevatten. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?name=Focus Room A1&includes=fullpath,devices,sensors,values,sensorsvalues
    ```

- Opslagruimten waarvan de bovenliggende de beschikbare ruimte-ID is ophalen en -afhankelijkheden opnemen. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?spaceId=YOUR_SPACE_ID&includes=fullpath,devices,sensors,values,sensorsvalues&traverse=Down&minLevel=1&minRelative=true&maxLevel=5&maxRelative=true
    ```

- De ruimte met de opgegeven ID, en bevatten berekend en de sensorwaarden.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?ids=YOUR_SPACE_ID&includes=Values,sensors,SensorsValues
    ```

- Get-sleutels voor een specifieke ruimte.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/propertykeys?spaceId=YOUR_SPACE_ID
    ```

- Aan de ruimten met eigenschapsleutel met de naam *AreaInSqMeters* en de waarde is 30. U kunt ook bewerkingen tekenreeks, bijvoorbeeld get met sleutel met opslagruimten `name = X contains Y`.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?propertyKey=AreaInSqMeters&propertyValue=30
    ```

- Alle namen met de naam *temperatuur* en de bijbehorende afhankelijkheden en anthologieën.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/types?names=Temperature&includes=space,ontologies,description,fullpath
    ```


## <a name="queries-for-roles-and-role-assignments"></a>Query's voor rollen en roltoewijzingen

In deze sectie bevat enkele query's voor meer informatie over functies en hun toewijzingen. 

- Alle rollen die worden ondersteund door Azure digitale dubbels ophalen.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/roles
    ```

- Alle roltoewijzingen in uw exemplaar van de digitale dubbels ophalen. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=down
    ```

- Get-roltoewijzingen voor een bepaald pad.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/A_SPATIAL_PATH
    ```

## <a name="queries-for-device-management"></a>Query's voor Apparaatbeheer

In deze sectie ziet u enkele voorbeelden van hoe u de Management API's gebruiken kunt voor specifieke informatie over uw apparaten. Alle API-aanroepen moeten worden geverifieerd GET HTTP-aanvragen.

- Ophalen van alle apparaten.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices
    ```

- Alle apparaatstatussen vinden.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/devices/statuses
    ```

- Krijg een specifiek apparaat.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID
    ```

- Alle apparaten die zijn gekoppeld aan de basis-ruimte krijgen.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?maxLevel=1
    ```

- Alle apparaten die zijn gekoppeld aan opslagruimten op het niveau van 2 tot en met 4 ophalen.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4
    ```

- Alle apparaten die rechtstreeks zijn gekoppeld aan een specifieke ruimte-ID.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID
    ```

- Alle apparaten die zijn gekoppeld aan een specifieke ruimte en de afhankelijkheden ophalen.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down
    ```

- Alle apparaten die zijn gekoppeld aan de descendanten van een spatie, met uitzondering van die ruimte krijgen.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true
    ```

- Ophalen van alle apparaten die zijn gekoppeld aan de direct onderliggende objecten van een spatie.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true
    ```

- Ophalen van alle apparaten die zijn gekoppeld aan een van de bovenliggende leden van een spatie.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Up&maxLevel=-1&maxRelative=true
    ```

- Alle apparaten die zijn gekoppeld aan de descendanten van een adresruimte die kleiner is dan of gelijk is aan 5 ophalen.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&maxLevel=5
    ```

- Alle apparaten die zijn gekoppeld aan opslagruimten die zich op hetzelfde niveau bevindt als de ruimte met ID *YOUR_SPACE_ID*.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true
    ```

- De IoT hub-verbindingsreeks ophalen voor een bepaald apparaat.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID?includes=ConnectionString
    ```

- Get-apparaat met de opgegeven hardware-ID, met inbegrip van gekoppelde sensors.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?hardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=sensors
    ```

- Ophalen van sensoren voor bepaalde gegevenstypen, in dit geval *beweging* en *temperatuur*.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/sensors?dataTypes=Motion,Temperature
    ```

## <a name="queries-for-matchers-and-user-defined-functions"></a>Query's voor vergelijkingsprogramma voor en de gebruiker gedefinieerde functies 

- Alle ingerichte vergelijkingsprogramma voor en hun-id's ophalen.

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers
    ```

- Meer informatie over een bepaalde matcher, met inbegrip van de ruimte en de gebruiker gedefinieerde functie die ermee verbonden zijn.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID?includes=description, conditions, fullpath, userdefinedfunctions, space
    ```

- Evalueer een matcher op basis van een sensor en inschakelen van logboekregistratie voor foutopsporing. Het rendement van dit bericht HTTP GET vertelt u of de matcher en de sensor deel uitmaken van het gegevenstype. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID/evaluate/YOUR_SENSOR_ID?enableLogging=true
    ```

- Haal de ID van de gebruiker gedefinieerde functies. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions
    ```

- De inhoud van een bepaald door de gebruiker gedefinieerde functie ophalen 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions/YOUR_USER_DEFINED_FUNCTION_ID/contents
    ```


## <a name="queries-for-users"></a>Query's voor gebruikers

In deze sectie ziet u enkele voorbeeldquery API's voor het beheren van gebruikers in digitale dubbels van Azure. Maken van een HTTP GET-aanvraag de tijdelijke aanduidingen vervangt door waarden van uw installatie. 

- Alle gebruikers ophalen. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users
    ```

- Een specifieke gebruiker ophalen.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users/ANY_USER_ID
    ```

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over verificatie met uw API Management, [verificatie met API's](./security-authenticating-apis.md).

Alle API-eindpunten, Zie [het gebruik van digitale dubbele Swagger](./how-to-use-swagger.md).
