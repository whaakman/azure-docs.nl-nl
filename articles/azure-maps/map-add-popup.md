---
title: Een pop-upvenster met Azure Maps toevoegen | Microsoft Docs
description: Een pop-upvenster toevoegen aan java script-toewijzing
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: caf661faf00d1d32664b7958a14a8719a37ab36e
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882102"
---
# <a name="add-a-popup-to-the-map"></a>Een pop-upvenster toevoegen aan de kaart

In dit artikel wordt uitgelegd hoe u een pop-upvenster kunt toevoegen aan een punt op een kaart.

## <a name="understand-the-code"></a>De code begrijpen

<a id="addAPopup"></a>

Met de volgende code wordt een punt functie `name` met en `description` eigenschappen aan de kaart toegevoegd met behulp van een Symbol-laag. Er wordt een exemplaar van de [pop](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) -upklasse gemaakt, maar dit wordt niet weer gegeven. Muis gebeurtenissen worden toegevoegd aan de symbool-laag om te activeren en sluiten van het pop-upvenster wanneer de muis aanwijzer over en op de symbool markering wordt gehouden. Wanneer het markerings symbool wordt aangevallen, wordt `position` de eigenschap van het pop-upvenster bijgewerkt met `content` de positie van de markering en de optie wordt bijgewerkt `name` met `description` een aantal HTML-code die de eigenschappen van de punt en de plaats van de functie aanwijst. De pop-up wordt vervolgens met behulp `open` van de functie weer gegeven op de kaart.

<br/>

<iframe height='500' scrolling='no' title='Een pop-up toevoegen met Azure Maps' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>een pop-up toevoegen met Azure Maps</a> door Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="reusing-a-popup-with-multiple-points"></a>Een pop-upvenster met meerdere punten opnieuw gebruiken

Wanneer u een groot aantal punten hebt en slechts één pop-up tegelijk wilt weer geven, is het raadzaam om één pop-up te maken en deze opnieuw te gebruiken in plaats van een pop-upvenster voor elke punt functie te maken. Door de pop-up opnieuw te gebruiken, wordt het aantal DOM-elementen dat door de toepassing is gemaakt, aanzienlijk verminderd, waardoor betere prestaties kunnen worden geboden. In het volgende voor beeld worden drie punt functies gemaakt. Als u op een van deze functies klikt, wordt er een pop-upvenster weer gegeven met de inhoud voor die punt functie.

<br/>

<iframe height='500' scrolling='no' title='Opnieuw gebruiken van pop-up met meerdere pincodes' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bekijk de pen met behulp van Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a> <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>met meerdere pincodes</a> .
</iframe>

## <a name="customizing-a-popup"></a>Een pop-upvenster aanpassen

Standaard heeft de pop-up een witte achtergrond, een aanwijzer pijl aan de onderkant en een knop Sluiten in de rechter bovenhoek. In het volgende voor beeld wordt de achtergrond kleur gewijzigd in `fillColor` zwart met behulp van de optie van de pop-up. De knop Sluiten wordt verwijderd door de `shoCloseButton` optie in te stellen op ONWAAR. De HTML-inhoud van het pop-upvenster gebruikt 10 pixels van de randen van het pop-upvenster en de tekst wordt wit weer gegeven, zodat deze fraai op de zwarte achtergrond verschijnt.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Aangepaste pop-up" src="//codepen.io/azuremaps/embed/ymKgdg/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de <a href='https://codepen.io/azuremaps/pen/ymKgdg/'>aangepaste</a> pen door Azure Maps ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>op <a href='https://codepen.io'>CodePen</a>().
</iframe>

## <a name="popup-events"></a>Pop-upgebeurtenisen

Pop-ups kunnen worden geopend, gesloten en gesleept. De pop-upklasse biedt gebeurtenissen voor de Help-ontwikkel aars die op deze acties reageren. In het volgende voor beeld ziet u welke gebeurtenissen worden geactiveerd wanneer u de pop-up opent, sluit of sleept. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Pop-upgebeurtenisen" src="//codepen.io/azuremaps/embed/BXrpvB/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de <a href='https://codepen.io/azuremaps/pen/BXrpvB/'>pop-up gebeurtenissen</a> van de<a href='https://codepen.io/azuremaps'>@azuremaps</a>pen per Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

Raadpleeg de volgende fantastische artikelen voor voor beelden van volledige code:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Een HTML-markering toevoegen](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Een vorm toevoegen](./map-add-shape.md)