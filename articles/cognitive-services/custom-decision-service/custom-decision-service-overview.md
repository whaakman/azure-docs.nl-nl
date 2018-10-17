---
title: Wat is Custom Decision Service?
titlesuffix: Azure Cognitive Services
description: In dit artikel vindt u een overzicht van Custom Decision Service.
services: cognitive-services
author: alekh
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-decision-service
ms.topic: overview
ms.date: 05/08/2018
ms.author: slivkins
ms.openlocfilehash: 273f2965a0fcaaa729175c5232da1aba69589eec
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364203"
---
# <a name="what-is-custom-decision-service"></a>Wat is Custom Decision Service?

In een gewone web- of mobiele toepassing wordt de eerste pagina gekoppeld aan verschillende artikelen of andere typen inhoud. Als de eerste pagina wordt geladen, kan Custom Decision Service worden gevraagd artikelen op die pagina te rangschikken. Wanneer een gebruiker een artikel kiest door erop te klikken, kan er een tweede aanvraag naar Custom Decision Service worden verzonden waarmee het resultaat van die keuze van de gebruiker wordt geregistreerd.

Custom Decision Service is eenvoudig te gebruiken, aangezien hiervoor alleen een RSS-feed voor uw inhoud nodig is en er een paar regels JavaScript in uw toepassing moeten worden toegevoegd.

Uw inhoud wordt in Custom Decision Service geconverteerd naar functies voor machine learning. Het systeem gebruikt deze functies om uw inhoud in termen van de tekst, afbeeldingen, video's en algeheel gevoel te begrijpen. Ook worden er diverse andere [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services) gebruikt, zoals [Entiteitskoppeling](../entitylinking/home.md), [Text Analytics](../text-analytics/overview.md), [Emotion](../emotion/home.md) en [Computer Vision](../computer-vision/home.md).

Enkele algemene gebruiksvoorbeelden voor Custom Decision Service zijn:

* Artikelen op een nieuwswebsite aan de persoonlijke voorkeur aanpassen
* Video-inhoud in een media-portal aan de persoonlijke voorkeur aanpassen
* Advertenties of webpagina's waar de advertenties naartoe leiden, optimaliseren
* Aanbevolen artikelen op een winkelwebsite rangschikken.

Custom Decision Service is momenteel als *openbare preview* beschikbaar. U kunt hiermee een lijst met artikelen op een website of in een app aan de persoonlijke voorkeur aanpassen. Het ophalen van de functie werkt het beste voor Engelstalige inhoud. Voor andere talen, zoals Spaans, Frans, Duits, Portugees en Japans, is de [functionaliteit beperkt](../text-analytics/overview.md). Als er nieuwe functionaliteit beschikbaar komt, wordt deze documentatie bijgewerkt.

Custom Decision Service kan worden gebruikt in toepassingen die zich niet in het domein voor inhoud met persoonlijke aanpassingen bevinden. Deze toepassingen zijn mogelijk geschikt voor een aangepaste preview. [Neem contact met ons op](https://azure.microsoft.com/overview/sales-number/) voor meer informatie.

## <a name="api-usage-modes"></a>Modi voor API-gebruik

Custom Decision Service kan worden toegepast op zowel webpagina's als mobiele apps. De API's kunnen worden aangeroepen vanuit een browser of een app. Het gebruik van de API is vergelijkbaar bij beide, maar sommige details verschillen.

## <a name="glossary-of-terms"></a>Verklarende woordenlijst

In deze documentatie komen bepaalde termen vaak voor:

* **Actieset**: de set met inhoudsitems voor Custom Decision Service die moeten worden gerangschikt. Deze set kan worden opgegeven als een *RSS*- of *Atom*-eindpunt.
* **Rangschikking**: bij elke aanvraag voor Custom Decision Service worden een of meer actiesets opgegeven. Het systeem reageert door alle inhoudsopties uit deze sets te kiezen en ze in gerangschikte volgorde te retourneren.
* **Callback-functie**: met deze functie, die u opgeeft, wordt de inhoud in uw gebruikersinterface weergegeven. De inhoud wordt geordend volgens de rangschikking die wordt geretourneerd door Custom Decision Service.
* **Beloning**: een meting van de manier waarop de gebruiker op de weergegeven inhoud heeft gereageerd. In Custom Decision Service worden reacties van gebruikers gemeten met klikken. De klikken worden gerapporteerd aan het systeem met behulp van aangepaste code die is ingevoegd in uw toepassing.

## <a name="next-steps"></a>Volgende stappen

* [Uw toepassing registreren](custom-decision-service-get-started-register.md) met Custom Decision Service
* Ga aan de slag met het optimaliseren van [een webpagina](custom-decision-service-get-started-browser.md) of [een smartphone-app](custom-decision-service-get-started-app.md).
* Raadpleeg de [API-naslag](custom-decision-service-api-reference.md) voor meer informatie over de geleverde functionaliteit.