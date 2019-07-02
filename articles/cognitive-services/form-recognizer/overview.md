---
title: Wat is Form Recognizer?
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van het formulier herkenning formulier-en tabelgegevens parseren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: a17e47fb059c23ab2e6eb69f3cfe6f2f8d0e51b2
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502908"
---
# <a name="what-is-form-recognizer"></a>Wat is Form Recognizer?

Herkenning van Azure formulier is een cognitive service die gebruikmaakt van machine learning-technologie om te bepalen en de sleutel/waarde-paren en tabelgegevens extraheren uit de form-documenten. Vervolgens voert het gestructureerde gegevens uit die ook de relaties uit het oorspronkelijke bestand omvatten. U kunt het model van uw aangepaste formulier herkenning aanroepen met behulp van een eenvoudige REST-API voor minder complexiteit en eenvoudig integreren in uw werkstroom of toepassing. Als u wilt beginnen, hoeft u maar vijf ingevuld formulier documenten of twee ingevuld in formulieren en een leeg formulier van hetzelfde type als uw invoer materiaal. Snel krijgt u nauwkeurige resultaten die zijn afgestemd op uw specifieke inhoud zonder zware handmatige tussenkomst of uitgebreide data science expertise.

## <a name="custom-models"></a>Aangepaste modellen

Het aangepaste formulier herkenning-model traint in uw eigen gegevens en u hoeft alleen vijf voorbeeld invoer formulieren om te starten. Wanneer u uw gegevens verzendt, het algoritme is, wordt de formulieren per type clusters, detecteert welke sleutels en tabellen aanwezig zijn en koppelt u waarden voor sleutels en vermeldingen naar tabellen. Vervolgens voert het gestructureerde gegevens uit die ook de relaties uit het oorspronkelijke bestand omvatten. Nadat u het model te trainen, kunt u testen en het opnieuw trainen en het uiteindelijk gebruiken op een betrouwbare manier om gegevens te extraheren uit meer formulieren op basis van uw behoeften.

Leerprocessen zonder supervisie stellen het model in staat om de lay-out en relaties tussen velden en items te begrijpen zonder handmatige labeling van gegevens of intensieve codering en onderhoud. Daarentegen, nodig vooraf getrainde machine learning-modellen gestandaardiseerde gegevens. Ze zijn minder nauwkeurig met invoer materiaal dat van traditionele indelingen, zoals branchespecifieke formulieren afwijkt.

## <a name="pre-built-receipt-model"></a>Vooraf gemaakte ontvangst-model

Herkenning van formulier bevat ook een model voor het lezen van genereert. Dit model extraheert belangrijke informatie zoals de tijd en datum van de transactie, zakelijke gegevens, hoeveelheden belastingen en totalen en meer. Bovendien wordt de vooraf gemaakte ontvangsten-model getraind en alle tekst op een ontvangstbevestiging retourneren.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator

Herkenning van formulier is beschikbaar als een REST-API. U kunt maken, trainen, en een aangepast model te beoordelen of toegang krijgen tot de vooraf gebouwde model door deze API's aan te roepen. Als u wilt, kunt u deze kunt trainen en aangepaste modellen uitvoeren in een lokale Docker-container.

## <a name="input-requirements-custom-model"></a>Vereisten voor invoer (aangepaste model)

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="request-access"></a>Toegang aanvragen

Herkenning van formulier is beschikbaar in het voorbeeld van een beperkte toegang. Voor toegang tot de Preview-versie, invullen en verzenden de [formulier herkenning toegangsaanvraag](https://aka.ms/FormRecognizerRequestAccess) formulier. Het formulier vraagt om informatie over u, uw bedrijf en het scenario voor gebruikers waarvoor u formulier herkenning gebruikt. Als uw aanvraag is goedgekeurd door het team van Azure Cognitive Services, ontvangt u een e-mailbericht met instructies voor het openen van de service.

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

**Stap 1:** Een formulier herkenning-resource maken in Azure portal.

**Stap 2:** Ga als volgt een snelstartgids voor het gebruik van de REST-API:
* [Snelstart: Een formulier herkenning-model te trainen en ophalen van gegevens met behulp van de REST-API met cURL](quickstarts/curl-train-extract.md)
* [Snelstart: Een formulier herkenning-model te trainen en gegevens ophalen met behulp van de REST-API met Python](quickstarts/python-train-extract.md)
* [Snelstart: Ontvangst-gegevens met cURL ophalen](quickstarts/curl-receipts.md)
* [Snelstart: Ontvangst-gegevens met behulp van Python ophalen](quickstarts/python-receipts.md)

U wordt aangeraden dat u de gratis service gebruiken wanneer u bij het leren van de technologie. Houd er rekening mee dat het aantal gratis pagina's beperkt tot 500 per maand is.

**Stap 3:** Controleer de REST API 's

U de volgende API's gebruiken om te trainen en extraheer gestructureerde gegevens uit formulieren.

|||
|---|---|
| Model trainen| Een nieuw model voor het analyseren van uw formulieren met behulp van vijf vormen van hetzelfde type trainen. Of trainen met een leeg formulier en twee ingevuld in formulieren.  |
| Formulier analyseren |Analyseer één document doorgegeven als een stroom om op te halen van sleutel/waarde-paren en tabellen in het formulier met uw aangepaste model.  |
| Analyseren van ontvangst |Een ontvangstdocument één om uit te pakken belangrijke informatie en andere ontvangstbewijstekst van de te analyseren.|

Verken de [REST API-referentiedocumentatie](https://aka.ms/form-recognizer/api) voor meer informatie. 

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Deze service wordt aangeboden als een [preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) van een Azure-service onder de [voorwaarden voor Online-Service](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Net als bij de cognitive services, moeten de ontwikkelaars die gebruikmaken van de service formulier herkenning rekening houden met Microsoft-beleid op gegevens van de klant. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voer een [snelstartgids](quickstarts/curl-train-extract.md) aan de slag met de [Form-herkenning API's](https://aka.ms/form-recognizer/api).
