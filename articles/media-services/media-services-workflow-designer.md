---
title: Geavanceerde codering werkstromen maken met Workflow Designer | Microsoft Docs
description: Meer informatie over het maken van geavanceerde codering werkstromen met Workflow Designer.
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako;johndeu;anilmur
ms.openlocfilehash: ef277710713d342eb9aaaf119e2985e8c1d82d0c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Geavanceerde coderingswerkstromen maken met Workflow Designer
## <a name="overview"></a>Overzicht
De **Workflow Designer** is een Windows-bureaublad hulpprogramma dat wordt gebruikt voor het ontwerpen en bouwen van aangepaste werkstromen voor codering met **Media Encoder Premium werkstroom**.
Met behulp van de kracht van de workflow designer hulpprogramma kunt u ontwerpen en maken van complexe werkstromen die worden uitgevoerd in **Media Encoder Premium**.  

Werkstromen kunnen klant besluit logica opnemen en vertakking op basis van eigenschappen van het bestand van de invoerbron. U kunt werkstromen maken met overschrijfbare eigenschappen en dynamische waarden dat zelfs de meest complexe codering taken gemakkelijker herhalen en aanpassen in de cloud.

Voorbeeld van de werkstromen die u kunt maken, zijn onder andere:

* Besluit op basis van werkstromen die de bron van de inhoud voor de omzetting van controleert en alleen de gewenste uitvoer nummers coderen.  Dit is helfpul doordat de verspilde nummers die door de bron-inhoud per ongeluk upscaling zou worden gegenereerd.
* Meerdere invoerbestanden kunnen worden gebruikt ter ondersteuning van bijschriften, overlays en hechten samen inhoud. 

Dit hulpprogramma kan ook worden gebruikt te wijzigen van onze [gepubliceerd werkstromen](media-services-workflow-designer.md#existing_workflows). 

> [!NOTE]
> Als u uw exemplaar van het hulpprogramma Workflow Designer, neem contact op met mepd@microsoft.com.
> 
> 

Zodra een werkstroombestand is gemaakt, kunnen worden geüpload als een actief en vervolgens worden gebruikt voor het coderen van media-bestanden. Voor informatie over het coderen met **Media Encoder Premium werkstroom** met **.NET**, Zie [geavanceerde codering met Media Encoder Premium Workflow](media-services-encode-with-premium-workflow.md).

## <a id="existing_workflows"></a>Bestaande werkstromen wijzigen
De standaardwaarde [gepubliceerd werkstromen](media-services-workflow-designer.md#existing_workflows) met behulp van de ontwerpfunctie hulpprogramma kan worden gewijzigd. U kunt de standaardwaarde krijgen Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). De map bevat ook de beschrijving van deze bestanden.

De volgende video's laten zien hoe de ontwerpfunctie gebruiken.

### <a name="day-1--getting-started"></a>Dag 1 – aan de slag
Heeft betrekking op dag 1 video:

* Overzicht van de ontwerpfunctie
* Basic werkstromen – "Hallo wereld"
* Maken van meerdere uitvoer MP4-bestanden voor gebruik met Azure Media Services-streaming

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>Dag 2
Heeft betrekking op dag 2 video:

* Verschillende scenario's voor het bestand van bron: audio verwerken
* Werkstromen met geavanceerde logica
* Fasen van de grafiek

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>Dag 3
Heeft betrekking op dag 3 video:

* In werkstromen/blauwdrukken Scripting
* Beperkingen op met de huidige Encoder
* MET Q & A

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-3/player]
> 
> 

## <a name="next-step"></a>Volgende stap
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

Als u moet ondersteunen of vragen hebt over het maken van aangepaste werkstromen in de Workflow designer hulpprogramma, stuurt u een e-mail naar mepd@microsoft.com.

## <a name="see-also"></a>Zie ook
[Azure Premium Encoder Workflow Designer Training video 's](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

