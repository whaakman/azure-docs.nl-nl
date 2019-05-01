---
title: Gezichtsdetectie en concepten van kenmerken
titleSuffix: Azure Cognitive Services
description: Meer informatie over concepten over gezichtsdetectie en face kenmerken.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: 3293067190386738efbfeb433bd38453c9e5699f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64572549"
---
# <a name="face-detection-and-attributes"></a>Gezichtsdetectie en kenmerken

In dit artikel worden de concepten van gezichtsherkenning en face kenmerkgegevens. Gezichtsdetectie is de bewerking van menselijke gezichten zoeken in een afbeelding en desgewenst retourneren een verscheidenheid aan gegevens met betrekking tot face.

U gebruikt de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) bewerking voor het detecteren van gezichten in een afbeelding. Ten minste elke gedetecteerde face komt overeen met een **faceRectangle** veld in het antwoord. Dit is een set pixelcoördinaten (links, boven, breedte, hoogte) markeert het gezicht bevindt. Met behulp van deze coördinaten, krijgt u de locatie van de face-, evenals de grootte ervan. In de API-reactie worden gezichten weergegeven in volgorde van de grootte van grootste naar kleinste.

## <a name="face-id"></a>Gezichts-id

De face-ID is gewoon een tekenreeks op de unieke id voor elke gedetecteerde gezicht in een afbeelding. U kunt aanvragen een face ID in uw [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API-aanroep.

## <a name="face-landmarks"></a>Gezichtsoriëntatiepunten

Gezichtsoriëntatiepunten zijn een verzameling van punten op een gezicht, zoals de leerlingen gemakkelijk te vinden of de punt van nose. Standaard zijn er 27 oriëntatiepunten vooraf gedefinieerde punten. De volgende afbeelding ziet u alle 27 punten:

![Een diagram face met alle 27 oriëntatiepunten met het label](../Images/landmarks.1.jpg)

De coördinaten van de punten worden geretourneerd in eenheden van pixels.

## <a name="attributes"></a>Kenmerken

Kenmerken zijn een set van aanvullende face-functies die optioneel kan worden gedetecteerd door de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. Hier volgen de kenmerken die kunnen worden gedetecteerd:

* **Leeftijd** de geschatte leeftijd, jaar van een bepaald gezicht.
* **Vervagen** de vervaging van het gezicht in de afbeelding. Dit retourneert een waarde tussen 0 en 1, evenals een informele classificatie ('laag', ' Gemiddeld', 'hoog').
* **Blootstelling** de blootstelling van het gezicht in de afbeelding. Dit retourneert een waarde tussen 0 en één, evenals een informele classificatie ("onderbelichting", "goodExposure", 'overbelichting').
* **Emoties** een lijst van emoties met hun detectie vertrouwen voor het opgegeven gezicht. Vertrouwen van de scores zijn genormaliseerd: de scores in alle emoties wordt toegevoegd aan een. De geretourneerde emoties zijn blijdschap, verdriet, neutraal, boosheid, minachting, walging, angst en verrassing.
* **Gezichtshaar** de geschatte gezichtshaar aanwezigheid en de lengte van de bepaald gezicht.
* **Geslacht** de geschatte geslacht van de bepaald gezicht. Mogelijke waarden zijn "Man", 'vrouwelijke' en 'genderless'.
* **Een bril** of de bepaald gezicht bril heeft. Mogelijke waarden zijn "NoGlasses", "ReadingGlasses", "Zonnebril" en 'Zwemmen dragen van een veiligheidsbril'.
* **Haar** de haar stijl van het gezicht. Dit laat zien of de haren zichtbaar is en welke haar kleuren worden gedetecteerd of baldness wordt gedetecteerd.
* **HEAD inhouden** stand van het gezicht in 3D-ruimte. Dit is beschreven door de presentatie, implementatie en plafondmontagemethoden hoeken in graden. De waardebereiken zijn [tussen-90 en, 90], [-180, 180] en [tussen-90 en, 90] graden respectievelijk. Zie het volgende diagram voor hoek toewijzingen:

    ![Een head met de presentatie, rollen en yaw assen met het label](../Images/headpose.1.jpg)
* **Make-up** of het gezicht Make-up heeft. Dit retourneert een Booleaanse waarde voor 'eyeMakeup' en 'lipMakeup'.
* **Ruis** de visual ruis gedetecteerd in het gezicht. Dit retourneert een waarde tussen 0 en 1, evenals een informele classificatie ('laag', ' Gemiddeld', 'hoog').
* **Bedekking** of er nog objecten delen van het gezicht blokkeren. Dit retourneert een Booleaanse waarde voor "eyeOccluded", "foreheadOccluded" en 'mouthOccluded'.
* **Lach** de glimlach-expressie van de bepaald gezicht. Dit is een waarde tussen nul (geen glimlach) en een (wissen glimlach).

> [!IMPORTANT]
> Face-kenmerken worden voorspeld door het gebruik van statistische algoritmen en altijd mogelijk niet in nauwkeurig. Wees voorzichtig bij het nemen van besluiten op basis van kenmerkgegevens.

## <a name="input-data"></a>Invoergegevens

Gebruik de volgende tips om te controleren of dat uw invoer afbeeldingen geven de meest nauwkeurige resultaten:

* De afbeelding ondersteunde indelingen zijn JPEG, PNG-, GIF-bestand (het eerste frame), BMP.
* De grootte van de installatiekopie-bestand mag niet groter zijn dan 4 MB zijn.
* Het bereik van de grootte detecteerbare gezicht is 36 x 36 naar 4096 x 4096 pixels. Gezichten buiten dit bereik wordt niet worden gedetecteerd.
* Sommige gezichten kunnen niet worden gedetecteerd vanwege technische problemen. Extreme face hoeken (hoofd houding) of face bedekking (objecten zoals zonnebril of handen onderdeel van het gezicht blokkeren) kan invloed hebben op detectie. Voorzijde en in de buurt binnen handbereik gezichten bieden de beste resultaten.

## <a name="next-steps"></a>Volgende stappen

Nu dat u bekend met face detection-concepten bent, leer over het schrijven van een eenvoudig script die gezichten gedetecteerd in een bepaalde installatiekopie.

* [Hoe u gezichtsherkenning in een afbeelding](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)