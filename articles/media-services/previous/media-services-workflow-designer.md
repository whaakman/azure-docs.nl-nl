---
title: Geavanceerde Coderingswerkstromen maken met Workflow Designer | Microsoft Docs
description: Meer informatie over hoe u geavanceerde coderingswerkstromen maken met Workflow Designer.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako;johndeu;anilmur
ms.openlocfilehash: fa858ffbae5c1a37851ea114efd0fbc9cf010e68
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200799"
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Geavanceerde encoding-werkstromen maken met Workflow Designer
## <a name="overview"></a>Overzicht
De **Workflow Designer** is een Windows-bureaublad hulpprogramma dat wordt gebruikt voor het ontwerpen en bouwen van aangepaste werkstromen voor codering met **Media Encoder Premium Workflow**.
Met behulp van de kracht van de workflow designer-hulpprogramma, u kunt ontwerpen en maken van complexe werkstromen die wordt uitgevoerd in de **Media Encoder Premium**.  

Werkstromen kunnen klant besluit logica en vertakkingen op basis van de eigenschappen van de invoerbron-bestand. U kunt werkstromen maken met overschrijfbare eigenschappen en dynamische waarden, zelfs de meest complexe coderingstaken om eenvoudig te maken en aanpassen in de cloud.

Voorbeeld van de werkstromen die u kunt maken, zijn onder andere:

* Besluit op basis van werkstromen die de bron van de inhoud voor het omzetten van controleren en alleen de gewenste uitvoer sporen coderen.  Dit is handig door het elimineren van de ongebruikte nummers die door de bron-inhoud per ongeluk upscaling wordt gegenereerd.
* Meerdere invoerbestanden kunnen worden gebruikt voor de ondersteuning van bijschriften, overlays en hechten samen inhoud. 

Dit hulpprogramma kan ook worden gebruikt om te wijzigen van een van onze [gepubliceerd werkstromen](media-services-workflow-designer.md#existing_workflows). 

> [!NOTE]
> Als u uw exemplaar van het hulpprogramma Workflow Designer, neem contact op met mepd@microsoft.com.
> 
> 

Wanneer een werkstroombestand is gemaakt, kunnen worden geüpload als een Asset, en vervolgens worden gebruikt voor het coderen van mediabestanden. Voor meer informatie over het coderen met **Media Encoder Premium Workflow** met behulp van **.NET**, Zie [geavanceerde codering met Media Encoder Premium Workflow](media-services-encode-with-premium-workflow.md).

## <a id="existing_workflows"></a>Wijzigen van bestaande werkstromen
De standaardwaarde [gepubliceerd werkstromen](media-services-workflow-designer.md#existing_workflows) kan worden gewijzigd met behulp van de ontwerpfunctie hulpprogramma. U kunt de standaardwaarde krijgen Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). De map bevat ook de beschrijving van deze bestanden.

De volgende video's laten zien hoe u de ontwerpfunctie.

### <a name="day-1--getting-started"></a>Dag 1 – aan de slag
Dag 1 video komen aan bod:

* Designer-overzicht
* Eenvoudige werkstromen: "Hallo wereld"
* Het maken van meerdere uitvoer MP4-bestanden voor gebruik met Azure Media Services streaming

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>Dag 2
Dag 2 video komen aan bod:

* Verschillende scenario's voor het bestand van bron: verwerken van audio
* Werkstromen met geavanceerde logica
* Graph-fasen

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>Dag 3
Dag 3 video komen aan bod:

* Uitvoeren van scripts in werkstromen/blauwdrukken
* Beperkingen bij het huidige coderingsprogramma
* Q &AMP; A

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-3/player]
> 
> 

## <a name="next-step"></a>Volgende stap
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

Als u moet ondersteunen of vragen hebt over het maken van aangepaste werkstromen in de Workflow designer-hulpprogramma, stuur e-mail naar mepd@microsoft.com.

## <a name="see-also"></a>Zie ook
[Azure Premium Encoder Workflow Designer trainingsvideo 's](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

