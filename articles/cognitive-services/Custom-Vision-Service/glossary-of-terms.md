---
title: Verklarende woordenlijst voor aangepaste Vision-Service - cognitieve Azure-Services | Microsoft Docs
description: Verklarende woordenlijst voor aangepaste visie Service.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/08/2017
ms.author: anroth
ms.openlocfilehash: 871617ce3c1c5a84df746c0c7d87c113b3a6f354
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344909"
---
# <a name="glossary-of-terms-for-custom-vision-service"></a>Verklarende woordenlijst voor aangepaste visie Service

De volgende zijn termen die in aangepaste visie Service, en hun betekenis.

## <a name="classifier"></a>Classificatie

Een classificatie is een model die u met behulp van aangepaste visie Service bouwen, met behulp van een paar training afbeeldingen. Zodra u klaar bent met het trainen van een nieuwe classificatie, ontvangt u een evaluatie-eindpunt (HTTPS) u aan uw app toevoegen kunt. Elke classificatie die u maakt is in een eigen-project en vindt u alle projecten zodra u zich hebt aangemeld.

## <a name="domain"></a>Domein

Als u een project maakt, selecteert u een 'domein' voor dat project. Het domein een classificatie voor een specifiek type van object in de afbeeldingen worden geoptimaliseerd. Bijvoorbeeld, als uw scenario is het classificeren tussen afbeeldingen van apple cirkelsegment versus afbeeldingen van carrot taart, selecteert u het domein 'Voeding'. Als u welk domein om te kiezen weet, selecteert u 'Algemene' domein.

- **Het domein voeding.** Geoptimaliseerd voor schalen u in een restaurant ziet. Het is niet geoptimaliseerd voor het herkennen van afzonderlijke fruit of fruit. Als u classificeren foto's van afzonderlijke fruit of fruit wilt, gebruikt u het algemene domein daarvoor.
- **De kenmerkende-domein.** Geoptimaliseerd voor herkenbare monumenten, natuurlijke en kunstmatige. Dit domein werkt het beste als de kenmerkende duidelijk zichtbaar is in de foto, zelfs als de kenmerkende enigszins vorm van een groep mensen die zich ervoor bevinden.
- **Het Retail-domein.** Geoptimaliseerd voor het classificeren afbeeldingen in een winkelwagen catalogus of de winkelwagen website. Als u wilt dat hoge precisie wanneer classificeren japonnen, broeken, shirt, enz., gebruikt u het Retail-domein.
- **Het volwassenen domein.** Geoptimaliseerd om beter te definiëren tussen inhoud voor volwassenen en niet-volwassene inhoud. Bijvoorbeeld, als u blokkeren afbeeldingen van mensen in bad kleuren wilt, kunt dit domein u voor het bouwen van een aangepaste classificatie kunt u doen.
- **Het algemene domein.** Geschikt voor tal van afbeelding classificatie taken.

De modellen die worden gegenereerd door **comprimeren domeinen** met de functionaliteit voor het exporteren van herhaling kunnen worden geëxporteerd. Ze zijn geoptimaliseerd voor de beperkingen van realtime classificatie op mobiele apparaten. Classificaties gebouwd met een compact domein mogelijk iets minder nauwkeurig een standaard domein met dezelfde mate van trainingsgegevens. Afweging is dat ze zijn kort lokaal in bijna realtime worden uitgevoerd. 

## <a name="training-image"></a>Training installatiekopie

Voor het maken van een hoge precisie classificatie, moet aangepaste visie Service zijn diverse training installatiekopieën. Een afbeelding training is een foto van de installatiekopie die u wilt dat aangepaste visie Service te classificeren. Als u wilt classificeren appels, moet u zou bijvoorbeeld verschillende installatiekopieën van appels uploaden naar aangepaste visie Service zodat de service voor het maken van een classificatie die appels kan herkennen. U wordt aangeraden ten minste 30 afbeeldingen per label.

## <a name="iteration"></a>Herhaling

Elke keer dat u Train of opnieuw trainen uw classificatie, kunt u een nieuwe versie van uw model maken. Wij houden verschillende afgelopen iteraties zodat u uw voortgang vergelijken gedurende een bepaalde periode. U kunt elke herhaling die niet meer nuttig voor u verwijderen. Houd er rekening mee dat het verwijderen van een herhaling permanent is en u alle installatiekopieën of wijzigingen die uniek voor deze herhaling zijn ook verwijderen. 

## <a name="workspace"></a>Werkruimte

Uw werkruimte bevat alle training-installatiekopieën en toont alle wijzigingen van de meest recente herhaling zoals verwijderd of toegevoegd installatiekopieën. Wanneer u uw classificatie training, maakt u een nieuwe versie van de classificatie met behulp van de afbeeldingen die aanwezig zijn in uw werkruimte.

## <a name="tags"></a>Tags

Codes gebruiken om de objecten in uw afbeeldingen training labelen. Als u een classificatie om honden en pony's te identificeren maakt, plaatst u een label 'aquaduct' op afbeeldingen die honden, bevatten een 'onder'-tag op afbeeldingen die pony's, en zowel een 'aquaduct' bevatten en een 'onder'-tag op afbeeldingen die zowel een aquaduct als onder bevatten.

## <a name="evaluation"></a>Evaluatie

Nadat u uw classificatie hebben getraind, kunt u een afbeelding voor evaluatie kunt verzenden met behulp van de automatisch gegenereerde https-eindpunt. De classificatie retourneert een set van voorspelde labels in de volgorde van vertrouwen.

## <a name="predictions"></a>Voorspellingen

Als uw classificatie nieuwe afbeeldingen accepteert te classificeren, slaat deze installatiekopieën van het voor u. U kunt deze installatiekopieën gebruiken voor het verbeteren van de precisie van de classificatie door het juist tagging installatiekopieën van het verkeerd voorspelde. U kunt deze nieuwe installatiekopieën vervolgens gebruiken voor het opnieuw trainen uw classificatie.

## <a name="precision"></a>Precisie

Wanneer het classificeren van een afbeelding, hoe waarschijnlijk is de classificatie voor het classificeren van de afbeelding correct? Alle installatiekopieën gebruikt voor het trainen van de classificatie (honden en pony's), buiten het welk percentage het model ophalen juist? 99 juiste labels buiten 100 afbeeldingen biedt een precisie van 99%.

## <a name="recall"></a>Terughalen

Buiten-alle afbeeldingen die moeten correct ingedeeld, hoeveel is uw classificatie identificatie correct? Terughalen van 100% betekent dat, als er 38 aquaduct afbeeldingen in de installatiekopieën gebruikt voor het trainen van de classificatie, zijn 38 honden gevonden door de classificatie.

## <a name="settings"></a>Instellingen

Er zijn twee soorten instellingen, instellingen en instellingen voor beveiliging op gebruikersniveau.

- Projectniveau instellingen: 
  
  Instellingen van toepassing op een project of classificatie. Deze omvatten:

   - Project-domein
   - Projectnaam
   - Projectbeschrijving
   - Gebruik:
      - Aantal training afbeeldingen
      - Aantal tags die zijn gemaakt
      - Aantal herhalingen is gemaakt

- Instellingen voor beveiliging op gebruikersniveau: 
   - Abonnement-codes: één voor Training, één voor de evaluatie van/voorspelling.
   - Gebruik:
      - Aantal projecten gemaakt
      - Het aantal uitgevoerde evaluatie/voorspelling API-aanroepen.
