---
title: Algemene scenario’s Azure Data Catalog
description: Een overzicht van veelvoorkomende scenario's voor Azure Data Catalog, met inbegrip van de registratie en detectie van waardevolle gegevensbronnen, self-service business intelligence inschakelen en het vastleggen van bestaande kennis over gegevensbronnen en processen.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 531c5890cb2164b51d7b383d0b1fdd1b92d9e8eb
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405026"
---
# <a name="azure-data-catalog-common-scenarios"></a>Algemene scenario’s Azure Data Catalog
In dit artikel geeft algemene scenario's waar Azure Data Catalog meer waarde halen uit de bestaande gegevensbronnen van uw organisatie kan helpen.

## <a name="scenario-1-registration-of-central-data-sources"></a>Scenario 1: De registratie van centrale gegevensbronnen
Organisaties hebben vaak veel waardevolle gegevensbronnen. Deze gegevensbronnen zijn line-of-business, online transactieverwerking (OLTP)-systemen, Datawarehouses en business intelligence en analyse-databases. Het aantal systemen en de overlapping tussen, groeit doorgaans na verloop van tijd naarmate uw bedrijfsbehoeften zich ontwikkelen en het bedrijf zelf zich verder via bijvoorbeeld fusies en aankopen ontwikkelt.

Het kan lastig zijn voor leden van de organisatie om te weten waar u de gegevens binnen deze gegevensbronnen. Vragen als volgt zijn alle te veel voorkomt:

* Van de drie HR-systemen binnen het bedrijf gebruikt, welke moet ik gebruiken om dit type rapport te maken?
* Waar moet ik gaat u naar de gecertificeerde verkoopcijfers ophalen voor het belastingjaar die zojuist is afgelopen?
* Wie moet ik vragen of wat is het proces dat ik gebruiken moet voor toegang tot het datawarehouse?
* Ik weet niet als deze waarden correct zijn. Wie kan ik om vragen inzicht op hoe deze gegevens worden gebruikt moeten voordat ik dit dashboard met mijn team delen?

Op deze en andere vragen krijgt u Azure Data Catalog antwoorden. De centrale waardevolle, IT-beheer gegevensbronnen die worden gebruikt in organisaties zijn vaak de logische beginpunt voor het invullen van de catalogus. Hoewel alle gebruikers zich voor een gegevensbron registreren kan, helpt met de catalogus met de gegevensbronnen die voor de waarde voor het grootste aantal gebruikers waarschijnlijk kick-started station acceptatie en gebruik van het systeem. 

Als u bent aan de slag met Azure Data Catalog, kunnen identificeren en registreren van belangrijke gegevensbronnen die worden gebruikt door veel verschillende teams van gebruikers van gegevens met de eerste stap voor succes zijn.

In dit scenario geeft ook een verkoopkans aantekeningen toevoegen aan de waardevolle gegevensbronnen, zodat ze gemakkelijker te begrijpen en te openen. Een belangrijk aspect van deze inspanning, moet u informatie over hoe gebruikers toegang tot de gegevensbron kunnen aanvragen. Met Azure Data Catalog, kunt u het e-mailadres van de gebruiker of team dat verantwoordelijk is voor het beheren van de gegevensbron toegang, koppelingen naar documentatie of bestaande hulpprogramma's of vrije tekst die het proces van de aanvraag voor toegang tot beschrijft opgeven. Deze informatie helpt de leden die geregistreerde gegevensbronnen detecteren, maar die nog niet machtigingen voor toegang tot de gegevens gemakkelijk om toegang te vragen met behulp van de processen die zijn gedefinieerd en beheerd door de eigenaren van de gegevensbron.

## <a name="scenario-2-self-service-business-intelligence"></a>Scenario 2: Self-service business intelligence
Hoewel traditionele zakelijke oplossingen voor business intelligence een waardevol onderdeel te vormen van veel organisaties gegevenslandschappen blijft, heeft het wijzigen van gelijke tred of-business gemaakt selfservice-BI meer belangrijk. Met behulp van selfservice-BI, IT-medewerkers en analisten kunnen maken hun eigen rapporten, werkmappen en dashboards zonder afhankelijkheid van een centrale IT-team of worden beperkt door het schema en de beschikbaarheid dat IT-team.

In selfservice BI-scenario's combineren gebruikers doorgaans gegevens uit meerdere bronnen, waarvan vele mogelijk niet eerder zijn gebruikt voor BI en analyse. Hoewel sommige van deze gegevensbronnen mogelijk al bekend zijn, kan het lastig om te ontdekken wat u moet doen om te zoeken en beoordelen van mogelijke gegevensbronnen voor een bepaalde taak.

Traditioneel deze discovery-proces is een handmatige: analisten gebruiken hun netwerkverbindingen peer voor anderen die werken met de gegevens wordt gezocht. Nadat een gegevensbron is gevonden en gebruikt, het proces wordt herhaald opnieuw voor elke volgende selfservice BI inspanning, met meerdere gebruikers voor het uitvoeren van een redundante handmatig proces van detectie.

Met Azure Data Catalog, kan uw organisatie deze cyclus van inspanning verbroken. Nadat u een gegevensbron via traditionele middelen ontdekte, kan een analist registreren, zodat deze eenvoudiger kunnen worden gedetecteerd door andere gebruikers in de toekomst. Hoewel de analist meer waarde toevoegen kan door de geregistreerde gegevensassets, hoeft deze aantekening niet om te worden toegepast op hetzelfde moment als registratie. Gebruikers kunnen bijdragen gedurende een periode, als hun schema's toestaan, geleidelijk waarde toe te voegen met de gegevensbronnen die zijn geregistreerd in de catalogus.

Deze organische groei van de catalogusinhoud is een natuurlijke aanvulling op de registratie vooraf te betalen van de centrale gegevensbronnen. Vooraf invullen van de catalogus met gegevens die door veel gebruikers moeten mag een motivator voor eerste gebruik en detectie. Hierdoor kunnen gebruikers zich registreren en aantekeningen toevoegen aan aanvullende bronnen is een manier om deze en andere leden van de organisatie die zich bezighoudt te houden.

Het is vermelden waard dat hoewel in dit scenario is gericht op selfservice-BI specifiek, dezelfde patronen en uitdagingen van toepassing op grote schaal corporate BI-projecten ook zijn. Met behulp van Data Catalog, kan uw organisatie zonder veel moeite een handmatig proces voor het doorzoeken van de gegevensbron van omvat verbeteren.

## <a name="scenario-3-capturing-tribal-knowledge"></a>Scenario 3: Bedrijfskennis vastleggen
Hoe weet u welke gegevens u moet uw werk en waar u kunt vinden die gegevens doen?

Als u in uw taak een tijdje is hebt, u waarschijnlijk hoeft te weten. U een geleidelijke leerproces hebt doorlopen, en na verloop van tijd hebt geleerd over de gegevensbronnen die essentieel voor uw dagelijkse werkzaamheden zijn.

Wanneer een nieuwe werknemer lid wordt van uw team, hoe die persoon weten welke gegevens is vereist voor de taak en waar u deze kunt vinden?

Kans zijn de nieuwe persoon komt naar u met deze vragen.

Deze continue overdracht van bedrijfskennis maakt deel uit van het detectieproces voor de gegevensbron in organisaties grote als kleine volumes. Senior en ervaren teamleden zijn gebouwd op basis van kennis in de afgelopen jaren en nieuwere teamleden hebt geleerd om te vragen wanneer ze vragen hebben. De meest essentiële informatie vaak alleen aanwezig is in de hoofden van enkele personen en wanneer deze personen zich op vakantie of het team laat, de organisatie heeft.

Data-experts maken gewoonlijk een poging om het document hun kennis te delen via e-mail of in Word-documenten op de SharePoint-site van een team. Hoewel deze methode nuttig zijn kan, het introduceert een nieuwe detectie-probleem: hoe mensen weten welke documentatie bestaat en waar u deze kunt vinden?

Met Azure Data Catalog heeft uw organisatie een centrale locatie voor het opslaan en delen van deze specifieke kennis en voor het maken van deze gemakkelijk kunnen worden gedetecteerd. In Data Catalog, kunnen de experts van uw gegevens rechtstreeks aantekeningen toevoegen aan gegevensassets en bevatten koppelingen naar bestaande documentatie. Wanneer leden van de organisatie de catalogus voor het detecteren van een gegevensbron, vindt ze niet alleen de bron zelf, maar ook de kennis die eerder alleen in de geesten op het gebied van experts van uw organisatie.
