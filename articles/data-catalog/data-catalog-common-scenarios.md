---
title: Azure Data Catalog algemene scenario's | Microsoft Docs
description: Een overzicht van veelvoorkomende scenario's voor Azure Data Catalog, met inbegrip van de registratie en detectie van waardevolle gegevensbronnen, self-service business intelligence inschakelen en het vastleggen van bestaande kennis over gegevensbronnen en processen.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: e01e6fa3b9871541bf9573963ced05848a3726e3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-common-scenarios"></a>Algemene scenario’s Azure Data Catalog
Dit artikel biedt een algemene scenario's waar uw organisatie meer waarde niet ophalen uit de bestaande gegevensbronnen kan helpen om Azure Data Catalog.

## <a name="scenario-1-registration-of-central-data-sources"></a>Scenario 1: De registratie van centrale gegevensbronnen
Organisaties hebben vaak veel waardevolle gegevensbronnen. Deze gegevensbronnen bevatten line-of-business, online transactieverwerking (OLTP-systemen)-systemen, Datawarehouses en business intelligence/analytics-databases. Het aantal systemen en de overlapping tussen deze twee, groeit doorgaans na verloop van tijd als nodig is en het bedrijf zelf zich verder via bijvoorbeeld fusies en aankopen ontwikkelen.

Het kan lastig zijn voor leden van de organisatie te weten waar u de gegevens binnen deze gegevensbronnen. Vragen als volgt zijn alle te veel:

* Van de drie HR-systemen binnen het bedrijf gebruikt, waarmee moet ik gebruiken voor het maken van dit type rapport?
* Waar moet ik gaat u naar de gecertificeerde verkoopcijfers ophalen voor het fiscale jaar die zojuist beëindigd?
* Wie moet ik vraag of wat is het proces dat ze toegang krijgen tot het datawarehouse moet ik gebruiken?
* Ik weet niet als deze getallen correct zijn. Die kan ik verzoeken voor inzicht in hoe deze gegevens worden gebruikt moeten voordat ik dit dashboard met mijn team delen?

Op deze en andere vragen kunnen Azure Data Catalog antwoorden bieden. De centrale, hoge waarde, IT beheerd gegevensbronnen die worden gebruikt tussen verschillende organisaties zijn vaak het logische beginpunt voor het invullen van de catalogus. Hoewel elke gebruiker een gegevensbron registreren kan, met de catalogus met de gegevensbronnen die zijn de meest waarschijnlijke waarde opgeven voor het grootste aantal gebruikers kick-started helpt te stimuleren en het gebruik van het systeem. 

Als u zijn aan de slag met Azure Data Catalog, zijn te identificeren en registreren van belangrijke gegevensbronnen die worden gebruikt door veel verschillende teams van gegevensgebruikers uw eerste stap voor succes.

Dit scenario biedt ook een kans om aantekeningen toevoegen aan de waardevolle gegevensbronnen om deze gemakkelijker te begrijpen en te openen. Er is een belangrijk aspect van deze inspanningen om op te nemen informatie over hoe gebruikers toegang tot de gegevensbron kunnen aanvragen. Met Azure Data Catalog, kunt u het e-mailadres van de gebruiker of het team dat verantwoordelijk is voor het beheren van de gegevensbron toegang, koppelingen bestaande hulpprogramma's of documentatie of de vrije tekst die het proces van de aanvraag voor toegang tot beschrijft bieden. Deze informatie helpt de leden die geregistreerde gegevensbronnen detecteren maar die nog geen machtigingen voor toegang tot de gegevens gemakkelijk om toegang te vragen met behulp van de processen die zijn gedefinieerd en beheerd door de eigenaren van de gegevensbron.

## <a name="scenario-2-self-service-business-intelligence"></a>Scenario 2: Self-service business intelligence
Hoewel traditionele oplossingen voor zakelijke business intelligence een waardevol zijn onderdeel van veel organisaties gegevens landschappen blijven, heeft het wijzigen van gelijke tred of-business doorgevoerd selfservice BI meer belangrijk. Met behulp van selfservice BI kunnen informatiemedewerkers en analisten maken hun eigen rapporten, werkmappen en dashboards zonder afhankelijk te zijn op een centrale IT-team of wordt beperkt door de beschikbaarheid en planning dat IT-team.

In selfservicescenario BI's combineren gebruikers doorgaans gegevens uit meerdere bronnen, die mogelijk niet eerder zijn gebruikt voor BI en analyse. Hoewel sommige van deze gegevensbronnen mogelijk al bekend, kan het lastig zijn om te ontdekken wat te doen om te zoeken en potentiële gegevensbronnen voor een bepaalde taak evalueren.

Traditioneel deze discovery-proces is een handmatige: analisten hun peer-netwerkverbindingen gebruiken om u te identificeren anderen die werken met de gegevens wordt aangevraagd. Nadat een gegevensbron is gevonden en gebruikt, het proces wordt herhaald opnieuw voor elke volgende selfservice BI inspanning, met meerdere gebruikers voor het uitvoeren van een redundant handmatig proces van detectie.

Met Azure Data Catalog, kan uw organisatie deze cyclus moeiteloos verbroken. Nadat een gegevensbron via traditionele manier registreren een analist zodat het eenvoudiger kunnen worden gedetecteerd door andere gebruikers in de toekomst. Hoewel de analist meer waarde door de geregistreerde gegevensassets aantekeningen toevoegen kan, hoeft deze aantekening niet worden uitgevoerd op hetzelfde moment als registratie. Gebruikers kunnen bijdragen gedurende een periode, als hun toestaan planningen geleidelijk waarde toe te voegen aan de gegevensbronnen die zijn geregistreerd in de catalogus.

Deze organische groei van de catalogusinhoud is een natuurlijke aanvulling op de vooraf registratie van centrale gegevensbronnen. De catalogus met gegevens die door veel gebruikers moeten vooraf in te vullen mag een motivator voor eerste gebruik en detectie. Hierdoor kunnen gebruikers zich registreren en aantekeningen toevoegen aan extra bronnen mag een manier om te zorgen dat deze en andere leden van de organisatie die betrokken zijn.

Hierbij moet worden opgemerkt dat hoewel dit scenario is gericht op BI selfservice specifiek, dezelfde patronen en uitdagingen voor grootschalige zakelijke BI projecten ook gelden. Met behulp van Data Catalog, kan uw organisatie moeite waarvoor een handmatig proces voor het doorzoeken van de gegevensbron van verbeteren.

## <a name="scenario-3-capturing-tribal-knowledge"></a>Scenario 3: Het vastleggen van de specifieke kennis
Hoe weet u welke gegevens u moet de taak en waar vind ik die gegevens doen?

Als u al uw job een tijdje u waarschijnlijk alleen maar weet. U hebt doorlopen van een gefaseerde leerproces en na verloop van tijd hebt geleerd, over de gegevensbronnen die essentieel voor uw dagelijkse werk zijn.

Wanneer een nieuwe werknemer lid wordt van uw team, hoe deze persoon weet welke gegevens is vereist voor de taak en kan vinden?

Kans zijn de nieuwe persoon geleverd voor u met deze vragen.

Deze lopende overdracht van de specifieke kennis is onderdeel van het detectieproces voor de gegevensbron in organisaties kleine en grote. Senior en ervaren teamleden kennis hebt gemaakt in de afgelopen jaren en nieuwere teamleden hebt geleerd om te vragen wanneer ze vragen hebben. De meest essentiële informatie vaak alleen aanwezig is in de kop van enkele personen en wanneer deze personen op vakantie worden of het team laat, de organisatie te lijden heeft onder.

Gegevens deskundigen ter normaal gesproken een poging om het document hun kennis te delen via e-mail of in Word-documenten op de SharePoint-site van een team. Hoewel deze benadering waardevolle, dit introduceert een nieuwe detectie-probleem: hoe mensen weet welke documentatie bestaat en kan vinden?

Met Azure Data Catalog heeft uw organisatie een centrale locatie voor het opslaan en delen van deze specifieke kennis en zodat u gemakkelijk kan worden gedetecteerd. In Data Catalog, uw experts gegevens rechtstreeks aantekeningen toevoegen aan gegevensassets en bevatten koppelingen naar de bestaande documentatie. Wanneer de catalogus leden van de organisatie gebruiken voor het detecteren van een gegevensbron, vinden niet alleen de bron zelf, maar ook de kennis die voorheen voorkwamen alleen bij de experts van uw organisatie.
