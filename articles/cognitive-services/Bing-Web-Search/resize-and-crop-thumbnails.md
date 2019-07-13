---
title: Het formaat en bijsnijden afbeeldingsminiaturen - Bing webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: Informatie over het vergroten of verkleinen en bijgesneden miniaturen geleverd door de Bing zoeken-API's.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 05A08B01-89FF-4781-AFE7-08DA92F25047
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 07/08/2019
ms.author: aahi
ms.openlocfilehash: 6a5b2dada254a0bfc7fa60172f56221ba67ad279
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867914"
---
# <a name="resize-and-crop-thumbnail-images"></a>Het formaat en miniatuurafbeeldingen bijsnijden

Sommige antwoorden van de Bing zoeken-API's URL's opnemen naar miniatuurafbeeldingen bediend door Bing, die u kunt het formaat en bijsnijden en queryparameters kunnen bevatten. Bijvoorbeeld:

`https://<host>/th?id=AMMS_92772df988...&w=110&h=73&rs=1&qlt=80&cdv=1&pid=16.1`

Als u een subset van deze miniatuurweergaven weergeeft, geeft u een optie om de resterende installatiekopieën weer te geven.

> [!NOTE]
> Zorg ervoor dat dat bijsnijden en de grootte van miniatuurafbeeldingen wijzigen een search-scenario waarin rekening wordt gehouden met de rechten van derden, zoals vereist door de Bing zoeken-API biedt [gebruiken en weergavevereisten](use-display-requirements.md).

## <a name="resize-a-thumbnail"></a>Formaat van een miniatuur 

Als u wilt het formaat van een miniatuur, Bing wordt aanbevolen dat u slechts één opgeven de `w` (breedte) of `h` queryparameters (hoogte) van de miniatuur-URL. Op te geven alleen de breedte of hoogte kunt Bing onderhouden van de oorspronkelijke hoogte van de installatiekopie. Geef de breedte en hoogte in pixels. 

Bijvoorbeeld, als de oorspronkelijke miniatuur 480 x 620:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=480&h=620`

En u wilt de grootte, stelt u de `w` parameter voor een nieuwe waarde (bijvoorbeeld `336`), en verwijder de `h` parameter:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=336`

Als u alleen de breedte of hoogte van een miniatuur opgeeft, wordt de oorspronkelijke hoogte-breedteverhouding van de installatiekopie worden onderhouden. Als u beide parameters opgeven en de hoogte-breedteverhouding niet wordt beheerd, voeg Bing wit opvulling toe aan de rand van de installatiekopie.

Bijvoorbeeld, als u het formaat van een afbeelding met een 480 x 359 200 x 200 zonder bijsnijden, bevat de volledige breedte van de installatiekopie van het maar de hoogte 25 pixels wit opvulling van de boven- en onderkant van de afbeelding bevat. Als de afbeelding 359 x 480 is, zou de linker- en randen wit opvulling bevatten. Als u de afbeelding bijsnijden, wit opvulling niet toegevoegd.  

De volgende afbeelding ziet u de oorspronkelijke grootte van een miniatuurafbeelding (480 x 300).  
  
![Oorspronkelijke liggend afbeelding](./media/resize-crop/bing-resize-crop-landscape.png)  
  
De volgende afbeelding ziet u de installatiekopie die is uitgebreid tot 200 x 200. De hoogte-breedteverhouding wordt onderhouden en de randen van de boven- en worden aangevuld met technische (hier de zwarte rand is opgenomen om weer te geven van de opvulling aan).  
  
![Formaat liggend installatiekopie](./media/resize-crop/bing-resize-crop-landscape-resized.png)  

Als u dimensies die groter dan de oorspronkelijke breedte en hoogte van de installatiekopie zijn opgeeft, voeg Bing wit opvulling toe aan de linker- en randen.  

## <a name="request-different-thumbnail-sizes"></a>Aanvragen van verschillende grootten van de miniatuur

Om aan te vragen de grootte van een andere miniatuurafbeelding, verwijdert u alle queryparameters uit van de miniatuur-URL, met uitzondering van de `id` en `pid` parameters. Voeg een de `&w` (breedte) of `&h` queryparameter (hoogte) met de grootte van de gewenste installatiekopie in pixels, maar niet beide. Bing handhaaft de oorspronkelijke hoogte-breedteverhouding van de installatiekopie. 

Als u wilt vergroten de breedte van de installatiekopie die wordt opgegeven door de bovenstaande URL 165 pixels, gebruikt u de volgende URL:

`https://<host>/th?id=AMMS_92772df988...&w=165&pid=16.1`

Als u een installatiekopie die groter is dan de oorspronkelijke grootte van de installatiekopie hebt aangevraagd, wordt in het Bing wit opvulling rond de afbeelding wordt toegevoegd indien nodig. Bijvoorbeeld, als de installatiekopie van het oorspronkelijke grootte 474 x 316 is en u `&w` op 500, Bing een 500 x 333-installatiekopie wordt geretourneerd. Deze installatiekopie wordt 8,5 pixels van wit opvulling langs de boven- en onderrand en 13 pixels van de opvulling van de linker- en rechterranden hebben.

Om te voorkomen dat Bing wit opvulling toe te voegen als de aangevraagde grootte groter is dan de oorspronkelijke grootte van de installatiekopie, stel de `&p` queryparameter op 0. Als u bijvoorbeeld de `&p=0` parameter in de bovenstaande URL, Bing, wordt een installatiekopie 474 x 316 in plaats van een installatiekopie 500 x 333 retourneren:

`https://<host>/th?id=AMMS_92772df988...&w=500&p=0&pid=16.1`

Als u beide opgeeft `&w` en `&h` queryparameters, Bing hoogte-breedteverhouding van de installatiekopie wordt onderhouden en voegt wit opvulling indien nodig. Bijvoorbeeld, als de installatiekopie van het oorspronkelijke grootte 474 x 316 is en u de breedte en hoogte-parameters instellen op 200 x 200 (`&w=200&h=200`), Bing retourneert een installatiekopie met 33 pixels van wit opvulling boven en onder. Als u de `&p` queryparameter, Bing retourneert een installatiekopie van een 200 x 134.

## <a name="crop-a-thumbnail"></a>Een miniatuur bijsnijden 

Als u wilt een afbeelding bijsnijden, bevatten de `c` queryparameter (bijsnijden). U kunt de volgende waarden:
  
- `4` &mdash; Blind verhouding  
- `7` &mdash; Slimme verhouding  

### <a name="smart-ratio-cropping"></a>Slim bijsnijden verhouding

Als u slimme verhouding bijsnijden vragen (door in te stellen de `c` parameter `7`), Bing wordt een afbeelding bijsnijden vanaf het midden van de regio van belang passieve, behoud van de hoogte-breedteverhouding van de installatiekopie. De regio van belang is het gebied van de installatiekopie die Bing bepaalt de meeste importeren onderdelen bevat. Hieronder ziet u een voorbeeld van de regio van belang zijn.  
  
![De regio van belang zijn](./media/resize-crop/bing-resize-crop-regionofinterest.png)

Als u het formaat van een installatiekopie en aanvragen slimme verhouding bijsnijden, vermindert Bing de afbeelding op de aangevraagde grootte terwijl de hoogte-breedteverhouding behouden. Bing snijdt vervolgens de installatiekopie op basis van de grootte van het formaat is gewijzigd. Als de breedte van de grootte kleiner dan of gelijk zijn aan de hoogte is, wordt Bing bijvoorbeeld bijsnijden in de afbeelding links en rechts van het midden van de regio van belang zijn. Anders wordt Bing bijsnijden aan de bovenkant en onderkant van het midden van de regio van belang zijn.  
  
 
Hieronder ziet u de afbeelding verkleind tot 200 x 200 met behulp van intelligente verhouding bijsnijden. Omdat Bing van een installatiekopie van de linkerbovenhoek maatregelen, wordt het onderste gedeelte van de afbeelding bijgesneden. 
  
![Afbeelding van Liggend bijgesneden tot 200 x 200](./media/resize-crop/bing-resize-crop-landscape200x200c7.png) 
  
Hieronder ziet u de afbeelding verkleind tot 200 x 100 met behulp van intelligente verhouding bijsnijden. Omdat Bing van een installatiekopie van de linkerbovenhoek maatregelen, wordt het onderste gedeelte van de afbeelding bijgesneden. 
   
![Afbeelding van Liggend bijgesneden tot 200 x 100](./media/resize-crop/bing-resize-crop-landscape200x100c7.png)
  
Hieronder ziet u de afbeelding verkleind tot 100 x 200 met behulp van intelligente verhouding bijsnijden. Omdat Bing van een installatiekopie van het center maatregelen, wordt de linker- en -onderdelen van de installatiekopie worden bijgesneden.
  
![Afbeelding van Liggend bijgesneden tot 100 x 200](./media/resize-crop/bing-resize-crop-landscape100x200c7.png) 

Als u Bing kan niet bepalen de regio van de installatiekopie van belang zijn, kan de service wordt gebruikt Blind verhouding bijsnijden.  

### <a name="blind-ratio-cropping"></a>Blind verhouding bijsnijden

Als u Blind verhouding bijsnijden vragen (door in te stellen de `c` parameter `4`), Bing maakt gebruik van de volgende regels op de afbeelding bijsnijden.  
  
- Als `(Original Image Width / Original Image Height) < (Requested Image Width / Requested Image Height)`, de installatiekopie is gemeten vanuit het linkermenu en bijgesneden aan de onderkant.  
- Als `(Original Image Width / Original Image Height) > (Requested Image Width / Requested Image Height)`, de installatiekopie wordt gemeten vanaf het midden en bijgesneden tot de links en rechts.  

Hieronder ziet u een staande afbeelding 225 x 300.  
  
![Oorspronkelijke sunflower afbeelding](./media/resize-crop/bing-resize-crop-sunflower.png)
  
Hieronder ziet u de afbeelding verkleind tot 200 x 200 met behulp van Blind verhouding bijsnijden. De afbeelding wordt gemeten in de linkerbovenhoek wat resulteert in het onderste gedeelte van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 200 x 200](./media/resize-crop/bing-resize-crop-sunflower200x200c4.png)
  
Hieronder ziet u de afbeelding verkleind tot 200 x 100 met behulp van Blind verhouding bijsnijden. De afbeelding wordt gemeten in de linkerbovenhoek wat resulteert in het onderste gedeelte van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 200 x 100](./media/resize-crop/bing-resize-crop-sunflower200x100c4.png)
  
Hieronder ziet u de afbeelding verkleind tot 100 x 200 met behulp van Blind verhouding bijsnijden. De afbeelding wordt gemeten vanaf het midden wat resulteert in de linker- en -onderdelen van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 100 x 200](./media/resize-crop/bing-resize-crop-sunflower100x200c4.png)

## <a name="next-steps"></a>Volgende stappen

* [Wat zijn de Bing zoeken-API's?](bing-api-comparison.md)
* [Bing zoeken-API gebruiken en weergavevereisten](use-display-requirements.md)
