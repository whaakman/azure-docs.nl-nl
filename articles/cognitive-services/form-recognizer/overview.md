---
title: Wat is Form Recognizer?
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van het formulier herkenning formulier-en tabelgegevens parseren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 04/08/2019
ms.author: pafarley
ms.openlocfilehash: 82ee2aa5627ac5fa4584f5af6b6b80cc2813c667
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441840"
---
# <a name="what-is-form-recognizer"></a>Wat is Form Recognizer?

Azure Form Recognizer is een Cognitive Service-dienst die gebruikmaakt van machine learning-technologie om sleutel-waardeparen en tabelgegevens uit formulierdocumenten te identificeren en te extraheren. Vervolgens voert het gestructureerde gegevens uit die ook de relaties uit het oorspronkelijke bestand omvatten. U kunt het model van uw aangepaste formulier herkenning aanroepen met behulp van een eenvoudige REST-API voor minder complexiteit en eenvoudig integreren in uw werkstroom of toepassing. Als u wilt beginnen, hoeft u maar vijf ingevuld formulier documenten of twee ingevuld in formulieren en een leeg formulier van hetzelfde type als uw invoer materiaal. Snel krijgt u nauwkeurige resultaten die zijn afgestemd op uw specifieke inhoud zonder zware handmatige tussenkomst of uitgebreide data science expertise.

## <a name="request-access"></a>Toegang aanvragen
Herkenning van formulier is beschikbaar in het voorbeeld van een beperkte toegang. Voor toegang tot de Preview-versie, invullen en verzenden de [formulier herkenning toegangsaanvraag](https://aka.ms/FormRecognizerRequestAccess) formulier. Het formulier vraagt om informatie over u, uw bedrijf en het scenario voor gebruikers waarvoor u formulier herkenning gebruikt. Als uw aanvraag is goedgekeurd door het team van Azure Cognitive Services, ontvangt u een e-mailbericht met instructies voor het openen van de service.

## <a name="what-it-does"></a>Wat het doet

Wanneer u uw gegevens verzendt, wordt het algoritme traint, clusters de formulieren door te typen, detecteert welke sleutels en tabellen aanwezig zijn, en leert om waarden voor sleutels en vermeldingen naar tabellen te koppelen. Leerprocessen zonder supervisie stellen het model in staat om de lay-out en relaties tussen velden en items te begrijpen zonder handmatige labeling van gegevens of intensieve codering en onderhoud. Daarentegen, vooraf getrainde machine learning-modellen gestandaardiseerde gegevens vereist en zijn minder nauwkeurig gebruikt in combinatie met invoer materiaal dat van traditionele indelingen, zoals branchespecifieke formulieren afwijkt.

Nadat u het model te trainen, kunt u testen en het opnieuw trainen en het uiteindelijk gebruiken op een betrouwbare manier om gegevens te extraheren uit meer formulieren op basis van uw behoeften.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator

Herkenning van formulier is beschikbaar als een REST-API. U kunt maken, trainen en te beoordelen van een model door de API aan te roepen. Als u wilt, kunt u het model kunt uitvoeren in een lokale Docker-container.

## <a name="input-requirements"></a>Vereisten voor invoer

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

**Stap 1:** Een formulier herkenning-resource maken in Azure portal.

**Stap 2:** Raadpleeg een snelstartgids voor praktische ervaring:
* [Snelstart: Een formulier herkenning-model te trainen en ophalen van gegevens met behulp van de REST-API met cURL](quickstarts/curl-train-extract.md)
* [Snelstart: Een formulier herkenning-model te trainen en gegevens ophalen met behulp van de REST-API met Python](quickstarts/python-train-extract.md)

U wordt aangeraden dat u de gratis service gebruiken wanneer u bij het leren van de technologie, maar houd er rekening mee dat het aantal gratis pagina's beperkt tot 500 pagina's per maand is.

**Stap 3:** Controleer de REST API 's

Gebruik de volgende API's te trainen en extraheer gestructureerde gegevens uit formulieren.

| REST-API | Description |
|-----|-------------|
| Trainen | Een nieuw model voor het analyseren van uw formulieren met behulp van vijf vormen van hetzelfde type trainen. Of trainen met een leeg formulier en twee ingevuld in formulieren.  |
| Analyseren  |Analyseer één document doorgegeven als een stroom om op te halen van sleutel / waarde-paren en tabellen in het formulier met uw aangepaste model.  |

Verken de [REST-API-verwijzing document](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Deze service wordt aangeboden als een [preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) van een Azure-service onder de [voorwaarden voor Online-Service](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Net als bij de cognitive services, moeten de ontwikkelaars die gebruikmaken van de service formulier herkenning rekening houden met Microsoft-beleid op gegevens van de klant. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voer een [snelstartgids](quickstarts/curl-train-extract.md) aan de slag met de [Form-herkenning API's](https://aka.ms/form-recognizer/api).
