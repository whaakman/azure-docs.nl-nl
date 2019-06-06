---
title: Het aanvragen van realtime-gegevens in Azure Maps | Microsoft Docs
description: Realtime gegevens met behulp van de Azure Maps Mobility-service aanvragen.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 4d8d34d8dec52dd9173cea80c39097f46d32bff5
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735479"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Realtime gegevens van aanvragen met behulp van de Mobility-Service van Azure Maps

Dit artikel laat u het gebruik van Azure Maps [Mobility-Service](https://aka.ms/AzureMapsMobilityService) realtime doorvoer gegevens van aanvragen.

In dit artikel leert u hoe u:


 * Aanvragen van de volgende realtime ontvangsten voor alle regels die binnenkomen in de opgegeven stoppen
 * Realtime informatie voor een bepaalde fiets basisstation aanvragen.


## <a name="prerequisites"></a>Vereisten

Als u wilt dat alle aanroepen naar de openbare doorvoer van Azure Maps API's, moet u een Maps-account en een sleutel. Zie voor meer informatie over het maken van een account en het ophalen van een sleutel [over het beheren van uw Azure kaarten-account en sleutels](how-to-manage-account-keys.md).

In dit artikel wordt de [Postman-app](https://www.getpostman.com/apps) REST-aanroepen te bouwen. U kunt elke API-ontwikkelomgeving die u wilt gebruiken.


## <a name="request-real-time-arrivals-for-a-stop"></a>Realtime ontvangsten van de aanvraag voor stoppen

Om een aanvraag realtime ontvangsten gegevens voor een bepaalde openbare doorvoer stoppen, moet u een aanvraag naar de [Real-time ontvangsten API](https://aka.ms/AzureMapsMobilityRealTimeArrivals) van de Azure-kaarten [Mobility-Service](https://aka.ms/AzureMapsMobilityService). U moet de **metroID** en **stopID** om de aanvraag te voltooien. Zie voor meer informatie over het aanvragen van deze parameters, onze handleiding voor instructies [aanvragen openbare doorvoer routes](https://aka.ms/AMapsHowToGuidePublicTransitRouting). 

We gebruiken '522' als onze in metro-ID, die het in metro is-ID voor "Seattle – Tacoma – Bellevue, WA" oppervlakte en gebruik de stop-ID '2060603', die is een bus stoppen op ' Ne 24th St & 162nd Ave, Ne, Bellevue WA ". Om aan te vragen de volgende vijf realtime ontvangsten gegevens voor alle volgende live ontvangsten op deze stoppen, voert u de volgende stappen uit:

1. Maak een verzameling waarin u voor het opslaan van de aanvragen. Selecteer in de Postman-app **nieuw**. In de **nieuw** venster **verzameling**. De naam van de verzameling en selecteer de **maken** knop.

2. Selecteer voor het maken van de aanvraag, **nieuw** opnieuw. In de **nieuw** venster **aanvragen**. Voer een **Aanvraagnaam** voor de aanvraag, selecteert u de verzameling die u in de vorige stap hebt gemaakt als de locatie voor de aanvraag opslaan en selecteer vervolgens **opslaan**.

    ![Maken van een aanvraag in Postman](./media/how-to-request-transit-data/postman-new.png)

3. Selecteer de GET HTTP-methode op het tabblad builder en voer de volgende URL voor het maken van een GET-aanvraag.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=2060603&transitType=bus
    ```

4. Nadat u hebt een aanvraag is gelukt ontvangt u het volgende antwoord.  U ziet de dat parameter-scheduleType' definieert of de verwachte aankomsttijd is gebaseerd op realtime of statische gegevens.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 4,
                "scheduleType": "realTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 30,
                "scheduleType": "scheduledTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }


## Real-time availability and vacancy information for bike docking station

The [Get Transit Dock Info API](https://aka.ms/AzureMapsMobilityTransitDock) of the Azure Maps Mobility Service, allows to request static and real-time information for a given bike or scooter docking station. We will make a request to get real-time data for a docking station for bikes. 

In order to make a request to the Get Transit Dock Info API, you will need the **dockId** for that station. You can get the dock ID by making a search request to the [Get Nearby Transit API](https://aka.ms/AzureMapsMobilityNearbyTransit) and setting the **objectType** parameter to "bikeDock". Follow the steps below to get real-time data of a docking station for bikes.


### Get dock ID

To get **dockID**, follow the steps below to make a request to the Get Nearby Transit API:

1. In Postman, click **New Request** | **GET request** and name it **Get dock ID**.

2.  On the Builder tab, select the **GET** HTTP method, enter the following request URL, and click **Send**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. Nadat u hebt een aanvraag is gelukt ontvangt u het volgende antwoord. U ziet dat we nu hebben de **id** in het antwoord dat kan later worden gebruikt als een queryparameter in de aanvraag voor ophalen doorvoer Dock Info API.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 30,
                    "lastUpdated": "2019-05-21T20:06:59-04:00",
                    "operatorInfo": {
                        "id": "80",
                        "name": "Citi Bike"
                    }
                },
                "position": {
                    "latitude": 40.767128,
                    "longitude": -73.962243
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 40.768039,
                        "longitude": -73.963413
                    },
                    "btmRightPoint": {
                        "latitude": 40.766216,
                        "longitude": -73.961072
                    }
                }
            }
        ]
    }
    ```


### <a name="get-real-time-bike-dock-status"></a>Status van realtime fiets dock ophalen

Volg de stappen hieronder om te vragen de ophalen doorvoer dokken Info-API om realtime gegevens voor de geselecteerde dock.

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem het **realtime dock gegevens ophalen**.

2.  Selecteer op het tabblad Builder de **ophalen** HTTP-methode, voer de volgende aanvraag-URL en klik op **verzenden**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. Nadat u hebt een aanvraag is gelukt ontvangt u een reactie van de volgende structuur:

    ```JSON
    {
        "availableVehicles": 1,
        "vacantLocations": 29,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-05-21T20:26:47-04:00",
        "operatorInfo": {
            "id": "80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Volgende stappen

Meer informatie over om aan te vragen van doorvoer gegevens met behulp van de Mobility-Service:

> [!div class="nextstepaction"]
> [Hoe u gegevens van aanvragen van doorvoer](how-to-request-transit-data.md)

De documentatie van Azure Maps Mobility Service-API verkennen:

> [!div class="nextstepaction"]
> [Mobility Service-API-documentatie](https://aka.ms/AzureMapsMobilityService)
