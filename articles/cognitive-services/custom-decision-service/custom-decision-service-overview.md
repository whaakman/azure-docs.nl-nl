---
title: Wat is aangepast besluit Service? -Azure-cognitieve Services | Microsoft Docs
description: Dit artikel overzichten Azure aangepaste besluit Service een cloud-gebaseerde API voor de contextuele besluitvorming die ervaring biedt.
services: cognitive-services
author: alekh
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh;marossi
ms.openlocfilehash: 774467446513dcd7ade7255d998b11f41824cafe
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "35350016"
---
# <a name="what-is-custom-decision-service"></a>Wat is aangepast besluit Service?

In een typische web of mobiele toepassing, een front-pagina bevat koppelingen naar verschillende artikelen of andere typen inhoud. Als de voorpagina wordt geladen, kan deze de waarden van positie artikelen die zijn opgenomen op die pagina front-Service besluit aangepaste aanvragen. Dus als een gebruiker een artikel door erop te klikken, kan een tweede aanvraag kan worden verzonden naar de aangepaste besluit-Service, die het resultaat van de keuze van die gebruiker, worden geregistreerd.

Aangepaste besluit-Service is eenvoudig te gebruiken, omdat hiervoor alleen een RSS-feed voor uw inhoud en een paar regels JavaScript, kunnen worden toegevoegd aan uw toepassing.

De inhoud van uw converteert aangepaste besluit Service naar functies voor machine learning. Het systeem gebruikt deze functies om te begrijpen van uw inhoud in termen van de tekst, afbeeldingen, video's en algemene gevoel. Dit maakt gebruik van verschillende andere [cognitieve Microsoft-Services](https://www.microsoft.com/cognitive-services), bijvoorbeeld [entiteit koppelen](../entitylinking/home.md), [Tekstanalyse](../text-analytics/overview.md), [Emotion](../emotion/home.md), en [Computer Vision](../computer-vision/home.md).

Er zijn enkele algemene gebruiksvoorbeelden voor aangepaste besluit Service:

* Personaliseren artikelen op een nieuwswebsite
* Video-inhoud op een medium portal aanpassen
* Ad plaatsingen of webpagina's waarmee de advertentie optimaliseren
* Rangschikking aanbevolen items op de website van een webwinkel.

Aangepaste besluit-Service is momenteel in *gratis openbare preview*. Deze kunt een lijst met artikelen op een website of een app aanpassen. Het uitpakken van de functie werkt het beste voor Engelstalige inhoud. [Beperkte functionaliteit](../text-analytics/overview.md) wordt geboden voor andere talen, zoals Spaans, Frans, Duits, Portugees en Japans. Deze documentatie worden herzien zodra er nieuwe functionaliteit beschikbaar.

Aangepaste besluit-Service kan worden gebruikt in toepassingen die zich niet in het domein van de inhoud aanpassen. Deze toepassingen mogelijk geschikt voor een aangepaste preview. [Neem contact met ons](https://azure.microsoft.com/overview/sales-number/) voor meer informatie.

## <a name="api-usage-modes"></a>API-gebruik modi

Aangepaste besluit-Service kan worden toegepast op webpagina's en de mobiele apps. De API's kan worden aangeroepen vanuit een browser of een app. Het gebruik van de API lijkt op beide, maar sommige van de details zijn verschillend.

## <a name="glossary-of-terms"></a>Verklarende woordenlijst

Verschillende voorwaarden vaak optreden in deze documentatie:

* **Actie set**: de reeks inhoudsitems voor aangepaste besluit Service wilt rangschikken. Deze set kan worden opgegeven als een *RSS* of *Atom* eindpunt.
* **Rangschikking**: elke aanvraag met aangepaste besluit Service bevat een of meer sets van de actie. Het systeem door het verzamelen van alle inhoud opties uit deze sets reageert en vrijgegeven, gaan ze in gerangschikte volgorde.
* **Callback-functie**: deze functie, die u opgeeft, wordt de inhoud in de gebruikersinterface weergegeven. De inhoud is geordend op de positie ordening door aangepaste besluit Service geretourneerd.
* **Derden**: een meting van hoe de gebruiker heeft geantwoord op de gerenderde inhoud. Aangepaste besluit Service meet antwoord van de gebruiker met behulp van klikken. De klikt op worden gerapporteerd aan het systeem met behulp van aangepaste code ingevoegd in uw toepassing.

## <a name="next-steps"></a>Volgende stappen

* [Uw toepassing registreren](custom-decision-service-get-started-register.md) met aangepaste besluit Service
* Aan de slag te optimaliseren [een webpagina](custom-decision-service-get-started-browser.md) of [een smartphone-app](custom-decision-service-get-started-app.md).
* Raadpleeg de [API-referentiemateriaal](custom-decision-service-api-reference.md) voor meer informatie over de functionaliteit voor opgegeven.