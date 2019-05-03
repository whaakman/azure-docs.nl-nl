---
title: Ethiek en verantwoordelijk gebruik - Personalizer
titleSuffix: Azure Cognitive Services
description: Deze richtlijnen zijn gericht op helpen u bij het implementeren van persoonlijke instellingen op een manier die helpt u bij het bouwen van vertrouwen in uw bedrijf en de service. Zorg dat onderbreken om te onderzoeken, informatie en opzettelijke over de gevolgen van de persoonlijke instellingen op het leven van mensen. Bij twijfel proberen richtlijnen.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 7b1e972b5516aa79d1754e32e487e17c9e68ac1d
ms.sourcegitcommit: eea74d11a6d6ea6d187e90e368e70e46b76cd2aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2019
ms.locfileid: "65035422"
---
# <a name="guidelines-for-responsible-implementation-of-personalizer"></a>Richtlijnen voor verantwoordelijk Personalizer-implementatie

Voor personen en society om te profiteren van het volledige potentieel van AI, moeten implementaties zo worden ontworpen zodanig dat ze verdienen op de vertrouwensrelatie van de AI toe te voegen aan hun toepassingen en de gebruikers van toepassingen die zijn gebouwd met AI. Deze richtlijnen zijn gericht op helpen u bij het Personalizer implementeren op een manier die helpt u bij het bouwen van vertrouwen in uw bedrijf en de service. Zorg dat onderbreken om te onderzoeken, informatie en opzettelijke over de gevolgen van de persoonlijke instellingen op het leven van mensen. Bij twijfel proberen richtlijnen.

Deze richtlijnen zijn niet bedoeld als juridisch advies en u moet afzonderlijk voor zorgen dat uw toepassing aan de snelle ontwikkelingen in het recht op dit gebied en in de sector voldoet.

Bij het ontwerpen van uw toepassing met behulp van Personalizer, moet u ook een breed scala aan verantwoordelijkheden hebt bij het ontwikkelen van een gegevens-en AI-systeem, inclusief ethische, privacy, beveiliging, veiligheid, opname, transparantie en verantwoordelijkheid overwegen. U kunt meer lezen over deze in de [lezen aanbevolen](#recommended-reading) sectie.

U kunt de volgende inhoud als een controlelijst starter gebruiken en aanpassen en Verfijn deze voor uw scenario. Dit document bevat twee hoofdsecties: De eerste is toegewezen aan het markeren van overwegingen die verantwoordelijk zijn gebruikt bij het kiezen van scenario's, functies en voordelen voor Personalizer. De tweede nemen een set waarden die Microsoft ervan overtuigd is moet worden gehouden bij het bouwen van AI-systemen en bruikbare suggesties aanbiedt en risico's op hoe het gebruik van Personalizer ze beïnvloedt. 


## <a name="your-responsibility"></a>Uw eigen verantwoordelijkheid

Alle richtlijnen voor verantwoordelijk uitvoering maken op basis ontwikkelaars en bedrijven die met Personalizer verantwoordelijk en aansprakelijk voor de gevolgen van het gebruik van deze algoritmen in de samenleving zijn. Als u een toepassing die wordt geïmplementeerd in uw organisatie ontwikkelt, moet u uw rol en de verantwoordelijkheid voor de werking ervan en hoe dit van invloed op gebruikers herkennen. Als u het ontwerpen van een toepassing worden geïmplementeerd door een derde partij, keert u naar een gedeelde wat ze van die verantwoordelijk is voor het gedrag van de toepassing is en die inzicht-document.

Vertrouwensrelatie is gebouwd op het principe van afgehandelde verplichtingen: Houd rekening met uw gebruikers, society en uw toepassingen werkt, om te identificeren van de expliciete en impliciete verplichtingen hebben misschien juridische framework.

Microsoft is voortdurend inspanning plaatsen in de hulpprogramma's en documenten om te reageren op deze verantwoordelijkheden. [Microsoft feedback geeft](mailto:cogsvcs-RL-feedback@microsoft.com?subject%3DPersonalizer%20Responsible%20Use%20Feedback&body%3D%5BPlease%20share%20any%20question%2C%20idea%20or%20concern%5D) als u denkt aanvullende hulpprogramma's dat, functies en -documenten wilt helpen u bij het implementeren van deze richtlijnen voor het gebruik van Personalizer.


## <a name="factors-for-responsibly-implementing-personalizer"></a>Factoren voor Personalizer verantwoorde manier te implementeren

Personalizer implementeren kan van geweldige waarde aan uw gebruikers en uw bedrijf zijn. Start om te Personalizer verantwoorde manier te implementeren, op basis van de volgende richtlijnen wanneer:

* Use-cases om toe te passen aan persoonlijke voorkeur aanpassen te kiezen.
* Het bouwen van [Beloon functies](https://github.com/Azure/personalization-rl/blob/master/docs/concepts-rewards.md).
* Kiezen welke [functies](https://github.com/Azure/personalization-rl/blob/master/docs/concepts-features.md) over de context en de mogelijke acties die u voor persoonlijke instellingen gebruiken wilt.


## <a name="choosing-use-cases-for-personalizer"></a>Gebruiksvoorbeelden kiezen voor Personalizer

Met behulp van een service waarmee u leert personaliseren van inhoud en gebruiker interfaces is handig. Het kan ook worden misapplied als de manier waarop de personalisatie negatieve neveneffecten in de echte wereld maakt, met inbegrip van gebruikers zich niet bewust van personalisatie van inhoud. 

Voorbeelden van het gebruik van Personalizer met betere mogelijkheden voor negatieve neveneffecten of een gebrek aan transparantie zijn scenario's waarin de 'prijs' is afhankelijk van veel op de lange termijn complexe factoren die, wanneer te veel vereenvoudigd in een onmiddellijke beloning ongunstig kan hebben resultaten voor personen. Deze meestal worden beschouwd als "GEVOLGSCHADE" keuzes of keuzes die betrekking hebben op een risico van schade. Bijvoorbeeld: 


* **Financiën**: Aanpassen aan uw wensen biedt op lening, financiële, en producten, waar de risicofactoren die zijn gebaseerd op gegevens personen zonder uw weten, kunnen niet worden verkregen, maar kunnen geen geschil. 
* **Onderwijs**: Aanpassen aan uw wensen posities voor school cursussen en onderwijsinstellingen waar aanbevelingen kunnen doorgeven vooroordelen en verminderen awareness voor gebruikers van andere opties.
* **Democratische en Civic deel te nemen**: Personaliseren van inhoud voor gebruikers met het doel van invloed op adviezen is gevolgschade en Manipulatieve.
* **Evaluatie van derden beloning**: Aanpassen aan uw wensen items waarbij de prijs is gebaseerd op een laatste 3rd party evaluatie van de gebruiker, in plaats van een beloning die worden gegenereerd door het gedrag van gebruiker.
* **Intolerantie verkennen**: Een situatie waarbij het gedrag van het verkennen van Personalizer schade kan veroorzaken.

Bij het kiezen van use cases voor Personalizer:

* Start het ontwerpproces overwegen hoe de personalisatie helpt uw gebruikers.
* Overweeg de negatieve gevolgen in de praktijk gebruiken als sommige items niet worden gerangschikt voor gebruikers vanwege personalisatie patronen of verkennen.
* U kunt zelf uitvoeren prophecy lussen. Dit kan gebeuren als een beloning personalisatie een model traint, zodat deze kan vervolgens verder uitsluiten van een demografische groep toegang tot relevante inhoud. Bijvoorbeeld, de meeste mensen in een lage inkomens groep niet verkrijgen van een premium-aanbieding voor verzekeringen en langzaam niemand in de groep om te zien van de aanbieding op alle doorgaans.
* Sla een kopie van modellen en het learning-beleid in het geval is het nodig zijn om het te reproduceren Personalizer in de toekomst. U kunt dit doen periodiek of elke vernieuwingsinterval model.
* Het niveau van onderzoek voldoende voor de ruimte en het gebruik ervan als hulpmiddel om "echo kamer" effecten te overwegen.


## <a name="selecting-features-for-personalizer"></a>Functies voor Personalizer selecteren

Personaliseren van inhoud, is afhankelijk van de met nuttige informatie over de inhoud en de gebruiker. Houd er rekening mee voor sommige toepassingen en de branches, sommige functies kunnen worden direct of indirect beschouwd als discriminerende en mogelijk ongeldige gebruiker.

Houd rekening met het effect van deze functies:

* **Gebruiker demografische gegevens**: Functies met betrekking tot geslacht, geslacht, leeftijd, race, religie: Deze functies mogelijk niet toegestaan in bepaalde toepassingen om wettelijke redenen en niet worden ethische omheen aanpassen omdat de personalisatie generalisaties en de afwijking wilt doorgeven. Een voorbeeld van deze containmentafwijking doorgifte is een taak voor engineering niet wordt weergegeven voor oudere of op basis van geslacht doelgroepen plaatsen.
* **Informatie over landinstellingen**: Op talloze plaatsen van de hele wereld, locatie-informatie (zoals een postcode, postcode of naam van groep) kan worden nauw verband houden met inkomsten, race en religie.
* **Gebruiker perceptie van eerlijkheid**: Houd rekening met het effect van gebruikers die inhoud die wordt weergegeven in uw toepassing worden gewijzigd op een manier die wordt weergegeven om te worden gecorreleerd met de functies die zou zijn om zelfs in gevallen waarin uw toepassing is maken van goede beslissingen.
* **Onbedoelde vertekening in functies**:  Er zijn typen vooroordelen die kunnen worden ingevoerd met behulp van functies die alleen van invloed op een subset van de populatie. Deze extra aandacht is vereist als functies algoritmisch worden gegenereerd, zoals wanneer met behulp van afbeelding analyse om op te halen die items in een afbeelding of tekst analytics voor het detecteren van entiteiten in tekst. Uzelf op de hoogte van de kenmerken van de services die u gebruikt om te maken van deze functies maken.

De volgende procedures gelden bij het kiezen van functies in de context en acties naar Personalizer verzenden:

* Houd rekening met het legaal en ethiek van het gebruik van bepaalde functies voor sommige toepassingen en of onschuldige ogende functies mogelijk proxy's voor anderen wilt of moet voorkomen,
* Worden transparant voor gebruikers of algoritmen en data-analyse worden gebruikt voor het aanpassen van de opties die ze zien.
* Stel uzelf de: Mijn gebruikers zou care en blij zijn als ik deze informatie gebruikt voor het aanpassen van de inhoud voor deze? Kan ik met vertrouwen, laten zien hoe de beslissing is geprobeerd te markeren of bepaalde items verbergen?
* Gebruik in plaats van de classificatie of segmentering gegevens op basis van andere kenmerken doorgevoerd. Demografische informatie is doorgaans gebruikt door detailhandelaren omwille van de historische – demografische kenmerken is eenvoudig te verzamelen en erop te reageren voordat u een digitale tijdperk - vraag, maar hoe relevante demografische informatie is wanneer er interactie met de werkelijke, contextuele en historische gegevens die betrekking beter aansluiten op de voorkeuren en de identiteit van gebruikers heeft.
* Houd rekening met het om te voorkomen dat functies 'vervalsen' door kwaadwillende gebruikers, wat als misbruik maakt van grote aantallen leiden kunnen tot Personalizer training in misleidende manieren opzettelijk verstoren, embarrass en lastigvallen bepaalde klassen van gebruikers. 
* Wanneer nodig en haalbaar is, ontwerp uw toepassing zodat uw gebruikers kunnen ervoor kiezen of u afmelden met bepaalde persoonlijke functies die worden gebruikt. Deze kunnen worden gegroepeerd, zoals "Locatiegegevens", 'Apparaatgegevens', 'Afgelopen aankoopgeschiedenis' enzovoort.


## <a name="computing-rewards-for-personalizer"></a>Voordelen voor Personalizer Computing

Personalizer streeft ernaar om te kiezen welke actie moet worden beloning op basis van de beloning score geleverd door de bedrijfslogica van uw toepassing verbeteren.

Een goed ontwikkelde beloning score fungeert als een op korte termijn proxy naar een doel business, die is gekoppeld aan de missie van een organisatie.

Bijvoorbeeld, belonen op klikken wordt de Personalizer Service zoeken klikken maken ten koste van alle andere taken, zelfs als afleiden of die niet gekoppeld aan een bedrijfsuitkomst wat wordt geklikt op is.

Als een voorbeeld van een contrasterend wilt een nieuwssite instellen beloningen gekoppeld aan een zinvolle dan klikt, zoals "Heeft de gebruiker besteed voldoende tijd om de inhoud te lezen?" 'Ze klikken op de relevante artikelen of verwijzingen?'. Met Personalizer is het eenvoudig om te koppelen van metrische gegevens om voordelen. Maar zorg ervoor dat u niet confound op korte termijn betrokkenheid van gebruikers met goede resultaten.

### <a name="unintended-consequences-from-reward-scores"></a>Ongewenste gevolgen van derden scores
Scores van derden kunnen worden gebouwd met de beste bedoelingen, maar gevolgen onverwachte of ongewenste resultaten nog steeds op hoe Personalizer ranks inhoud kunnen maken. 

Houd rekening met de volgende voorbeelden:

* Beloning voor de video personalisatie van inhoud van het percentage van de lengte van de video bekeken vaak waarschijnlijk naar positie kortere video's.
* Beloning voor sociale media-shares, zonder sentimentanalyse van hoe deze worden gedeeld of de inhoud zelf, kan leiden tot trefwoordenrangschikking aanstootgevend, niet altijd gecontroleerd of Opruiend inhoud, die doorgaans veel "engagement" incite, maar weinig kunt laten groeien.
* Beloning voor de actie voor gebruikersinterface-elementen die gebruikers niet had verwacht wijzigen kan leiden tot problemen met de gebruiksvriendelijkheid en voorspelbaarheid van de gebruikersinterface, waar knoppen verrassend wijzigt locatie of doel zonder waarschuwing, waardoor het moeilijker voor bepaalde groepen van gebruikers om productief te blijven.

Implementeer deze aanbevolen procedures:

* Offline experimenten uitvoeren op uw systeem met behulp van verschillende beloning methoden om te begrijpen van de impact en neveneffecten.
* Evalueren van uw functies beloning en u zich afvragen hoe zou een persoon extreem naïve buigen de interpretatie en ongewenste resultaten met het bereik.


## <a name="responsible-design-considerations"></a>Verantwoordelijk ontwerpoverwegingen

De volgende zijn gebieden van ontwerp voor verantwoordelijk implementaties van AI. Meer informatie dit framework in maar [berekend voor de toekomst](https://news.microsoft.com/futurecomputed/).

![AI-waarden in de toekomst berekend](media/ethics-and-responsible-use/ai-values-future-computed.png)

### <a name="accountability"></a>Aansprakelijkheid
*Mensen die bij het ontwerpen en implementeren van AI-systemen moet zijn verantwoordelijk voor de werking van hun systemen*. 

* Maken van interne richtlijnen over het implementeren van Personalizer, documenteer en kennis van uw team, leidinggevenden en leveranciers.
* Uitvoeren van periodieke beoordelingen van hoe beloning scores worden berekend, het uitvoeren van offline evaluaties om te zien welke functies zijn die betrekking hebben op Personalizer en de resultaten gebruiken om overbodige en onnodige functies weg te nemen.
* Communiceer duidelijk aan uw gebruikers hoe Personalizer worden gebruikt, wat is het doel, en met welke gegevens.
* Archiveren van gegevens en assets - zoals modellen, learning beleid en andere gegevens - die gebruikmaakt van Personalizer werken, kunnen reproduceren resultaten.

### <a name="transparency"></a>Transparantie
*AI-systemen moet Understandable*. Met Personalizer

• Geeft gebruikers meer informatie over hoe de inhoud is aangepast. Bijvoorbeeld, kunt u weergeven uw gebruikers een knop met de naam "waarom deze suggesties?" welke belangrijkste functies van de gebruiker en de acties die een rol gespeeld bij de resultaten van Personalizer worden weergegeven.
• Zorg ervoor dat de voorwaarden van de gebruik maken die u informatie over gebruikers en hun gedrag gebruikt voor het aanpassen van de ervaring worden vermeld.


* *Geef gebruikers informatie over hoe de inhoud is aangepast.* Bijvoorbeeld, kunt u weergeven uw gebruikers een knop met de naam `Why These Suggestions?` met welke belangrijkste functies van de gebruiker en de acties die een rol gespeeld bij de personalisatie.
* Zorg ervoor dat uw gebruiksvoorwaarden vermeld die u informatie over gebruikers gebruikt voor het aanpassen van de ervaring.

### <a name="fairness"></a>Eerlijkheid
* AI systemen moet alle personen vrij behandelen.

* Gebruik geen Personalizer voor use-cases waarbij de resultaten zijn op de lange termijn, gevolgschade of betrekking hebben op echte schade.
* Gebruik geen functies die niet geschikt is voor het personaliseren van inhoud met zijn of die kan helpen bij ongewenst vooroordelen doorgegeven. Bijvoorbeeld, ziet iedereen met vergelijkbare financiële omstandigheden de dezelfde persoonlijke aanbevelingen voor de financiële producten.
* Krijg inzicht in vooroordelen die mogelijk aanwezig zijn in de functies die zijn afkomstig van editors, algoritmische hulpprogramma's of gebruikers zelf.

### <a name="reliability-and-safety"></a>Betrouwbaarheid en veiligheid
*AI-systemen moet uitvoeren, betrouwbaar en veilig*. Voor Personalizer:

* *Geen acties bieden aan Personalizer die al dan niet mogen worden gekozen*. Bijvoorbeeld, moeten ongepast films worden gefilterd buiten de acties om aan te passen als een aanbeveling voor een anonieme of een te voorzichtige leeftijd gebruiker.
* *Beheren van uw modellen vervolgens Personalizer als een activum business*.  Houd rekening met hoe vaak u wilt opslaan en back-up van het model en beleidsregels achter uw lus Personalizer leren en dit anders wordt behandeld als een belangrijke zakelijke activa. Afgelopen resultaten reproduceren is belangrijk voor zelf controleren en de meting wordt verbeterd.
* *Kanalen voor direct feedback van gebruikers bieden*. Naast het coderen van veiligheid controles om ervoor te zorgen zien alleen de doelgroepen die rechts de juiste inhoud, bieden een mechanisme voor feedback voor gebruikers om inhoud van het rapport dat is mogelijk verrassende of aanstootgevende. Met name als uw inhoud afkomstig zijn van gebruikers of 3e partij, overweeg het gebruik van Microsoft Content Moderator of extra hulpprogramma's om te controleren en valideren van inhoud.
* *Uitvoeren van regelmatige offline evaluaties*. Hierdoor kunt u trends kunt controleren en zorg ervoor dat effectiviteit bekend is.
* *Tot stand brengen van een proces voor het detecteren en reageren op schadelijke manipulatie*. Er zijn actors die van een machine learning en AI-systemen de mogelijkheid om van hun omgeving gebruikmaken wilt verschuiven van de resultaten naar hun doelstellingen te leren. Als uw gebruik van Personalizer invloed uitoefenen op belangrijke keuzes, zorg ervoor dat in de juiste manier om te detecteren en oplossen van deze klassen van aanvallen, met inbegrip van menselijke beoordeling in bepaalde gevallen.

### <a name="security-and-privacy"></a>Beveiliging en privacy
*AI-systemen moeten worden beveiligd en respecteren privacy*. Als u Personalizer:

* *Informeer gebruikers een van de gegevens die worden verzameld en hoe deze wordt gebruikt en hun toestemming vooraf ophalen*, uw lokale en branchevoorschriften regelgeving te volgen.
* *Geef gebruikersbesturingselementen privacy beschermen.* Voor toepassingen die persoonlijke gegevens opgeslagen, kunt u overwegen een knop gemakkelijk te vinden voor functies, zoals: 
   * `Show me all you know about me`    
   * `Forget my last interaction` 
   * `Delete all you know about me`

In sommige gevallen is het mogelijk dat dit wettelijk verplicht is. Houd rekening met het gebruik van systeembronnen in opnieuw trainen van modellen periodiek, zodat ze niet traceringen van verwijderde gegevens bevatten.

### <a name="inclusiveness"></a>Inclusiveness
*Adres van een grote verscheidenheid aan de behoeften en ervaringen*.
* *Persoonlijke ervaringen bieden voor interfaces toegankelijkheid is ingeschakeld.* De efficiëntie die afkomstig zijn van goede personalisatie - toegepast om te verminderen de hoeveelheid werk, -verplaatsing en onnodige herhaling in interacties kunnen worden vooral handig voor mensen met een handicap.
* *Aanpassen van de werking van de toepassing naar context*. U kunt Personalizer gebruiken om de dubbelzinnigheid tussen intents in een chatbot, bijvoorbeeld, als het is mogelijk dat de juiste interpretatie contextuele en een oplossing mogelijk niet alle voldoende. 


## <a name="proactive-readiness-for-increased-data-protection-and-governance"></a>Proactieve gereed is voor grotere hoeveelheden gegevensbescherming en beheeracties

Het is moeilijk te voorspellen, bepaalde wijzigingen in de context van regelgeving, maar in het algemeen het verstandig verder gaan dan de minimale juridische framework in ervoor te zorgen dat de privacy gebruik van persoonlijke gegevens en transparantie en keuze met betrekking tot algoritmische besluitvorming normaal zou zijn.


* Houd rekening met het plannen voor de toekomst een situatie waarbij kunnen er nieuwe beperkingen met betrekking tot de gegevens die worden verzameld van personen en nodig is om weer te geven hoe het is gebruikt om beslissingen te nemen.
* U kunt extra gereedheid waar gebruikers kwetsbaar populaties wegge, kinderen, gebruikers economische beveiligingslek in of gebruikers anders omvat mogelijk vatbaar voor van algoritmische manipulatie van invloed zijn op.
* Houd rekening met de wijdverbreid ontevredenheid met hoe doelgroep en doelgroep beïnvloeden gegevensverzameling programma's en -algoritmen uit hebben gespeeld en hoe u bewezen strategische fouten voorkomen.


## <a name="proactive-assessments-during-your-project-lifecycle"></a>Proactieve evaluaties tijdens de levenscyclus van uw project

Houd rekening met het maken van methoden voor leden van een team, gebruikers en eigenaren van zakelijke problemen met betrekking tot verantwoordelijk gebruik en het maken van een proces dat bepaalt de volgorde van de resolutie en voorkomt u dat vergelding voor het rapport.

Elke persoon nadenken over neveneffecten van het gebruik van elke technologie wordt beperkt door hun ervaring perspectief en levensduur. Het bereik van adviezen beschikbaar uitbreiden door te brengen in diverser stemmen in uw teams, gebruikers of samengewerkt; zo dat het is mogelijk en wordt aanbevolen om ze dat hardop zeggen. Houd rekening met trainings- en leerprogramma materialen op de kennis van het team in dit domein verder uitbreiden en om toe te voegen van de mogelijkheid om u te ingewikkeld en gevoelige onderwerpen worden besproken.

Houd rekening met het behandelen van taken met betrekking tot verantwoordelijk gebruik net als andere taken "crosscutting" in de levensduur van toepassingen, zoals taken met betrekking tot de gebruiker te maken krijgen, beveiliging of devops. Deze taken en de bijbehorende vereisten kunnen achteraf niet. Verantwoordelijk gebruik moet worden besproken en geverifieerd gedurende de levensduur van toepassingen.
 
## <a name="questions-and-feedback"></a>Vragen en feedback

Microsoft zich continu inzet in hulpprogramma's en documenten om te reageren op deze verantwoordelijkheden. Ons team nodigt u [feedback naar Microsoft](mailto:cogsvcs-RL-feedback@microsoft.com?subject%3DPersonalizer%20Responsible%20Use%20Feedback&body%3D%5BPlease%20share%20any%20question%2C%20idea%20or%20concern%5D) als u denkt dat ze extra hulpprogramma's, productfuncties, en documenten u bij het implementeren van deze richtlijnen helpen wilt voor het gebruik van Personalizer.

## <a name="recommended-reading"></a>Aanbevolen documentatie

* Zie zes principes van Microsoft voor de verantwoordelijk ontwikkeling van AI gepubliceerd in het adresboek januari 2018 [berekend voor de toekomst](https://news.microsoft.com/futurecomputed/)
* [Wie is eigenaar van de toekomst? ](https://www.goodreads.com/book/show/15802693-who-owns-the-future) door Jaron Lanier.
* [Wapens van Math vernietiging](https://www.goodreads.com/book/show/28186015-weapons-of-math-destruction) door - kitty O'Neil
* [Ethiek en Data Science](https://www.oreilly.com/library/view/ethics-and-data/9781492043898/) door DJ Patil, Hilary Mason, Mike Loukides.
* [Code van de ethiek ACM](https://www.acm.org/code-of-ethics)
* [Genetic Information Nondiscrimination Act - GINA](https://en.wikipedia.org/wiki/Genetic_Information_Nondiscrimination_Act)
* [FATML beginselen voor het verantwoordelijk algoritmen](http://www.fatml.org/resources/principles-for-accountable-algorithms)


## <a name="next-steps"></a>Volgende stappen

[Functies: de actie en de context](concepts-features.md).
