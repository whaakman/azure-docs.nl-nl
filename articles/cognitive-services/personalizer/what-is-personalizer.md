---
title: Wat is Personalizer?
titleSuffix: Azure Cognitive Services
description: Personalizer is een op cloud-gebaseerde API-service waarmee u de beste ervaring om weer te geven aan uw gebruikers, leren van hun realtime gedrag kiezen.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 286a19207236392367b924bea7e26e90fd0db8d5
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253449"
---
# <a name="what-is-personalizer"></a>Wat is Personalizer?

Azure Personalizer is een op de cloud gebaseerde API-service waarmee u de beste ervaring kunt kiezen om aan uw gebruikers te laten zien, waarbij u leert van hun real-time gedrag.

* Bevatten informatie over uw gebruikers en de inhoud en de bovenste actie om weer te geven uw gebruikers ontvangen. 
* U hoeft te reinigen en labelen voordat u Personalizer.
* Feedback aan Personalizer wanneer het u uitkomt. 
* Realtime analytische gegevens bekijken. 
* Personalizer gebruiken als onderdeel van een grotere data science-inspanning om te valideren van bestaande experimenten.

## <a name="how-does-personalizer-work"></a>Hoe werkt Personalizer?

Personalizer maakt gebruik van machine learning-modellen om te ontdekken welke actie moet worden in een context hoogste positie. Uw clienttoepassing bevat een lijst met mogelijke acties, met informatie over deze; en informatie over de context, waaronder informatie over de gebruiker, apparaat, enzovoort. Personalizer bepaalt de actie te ondernemen. Nadat u de clienttoepassing gebruikmaakt van de gekozen actie, biedt feedback voor Personalizer in de vorm van een score van derden. Nadat de feedback is ontvangen, worden de eigen model dat wordt gebruikt voor toekomstige posities Personalizer automatisch bijgewerkt.

## <a name="how-do-i-use-the-personalizer"></a>Hoe gebruik ik de Personalizer?

![Met behulp van Personalizer om te kiezen welke video om weer te geven aan een gebruiker](media/what-is-personalizer/personalizer-example-highlevel.png)

1. Kies een ervaring in uw app om aan te passen.
1. Maken en configureren van een exemplaar van de Service persoonlijke instellingen in de Azure-portal. Elke instantie is een lus Personalizer.
1. Gebruik de SDK om aan te roepen Personalizer met informatie (_functies_) over uw gebruikers en de inhoud (_acties_). U hoeft niet te bieden verwijdert, met het label gegevens voordat u Personalizer. 
1. Weergeven in de clienttoepassing, de gebruiker de actie die door Personalizer is geselecteerd.
1. Gebruik de SDK feedback te geven aan Personalizer waarmee wordt aangegeven als de gebruiker van Personalizer actie geselecteerd. Dit is een _Beloon score_, meestal tussen 1 en 1.
1. Analytics in Azure portal om te evalueren hoe het systeem werkt en hoe uw gegevens helpt persoonlijke instellingen weergeven.

## <a name="where-can-i-use-personalizer"></a>Waar kan ik Personalizer gebruiken?

Uw clienttoepassing kunt bijvoorbeeld Personalizer aan toevoegen:

* Aanpassen wat artikel op een nieuwswebsite is gemarkeerd.    
* Optimaliseer ad plaatsing op een website.
* Een persoonlijke "aanbevolen item" worden weergegeven op de website van een winkelwagen.
* Gebruikersinterface-elementen, zoals filters toe te passen op een specifieke foto voorstellen.
* Kies een chatbot antwoord om te verduidelijken bedoeling van gebruikers of een actie voorstellen.
* Suggesties van wat een gebruiker als de volgende stap in een bedrijfsproces doen moet prioriteren.

## <a name="personalization-for-developers"></a>Persoonlijke instellingen voor ontwikkelaars

Personalizer Service heeft twee API's:

* Verzenden van informatie (_functies_) over uw gebruikers en de inhoud (_acties_) om aan te passen. Personalizer reageert met de eerste actie.
* Feedback verzenden naar Personalizer over hoe goed de rangorde als een getal meestal tussen 0 en 1 gewerkt (de vorige sectie gezegd 1 en 1). 

![Algemene volgorde van gebeurtenissen voor persoonlijke instellingen](media/what-is-personalizer/personalization-intro.png)

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Maken van een feedback-lus inC#](csharp-quickstart-commandline-feedback-loop.md)
* [Gebruik de interactieve demo](https://personalizationdemo.azurewebsites.net/)
