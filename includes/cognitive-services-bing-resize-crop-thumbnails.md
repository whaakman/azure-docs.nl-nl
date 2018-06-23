Sommige antwoorden Bing omvatten URL's kunnen worden bediend door Bing miniatuurafbeeldingen. U kunt vergroten of verkleinen en de miniatuurafbeeldingen bijsnijden. 

> [!NOTE]
> Zorg ervoor dat de grootte en bijsnijden van de miniatuur bieden een zoekopdracht scenario en de rechten van derden, zoals vereist door Bing zoeken-API gebruiken respecteren en vereisten worden weergegeven.


Als u een installatiekopie, zijn onder andere de b (breedte) en h (hoogte) queryparameters in de miniatuur-URL. Geef de breedte en hoogte in pixels. Bijvoorbeeld:  
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Als u de grootte van de installatiekopie, wordt de hoogte-breedteverhouding behouden. Als u wilt de hoogte-breedteverhouding behouden, kan witte opvulling worden toegevoegd aan de rand van de afbeelding. Bijvoorbeeld, zijn als u het formaat van een afbeelding met een 480 x 359 200 x 200 zonder bijsnijden, de volledige breedte bevat de installatiekopie, maar de hoogte 25 pixels wit opvulling boven en onder aan de installatiekopie bevat. Dezelfde zou waar zijn als de installatiekopie 359 x 480, behalve de linkerkant is en rechterrand witte opvulling zou bevatten. Als u de afbeelding bijsnijden, witte opvulling niet toegevoegd.  

 
De volgende afbeelding ziet de oorspronkelijke grootte van een miniatuur (480 x 300).  
  
![Oorspronkelijke liggend afbeelding](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
De volgende afbeelding ziet de installatiekopie van het formaat 200 x 200. De hoogte-breedteverhouding behouden blijft en de randen boven- en ondermarge zijn opgevuld met wit (de zwarte rand is opgenomen om weer te geven van de opvulling).  
  
![De installatiekopie van het formaat is gewijzigd liggend](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Als u dimensies die groter dan de oorspronkelijke breedte en hoogte van de afbeelding zijn opgeeft, wordt de afbeelding opgevuld met wit op de linker- en randen.  
  
Als u wilt een afbeelding bijsnijden, omvatten de c (bijsnijden) queryparameter. Hier volgen de mogelijke waarden die u kunt opgeven.  
  
- 4&mdash;visueel verhouding  
- 7&mdash;verhouding van smartcard  
  
Als u Smart verhouding bijsnijden vragen (c = 7), de afbeelding wordt bijgesneden vanaf het midden van de regio van de afbeelding van belang passieve terwijl de installatiekopie hoogte-breedteverhouding behouden. De regio van belang is het gebied van de afbeelding die Bing bepaalt het grootste gedeelte van de invoer bevat. Hier volgt een voorbeeld van de regio van belang.  
  
![De regio van belang zijn](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Als u het formaat van een installatiekopie van een en aanvragen Smart verhouding bijsnijden, wordt de installatiekopie op de aangevraagde grootte verminderd terwijl de hoogte-breedteverhouding behouden. De installatiekopie vervolgens op basis van de dimensies formaat is gewijzigd bijgesneden. Bijvoorbeeld, als de breedte van de grootte kleiner dan of gelijk zijn aan de hoogte is, wordt de afbeelding bijgesneden naar links en rechts van het midden van de regio van belang. Anders wordt wordt de afbeelding bijgesneden boven en onder aan het midden van de regio van belang.  
  
 
Hieronder ziet u de installatiekopie is verlaagd tot 200 x 200 met slim verhouding bijsnijden.  
  
![Afbeelding van Liggend bijgesneden tot 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
Hieronder ziet u de installatiekopie is verlaagd tot 200 x 100 met slim verhouding bijsnijden.  
   
![Afbeelding van Liggend bijgesneden tot 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
Hieronder ziet u de installatiekopie is verlaagd tot 100 x 200 met slim verhouding bijsnijden.  
  
![Afbeelding van Liggend bijgesneden tot 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Als Bing niet belangrijk gebied van de afbeelding, gebruikt Bing Blind verhouding bijsnijden.  
  
Als u Blind verhouding bijsnijden vragen (c = 4), Bing gebruikt de volgende regels voor de afbeelding bijsnijden.  
  
- Als (oorspronkelijke breedte van afbeelding / oorspronkelijke hoogte van de afbeelding) < (aangevraagd van de breedte van afbeelding / aangevraagd hoogte van afbeelding), de installatiekopie wordt gemeten vanaf de bovenkant linkerbovenhoek en aan de onderkant bijgesneden.  
- Als (oorspronkelijke breedte van afbeelding / oorspronkelijke hoogte van de afbeelding) > (aangevraagd van de breedte van afbeelding / aangevraagd hoogte van afbeelding), de installatiekopie wordt gemeten vanaf het midden en links, rechts bijgesneden.  



Hieronder ziet u een installatiekopie staand die 225 x 300.  
  
![Oorspronkelijke sunflower afbeelding](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
Hieronder ziet u de installatiekopie is verlaagd tot 200 x 200 met Blind verhouding bijsnijden. De afbeelding wordt gemeten vanaf de linkerbovenhoek wat resulteert in het onderste gedeelte van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
Hieronder ziet u de installatiekopie is verlaagd tot 200 x 100 met Blind verhouding bijsnijden. De afbeelding wordt gemeten vanaf de linkerbovenhoek wat resulteert in het onderste gedeelte van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
Hieronder ziet u de installatiekopie is verlaagd tot 100 x 200 met Blind verhouding bijsnijden. De afbeelding wordt gemeten vanuit het beheercentrum waardoor de linker- en delen van de afbeelding wordt bijgesneden.  
  
![Sunflower afbeelding bijgesneden tot 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)

