---
title: Een taalmodel in Video Indexer - Azure aanpassen
titlesuffix: Azure Media Services
description: In dit artikel biedt een overzicht van wat is een taalmodel in Video Indexer en hoe u kunt aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 516ecd8842e7b673201cc640b283c081a02d2b2f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799549"
---
# <a name="customize-a-language-model-with-video-indexer"></a>Een taalmodel met Video Indexer aanpassen

Video Indexer biedt ondersteuning voor automatische spraakherkenning dankzij de integratie met de Microsoft [Custom Speech Service](https://azure.microsoft.com/services/cognitive-services/custom-speech-service/). U kunt het taalmodel aanpassen door aanpassing van woordenlijsten tekst, namelijk de tekst uit het domein waarvan u wilt dat de engine om aan te passen voor uploaden. Nadat u uw model te trainen, nieuwe woorden die voorkomen in de aanpassing van woordenlijsten tekst wordt herkend, ervan uitgaande dat het standaard uitspraak van, en het taalmodel leert nieuwe waarschijnlijk reeksen woorden. Aangepaste taalmodellen worden ondersteund voor Engels, Spaans, Frans, Duits, Italiaans, Chinees (Vereenvoudigd), Japans, Russisch, Portugees (Brazilië), Hindi en Koreaans. 

We gaan een woord dat is zeer specifieke, zoals 'Kubernetes' (in de context van Azure Kubernetes service), als voorbeeld. Omdat het woord nieuw in Video Indexer is, deze wordt herkend als 'community's '. U moet het model voor het herkennen van deze als 'Kubernetes' te trainen. In andere gevallen de woorden bestaan, maar ze worden weergegeven in een bepaalde context wordt niet verwacht door het taalmodel. Bijvoorbeeld is 'containerservice' niet een 2-woordvolgorde op die een niet-specifieke taalmodel als een bepaalde reeks woorden herkennen zou.

U hebt de optie voor het uploaden van woorden zonder context in een lijst in een tekstbestand. Dit wordt beschouwd als gedeeltelijke aanpassing. U kunt ook bestanden van de tekst van de documentatie of de zinnen met betrekking tot uw inhoud voor betere aanpassing uploaden.

U kunt de Video Indexer-API's of de website maken en bewerken van aangepaste taalmodellen, zoals beschreven in de onderwerpen in de [Vervolgstappen](#next-steps) sectie van dit onderwerp.

## <a name="best-practices-for-custom-language-models"></a>Aanbevolen procedures voor het aangepaste taalmodellen

Video Indexer aan op basis van de waarschijnlijkheid van combinaties van word, dus voor meer het beste leert:

* Geef voldoende praktijkvoorbeelden van zinnen als ze zouden worden uitgesproken.
* Slechts één zin per regel, niet meer plaatsen. Het systeem leert anders kansen in zinnen.
* Het is goed dat één woord plaatsen als een zin om het woord op basis van anderen te verbeteren, maar het systeem leert beste uit volledige zinnen.
* Wanneer introduceert nieuwe woorden of afkortingen, indien mogelijk zo veel voorbeelden geven van het gebruik in een volledige zin zoveel context geven mogelijk tot het systeem.
* Probeer verschillende mogelijkheden voor aanpassing, en ontdek hoe deze werken voor u.
* Herhaling van de exacte dezelfde zin meerdere keren voorkomen. Deze mogen afwijking op basis van de rest van de invoer maken.
* Vermijd het gebruik van ongebruikelijke symbolen (~, # @ % &) als ze zullen krijgen verwijderd. De zinnen waarin ze worden weergegeven ophalen, ook genegeerd.
* Plaats het te groot invoer, zoals honderden of duizenden zinnen, omdat hierdoor het effect van het versterkingsbereik wordt Breng.

## <a name="next-steps"></a>Volgende stappen

[Taalmodel met behulp van API's aanpassen](customize-language-model-with-api.md)

[Taalmodel met behulp van de website aanpassen](customize-language-model-with-website.md)
