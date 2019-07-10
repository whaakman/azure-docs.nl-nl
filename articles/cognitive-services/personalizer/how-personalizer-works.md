---
title: How Personalizer Works - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer maakt gebruik van machine learning om te ontdekken welke actie moet worden gebruikt in een context. Elke lus learning is een model dat uitsluitend op gegevens die u hebt verzonden naar het via de positie en aanroepen van derden wordt getraind. Elke lus learning is volledig onafhankelijk van elkaar.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: edjez
ms.openlocfilehash: 38480d3cc32d53084b79af627e4f7e6ae7dcc03d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722358"
---
# <a name="how-personalizer-works"></a>Hoe Personalizer werkt

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

* **[Functies](concepts-features.md)** : Een eenheid met gegevens over een item voor inhoud of een gebruikerscontext.

* **Beloning**: Een meting van hoe de gebruiker wordt gereageerd op de positie API actie, als een score tussen 0 en 1 geretourneerd. De waarde van 0 tot en met 1 wordt ingesteld door uw zakelijke logica, op basis van hoe de keuze geholpen bij het bereiken van de uw doelstellingen van persoonlijke instellingen. 

* **Verkenning**: De service Personalizer onderzoekt wanneer in plaats van de aanbevolen actie wordt geretourneerd, een andere actie voor de gebruiker worden gekozen. De service Personalizer voorkomt drift, stagnatie, en kan worden aangepast aan de lopende gebruikersgedrag door te verkennen. 

* **Duur van experiment**: De hoeveelheid tijd die de Personalizer-service moet voor een beloning wachten, starten vanaf het moment dat de aanroep van de positie is er gebeurd voor die gebeurtenis.

* **Niet-actieve gebeurtenissen**: Een inactieve gebeurtenis is een waar u positie aangeroepen, maar u niet zeker weet dat de gebruiker ziet het resultaat, vanwege client toepassing besluiten ooit. Niet-actieve gebeurtenissen kunnen u maken en opslaan van resultaten van persoonlijke instellingen en vervolgens wilt deze later verwijderen zonder enige impact op de machine learning-model.

* **Model**: Een model Personalizer bevat alle gegevens hebt geleerd over gebruikersgedrag, trainingsgegevens ophalen uit een combinatie van de argumenten die u verzendt voor positie en beloon en met een bepaald door het beleid Learning training-gedrag. 

## <a name="example-use-cases-for-personalizer"></a>Voorbeeld van de use cases voor Personalizer

* Uitleg van intentie & ondubbelzinnigheid: help uw gebruikers een betere ervaring hebben wanneer de bedoeling is niet gewist door te geven van een optie die is afgestemd op elke gebruiker.
* Suggesties voor en opties van menu's standaard: zijn de bot de meest waarschijnlijke item in een persoonlijke manier voorstellen als een eerste stap, in plaats van een onpersoonlijke menu of een lijst met alternatieven wordt gepresenteerd.
* Eigenschappen van de bot & toon: voor bots die verschillen kan toon, uitgebreid en schrijfstijl, kunt u overwegen deze kenmerken in een persoonlijke manieren variëren.
* Inhoud melding & Waarschuwing: bepalen welke tekst moet worden gebruikt voor waarschuwingen om meer gebruikers actief betrekken.
* Melding & waarschuwing timing: gepersonaliseerde leren van wanneer meldingen verzenden naar gebruikers contact opnemen met deze informatie.

## <a name="checklist-for-applying-personalizer"></a>Controlelijst voor het toepassen van Personalizer

U kunt Personalizer toepassen in situaties waar:

* U hebt een bedrijf of de bruikbaarheid doel voor uw toepassing.
* U hebt een centrale locatie in uw toepassing waar u een contextuele beslissing over welke om weer te geven aan gebruikers die doel wordt verbeterd.
* De beste keuze kan en moet worden geleerd van collectieve gebruiker gedrag en de totale prijs score.
* Het gebruik van machine learning voor persoonlijke instellingen volgt [verantwoordelijk gebruik richtlijnen](ethics-responsible-use.md) en opties voor uw team.
* De beslissing kan worden uitgedrukt als de volgorde van de beste optie ([actie](concepts-features.md#actions-represent-a-list-of-options) van een beperkt aantal mogelijkheden.
* Hoe goed deze keuze gewerkt wordt berekend door uw zakelijke logica, door het meten van een bepaald aspect van gebruikersgedrag en uit te drukken in een getal tussen 1 en 1.
* De score beloning niet plaatsen in te veel vertekening of externe factoren, specifiek de duur van het experiment is laag genoeg is dat de score van derden kunnen worden berekend terwijl deze wel relevant is.
* Als een woordenlijst met ten minste 5 functies die u wilt helpen de juiste keuze en die niet persoonlijk identificeerbare informatie bevatten, kunt u de context voor de rang uitdrukken.
* Hebt u meer informatie over elke actie als wanneer u een woordenlijst van ten minste 5 kenmerken of functies die u denkt dat de juiste keuze Personalizer helpt.
* Kunt u gegevens bewaren lang genoeg is voor het verzamelen van een geschiedenis van ten minste 100.000 interacties.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Machine learning-overwegingen voor het toepassen van Personalizer

Personalizer is gebaseerd op versterkend leren, een benadering tot machine learning die opgehaald en onderwezen door feedback geeft u deze. 

Personalizer leert geschikt in situaties waar:
* Er is onvoldoende gebeurtenissen op de hoogte van optimale persoonlijke instellingen als het probleem drifts na verloop van tijd (zoals voorkeuren in nieuws of Mode). Personalizer continue wijziging in de praktijk wordt aangepast, maar resultaten niet optimaal als er niet voldoende gegevens om te leren uit om te detecteren en te betalen op nieuwe patronen over gebeurtenissen en. U moet een use-case die vaak genoeg gebeurt. Kunt u op zoek naar use-cases die ten minste 500 keer per dag plaatsvinden.
* Context en acties hebt voldoende functies om learning mogelijk te maken.
* Er zijn minder dan 50 acties voor positie per aanroep.
* Uw instellingen voor het bewaren van gegevens kunnen Personalizer voor het verzamelen van voldoende gegevens om uit te voeren offline evaluaties en beleid voor optimalisatie. Dit is meestal ten minste 50.000 gegevenspunten.

## <a name="how-to-use-personalizer-in-a-web-application"></a>Het gebruik van Personalizer in een webtoepassing

Een lus toe te voegen aan een web-App bevat:

* Bepalen welke ervaring aanpassen, welke acties en de functies hebt, welke functies context moet worden gebruikt, en welke beloning die u hebt ingesteld.
* Voeg een verwijzing naar de SDK persoonlijke instellingen in uw toepassing.
* De positie-API aanroepen wanneer u bent klaar om aan te passen.
* De gebeurtenis-id Store. U verzendt een beloning met de API van derden later opnieuw.
1. Activeren-aanroep voor de gebeurtenis als u zeker weet dat uw persoonlijke pagina is gezien door de gebruiker.
1. Wachten op selectie van de gebruiker van gerangschikte inhoud. 
1. Roep de API van derden om op te geven hoe goed de uitvoer van de positie-API hebt.

## <a name="how-to-use-personalizer-with-a-chat-bot"></a>Personalizer gebruiken met een chatbot

In dit voorbeeld ziet u hoe u een standaard suggestie wilt doen in plaats van de gebruiker in een reeks van menu's of keuzen elke keer dat met persoonlijke instellingen.

* Krijgen de [code](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/ChatbotExample) voor dit voorbeeld.
* Instellen van uw bot-oplossing. Zorg ervoor dat uw LUIS-toepassing wilt publiceren. 
* Beheren van de positie en beloning API-aanroepen voor bot.
    * Code voor het beheren van LUIS intentie verwerking toevoegen. Als de **geen** wordt geretourneerd als het belangrijkste doel of het belangrijkste doel score lager dan de drempelwaarde van uw zakelijke logica is, verzendt de intenties lijst Personalizer op positie de intents.
    * Intentie lijst weergeven voor gebruiker als koppelingen in met het eerste doel is de bedoeling boven gerangschikt van positie-API-reactie wordt alle geselecteerd.
    * Selectie van de gebruiker vastleggen en verzendt u deze in de beloning API-aanroep. 

### <a name="recommended-bot-patterns"></a>Aanbevolen bot patronen

* Personalizer positie API-aanroepen telkens wanneer een oplossen van ambiguïteit nodig is, in plaats van in cache opslaan van resultaten voor elke gebruiker. Het resultaat van het oplossen van dubbelzinnigheden in doel kan worden gewijzigd na verloop van tijd voor één persoon en zodat de positie-API voor het verkennen van afwijkingen versnelt de algehele learning.
* Kies een interactie die geldt met veel gebruikers zodat er voldoende gegevens om aan te passen. Bijvoorbeeld, inleidende vragen mogelijk beter geschikt dan kleinere verduidelijkingen diep in de conversatie-grafiek die alleen enkele gebruikers tot krijgen kunnen.
* Positie API-aanroepen gebruiken om in te schakelen "eerste voorstel is rechts" gesprekken, waarbij de gebruiker wordt gevraagd "Wilt u X?" of "Bedoelde u X?" en de gebruiker kan alleen bevestigen; in plaats van met opties voor de gebruiker waar ze moeten kiezen uit een menu. Bijvoorbeeld gebruiker: 'Ik wil een kopje koffie order' Bot: "Wilt u een dubbele espresso?". Op deze manier het signaal beloning is ook sterk wordt rechtstreeks naar de één suggestie ingegaan.

## <a name="how-to-use-personalizer-with-a-recommendation-solution"></a>Personalizer gebruiken met een aanbeveling-oplossing

De engine voor aanbevelingen voor het filteren van een grote catalogus om enkele items die vervolgens kunnen worden weergegeven als 30 mogelijke acties die worden verzonden naar de API van de positie te gebruiken.

U kunt Personalizer aanbevelingsengines gebruiken:

* Instellen van de [aanbeveling oplossing](https://github.com/Microsoft/Recommenders/). 
* Bij het weergeven van een pagina, aanroepen van het Model van de aanbeveling om een korte lijst aanbevelingen.
* Persoonlijke instellingen voor de rangschikking van de uitvoer van de aanbeveling oplossing aanroepen.
* Feedback over uw gebruikersbewerking met de beloning API-aanroep.


## <a name="pitfalls-to-avoid"></a>Valkuilen om te voorkomen

* Gebruik geen Personalizer waarbij de persoonlijke gedrag is niet iets dat voor alle gebruikers kunnen worden gedetecteerd, maar in plaats van iets dat moet worden onthouden voor specifieke gebruikers of afkomstig is van een bepaalde gebruiker lijst alternatieven. Bijvoorbeeld, met behulp van Personalizer een eerste pizza volgorde uit een lijst met 20 mogelijk menuopdrachten is nuttig, maar die contactpersoon aan te roepen vanuit de gebruikers lijst met contactpersonen als hulp bij kinderen (zoals 'oma') die niet iets aanpasbare via voorstellen uw gebruikersbasis.


## <a name="adding-content-safeguards-to-your-application"></a>Inhoud beveiliging toe te voegen aan uw toepassing

Als uw toepassing kunt u grote verschillen in de inhoud die wordt weergegeven aan gebruikers en enkele van die inhoud mogelijk onveilige of niet geschikt zijn voor sommige gebruikers, u moet plan vooruit om ervoor te zorgen dat zijn de juiste beveiligingen om te voorkomen dat uw gebruikers herstelopties zien niet toegestaan. inhoud. De aanbevolen patroon voor het implementeren van veiligheidsmaatregelen is: De aanbevolen patroon voor het implementeren van veiligheidsmaatregelen is:
    * De lijst met acties op de positie ophalen.
    * Filter de waarden die niet haalbaar voor het publiek.
    * Alleen deze levensvatbare acties rangschikken.
    * De bovenste gerangschikt actie voor de gebruiker worden weergegeven.

In sommige architecturen, de bovenstaande reeks mogelijk moeilijk te implementeren. In dat geval wordt er is een alternatieve methode voor het implementeren van beveiliging na rangorde, maar een voorziening moet worden gemaakt, zodat de acties die valt buiten de beveiliging niet worden gebruikt voor het Personalizer-model te trainen.

* De lijst met acties wilt rangschikken, verkrijgen met learning gedeactiveerd.
* Positie acties.
* Controleer of de bovenste actie mogelijk is.
    * Als de bovenste actie mogelijk is, learning voor deze rangschikking activeren, wordt deze aan de gebruiker weergegeven.
    * Als de eerste actie niet levensvatbare, training voor deze rangschikking activeren en bepalen via uw eigen logica of alternatieve methoden wat om weer te geven aan de gebruiker. Zelfs als u de tweede beste gerangschikte-optie gebruikt, hebt u training voor deze classificatie niet geactiveerd.

## <a name="verifying-adequate-effectiveness-of-personalizer"></a>Voldoende effectiviteit van Personalizer controleren

U kunt de effectiviteit van Personalizer periodiek controleren door te voeren [offline evaluaties](how-to-offline-evaluation.md)

## <a name="next-steps"></a>Volgende stappen

Inzicht in [waar kunt u Personalizer](where-can-you-use-personalizer.md).