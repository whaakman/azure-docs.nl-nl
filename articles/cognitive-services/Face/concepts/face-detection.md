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
ms.openlocfilehash: e61048eeab9d7061c18f3237db22fc87ca52f526
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65891168"
---
# <a name="face-detection-and-attributes"></a>Gezichtsdetectie en kenmerken

In dit artikel worden de concepten van gezichtsherkenning en face kenmerkgegevens. Gezichtsdetectie is de bewerking van menselijke gezichten zoeken in een afbeelding en desgewenst retourneren van verschillende soorten gegevens met betrekking tot face.

U gebruikt de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) bewerking voor het detecteren van gezichten in een afbeelding. Ten minste elke gedetecteerde face komt overeen met een veld faceRectangle in het antwoord. Deze reeks pixelcoördinaten voor de links, boven, breedte en hoogte markeren het gezicht bevindt. Met behulp van deze coördinaten, krijgt u de locatie van het gezicht en de grootte ervan. In de API-reactie worden gezichten weergegeven in volgorde van de grootte van grootste naar kleinste.

## <a name="face-id"></a>Gezichts-id

De face-ID is een tekenreeks met de unieke id voor elke gedetecteerde gezicht in een afbeelding. U kunt aanvragen een face ID in uw [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API-aanroep.

## <a name="face-landmarks"></a>Gezichtsoriëntatiepunten

Gezichtsoriëntatiepunten zijn een set van punten op een gezicht, zoals de leerlingen of de tip van de voorzijde gemakkelijk te vinden. Standaard zijn er 27 oriëntatiepunten vooraf gedefinieerde punten. De volgende afbeelding ziet u alle 27 punten:

![Een diagram face met alle 27 oriëntatiepunten met het label](../Images/landmarks.1.jpg)

De coördinaten van de punten worden geretourneerd in eenheden van pixels.

## <a name="attributes"></a>Kenmerken

Kenmerken zijn een set van functies die optioneel kan worden gedetecteerd door de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. De volgende kenmerken kunnen worden gedetecteerd:

* **Leeftijd**. De geschatte leeftijd in jaar van een bepaald gezicht.
* **Blur**. De vervaging van het gezicht in de afbeelding. Dit kenmerk retourneert een waarde tussen 0 en 1 en een informele classificatie van laag, Gemiddeld of hoog.
* **Emoties**. Een lijst van emoties met hun detectie vertrouwen voor het opgegeven gezicht. Vertrouwen van de scores zijn genormaliseerd en de scores in alle emoties toevoegen aan een. De geretourneerde emoties zijn blijdschap, verdriet, neutraal, boosheid, minachting, walging, angst en verrassing.
* **Blootstelling**. De blootstelling van het gezicht in de afbeelding. Dit kenmerk retourneert een waarde tussen 0 en 1 en een informele classificatie van onderbelichting, goodExposure of overbelichting.
* **Gezichtshaar**. De geschatte gezichtshaar aanwezigheid en de lengte voor een bepaald gezicht.
* **Geslacht**. De geschatte geslacht van de bepaald gezicht. Mogelijke waarden zijn man, vrouw en genderless.
* **Een bril**. Bepaalt of de bepaald gezicht bril heeft. Mogelijke waarden zijn NoGlasses, ReadingGlasses zonnebril en dragen van een veiligheidsbril zwemmen.
* **Haar**. Het type haar van het gezicht. Dit kenmerk bevat of de haren zichtbaar is en welke haar kleuren worden gedetecteerd of baldness wordt gedetecteerd.
* **HEAD inhouden**. Richting van het gezicht in 3D-ruimte. Dit kenmerk wordt beschreven door de presentatie, implementatie en plafondmontagemethoden hoeken in graden. De waardebereiken zijn 90 graden op 90 graden, -180 en 180 graden en 90 graden 90 graden. Zie het volgende diagram voor hoek toewijzingen:

    ![Een head met de presentatie, rollen en yaw assen met het label](../Images/headpose.1.jpg)
* **Makeup**. Bepaalt of het gezicht Make-up heeft. Dit kenmerk retourneert een Booleaanse waarde voor eyeMakeup en lipMakeup.
* **Ruis**. De visual ruis gedetecteerd in het gezicht. Dit kenmerk retourneert een waarde tussen 0 en 1 en een informele classificatie van laag, Gemiddeld of hoog.
* **Bedekking**. Of er blokkerende onderdelen van de face-objecten. Dit kenmerk retourneert een Booleaanse waarde voor eyeOccluded foreheadOccluded en mouthOccluded.
* **Lach**. De expressie glimlach van de bepaald gezicht. Deze waarde is tussen de nul voor geen glimlach en één voor glimlach wissen.

> [!IMPORTANT]
> Face-kenmerken worden voorspeld door het gebruik van statistische algoritmen. Ze altijd mogelijk niet nauwkeurig. Wees voorzichtig wanneer u op basis van kenmerkgegevens beslissingen.

## <a name="input-data"></a>Invoergegevens

Gebruik de volgende tips om ervoor te zorgen dat uw invoer afbeeldingen de meest nauwkeurige resultaten geven:

* De afbeelding ondersteunde indelingen zijn JPEG, PNG-, GIF-bestand voor de eerste frame, en BMP.
* De grootte van de installatiekopie mag niet groter zijn dan 4 MB zijn.
* Het bereik van de grootte detecteerbare gezicht is 36 x 36 naar 4096 x 4096 pixels. Gezichten buiten dit bereik wordt niet worden gedetecteerd.
* Sommige gezichten kunnen niet worden gevonden vanwege technische problemen. Extreme face hoeken (hoofd houding) of face bedekking (objecten zoals zonnebril of handen die deel uitmaken van het gezicht blokkeren) kan invloed hebben op detectie. Voorzijde en in de buurt binnen handbereik gezichten bieden de beste resultaten.

Als u gezichten in een feed video detecteren bent, kunt u mogelijk de prestaties verbeteren door bepaalde instellingen op uw videocamera aan te passen:

* **Vloeiend**: Veel video camera's toepassen vloeiend maken het effect. U moet dit uitschakelen als u de kunt omdat het een vervaging tussen frames maakt en duidelijkheid vermindert.
* **Sluitersnelheid**: Waarop u sneller wordt de hoeveelheid beweging tussen frames en elk duidelijker frame maakt. U wordt aangeraden waarop snelheden van 1/60 seconden of sneller.
* **Waarop hoek**: Sommige camera's opgeven waarop hoek in plaats van waarop snelheid. Indien mogelijk moet u een lagere waarop hoek gebruiken. Dit wordt duidelijker videoframes leiden.

    >[!NOTE]
    > Een camera met een lagere waarop hoek ontvangt minder licht in elk frame, zodat de afbeelding donkerder. U moet bepalen het juiste niveau te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Nu dat u bekend met face detection-concepten bent, Leer hoe u een script schrijven dat gezichten gedetecteerd in een bepaalde installatiekopie.

* [Detecteer gezichten in een afbeelding](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)