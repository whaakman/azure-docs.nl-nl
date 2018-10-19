---
title: Wat is de Computer Vision-API?
titlesuffix: Azure Cognitive Services
description: De Computer Vision-API geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: overview
ms.date: 08/10/2017
ms.author: kefre
ms.openlocfilehash: e2f3a5655b2fbedf3ad80d555421599e26225196
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982079"
---
# <a name="what-is-computer-vision-api-version-10"></a>Wat is Computer Vision API versie 1.0?

> [!IMPORTANT]
> Een nieuwe versie van de Computer Vision-API is nu beschikbaar, zie:
>- [Overzicht](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Computer Vision API versie 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

De cloud-gebaseerde Computer Vision-API geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie. Door een afbeelding te uploaden of een afbeeldings-URL op te geven, kan visuele inhoud door Microsoft Computer Vision-algoritmen op verschillende manieren worden geanalyseerd op basis van invoer en gebruikersopties. Met de Computer Vision-API kunnen gebruikers afbeeldingen analyseren voor de volgende doeleinden:
* [Afbeeldingen taggen op basis van inhoud.](#Tagging)
* [Afbeeldingen categoriseren.](#Categorizing)
* [Het type en de kwaliteit van afbeeldingen identificeren.](#Identifying)
* [Menselijke gezichten detecteren en hun coördinaten retourneren. ](#Faces)
* [Domeinspecifieke inhoud herkennen.](#Domain-Specific)
* [Beschrijvingen van de inhoud genereren.](#Descriptions)
* [Optische tekenherkenning gebruiken om gedrukte tekst in afbeeldingen te identificeren.](#OCR)
* [Handgeschreven tekst herkennen.](#RecognizeText)
* [Onderscheid maken tussen kleurenschema's.](#Color)
* [Inhoud voor volwassenen markeren.](#Adult)
* [Foto's bijsnijden voor gebruik als miniaturen.](#Thumbnails)

## <a name="requirements"></a>Vereisten
* Ondersteunde invoermethoden: binaire gegevens over onbewerkte afbeeldingen in de vorm van een toepassings-/octetstream of afbeeldings-URL.
* Ondersteunde afbeeldingsindelingen: JPEG, PNG, GIF, BMP.
* Bestandsgrootte van afbeeldingen: minder dan 4 MB.
* Afmetingen van afbeeldingen: meer dan 50 x 50 pixels.

## <a name="tagging-images"></a>Afbeeldingen taggen
Computer Vision-API retourneert tags op basis van meer dan 2000 herkenbare objecten, levende wezens, landschappen en acties. Wanneer tags ambigu of niet algemeen bekend zijn, geeft de API-reactie tips om de betekenis van de tag in de context van een bekende situatie te verduidelijken. Tags zijn niet ingedeeld als taxonomie en er bestaan geen overnamehiërarchieën. Een verzameling inhoudstags vormt de basis voor een 'beschrijving van de afbeelding, weergegeven als voor mensen leesbare taal opgemaakt in volledige zinnen. Houd er rekening mee dat Engels op dit moment de enige ondersteunde taal is voor de beschrijving van afbeeldingen.

Wanneer een afbeelding is geüpload of een afbeeldings-URL is opgegeven, worden door de algoritmen van de Computer Vision-API tags gegenereerd op basis van de objecten, levende wezens en acties die in de afbeelding zijn geïdentificeerd. U kunt tagging niet alleen gebruiken voor het hoofdonderwerp, zoals een persoon op de voorgrond, maar ook voor de omgeving (binnen of buiten), meubels, gereedschap, planten, dieren, accessoires, gadgets enzovoort.

### <a name="example"></a>Voorbeeld
![House_Yard](./Images/house_yard.png) '

```json
Returned Json
{
   'tags':[
      {
         "name":"grass",
         "confidence":0.999999761581421
      },
      {
         "name":"outdoor",
         "confidence":0.999970674514771
      },
      {
         "name":"sky",
         "confidence":999289751052856
      },
      {
         "name":"building",
         "confidence":0.996463239192963
      },
      {
         "name":"house",
         "confidence":0.992798030376434
      },
      {
         "name":"lawn",
         "confidence":0.822680294513702
      },
      {
         "name":"green",
         "confidence":0.641222536563873
      },
      {
         "name":"residential",
         "confidence":0.314032256603241
      },
   ],
}
```
## <a name="categorizing-images"></a>Afbeeldingen categoriseren
Behalve tags en beschrijvingen retourneert de Computer Vision-API ook de op taxonomie gebaseerde categorieën die in eerdere versies zijn gedefinieerd. Deze categorieën zijn ingedeeld als taxonomie met bovenliggende/onderliggende overdraagbare hiërarchieën. Alle categorieën zijn in het Engels. Ze kunnen zelfstandig worden gebruikt of in combinatie met onze nieuwe modellen.

### <a name="the-86-category-concept"></a>Het concept van de 86 categorieën
Op basis van een lijst met 86 concepten die worden weergegeven in het volgende diagram, kunnen visuele kenmerken in een afbeelding worden gecategoriseerd, variërend van algemeen tot specifiek. Zie [Categorietaxonomie](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy) voor de volledige taxonomie in tekstindeling.

![Categorieën analyseren](./Images/analyze_categories.png)

Installatiekopie                                                  | Antwoord
------------------------------------------------------ | ----------------
![Vrouw op dak](./Images/woman_roof.png)                 | people
![Familiefoto](./Images/family_photo.png)             | people_crowd
![Leuke hond](./Images/cute_dog.png)                     | animal_dog
![Berglandschap](./Images/mountain_vista.png)       | outdoor_mountain
![Vision-analyse Voedsel en brood](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>Afbeeldingstypen identificeren
U kunt afbeeldingen op verschillende manieren categoriseren. Computer Vision-API kan een Booleaanse vlag instellen die aangeeft of een afbeelding zwart-wit of gekleurd is. De API kan ook een vlag instellen om aan te geven of een afbeelding een lijntekening is. De API kan ook aangeven of een afbeelding een illustratie is en de kwaliteit ervan aangeven op een schaal van 0-3.

### <a name="clip-art-type"></a>Type illustratie
Detecteert of een afbeelding een illustratie is.  

Waarde | Betekenis
----- | --------------
0     | Non-clip-art (geen illustratie)
1     | ambiguous (dubbelzinnig)
2     | normal-clip-art (normale illustratie)
3     | good-clip-art (goede illustratie)

Installatiekopie|Antwoord
----|----
![Vision-analyse Kaas-illustratie](./Images/cheese_clipart.png)|3 goede illustratie
![Vision-analyse Huis met tuin](./Images/house_yard.png)|0 geen illustratie

### <a name="line-drawing-type"></a>Type lijntekening
Detecteert of een afbeelding een lijntekening is.

Installatiekopie|Antwoord
----|----
![Vision-analyse Leeuw-tekening](./Images/lion_drawing.png)|True
![Vision-analyse Bloem](./Images/flower.png)|False

### <a name="faces"></a>Gezichten
Detecteert menselijke gezichten in een afbeelding en genereert de gezichtscoördinaten, de rechthoek voor het gezicht, geslacht en leeftijd. Deze visuele kenmerken vormen een subset metagegevens die worden gegenereerd voor gezicht. Gebruik de Face-API om meer uitgebreide metagegevens te genereren voor gezichten (gezichtsherkenning, houdingdetectie en meer).  

Installatiekopie|Antwoord
----|----
![Vision-analyse Vrouw op dak-gezicht](./Images/woman_roof_face.png) | [ { "leeftijd": 23, "geslacht": "vrouw", "faceRectangle": { "links": 1379, "boven": 320, "breedte": 310, "hoogte": 310 } } ]
![Vision-analyse Moeder en dochter-gezicht](./Images/mom_daughter_face.png) | [ { "leeftijd": 28, "geslacht": "vrouw", "faceRectangle": { "links": 447, "boven": 195, "breedte": 162, "hoogte": 162 } }, { "leeftijd": 10, "geslacht": "man", "faceRectangle": { "links": 355, "boven": 87, "breedte": 143, "hoogte": 143 } } ]
![Vision-analyse Familiefoto-gezicht](./Images/family_photo_face.png) | [ { "leeftijd": 11, "geslacht": "man", "faceRectangle": { "links": 113, "boven": 314, "breedte": 222, "hoogte": 222 } }, { "leeftijd": 11, "geslacht": "vrouw", "faceRectangle": { "links": 1200, "boven": 632, "breedte": 215, "hoogte": 215 } }, { "leeftijd": 41, "geslacht": "man", "faceRectangle": { "links": 514, "boven": 223, "breedte": 205, "hoogte": 205 } }, { "leeftijd": 37, "geslacht": "vrouw", "faceRectangle": { "links": 1008, "boven": 277, "breedte": 201, "hoogte": 201 } } ]


## <a name="domain-specific-content"></a>Domeinspecifieke inhoud detecteren

Computer Vision-API biedt, naast tagging en categorisatie op het hoogste niveau, ook ondersteuning voor gespecialiseerde (of domeinspecifieke) informatie. Gespecialiseerde informatie kan worden geïmplementeerd als zelfstandige methode of met de categorisatie op hoog niveau. Dit fungeert als een manier om de taxonomie van de 86 categorieën verder te verfijnen door toevoeging van domeinspecifieke modellen.

De enige gespecialiseerde informatie die op dit moment wordt ondersteund, is herkenning van beroemdheden en herkenning van oriëntatiepunten. Dit zijn domeinspecifieke verfijningen van de categorieën people (mensen) en people group (groep mensen), en landmarks (oriëntatiepunten) over de hele wereld.

Er zijn twee opties voor het gebruik van de domeinspecifieke modellen:

### <a name="option-one---scoped-analysis"></a>Eerste optie - Analyse met bereik
Alleen een gekozen model analyseren via een HTTP POST-aanroep. Als u deze optie gebruikt en weet welk model u wilt gebruiken, geeft u de naam van het model op en krijgt u alleen informatie die relevant is voor dit model. U kunt deze optie bijvoorbeeld gebruiken als u alleen herkenning van beroemdheden wilt. Het antwoord bevat een lijst met mogelijke overeenkomsten met beroemdheden, samen met de betrouwbaarheidsscores.

### <a name="option-two---enhanced-analysis"></a>Tweede optie - Uitgebreide analyse
Gebruik analyse om aanvullende informatie voor de categorieën van de 86-categorieëntaxonomie op te geven. Deze optie is beschikbaar voor toepassingen waarbij gebruikers een algemene afbeeldingsanalyse willen ophalen naast details uit één of meer domeinspecifieke modellen. Wanneer deze methode wordt aangeroepen, wordt eerst de classificatie van de 86-categorieëntaxonomie aangeroepen. Als een van de categorieën overeenkomt met de categorie van een bekend/overeenkomstig model, wordt een tweede ronde van classificatieaanroepen uitgevoerd. Als u bijvoorbeeld 'details = alles' of 'details' inclusief 'beroemdheden' gebruikt, roept de methode na de classificatie van 86 categorieën eerst de classificatie van beroemdheden aan. Het resultaat bevat tags die beginnen met 'people_'.

## <a name="generating-descriptions"></a>Beschrijvingen genereren 
De algoritmen van Computer Vision-API analyseren de inhoud in een afbeelding. Deze analyse vormt de basis voor een 'beschrijving‘ die wordt weergegeven als voor mensen leesbare taal opgemaakt in volledige zinnen. De beschrijving bevat een overzicht van wat er in de afbeelding is gevonden. Met de algoritmen van de Computer Vision-API worden verschillende beschrijvingen gegenereerd op basis van de objecten die zijn geïdentificeerd in de afbeelding. De beschrijvingen worden geëvalueerd en hiervoor wordt een betrouwbaarheidsscore gegenereerd. Vervolgens wordt er een lijst geretourneerd die is geordend van de hoogste naar de laagste betrouwbaarheidsscore. Een voorbeeld van een bot die gebruikmaakt van deze technologie voor het genereren van afbeeldingsbijschriften, vindt u [hier](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Voorbeeldbeschrijving genereren
![Z&W Gebouwen](./Images/bw_buildings.png) '
```json
 Returned Json

'description':{
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a large city',
         'confidence':0.607638706850331
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a photo of a large city',
         'confidence':0.577256764264197
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a city',
         'confidence':0.538493271791207
      }
   ]   
   'description':[
      "tags":{
         "outdoor",
         "city",
         "building",
         "photo",
         "large",
      }
   ]
}
```

## <a name="perceiving-color-schemes"></a>Kleurenschema’s waarnemen
De Computer Vision-algoritme extraheert kleuren uit een afbeelding. De kleuren worden in drie verschillende contexten geanalyseerd: voorgrond, achtergrond en geheel. Ze worden in 12 dominante accentkleuren gegroepeerd. Deze accentkleuren zijn zwart, blauw, bruin, grijs, groen, oranje, roze, paars, rood, groenblauw, wit en geel. Afhankelijk van de kleuren in een afbeelding kunnen ofwel gewoon zwart-wit ofwel accentkleuren worden geretourneerd in hexadecimale kleurcodes.

Installatiekopie                                                       | Voorgrond |Achtergrond| Kleuren
----------------------------------------------------------- | --------- | ------- | ------
![Berglandschap](./Images/mountain_vista.png)            | Zwart     | Zwart   | Wit
![Vision-analyse Bloem](./Images/flower.png)               | Zwart     | Wit   | Wit, zwart, groen
![Vision-analyse Treinstation](./Images/train_station.png) | Zwart     | Zwart   | Zwart

### <a name="accent-color"></a>Accentkleur
Kleur geëxtraheerd uit een afbeelding, bedoeld om de meest opvallende kleur weer te geven voor gebruikers via een combinatie van dominante kleuren en de verzadiging.

Installatiekopie                                                       | Antwoord
----------------------------------------------------------- | ----
![Berglandschap](./Images/mountain_vista.png)            | #BC6F0F
![Vision-analyse Bloem](./Images/flower.png)               | #CAA501
![Vision-analyse Treinstation](./Images/train_station.png) | #484B83


### <a name="black--white"></a>Zwart- wit
Booleaanse vlag die aangeeft of een afbeelding zwart-wit is.

Installatiekopie                                                      | Antwoord
---------------------------------------------------------- | ----
![Vision-analyse Gebouw](./Images/bw_buildings.png)      | True
![Vision-analyse Huis met tuin](./Images/house_yard.png)      | False

## <a name="flagging-adult-content"></a>Inhoud voor volwassenen markeren
De groep met inhoud van voor volwassenen en ongepaste inhoud is een van de visuele categorieën. Met deze categorie wordt inhoud voor volwassenen gedetecteerd en de weergave van afbeeldingen met seksuele inhoud beperkt. Het filter voor de detectie van inhoud voor volwassenen en ongepaste inhoud kan worden ingesteld met een glijdende schaal waarmee gebruikers hun voorkeuren kunnen aangeven.

## <a name="optical-character-recognition-ocr"></a>Optische tekenherkenning (OCR)
OCR-technologie detecteert tekstinhoud in een afbeelding en extraheert de herkende tekst naar een machinaal leesbare tekenstroom. U kunt het resultaat gebruiken voor zoekopdrachten en talloze andere doeleinden, zoals medische dossiers, beveiliging en bankieren. Taal wordt automatisch gedetecteerd. Met OCR besparen gebruikers tijd en kunnen ze eenvoudig foto’s van tekst maken in plaats van deze over te schrijven.

OCR ondersteunt 25 talen. Ondersteunde talen zijn: Arabisch, Chinees (traditioneel), Chinees (vereenvoudigd), Deens, Duits, Engels, Fins, Frans, Grieks, Hongaars, Italiaans, Japans, Koreaans, Nederlands, Noors, Pools, Portugees, Roemeens, Russisch, Servisch (Cyrillisch en Latijns) Slowaaks, Spaans, Tsjechisch, Turks en Zweeds.

Via OCR wordt zo nodig de draaiing van de herkende tekst gecorrigeerd, in graden, rond de horizontale as van de afbeelding. OCR biedt de framecoördinaten van elk woord, zoals weergegeven in onderstaande afbeelding.

![Overzicht van OCR](./Images/vision-overview-ocr.png) Vereisten voor OCR:
- De grootte van de ingevoerde afbeelding moet tussen de 40 x 40 en 3200 x 3200 pixels zijn.
- De afbeelding kan niet groter zijn dan 10 megapixels.

De ingevoerde afbeelding kan worden gedraaid met meervouden van 90 graden plus een kleine hoek van maximaal 40 graden.

De nauwkeurigheid van de tekstherkenning is afhankelijk van de kwaliteit van de afbeelding. Een onnauwkeurige aflezing kan worden veroorzaakt door de volgende situaties:
- Wazige afbeeldingen.
- Handgeschreven of cursieve tekst.
- Artistieke lettertypestijlen.
- Kleine lettergrootte.
- Complexe achtergronden, schaduwen of schittering op de tekst of vervorming van het perspectief.
- Te grote of geen hoofdletter aan het begin van woorden
- Subscript, superscript of doorgehaalde tekst.

Beperkingen: op bepaalde foto's waarin tekst het dominante element is, kunnen fout-positieven worden veroorzaakt door gedeeltelijk herkende woorden. Precisie kan op sommige foto's, met name foto's zonder tekst, sterk variëren afhankelijk van het type afbeelding.

## <a name="recognize-handwritten-text"></a>Handgeschreven tekst herkennen
Met deze technologie kunt u handgeschreven tekst detecteren en extraheren uit notities, brieven, essays, whiteboards, formulieren enzovoort. Deze technologie kan worden gebruikt voor verschillende oppervlakken en achtergronden, zoals wit papier, gele plaknotities en whiteboards.

Met handgeschreven tekstherkenning bespaart u tijd en moeite. U kunt productiever werken door afbeeldingen van tekst te maken in plaats van dat u deze moet transcriberen. U kunt hiermee notities digitaliseren. Door deze digitalisering kunt u snel en eenvoudig zoekfuncties implementeren. Bovendien verspilt u minder papier.

Vereisten voor invoer:
- Ondersteunde afbeeldingsindelingen: JPEG, PNG en BMP.
- Maximale grootte van afbeeldingsbestand: 4 MB.
- Afmetingen van de afbeelding: minimaal 40 x 40, maximaal 3200 x 3200.

Opmerking: deze technologie is nog in de preview-fase en is alleen beschikbaar voor Engelse tekst.

## <a name="generating-thumbnails"></a>Miniaturen genereren
Een miniatuur is een kleine weergave van een afbeelding op volledige grootte. Verschillende indelingen en miniatuurweergaven zijn vereist voor de gebruikerservaring (UX) op uiteenlopende apparaten, zoals telefoons, tablets en pc's. De functie voor slim bijsnijden van de Computer Vision-API helpt dit probleem op te lossen.

Wanneer een afbeelding is geüpload, wordt een miniatuur van hoge kwaliteit gegenereerd en de algoritme van de Computer Vision-API analyseert de objecten in de afbeelding. De afbeelding wordt vervolgens aangepast aan de vereisten van het 'interessegebied'. De uitvoer wordt weergegeven in een speciaal frame, zoals weergegeven in onderstaande afbeelding. De gegenereerde miniatuur kan worden weergegeven met een andere hoogte-breedteverhouding dan wordt gebruikt in de oorspronkelijke afbeelding, afhankelijk van wat de gebruiker nodig heeft.

De miniaturenalgoritme werkt als volgt:

1. Storende elementen worden verwijderd uit de afbeelding en het hoofdobject, ofwel het 'interessegebied', wordt herkend.
2. De afbeelding wordt bijgesneden op basis van het geïdentificeerde interessegebied.
3. De hoogte-breedteverhouding wordt aangepast aan de afmetingen van de beoogde miniatuur.

![Miniaturen](./Images/thumbnail-demo.png)
