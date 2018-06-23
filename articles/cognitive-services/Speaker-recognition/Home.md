---
title: Spreker erkenning API | Microsoft Docs
description: Geavanceerde algoritmen gebruikt voor verificatie van de spreker en spreker identificatie met de API spreker opname in cognitieve Services.
services: cognitive-services
author: dwlin
manager: zhang
ms.service: cognitive-services
ms.component: speaker-recognition
ms.topic: article
ms.date: 03/20/2016
ms.author: dwlin
ms.openlocfilehash: 6d5e4e4bbe0cb5e57d2556f680ffcf8d16ee1818
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344669"
---
# <a name="speaker-recognition-api"></a>Sprekerherkenning-API

Welkom bij de Microsoft spreker erkenning API's. Spreker erkenning API's zijn cloud-gebaseerde API's waarmee de meest geavanceerde algoritmen voor sprekeridentificatie verificatie en spreker identificatie. Spreker herkenning kan worden onderverdeeld in twee categorieën worden onderverdeeld: spreker verificatie en spreker identificatie.


## <a name="speaker-verification"></a>Sprekerverificatie


Voice heeft unieke kenmerken die kunnen worden gebruikt om een persoon, net zoals een vingerafdruk te identificeren.  Met behulp van spraak een signaal voor scenario's voor controle en verificatie geworden dat een nieuw innovatieve hulpprogramma – in wezen aanbieden van een niveau omhoog in de beveiliging die de verificatie-ervaring voor klanten vereenvoudigt.

API's voor sprekerverificatie kan automatisch controleren en verifiëren van gebruikers met hun stem of spraak.

### <a name="enrollment"></a>Inschrijving

Inschrijving voor sprekeridentificatie verificatie is tekst-afhankelijke, wat betekent luidsprekers moeten een specifieke wachtwoordzin dat voor gebruik tijdens de inschrijving en de verificatie te kiezen. 

In registratie van de spreker stem is vastgelegd dat een specifieke woordgroep en vervolgens een aantal functies zijn uitgepakt en de gekozen woordgroep wordt herkend. Zowel uitgepakte functies en de gekozen woorden samen formulier een unieke voice-handtekening.

### <a name="verification"></a>Verificatie
###
Verificatie, een invoer spraak en woordgroep worden vergeleken met de inschrijving stem handtekening en woordgroep: om te controleren of ze afkomstig zijn van dezelfde persoon en als ze de juiste woordgroep wordt gezegd.

Voor meer informatie over spreker verificatie, raadpleegt u de API [Sprekeridentificatie - verificatie](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652).

## <a name="speaker-identification"></a>Sprekeridentificatie

API's voor sprekeridentificatie kan automatisch de persoon die spreken in een audiobestand, krijgt een aantal potentiële luidsprekers identificeren. De invoer audio op de opgegeven groep luidsprekers is gekoppeld, en in het geval dat er een overeenkomst is gevonden, de identiteit van de spreker wordt geretourneerd.

Alle luidsprekers moeten gaat u door een registratieproces eerst om op te halen van hun stem is geregistreerd bij het systeem en voice-afdrukken is gemaakt.


### <a name="enrollment"></a>Inschrijving

Registratie voor sprekeridentificatie identificatie is tekst-onafhankelijke, wat betekent dat er geen beperkingen zijn op de spreker in de audio staat. De spreker stem is geregistreerd en een aantal functies worden opgehaald om een unieke voice-handtekening. 


### <a name="recognition"></a>Erkenning

De audio van de onbekende spreker, samen met de potentiële groep luidsprekers, is opgegeven tijdens de herkenning. De invoer stem wordt vergeleken met alle luidsprekers om te bepalen wie stem is, en als er een overeenkomst is gevonden, de identiteit van de spreker wordt geretourneerd.


Voor meer informatie over spreker identificatie, raadpleegt u de API [Sprekeridentificatie - identificatie](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e).
