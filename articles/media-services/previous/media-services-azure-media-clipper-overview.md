---
title: Maak korte clips met Azure Media Clipper | Microsoft Docs
description: Overzicht van Azure Media Clipper, een hulpprogramma voor het bouwen van mediaclips van assets
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 35f1f359b44af00000ccd9047673b80ca541d376
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57998205"
---
# <a name="create-clips-with-azure-media-clipper"></a>Maak korte clips met Azure Media Clipper 

Azure Media Clipper is een gratis JavaScript-bibliotheek waarmee webontwikkelaars om te voorzien van hun gebruikers een interface voor het maken van mediaclips. Dit hulpprogramma kan worden geïntegreerd in elke webpagina en bevat API's voor het laden van assets en kniptaken verzenden.

Azure Media Clipper kunt u:
- De vooraf slate Trim en na slate van live archieven 
- Video hoogtepunten van AMS live-evenementen, live-archieven of fMP4 VOD bestanden opstellen 
- Video's van meerdere bronnen samenvoegen 
- Samenvatting korte clips van uw AMS-media-items maken 
- Video's bijsnijden met frame nauwkeurigheid 
- Dynamisch-manifestfilters genereren via de bestaande live en VOD activa met een nauwkeurigheid van de groep van afbeeldingen (GOP) 
- Coderingstaken op basis van de activa in uw media services-account maken

Voor het aanvragen van nieuwe functies bieden ideeën of feedback wilt geven, verzenden naar [UserVoice voor Azure Media Services](https://aka.ms/amsvoice/). Als u hebt en specifieke problemen, vragen of zoeken naar fouten, verwijder de Media Services-team een lijn op amcinfo@microsoft.com.

De volgende afbeelding ziet u de interface Clipper: ![Azure Media Clipper](media/media-services-azure-media-clipper-overview/media-services-azure-media-clipper-interface.PNG)

## <a name="release-notes"></a>Releaseopmerkingen
Zie de volgende lijst voor het blogbericht Clipper, verschillende bekende problemen en het wijzigingenlogboek voor de nieuwste versie van de Clipper:
- [Blogbericht](https://azure.microsoft.com/blog/azure-media-clipper/)
- [Lijst met bekende problemen](https://amp.azure.net/libs/amc/latest/docs/known_issues.html)
- [Changelog](https://amp.azure.net/libs/amc/latest/docs/changelog.html)

## <a name="browser-support"></a>Browserondersteuning
Azure Media Clipper is gebouwd met behulp van moderne HTML5-technologieën en ondersteunt de volgende browsers:

- Microsoft Edge 13+
- Internet Explorer 11+
- Chrome 54+
- Safari 10+
- Firefox meer dan 50

> [!NOTE]
> Alleen HTML5 afspelen van stromen van Azure Media Services is momenteel niet ondersteund.

## <a name="language-support"></a>Taalondersteuning
De widget Clipper is beschikbaar in de volgende 18 talen:
- Chinees (vereenvoudigd)
- Chinees (traditioneel)
- Tsjechisch
- Nederlands, Vlaams
- Engels
- Frans
- Duits
- Hongaars
- Italiaans
- Japans
- Koreaans
- Pools
- Portugees (Brazilië)
- Portugees (Portugal)
- Russisch
- Spaans
- Zweeds
- Turks

## <a name="next-steps"></a>Volgende stappen
Als u wilt aan de slag met Azure Media Clipper, lees de [aan de slag](media-services-azure-media-clipper-getting-started.md) artikel voor meer informatie over het implementeren van de widget.
