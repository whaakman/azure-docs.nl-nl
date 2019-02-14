---
title: Wat is de Computer Vision-API? - Computer Vision
titlesuffix: Azure Cognitive Services
description: De Computer Vision-service geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 08/22/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4322b118268ee88235190f5672585d17ea365495
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55872640"
---
# <a name="what-is-computer-vision"></a>Wat is Computer Vision?

De Computer Vision-service van Azure geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie. Voor het analyseren van een afbeelding kunt u een afbeeldingsindeling uploaden of een afbeeldings-URL opgeven. Met de algoritmen voor het verwerken van afbeeldingen kunt u de inhoud van een afbeelding op verschillende manieren analyseren, afhankelijk van de visuele kenmerken waarin u bent geïnteresseerd. Met Computer Vision kunt u bijvoorbeeld bepalen of een afbeelding inhoud voor volwassenen of ongepaste inhoud bevat of kunt u zoeken naar alle gezichten in een afbeelding.

U kunt Computer Vision in uw toepassing gebruiken met behulp van een systeemeigen SDK of door de REST-API rechtstreeks aan te roepen. Deze pagina laat grosso modo zien wat u kunt doen met Computer Vision.

## <a name="analyze-images-for-insight"></a>Analyseren van afbeeldingen voor inzicht

U kunt afbeeldingen analyseren om inzicht te krijgen in de visuele kenmerken en eigenschappen van die afbeeldingen. Alle functies in de onderstaande tabel worden geleverd door de [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API.

| Bewerking | Beschrijving |
| ------ | ----------- |
|**[Visuele kenmerken taggen](concept-tagging-images.md)**|Identificeer en tag visuele kenmerken in een afbeelding op basis van een set met duizenden herkenbare objecten, levende wezens, landschappen en acties. Als de tags dubbelzinnig of niet algemeen bekend zijn, worden via de API-reactie tips gegeven om de betekenis van de tag in de context van een bekende situatie te verduidelijken. U kunt tagging niet alleen gebruiken voor het hoofdonderwerp, zoals een persoon op de voorgrond, maar ook voor de omgeving (binnen of buiten), meubels, gereedschap, planten, dieren, accessoires, gadgets en enzovoort.|
|**[Objecten detecteren](concept-object-detection.md)**| Objectdetectie is vergelijkbaar met het gebruik van tags, maar de API retourneert de coördinaten van de omsluitende box voor elke tag die wordt toegepast. Als een afbeelding bijvoorbeeld een hond, kat en persoon bevat, zal de detectiebewerking deze objecten vermelden, samen met hun coördinaten in de afbeelding. U kunt deze functie gebruiken om verdere relaties tussen de objecten in een afbeelding te verwerken. Ook weet u daardoor wanneer er meerdere exemplaren van dezelfde tag in een afbeelding voorkomen.|
|**[Merken detecteren](concept-brand-detection.md)**|Identificeer commerciële merken in afbeeldingen of video's met behulp van een database met duizenden logo's. U kunt deze functie bijvoorbeeld gebruiken om te ontdekken welke merken het populairst zijn op sociale media of het meest voorkomen in productplaatsing in de media.|
|**[Een afbeelding categoriseren](concept-categorizing-images.md)**|Identificeer en categoriseer een volledige afbeelding met behulp van een [categorietaxonomie](Category-Taxonomy.md) met bovenliggende/onderliggende erfelijke hiërarchieën. Categorieën kunnen zelfstandig worden gebruikt of met onze nieuwe tagmodellen.<br/>Engels is momenteel de enige ondersteunde taal voor het taggen en categoriseren van afbeeldingen.|
|**[Een afbeelding beschrijven](concept-describing-images.md)**|Genereer een beschrijving van een volledige afbeelding in leesbare taal met behulp van volledige zinnen. Met de algoritmen van Computer Vision worden verschillende beschrijvingen gegenereerd op basis van de objecten die zijn geïdentificeerd in de afbeelding. De beschrijvingen worden geëvalueerd en hiervoor wordt een betrouwbaarheidsscore gegenereerd. Vervolgens wordt er een lijst geretourneerd die is geordend van de hoogste naar de laagste betrouwbaarheidsscore.|
|**[Gezichten detecteren](concept-detecting-faces.md)** |Detecteer gezichten in een afbeelding en geef informatie op over elk gedetecteerd gezicht. Met Computer Vision worden de coördinaten, de rechthoek, het geslacht en de leeftijd van elk gedetecteerd gezicht geretourneerd.<br/>Computer Vision biedt een subset van de functionaliteit die beschikbaar is in [Face](/azure/cognitive-services/face/) en u kunt de Face-service gebruiken voor gedetailleerdere analyse, zoals gezichtsherkenning en houdingsdetectie.|
|**[Afbeeldingstypen detecteren](concept-detecting-image-types.md)**|Detecteer kenmerken van een afbeelding, bijvoorbeeld of een afbeelding een lijntekening of een illustratie is.|
|**[Domeinspecifieke inhoud detecteren](concept-detecting-domain-content.md)**|Gebruik domeinmodellen om domeinspecifieke inhoud in een afbeelding te detecteren en te identificeren, zoals beroemdheden en oriëntatiepunten. Als een afbeelding bijvoorbeeld mensen bevat, kan Computer Vision een domeinmodel gebruiken voor beroemdheden die in de service zijn opgenomen om te bepalen of de personen die in de afbeelding worden gedetecteerd, overeenkomen met bekende beroemdheden.|
|**[Het kleurenschema detecteren](concept-detecting-color-schemes.md)**|Analyseer het kleurgebruik in een afbeelding. Met Computer Vision kan worden bepaald of een afbeelding zwart-wit of kleur is en voor kleurenafbeeldingen kunnen de dominante kleuren en accentkleuren worden geïdentificeerd.|
|**[Een miniatuur genereren](concept-generating-thumbnails.md)**|Analyseer de inhoud van een afbeelding om een geschikte miniatuur voor deze afbeelding te genereren. Computer Vision genereert eerst een miniatuur van hoge kwaliteit, waarna de objecten in de afbeelding worden geanalyseerd om het *interessegebied* te bepalen. Computer Vision past de afbeelding vervolgens aan de vereisten van het interessegebied aan. De gegenereerde miniatuur kan worden weergegeven met een andere hoogte-breedteverhouding dan wordt gebruikt in de oorspronkelijke afbeelding, afhankelijk van uw behoeften.|
|**[Interessegebied ophalen](concept-generating-thumbnails.md#area-of-interest)**|Analyseer de inhoud van een afbeelding om de coördinaten van het *interessegebied* te retourneren. Deze functie wordt ook gebruikt om een miniatuur te genereren, maar in plaats van de afbeelding bij te snijden retourneert Computer Vision de coördinaten van het omsluitende vak van de regio, zodat de aanroepende toepassing de oorspronkelijke afbeelding naar wens kan wijzigen.|


## <a name="extract-text-from-images"></a>Extraheren van tekst uit afbeeldingen

U kunt Computer Vision gebruiken om [tekst uit een afbeelding met OCR te extraheren](concept-extracting-text-ocr.md) in een tekenstroom die door een machine kan worden gelezen. Via OCR wordt zo nodig de draaiing van de herkende tekst gecorrigeerd, in graden rond de horizontale as van de afbeelding en worden de framecoördinaten van elk woord weergegeven. OCR ondersteunt 25 talen en detecteert automatisch de taal van de geëxtraheerde tekst.

U kunt ook [gedrukte en handgeschreven tekst](concept-recognizing-text.md) in een afbeelding herkennen. Met Computer Vision kan zowel gedrukte als handgeschreven tekst worden gedetecteerd en geëxtraheerd uit afbeeldingen van verschillende objecten met verschillende oppervlakken en achtergronden, zoals bonnetjes, posters, visitekaartjes, brieven en whiteboards. Momenteel is het herkennen van gedrukte en handgeschreven tekst in preview en is Engels de enige ondersteunde taal.  

## <a name="moderate-content-in-images"></a>Beheren van inhoud in afbeeldingen

U kunt Computer Vision gebruiken om [inhoud voor volwassenen of ongepaste inhoud](concept-detecting-adult-content.md) in een afbeelding te detecteren, waarbij u beoordeelt hoe waarschijnlijk het is dat de afbeelding inhoud voor volwassenen of ongepaste inhoud bevat en een betrouwbaarheidsscore voor beide genereert. Het filter voor de detectie van inhoud voor volwassenen en ongepaste inhoud kan worden ingesteld met een glijdende schaal om uw voorkeuren aan te geven.

## <a name="use-containers"></a>Containers gebruiken

[Gebruik Computer Vision-containers](computer-vision-how-to-install-containers.md) voor het lokaal herkennen van gedrukte en handgeschreven tekst door het installeren van een gestandaardiseerde Docker-container die zich dichter bij uw gegevens bevindt.

## <a name="image-requirements"></a>Vereisten voor installatiekopieën

Met Computer Vision kunt u afbeeldingen analyseren die voldoen aan de volgende vereisten:

- De afbeelding moet worden weergegeven in de JPEG-, PNG-, GIF- of BMP-indeling
- De afbeelding moet kleiner zijn dan 4 megabyte (MB)
- De afmetingen van de afbeelding moet groter zijn dan 50 x 50 pixels
  - Voor OCR moet de afbeelding tussen de 50 x 50 en 4200 x 4200 pixels groot zijn

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Zoals geldt voor alle Cognitive Services, dienen ontwikkelaars die de Computer Vision-service gebruiken op de hoogte te zijn van het beleid van Microsoft inzake klantgegevens. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met Computer Vision door een quickstart te volgen:

- [Snelstart: Een afbeelding analyseren](quickstarts-sdk/csharp-analyze-sdk.md)
- [Snelstart: Handgeschreven tekst extraheren](quickstarts-sdk/csharp-hand-text-sdk.md)
- [Snelstart: Een miniatuur genereren](quickstarts-sdk/csharp-thumb-sdk.md)
