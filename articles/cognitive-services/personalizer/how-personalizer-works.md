---
title: How Personalizer Works - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer maakt gebruik van machine learning om te ontdekken welke actie moet worden gebruikt in een context. Elke lus learning is een model dat uitsluitend op gegevens die u hebt verzonden naar het via de positie en aanroepen van derden wordt getraind. Elke lus learning is volledig onafhankelijk van elkaar.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 6b2237f27fba5eaf952932cd6592052649400b96
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027129"
---
# <a name="how-personalizer-works"></a>De werking van Personalizer

Personalizer maakt gebruik van machine learning om te ontdekken welke actie moet worden gebruikt in een context. Elke lus learning is een model dat is getraind op gegevens die u hebt verzonden naar via uitsluitend op **positie** en **beloning** aanroepen. Elke lus learning is volledig onafhankelijk van elkaar. Maak een lus learning voor elk onderdeel of het gedrag van uw toepassing die u wilt aanpassen.

Voor elke lus **aanroepen van de positie-API met** op basis van de huidige context met:

* Lijst met mogelijke acties: inhoud van items uit die bovenste actie selecteren.
* Lijst met [context functies](concepts-features.md): contextueel relevante gegevens, zoals gebruikers, inhoud en context.

De **positie** API wil gebruiken:

* _Exploit_: Het huidige model om te bepalen van de actie die het beste op basis van oude gegevens.
* _Verken_: Selecteer een andere actie in plaats van de eerste actie.

De **beloning** API:

* Verzamelt gegevens voor het model te trainen door de functies en scores van de prijs van elke aanroep van de positie op te nemen.
* Maakt gebruik van die gegevens bijwerken van het model op basis van de instellingen die zijn opgegeven de _Learning beleid_.

## <a name="architecture"></a>Architectuur

De volgende afbeelding toont de architectuur stroom van het aanroepen van de positie en beloon aanroepen:

![alternatieve tekst](./media/how-personalizer-works/personalization-how-it-works.png "hoe personalisatie werkt")

1. Personalizer maakt gebruik van een interne AI-model om te bepalen de positie van de actie.
1. De service besluit of misbruik te maken van het huidige model of nieuwe keuzes voor het model te verkennen.  
1. Het resultaat van de classificatie is verzonden naar Event hub.
1. Wanneer Personalizer de vergoeding ontvangt, wordt de prijs verzonden naar Event hub. 
1. De positie en beloon worden gecorreleerd.
1. Het AI-model wordt bijgewerkt op basis van de correlatie-resultaten.
1. De engine Deductie wordt bijgewerkt met het nieuwe model. 

## <a name="research-behind-personalizer"></a>Onderzoek achter Personalizer

Personalizer is gebaseerd op geavanceerde science en onderzoek op het gebied van [Bekrachtigingen](concepts-reinforcement-learning.md) inclusief documenten, onderzoek en doorlopende gebieden van verkennen in Microsoft Research.

## <a name="terminology"></a>Terminologie

* **Learning-lus**: U kunt een lus learning maken voor elk onderdeel van uw toepassing die u van persoonlijke instellingen profiteren kunt. Als u meer dan één ervaring aanpassen hebt, maakt u een lus voor elke. 

* **Acties**: Acties zijn de inhoud items, zoals producten of -aanbiedingen, waaruit u kunt kiezen. Personalizer kiest de bovenste actie om weer te geven aan uw gebruikers bekend als de _Beloon actie_, via de API positie. Elke actie kan functies die worden verzonden met de positie aanvraag hebben.

* **Context**: Bevatten informatie over de context, bijvoorbeeld voor een meer nauwkeurige positie:
    * De gebruiker.
    * Het apparaat dat ze zijn verbonden. 
    * De huidige tijd.
    * Andere gegevens over de huidige situatie.
    * Historische gegevens over de gebruiker of een context.

    Uw specifieke toepassing bevat mogelijk andere contextinformatie. 

* **[Functies](concepts-features.md)**: Een eenheid met gegevens over een item voor inhoud of een gebruikerscontext.

* **Beloning**: Een meting van hoe de gebruiker wordt gereageerd op de positie API actie, als een score tussen 0 en 1 geretourneerd. De waarde van 0 tot en met 1 wordt ingesteld door uw zakelijke logica, op basis van hoe de keuze geholpen bij het bereiken van de uw doelstellingen van persoonlijke instellingen. 

* **Verkenning**: De service Personalizer onderzoekt wanneer in plaats van de aanbevolen actie wordt geretourneerd, een andere actie voor de gebruiker worden gekozen. De service Personalizer voorkomt drift, stagnatie, en kan worden aangepast aan de lopende gebruikersgedrag door te verkennen. 

* **Duur van experiment**: De hoeveelheid tijd die de Personalizer-service moet voor een beloning wachten, starten vanaf het moment dat de aanroep van de positie is er gebeurd voor die gebeurtenis.

* **Niet-actieve gebeurtenissen**: Een inactieve gebeurtenis is een waar u positie aangeroepen, maar u niet zeker weet dat de gebruiker ziet het resultaat, vanwege client toepassing besluiten ooit. Niet-actieve gebeurtenissen kunnen u maken en opslaan van resultaten van persoonlijke instellingen en vervolgens wilt deze later verwijderen zonder enige impact op de machine learning-model.

* **Model**: Een model Personalizer bevat alle gegevens hebt geleerd over gebruikersgedrag, trainingsgegevens ophalen uit een combinatie van de argumenten die u verzendt voor positie en beloon en met een bepaald door het beleid Learning training-gedrag. 

## <a name="next-steps"></a>Volgende stappen

Inzicht in [waar kunt u Personalizer](where-can-you-use-personalizer.md).