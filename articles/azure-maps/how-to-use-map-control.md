---
title: Het gebruik van Azure Maps Map Control | Microsoft Docs
description: Informatie over het gebruik van de Map Control van Azure Maps-client-side Javascript-bibliotheek.
author: dsk-2015
ms.author: dkshir
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 850f9b28c112c11fd98a8abc81a1811cd26d81cc
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166018"
---
# <a name="use-the-azure-maps-map-control"></a>Azure Maps Map Control gebruiken

De Map Control-client-side Javascript-bibliotheek kunt u voor rendering van kaarten en ingesloten Azure kaarten-functionaliteit in uw web- of mobiele toepassing.

## <a name="create-a-new-map-in-a-web-page"></a>Een nieuwe map maken in een webpagina

U kunt een kaart insluiten in webpagina's met behulp van de Map Control-client-side Javascript-bibliotheek.

1. Maak een nieuw bestand en geef deze de naam **MapSearch.html**.

2. Toevoegen van de Azure-kaarten stylesheet en script bron verwijzingen naar de `<head>` element van het bestand:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
    ```

3. Voor het renderen van een nieuwe kaart in uw browser, Voeg een **#map** verwijzing in de `<style>` element:

    ```html
    <style>
        #map {
            width: 100%;
            height: 100%;
        }
    </style>
    ```

4. Voor het initialiseren van het kaartbesturingselement, definieert een nieuwe sectie in de HTML-tekst en een script maken. Gebruik de accountsleutel van uw eigen Azure-kaarten in het script. Als u nodig hebt voor het maken van een account of het vinden van uw sleutel, Zie [over het beheren van uw Azure kaarten-account en sleutels](how-to-manage-account-keys.md). De **setLanguage** methode Hiermee geeft u de taal die moet worden gebruikt voor Kaartlabels en besturingselementen. Zie voor meer informatie over ondersteunde talen [ondersteunde talen](https://docs.microsoft.com/azure/azure-maps/supported-languages).

    ```html
    <div id="map">
        <script>
            atlas.setSubscriptionKey("<_your account key_>");
            atlas.setLanguage("en");
            var map = new atlas.Map("map", {
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```

5. Open het bestand in uw webbrowser en de gerenderde kaart weergeven.

## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van een kaart met behulp van een compleet voorbeeld:

> [!div class="nextstepaction"]
> [Een kaart maken](map-create.md)

Meer informatie over voor de stijl van een kaart:

> [!div class="nextstepaction"]
> [Kies een stijl kaart](choose-map-style.md)
