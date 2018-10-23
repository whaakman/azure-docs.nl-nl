---
title: Wat is de Face-API-Service?
titleSuffix: Azure Cognitive Services
description: In dit onderwerp worden de Face-API-service en gerelateerde termen uitgelegd.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 10/11/2018
ms.author: sbowles
ms.openlocfilehash: 6c1e0d0a51bc01c581c05e7ce3215f5501b4be76
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310354"
---
# <a name="what-is-the-face-api-service"></a>Wat is de Face-API-Service?

De Face-API-service is een cloudgebaseerde service die algoritmen biedt voor het analyseren van menselijke gezichten in afbeeldingen en video’s. De Face-API heeft twee belangrijke functies: gezichtsdetectie met kenmerken en gezichtsherkenning.

## <a name="face-detection"></a>Gezichtsdetectie

De Face-API kan maximaal 64 gezichten van mensen met hoge precisie gezichtslocatie in een afbeelding detecteren. De afbeelding kan worden opgegeven door middel van een bestand (een bytestream) of een geldige URL.

![Overzicht - Gezichtsdetectie](./Images/Face.detection.jpg)

De gezichtsrechthoek (links, boven, breedte en hoogte) waarmee wordt aangegeven dat de gezichtslocatie in de afbeelding wordt geretourneerd, samen met elk gedetecteerd gezicht. Gezichtsdetectie extraheert (optioneel) een reeks face-gerelateerde kenmerken zoals houding, geslacht, leeftijd, hoofdhouding, gezichtshaar en bril. Ga voor meer informatie naar [Gezicht - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Gezichtsherkenning

De mogelijkheid menselijke gezichten te kunnen herkennen is belangrijk in veel scenario’s met inbegrip van beveiliging, natuurlijke gebruikersinterface, analyse van afbeeldingsinhoud, mobiele apps en robotica. De Face-API biedt vier gezichtsherkenningsfuncties: gezichtsverificatie, soortgelijke gezichten zoeken, gezichten groeperen en identificatie van personen.

### <a name="face-verification"></a>Gezichtsverificatie

Gezichtsverificatie voert een authenticatie uit op basis van twee gedetecteerde gezichten of een van een gedetecteerd gezicht voor één persoon-object. Ga voor meer gedetailleerde informatie naar [Gezicht - Verifiëren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-faces"></a>Vergelijkbare gezichten zoeken

Opgegeven een doel-gedetecteerd gezicht en een set potentiële gezichten om mee te zoeken, zoekt de service een kleine set gezichten die meest op het doelgezicht lijken. Er worden twee werkingsmodi, **matchFace** en **matchPerson**, ondersteund. De **matchPerson**-modus retourneert gelijkende gezichten na het toepassen van een drempelwaarde van dezelfde persoon afgeleid van [Gezicht - Verifiëren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). De **matchFace**-modus negeert de drempelwaarde voor dezelfde persoon en retourneert de meest soortgelijke potentiële gezichten. Het volgende voorbeeld worden potentiële gezichten weergegeven.
![Overzicht - vergelijkbare gezichten zoeken](./Images/FaceFindSimilar.Candidates.jpg) Het gezochte gezicht is dit.
![Overzicht - vergelijkbare gezichten zoeken](./Images/FaceFindSimilar.QueryFace.jpg)

Om vier soortgelijke gezichten te zoeken, zou de **matchPerson**-modus (a) en (b) retourneren, die dezelfde persoon afbeelden als het zoekgezicht. De **matchFace**-modus retourneert (a), (b), (c) en (d), precies vier kandidaten, zelfs als sommige weinig gelijkenis hebben. Ga voor meer informatie naar [Gezicht - Gelijkende vinden](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Gezichtsgroepering

Met één set met onbekende gezichten, verdeelt de API voor gezichtsgroepering deze automatisch in verschillende groepen op basis van overeenkomsten. Elke groep is een niet-aaneengesloten juiste subset van de oorspronkelijke onbekende gezichtsset en bevat soortgelijke gezichten. Alle gezichten in dezelfde groep kunnen worden beschouwd als behorend tot dezelfde persoon. Ga voor meer informatie naar [Gezicht - Groeperen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="person-identification"></a>Identificatie van personen

De Face-API kan worden gebruikt om personen op basis van een gedetecteerd gezicht en een personen-database te identificeren. U maakt deze database vooraf en deze kan in de loop van de tijd worden bewerkt.

De volgende afbeelding is een voorbeeld van een database met de naam 'myfriends'. Elke groep kan maximaal 1.000.000/10.000 verschillende persoonsobjecten bevatten. Voor elk persoonsobject kunnen maximaal 248 gezichten zijn geregistreerd.

![Overzicht - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Nadat een database is gemaakt en getraind, kan de identificatie worden uitgevoerd op basis van de groep en een nieuw gedetecteerd gezicht. Als het gezicht wordt geïdentificeerd als een persoon in de groep, wordt het persoonsobject geretourneerd.

Voor meer informatie over de identificatie van personen, raadpleegt u de volgende API-handleidingen:

[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)  
[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)  
[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)  
[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)  
[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)  
[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)  
[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)  

#### <a name="face-storage-and-pricing"></a>Gezichtsopslag en prijzen

Gezichtsopslag laat een Standard-abonnement toe extra permanente gezichten op te slaan wanneer u LargePersonGroup/PersonGroup persoonsobjecten gebruikt ([PersonGroup persoon - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup persoon - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) of LargeFaceLists/FaceLists ([FaceList - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) voor identificatie of gelijkenisovereenkomst met de Face-API. De opgeslagen afbeeldingen worden als volgt in rekening gebracht: $0,50 per 1000 gezichten. Dit tarief is per dag pro rato. De gratis abonnementen zijn beperkt tot een totaal van 1000 personen.

Als uw account elke dag bijvoorbeeld 10000 permanente gezichten in de eerste helft van de maand gebruikt, maar geen in de tweede helft, wordt u alleen gefactureerd voor de 10000 gezichten voor de dagen waarop deze zijn opgeslagen. Nog een voorbeeld: als u elke dag gedurende de maand enkele uren 1000 permanente gezichten gebruikt en deze dan elke nacht verwijdert, wordt u nog steeds gefactureerd voor 1000 permanente gezichten per dag.

## <a name="sample-apps"></a>Voorbeeld-apps

Bekijk deze voorbeeldtoepassingen die gebruik maken van de Face-API.

- [Microsoft Face-API: Windows Clientbibliotheek en voorbeeld](https://github.com/Microsoft/Cognitive-Face-Windows)
  - WPF voorbeeld-app die verschillende scenario's laat zien Gezichtsdetectie, analyse en identificatie.
- [FamilyNotes UWP-app](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Universal Windows Platform (UWP)- voorbeeld-app waarin het gebruik van spraak, Cortana, inkt en camera via een scenario met delen van familienotities.
- [Videoframe analysevoorbeeld](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Win32-voorbeeld-app die de analyse toont van live video gegevensstromen bijna in realtime met behulp van de Face-, Computer Vision en Emotion-API's.

## <a name="tutorials"></a>Zelfstudies
De volgende zelfstudies demonstreren de Face-API-basisfuncties en processen voor abonnementen:
- [Aan de slag met de Face-API in CSharp-zelfstudie](Tutorials/FaceAPIinCSharpTutorial.md)
- [Aan de slag met de Face-API in Java voor Android-zelfstudie](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Aan de slag met de Face-API in Python-zelfstudie](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="next-steps"></a>Volgende stappen

Raadpleeg een snelstart voor het implementeren van een eenvoudig scenario voor de Face-API.
- [Snelstart: Gezichten in een afbeelding detecteren met C#](quickstarts/csharp.md) (andere talen beschikbaar)
