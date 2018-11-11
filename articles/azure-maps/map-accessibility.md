---
title: Het maken van een toepassing toegankelijk is met Azure Maps | Microsoft Docs
description: Over het bouwen van een toegankelijke toepassing met behulp van Azure Maps
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 09/17/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: ef948b4dca3d3800a81ac52f3a73beee2558d21c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247225"
---
# <a name="building-an-accessible-application"></a>Het bouwen van een toepassing toegankelijk is

In dit artikel wordt beschreven hoe u een kaart toepassing bouwt die kan worden gebruikt door een schermlezer.

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Een toepassing toegankelijk maken' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>van een toepassing toegankelijk</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De kaart bevat vooraf gedefinieerde met toegankelijkheidsfuncties. Gebruikers kunnen gaan, de kaart met het toetsenbord. Als een schermlezer wordt uitgevoerd, is de kaart, ontvangt de gebruiker van wijzigingen in de staat.
Bijvoorbeeld, worden gebruikers geïnformeerd over wijzigingen in toewijzingen, wanneer de kaart wordt gepand of op schaal. Aanvullende informatie die wordt geplaatst op de basiskaart hebt bijbehorende tekstuele informatie voor gebruikers van schermlezers.

Met behulp van [pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) volgt één manier om dit te bereiken. In het bovenstaande voorbeeld van de zoekopdracht wordt een pop-upvenster met tekstuele informatie toegevoegd aan de kaart voor elke pincode die op de kaart is geplaatst. Met behulp van de [van pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) [koppelen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach) methode kunt u het pop-upvenster kunnen worden bekeken door een schermlezer zonder dat het pop-upvenster visueel wordt weergegeven op de kaart.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de pop-klasse en de bijbehorende methoden die in dit artikel wordt gebruikt:

> [!div class="nextstepaction"]
> [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Bekijk meer voorbeelden van code in:

> [!div class="nextstepaction"]
> [Voorbeeld-codetabel](https://aka.ms/AzureMapsSamples)
