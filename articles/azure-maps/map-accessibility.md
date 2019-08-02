---
title: Een toegankelijke toepassing maken met Azure Maps | Microsoft Docs
description: Een toegankelijke toepassing bouwen met behulp van Azure Maps
services: azure-maps
author: chgennar
ms.author: chgennar
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
manager: timlt
ms.openlocfilehash: 8865027c895be09150797608e43184f1fdefb267
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638787"
---
# <a name="building-an-accessible-application"></a>Een toegankelijke toepassing bouwen

In dit artikel wordt beschreven hoe u een kaart toepassing bouwt die kan worden gebruikt door een scherm lezer.

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Een toegankelijke toepassing maken' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>een toegankelijke toepassing maken</a> met behulp<a href='https://codepen.io/azuremaps'>@azuremaps</a>van Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De kaart is gebaseerd op de toegankelijkheids functies. Gebruikers kunnen met behulp van het toetsen bord door de kaart navigeren. Als er een scherm lezer wordt uitgevoerd, wordt de gebruiker op de hoogte gebracht van wijzigingen in de status van de kaart.
Gebruikers worden bijvoorbeeld op de hoogte gebracht van de toewijzings wijzigingen wanneer de kaart wordt panned of ingezoomd. Aanvullende informatie die op de basis kaart wordt geplaatst, moet bijbehorende tekstuele informatie bevatten voor gebruikers van scherm lezers.

Het gebruik van [pop-up](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) is een manier om dit te doen. In het bovenstaande Zoek voorbeeld wordt een pop-upvenster met tekstuele informatie toegevoegd aan de kaart voor elke pincode die op de kaart wordt geplaatst. Met de methode bijvoegen [van pop-up](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) [](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach) kan de pop-up worden weer gegeven met een scherm lezer zonder dat de pop-up op de kaart visueel wordt weer gegeven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de pop-upklasse en de methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Meer code voorbeelden bekijken:

> [!div class="nextstepaction"]
> [Voorbeeld pagina voor code](https://aka.ms/AzureMapsSamples)
