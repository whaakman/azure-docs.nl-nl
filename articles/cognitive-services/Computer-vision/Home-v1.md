---
title: Computer Vision API voor Microsoft cognitieve Services | Microsoft Docs
description: Geavanceerde algoritmen in de Computer Vision-API gebruiken bij het verwerken van afbeeldingen en informatie geretourneerd in cognitieve Microsoft-Services.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/10/2017
ms.author: kefre
ms.openlocfilehash: 86e0441c600162e479c678d3cb1dbeaad423ddb5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345528"
---
# <a name="what-is-computer-vision-api-version-10"></a>Wat is de Computer Vision-API-versie 1.0?

> [!IMPORTANT]
> Een nieuwe versie van de Computer Vision-API is nu beschikbaar, Zie:
>- [Overzicht](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Computer Vision-API-versie 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

De cloud-gebaseerde Computer Vision-API biedt ontwikkelaars met toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en informatie te retourneren. Een afbeelding te uploaden of een afbeeldings-URL op te geven, kunnen Microsoft Computer Vision algoritmen analyseren visuele inhoud op verschillende manieren op basis van de invoer en gebruikersopties. Met de Computer Vision-API kunnen gebruikers afbeeldingen te analyseren:
* [Installatiekopieën van het label op basis van inhoud.](#Tagging)
* [Categoriseren installatiekopieën.](#Categorizing)
* [Identificeer het type en de kwaliteit van afbeeldingen.](#Identifying)
* [Menselijke vlakken detecteren en hun coördinaten retourneren. ](#Faces)
* [Domein-specifieke inhoud herkennen.](#Domain-Specific)
* [Beschrijvingen van de inhoud wordt gegenereerd.](#Descriptions)
* [Gebruik OCR afgedrukte tekst gevonden in de afbeeldingen te identificeren.](#OCR)
* [Geschreven tekst worden herkend.](#RecognizeText)
* [Kleurenschema's onderscheid worden gemaakt.](#Color)
* [Inhoud voor volwassenen vlag.](#Adult)
* [Foto's bijsnijden moet worden gebruikt als miniatuurafbeeldingen.](#Thumbnails)

## <a name="requirements"></a>Vereisten
* Ondersteund invoermethoden: onbewerkte afbeelding binaire in de vorm van een application/octet-stream of afbeelding-URL.
* Ondersteunde bestandsindelingen: JPEG-, PNG, GIF, BMP.
* Afbeeldingsgrootte van bestand: minder dan 4 MB.
* De installatiekopie van dimensie: meer dan 50 x 50 pixels.

## <a name="tagging-images"></a>Installatiekopieën tagging
Computer Vision-API retourneert labels op basis van meer dan 2000 herkenbare objecten, levende wezens achtergrond en acties. Wanneer labels zijn niet-eenduidige of niet-algemene kennis, biedt de API-reactie 'hints' om te verduidelijken de betekenis van de code in de context van een bekende instelling. Labels zijn niet ingedeeld als een taxonomie en geen overnamehiërarchieën bestaan. Een verzameling van inhoud labels vormt de basis voor de installatiekopie van een 'description', weergegeven als menselijke leesbare taal opgemaakt in volledige zinnen. Houd er rekening mee dat op dit moment Engels is de enige ondersteunde taal voor beschrijving van de installatiekopie.

Computer Vision-API's algoritmen uitvoer na een afbeelding te uploaden of een afbeeldings-URL op te geven, tags op basis van de objecten, levende wezens en acties die in de afbeelding geïdentificeerd. Labels is niet beperkt tot de belangrijkste onderwerp, zoals een persoon op de voorgrond is geplaatst, maar bevat ook de instelling (binnen of buiten), meubilair, hulpprogramma's, planten, dieren, accessoires, gadgets enzovoort.

### <a name="example"></a>Voorbeeld
![House_Yard](./Images/house_yard.jpg) '

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
## <a name="categorizing-images"></a>Installatiekopieën categoriseren
Computer Vision-API retourneert naast labels en beschrijvingen van de categorieën op basis van taxonomie is gedefinieerd in eerdere versies. Deze categorieën worden ingedeeld als een taxonomie met bovenliggende/onderliggende erfelijke hiërarchieën. Alle categorieën zijn in het Engels. Ze kunnen alleen of met onze nieuwe modellen worden gebruikt.

### <a name="the-86-category-concept"></a>Het concept 86 categorie
Op basis van een lijst met 86 concepten die worden weergegeven in het volgende diagram, kunnen visuele onderdelen gevonden in een afbeelding worden ingedeeld variërend van brede tot specifieke. Zie voor de volledige taxonomie in tekstindeling [categorie taxonomie](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Categorieën analyseren](./Images/analyze_categories.jpg)

Afbeelding                                                  | Reactie
------------------------------------------------------ | ----------------
![Vrouw dak](./Images/woman_roof.jpg)                 | personen
![Foto van uw gezin](./Images/family_photo.jpg)             | people_crowd
![Leuk aquaduct](./Images/cute_dog.jpg)                     | animal_dog
![Open Mountain](./Images/mountain_vista.jpg)       | outdoor_mountain
![Visie voeding brood analyseren](./Images/bread.jpg)       | food_bread

## <a name="identifying-image-types"></a>Afbeeldingstypen identificeren
Er zijn verschillende manieren om installatiekopieën te categoriseren. Computer Vision-API kan een Booleaanse vlag die aangeeft of een installatiekopie van een zwart of kleur is ingesteld. Het kan ook een markering om aan te geven of een installatiekopie van een regel tekening dan niet is ingesteld. Het kan ook duiden op of een installatiekopie van een illustraties of niet is en aangeven van de kwaliteit als zodanig op een schaal van 0-3.

### <a name="clip-art-type"></a>Illustraties type
Detecteert of een installatiekopie van een illustratie of niet is.  

Waarde | Betekenis
----- | --------------
0     | Niet-illustraties
1     | niet-eenduidige
2     | Normaal illustraties
3     | goede illustraties

Afbeelding|Reactie
----|----
![Visie kaas illustraties analyseren](./Images/cheese_clipart.jpg)|3 goede--illustraties
![Visie House Yard analyseren](./Images/house_yard.jpg)|0 Non--illustraties

### <a name="line-drawing-type"></a>Type lijntekening
Detecteert of een installatiekopie van een regel tekening of niet is.

Afbeelding|Reactie
----|----
![Visie analyseren Lion tekenen](./Images/lion_drawing.jpg)|Waar
![Visie bloem analyseren](./Images/flower.jpg)|False

### <a name="faces"></a>Gezichten
Menselijke vlakken binnen een afbeelding detecteert en genereert de coördinaten face, de rechthoek voor de face, geslacht en leeftijd. Deze visual functies zijn een subset van metagegevens voor face gegenereerd. Voor uitgebreidere metagegevens voor vlakken (analyseert identificatie, pose detectie en meer) gegenereerd, moet u de Face-API gebruiken.  

Afbeelding|Reactie
----|----
![Visie vrouw dak Face analyseren](./Images/woman_roof_face.png) | [{"leeftijd": 23, 'geslacht': "Vrouwelijke', 'faceRectangle": {"left": 1379 'top': 320, 'breedte': 310, 'hoogte': 310}}]
![Visie Mom dochter Face analyseren](./Images/mom_daughter_face.png) | [{"leeftijd": 28 'geslacht': 'Vrouwelijke', 'faceRectangle': {"left": 447 'top': 195, 'breedte': 162, 'hoogte': 162}}, {"leeftijd": 10 "geslacht': 'Man', 'faceRectangle': {"left": 355 'top': 87, 'breedte': 143, 'hoogte': 143}}]
![Visie familie foto Face analyseren](./Images/family_photo_face.png) | [{"leeftijd": 11 "geslacht': 'Man', 'faceRectangle': {"left": 113 'top': 314, 'breedte': 222, 'hoogte': 222}}, {'leeftijd': 11"geslacht': 'Vrouwelijke', 'faceRectangle': {"left": 1200, 'top': 632, 'breedte': 215, 'hoogte': 215}}, {'leeftijd': 41, 'geslacht': 'Man', ' faceRectangle": {"left": 514 'top': 223, 'breedte': 205, 'hoogte': 205}}, {"leeftijd": 37, 'geslacht':"Vrouwelijke', 'faceRectangle": {"left": 1008 'top': 277, 'breedte': 201, 'hoogte': 201}}]


## <a name="domain-specific-content"></a>Domein-specifieke inhoud

Labels en op het hoogste niveau categorisatie, Computer Vision-API ondersteunt bovendien ook speciale (of domeinspecifieke) informatie. Specifieke informatie kan worden geïmplementeerd als zelfstandige methode of met de classificatie op hoog niveau. Deze fungeert als een manier om de taxonomie 86 categorie door de toevoeging van domeinspecifieke modellen verder te verfijnen.

De enige speciale informatie ondersteund zijn momenteel beroemdheden herkenning en kenmerkende herkenning. Ze zijn domeinspecifieke verfijningen voor personen en personen groep categorieën en monumenten over de hele wereld.

Er zijn twee opties voor het gebruik van de domeinspecifieke modellen:

### <a name="option-one---scoped-analysis"></a>Optie één - bereik-analyse
Alleen een gekozen model analyseren door het aanroepen van een HTTP POST-aanroep. Voor deze optie als u welk model u wilt gebruiken weet, u de naam van het model en u alleen informatie ophalen relevant zijn voor dit model. Gebruik deze optie kunt u bijvoorbeeld zoekt alleen beroemdheden herkenning. Het antwoord bevat een lijst met mogelijkheden die overeenkomt met beroemdheden, vergezeld gaan van hun scores vertrouwen.

### <a name="option-two---enhanced-analysis"></a>Optie 2 - verbeterde analyse
U vindt u meer informatie met betrekking tot categorieën van de taxonomie 86 categorie analyseren. Deze optie is beschikbaar voor gebruik in toepassingen waar gebruikers wilt algemene afbeelding analysis naast gegevens ophalen uit een of meer domeinspecifieke-modellen. Wanneer deze methode is aangeroepen, wordt de classificatie 86 categorie taxonomie eerst aangeroepen. Als een van de categorieën die overeenkomen met bekende/modellen overeenkomen, volgt een tweede reeks classificatie-aanroepen. Bijvoorbeeld als ' details = all "of"gegevens"'beroemdheden' bevatten, de methode roept de classificatie beroemdheden nadat de classificatie 86 categorie is aangeroepen. Het resultaat bevat tags die beginnen met 'people_'.

## <a name="generating-descriptions"></a>Beschrijvingen genereren 
Computer Vision-API's algoritmen Analyseer de inhoud van een installatiekopie. Deze analyse vormt de basis voor een 'description', weergegeven als leesbare taal in de volledige zinnen. De beschrijving bevat een overzicht van wat er in de afbeelding is gevonden. Computer Vision-API's algoritmen genereren verschillende beschrijvingen op basis van de objecten in de afbeelding geïdentificeerd. De beschrijvingen zijn elk geëvalueerd en een score vertrouwen gegenereerd. Een lijst wordt vervolgens geretourneerd geordende vanaf hoogste vertrouwen score naar laagste aantal gesorteerd. Een voorbeeld van een bot die gebruikmaakt van deze technologie voor het genereren van afbeelding bijschriften vindt [hier](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Voorbeeld van de beschrijving generatie
![B & W gebouwen](./Images/bw_buildings.jpg) '
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

## <a name="perceiving-color-schemes"></a>Perceiving kleurenschema 's
De Computer Vision-algoritme extraheert kleuren van een installatiekopie. De kleuren worden in drie verschillende contexten geanalyseerd: voorgrond, achtergrond en geheel. Ze worden twaalf 12 verwerkingsflexibiliteit Accentkleuren gegroepeerd. Deze Accentkleuren zijn zwart, blauw, bruine, grijs, groen, oranje, roze, paars, rood, groenblauw, wit en geel. Afhankelijk van de kleuren in een afbeelding kunnen eenvoudige zwart of Accentkleuren in hexadecimale kleurcodes worden geretourneerd.

Afbeelding                                                       | Voorgrond |Achtergrond| Kleuren
----------------------------------------------------------- | --------- | ------- | ------
![Open Mountain](./Images/mountain_vista.jpg)            | Zwart     | Zwart   | Wit
![Visie bloem analyseren](./Images/flower.jpg)               | Zwart     | Wit   | Wit, zwart, groen
![Visie Train Station analyseren](./Images/train_station.jpg) | Zwart     | Zwart   | Zwart

### <a name="accent-color"></a>Accentkleur
De kleur opgehaald uit een installatiekopie die is ontworpen om de meest indrukwekkende kleur voor gebruikers via een combinatie van verwerkingsflexibiliteit kleuren en verzadiging vertegenwoordigen.

Afbeelding                                                       | Reactie
----------------------------------------------------------- | ----
![Open Mountain](./Images/mountain_vista.jpg)            | #BC6F0F
![Visie bloem analyseren](./Images/flower.jpg)               | #CAA501
![Visie Train Station analyseren](./Images/train_station.jpg) | #484B83


### <a name="black--white"></a>Zwart- wit
Booleaanse vlag die aangeeft of een installatiekopie van een zwarte is wit of niet.

Afbeelding                                                      | Reactie
---------------------------------------------------------- | ----
![Visie gebouw analyseren](./Images/bw_buildings.jpg)      | Waar
![Visie House Yard analyseren](./Images/house_yard.jpg)      | False

## <a name="flagging-adult-content"></a>Als u inhoud voor volwassenen markeert
Over de verschillende categorieën visual is de groep volwassenen en mooie, waardoor schakelt u het detecteren van volwassenen materialen en de weergave van afbeeldingen met seks worden beperkt. Het filter voor de detectie van volwassenen en mooie inhoud kan worden ingesteld op een schaal van de schuifregelaar om de gebruikersvoorkeur onder te brengen.

## <a name="optical-character-recognition-ocr"></a>(Optische tekenherkenning)
OCR technologie tekstinhoud detecteert in een installatiekopie en haalt de geïdentificeerde tekst in een stream machineleesbare teken. U kunt het resultaat voor zoeken en vele andere doeleinden zoals medische gegevens, beveiliging en bankieren gebruiken. Hij detecteert de taal automatisch. OCR bespaart tijd en gebruiksgemak biedt voor gebruikers door hen foto's van tekst in plaats van ze de tekst.

OCR 25 talen worden ondersteund. Deze talen zijn: Arabisch, vereenvoudigd Chinees, traditioneel Chinees, Tsjechisch, Deens, Nederlands, Engels, Fins, Frans, Duits, Grieks, Hongaars, Italiaans, Japans, Koreaans, Noors, Pools, Portugees, Roemeens, Russisch, Servisch (Cyrillisch en Latijn) Slowaaks, Spaans, Zweeds en Turks.

Indien nodig, corrigeert OCR de rotatie van de herkende tekst, in graden rond de afbeelding van de horizontale as. OCR bevat de coördinaten van het kader van elk woord, zoals weergegeven in onderstaande afbeelding.

![Overzicht van OCR](./Images/vision-overview-ocr.png) vereisten voor OCR:
- De grootte van de installatiekopie van de invoer moet liggen tussen 40 x 40 en 3200 x 3200 pixels.
- De installatiekopie mag niet groter zijn dan 10 megapixels.

De installatiekopie van de invoer kan worden gedraaid een veelvoud van 90 graden plus een kleine hoek van maximaal ' 40 graden.

De nauwkeurigheid van de tekstherkenning is afhankelijk van de kwaliteit van de afbeelding. Het lezen van een onjuist kan worden veroorzaakt door de volgende situaties:
- Fuzzy afbeeldingen.
- De tekst is geschreven of cursief.
- Artistieke lettertypestijlen.
- Grootte van kleine tekst.
- Complexe achtergrond, schaduwen of schitterende via SMS of perspectief vervormd.
- Te groot of ontbrekende hoofdletters aan het begin van woorden
- Het subscript ligt, superscript of Doorhalen tekst.

Beperkingen: Op foto's waarin tekst verwerkingsflexibiliteit is valse positieven kunnen afkomstig zijn van gedeeltelijk herkende woorden. Precisie kan op sommige foto's, met name foto's zonder tekst, veel variëren afhankelijk van het type afbeelding.

## <a name="recognize-handwritten-text"></a>Geschreven tekst herkennen
Deze technologie kunt u om te detecteren en geschreven tekst ophalen uit opmerkingen, letters, verslagen, whiteboards, formulieren, enzovoort. Deze technologie kan worden gebruikt voor verschillende oppervlakken en achtergronden, zoals wit papier, gele plaknotities en whiteboards.

Met handgeschreven tekstherkenning bespaart u tijd en moeite. U kunt productiever werken door afbeeldingen van tekst te maken in plaats van dat u deze moet transcriberen. Maakt het mogelijk gedigitaliseerd opmerkingen. Deze digitization kunt u snel en eenvoudig zoekfuncties moeten worden geïmplementeerd. Bovendien verspilt u minder papier.

Vereisten voor invoer:
- Ondersteunde bestandsindelingen: JPEG-, PNG en BMP.
- De grootte van de installatiekopie-bestand moet kleiner zijn dan 4 MB.
- Afmetingen van de afbeelding moet minimaal 40 x 40, maximaal 3200 x 3200.

Opmerking: deze technologie bevindt zich in de preview-fase en is alleen beschikbaar voor Engelse tekst.

## <a name="generating-thumbnails"></a>Genereren van miniaturen
Een miniatuur is een kleine weergave van een afbeelding op volledige grootte. Uiteenlopende apparaten, zoals telefoons, tablets en pc's, maak een andere gebruikerservaring (UX) indelingen en miniatuurweergaven nodig. Met slim bijsnijden, kunt deze Computer Vision-API-functie u het probleem wordt opgelost.

Na het uploaden van een installatiekopie van een miniatuur van hoge kwaliteit wordt gegenereerd en de Computer Vision-API-algoritme analyseert de objecten in de afbeelding. Deze vervolgens wordt de installatiekopie aan de behoeften van de 'regio van belang' bijgesneden (ROI). De uitvoer opgehaald in een speciale kader weergegeven, zoals weergegeven in onderstaande afbeelding. De gegenereerde miniatuur kan met behulp van een aspect hoeveelheid die verschilt van de hoogte-breedteverhouding van de oorspronkelijke afbeelding om aan de behoeften van een gebruiker te worden gepresenteerd.

De miniatuur algoritme werkt als volgt:

1. Hiermee verwijdert u af elementen van de installatiekopie en herkent het object, de 'regio van belang' (ROI).
2. De installatiekopie op basis van de geïdentificeerde regio van belang zijn, wordt bijgesneden.
3. Hiermee wijzigt u de hoogte-breedteverhouding aanpassen aan de miniatuur doeldimensies.

![Miniaturen](./Images/thumbnail-demo.png)
