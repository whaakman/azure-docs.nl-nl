---
title: Release-opmerkingen voor de Service Face-API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Release-opmerkingen voor de Face-API-Service bevatten een geschiedenis van wijzigingen in de release voor verschillende versies.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 918b3ea5dbaaa44e4eee1a679354c7becffd4686
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345640"
---
# <a name="face-api-release-notes"></a>Face-API-Release-opmerkingen

In dit artikel heeft betrekking op Microsoft Face-API-Service, versie 1.0.

### <a name="release-changes-in-may-2018"></a>Wijzigingen van de release in mei 2018

* Verbeterde `gender` kenmerk ook en aanzienlijk verbeterd `age`, `glasses`, `facialHair`, `hair`, `makeup` kenmerken. Gebruik ze via [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parameter. 

* Verbeterde invoer installatiekopie maximale bestandsgrootte van 4 MB tot 6 MB in [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) en [LargePersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Wijzigingen in maart 2018 release

* Toegevoegde miljoen Scale Container: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) en [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Meer informatie in [het gebruik van de functie voor grootschalige](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Verhoogd [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` parameter vanuit [1, 5] naar [1, 100] en standaard 10.

### <a name="release-changes-in-may-2017"></a>Wijzigingen van de release in mei 2017

* Toegevoegd `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure`, en `noise` kenmerken in [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parameter.

* 10K personen ondersteund in een PersonGroup en [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Paginering in ondersteund [PersonGroup persoon - lijst](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) met optionele parameters: `start` en `top`.

* Ondersteunde gelijktijdigheid in vlakken tegen verschillende FaceLists en andere personen in PersonGroup toevoegen/verwijderen.

### <a name="release-changes-in-march-2017"></a>Wijzigingen in maart 2017 release
* Toegevoegd `emotion` kenmerk in [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parameter.

* Vaste de face kan niet opnieuw worden gedetecteerd met rechthoek geretourneerd van [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als `targetFace` in [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) en [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Een vaste grootte van de waarneembaar face om ervoor te zorgen dat geldt uitsluitend tussen 36 x 36 naar 4096 x 4096 pixels.

### <a name="release-changes-in-november-2016"></a>Release van wijzigingen in November 2016
* Face Storage Standard abonnement voor het opslaan van aanvullende persistente vlakken bij gebruik van toegevoegd [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) of [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) voor identificatie of gelijkenis overeenkomen. De opgeslagen afbeeldingen worden in rekening gebracht volgens $0,5 per 1000 vlakken en deze snelheid is naar rato per dag. Gratis laag abonnementen blijven worden beperkt tot 1000 totale personen.

### <a name="release-changes-in-october-2016"></a>Release van wijzigingen in oktober 2016
* Het foutbericht van meer dan een gezicht in de targetFace van "Er zijn meer dan één face in de afbeelding' aan 'Is er meer dan één face in de afbeelding' gewijzigd in [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) en [PersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Release van wijzigingen in juli 2016
* Face ondersteund met persoon object verificatie in [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Optionele toegevoegd `mode` parameter inschakelen van de selectie van de twee modi van werkende: `matchPerson` en `matchFace` in [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) en de standaardwaarde is `matchPerson`.

* Optionele toegevoegd `confidenceThreshold` parameter voor de gebruiker in te stellen de drempelwaarde van of een face behoort tot een persoon-object in [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Optionele toegevoegd `start` en `top` parameters in [PersonGroup - lijst](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) zodat de gebruiker het beginpunt en het totale aantal PersonGroups naar lijst op te geven.

### <a name="v10-changes-from-v0"></a>V1.0 wordt gewijzigd van V0
* Service-eindpunt hoofdmap van bijgewerkt ```https://westus.api.cognitive.microsoft.com/face/v0/``` naar ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Wijzigingen zijn toegepast op: [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) en [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* De minimale waarneembaar face-grootte is bijgewerkt en 36 x 36 pixels. Vlakken kleiner zijn dan 36 x 36 pixels niet gedetecteerd.

* De gegevens PersonGroup en persoon in Face V0 afgeschaft. Deze gegevens zijn niet toegankelijk met de service Face V1.0.

* Het eindpunt V0 van Face-API op 30 juni 2016 afgeschaft.
