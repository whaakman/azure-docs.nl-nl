---
title: Computer Vision-API voor Microsoft Cognitive Services | Microsoft Docs
description: Met geavanceerde algoritmen in de Computer Vision-API kunt u afbeeldingen verwerken en geeft u informatie in Microsoft Cognitive Services.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/10/2017
ms.author: kefre
ms.openlocfilehash: 84d931ad79bf32b39a4d771f6afd1c9a05ad2395
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714816"
---
# <a name="what-is-computer-vision-api-version-10"></a>Wat is de Computer Vision-API versie 1.0?

> [!IMPORTANT]
> Een nieuwe versie van de Computer Vision-API is nu beschikbaar, Zie:
>- [Overzicht](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Computer Vision-API-versie 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

De cloud-gebaseerde Computer Vision-API geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie. Door een afbeelding te uploaden of een afbeeldings-URL op te geven, kan visuele inhoud door Microsoft Computer Vision-algoritmen op verschillende manieren worden geanalyseerd op basis van invoer en gebruikersopties. Met de Computer Vision-API kunnen gebruikers afbeeldingen te analyseren:
* [Tag-installatiekopieën op basis van inhoud.](#Tagging)
* [Categoriseer installatiekopieën.](#Categorizing)
* [Identificeer het type en de kwaliteit van afbeeldingen.](#Identifying)
* [Detecteer menselijke gezichten en hun coördinaten retourneren. ](#Faces)
* [Domeinspecifieke inhoud herkennen.](#Domain-Specific)
* [Beschrijvingen van de inhoud worden gegenereerd.](#Descriptions)
* [Gebruik optical character recognition gedrukte tekst in afbeeldingen identificeren.](#OCR)
* [Handgeschreven tekst worden herkend.](#RecognizeText)
* [Onderscheid tussen kleurenschema's.](#Color)
* [Inhoud voor volwassenen markeren.](#Adult)
* [Foto's bijsnijden moet worden gebruikt als miniatuurafbeeldingen.](#Thumbnails)

## <a name="requirements"></a>Vereisten
* Invoermethoden ondersteund: Raw image binaire in de vorm van een toepassing/octet-stream of de afbeelding-URL.
* Ondersteunde afbeeldingsindelingen voor: JPEG, PNG-, GIF-, BMP.
* Bestand Afbeeldingsgrootte: minder dan 4 MB.
* De installatiekopie van dimensie: groter is dan 50 x 50 pixels.

## <a name="tagging-images"></a>Installatiekopieën taggen
Computer Vision-API retourneert tags op basis van meer dan 2000 herkenbare objecten, levende wezens, landschappen en acties. Wanneer tags zijn ambigu of niet algemeen bekend, biedt de API-reactie "tips" voor het verduidelijken van de betekenis van de code in de context van een bekende instelling. Tags zijn niet ingedeeld als een taxonomie en er zijn geen overnamehiërarchieën aanwezig is. Een verzameling van inhoud labels vormt de basis vormt voor een afbeelding 'description', weergegeven als een mens leesbaar taal opgemaakt in volledige zinnen. Houd er rekening mee dat op dit moment Engels is de enige ondersteunde taal voor de beschrijving bij afbeelding.

Computer Vision-API van algoritmen uitvoer na het uploaden van een afbeelding of een afbeeldings-URL op te geven, tags op basis van de objecten, levende wezens en acties die zijn geïdentificeerd in de afbeelding. Tags is niet beperkt tot de belangrijkste onderwerp, zoals een persoon op de voorgrond is geplaatst, maar bevat ook de instelling (binnen of buiten), meubels, hulpprogramma's, fabrieken, dieren, accessoires, gadgets enzovoort.

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
## <a name="categorizing-images"></a>Het categoriseren van beelden
Computer Vision-API retourneert naast de labels en beschrijvingen, de taxonomie op basis van categorieën die in eerdere versies zijn gedefinieerd. Deze categorieën zijn ingedeeld als een taxonomie met bovenliggende/onderliggende erfelijke hiërarchieën. Alle categorieën zijn in het Engels. Ze kunnen worden gebruikt alleen of met onze nieuwe modellen.

### <a name="the-86-category-concept"></a>Het concept 86-categorie
Op basis van een lijst met 86 concepten die worden weergegeven in het volgende diagram, kunnen visuele kenmerken gevonden in een afbeelding worden gecategoriseerd variërend van groot tot specifieke. Zie voor de volledige taxonomie in tekstindeling, [Categorietaxonomie](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Categorieën analyseren](./Images/analyze_categories.png)

Installatiekopie                                                  | Antwoord
------------------------------------------------------ | ----------------
![Vrouw plafond](./Images/woman_roof.png)                 | personen
![Familie foto](./Images/family_photo.png)             | people_crowd
![Schattige hond](./Images/cute_dog.png)                     | animal_dog
![Outdoor Mountain](./Images/mountain_vista.png)       | outdoor_mountain
![Vision Food brood analyseren](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>Identificeren van afbeeldingstypen
Er zijn verschillende manieren voor het categoriseren van afbeeldingen. Computer Vision-API kan een Booleaanse vlag die aangeeft of een installatiekopie van een zwart-wit of kleur is ingesteld. Het kan ook een markering om aan te geven of een afbeelding een lijntekening of niet is instellen. Het kan ook duiden of een afbeelding illustraties of niet is en geven de kwaliteit daarom op een schaal van 0-3.

### <a name="clip-art-type"></a>Illustraties type
Detecteert of de installatiekopie van een illustratie of niet is.  

Waarde | Betekenis
----- | --------------
0     | Niet-illustraties
1     | niet-eenduidige
2     | Normaal illustraties
3     | goede illustraties

Installatiekopie|Antwoord
----|----
![Vision kaas illustraties analyseren](./Images/cheese_clipart.png)|3 goed--illustraties
![Vision House Yard analyseren](./Images/house_yard.png)|0 niet--illustraties

### <a name="line-drawing-type"></a>Type lijntekening
Detecteert of een afbeelding een lijntekening of niet is.

Installatiekopie|Antwoord
----|----
![Vision Lion tekening analyseren](./Images/lion_drawing.png)|True
![Vision bloem analyseren](./Images/flower.png)|False

### <a name="faces"></a>Gezichten
Detecteert menselijke gezichten op een afbeelding en genereert de face-coördinaten, de rechthoek voor de face-, geslacht en leeftijd. Deze visuele kenmerken vormen een subset van metagegevens voor face gegenereerd. Gebruik de Face-API voor meer uitgebreide metagegevens worden gegenereerd voor gezichten (gezichtsherkenning-id, houding detectie en meer).  

Installatiekopie|Antwoord
----|----
![Vision vrouw plafond Face analyseren](./Images/woman_roof_face.png) | [{"leeftijd": 23, "geslacht": "Vrouwelijke", "faceRectangle": {"links": 1379, 'top': 320, "width": 310, "hoogte": 310}}]
![Vision Mom dochter Face analyseren](./Images/mom_daughter_face.png) | [{"leeftijd": 28 "geslacht": "Vrouwelijke", "faceRectangle": {"links": 447, 'top': 195, "width": 162, "hoogte": 162}}, {"leeftijd": 10, "geslacht':"Man","faceRectangle": {"links": 355, 'top': 87,"width": 143,"hoogte": 143}}]
![Vision familie foto Face analyseren](./Images/family_photo_face.png) | [{"leeftijd": 11 "geslacht':"Man","faceRectangle": {"links": 113 'top': 314,"width": 222,"hoogte": 222}}, {"leeftijd": 11"geslacht":"Vrouwelijke","faceRectangle": {"links": 1200, 'top': 632,"width": 215,"hoogte": 215}}, {"leeftijd": 41,"geslacht': "Man", " faceRectangle": {"links": 514, 'top': 223,"width": 205,"hoogte": 205}}, {"leeftijd": 37,"geslacht":"Vrouwelijke","faceRectangle": {"links": 1008, 'top': 277,"width": 201,"hoogte": 201}}]


## <a name="domain-specific-content"></a>Domein-specifieke inhoud

Daarnaast categorisatie tagging en op het hoogste niveau, Computer Vision-API biedt ook ondersteuning voor gespecialiseerde (of domeinspecifieke) informatie. Specifieke informatie kan worden geïmplementeerd als zelfstandige methode of met de classificatie op hoog niveau. Het fungeert als een manier om verder te verfijnen 86 categorietaxonomie door toevoeging van domeinspecifieke modellen.

Op dit moment zijn de enige gespecialiseerde informatie ondersteund herkenning van beroemdheden en oriëntatiepunten herkenning. Ze zijn domeinspecifieke verfijningen voor de mensen en categorieën van personen en oriëntatiepunten over de hele wereld.

Er zijn twee opties voor het gebruik van de domeinspecifieke modellen:

### <a name="option-one---scoped-analysis"></a>Optie One - Scoped-analyse
Alleen een gekozen model analyseren door het aanroepen van een HTTP POST-aanroep. Voor deze optie als u welk model u wilt gebruiken weet, u de naam van het model opgeven en u krijgt alleen informatie relevant zijn voor dit model. Bijvoorbeeld, kunt u deze optie alleen gezocht naar herkenning van beroemdheden. Het antwoord bevat een lijst met mogelijkheden die overeenkomt met beroemdheden, vergezeld gaan van hun scores vertrouwen.

### <a name="option-two---enhanced-analysis"></a>Optie 2 - uitgebreide analyse
Analyseren om aanvullende informatie met betrekking tot categorieën van de 86-categorietaxonomie te leveren. Deze optie is beschikbaar voor gebruik in toepassingen waar gebruikers wilt analyse van de installatiekopie van het algemene gedetailleerde gegevens ophalen uit een of meer domeinspecifieke modellen. Wanneer deze methode wordt aangeroepen, wordt eerst de classificatie 86 categorietaxonomie genoemd. Als een van de categorieën die overeenkomen met bekende/modellen overeenkomen, volgt een tweede fase van de classificatie-aanroepen. Bijvoorbeeld, als ' details = all' of 'details' 'beroemdheden' bevatten, de methode roept de classificatie beroemdheden nadat de classificatie 86 categorie wordt genoemd. Het resultaat bevat tags die met 'people_'.

## <a name="generating-descriptions"></a>Beschrijvingen van genereren 
Computer Vision-API van algoritmen analyseren de inhoud in een afbeelding. Deze analyse vormt de basis vormt voor een 'description', weergegeven als leesbare taal in volledige zinnen. De beschrijving bevat een overzicht van wat er in de afbeelding is gevonden. Algoritmen voor computer Vision-API's genereren verschillende beschrijvingen op basis van de objecten die zijn geïdentificeerd in de afbeelding. De beschrijvingen worden geëvalueerd en hiervoor wordt een betrouwbaarheidsscore gegenereerd. Vervolgens wordt er een lijst geretourneerd die is geordend van de hoogste naar de laagste betrouwbaarheidsscore. Een voorbeeld van een bot die gebruikmaakt van deze technologie voor het genereren van afbeelding bijschriften vindt [hier](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Voorbeeld van de beschrijving genereren
![B & W gebouwen](./Images/bw_buildings.png) '
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
De Computer Vision-algoritme extraheert kleuren uit een afbeelding. De kleuren worden in drie verschillende contexten geanalyseerd: voorgrond, achtergrond en geheel. Ze worden in 12 dominante Accentkleuren gegroepeerd. Deze Accentkleuren zijn zwart, blauw, brown, grijs, groen, oranje, roze, paars, rood, groenblauwe, wit en geel. Afhankelijk van de kleuren in een afbeelding, kunnen de eenvoudige zwart-wit of Accentkleuren in hexadecimale kleurcodes worden geretourneerd.

Installatiekopie                                                       | voorgrond |Achtergrond| Kleuren
----------------------------------------------------------- | --------- | ------- | ------
![Outdoor Mountain](./Images/mountain_vista.png)            | Zwart     | Zwart   | Wit
![Vision bloem analyseren](./Images/flower.png)               | Zwart     | Wit   | Groen-wit, zwart,
![Vision Station van de trein analyseren](./Images/train_station.png) | Zwart     | Zwart   | Zwart

### <a name="accent-color"></a>Accentkleur
Kleur van een installatiekopie die is ontworpen om weer te geven van de meest verbluffende kleur aan gebruikers via een combinatie van dominante kleuren en de verzadiging hebt uitgepakt.

Installatiekopie                                                       | Antwoord
----------------------------------------------------------- | ----
![Outdoor Mountain](./Images/mountain_vista.png)            | #BC6F0F
![Vision bloem analyseren](./Images/flower.png)               | #CAA501
![Vision Station van de trein analyseren](./Images/train_station.png) | #484B83


### <a name="black--white"></a>Zwart- wit
Booleaanse vlag die aangeeft of een installatiekopie van een zwart- wit of niet.

Installatiekopie                                                      | Antwoord
---------------------------------------------------------- | ----
![Visie ontwikkelen analyseren](./Images/bw_buildings.png)      | True
![Vision House Yard analyseren](./Images/house_yard.png)      | False

## <a name="flagging-adult-content"></a>Markeren van inhoud voor volwassenen
Is de groep erotische en ongepaste, schakelt u het detecteren van volwassenen materialen en Hiermee beperkt u de weergave van afbeeldingen die seksuele inhoud bevat over de verschillende categorieën van de visual. Het filter voor de detectie van erotische en ongepaste inhoud kan worden ingesteld op een schaal van de schuifregelaar om voorkeur van de gebruiker mogelijk te maken.

## <a name="optical-character-recognition-ocr"></a>Optische tekenherkenning (OCR)
OCR-technologie detecteert tekstinhoud in een afbeelding en worden de herkende tekst naar een machinaal leesbare tekenstroom geëxtraheerd. U kunt het resultaat voor zoeken en talloze andere doeleinden, zoals medische dossiers, beveiliging en bankieren. Hij detecteert de taal automatisch. OCR bespaart tijd en biedt gebruikers gemak door ze om foto's van tekst in plaats van de tekst te transcriberen.

OCR 25 talen worden ondersteund. Deze talen zijn: Arabisch, vereenvoudigd Chinees, traditioneel Chinees, Tsjechisch, Deens, Nederlands, Engels, Fins, Frans, Duits, Grieks, Hongaars, Italiaans, Japans, Koreaans, Noors, Pools, Portugees, Roemeens, Russisch, Servisch (Cyrillisch en Latijns) Slowaaks, Spaans, Zweeds en Turks.

Indien nodig, corrigeert OCR de rotatie van de herkende tekst, in graden rond de afbeelding van de horizontale as. OCR biedt het frame-coördinaten van elk woord, zoals weergegeven in onderstaande afbeelding.

![Overzicht van OCR](./Images/vision-overview-ocr.png) vereisten voor OCR:
- De grootte van de afbeelding moet liggen tussen 40, 40 en 3200 x 3200 pixels.
- De afbeelding kan niet groter zijn dan 10 megapixels.

Afbeelding kan worden gedraaid door meerdere van 90 graden plus een kleine hoek van maximaal ' 40 graden.

De nauwkeurigheid van de tekstherkenning, is afhankelijk van de kwaliteit van de installatiekopie. Een lezen onnauwkeurige kan worden veroorzaakt door de volgende situaties:
- Fuzzy afbeeldingen.
- Handgeschreven of cursieve tekst.
- Artistieke lettertypestijlen.
- Kleine tekengrootte.
- Complexe achtergronden, schaduwen of schitterende via vervorming van tekst of een perspectief.
- Ontbreekt of grote hoofdletters aan het begin van woorden
- Subscript, superscript of doorgehaalde tekst.

De volgende beperkingen: Op de foto's waarin tekst dominante wordt, fout-positieven kunnen afkomstig zijn van gedeeltelijk herkende woorden. Precisie kan op sommige foto's, met name foto's zonder tekst, veel variëren afhankelijk van het type afbeelding.

## <a name="recognize-handwritten-text"></a>Handgeschreven tekstherkenning
Deze technologie kunt u om te detecteren en extraheer handgeschreven tekst uit notities, brieven, essays, whiteboards, formulieren, enzovoort. Deze technologie kan worden gebruikt voor verschillende oppervlakken en achtergronden, zoals wit papier, gele plaknotities en whiteboards.

Met handgeschreven tekstherkenning bespaart u tijd en moeite. U kunt productiever werken door afbeeldingen van tekst te maken in plaats van dat u deze moet transcriberen. U kunt hiermee u notities digitaliseren. Deze digitalisering kunt u snel en eenvoudig zoekfuncties moeten worden geïmplementeerd. Bovendien verspilt u minder papier.

Vereisten voor invoer:
- Ondersteunde afbeeldingsindelingen voor: JPEG, PNG, en BMP.
- Installatiekopie-bestandsgrootte moet minder dan 4 MB.
- Afbeeldingsgrootte moet ten minste 40, 40, maximaal 3200 x 3200.

Opmerking: deze technologie bevindt zich in de preview-fase en is alleen beschikbaar voor Engelse tekst.

## <a name="generating-thumbnails"></a>Miniaturen genereren
Een miniatuur is een kleine weergave van een afbeelding op volledige grootte. Uiteenlopende apparaten zoals telefoons, tablets en pc's maakt u een andere gebruiker gebruikerservaring (UX)-indelingen en miniatuurweergaven nodig. Met slim bijsnijden, kunt deze Computer Vision-API-functie u het probleem is opgelost.

Nadat het uploaden van een afbeelding, een miniatuur van hoge kwaliteit wordt gegenereerd en de algoritme van de Computer Vision-API analyseert de objecten binnen de installatiekopie. Deze vervolgens de installatiekopie aan de behoeften van de 'regio van belang zijn' snijdt (ROI). De uitvoer wordt weergegeven in een speciale framework zoals weergegeven in onderstaande afbeelding. De miniatuur van het gegenereerde kan met behulp van een aspect hoeveelheid die verschilt van de hoogte-breedteverhouding van de oorspronkelijke afbeelding om aan de behoeften van een gebruiker te worden weergegeven.

De miniaturen algoritme werkt als volgt:

1. Hiermee verwijdert u storende elementen van de afbeelding en herkent het object, de 'regio van belang zijn' (ROI).
2. Snijdt de afbeelding op basis van het geïdentificeerde regio van belang zijn.
3. Hiermee wijzigt u de hoogte-breedteverhouding aanpassen aan de grootte van het doel miniatuur.

![Miniaturen](./Images/thumbnail-demo.png)
