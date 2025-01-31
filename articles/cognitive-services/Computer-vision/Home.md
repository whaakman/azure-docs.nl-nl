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
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f92d8ce8e95962558b3bdb7b4a4d8fe70c725f46
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604497"
---
# <a name="what-is-computer-vision"></a>Wat is Computer Vision?

De Computer Vision-service van Azure geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie. Voor het analyseren van een afbeelding kunt u een afbeeldingsindeling uploaden of een afbeeldings-URL opgeven. Met de algoritmen voor het verwerken van afbeeldingen kunt u de inhoud van een afbeelding op verschillende manieren analyseren, afhankelijk van de visuele kenmerken waarin u bent geïnteresseerd. Computer Vision kan bijvoorbeeld bepalen of een installatiekopie van een volwassene of ongepaste inhoud bevat of alle van de menselijke gezichten zoeken in een afbeelding.

U kunt Computer Vision in uw toepassing gebruiken met behulp van een systeemeigen SDK of door de REST-API rechtstreeks aan te roepen. Deze pagina laat grosso modo zien wat u kunt doen met Computer Vision.

## <a name="analyze-images-for-insight"></a>Analyseren van afbeeldingen voor inzicht

U kunt afbeeldingen analyseren om inzicht te krijgen in de visuele kenmerken en eigenschappen van die afbeeldingen. Alle functies in de onderstaande tabel worden geleverd door de [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API.

| Bewerking | Description |
| ------ | ----------- |
|**[Visuele kenmerken taggen](concept-tagging-images.md)**|Identificeer en tag visuele kenmerken in een afbeelding op basis van een set met duizenden herkenbare objecten, levende wezens, landschappen en acties. Wanneer de codes zijn ambigu of niet algemeen bekend, biedt de API-reactie hints voor het verduidelijken van de context van de tag. U kunt tagging niet alleen gebruiken voor het hoofdonderwerp, zoals een persoon op de voorgrond, maar ook voor de omgeving (binnen of buiten), meubels, gereedschap, planten, dieren, accessoires, gadgets en enzovoort.|
|**[Objecten detecteren](concept-object-detection.md)**| Objectdetectie is vergelijkbaar met het gebruik van tags, maar de API retourneert de coördinaten van de omsluitende box voor elke tag die wordt toegepast. Als een afbeelding bijvoorbeeld een hond, kat en persoon bevat, zal de detectiebewerking deze objecten vermelden, samen met hun coördinaten in de afbeelding. U kunt deze functie gebruiken om verdere relaties tussen de objecten in een afbeelding te verwerken. Ook weet u daardoor wanneer er meerdere exemplaren van dezelfde tag in een afbeelding voorkomen.|
|**[Merken detecteren](concept-brand-detection.md)**|Identificeer commerciële merken in afbeeldingen of video's met behulp van een database met duizenden logo's. U kunt deze functie bijvoorbeeld gebruiken om te ontdekken welke merken het populairst zijn op sociale media of het meest voorkomen in productplaatsing in de media.|
|**[Een afbeelding categoriseren](concept-categorizing-images.md)**|Identificeer en categoriseer een volledige afbeelding met behulp van een [categorietaxonomie](Category-Taxonomy.md) met bovenliggende/onderliggende erfelijke hiërarchieën. Categorieën kunnen zelfstandig worden gebruikt of met onze nieuwe tagmodellen.<br/>Engels is momenteel de enige ondersteunde taal voor het taggen en categoriseren van afbeeldingen.|
|**[Een afbeelding beschrijven](concept-describing-images.md)**|Genereer een beschrijving van een volledige afbeelding in leesbare taal met behulp van volledige zinnen. Met de algoritmen van Computer Vision worden verschillende beschrijvingen gegenereerd op basis van de objecten die zijn geïdentificeerd in de afbeelding. De beschrijvingen worden geëvalueerd en hiervoor wordt een betrouwbaarheidsscore gegenereerd. Vervolgens wordt er een lijst geretourneerd die is geordend van de hoogste naar de laagste betrouwbaarheidsscore.|
|**[Gezichten detecteren](concept-detecting-faces.md)** |Detecteer gezichten in een afbeelding en geef informatie op over elk gedetecteerd gezicht. Met Computer Vision worden de coördinaten, de rechthoek, het geslacht en de leeftijd van elk gedetecteerd gezicht geretourneerd.<br/>Computer Vision biedt een subset van de [Face](/azure/cognitive-services/face/) service-functionaliteit. U kunt de Face-service voor meer gedetailleerde analyse, zoals gezichtsherkenning-id gebruiken en detectie met zich meebrengen.|
|**[Afbeeldingstypen detecteren](concept-detecting-image-types.md)**|Detecteer kenmerken van een afbeelding, bijvoorbeeld of een afbeelding een lijntekening of een illustratie is.|
|**[Domeinspecifieke inhoud detecteren](concept-detecting-domain-content.md)**|Gebruik domeinmodellen om domeinspecifieke inhoud in een afbeelding te detecteren en te identificeren, zoals beroemdheden en oriëntatiepunten. Bijvoorbeeld, als een installatiekopie personen bevat, kunt Computer Vision een domeinmodel voor beroemdheden gebruiken om te bepalen of de mensen die zijn gedetecteerd in de afbeelding beroemdheden zijn bekend.|
|**[Het kleurenschema detecteren](concept-detecting-color-schemes.md)**|Analyseer het kleurgebruik in een afbeelding. Met Computer Vision kan worden bepaald of een afbeelding zwart-wit of kleur is en voor kleurenafbeeldingen kunnen de dominante kleuren en accentkleuren worden geïdentificeerd.|
|**[Een miniatuur genereren](concept-generating-thumbnails.md)**|Analyseer de inhoud van een afbeelding om een geschikte miniatuur voor deze afbeelding te genereren. Computer Vision genereert eerst een miniatuur van hoge kwaliteit, waarna de objecten in de afbeelding worden geanalyseerd om het *interessegebied* te bepalen. Computer Vision past de afbeelding vervolgens aan de vereisten van het interessegebied aan. De gegenereerde miniatuur kan worden weergegeven met een andere hoogte-breedteverhouding dan wordt gebruikt in de oorspronkelijke afbeelding, afhankelijk van uw behoeften.|
|**[Interessegebied ophalen](concept-generating-thumbnails.md#area-of-interest)**|Analyseer de inhoud van een afbeelding om de coördinaten van het *interessegebied* te retourneren. Computer Vision retourneert de omsluitende box-coördinaten van de regio, in plaats van de afbeelding bijsnijden en het genereren van een miniatuur, dus de aanroepende toepassing de oorspronkelijke afbeelding naar wens kunt aanpassen.|

## <a name="extract-text-from-images"></a>Extraheren van tekst uit afbeeldingen

U kunt Computer Vision [lezen API](concept-recognizing-text.md#read-api) naar Extraheer gedrukte en handgeschreven tekst uit afbeeldingen naar een machinaal leesbare tekenstroom. De lezen-API maakt gebruik van onze nieuwste modellen en werkt met de tekst op een aantal verschillende oppervlakken en achtergronden, zoals ontvangstbevestigingen voor e-mails, posters, visitekaartjes, letters en whiteboards. Engels is momenteel de enige ondersteunde taal.

U kunt ook de [optische tekenherkenning (OCR)](concept-recognizing-text.md#ocr-optical-character-recognition-api) API om gedrukte tekst in meerdere talen te extraheren. Via OCR wordt zo nodig de draaiing van de herkende tekst gecorrigeerd en worden de framecoördinaten van elk woord weergegeven. OCR ondersteunt 25 talen en detecteert automatisch de taal van de herkende tekst.



## <a name="moderate-content-in-images"></a>Beheren van inhoud in afbeeldingen

U kunt Computer Vision gebruiken om [erotische en ongepaste inhoud te detecteren](concept-detecting-adult-content.md) in een afbeelding en voor beide een vertrouwensscore laten retourneren. U kunt het filter voor de detectie van erotische en ongepaste inhoud instellen op een schaal van de schuifregelaar om uw voorkeuren mogelijk te maken.

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
