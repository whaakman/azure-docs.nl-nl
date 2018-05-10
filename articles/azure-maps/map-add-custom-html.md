---
title: Toevoegen van aangepaste html in Azure Maps | Microsoft Docs
description: Aangepaste html toevoegen aan een Javascript-kaart
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: codepen
ms.openlocfilehash: df50774e1bdca53034d4856f38a6133fe8e89855
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="add-custom-html-to-the-map"></a>Aangepaste HTML toevoegen aan de kaart

In dit artikel leest u hoe een aangepaste HTML zoals een bestand van de installatiekopie toevoegen aan de kaart. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='466' scrolling='no' title='Aangepaste html toevoegen aan een kaart - png' src='//codepen.io/azuremaps/embed/MVoeVw/?height=466&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>aangepaste html toevoegen aan een kaart - png</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok maakt een HTML-element van een installatiekopie.

Maakt gebruik van het laatste blok van code [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addhtml) functie van de kaart klasse de installatiekopie toevoegen aan de opgegeven positie van de kaart.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die worden gebruikt in dit artikel: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addhtml)
