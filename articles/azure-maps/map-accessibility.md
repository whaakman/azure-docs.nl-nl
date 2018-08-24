---
title: Het maken van een toepassing toegankelijk is met Azure Maps | Microsoft Docs
description: Over het bouwen van een toegankelijke toepassing met behulp van Azure Maps
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 05/18/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: 3fe0ba47e2e3529352ca8386dc7531a96a2689af
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746209"
---
# <a name="building-an-accessible-application"></a>Het bouwen van een toepassing toegankelijk is

In dit artikel wordt beschreven hoe u een kaart toepassing bouwt die kan worden gebruikt door een schermlezer.

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Een toepassing toegankelijk maken' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>van een toepassing toegankelijk</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De kaart is vooraf gemaakte met sommige toegankelijkheidsfuncties. Een gebruiker de kaart met het toetsenbord kunt navigeren en als een schermlezer wordt uitgevoerd, de kaart ontvangt de gebruiker van wijzigingen in de staat. Bijvoorbeeld, de gebruiker een melding van de kaart nieuwe breedtegraad, lengtegraad, zoomen en plaats wanneer de kaart wordt gepand of op schaal. Aanvullende informatie die wordt geplaatst op de basiskaart hebt bijbehorende tekstuele informatie voor gebruikers van schermlezers. Met behulp van [pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) volgt één manier om dit te bereiken. In het bovenstaande voorbeeld van de zoekopdracht wordt een pop-upvenster met tekstuele informatie toegevoegd aan de kaart voor elke pincode die op de kaart is geplaatst. Met behulp van de [van pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) koppelen methode kunt u het pop-upvenster kunnen worden bekeken door een schermlezer zonder dat het pop-upvenster visueel wordt weergegeven op de kaart.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de pop-klasse en de bijbehorende methoden die in dit artikel wordt gebruikt:

* [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [koppelen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach)
    * [verwijderen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#remove)
    * [openen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [Sluiten](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)

Bekijk onze [voorbeeld codetabel](http://aka.ms/AzureMapsSamples) voor meer scenario's voor toewijzing.
