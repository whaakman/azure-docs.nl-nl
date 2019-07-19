---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: f96c3a693ce8fc099374c998b35ce2fa90f4bb3f
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868983"
---
Enkele Bing-antwoorden bevatten Url's naar miniatuur afbeeldingen die door Bing worden geleverd. U kunt de miniatuur afbeeldingen verg Roten of verkleinen en bijsnijden. 

> [!NOTE]
> Zorg ervoor dat de miniatuur grootte en bijsnijden een zoek scenario bieden en de rechten van derden eerbiedigen, zoals wordt vereist door Bing Search-API-gebruik en-weergave vereisten.


Als u het formaat van een afbeelding wilt wijzigen, neemt u de para meter w (breedte) query parameter, h (hoogte) op in de URL van de miniatuur. Geef de breedte en hoogte op in pixels. Bijvoorbeeld:  
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Als u alleen de breedte of alleen de para meter voor de hoogte query opgeeft, wordt de hoogte-breedte verhouding van de afbeelding gehandhaafd. Als u zowel width als Height opgeeft en u de oorspronkelijke hoogte-breedte verhouding van de afbeelding niet wilt behouden, voegt Bing witte opvulling toe aan de rand van de afbeelding. Als u bijvoorbeeld het formaat van een 480x359-afbeelding wijzigt in 200x200 zonder bijsnijden, bevat de volledige breedte de afbeelding, maar de hoogte bevat 25 pixels witte opvulling boven en onder aan de afbeelding. Hetzelfde geldt als de afbeelding 359x480, met uitzonde ring van de linker-en rechter rand zou witte opvulling bevatten. Als u de afbeelding bijsnijdt, wordt er geen witte opvulling toegevoegd.  

 
In de volgende afbeelding ziet u de oorspronkelijke grootte van een miniatuur afbeelding (480x300).  
  
![Oorspronkelijke liggende afbeelding](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
In de volgende afbeelding ziet u de afbeelding waarvan de grootte is gewijzigd in 200x200. De hoogte-breedte verhouding wordt gehandhaafd en de boven-en onderrand worden gevuld met wit (de zwarte rand wordt opgenomen om de opvulling weer te geven).  
  
![Grootte van liggende afbeelding gewijzigd](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Als u dimensies opgeeft die groter zijn dan de oorspronkelijke breedte en hoogte van de afbeelding, wordt de afbeelding opgevuld met wit aan de linker-en bovenrand.  
  
Als u een afbeelding wilt bijsnijden, neemt u de query parameter c (gewas) op. Hier volgen de mogelijke waarden die u kunt opgeven.  
  
- 4&mdash;-blinde verhouding  
- 7&mdash;-slimme verhouding  
  
Als u bijsnijding van Smart quote (c = 7) aanvraagt, wordt de afbeelding bijgesneden van het midden van de regio van de afbeelding naar buiten en met behoud van de hoogte-breedte verhouding van de afbeelding. De gewenste regio is het gebied van de installatie kopie die door Bing wordt bepaald en bevat de meeste import onderdelen. Hieronder ziet u een voor beeld-regio.  
  
![Interesse gebied](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Als u het formaat van een afbeelding wijzigt en het bijsnijden van intelligente verhoudingen vraagt, wordt de afbeelding gereduceerd tot de gevraagde grootte en blijft de hoogte-breedte verhouding behouden. De afbeelding wordt vervolgens bijgesneden op basis van de afmetingen waarvan het formaat is gewijzigd. Als de breedte van het formaat bijvoorbeeld kleiner is dan of gelijk is aan de hoogte, wordt de afbeelding links en rechts van het midden van de interesse bijgesneden. Anders wordt de afbeelding bijgesneden tot de boven-en onderkant van het midden van de interesse.  
  
 
Hieronder ziet u de afbeelding die is gereduceerd tot 200x200 met behulp van slimme verhoudingen.  
  
![Liggende afbeelding bijgesneden tot 200x200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
Hieronder ziet u de afbeelding die is gereduceerd tot 200x100 met behulp van slimme verhoudingen.  
   
![Liggende afbeelding bijgesneden tot 200x100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
Hieronder ziet u de afbeelding die is gereduceerd tot 100x200 met behulp van slimme verhoudingen.  
  
![Liggende afbeelding bijgesneden tot 100x200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Als Bing de gewenste regio van de installatie kopie niet kan bepalen, gebruikt Bing het versnijden van blinde verhoudingen.  
  
Als u het bijsnijden van blinde verhouding (c = 4) aanvraagt, worden de volgende regels gebruikt om de afbeelding bij te snijden.  
  
- Als (oorspronkelijke afbeelding breedte/oorspronkelijke afbeelding hoogte) < (aangevraagde afbeelding breedte/aangevraagde afbeelding hoogte), wordt de afbeelding gemeten vanaf de linkerbovenhoek en aan de onderkant bijgesneden.  
- Als (oorspronkelijke afbeelding breedte/oorspronkelijke afbeelding hoogte) > (aangevraagde afbeelding breedte/aangevraagde afbeelding hoogte), wordt de afbeelding gemeten vanuit het midden en aan de linker-en rechter kant bijgesneden.  



Hieronder ziet u een staande afbeelding 225x300.  
  
![Oorspronkelijke zonnebloem-afbeelding](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 200x200 met behulp van blinde verhouding bijsnijden. De afbeelding wordt gemeten vanaf de linkerbovenhoek, waardoor het onderste deel van de afbeelding wordt bijgesneden.  
  
![Zonnebloem-afbeelding bijgesneden tot 200x200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 200x100 met behulp van blinde verhouding bijsnijden. De afbeelding wordt gemeten vanaf de linkerbovenhoek, waardoor het onderste deel van de afbeelding wordt bijgesneden.  
  
![Zonnebloem-afbeelding bijgesneden tot 200x100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
Hieronder ziet u de afbeelding verkleind tot 100x200 met behulp van blinde verhouding bijsnijden. De afbeelding wordt gemeten vanuit het midden, waardoor de linker-en rechter delen van de afbeelding worden bijgesneden.  
  
![Zonnebloem-afbeelding bijgesneden tot 100x200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)

