---
title: Microsoft Translator tekst API vertaling aanpassing | Microsoft Docs
description: De Microsoft Translator Hub gebruiken voor het bouwen van uw eigen machinevertaling-systeem met behulp van uw voorkeur terminologie en stijl.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 05/10/2018
ms.author: v-jansko
ms.openlocfilehash: 1db22a414c41f338c4e7fd6ce9dc7ac739fa9237
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345602"
---
# <a name="customize-your-text-translations"></a>Uw tekstvertalingen aanpassen

De evaluatieversie van Microsoft aangepaste conversieprogramma is onderdeel van de service Microsoft Translator, waarmee gebruikers van Microsoft Translator geavanceerde neural machinevertaling aanpassen bij het vertalen van de tekst met behulp van de API van Microsoft Translator tekst (alleen versie 3). 

De functie kan ook worden gebruikt om aan te passen spraak vertaling gebruikt in combinatie met [cognitieve Services spraak preview](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Aangepaste conversieprogramma
Met aangepaste conversieprogramma, kunt u neural vertaling systemen die de terminologie die gebruikt in uw eigen bedrijven en andere begrijpt bouwen. Het systeem aangepaste vertaling wordt vervolgens integreren met bestaande toepassingen, werkstromen en websites. 

### <a name="how-does-it-work"></a>Hoe werkt het?
Gebruik uw eerder vertaalde documenten (folders, webpagina's, documentatie, enzovoort) voor het bouwen van een systeem vertaling die overeenkomt met uw domeinspecifieke terminologie en stijl, beter dan een algemene vertaling-systeem. Gebruikers kunnen TMX, XLIFF, TXT, DOCX en XLSX-documenten te uploaden.  

Het systeem ook accepteert gegevens die parallel op het niveau, maar nog niet is afgestemd op het niveau van zin. Als gebruikers toegang tot versies van dezelfde inhoud in meerdere talen, maar in afzonderlijke documenten hebben zich aangepaste conversieprogramma om automatisch zinnen meerdere documenten.  Het systeem kan ook ééntalig gegevens in een of beide talen gebruiken als aanvulling op de parallelle trainingsgegevens ter verbetering van de vertalingen. 

Het aangepaste systeem is vervolgens beschikbaar via een gewone aanroep van de Microsoft Translator tekst API met de categorieparameter.

Gegeven het juiste type en de hoeveelheid trainingsgegevens gebeurt niet kunt verwachten toeneemt tussen 5 en 10 of zelfs meer BLEU verwijst van vertaling kwaliteit met behulp van aangepaste conversieprogramma.

Meer informatie over de verschillende niveaus van aanpassingen op basis van beschikbare gegevens vindt u in de [gebruikershandleiding voor de aangepaste conversieprogramma](http://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Translator Hub

Het verouderde dat Microsoft Translator Hub kan worden gebruikt voor statistische machinevertaling vertalen. [Meer informatie](https://www.microsoft.com/en-us/translator/hub.aspx) 

## <a name="custom-translator-versus-hub"></a>Aangepaste conversieprogramma versus Hub

|   | **Hub** | **Aangepaste conversieprogramma**|
|:-----|:----:|:----:|
|Functiestatus voor aanpassing   | Algemene beschikbaarheid  | Preview |
| API-tekstversie  | Alleen v2   | Alleen v3 |
| SMT-aanpassing | Ja   | Nee | 
| NMT aanpassing | Nee    | Ja |
| Nieuwe geïntegreerde spraak services aanpassing | Nee    | Ja | 
| [Er is geen tracering](http://www.aka.ms/notrace) | Ja   | Ja | 

## <a name="collaborative-translations-framework"></a>Samenwerking vertalingen Framework

> [!NOTE]
> Vanaf 1 februari 2018, AddTranslation() en AddTranslationArray() zijn niet langer beschikbaar voor gebruik met het conversieprogramma tekst API V2.0. Deze methoden mislukt en er is niets geschreven. De V3.0 conversieprogramma tekst API biedt geen ondersteuning voor deze methoden.

>Vergelijkbare functionaliteit is beschikbaar in de Hub conversieprogramma API. Zie [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger). 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een aangepaste taalsysteem met behulp van aangepaste conversieprogramma instellen](http://aka.ms/CustomTranslatorDocs)
