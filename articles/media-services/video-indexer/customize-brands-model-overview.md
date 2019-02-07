---
title: Een model merken in Video Indexer - Azure aanpassen
titlesuffix: Azure Media Services
description: In dit artikel biedt een overzicht van wat is een model merken in Video Indexer en hoe u kunt aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/05/2018
ms.author: anzaman
ms.openlocfilehash: 59a865cf6e822ecd1918d0c04b98f0edc0355223
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55815469"
---
# <a name="customize-a-brands-model-in-video-indexer"></a>Een model merken in Video Indexer aanpassen

Video Indexer biedt ondersteuning voor detectie merk van spraak en tekst visual tijdens het indexeren en indexeren van video en audio-inhoud. De mogelijkheid voor het detecteren van merk vermeldingen van producten, services, identificeert en bedrijven voorgesteld door de Bing merken database. Bijvoorbeeld, als Microsoft wordt vermeld in een video of audio-inhoud of als deze weergegeven in de visual tekst in een video wordt, Video Indexer wordt dit gedetecteerd als een onderdeel in de inhoud. Merken zijn disambiguated van andere context met voorwaarden.

Merk-detectie is handig in tal van zakelijke scenario's zoals inhoud archief en detectie, contextuele reclame, analyse van sociale media, concurreren retail analysis en nog veel meer. Video Indexer merk detectie kunt u index merk vermeldingen in spraak- en visual tekst, met behulp van de Bing merken database evenals met aanpassingen door het bouwen van een aangepast model merken voor elke Video Indexer-account. De aangepaste merken model-functie kunt u aangeven of Video Indexer merken uit de database van Bing merken, uitsluiten van bepaalde merken detecteert wordt gedetecteerd (in wezen het maken van een zwarte lijst met merken), en omvatten merken die deel van uitmaken moeten uw model die mogelijk niet in de database van de Bing merken (in wezen het maken van een technisch overzicht van merken).

## <a name="out-of-the-box-detection-example"></a>Buiten de voorbeeld-detectie

In de [Microsoft Build 2017 dag 2](https://www.videoindexer.ai/media/ed6ede78ad/) presentatie, het merk "Microsoft Windows" wordt meerdere keren weergegeven. Soms in het transcript, soms visual tekst en nooit als verbatim. Video Indexer detecteert met hoge precisie is een term inderdaad merk op basis van de context, die betrekking hebben op meer dan 90k merken gebruiksklaar en voortdurend bijgewerkt. Video Indexer detecteert het merk van gesproken inhoud om 02:25, en vervolgens opnieuw op 02:40 uit visual tekst, die deel uitmaakt van het windows-logo.

![Overzicht van merken](./media/content-model-customization/brands-overview.png)

Praten over windows in de context van de bouw, wordt het woord 'Windows' als een onderdeel, en dezelfde voor Box, Apple, Fox, enz., op basis van geavanceerde Machine Learning-algoritmen die weet hoe dubbelzinnigheid van context niet detecteren. Merk detectie werkt voor onze ondersteunde talen. Klik hier voor [volledige Microsoft Build 2017 dag 2 inleidende video en index](http://www.videoindexer.ai/media/ed6ede78ad/).

Bekijk de volgende stappen om uw eigen merken.

## <a name="next-steps"></a>Volgende stappen

[Pas merken model met behulp van API 's](customize-brands-model-with-api.md)

[Merken model met behulp van de website aanpassen](customize-brands-model-with-website.md)
