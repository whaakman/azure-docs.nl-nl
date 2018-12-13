---
title: Automatisch schalen en App Service Environment v1 - Azure
description: Automatisch schalen en App Service-omgeving
services: app-service
documentationcenter: ''
author: btardif
manager: erikre
editor: ''
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 29a639142395c43fea06c1d6d18909b3c9f33b86
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270022"
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Automatisch schalen en App Service Environment v1

> [!NOTE]
> In dit artikel gaat over de App Service Environment v1.  Er is een nieuwere versie van de App Service Environment die gebruiksvriendelijker en wordt uitgevoerd op een krachtigere infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot App Service Environment](intro.md).
> 

Ondersteuning voor Azure App Service-omgevingen *automatisch schalen*. U kunt afzonderlijke werknemersgroepen automatisch schalen op basis van metrische gegevens of schema.

![Opties voor automatisch schalen voor een groep met werkrollen.][intro]

Automatisch schalen optimaliseert uw Resourcegebruik door automatisch vergroten en verkleinen van een App Service-omgeving waarin u binnen uw budget passen en of profiel laden.

## <a name="configure-worker-pool-autoscale"></a>Worker-groep van toepassingen automatisch schalen configureren
U hebt toegang tot de functionaliteit voor automatisch schalen van de **instellingen** tabblad van de groep met werkrollen.

![Tabblad instellingen van de groep met werkrollen.][settings-scale]

Van daaruit is dat de interface moet bekend zijn omdat het de dezelfde ervaring die u ziet wanneer u de schaal van een App Service-plan. 

![De instellingen handmatig schalen.][scale-manual]

U kunt ook een profiel voor automatisch schalen configureren.

![Instellingen voor automatisch schalen.][scale-profile]

Profielen voor automatisch schalen zijn handig om een limiet instellen voor de schaalset. Op deze manier kunt u een consistente prestaties ervaren door een ondergrens schaalwaarde (1) en een voorspelbare uitgaven maximum instellen door in te stellen van een bovengrens (2) hebben.

![Schaalinstellingen in het profiel.][scale-profile2]

Nadat u een profiel hebt gedefinieerd, kunt u regels voor automatisch schalen om te schalen omhoog of omlaag het aantal exemplaren in de groep met werkrollen binnen de grenzen die zijn gedefinieerd door het profiel kunt toevoegen. Regels voor automatisch schalen zijn gebaseerd op de metrische gegevens.

![Schaalregel.][scale-rule]

 Een groep met werkrollen of metrische front-endgegevens kan worden gebruikt voor het definiëren van regels voor automatisch schalen. Deze metrische gegevens zijn dezelfde metrische gegevens kunt u in de grafieken van de blade resource te controleren of instellen van waarschuwingen voor.

## <a name="autoscale-example"></a>Voorbeeld van de functie voor automatisch schalen
Automatisch schalen van een App Service environment kan door te gaan met een scenario voor uw beste worden geïllustreerd.

Dit artikel wordt uitgelegd van de benodigde overwegingen bij het instellen van automatisch schalen. Het artikel helpt u bij de interacties die aan de orde komen als u automatisch schalen App Service-omgevingen die worden gehost in App Service-omgeving.

### <a name="scenario-introduction"></a>Scenario-Inleiding
Frank is een sysadmin voor een onderneming die een deel van de werklasten die hij beheert is gemigreerd naar een App Service-omgeving.

De App Service-omgeving is geconfigureerd voor handmatig schalen als volgt:

* **Front-ends:** 3
* **Groep met werkrollen 1**: 10
* **Groep met werkrollen 2**: 5
* **Groep met werkrollen 3**: 5

Groep met werkrollen 1 wordt gebruikt voor werkbelastingen voor productie, terwijl de groep met werkrollen 2 en 3 groep met werkrollen worden gebruikt voor kwaliteitscontrole (QA) en werkvolumes voor ontwikkeling.

De App Service-plannen voor QA en dev zijn ingesteld op handmatig schalen. De productie-App Service-plan is ingesteld op automatisch schalen te bekommeren om variaties in de belasting en het verkeer.

Frank is erg bekend zijn met de toepassing. Hij weet dat de piekuren voor taakverdeling tussen 9:00 uur en 18:00:00 uur zijn omdat dit een line-of-business (LOB)-toepassing die werknemers gebruiken terwijl ze op kantoor zijn. Gebruik komt hierna als gebruikers zijn klaar voor die dag. Buiten de piekuren is er nog steeds bepaalde laden omdat gebruikers toegang hebben tot de app op afstand met behulp van hun mobiele apparaten of home-pc's. De productie-App Service-plan is al geconfigureerd voor automatisch schalen op basis van CPU-gebruik met de volgende regels:

![Specifieke instellingen voor LOB-app.][asp-scale]

| **Profiel voor automatisch schalen: weekdagen: App Service-plan** | **Profiel voor automatisch schalen: tijdens het weekend: App Service-plan** |
| --- | --- |
| **Naam:** Profiel weekdag |**Naam:** Weekend profiel |
| **Schaal:** Regels voor planning en prestaties |**Schaal:** Regels voor planning en prestaties |
| **Profiel:** Weekdagen |**Profiel:** Weekend |
| **Type:** Terugkeerpatroon |**Type:** Terugkeerpatroon |
| **Doelbereik van:** 5 tot 20 exemplaren |**Doelbereik van:** 3 tot en met 10 exemplaren |
| **Aantal dagen:** Maandag, dinsdag, woensdag, donderdag, vrijdag |**Aantal dagen:** Zaterdag, zondag |
| **Begintijd:** 9:00 UUR |**Begintijd:** 9:00 UUR |
| **Tijdzone:** UTC-08 |**Tijdzone:** UTC-08 |
|  | |
| **Regel voor automatisch schalen (Omhoog schalen)** |**Regel voor automatisch schalen (Omhoog schalen)** |
| **bron:** Productie (App Service-omgeving) |**bron:** Productie (App Service-omgeving) |
| **Metriek:** CPU-percentage |**Metriek:** CPU-percentage |
| **Bewerking:** Meer dan 60% |**Bewerking:** Meer dan 80% |
| **Duur:** 5 minuten |**Duur:** 10 minuten |
| **Tijdverzameling:** Gemiddeld |**Tijdverzameling:** Gemiddeld |
| **Actie:** Aantal verhogen met 2 |**Actie:** Aantal verhogen met 1 |
| **Afkoelen (minuten):** 15 |**Afkoelen (minuten):** 20 |
|  | |
| **Regel voor automatisch schalen (schaal omlaag)** |**Regel voor automatisch schalen (schaal omlaag)** |
| **bron:** Productie (App Service-omgeving) |**bron:** Productie (App Service-omgeving) |
| **Metriek:** CPU-percentage |**Metriek:** CPU-percentage |
| **Bewerking:** Minder dan 30% |**Bewerking:** Minder dan 20% |
| **Duur:** 10 minuten |**Duur:** 15 minuten |
| **Tijdverzameling:** Gemiddeld |**Tijdverzameling:** Gemiddeld |
| **Actie:** Aantal verlagen met 1 |**Actie:** Aantal verlagen met 1 |
| **Afkoelen (minuten):** 20 |**Afkoelen (minuten):** 10 |

### <a name="app-service-plan-inflation-rate"></a>De snelheid van App Service-plan
App Service-plannen die zijn geconfigureerd om automatisch schalen te doen met een maximale prijs per uur. Deze snelheid kan worden berekend op basis van de waarden geleverd op de regel voor automatisch schalen.

Informatie over en het berekenen van de *App Service-plan de rate* is belangrijk voor App Service-omgeving voor automatisch schalen, omdat wijzigingen aan een groep met werkrollen niet onmiddellijk.

De App Service-plan de snelheid wordt als volgt berekend:

![App Service-plan tarief voor de berekening.][ASP-Inflation]

Op basis van de functie voor automatisch schalen: omhoog schalen-regel voor het profiel van de weekdag van de productie-App Service-plan:

![App Service-plan de snelheid voor weekdagen op basis van automatisch schalen regel omhoog schalen.][Equation1]

De formule zou in het geval van de functie voor automatisch schalen: omhoog schalen-regel voor het Weekend profiel van de productie-App Service-plan, omgezet in:

![App Service-plan de snelheid voor weekends op basis van automatisch schalen regel omhoog schalen.][Equation2]

Deze waarde kan ook worden berekend voor bewerkingen voor omlaag schalen.

Op basis van de functie voor automatisch schalen: schaal omlaag regel voor het profiel van de weekdag van de productie-App Service-plan, dit zou er als volgt uitzien:

![App Service-plan de snelheid voor weekdagen op basis van automatisch schalen regel omlaag schalen.][Equation3]

De formule zou in het geval van de functie voor automatisch schalen: schaal omlaag regel voor het Weekend profiel van de productie-App Service-plan, omgezet in:  

![App Service-plan de snelheid voor weekends op basis van automatisch schalen regel omlaag schalen.][Equation4]

De productie-App Service-plan kan groeien met een maximale snelheid van acht instanties/uur tijdens de week en vier instanties/uur tijdens het weekend. Deze kunt instanties met een maximale snelheid van vier exemplaren per uur in de week en zes exemplaren/uur tijdens het weekend vrijgeven.

Als meerdere App Service-abonnementen zijn die wordt gehost in een groep met werkrollen, u hebt voor het berekenen van de *totale snelheid van de* als de som van de frequentie van de voor alle App Service-abonnementen die worden gehost in die groep met werkrollen.

![De berekening van de totale snelheid van de voor meerdere App Service-plannen die wordt gehost in een groep met werkrollen.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>De App Service-plan de snelheid voor het definiëren van regels voor automatisch schalen van werknemer pool gebruiken
Werknemer-pools die als host fungeren App Service-plannen die zijn geconfigureerd voor automatisch schalen moeten worden toegewezen een buffer van capaciteit. De buffer kan voor de bewerkingen voor automatisch schalen om te vergroten of verkleinen van de App Service-plan, indien nodig. De minimale buffer zou de berekende totale App Service Plan de frequentie.

Omdat App Service-omgeving schaalbewerkingen even duren om toe te passen, moet elke wijziging rekening gehouden met meer aanvraag-wijzigingen die optreden kunnen, terwijl een schaalbewerking uitgevoerd wordt. Om te voldoen aan deze latentie, raden wij aan dat u het berekende totale aantal App Service Plan de tarief als het minimum aantal exemplaren die worden toegevoegd voor elke bewerking voor automatisch schalen.

Met deze informatie kunt Frank het profiel van de volgende voor automatisch schalen en regels definiëren:

![De profiel-regels voor automatisch schalen voor LOB-voorbeeld.][Worker-Pool-Scale]

| **Profiel voor automatisch schalen: weekdagen** | **Profiel voor automatisch schalen: tijdens het weekend** |
| --- | --- |
| **Naam:** Profiel weekdag |**Naam:** Weekend profiel |
| **Schaal:** Regels voor planning en prestaties |**Schaal:** Regels voor planning en prestaties |
| **Profiel:** Weekdagen |**Profiel:** Weekend |
| **Type:** Terugkeerpatroon |**Type:** Terugkeerpatroon |
| **Doelbereik van:** 13-25 exemplaren |**Doelbereik van:** 6 tot en met 15-instanties |
| **Aantal dagen:** Maandag, dinsdag, woensdag, donderdag, vrijdag |**Aantal dagen:** Zaterdag, zondag |
| **Begintijd:** 7:00 UUR |**Begintijd:** 9:00 UUR |
| **Tijdzone:** UTC-08 |**Tijdzone:** UTC-08 |
|  | |
| **Regel voor automatisch schalen (Omhoog schalen)** |**Regel voor automatisch schalen (Omhoog schalen)** |
| **bron:** Worker-groep 1 |**bron:** Worker-groep 1 |
| **Metriek:** WorkersAvailable |**Metriek:** WorkersAvailable |
| **Bewerking:** Minder dan 8 |**Bewerking:** Minder dan 3 |
| **Duur:** 20 minuten |**Duur:** 30 minuten |
| **Tijdverzameling:** Gemiddeld |**Tijdverzameling:** Gemiddeld |
| **Actie:** Aantal verhogen met 8 |**Actie:** Aantal verhogen met 3 |
| **Afkoelen (minuten):** 180 |**Afkoelen (minuten):** 180 |
|  | |
| **Regel voor automatisch schalen (schaal omlaag)** |**Regel voor automatisch schalen (schaal omlaag)** |
| **bron:** Worker-groep 1 |**bron:** Worker-groep 1 |
| **Metriek:** WorkersAvailable |**Metriek:** WorkersAvailable |
| **Bewerking:** Groter is dan 8 |**Bewerking:** Groter dan 3 |
| **Duur:** 20 minuten |**Duur:** 15 minuten |
| **Tijdverzameling:** Gemiddeld |**Tijdverzameling:** Gemiddeld |
| **Actie:** Aantal verlagen met 2 |**Actie:** Aantal verlagen met 3 |
| **Afkoelen (minuten):** 120 |**Afkoelen (minuten):** 120 |

De doel-bereik dat is gedefinieerd in het profiel wordt berekend door de minimale exemplaren die zijn gedefinieerd in het profiel voor de App Service-plan + buffer.

Het maximum aantal bereik is de som van alle maximum bereiken voor alle App Service-abonnementen die worden gehost in de groep met werkrollen.

Het aantal verhogen voor de schaal van regels moet worden ingesteld op ten minste 1 X de App Service-Plan de snelheid voor scale-.

Aantal verlagen kan worden aangepast op een andere waarde tussen 1/2 X of 1 X de App Service-Plan de snelheid voor schaal omlaag.

### <a name="autoscale-for-front-end-pool"></a>Automatisch schalen voor de front-end-pool
Regels voor front-automatisch schalen zijn eenvoudiger dan voor werknemersgroepen. Primair, moet u  
Zorg ervoor dat deze duur van de meting en de afkoeltijd timers Houd rekening met schaalbewerkingen op een App Service-plan zijn niet onmiddellijk.

Voor dit scenario weet Frank dat de frequentie van fouten wordt verhoogd wanneer de front-ends tot 80% CPU-gebruik en Hiermee stelt u de regel voor automatisch schalen om te verhogen instanties als volgt:

![Instellingen voor automatisch schalen voor de front-end-pool.][Front-End-Scale]

| **Front-ends met het profiel voor automatisch schalen:** |
| --- |
| **Naam:** Front-ends met automatisch schalen |
| **Schaal:** Regels voor planning en prestaties |
| **Profiel:** elke dag |
| **Type:** Terugkeerpatroon |
| **Doelbereik van:** 3 tot en met 10 exemplaren |
| **Aantal dagen:** elke dag |
| **Begintijd:** 9:00 UUR |
| **Tijdzone:** UTC-08 |
|  |
| **Regel voor automatisch schalen (Omhoog schalen)** |
| **bron:** Front-end-pool |
| **Metriek:** CPU-percentage |
| **Bewerking:** Meer dan 60% |
| **Duur:** 20 minuten |
| **Tijdverzameling:** Gemiddeld |
| **Actie:** Aantal verhogen met 3 |
| **Afkoelen (minuten):** 120 |
|  |
| **Regel voor automatisch schalen (schaal omlaag)** |
| **bron:** Worker-groep 1 |
| **Metriek:** CPU-percentage |
| **Bewerking:** Minder dan 30% |
| **Duur:** 20 minuten |
| **Tijdverzameling:** Gemiddeld |
| **Actie:** Aantal verlagen met 3 |
| **Afkoelen (minuten):** 120 |

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
