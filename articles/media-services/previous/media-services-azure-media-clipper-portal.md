---
title: Gebruik Azure Media openen in de Portal | Microsoft Docs
description: Illustraties met Azure Media openen vanuit de Azure Portal maken
services: media-services
keywords: clip; subclip; codering; media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 1deca68cd6a61ede7536c4d5544036a10c54209b
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="create-clips-with-azure-media-clipper-in-the-portal"></a>Illustraties maken met Azure Media openen in de portal
U kunt Azure Media openen in de portal voor illustraties van de activa in uw media services-accounts maken. Navigeer naar uw media services-account in de portal om te beginnen. Selecteer vervolgens de **Subclip** tabblad.

Op de **Subclip** tabblad, kun je je begint illustraties opstellen. In de portal wordt geladen met de openen single-bitrate MP4s en MP4s multi-bitrate live archieven die zijn gepubliceerd met een geldige streaming-locator. Niet-gepubliceerde activa zijn niet geladen.

Het openen is momenteel in de openbare preview. Voor toegang tot de openen in de Azure portal, gaat u naar dit [openbare preview pagina](https://portal.azure.com/?feature.subclipper=true).

De volgende afbeelding ziet u de landingspagina openen in uw media services-account: ![Azure Media openen in Azure-portal](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-portal.png)

## <a name="producing-clips"></a>Het opstellen van illustraties
Voor het maken van een clip slepen en neerzetten van een actief op de interface clip. Als de markering-tijden gekend zijn, kunt u ze handmatig invoeren in de interface. Anders wordt de asset afspelen of sleep de afspeelkop om te zoeken naar de gewenste markeren in en het tijdstip van de markering-out. Als een markeren of markering periode niet is opgegeven, wordt de illustratie wordt gestart vanaf het begin of het einde van de invoer asset respectievelijk blijft.

Gebruik de knoppen frame-zone voor forward/GOP-forward of frame-achterwaartse/GOP-terug wilt gaan met een frame nauwkeurigheid/GOP nauwkeurigheid. Voor paginaknipsel tegen meerdere elementen slepen en neerzetten van meerdere elementen in de illustratie-interface in het Configuratiescherm van de selectie actief. U kunt selecteren en activa in de interface voor de gewenste volgorde rangschikken. Het deelvenster van de selectie actief biedt asset duur, type en resolutie metagegevens voor elke asset. Wanneer meerdere assets samen cookievalidatie, houd rekening met de resolutie van de bron van elk bestand voor invoer. Als de bron-oplossingen verschillen, wordt de lagere resolutie invoer upscaled om te voldoen aan de resolutie van de hoogste resolutie asset. De uitvoer van de taak paginaknipsel bekijken, selecteert u de knop Voorbeeld en de clip speelt af vanaf de tijden geselecteerde is ingeschakeld.

## <a name="producing-dynamic-manifest-filters"></a>Het opstellen van dynamische manifest filters
[Dynamische manifest filters](https://azure.microsoft.com/blog/dynamic-manifest/) beschrijven een reeks regels op basis van de manifest-kenmerken en asset tijdlijn. Deze regels bepalen hoe uw streaming-eindpunt bewerkt in de uitvoer afspeellijst (manifest). Het filter kan worden gebruikt om te wijzigen welke segmenten worden gestreamd om af te spelen. De filters die wordt geproduceerd door de openen lokale filters en specifiek zijn voor de bron-asset. In tegenstelling tot gerenderde videoclips, filters zijn geen nieuwe activa en hoeven niet een codeertaak om te produceren. Ze snel kunnen worden gemaakt de [.NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-dynamic-manifest) of [REST-API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest), maar ze alleen GOP juist zijn. Activa voor streaming gecodeerde hebben doorgaans een GOP grootte van twee seconden.

Als u wilt maken van een dynamische manifest filter, gaat u naar de **activa** tabblad en selecteer de gewenste asset. Selecteer de **Subclip** knop voor het bovenste menu. Selecteer dynamisch manifest filter als de modus knipsel in de geavanceerde instellingen. Vervolgens kunt u hetzelfde proces voor het produceren van een gerenderde clip om het filter te maken. Filters kunnen alleen worden geproduceerd met één element.

De volgende afbeelding ziet u de openen in de dynamische manifest filtermodus in de Azure portal: ![Azure Media openen in de dynamische manifest filtermodus in Azure-portal](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-filter.PNG)

## <a name="submitting-clipping-jobs"></a>Knipsel taken verzenden
Wanneer u klaar bent met het samenstellen van de clip, selecteer de verzendknop van de taak voor het initiëren de bijbehorende paginaknipsel taak of dynamische manifest aanroep.

## <a name="next-steps"></a>Volgende stappen
Om aan de slag met Azure Media openen, lezen de [aan de slag](media-services-azure-media-clipper-getting-started.md) artikel voor meer informatie over het implementeren van de widget.