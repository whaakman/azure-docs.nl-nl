---
title: Het maken van een toepassing toegankelijk met Azure Maps | Microsoft Docs
description: Het bouwen van een toegankelijke toepassing met behulp van Azure Maps
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
ms.openlocfilehash: 2523287669628b8c58028cae9887743c20b6bc5e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654919"
---
# <a name="building-an-accessible-application"></a>Maken van een toepassing toegankelijk

In dit artikel leest u hoe een kaart toepassing bouwt die kan worden gebruikt door een schermlezer.

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Een toepassing toegankelijk maken' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>van een toepassing toegankelijk</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De kaart is vooraf gedefinieerde met sommige toegankelijkheidsfuncties. Een gebruiker kan de kaart met het toetsenbord navigeren en als een schermlezer wordt uitgevoerd, de kaart stelt de gebruiker van wijzigingen in de staat. Bijvoorbeeld: de gebruiker een melding van de kaart nieuwe breedtegraad, lengtegraad, zoomen en plaats wanneer de kaart wordt gepand of ingezoomd. Aanvullende informatie die wordt geplaatst op de grondtal kaart moet overeenkomende tekstinformatie voor schermlezers hebben. Met behulp van [pop](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) is één manier om dit te bereiken. In het bovenstaande voorbeeld van de zoekactie wordt een pop-upvenster met tekstinformatie toegevoegd aan de kaart voor elke pincode die op de kaart wordt geplaatst. Met behulp van de [van pop](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) koppelen kan het de pop-up kunnen worden bekeken door een schermlezer zonder dat het pop-upvenster visueel wordt weergegeven op de kaart.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de pop-klasse en de bijbehorende methoden in dit artikel gebruikt:

* [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [koppelen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#attach)
    * [verwijderen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#remove)
    * [openen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Sluiten](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Bekijk onze [code voorbeeldpagina](http://aka.ms/AzureMapsSamples) voor scenario's met meer toewijzing.
