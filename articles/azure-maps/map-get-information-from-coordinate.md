---
title: Tonen informatie over een coördinaat met Azure Maps | Microsoft Docs
description: Informatie over een adres op de kaart weergeven als een gebruiker een coördinaat selecteert
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 3caae47f7f8f5f9c917e3a59513e6cd33cdcaeae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600490"
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

Zie de volgende artikelen voor meer voorbeelden van programmacode toevoegen aan uw maps: 
* [Richtlijnen voor het weergeven van A naar B](./map-route.md)
* [Verkeer weergeven](./map-show-traffic.md)
