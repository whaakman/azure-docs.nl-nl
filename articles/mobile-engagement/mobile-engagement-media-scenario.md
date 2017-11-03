---
title: Azure Mobile Engagement-toepassing voor Media-App
description: Media-app-scenario voor het implementeren van Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48201cc8-4e04-485c-a8dc-d6406d23f3ed
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c1591c3e436981e621830916cf0cdc4b7f395d7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="implement-mobile-engagement-with-media-app"></a>Mobile Engagement met Media-App implementeren
## <a name="overview"></a>Overzicht
Johan is een mobiele projectmanager voor een bedrijf big media. Hij onlangs een nieuwe app die een zeer hoge downloaden heeft gestart. Hij zijn doelstellingen voor het downloaden is bereikt, maar nog steeds zijn retourneren op Investment(ROI) per gebruiker niet voldoet aan de vereisten. 

Jeroen heeft al geïdentificeerd daarom zijn rendement te laag is. Gebruikers stop vaak zijn app na alleen 2 weken gebruiken en de meeste van deze nooit terugkeren. Hij wil de retentie van de app te verhogen.

Nadat een aantal initiële testen, vaak dat hij heeft geleerd wanneer hij de gebruikers met pushmeldingen, stelt ze om door te gaan met de app. Gebruikers die niet actief zijn wordt ook vaak naar de app, afhankelijk van de meldingen hij deze verzendt geretourneerd. Jeroen wil investeren in een soort Betrokkenheidsprogramma voor de app die gebruikmaakt van geavanceerde doelen met pushmeldingen.

Jeroen heeft onlangs Lees de [Azure Mobile Engagement - instructiehandleiding met aanbevolen procedures](mobile-engagement-getting-started-best-practices.md) heeft gekozen voor het implementeren van de aanbevelingen van de handleiding.

## <a name="objectives-and-kpis"></a>Doelstellingen en KPI 's
Voldoen aan de belangrijkste belanghebbenden voor Johan app. Zakelijke is gegenereerd op basis van advertenties als gebruikers zijn media gebruiken. Jeroen verhoogt doordat inhoud verbruikt per gebruiker zijn opbrengsten. Alle overeenstemming worden bereikt over een hoofddoel: verkoop van advertenties met 25% te verhogen. Ze maken deze doelstelling Business Key Performance Indicators (KPI's) naar een meting en het station

* Aantal advertenties waarop is geklikt per gebruiker
* Hoeveel artikel's bezocht (per gebruiker / per sessie / per week / maand...)
* Wat zijn de favoriete categorieën

Op basis van Johan vergadering met de belangrijkste belanghebbenden heeft hij zijn zakelijke KPI's gedefinieerd. Hij volgt deel 1 van de [Azure Mobile Engagement - instructiehandleiding met aanbevolen procedures](mobile-engagement-getting-started-best-practices.md). 

Vervolgens maakt hij de volgende Betrokkenheids-KPI's om ervoor te zorgen dat de doelstellingen worden bereikt:

* Bewaken van de bewaartermijn voor de volgende intervallen: dagelijks, wekelijks, tweewekelijks en maandelijkse.
* Telt het aantal actieve gebruikers
* De classificatie van apps in de app wordt opgeslagen

Op basis van de aanbevelingen van het IT-team, zijn de volgende technische KPI's toegevoegd aan de volgende vragen beantwoorden:

* Wat is er een pad naar de gebruiker (welke bezochte hoeveel gebruikers besteden aan het)
* Het aantal crashes en fouten per sessie opgetreden?
* Welke versies van het besturingssysteem worden uitgevoerd voor Mijn gebruikers?
* Wat is de gemiddelde grootte van het scherm voor Mijn gebruikers?
* Wat voor soort internet-verbindingen hebt mijn gebruikers?

Hij classificeert de gegevens die nodig zijn voor elke KPI en hij deze op de juiste locatie van diens playbook registreert.

## <a name="engagement-program-and-integration"></a>Betrokkenheidsprogramma en integratie
Nu dat Jeroen klaar is met het definiëren van de KPI's, begint zijn Engagement-strategie fase hij door 4 betrokkenheidsprogramma's en hun doelstellingen te definiëren:![][1]

Vervolgens gaat John diepere door met gedetailleerde informatie over pushmeldingen voor elk programma. Push-bericht zijn gedefinieerd door vijf elementen:

1. Doelstelling: Wat is het doel van de melding
2. Hoe het doel wordt bereikt
3. Doel: die de melding ontvangt?
4. Inhoud: Wat is de tekst en de indeling van de melding (In App/Out van App)
5. Wanneer: Wat is het beste moment om deze pushmelding te verzenden
   
    ![][2]

Raadpleeg voor meer informatie de [hulpmiddelen marketing en verkoop](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Volgens het deel 2 van het witboek John doelsectie gebruikt voor het definiëren van welke hij heeft voor het verzamelen van gegevens en schrijft zijn Tagplan samen met de IT-team om de oplossing te implementeren. Jeroen na 1 week van de implementatie en gebruikersacceptatietests kunt ten slotte zijn programma's starten.

## <a name="program-results"></a>Programma resultaten
4 maanden later neemt Jeroen de prestaties van programma's. Het Welkomstprogramma en het wekelijkse programma zijn doelstellingen zijn. Het nummer van de gebruiker met slechts één sessie afname, meer functies van de app worden gebruikt en het aantal verbindingen per week is verdubbeld.

De **inactieve programma** helpt John gebruiker instinct begrijpen. Het lijkt erop dat 15% van de inactieve gebruikers keert u terug naar de app. Maar deze meestal niet blijven actief meer dan 1 maand. Jeroen voorziet in een potentiële optimalisatie van deze reeks met extra meldingen en zijn inhoud keuzes uitbreiden.

De **programma Discover** niet goed werkt. De bijbehorende waarde stijgt kruislings verkopen maar niet voldoende zijn doelstellingen te bereiken. Jeroen identificeert dat hij geen voldoende gegevens om relevante als doel en de betreffende inhoud voor te stellen. Hij dit programma wordt gestopt en legt de nadruk op 'redactionele pushmeldingen' met Azure Mobile Engagement verzenden. Zijn journalisten al een CMS-oplossing om pushmeldingen te verzenden en ze niet wilt wijzigen.

Jeroen wil de Reach API die een HTTP REST-API waarmee het beheren van Reach-campagnes zonder gebruik te maken van AZME-Web-interface gebruiken. Met deze methode kunt John verzamelen van de gegevens die hij nodig heeft en zijn schrijvers toestaan te blijven gebruiken de CMS-oplossing.

Om ervoor te zorgen dat functie correct werkt, vraagt Jeroen de IT-team worden vigilant op de volgende punten:

1. **Bewerking systemen** : alle hebben hun eigen regels voor het beheren van pushmeldingen, zodat Jeroen wil de lijst van alle gevallen en als de API's; het verwerkt wordt gecontroleerd.
   Bijvoorbeeld: Android push-systeem kan grote afbeelding die niet het geval is bij iOS.
2. **Tijdsbestek**: Jeroen wil een API die het tijdsbestek ingesteld en een einde campagnes. Hij gebruikers van elke verstoren melding bombing behouden.
3. **Categorieën**: marketingteam bereidt een sjabloon voor elk type waarschuwingen. Jeroen vraagt de IT-team categorieën binnen de API instellen.

Johan is na een bepaalde tests tevreden. Dankzij deze API kunnen journalisten nog steeds verzenden pushmeldingen met hun CMS- en Azure Mobile Engagement worden alle gebruikersgedrag gegevens verzameld voor deze

Na deze 4 eerst maanden, resultaten weer een goede algehele prestaties en biedt betrouwbaarheid voor John en zijn mededelingenbord, ROI per gebruiker toeneemt per 15% en mobiele verkoop 17,5% van de totale verkoop een toename van 7.5% in slechts vier maanden vertegenwoordigen.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
