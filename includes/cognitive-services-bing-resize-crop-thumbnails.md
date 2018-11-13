---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: b1cf3dc24482da70372f775d0ba63a203df1b285
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572058"
---
Sommige antwoorden Bing toevoegen URL's voor miniatuurafbeeldingen bediend door Bing. U kunt vergroten of verkleinen en de miniatuurafbeeldingen bijsnijden. 

> [!NOTE]
> Zorg ervoor dat de grootte en bijsnijden van de miniatuur van het bieden van een scenario voor het zoeken en aansluiten bij de rechten van derden, zoals vereist door de Bing zoeken-API en weergavevereisten.


Als u wilt het formaat van een afbeelding, zijn onder andere de w (breedte) en h (hoogte) queryparameters in van de miniatuur-URL. Geef de breedte en hoogte in pixels. Bijvoorbeeld:  
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Als u het formaat van de installatiekopie, wordt de hoogte-breedteverhouding behouden. Als u wilt de hoogte-breedteverhouding behouden, kan witte opvulling worden toegevoegd aan de rand van de installatiekopie. Bijvoorbeeld, als u het formaat van een afbeelding met een 480 x 359 200 x 200 zonder bijsnijden, de volledige breedte van de afbeelding bevat, maar de hoogte bevat 25 pixels wit opvulling van de boven- en onderkant van de afbeelding. Hetzelfde zijn ' True ' als de afbeelding 359 x 480, behalve de links is en rechts randen wit opvulling zou bevatten. Als u de afbeelding bijsnijden, wit opvulling niet toegevoegd.  

 
De volgende afbeelding ziet u de oorspronkelijke grootte van een miniatuurafbeelding (480 x 300).  
  
![Oorspronkelijke liggend afbeelding](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
De volgende afbeelding ziet u de installatiekopie die is uitgebreid tot 200 x 200. De hoogte-breedteverhouding wordt onderhouden en de randen van de boven- en worden aangevuld met technische (de zwarte rand is opgenomen om weer te geven van de opvulling aan).  
  
![Formaat liggend installatiekopie](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Als u dimensies die groter dan de oorspronkelijke breedte en hoogte van de installatiekopie zijn opgeeft, wordt de installatiekopie van het opgevuld met witte op de linker- en randen.  
  
Als u wilt een afbeelding bijsnijden, bevatten de c (bijsnijden) queryparameter. Hier volgen de mogelijke waarden die u kunt opgeven.  
  
- 4&mdash;blind verhouding  
- 7&mdash;verhouding van smartcard  
  
Als u slimme verhouding bijsnijden vragen (c = 7), de afbeelding wordt bijgesneden vanuit het midden van de regio van de installatiekopie van belang passieve behoud van de hoogte-breedteverhouding van de installatiekopie. De regio van belang is het gebied van de installatiekopie die Bing bepaalt de meeste importeren onderdelen bevat. Hieronder ziet u een voorbeeld van de regio van belang zijn.  
  
![De regio van belang zijn](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Als u het formaat van een installatiekopie en aanvragen slimme verhouding bijsnijden, wordt de afbeelding op de aangevraagde grootte verminderd terwijl de hoogte-breedteverhouding behouden. De afbeelding wordt vervolgens bijgesneden op basis van de grootte van het formaat is gewijzigd. Bijvoorbeeld, als de breedte van de grootte kleiner dan of gelijk zijn aan de hoogte is, de afbeelding wordt bijgesneden naar links en rechts van het midden van de regio van belang zijn. Anders wordt wordt de afbeelding bijgesneden aan de bovenkant en onderkant van het midden van de regio van belang zijn.  
  
 
Hieronder ziet u de afbeelding verkleind tot 200 x 200 met behulp van intelligente verhouding bijsnijden.  
  
![Afbeelding van Liggend bijgesneden tot 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 200 x 100 met behulp van intelligente verhouding bijsnijden.  
   
![Afbeelding van Liggend bijgesneden tot 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 100 x 200 met behulp van intelligente verhouding bijsnijden.  
  
![Afbeelding van Liggend bijgesneden tot 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Als u Bing kan niet bepalen de regio van de installatiekopie van belang zijn, gebruikt Bing Blind verhouding bijsnijden.  
  
Als u Blind verhouding bijsnijden vragen (c = 4), Bing maakt gebruik van de volgende regels op de afbeelding bijsnijden.  
  
- Als (oorspronkelijke breedte van afbeelding / hoogte van de oorspronkelijke afbeelding) < (aangevraagd van de breedte van afbeelding / hoogte van de afbeelding aangevraagd), de installatiekopie wordt gemeten vanaf de bovenkant linkerbovenhoek en bijgesneden aan de onderkant.  
- Als (oorspronkelijke breedte van afbeelding / hoogte van de oorspronkelijke afbeelding) > (aangevraagd van de breedte van afbeelding / hoogte van de afbeelding aangevraagd), de installatiekopie wordt gemeten vanaf het midden en bijgesneden tot de links en rechts.  



Hieronder ziet u een staande afbeelding 225 x 300.  
  
![Oorspronkelijke sunflower afbeelding](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 200 x 200 met behulp van Blind verhouding bijsnijden. De afbeelding wordt gemeten in de linkerbovenhoek wat resulteert in het onderste gedeelte van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 200 x 100 met behulp van Blind verhouding bijsnijden. De afbeelding wordt gemeten in de linkerbovenhoek wat resulteert in het onderste gedeelte van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 100 x 200 met behulp van Blind verhouding bijsnijden. De afbeelding wordt gemeten vanaf het midden wat resulteert in de linker- en -onderdelen van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)

