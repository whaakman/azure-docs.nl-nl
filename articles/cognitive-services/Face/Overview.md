---
title: Wat is de Face-API-Service?
titleSuffix: Azure Cognitive Services
description: De woordenlijst verklaart de termen die u kunt tegenkomen wanneer u met de Face-API-Service werkt.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 15de899be5ab85e9fe84ba1b6284bc9419fcf8a1
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123467"
---
# <a name="what-is-the-face-api-service"></a>Wat is de Face-API-Service?

Welkom bij de Face-API-Service, een cloud-gebaseerde service die de meest geavanceerde gezichtsalgoritmen verstrekt. Face-API heeft twee belangrijke functies: gezichtsdetectie met kenmerken en gezichtsherkenning.

## <a name="face-detection"></a>Gezichtsdetectie

Face-API detecteert maximaal 64 gezichten van mensen met hoge precisie gezichtslocatie in een afbeelding. En de afbeelding kan worden opgegeven per bestand in bytes of een geldige URL.

![Overzicht - Gezichtsdetectie](./Images/Face.detection.jpg)

Gezichtsrechthoek (links, boven, breedte en hoogte) waarmee wordt aangegeven dat de gezichts-locatie in de afbeelding wordt geretourneerd, samen met elk gedetecteerd gezicht. Gezichtsdetectie extraheert (optioneel) een reeks face-gerelateerde kenmerken zoals houding, geslacht, leeftijd, hoofdhouding, gezichtshaar en bril. Ga voor meer informatie naar [Gezicht - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Gezichtsherkenning

Gezichtsherkenning wordt op grote schaal gebruikt in veel scenario’s met inbegrip van beveiliging, natuurlijke gebruikersinterface, kopie inhoudsanalyse en beheer, mobiele apps en robotica. U vindt vier gezichtsherkenningsfuncties: gezichtsverificatie, soortgelijke gezichten zoeken, gezichten groeperen en identificatie van personen.

### <a name="face-verification"></a>Gezichtsverificatie

Face-API-verificatie voert een authenticatie uit op basis van twee gedetecteerde gezichten of een authenticatie van een gedetecteerd gezicht voor één persoon-object. Ga voor meer gedetailleerde informatie naar [Gezicht - Verifiëren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Vergelijkbare gezichten zoeken

Opgegeven een doel-gedetecteerd gezicht en een set potentiële gezichten om mee te zoeken, zoekt de service een kleine set gezichten die meest op het doelgezicht lijken. Twee werkingsmodi werken `matchFace` en `matchPerson` worden ondersteund. `matchPerson`-modus retourneert gelijkende gezichten na het toepassen van een drempelwaarde van dezelfde persoon afgeleid van [Gezicht - Verifiëren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). `matchFace`-modus negeert de drempelwaarde voor dezelfde persoon en retourneert de meest soortgelijke potentiële gezichten. Het volgende voorbeeld nemen, potentiële gezichten worden weergegeven.
![Overzicht - Zoek gelijkende gezichten](./Images/FaceFindSimilar.Candidates.jpg) en query gezicht is ![Overzicht - Zoek gelijkende gezichten](./Images/FaceFindSimilar.QueryFace.jpg)

Om vier gelijkende gezichten te zoeken, `matchPerson`-modus retourneert (a) en (b), die behoren tot dezelfde persoon met query gezicht. `matchFace`-modus retourneert (a), (b), (c) en (d), precies vier kandidaten, zelfs als er weinig gelijkenis is. Ga voor meer informatie naar [Gezicht - Gelijkende vinden](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Gezichtsgroepering

Opgegeven één set met onbekende gezichten, verdeelt face-API ze automatisch in verschillende groepen op basis van overeenkomsten. Elke groep is een niet-aaneengesloten juiste subset van de oorspronkelijke onbekende gezichtsset en bevat soortgelijke gezichten. En al de gezichten in dezelfde groep kunnen worden beschouwd als behorend tot hetzelfde persoonsobject. Ga voor meer informatie naar [Gezicht - Groeperen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="face-identification"></a>Gezichtsidentificatie

Face-API kan worden gebruikt om personen op basis van een gedetecteerde gezicht en een personen-database (gedefinieerd als een LargePersonGroup/PersonGroup) te identificeren. Deze database, vooraf gemaakt, kan na verloop van tijd worden bewerkt.

De volgende afbeelding is een voorbeeld van een LargePersonGroup/PersonGroup met de naam "myfriends". Elke groep kan maximaal 1.000.000/10.000 persoonsobjecten bevatten. Elk persoonsobject kan ondertussen tot 248 gezichten geregistreerd hebben.

![Overzicht - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Nadat een LargePersonGroup/PersonGroup is gemaakt en getraind, kan de identificatie worden uitgevoerd op basis van de groep en een nieuw gedetecteerd gezicht. Als het gezicht wordt geïdentificeerd als een persoonsobject in de groep, wordt het persoonsobject geretourneerd.

Voor meer informatie over de identificatie van personen, raadpleegt u de volgende API-handleidingen:

[Gezicht - Identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[PersonGroup persoon - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup - Trainen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) 
 [LargePersonGroup - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[LargePersonGroup persoon - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup - Trainen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Gezichtsopslag

Gezichtsopslag laat een Standard-abonnement toe extra permanente gezichten op te slaan wanneer u LargePersonGroup/PersonGroup persoonsobjecten gebruikt ([PersonGroup persoon - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup persoon - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) of LargeFaceLists/FaceLists ([FaceList - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) voor identificatie of gelijkenisovereenkomst met de Face-API. De opgeslagen afbeeldingen worden als volgt in rekening gebracht: $0,5 per 1000 gezichten. Dit tarief is per dag pro rato. De gratis laag abonnementen zijn gratis, maar beperkt tot een totaal van 1000 personen.

De prijs voor Gezichtsopslag is per dag pro rato. Als uw account elke dag bijvoorbeeld 10000 permanente gezichten in de eerste helft van de maand gebruikt, maar geen in de tweede helft, wordt u alleen gefactureerd voor de 10000 gezichten voor de dagen waarop deze zijn opgeslagen. Nog een voorbeeld: als u elke dag gedurende de maand enkele uren 1000 permanente gezichten gebruikt en deze dan elke nacht verwijdert, wordt u nog steeds gefactureerd voor 1000 permanente gezichten per dag.

## <a name="getting-started-tutorials"></a>Zelfstudies voor Aan de slag

De volgende zelfstudies demonstreren de Face-API-basisfuncties en processen voor abonnementen:

- [Aan de slag met de Face-API in CSharp-zelfstudie](Tutorials/FaceAPIinCSharpTutorial.md)
- [Aan de slag met de Face-API in Java voor Android-zelfstudie](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Aan de slag met de Face-API in Python-zelfstudie](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Voorbeeld-apps

Bekijk deze voorbeeldtoepassingen die gebruik maken van de Face-API.

- [Microsoft Face-API: Windows Clientbibliotheek en voorbeeld](https://github.com/Microsoft/Cognitive-Face-Windows)
  - WPF voorbeeld-app die verschillende scenario's laat zien Gezichtsdetectie, analyse en identificatie.
- [FamilyNotes UWP-app](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Universal Windows Platform (UWP)- voorbeeld-app waarin het gebruik van spraak, Cortana, inkt en camera via een scenario met delen van familienotities.
- [Videoframe analysevoorbeeld](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Universal Windows Platform (UWP)-voorbeeld-app toont de analyse van live video gegevensstromen bijna in realtime met behulp van de Face-, Computer Vision en Emotion-API's.

## <a name="related-topics"></a>Verwante onderwerpen

- [Face-API-versie 1.0 Opmerkingen bij de release](ReleaseNotes.md)
- [Gezichten in een afbeelding detecteren](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Gezichten in een afbeelding identificeren](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
