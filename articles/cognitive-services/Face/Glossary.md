---
title: Verklarende woordenlijst voor de Service Face-API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: De verklarende woordenlijst verklaart de termen die u kunt tegenkomen bij het werken met de Face-API-Service.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: ec3068c013be9f2fa4ff34b1c24596e46e7c5d05
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344589"
---
# <a name="glossary"></a>Woordenlijst

## <a name="a"></a>A

#### <a name="Attributes"></a>kenmerken

Kenmerken zijn optioneel in de [detectie](#Detection-Face-Detection) resulteert, zoals [leeftijd](#Age-Attribute), [geslacht](#Gender-Attribute), [head pose](#Head-Pose-Attribute), [analyseert haar](#Facial-Hair-Attribute), [Glimlachende](#Smile-Attribute).
Ze kunnen worden verkregen van de [detectie](#Detection-Face-Detection) API door te geven van de queryparameters: returnFaceAttributes. Kenmerken geven extra informatie met betrekking tot geselecteerde [vlakken](#Face); naast de [geconfronteerd ID](#Face-ID) en de [rechthoek](#Face-Rectangle).

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Age-Attribute"></a>Leeftijd (kenmerk)

Leeftijd is een van de [kenmerken](#Attributes) die de leeftijd van een bepaalde gezicht beschrijft. Het leeftijdkenmerk is optioneel in de [detectie](#Detection-Face-Detection) resulteert en kan worden beheerd met een [detectie](#Detection-Face-Detection) aanvraag door de parameter returnFaceAttributes te geven.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="b"></a>B

## <a name="c"></a>C

#### <a name="Candidate"></a>Kandidaat

Kandidaten zijn in wezen [identificatie](#Identification) resultaten (bijvoorbeeld geïdentificeerde personen en betrouwbaar detecties). Een kandidaat wordt vertegenwoordigd door de [PersonID](#Person-ID) en [vertrouwen](#Confidence), die aangeeft dat de persoon die wordt geïdentificeerd met een hoge mate van betrouwbaarheid.

Raadpleeg voor meer informatie de handleiding [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

#### <a name="Confidence"></a>Vertrouwen

Vertrouwen is een meting weer te geven de overeenkomsten tussen [vlakken](#Face) of [persoon](#Person) in numerieke waarden – die wordt gebruikt in [identificatie](#Identification), en [ verificatie](#Verification) om aan te geven van de overeenkomsten gezocht, geïdentificeerd en geverifieerde resultaten.

Voor meer informatie raadpleegt u de volgende handleidingen: [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="d"></a>D

#### <a name="Detection-Face-Detection"></a>Detectie/Face-detectie

Face-detectie is de actie vlakken in afbeeldingen te zoeken. Gebruikers kunnen een installatiekopie uploaden of een afbeeldings-URL opgeven in de aanvraag. De gedetecteerde vlakken worden geretourneerd met [id's wordt geconfronteerd](#Face-ID) die wijzen op een unieke identiteit in Face-API. De rechthoeken geven de face-locaties in de afbeelding in pixels, evenals de optionele [kenmerken](#Attributes) voor elk vlak zoals [leeftijd](#Age-Attribute), [geslacht](#Gender-Attribute), [head pose ](#Head-Pose-Attribute), [analyseert haar](#Facial-Hair-Attribute) en [Glimlachende](#Smile-Attribute).

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="e"></a>E

## <a name="f"></a>F

#### <a name="Face"></a>Face

Face is een uniform term voor de resultaten die zijn gerelateerd aan gedetecteerde vlakken Face-API is afgeleid. Uiteindelijk face wordt vertegenwoordigd door een uniforme identiteit ([Face-ID](#Face-ID)), een bepaald gebied in installatiekopieën ([Face rechthoek](#Face-Rectangle)), en extra face gerelateerde [kenmerken](#Face-Attributes-Facial-Attributes), zoals [leeftijd](#Age-Attribute), [geslacht](#Gender-Attribute), [monumenten](#Face-Landmarks-Facial-Landmarks) en [head pose](#Head-Pose-Attribute). Bovendien vlakken kunnen worden geretourneerd van [detectie](#Detection-Face-Detection).

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Face-API"></a>Face-API

Face-API is een cloud-gebaseerde API met de meest geavanceerde algoritmen voor face detection en herkenning. De belangrijkste functionaliteit van Face-API kan worden onderverdeeld in twee categorieën worden onderverdeeld: face [detectie](#Detection-Face-Detection) met [kenmerken](#Face-Attributes-Facial-Attributes), en face [erkenning](#Recognition).

Voor meer informatie raadpleegt u de volgende handleidingen: [Face-API-overzicht](./Overview.md), [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [Geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="Face-Attributes-Facial-Attributes"></a>Face kenmerken/gezichtsbehandeling kenmerken

Zie [kenmerken](#Attributes).

#### <a name="Face-ID"></a>Face-ID

Face-ID is afgeleid van de [detectie](#Detection-Face-Detection) resultaten, waarin een tekenreeks vertegenwoordigt een [face](#Face) in [Face-API](#Face-API).

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Face-Landmarks-Facial-Landmarks"></a>Face monumenten/gezichtsbehandeling monumenten

Monumenten zijn optioneel in de [detectie](#Detection-Face-Detection) de resultaten; semantische analyseert punten, zoals de ogen, neus en mond (geïllustreerd in de volgende afbeelding). Monumenten kunnen worden beheerd met een [detectie](#Detection-Face-Detection) verzoek van de Booleaanse nummer returnFaceLandmarks. Als returnFaceLandmarks is ingesteld als waar, wordt de geretourneerde vlakken kenmerkende kenmerken hebben.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![HowToDetectFace](./Images/landmarks.1.jpg)

#### <a name="Face-Rectangle"></a>Face-rechthoek

Face-rechthoek is afgeleid van de [detectie](#Detection-Face-Detection) resultaten, die een staande rechthoek (links, top, width en height) in de afbeeldingen in pixels. De linkerbovenhoek van een [face](#Face) (links, top), naast de breedte en hoogte geeft face grootte x en y-as respectievelijk.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Facial-Hair-Attribute"></a>Analyseert haar (kenmerk)

Analyseert haar is een van de [kenmerken](#Attributes) gebruikt om te beschrijven de analyseert haar lengte van de beschikbare vlakken. Het kenmerk analyseert haar is optioneel in de [detectie](#Detection-Face-Detection) resulteert en kan worden beheerd met een [detectie](#Detection-Face-Detection) aanvraag door returnFaceAttributes. Als returnFaceAttributes 'facialHair' bevat, wordt de geretourneerde vlakken analyseert haar kenmerken hebben.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="FaceList"></a>FaceList

FaceList is een verzameling [PersistedFace](#PersistedFace) en is de maateenheid [Zoek vergelijkbare](#Find-Similar). Een FaceList wordt geleverd met een [FaceList ID](#FaceList-ID), plus andere kenmerken, zoals [naam](#Name) en [gebruikersgegevens](#UserData-User-Data).

Voor meer informatie raadpleegt u de volgende handleidingen: [FaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="FaceList-ID"></a>FaceList-ID

FaceList-ID is een gebruiker opgegeven tekenreeks die wordt gebruikt als een id van een [FaceList](#FaceList). De FaceList-ID moet uniek zijn binnen het abonnement.

Voor meer informatie raadpleegt u de volgende handleidingen: [FaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="Find-Similar"></a>Zoek vergelijkbare

Deze API wordt gebruikt om de zoekopdracht vergelijkbare vlakken op basis van een verzameling van vlakken. Query vlakken en face verzamelingen worden weergegeven als [id's wordt geconfronteerd](#Face-ID) of [FceList ID](#FaceList-ID)/[LargeFaceList ID](#LargeFaceList-ID) in de aanvraag. Geretourneerde resultaten worden gezocht vergelijkbare vlakken, vertegenwoordigd door [id's wordt geconfronteerd](#Face-ID) of [PersistedFace-id's](#PersistedFace-ID).

Voor meer informatie raadpleegt u de volgende handleidingen: [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [LargeFaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [FaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b).

## <a name="g"></a>G

#### <a name="Gender-Attribute"></a>Geslacht (kenmerk)

Geslacht is een van de [kenmerken](#Attributes) gebruikt om te beschrijven de geslachten van de beschikbare vlakken. Het kenmerk geslacht is optioneel in de [detectie](#Detection-Face-Detection) resulteert en kan worden beheerd met een [detectie](#Detection-Face-Detection) aanvraag door returnFaceAttributes. Als returnfaceAttributes 'geslacht' bevat, wordt de geretourneerde vlakken geslacht kenmerken hebben.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Grouping"></a>Groeperen

Face groepering is de groepering van een verzameling van vlakken volgens analyseert overeenkomsten. Face-verzamelingen worden aangeduid met de verzamelingen face-ID in de aanvraag. Als gevolg van groepering vergelijkbare vlakken bij elkaar worden gegroepeerd als [groepen](#Groups), en vlakken die niet vergelijkbaar met een andere vlak zijn samengevoegd als een rommelige groep. Er is maximaal, één [rommelige groep](#Messy-Group) in het resultaat van de groepering.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

#### <a name="Groups"></a>Groepen

Groepen zijn afgeleid van de [groeperen](#Grouping) resultaten. Elke groep bevat een verzameling van soortgelijke vlakken, waarbij vlakken worden aangeduid met [id's wordt geconfronteerd](#Face-ID).

Raadpleeg voor meer informatie de handleiding [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="h"></a>H

#### <a name="Head-Pose-Attribute"></a>HEAD inhouden (kenmerk)

HEAD pose is een van de [kenmerken](#Attributes) dat vertegenwoordigt toegankelijk zijn vanaf de afdrukstand in 3D-ruimte volgens album, presentatie en plafondmontagemethoden hoeken, zoals wordt weergegeven in de volgende afbeelding. De waardebereiken van draaien en weet [-180, 180] en [-90, 90] yaw in graden. In de huidige versie, de presentatie geretourneerde waarde van detectie is altijd 0. Het kenmerk head pose is optioneel in de [detectie](#Detection-Face-Detection) resulteert en kan worden beheerd met een [detectie](#Detection-Face-Detection) aanvraag door de parameter returnFaceAttributes. Als returnFaceAttributes parameter 'headPose' bevat, wordt de geretourneerde vlakken head bedreigen kenmerken hebben.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![GlossaryHeadPose](./Images/headpose.1.jpg)

## <a name="i"></a>I

#### <a name="Identification"></a>Identificatie

Identificatie is het identificeren van een of meer vlakken van een LargePersonGroup/PersonGroup.
Een [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup) is een verzameling [personen](#Person).
Vlakken en de LargePersonGroup/PersonGroup worden vertegenwoordigd door [id's wordt geconfronteerd](#Face-ID) en [LargePersonGroup-id's](#LargePersonGroup-ID)/[PersonGroup-id's](#PersonGroup-ID) respectievelijk in de de aanvraag.
De resultaten van de geïdentificeerde [kandidaten](#Candidate), dat wordt vertegenwoordigd door [personen](#Person) met vertrouwen.
Meerdere vlakken in de invoer afzonderlijk worden beschouwd en elk vlak heeft een eigen geïdentificeerde resultaat.

> [!NOTE]
> De LargePersonGroup/PersonGroup moet vóór de identificatie met succes worden getraind. Als de LargePersonGroup/PersonGroup is niet getraind, of de training [status](#Status-Train) wordt niet weergegeven als 'voltooid' (dat wil zeggen ' actief', 'mislukt' of 'out'), het antwoord van de aanvraag is 400.
> 

Voor meer informatie raadpleegt u de volgende handleidingen: [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [LargePersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [ LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="Is-Identical"></a>IsIdentical

IsIdentical is een Boole-veld van [verificatie](#Verification) resultaten die aangeeft of twee vlakken deel uitmaken van dezelfde persoon.

Raadpleeg voor meer informatie de handleiding [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="j"></a>D

## <a name="k"></a>K

## <a name="l"></a>L

#### <a name="landmarks"></a>Monumenten

Zie [geconfronteerd monumenten](#Face-Landmarks-Facial-Landmarks).

#### <a name="LargeFaceList"></a>LargeFaceList

LargeFaceList is een verzameling [PersistedFace](#PersistedFace) en is de maateenheid [Zoek vergelijkbare](#Find-Similar). Een LargeFaceList wordt geleverd met een [LargeFaceList ID](#LargeFaceList-ID), plus andere kenmerken, zoals [naam](#Name) en [gebruikersgegevens](#UserData-User-Data).

Voor meer informatie raadpleegt u de volgende handleidingen: [LargeFaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce), [LargeFaceList - lijst Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158db4d2de3616c086f2d6).

#### <a name="LargeFaceList-ID"></a>LargeFaceList-ID

LargeFaceList-ID is een gebruiker opgegeven tekenreeks die wordt gebruikt als een id van een [LargeFaceList](#LargeFaceList). De LargeFaceList-ID moet uniek zijn binnen het abonnement.

Voor meer informatie raadpleegt u de volgende handleidingen: [LargeFaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce).

#### <a name="LargePersonGroup"></a>LargePersonGroup

LargePersonGroup is een verzameling [personen](#Person) en is de maateenheid [identificatie](#Identification). Een LargePersonGroup wordt geleverd met een [LargePersonGroup ID](#LargePersonGroup-ID), plus andere kenmerken, zoals [naam](#Name) en [gebruikersgegevens](#UserData-User-Data).

Voor meer informatie raadpleegt u de volgende handleidingen: [LargePersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [LargePersonGroup persoon - lijst](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adda06ac60f11b48b5aa1).

#### <a name="LargePersonGroup-ID"></a>LargePersonGroup-ID

LargePersonGroup-ID is een gebruiker opgegeven tekenreeks die wordt gebruikt als een id van een [LargePersonGroup](#LargePersonGroup). De LargePersonGroup-ID moet uniek zijn binnen het abonnement.

Voor meer informatie raadpleegt u de volgende handleidingen: [LargePersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e).

## <a name="m"></a>M

#### <a name="Messy-Group"></a>Rommelige groep

Rommelige groep is afgeleid van de [groeperen](#Grouping) resultaten; die niet vergelijkbaar is met een andere vlak vlakken bevat. Elk vlak in een groep rommelige wordt aangegeven door de [geconfronteerd ID](#Face-ID).

Raadpleeg voor meer informatie de handleiding [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="n"></a>N

#### <a name="name-person"></a>Naam (persoon)

De naam is een gebruikersvriendelijker beschrijvende tekenreeks voor [persoon](#Person). In tegenstelling tot de [persoon ID](#Person-ID), de naam van de mensen binnen een groep kan worden gedupliceerd.

Voor meer informatie raadpleegt u de volgende handleidingen: [LargePersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup persoon - ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Ophalen van de persoon PersonGroup -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="Name"></a>Naam (LargePersonGroup/PersonGroup)

De naam is ook een gebruikersvriendelijker beschrijvende tekenreeks voor [LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup). In tegenstelling tot de [LargePersonGroup ID](#LargePersonGroup-ID)/[PersonGroup ID](#PersonGroup-ID), de naam van LargePersonGroups/PersonGroups kan worden gedupliceerd binnen een abonnement.

Voor meer informatie raadpleegt u de volgende handleidingen: [LargePersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [ PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

## <a name="o"></a>O

## <a name="p"></a>P

#### <a name="PersistedFace"></a>PersistedFace

PersistedFace is een gegevensstructuur in Face-API. PersistedFace wordt geleverd met een [PersistedFace ID](#PersistedFace-ID), plus andere kenmerken, zoals [naam](#Name), en [gebruikersgegevens](#UserData-User-Data).

Voor meer informatie raadpleegt u de volgende handleidingen: [LargeFaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [ PersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="Person-ID"></a>ID van de persoon

ID van de persoon die wordt gegenereerd wanneer een [PersistedFace](#PersistedFace) is gemaakt. Een tekenreeks gemaakt waarmee dit Face in [Face-API](#Face-API).

Voor meer informatie raadpleegt u de volgende handleidingen: [LargeFaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [ PersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="Person"></a>Persoon

Persoon is een gegevensstructuur die wordt beheerd in Face-API. Persoon wordt geleverd met een [persoon ID](#Person-ID), plus andere kenmerken, zoals [naam](#Name), een verzameling van [PersistedFace](#PersistedFace), en [gebruikersgegevens](#UserData-User-Data).

Voor meer informatie raadpleegt u de volgende handleidingen: [LargePersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup persoon - ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Ophalen van de persoon PersonGroup -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="Person-ID"></a>ID van de persoon

ID van de persoon die wordt gegenereerd wanneer een [persoon](#Person) is gemaakt. Een tekenreeks gemaakt waarmee deze persoon in [Face-API](#Face-API).

Voor meer informatie raadpleegt u de volgende handleidingen: [LargePersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup persoon - ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Ophalen van de persoon PersonGroup -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="PersonGroup"></a>PersonGroup

PersonGroup is een verzameling [personen](#Person) en is de maateenheid [identificatie](#Identification). Een PersonGroup wordt geleverd met een [PersonGroup ID](#PersonGroup-ID), plus andere kenmerken, zoals [naam](#Name) en [gebruikersgegevens](#UserData-User-Data).

Voor meer informatie raadpleegt u de volgende handleidingen: [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246), [PersonGroup persoon - lijst](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241).

#### <a name="PersonGroup-ID"></a>PersonGroup-ID

PersonGroup-ID is een gebruiker opgegeven tekenreeks die wordt gebruikt als een id van een [PersonGroup](#PersonGroup). De groeps-ID moet uniek zijn binnen het abonnement.

Voor meer informatie raadpleegt u de volgende handleidingen: [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

#### <a name="pose-attribute"></a>Inhouden (kenmerk)

Zie [Head bedreigen](#Head-Pose-Attribute).

## <a name="q"></a>Q

## <a name="r"></a>R

#### <a name="Recognition"></a>Opname

Herkenning is een populair Sales voor face-technologieën, zoals [Zoek vergelijkbare](#Find-Similar), [groeperen](#Grouping), [identificeren](#Identification),[bespreekt dezelfde twee verifiëren of niet ](#Verification).

Voor meer informatie raadpleegt u de volgende handleidingen: [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="rectangle-face"></a>Rechthoek (Face)

Zie [face rechthoek](#Face-Rectangle).

## <a name="s"></a>S

#### <a name="Smile-Attribute"></a>Glimlach (kenmerk)

Glimlach is een van de [kenmerken](#Attributes) gebruikt om de expressie glimlach van de beschikbare vlakken te beschrijven. Het kenmerk glimlach is optioneel in de [detectie](#Detection-Face-Detection) resulteert en kan worden beheerd met een [detectie](#Detection-Face-Detection) aanvraag door returnFaceAttributes. Als returnFaceAttributes bevat de glimlach', wordt de geretourneerde vlakken glimlach kenmerken hebben.

Raadpleeg voor meer informatie de handleiding [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="similar-face-searching"></a>Vergelijkbare gezichten zoeken

Zie [Zoek vergelijkbare](#Find-Similar).

#### <a name="Status-Train"></a>Status (Train)

De status is een tekenreeks die wordt gebruikt om te beschrijven van de procedure voor het [Training LargePersonGroups-LargeFaceList/PersonGroups](#Train), inclusief notstarted, 'actief', 'geslaagd', 'mislukt'.

Raadpleeg voor meer informatie de handleiding [LargeFaceList - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="subscription-key"></a>Abonnementssleutel

Abonnementssleutel is een tekenreeks die u moet opgeven als een queryreeksparameter opgeven om te kunnen Face-API aanroepen. De abonnementssleutel vindt u in Mijn abonnementen pagina nadat u zich aanmeldt bij Microsoft cognitieve Services-portal. Er zijn twee sleutels die zijn gekoppeld aan elk abonnement: een primaire sleutel en een secundaire sleutel. Beide kunnen worden gebruikt om aan te roepen API identiek. U moet ervoor zorgen dat de abonnement-sleutels veilig en kunt u abonnement sleutels opnieuw genereren op elk gewenst moment en Mijn abonnementen pagina.

## <a name="t"></a>T

#### <a name="Train"></a>Train (LargePersonGroup-LargeFaceList/PersonGroup)

Deze API wordt gebruikt voor het vooraf verwerken de [LargeFaceList](#LargeFaceList)/[LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup) om te controleren of de [vinden Vergelijkbare](#Find-Similar)/[identificatie](#Identification) prestaties. Als de training niet wordt beheerd, of de [Training Status](#Status-Train) wordt niet weergegeven als is voltooid, de id voor deze PersonGroup leidt ertoe dat is mislukt.

Voor meer informatie raadpleegt u de volgende handleidingen: [LargeFaceList - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249), [geconfronteerd: bepalen ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="u"></a>U

#### <a name="UserData-User-Data"></a>UserData/gebruikersgegevens

Gebruikersgegevens is extra informatie die is gekoppeld aan [persoon](#Person) en [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup). Gebruikersgegevens is ingesteld door gebruikers gegevens gemakkelijker te gebruiken, te begrijpen en te onthouden.

Voor meer informatie raadpleegt u de volgende handleidingen: [LargePersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Update](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acfc83a7b9412a4d53f3f), [LargePersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup persoon - bijwerken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ade043a7b9412a4d53f41), [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Update](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524a), [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Bijwerken van de persoon PersonGroup -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395242).

## <a name="v"></a>V

#### <a name="Verification"></a>Verificatie

Deze API wordt gebruikt om te controleren of twee vlakken hetzelfde of niet zijn. Beide kanten worden weergegeven als id's in de aanvraag te maken. Resultaten bevatten een Boole-veld geverifieerd ([isIdentical](#Is-Identical)) die wijzen op dezelfde als de waarde true en een numeriek veld ([vertrouwen](#Confidence)) die aangeeft van de mate van betrouwbaarheid.

Raadpleeg voor meer informatie de handleiding [geconfronteerd: Controleer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="w"></a>W

## <a name="x"></a>X

## <a name="y"></a>J

## <a name="z"></a>Z
