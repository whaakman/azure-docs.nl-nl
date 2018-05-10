---
title: Tonen informatie over een coördinaat met Azure Maps | Microsoft Docs
description: Informatie over een adres op de kaart weergeven als een gebruiker een coördinaat selecteert
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
ms.openlocfilehash: bb8644724cc872a0a8bc331e76251218492fd93d
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="get-information-from-a-coordinate"></a>Informatie ophalen uit een coördinaat

In dit artikel laat zien hoe omgekeerde adres zoeken en weergeven op een muisklik het adres van de locatie waarop is geklikt in een pop-up. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Informatie ophalen uit een coördinaat' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>informatie ophalen uit een coördinaat</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok wordt de stijl van de muisaanwijzer naar een wijzer bijgewerkt.

Het derde blok van code maakt een pop-up. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het laatste blok van code wordt toegevoegd een gebeurtenislistener voor klikken met de muis. Bij een muisklik, stuurt deze een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps Reverse adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Voor een geslaagd antwoord, verzamelt het adres voor de locatie waarop is geklikt en definieert de pop-inhoud en positie via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) functie van de pop-klasse

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die worden gebruikt in dit artikel: 
* [Omgekeerde zoeken](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [openen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Sluiten](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)
