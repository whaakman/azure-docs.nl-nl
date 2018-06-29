---
title: Overzicht van de API-Service wordt geconfronteerd | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: De verklarende woordenlijst verklaart de termen die u kunt tegenkomen bij het werken met de Face-API-Service.
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: fb1d14ff80bf53adc3008d79cc998739ffffde1b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048666"
---
# <a name="what-is-face-api"></a>Wat is Face-API?

Welkom bij de Microsoft Face-API een cloudservice waarmee u de meest geavanceerde face-algoritmen. Face-API heeft twee belangrijkste functies: geconfronteerd detectie met kenmerken en wordt geconfronteerd herkenning.

## <a name="face-detection"></a>Gezichtsdetectie

Face-API detecteert maximaal 64 menselijke vlakken met hoge precisie face locatie in een afbeelding. En de installatiekopie van het bestand in bytes of een geldige URL kan worden opgegeven.

![Overzicht - Face-detectie](./Images/Face.detection.jpg)

Face-rechthoek (links, top, breedte en hoogte) die aangeeft de face-locatie in de afbeelding wordt geretourneerd, samen met elk face gedetecteerd. Face detection extraheert eventueel een reeks face-gerelateerde kenmerken, zoals pose, geslacht, leeftijd, head pose, analyseert haar en glas. Zie voor meer informatie [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Face erkenning

Face erkenning wordt veel gebruikt in veel scenario's, waaronder beveiliging, natuurlijke gebruikersinterface Inhoudsanalyse installatiekopie en beheer, mobiele apps en robotics. Vier face erkenning functies zijn beschikbaar: geconfronteerd verificatie, zoeken naar vergelijkbare vlakken face groepering en identificatie.

### <a name="face-verification"></a>Gezichtcontrole

Face-API verificatie voert een verificatie op basis van twee gedetecteerde vlakken of verificatie van één gedetecteerde face op één persoon-object. Voor meer informatie gedetailleerde, Zie [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Zoeken naar vergelijkbare Face

Uitgaande van een gezicht doel gedetecteerd en een set candidate vlakken om te zoeken met, vindt de service een kleine set vlakken die meest op het vlak van het doel lijken. Twee werkende modi `matchFace` en `matchPerson` worden ondersteund. `matchPerson` modus geeft aan dat vergelijkbare vlakken na het toepassen van een drempelwaarde dezelfde persoon afgeleid van [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). `matchFace` modus negeert de drempelwaarde dezelfde persoon en retourneert de bovenste vergelijkbare candidate vlakken. Het volgende voorbeeld nemen, candidate vlakken worden vermeld.
![Overzicht - Face Zoek vergelijkbare](./Images/FaceFindSimilar.Candidates.jpg) en query face ![overzicht - Face Zoek vergelijkbare](./Images/FaceFindSimilar.QueryFace.jpg)

Vier vergelijkbare vlakken, vinden `matchPerson` modus retourneert (a) en (b), die deel uitmaken van dezelfde persoon met query face. `matchFace` modus retourneert (a), (b), (c) en (d), exact vier kandidaten zelfs als lage overeenkomsten. Zie voor meer informatie [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Gezichten groeperen

Uitgaande van een reeks onbekende vlakken, onderverdeeld face-API automatisch groeperen in verschillende groepen op basis van de overeenkomst. Elke groep is een niet-aaneengesloten juiste subset van de oorspronkelijke onbekende gezicht ingesteld en vergelijkbare vlakken bevat. En de vlakken in dezelfde groep kunnen worden overwogen te behoren tot het object met dezelfde persoon. Zie voor meer informatie [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="face-identification"></a>Gezichtidentificatie

Face-API kan worden gebruikt om personen op basis van een gezicht gedetecteerd en een gebruikers-database (gedefinieerd als een LargePersonGroup/PersonGroup) te identificeren. Deze database vooraf maken, welke na verloop van tijd kunnen worden bewerkt.

De volgende afbeelding is een voorbeeld van een LargePersonGroup/PersonGroup met de naam 'myfriends'. Elke groep mag de 1.000.000/10.000 persoon objecten bevatten. Elk object persoon kan ondertussen tot 248 vlakken geregistreerd hebben.

![Overzicht - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Nadat een LargePersonGroup/PersonGroup is gemaakt en getraind, kan identificatie op basis van de groep en een nieuwe gedetecteerde gezicht worden uitgevoerd. Als de face wordt geïdentificeerd als een persoon-object in de groep, wordt de persoon-object geretourneerd.

Zie voor meer informatie over identificatie van de persoon die de volgende API-handleidingen:

[Geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) 
 [LargePersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[LargePersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Opslag van gezichten

Face-opslag kunnen een standaardabonnement voor het opslaan van aanvullende persistente vlakken bij gebruik van LargePersonGroup/PersonGroup persoon objecten ([PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) of LargeFaceLists/FaceLists ([FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) voor de identificatie of gelijkenis die overeenkomt met de Face-API. De opgeslagen afbeeldingen worden in rekening gebracht volgens $0,5 per 1000 vlakken en deze snelheid is naar rato per dag. Gratis laag abonnementen zijn gratis, maar beperkt tot 1000 totale personen.

Prijzen voor Face-opslag is naar rato per dag. Bijvoorbeeld, als uw account gebruikt 10.000 persistente vlakken per dag voor de eerste helft van de maand en er is geen de tweede helft u alleen voor de 10.000 vlakken wordt gefactureerd voor de dagen opgeslagen. U kunt ook als elke dag in de maand een paar uur 1000 vlakken aanhouden en verwijder deze elke nacht, wordt u nog steeds gefactureerd voor 1000 persistente vlakken elke dag.

## <a name="getting-started-tutorials"></a>Zelfstudies aan de slag

De volgende zelfstudies demonstreren de basic Face-API-functies en processen voor abonnementen:

- [Aan de slag met Face-API in CSharp-zelfstudie](Tutorials/FaceAPIinCSharpTutorial.md)
- [Aan de slag met Face-API in Java voor Android-zelfstudie](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Aan de slag met Face-API in Python-zelfstudie](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Voorbeeld-apps

Bekijk deze voorbeeldtoepassingen die gebruik maken van Face-API.

- [Microsoft Face-API: De bibliotheek voor Windows-Client & voorbeeld](https://github.com/Microsoft/Cognitive-Face-Windows)
  - WPF voorbeeld-app die u laat zien van verschillende scenario's voor Face-detectie, analyse en identificatie.
- [FamilyNotes UWP-app](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Universal Windows Platform (UWP) voorbeeld-app waarin informatie over het gebruik van spraak, Cortana inkt en camera via een serie Opmerking delen scenario.
- [Voorbeeld van video Frame-analyse](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Universal Windows Platform (UWP) voorbeeld-app waarin live video gegevensstromen in bijna realtime met Face-, Computer Vision- en Emotion-API's analyseren.

## <a name="related-topics"></a>Verwante onderwerpen

- [Face-API-versie 1.0 Release-opmerkingen](ReleaseNotes.md)
- [Het vaststellen van bespreekt in afbeelding](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Het identificeren van bespreekt in afbeelding](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
