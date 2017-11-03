---
title: Automatisch schalen en App Service-omgeving v1
description: Automatisch schalen en App-serviceomgeving
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 0feb6e4862f643c35a58c0321181bdda22b032e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Automatisch schalen en App Service-omgeving v1

> [!NOTE]
> In dit artikel gaat over de v1 App Service-omgeving.  Er is een nieuwere versie van de App Service-omgeving die eenvoudiger te gebruiken en wordt uitgevoerd op krachtiger infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot de App-serviceomgeving](intro.md).
> 

Ondersteuning voor Azure App Service-omgevingen *automatisch schalen*. U kunt afzonderlijke werknemersgroepen automatisch schalen op basis van de metrische gegevens of schema.

![Opties voor automatisch schalen voor een worker-groep.][intro]

Automatisch schalen optimaliseert uw Resourcegebruik door automatisch vergroten en verkleinen van een App Service-omgeving om aanpassen aan uw budget en of profiel laden.

## <a name="configure-worker-pool-autoscale"></a>Worker-groep automatisch schalen configureren
U hebt toegang tot de functionaliteit voor automatisch schalen van de **instellingen** tabblad van de worker-groep.

![Tabblad instellingen van de werknemer van toepassingen.][settings-scale]

Vanuit is dat de interface moet bekend zijn omdat het dat u ziet wanneer u een App Service-plan schalen dezelfde ervaring. 

![De instellingen handmatig schalen.][scale-manual]

U kunt ook een profiel voor automatisch schalen configureren.

![Instellingen voor automatisch schalen.][scale-profile]

Profielen voor automatisch schalen zijn handig om beperkingen op schaal. Op deze manier kunt u een consistente prestaties ervaren door een ondergrens schaalwaarde (1) en een voorspelbare uitgaven initiaal door in te stellen van een bovengrens (2) hebben.

![Scale-instellingen in het profiel.][scale-profile2]

Nadat u een profiel definieert, kunt u regels voor automatisch schalen schaal omhoog of omlaag het aantal exemplaren in de worker-groep binnen de grenzen die zijn gedefinieerd door het profiel kunt toevoegen. Regels voor automatisch schalen die zijn gebaseerd op metrische gegevens.

![Schaalregel.][scale-rule]

 Een werknemersgroep of de front-metrische gegevens kan worden gebruikt voor het definiëren van regels voor automatisch schalen. Deze metrische gegevens zijn dezelfde metrische gegevens kunt u meldingen instellen voor of bewaken in de resource-blade-grafieken.

## <a name="autoscale-example"></a>Voorbeeld voor automatisch schalen
Automatisch schalen van een App Service-omgeving kan best worden geïllustreerd door het doorlopen van een scenario.

Dit artikel wordt uitgelegd van de benodigde overwegingen bij het instellen van automatisch schalen. Het artikel doorloopt u de interacties die een rol spelen als u automatisch schalen App Service-omgevingen die worden gehost in App Service-omgeving.

### <a name="scenario-introduction"></a>Scenario Inleiding
Frank is een sysadmin voor een onderneming die een deel van de werkbelastingen die hij beheert is gemigreerd naar een App Service-omgeving.

De App Service-omgeving is geconfigureerd voor handmatige scale als volgt:

* **FrontPage-ends:** 3
* **Worker-groep 1**: 10
* **Werknemersgroep 2**: 5
* **Werknemersgroep 3**: 5

Worker-groep 1 wordt gebruikt voor productieworkloads, hoewel werknemersgroep 2 en 3 van werknemersgroep worden gebruikt voor kwaliteit assurance (QA) en ontwikkeling werkbelastingen.

De App Service-abonnementen voor QA en dev zijn geconfigureerd voor handmatige schaal. De productie-App Service-abonnement is ingesteld op automatisch schalen om te gaan met de verschillen in de belasting en verkeer.

Frank zeer vertrouwd is met de toepassing. Hij weet dat de piekuren voor load tussen 9:00 uur en 18:00 uur zijn omdat dit een line-of-business (LOB)-toepassing die werknemers gebruiken terwijl ze op kantoor zijn. Gebruik zakt hierna wanneer gebruikers klaar bent voor die dag. Buiten de piekuren is er nog steeds bepaalde laden omdat gebruikers toegang de app op afstand tot hebben via hun mobiele apparaten of thuiscomputers. De productie-App Service-abonnement is al geconfigureerd voor automatisch schalen op basis van CPU-gebruik met de volgende regels:

![Specifieke instellingen voor LOB-app.][asp-scale]

| **Profiel voor automatisch schalen: weekdagen-App Service-abonnement** | **Profiel voor automatisch schalen: tijdens het weekend-App Service-abonnement** |
| --- | --- |
| **Naam:** profiel weekdag |**Naam:** profiel voor het Weekend |
| **Schaal door:** regels voor planning en prestaties |**Schaal door:** regels voor planning en prestaties |
| **Profiel:** weekdagen |**Profiel:** Weekend |
| **Type:** terugkeerpatroon |**Type:** terugkeerpatroon |
| **Doelbereik:** 5-20-exemplaren |**Doelbereik:** 3 tot 10 exemplaren |
| **Aantal dagen:** maandag, dinsdag, woensdag, donderdag en vrijdag |**Aantal dagen:** zaterdag, zondag |
| **Begintijd:** 9:00 uur |**Begintijd:** 9:00 uur |
| **Tijdzone:** UTC-08 |**Tijdzone:** UTC-08 |
|  | |
| **Regel voor automatisch schalen (Omhoog schalen)** |**Regel voor automatisch schalen (Omhoog schalen)** |
| **Bron:** productie (App Service-omgeving) |**Bron:** productie (App Service-omgeving) |
| **Metrische gegevens:** CPU-percentage |**Metrische gegevens:** CPU-percentage |
| **Bewerking:** groter zijn dan 60% |**Bewerking:** groter is dan 80% |
| **Duur:** 5 minuten |**Duur:** 10 minuten |
| **Tijd aggregatie:** gemiddelde |**Tijd aggregatie:** gemiddelde |
| **Actie:** aantal verhogen door 2 |**Actie:** aantal verhoogd met 1 |
| **Cool omlaag (minuten):** 15 |**Cool omlaag (minuten):** 20 |
|  | |
| **Regel voor automatisch schalen (schaal omlaag)** |**Regel voor automatisch schalen (schaal omlaag)** |
| **Bron:** productie (App Service-omgeving) |**Bron:** productie (App Service-omgeving) |
| **Metrische gegevens:** CPU-percentage |**Metrische gegevens:** CPU-percentage |
| **Bewerking:** minder dan 30% |**Bewerking:** minder dan 20% |
| **Duur:** 10 minuten |**Duur:** 15 minuten |
| **Tijd aggregatie:** gemiddelde |**Tijd aggregatie:** gemiddelde |
| **Actie:** aantal verlagen door 1 |**Actie:** aantal verlagen door 1 |
| **Cool omlaag (minuten):** 20 |**Cool omlaag (minuten):** 10 |

### <a name="app-service-plan-inflation-rate"></a>De snelheid van App Service-abonnement
App Service-abonnementen die zijn geconfigureerd voor automatisch schalen, doen dit met een maximale snelheid per uur. Deze snelheid kan worden berekend op basis van de waarden op de regel voor automatisch schalen.

Begrijpen en het berekenen van de *App Service plan de frequentie* is belangrijk voor App Service-omgeving voor automatisch schalen schaalwijzigingen in een werknemersgroep worden niet onmiddellijk.

De snelheid van App Service plan de is als volgt berekend:

![De frequentie waarmee de berekening van App Service-abonnement.][ASP-Inflation]

Op basis van het automatisch schalen – omhoog schalen-regel voor het profiel werkdag van de productie-App Service-abonnement:

![App Service plan de snelheid voor weekdagen op basis van automatisch schalen – omhoog schalen-regel.][Equation1]

In het geval van het automatisch schalen – omhoog schalen-regel voor het Weekend profiel van de productie-App Service-abonnement, zou de formule omzetten in:

![App Service plan de snelheid voor het weekend op basis van automatisch schalen – omhoog schalen-regel.][Equation2]

Deze waarde kan ook worden berekend voor omlaag schalen bewerkingen.

Op basis van het automatisch schalen: Scale omlaag regel voor het profiel werkdag van de productie-App Service-plan ziet dit er als volgt:

![App Service plan de snelheid voor weekdagen op basis van automatisch schalen: Scale omlaag regel.][Equation3]

In het geval van het automatisch schalen: Scale omlaag regel voor het Weekend profiel van de productie-App Service-abonnement, zou de formule omzetten in:  

![App Service plan de snelheid voor het weekend op basis van automatisch schalen: Scale omlaag regel.][Equation4]

De productie-App Service-abonnement kan worden uitgebreid met een maximale snelheid van acht exemplaren per uur in de week en vier exemplaren per uur tijdens het weekend. Deze kunt vrijgeven exemplaren met een maximale snelheid van vier exemplaren per uur in de week en zes exemplaren per uur tijdens het weekend.

Als meerdere App Service-abonnementen worden wordt gehost in een werknemersgroep, hebt u voor het berekenen van de *totale frequentie van de* als de som van de frequentie van de voor alle App Service-abonnementen die worden host in die worker-groep.

![De berekening van de totale frequentie van de voor meerdere App Service-abonnementen die in een werknemersgroep worden gehost.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Gebruik van de snelheid van App Service plan de worker-groep voor automatisch schalen regels definiëren
Werknemer die host App Service-abonnementen die zijn geconfigureerd voor automatisch schalen moeten een buffer van de capaciteit worden toegewezen aan de groep. De buffer kunt u de bewerkingen voor automatisch schalen om te vergroten of verkleinen van de App Service-plan, indien nodig. De minimale buffer zou de berekende totale App Service Plan de frequentie zijn.

Omdat App Service-omgeving schaalbewerkingen even duren om toe te passen, wordt elke wijziging moet rekening houden met verder vraag verandert die optreden kunnen wanneer een schaalaanpassing uitgevoerd wordt. Om te voldoen deze latentie, wordt u aangeraden de berekende totale App Service Plan de frequentie te gebruiken als het minimum aantal exemplaren die zijn toegevoegd voor elke bewerking automatisch schalen.

Met deze informatie kunt Frank het volgende profiel voor automatisch schalen en regels definiëren:

![Regels voor automatisch schalen profiel voor LOB-voorbeeld.][Worker-Pool-Scale]

| **Profiel voor automatisch schalen: weekdagen** | **Profiel voor automatisch schalen: tijdens het weekend** |
| --- | --- |
| **Naam:** profiel weekdag |**Naam:** profiel voor het Weekend |
| **Schaal door:** regels voor planning en prestaties |**Schaal door:** regels voor planning en prestaties |
| **Profiel:** weekdagen |**Profiel:** Weekend |
| **Type:** terugkeerpatroon |**Type:** terugkeerpatroon |
| **Doelbereik:** 13 tot en met 25-exemplaren |**Doelbereik:** 6 tot en met 15 exemplaren |
| **Aantal dagen:** maandag, dinsdag, woensdag, donderdag en vrijdag |**Aantal dagen:** zaterdag, zondag |
| **Begintijd:** 7:00 uur |**Begintijd:** 9:00 uur |
| **Tijdzone:** UTC-08 |**Tijdzone:** UTC-08 |
|  | |
| **Regel voor automatisch schalen (Omhoog schalen)** |**Regel voor automatisch schalen (Omhoog schalen)** |
| **Bron:** werknemersgroep 1 |**Bron:** werknemersgroep 1 |
| **Metrische gegevens:** WorkersAvailable |**Metrische gegevens:** WorkersAvailable |
| **Bewerking:** minder dan 8 |**Bewerking:** minder dan 3 |
| **Duur:** 20 minuten |**Duur:** 30 minuten |
| **Tijd aggregatie:** gemiddelde |**Tijd aggregatie:** gemiddelde |
| **Actie:** aantal verhogen door 8 |**Actie:** aantal verhogen door 3 |
| **Cool omlaag (minuten):** 180 |**Cool omlaag (minuten):** 180 |
|  | |
| **Regel voor automatisch schalen (schaal omlaag)** |**Regel voor automatisch schalen (schaal omlaag)** |
| **Bron:** werknemersgroep 1 |**Bron:** werknemersgroep 1 |
| **Metrische gegevens:** WorkersAvailable |**Metrische gegevens:** WorkersAvailable |
| **Bewerking:** groter is dan 8 |**Bewerking:** groter is dan 3 |
| **Duur:** 20 minuten |**Duur:** 15 minuten |
| **Tijd aggregatie:** gemiddelde |**Tijd aggregatie:** gemiddelde |
| **Actie:** aantal verlagen door 2 |**Actie:** aantal verlagen door 3 |
| **Cool omlaag (minuten):** 120 |**Cool omlaag (minuten):** 120 |

De doel-bereik dat is gedefinieerd in het profiel wordt berekend door de minimale exemplaren die zijn gedefinieerd in het profiel voor de App Service-abonnement + buffer.

Het Maximum-bereik is de som van de maximale bereiken voor alle App Service-abonnementen die worden gehost in de worker-groep.

Het aantal van de verhoging voor de schaal van regels moet worden ingesteld op ten minste 1 X-App Service Plan de frequentie voor scale-.

Verklein het aantal kan worden aangepast op een andere waarde tussen 1/2 X of 1 X-App Service Plan de frequentie voor schaal omlaag.

### <a name="autoscale-for-front-end-pool"></a>Voor de front-toepassingen voor automatisch schalen
Regels voor front-automatisch schalen zijn eenvoudiger dan voor werknemersgroepen. U dient voornamelijk om  
Zorg ervoor dat de duur van de meting en de vernieuwingstimer cooldown rekening houdt scale-bewerkingen op een App Service-abonnement zijn niet onmiddellijk.

In dit scenario weet Frank dat de frequentie van fouten neemt toe nadat front-ends CPU-gebruik 80% bereiken en stelt de regel voor automatisch schalen om te verhogen exemplaren als volgt:

![Instellingen voor automatisch schalen voor de front-toepassingen.][Front-End-Scale]

| **Profiel voor automatisch schalen: begin eindigt** |
| --- |
| **Naam:** automatisch schalen: begin eindigt |
| **Schaal door:** regels voor planning en prestaties |
| **Profiel:** dagelijkse |
| **Type:** terugkeerpatroon |
| **Doelbereik:** 3 tot 10 exemplaren |
| **Aantal dagen:** dagelijkse |
| **Begintijd:** 9:00 uur |
| **Tijdzone:** UTC-08 |
|  |
| **Regel voor automatisch schalen (Omhoog schalen)** |
| **Bron:** front-groep |
| **Metrische gegevens:** CPU-percentage |
| **Bewerking:** groter zijn dan 60% |
| **Duur:** 20 minuten |
| **Tijd aggregatie:** gemiddelde |
| **Actie:** aantal verhogen door 3 |
| **Cool omlaag (minuten):** 120 |
|  |
| **Regel voor automatisch schalen (schaal omlaag)** |
| **Bron:** werknemersgroep 1 |
| **Metrische gegevens:** CPU-percentage |
| **Bewerking:** minder dan 30% |
| **Duur:** 20 minuten |
| **Tijd aggregatie:** gemiddelde |
| **Actie:** aantal verlagen door 3 |
| **Cool omlaag (minuten):** 120 |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
