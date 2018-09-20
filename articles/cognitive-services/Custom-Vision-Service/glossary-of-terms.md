---
title: Verklarende woordenlijst - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Verklarende woordenlijst voor Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 05/08/2017
ms.author: anroth
ms.openlocfilehash: ae68db3de5d1f7eaacbe355133b9b7b61f145f04
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363408"
---
# <a name="glossary-of-terms-for-custom-vision-service"></a>Verklarende woordenlijst voor Custom Vision Service

Hier volgen enkele termen die wordt gebruikt voor Custom Vision Service, en hun betekenis.

## <a name="classifier"></a>Classificatie

Een classificatie is een model dat u met behulp van Custom Vision Service bouwen, met behulp van een paar trainingsafbeeldingen. Zodra u klaar bent met het trainen van een nieuwe classificatie, ontvangt u een evaluatie-eindpunt (HTTPS) die u aan uw app toevoegen kunt. Elke classificatie die u maakt in een eigen project is en u kunt alle projecten weergeven nadat u zich hebt aangemeld.

## <a name="domain"></a>Domain

Als u een project maakt, selecteert u een "domein" voor dat project. Het domein optimaliseert een classificatie voor een specifiek type object in uw installatiekopie. Bijvoorbeeld, als uw scenario voor het classificeren van tussen de afbeeldingen van apple cirkeldiagram ten opzichte van installatiekopieën van carrot taart, selecteer het domein 'Voeding'. Als u niet welk domein om te kiezen weet, selecteert u "Generic" domein.

- **Het domein Food.** Geoptimaliseerd voor gerechten u in een restaurant ziet. Het is niet geoptimaliseerd voor het herkennen van afzonderlijke vruchten of fruit. Als u wilt voor het classificeren van foto's van individuele vruchten of fruit, gebruikt u de algemene domein voor dat doel.
- **Het domein oriëntatiepunt.** Geoptimaliseerd voor het herkennen van oriëntatiepunten, natuurlijke en kunstmatige. Dit domein werkt het beste als de oriëntatiepunt duidelijk zichtbaar zijn in de foto, is zelfs als de oriëntatiepunt iets vorm van een groep mensen die zich ervoor bevinden.
- **Het domein van een Retailanalyse.** Geoptimaliseerd voor het classificeren afbeeldingen in een winkelwagen catalogus of webwinkels zijn. Als u wilt dat hoge precisie bij het classificeren van japonnen, broeken, shirt, enz., gebruikt u het domein van een Retailanalyse.
- **Het volwassenen domein.** Geoptimaliseerd voor beter definiëren tussen de inhoud voor volwassenen en niet-volwassene inhoud. Bijvoorbeeld, als u blokkeren van installatiekopieën van mensen in bad kleuren wilt, kunt dit domein u een aangepaste classificatie om dit te doen maken.
- **De algemene domein.** Geschikt voor een brede verscheidenheid aan taken van installatiekopie-classificatie.

De modellen die worden gegenereerd door **comprimeren domeinen** kunnen worden geëxporteerd met de functionaliteit voor het exporteren van iteratie. Ze zijn geoptimaliseerd voor de beperkingen van realtime classificatie op mobiele apparaten. Classificaties die zijn gebouwd met een compact domein mogelijk iets minder nauwkeurig een standaard domein met dezelfde hoeveelheid trainingsgegevens. Een afweging is dat ze klein genoeg moet lokaal in bijna realtime worden uitgevoerd. 

## <a name="training-image"></a>Afbeelding van de training

Voor het maken van een classificatie van hoge precisie moet Custom Vision Service verschillende trainingsafbeeldingen. Een afbeelding training is een foto van de installatiekopie die u wilt dat Custom Vision Service om te classificeren. Als u wilt classificeren sinaasappels, moet u zou bijvoorbeeld verschillende afbeeldingen van sinaasappels uploaden naar de Custom Vision Service zodat de service te maken van een classificatie waarbij sinaasappels kan herkennen. U wordt aangeraden ten minste 30 afbeeldingen per tag.

## <a name="iteration"></a>Herhaling

Elke keer dat u Train of opnieuw trainen van uw classificatie, maken van een nieuwe versie van het model. We bewaren verschillende afgelopen iteraties groeit zodat u uw voortgang vergelijken na verloop van tijd. U kunt elke iteratie die niet meer nuttig voor u verwijderen. Houd er rekening mee dat het verwijderen van een iteratie permanent is en u ook alle afbeeldingen en wijzigingen die uniek voor deze herhaling zijn verwijderen. 

## <a name="workspace"></a>Werkruimte

Uw werkruimte bevat alle installatiekopieën in uw trainingen en toont alle wijzigingen van de meest recente iteratie zoals verwijderd of installatiekopieën toegevoegd. Wanneer u uw classificatie trainen, maakt u een nieuwe versie van de classificatie, met behulp van de afbeeldingen die aanwezig zijn in uw werkruimte.

## <a name="tags"></a>Tags

Tags gebruiken om te labelen van de objecten in uw trainingen-installatiekopieën. Als u een classificatie voor het identificeren van honden en pony's maakt, plaatst u een 'hond'-tag op afbeeldingen met honden, een 'pony'-tag op afbeeldingen met pony's, en zowel een 'hond' en een 'pony'-tag op afbeeldingen die een hond zowel een pony bevatten.

## <a name="evaluation"></a>Evaluatie

Nadat u uw classificatie hebt getraind, kunt u een andere afbeelding voor de evaluatie kunt indienen met behulp van de automatisch gegenereerde https-eindpunt. De gehanteerde classificatie retourneert een set met voorspelde tags in de volgorde van vertrouwen.

## <a name="predictions"></a>Voorspellingen

Als uw classificatie nieuwe afbeeldingen accepteert voor het classificeren, worden de afbeeldingen die voor u opgeslagen. U kunt deze installatiekopieën gebruiken voor het verbeteren van de precisie van de classificatie door te labelen correct de onjuiste voorspelde afbeeldingen. U kunt deze nieuwe installatiekopieën vervolgens gebruiken voor het opnieuw trainen van uw classificatie.

## <a name="precision"></a>Precisie

Wanneer het classificeren van een afbeelding, hoe waarschijnlijk is uw classificatie voor het classificeren van de installatiekopie goed? Uit alle installatiekopieën waarmee u de classificatie (honden en pony's) te trainen, welk percentage het model krijg correct? 99 juiste tags uit 100 afbeeldingen biedt een precisie van 99%.

## <a name="recall"></a>Terughalen

Uit alle installatiekopieën die moeten correct ingedeeld, hoeveel uw classificeerder leren herkennen correct? Een Terugroepingspercentage van 100% betekent dat, als er 38 gebruiken graag onze eigen installatiekopieën in de installatiekopieën waarmee u de classificatie te trainen, zijn 38 honden gevonden door de classificatie.

## <a name="settings"></a>Instellingen

Er zijn twee soorten instellingen, instellingen en niveau van de gebruiker.

- Niveau van het project instellingen: 
  
  Niveau van het project instellingen gelden voor een project of een classificatie. Ze omvatten:

   - Project-domein
   - Projectnaam
   - Projectbeschrijving
   - Gebruik:
      - Aantal trainingsafbeeldingen
      - Aantal tags die zijn gemaakt
      - Aantal herhalingen gemaakt

- Instellingen voor beveiliging op gebruikersniveau: 
   - Abonnementssleutels: één voor de Training, één voor evaluatie/voorspelling.
   - Gebruik:
      - Het aantal projecten die zijn gemaakt
      - Het aantal evaluatie/voorspelling API-aanroepen.
