---
title: Evaluatie van de scenario - Personalizer
titleSuffix: Azure Cognitive Services
description: 'Personalizer kan worden toegepast in een situatie waarin uw toepassing selecteren kunt de juiste item, de actie of het product om weer te geven: om te kunnen verbeteren de ervaring en bereiken van betere bedrijfsresultaten te verbeteren en de productiviteit te verbeteren.'
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: b24e3d7fd71ef60cf8ebe5ba2c33889ff518580c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027108"
---
# <a name="where-can-you-use-personalizer"></a>Waar gebruikt u Personalizer?

Gebruik Personalizer in een situatie waarin de toepassing nodig heeft om de juiste item, actie of product om weer te geven - om de ervaring verbeteren bereiken van betere bedrijfsresultaten te verbeteren, of de productiviteit te verbeteren. 

Personalizer gebruikt machine learning om te selecteren welke actie om de gebruiker weer te geven. De selectie kan aanzienlijk variëren afhankelijk van het aantal, de kwaliteit en de verdeling van gegevens naar de service verzonden.

### <a name="checklist-for-applying-personalizer"></a>Controlelijst voor het toepassen van Personalizer


U kunt Personalizer toepassen in situaties waar:

* U hebt een bedrijf of de bruikbaarheid doel voor uw toepassing.
* U hebt een centrale locatie in uw toepassing waar u een contextuele beslissing over welke om weer te geven aan gebruikers die doel wordt verbeterd.
* De beste keuze kan en moet worden geleerd van collectieve gebruiker gedrag en de totale prijs score.
* Het gebruik van machine learning voor persoonlijke instellingen volgt [verantwoordelijk gebruik richtlijnen](ethics-responsible-use.md) en opties die u hebt gekozen.
* De contextuele beslissing kan worden uitgedrukt als de positie van de beste optie (actie) van een beperkt aantal mogelijkheden.
* Hoe goed de gerangschikte keuze gewerkt voor uw toepassing kan worden bepaald door het meten van een bepaald aspect van gebruikersgedrag en uitdrukken in een _Beloon score_. Dit is een getal tussen 1 en 1.
* De score beloning doen niet in te veel vertekening of externe factoren. De duur van experiment is laag genoeg is dat de score van derden kunnen worden berekend terwijl deze wel relevant is.
* U kunt de context voor de positie als een lijst met ten minste 5 express [functies](concepts-features.md) waarvan u denkt dat de juiste keuze wilt helpen en die bevat geen persoonlijke gegevens. (PII).
* Hebt u meer informatie over elke inhoud keuze _actie_, als een lijst met ten minste 5 [functies](concepts-features.md) waarvan u denkt dat help Personalizer maakt u de juiste keuze.
* Uw toepassing kan gegevens bewaren lang genoeg is voor het verzamelen van een geschiedenis van ten minste 100.000 interacties.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Machine learning-overwegingen voor het toepassen van Personalizer

Personalizer is gebaseerd op versterkend leren, een benadering voor het machine learning-dat wil zeggen en onderwezen door feedback geeft u deze. 

Personalizer leert geschikt in situaties waar:

* Er is onvoldoende gebeurtenissen op de hoogte van optimale persoonlijke instellingen als het probleem drifts na verloop van tijd (zoals voorkeuren in nieuws of Mode). Personalizer continue wijziging in de praktijk wordt aangepast, maar resultaten niet optimaal als er niet voldoende gegevens om te leren uit om te detecteren en te betalen op nieuwe patronen over gebeurtenissen en. U moet een use-case die vaak genoeg gebeurt. Kunt u op zoek naar use-cases die ten minste 500 keer per dag plaatsvinden.
* Context en acties voldoende [functies](concepts-features.md) om learning mogelijk te maken.
* Er zijn minder dan 50 acties wilt rangschikken per aanroep.
* Uw instellingen voor het bewaren van gegevens kunnen Personalizer voor het verzamelen van voldoende gegevens om uit te voeren offline evaluaties en beleid voor optimalisatie. Dit is meestal ten minste 50.000 gegevenspunten.

## <a name="monitor-effectiveness-of-personalizer"></a>Monitor effectiviteit van Personalizer

U kunt de effectiviteit van Personalizer periodiek controleren door te voeren [offline evaluaties](concepts-offline-evaluation.md).

## <a name="use-personalizer-with-recommendation-engines"></a>Gebruik Personalizer met aanbevelingsengines

Veel bedrijven gebruiken aanbevelingsengines, hulpprogramma's voor marketing en campaigning doelgroep Segmentatie en clustering, gezamenlijke filterfuncties en andere manier om aan te bevelen producten uit een grote catalogus aan klanten.

De [Microsoft Recommenders GitHub-opslagplaats ](https://github.com/Microsoft/Recommenders) bevat voorbeelden en aanbevolen procedures voor het bouwen van aanbevelingssystemen, opgegeven als Jupyter-notebooks. Het biedt werkende voorbeelden voor het voorbereiden van gegevens, het ontwikkelen van modellen, evalueren, afstemmen en tot het operationaliseren van de aanbevelingsengines, voor veel algemene benaderingen xDeepFM, inclusief SAR, ALS, RBM, DKN.

Personalizer kunt werken met een engine voor aanbevelingen als deze aanwezig is.

* Aanbevelingsengines nemen van grote hoeveelheden items (bijvoorbeeld 500.000) en een subset zijn (zoals de top 20) uit honderden of duizenden opties aanbevolen.
* Personalizer neemt een klein aantal acties met veel informatie over deze en in realtime voor een bepaalde uitgebreide context ranks, hoewel de meeste aanbevelingsengines alleen enkele kenmerken voor gebruikers, producten en hun interacties gebruiken.
* Personalizer is ontworpen om te verkennen autonoom gebruikersvoorkeuren voortdurend, waardoor er betere resultaten wanneer inhoud wordt gewijzigd snel, zoals nieuws, live-evenementen, live community-inhoud of inhoud met dagelijkse updates seizoensgebonden inhoud.

De uitvoer van een engine voor aanbevelingen (bijvoorbeeld de 20 populairste producten voor een bepaalde klant) en gebruiken als de invoer acties voor Personalizer wordt vaak gebruikt.

## <a name="next-steps"></a>Volgende stappen

[Ethische & verantwoordelijk gebruik](ethics-responsible-use.md).