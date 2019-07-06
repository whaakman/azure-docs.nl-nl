---
title: Wat is de Face-API?
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Face-service voor het detecteren en analyseren van gezichten op afbeeldingen.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 4be33f781dec93fd9fe1b1846322672266cd7350
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606917"
---
# <a name="what-is-the-azure-face-api"></a>Wat is de Azure Face-API?

De Azure Cognitive Services Face-API biedt algoritmen die worden gebruikt om te detecteren en analyseren van menselijke gezichten in foto's herkennen. De mogelijkheid voor het verwerken van menselijke gezichten informatie is belangrijk in veel scenario's voor andere software. Scenario's met voorbeelden zijn beveiliging, natuurlijke gebruikersinterface, Inhoudsanalyse van de afbeelding en beheer, mobiele apps en robotiek.

De Face-API biedt verschillende functies. Elke functie wordt beschreven in de volgende secties. Lees verder voor meer informatie over deze.

## <a name="face-detection"></a>Gezichtsdetectie

De Face-API detecteert menselijke gezichten in een afbeelding en retourneert de rechthoekcoördinaten van hun locatie. Gezichtsdetectie kan eventueel een reeks face-gerelateerde kenmerken extraheren. Voorbeelden zijn head houding, geslacht, leeftijd, emotie, gezichtshaar en bril.

> [!NOTE]
> De face detection-functie is ook beschikbaar via de [Computer Vision-API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home). Als u wilt meer bewerkingen op gegevens van de face-, gebruikt u de Face-API, dit is de service die in dit artikel worden besproken.

![Een installatiekopie van een vrouw en een man, waarbij kleinere rechthoeken getekend rond de gezichten en leeftijd en geslacht weergegeven](./Images/Face.detection.jpg)

Zie voor meer informatie over gezichtsdetectie, de [Gezichtsdetectie](concepts/face-detection.md) concepten artikel. Zie ook de [API detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) verwijzen naar documentatie.

## <a name="face-verification"></a>Gezichtsverificatie

De Verificatie-API voert een verificatie uit op basis van twee gedetecteerde gezichten of op basis van een gedetecteerd gezicht in relatie tot een object van één persoon. Waar het eigenlijk op neerkomt is dat de Verificatie-API evalueert of twee gezichten van dezelfde persoon zijn. Deze mogelijkheid is mogelijk handig in scenario's voor beveiliging. Zie voor meer informatie de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of de [API controleren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) verwijzen naar documentatie.

## <a name="find-similar-faces"></a>Vergelijkbare gezichten zoeken

De vergelijkbare zoeken-API vergelijkt een gezicht doel met een set candidate gezichten vinden van een kleiner aantal gezichten die op het vlak van het doel lijken. Twee werkdagen modi, matchPerson en matchFace, worden ondersteund. De modus matchPerson soortgelijke gezichten geretourneerd nadat deze filters voor dezelfde persoon met behulp van de [API controleren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). De modus matchFace negeert het filter dezelfde persoon. Deze retourneert een lijst van soortgelijke kandidaat gezichten die wel of niet kan behoren tot dezelfde persoon.

Het volgende voorbeeld ziet u het gezicht van doel:

![Een glimlachende vrouw](./Images/FaceFindSimilar.QueryFace.jpg)

En dit zijn de kandidaatgezichten:

![Vijf afbeeldingen van glimlachende mensen Installatiekopieën van een en dezelfde persoon b.](./Images/FaceFindSimilar.Candidates.jpg)

Als u wilt zoeken vier soortgelijke gezichten, de modus matchPerson retourneert een en b, die dezelfde persoon als de doel-face weergeven. De matchFace modus retourneert a, b, c en d, precies vier kandidaten, zelfs als sommige zijn niet dezelfde persoon als het doel of lage vergelijkbaar zijn. Zie voor meer informatie de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of de [vergelijkbare API vinden](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) verwijzen naar documentatie.

## <a name="face-grouping"></a>Gezichtsgroepering

De Groeperings-API verdeelt een set onbekende gezichten in groepen op basis van gelijkenis. Elke groep is een niet-aaneengesloten juiste subset van de oorspronkelijke set van gezichten. Alle van de gezichten in een groep zijn waarschijnlijk te behoren tot dezelfde persoon. Er zijn diverse verschillende groepen voor één persoon. De groepen worden onderscheiden door een andere factoren, zoals expressie, bijvoorbeeld. Zie voor meer informatie de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of de [groep API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) verwijzen naar documentatie.

## <a name="person-identification"></a>Identificatie van personen

De API identificeren wordt gebruikt om een gedetecteerde vlak voor een database van mensen te identificeren. Deze functie kan handig zijn voor automatische afbeeldingen taggen in software voor beheer van foto's. U de database vooraf maken en kunt u deze bewerken na verloop van tijd.

De volgende afbeelding toont een voorbeeld van een database met de naam `"myfriends"`. Elke groep kan maximaal 1 miljoen andere persoonsobjecten bevatten. Voor elk persoonsobject kunnen maximaal 248 gezichten zijn geregistreerd.

![Een raster met drie kolommen voor andere personen, elk met drie rijen van de face-installatiekopieën](./Images/person.group.clare.jpg)

Nadat u maken en trainen van een database, kunt u identificatie op basis van de groep met een nieuwe gedetecteerde gezicht uitvoeren. Als het gezicht wordt geïdentificeerd als een persoon in de groep, wordt het persoonsobject geretourneerd.

Zie voor meer informatie over de identificatie van personen, de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of de [identificeren API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) verwijzen naar documentatie.

## <a name="use-containers"></a>Containers gebruiken

[Gebruik de Face-container](face-how-to-install-containers.md) om te detecteren, herkennen en Identificeer gezichten door het installeren van een gestandaardiseerde Docker-container dichter bij met uw gegevens.

## <a name="sample-apps"></a>Voorbeeld-apps

De volgende voorbeeldtoepassingen weergeven enkele manieren waarop u met de Face-API:

- [Microsoft Face-API: Windows Client-bibliotheek en voorbeeldapp](https://github.com/Microsoft/Cognitive-Face-Windows) is een WPF-app die laat zien van verschillende scenario's voor de detectie, analyse en identificatie Face.
- [FamilyNotes UWP-app](https://github.com/Microsoft/Windows-appsample-familynotes) is een Universal Windows Platform (UWP)-app die gebruikt face-identificatie, samen met de spraak-, Cortana, inkt en camera in een familie scenario voor het delen van de opmerking.

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Net als bij alle van de Cognitive Services-resources, moet ontwikkelaars die gebruikmaken van de Face-service op de hoogte van het beleid van Microsoft op gegevens van de klant. Zie voor meer informatie de [Cognitive Services-pagina](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) op het Microsoft Trust Center.

## <a name="next-steps"></a>Volgende stappen

Ga als volgt een snelstartgids voor het implementeren van een gezichtsherkenning scenario in code:

- [Snelstart: Detecteer gezichten in een afbeelding met behulp van de .NET-SDK met C# ](quickstarts/csharp.md). Andere talen zijn beschikbaar.
