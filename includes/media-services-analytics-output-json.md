De taak wordt een JSON-uitvoer-bestand met metagegevens over gedetecteerde en bijgehouden. De metagegevens bevat de coördinaten die de locatie van vlakken, evenals een face-id die aangeeft dat afzonderlijke volgen. Face-id-nummers zijn kwetsbaar voor opnieuw omstandigheden worden ingesteld als de voorzijde face verloren is gegaan of elkaar overlappen in het kader waardoor bepaalde personen ophalen van meerdere id's toegewezen.

De JSON-uitvoer bevat de volgende elementen:

### <a name="root-json-elements"></a>Hoofdmap JSON-elementen

| Element | Beschrijving |
| --- | --- |
| versie |Dit verwijst naar de versie van de Video-API. |
| Tijdschaal |'Maatstreepjes' per seconde van de video. |
| offset |Dit is het tijdverschil voor tijdstempels. Versie 1.0 van Video-API's, zal dit altijd 0 zijn. In de toekomst scenario's die wordt ondersteund, deze waarde kan worden gewijzigd. |
| de breedte van veel |De breedte en veel van de video frame van uitvoer in pixels.|
| framesnelheid |Frames per seconde van de video. |
| [fragmenten](#fragments-json-elements) |De metagegevens wordt gesegmenteerde in verschillende segmenten fragmenten aangeroepen. Elke fragment bevat een start, duur, Intervalnummer en gebeurtenis(sen). |

### <a name="fragments-json-elements"></a>Fragmenten JSON-elementen

|Element|Beschrijving|
|---|---|
| start |De begintijd van de eerste gebeurtenis in "tikken." |
| Duur |De lengte van het fragment, in "tikken." |
| index | (Alleen van toepassing op Azure Media Redactor) definieert de frame-index van de huidige gebeurtenis. |
| interval |Het interval van elke gebeurtenisvermelding in het fragment, in "tikken." |
| events |Elke gebeurtenis bevat de vlakken gedetecteerd en bijgehouden in die tijd. Er is een matrix van gebeurtenissen. De buitenste matrix vertegenwoordigt een tijdsinterval. De interne matrix bestaat uit 0 of meer gebeurtenissen die hebben plaatsgevonden op dat moment. Een leeg accolade [] betekent dat er geen vlakken zijn gedetecteerd. |
| id |De ID van het oppervlak dat wordt bijgehouden. Dit nummer mogelijk per ongeluk wijzigen als een gezicht niet gevonden wordt. Een bepaalde persoon moet dezelfde ID in de algehele video hebben, maar dit kan niet worden gegarandeerd door beperkingen in de detectie-algoritme (Occlusie, enzovoort). |
| x, y |De linkerbovenhoek X en Y-coördinaten van het oppervlak begrenzingsvak in een genormaliseerde schaal van 0,0 en 1,0. <br/>-X en Y coördinaten ten opzichte van Liggend altijd, zijn dus als er een portret video (of omlaag, in het geval van iOS), hebt u de coördinaten dienovereenkomstig omzetting. |
| breedte, hoogte |De breedte en hoogte van het oppervlak begrenzingsvak in een genormaliseerde schaal van 0,0 en 1,0. |
| facesDetected |Dit is gevonden aan het einde van de JSON-resultaten en bevat een overzicht van het aantal vlakken die het algoritme dat tijdens de video wordt gedetecteerd. Omdat de id's per ongeluk kunnen worden hersteld als een gezicht niet gevonden wordt (bijvoorbeeld de face scherm ziet er verwijderd), gaat het true aantal vlakken in de video mogelijk niet altijd gelijk aan dit nummer. |
