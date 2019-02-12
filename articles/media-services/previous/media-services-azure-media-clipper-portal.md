---
title: In de Portal gebruiken voor Azure Media Clipper | Microsoft Docs
description: Maak korte clips met behulp van Azure Media Clipper vanuit de Azure-Portal
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 02/08/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 510176331a1b79a1069b42afabb63b6b326951cc
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992309"
---
# <a name="create-clips-with-azure-media-clipper-in-the-portal"></a>Maak korte clips met Azure Media Clipper in de portal  

U kunt Azure Media Clipper in de portal Maak korte clips van assets in uw media services-accounts. Als u wilt beginnen, gaat u naar uw media services-account in de portal. Selecteer vervolgens de **Subclip** tabblad.

Op de **Subclip** tabblad kunt u om te beginnen met korte clips samenstellen. In de portal laadt de Clipper MP4s single-bitrate en multi-bitrate MP4s live-archieven die zijn gepubliceerd met een geldige streaming-locator. Niet-gepubliceerde activa zijn niet geladen.

De Clipper is momenteel in openbare preview. Voor toegang tot de Clipper in Azure portal, gaat u naar dit [openbare preview-pagina](https://portal.azure.com/?feature.subclipper=true).

De volgende afbeelding ziet de startpagina Clipper in uw media services-account: ![Azure Media Clipper in Azure portal](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-portal.png)

## <a name="producing-clips"></a>Korte clips produceren
Voor het maken van een videoclip slepen en neerzetten een asset naar de clip-interface. Als de markering-tijden bekend zijn, kunt u ze handmatig invoeren in de interface. Anders wordt afspelen van de asset of sleep de afspeelkop om te zoeken naar de gewenste markeren in en het tijdstip van de markering-out. Als een tijd is ingeschakeld in- of markeren-out niet opgegeven is, wordt de clip wordt gestart vanaf het begin of het einde van het invoeractivum respectievelijk blijft.

Gebruik de knoppen frame vooruit/GOP vooruit of frame-achterwaartse/GOP-terug om te navigeren met frame nauwkeurigheid/GOP nauwkeurigheid. Voor knippen op basis van meerdere assets slepen en neerzetten van meerdere assets in de illustratie-interface in het deelvenster van de selectie asset. U kunt selecteren en assets in de interface voor de gewenste volgorde rangschikken. Het deelvenster van de selectie asset biedt assetduur, het type en de metagegevens van het probleem zou moeten voor elk actief. Wanneer meerdere assets samen samenvoegen, moet u rekening houden met de resolutie van de bron van elk invoerbestand. Als de bron-oplossingen verschillen, wordt de lagere resolutie invoer upscaled om te voldoen aan de resolutie van de hoogste resolutie asset. De uitvoer van de knippen-taak bekijken, selecteert u de knop Voorbeeld en de clip wordt afgespeeld van de tijden van de geselecteerde is ingeschakeld.

## <a name="producing-dynamic-manifest-filters"></a>Dynamisch-manifestfilters produceren
[Dynamisch-manifestfilters](https://azure.microsoft.com/blog/dynamic-manifest/) beschrijven een set regels op basis van het manifest kenmerken en de tijdlijn van de asset. Deze regels bepalen hoe de uitvoer-afspeellijst (manifest) in uw streaming-eindpunt worden bewerkt. Het filter kan worden gebruikt om te wijzigen welke segmenten worden gestreamd om te worden afgespeeld. De filters die worden geproduceerd door de Clipper lokale filters zijn en zijn specifiek voor de bron-asset. In tegenstelling tot de gerenderde clips filters zijn niet nieuwe activa en hoeven niet een coderingstaak te produceren. Ze snel kunnen worden gemaakt de [.NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-dynamic-manifest) of [REST-API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest), ze zijn echter alleen GOP nauwkeurige. Normaal gesproken hebben gecodeerd voor streaming-assets een grootte GOP van twee seconden.

Voor het maken van een dynamisch-manifestfilter, gaat u naar de **activa** tabblad en selecteer de gewenste asset. Selecteer de **Subclip** knop voor het bovenste menu. Selecteer dynamisch-manifestfilter als de knipmodus in het menu Geavanceerd. Vervolgens kunt u de dezelfde procedure voor het produceren van een videoclip weergegeven voor het maken van het filter volgen. Filters kunnen alleen worden gemaakt van een enkel actief.

De volgende afbeelding ziet u de Clipper in dynamische manifest filtermodus in Azure portal: ![Azure Media Clipper in dynamische manifest filtermodus in Azure portal](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-filter.PNG)

## <a name="submitting-clipping-jobs"></a>Kniptaken verzenden
Wanneer u klaar bent met het samenstellen van de clip, selecteert u de verzendknop van de taak voor initiëren de taak van corresponderende knippen of dynamische manifest aanroep.

## <a name="next-steps"></a>Volgende stappen
Als u wilt aan de slag met Azure Media Clipper, lees de [aan de slag](media-services-azure-media-clipper-getting-started.md) artikel voor meer informatie over het implementeren van de widget.
