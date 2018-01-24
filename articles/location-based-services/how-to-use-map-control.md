---
title: Het gebruik van het Azure locatie op basis van Services Kaartbesturingselement | Microsoft Docs
description: Informatie over het gebruik van het Kaartbesturingselement voor Services van Azure locatie-gebaseerd clientzijde Javascript-bibliotheek.
services: location-based-services
keywords: Voeg geen of trefwoorden zonder overleg met uw EXPERT in Zoekmachineoptimalisatie bewerken.
author: philmea
ms.author: philmea
ms.date: 11/22/2017
ms.topic: article
ms.service: location-based-services
manager: timlt
ms.openlocfilehash: 494a8308a5ed4ae37ed9561d051155e7433e6193
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="how-to-use-the-azure-location-based-services-map-control"></a>Het gebruik van het Azure locatie op basis van Services Kaartbesturingselement
Het Kaartbesturingselement clientzijde Javascript-bibliotheek kunt u het weergeven van maps en ingesloten Services van Azure-locatie op basis van functies in uw web- of mobiele toepassing. 

## <a name="prerequisites"></a>Vereisten
Een locatie op basis van Azure-Services-account en de sleutel. Zie voor meer informatie over het maken van een account en bij het ophalen van een sleutel [uw locatie op basis van Azure-Services-account en de sleutels beheren](how-to-manage-account-keys.md). 

## <a name="create-a-new-map-in-a-web-page-using-the-map-control-api"></a>Een nieuwe map maken in een webpagina met behulp van de kaart besturingselement-API
U kunt een kaart insluiten in een webpagina met behulp van het Kaartbesturingselement clientzijde Javascript-bibliotheek.

1. Maak een nieuw bestand en noem deze MapSearch.html.

2. Toevoegen van de Services van Azure-locatie op basis van opmaakmodel en het script bron verwijzingen naar de `<head>` element van het bestand:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Om een nieuwe kaart in uw browser renderen, Voeg een **#map** verwijzing in de `<style>` element.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Als u wilt initialiseren van het kaartbesturingselement, het definiëren van een nieuwe sectie in de hoofdtekst html en maakt u een script. Gebruik de sleutel van uw eigen Azure locatie op basis van Services in het script. 

    ```html
    <div id="map">
        <script>
            var LBSAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": LBSAccountKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Open het bestand in uw webbrowser en bekijk de gerenderde kaart.