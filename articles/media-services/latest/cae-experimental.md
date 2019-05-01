---
title: Een experimenteel voorinstelling voor inhoud-bewuste encoding - Azure | Microsoft Docs
description: Dit artikel worden besproken inhoud-bewuste codering in Azure Media Services
services: media-services
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/05/2019
ms.author: sethm
ms.custom: ''
ms.openlocfilehash: b0b5a74a6ca0085f945075a8896c05a724ff062c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717962"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>Experimentele voorinstelling voor encoding op inhoud-bewuste

Als u wilt voorbereiden van inhoud voor de levering van [streaming met adaptieve bitrates](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), video moet worden gecodeerd in meerdere bitsnelheden (hoog naar laag). Om ervoor te zorgen correcte vermindering van de kwaliteit, zoals de bitrate worden verlaagd en is dus de resolutie van de video. Dit resulteert in een zogenaamde codering ladder: een tabel met oplossingen en bitrates; Zie de Media Services [ingebouwde coderingsstandaarden](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset).

## <a name="overview"></a>Overzicht

Interesse in voorbij de benadering van een een-definitie-past-all-video's toegenomen na de publicatie van Netflix hun [blog](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2) in December 2015. Sindsdien zijn meerdere oplossingen voor het coderen van inhoud-bewust vrijgegeven op marketplace; Zie [in dit artikel](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx) voor een overzicht. Het idee is om te worden op de hoogte van de inhoud voor het aanpassen of de codering ladder aan de complexiteit van de afzonderlijke video af te stemmen. Elke resolutie is er een bitrate waarboven toename van de kwaliteit is niet perceptive – het coderingsprogramma werkt op deze optimale bitrate-waarde. Het volgende niveau van optimalisatie is om te selecteren van de oplossingen die op basis van de inhoud – bijvoorbeeld een video van een PowerPoint-presentatie niet profiteren van gaan hieronder 720p. Verder gaat, kan het coderingsprogramma voor het optimaliseren van de instellingen voor elk schot binnen de video worden belast. NetFlix beschreven [dergelijke een benadering](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830) in 2018.

Begin 2017 bracht Microsoft de [adaptief streamen](autogen-bitrate-ladder.md) vooraf ingestelde om het probleem van de veranderlijkheid van de kwaliteit en de resolutie van de bron-video's. Onze klanten hebben een uiteenlopende combinatie van inhoud, enkele bij 1080p, anderen bij 720p en een aantal wordt aan SD en lagere resoluties. Bovendien was niet alle broninhoud mezzanines van hoge kwaliteit van films of TV studios. De adaptieve Streaming vooraf ingestelde adressen deze problemen door ervoor te zorgen dat de bitrateladder nooit meer dan de oplossing of de gemiddelde bitrate van de invoer tussentijds.

De experimentele inhoud-bewuste vooraf ingestelde standaardcodering breidt dit mechanisme, door de integratie van aangepaste logica waarmee de encoder die proberen de optimale bitrate-waarde voor een bepaalde oplossing, maar zonder uitgebreide rekenkundige analyse. Het resultaat is dat deze nieuwe definitie produceert een uitvoer met lagere bitrate dan de voorinstelling adaptief streamen, maar met een hogere kwaliteit. Zie het volgende voorbeeld grafieken die laten zien van de vergelijking met metrische gegevens van kwaliteit zoals [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) en [VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion). De bron is gemaakt door het samenvoegen van korte clips van hoge complexiteit foto's van films en TV-programma, bedoeld voor het coderingsprogramma stress. Per definitie deze vooraf ingestelde produceert resultaten die verschillen van inhoud naar inhoud – betekent het ook dat voor sommige inhoud, er niet significante vermindering van bitrate of verbetering van de kwaliteit zijn kan.

![Tarief vervorming (RD)-curve met behulp van PSNR](media/cae-experimental/msrv1.png)

**Afbeelding 1: Tarief vervorming (RD)-curve met behulp van PSNR metrische gegevens voor hoge complexiteit bron**

![Tarief vervorming (RD)-curve met behulp van VMAF](media/cae-experimental/msrv2.png)

**Afbeelding 2: Tarief vervorming (RD)-curve met behulp van VMAF metrische gegevens voor hoge complexiteit bron**

De vooraf gedefinieerde instellingen op dit moment is afgestemd op hoge complexiteit, van hoge kwaliteit bronvideo (films, tv-programma's). Werk wordt uitgevoerd aan te passen aan inhoud voor weinig complexiteit (bijvoorbeeld PowerPoint-presentaties), evenals slechtere kwaliteit video's. Deze definitie gebruikt ook de dezelfde set oplossingen als voorinstelling voor adaptief streamen. Microsoft is bezig met methoden voor het selecteren van de minimale set oplossingen op basis van de inhoud. Als volgt worden resultaten voor een andere categorie van de bron van de inhoud, waarbij het coderingsprogramma kunnen bepalen is dat de invoer van slechte kwaliteit (veel compressie artefacten vanwege de lage bitrate) is. Houd er rekening mee dat met de experimentele vooraf ingesteld, de encoder besloten voor het produceren van slechts één uitvoer laag – met een laag genoeg bitsnelheid zodat de meeste clients zou kunnen afspelen van de gegevensstroom zonder ophoudt.

![Extern bureaublad-curve met behulp van PSNR](media/cae-experimental/msrv3.png)

**Afbeelding 3: Extern bureaublad-curve met behulp van PSNR voor lage kwaliteit invoer (bij 1080p)**

![Extern bureaublad-curve met behulp van VMAF](media/cae-experimental/msrv4.png)

**Afbeelding 4: Extern bureaublad-curve met behulp van VMAF voor lage kwaliteit invoer (bij 1080p)**

## <a name="use-the-experimental-preset"></a>De experimentele voorinstelling gebruiken

U kunt de transformaties die gebruikmaken van deze definitie als volgt maken. Als u met behulp van een zelfstudie [zoals dit](stream-files-tutorial-with-api.md), kunt u de code als volgt bijwerken:

```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new experimental preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncodingExperimental
      }
   }
};
```

> [!NOTE]
> Het voorvoegsel 'experimentele' wordt hier gebruikt om aan te geven dat de onderliggende algoritmen zijn nog in ontwikkeling. Daar kunt en worden wijzigingen gedurende een bepaalde periode de logica die wordt gebruikt voor het genereren van bitrate ladders, met het doel van een algoritme die is robuust en aangepast aan een groot aantal invoer voorwaarden geconvergeerd. Coderingstaken met behulp van deze vooraf ingestelde wordt nog steeds worden gefactureerd op basis van uitvoerminuten, en de uitvoerasset kan worden geleverd vanuit onze streaming-eindpunten in protocollen zoals DASH- en HLS.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over deze nieuwe optie voor het optimaliseren van uw video's, nodigen we u om het te proberen. Stuur ons feedback via de koppelingen aan het einde van dit artikel of betrekken ons meer rechtstreeks op <amsved@microsoft.com>.
