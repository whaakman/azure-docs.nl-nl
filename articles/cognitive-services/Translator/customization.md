---
title: Aanpassing van vertaling van Microsoft Translator Text-API | Microsoft Docs
description: Gebruik de Microsoft Translator-Hub om uw eigen automatische vertaling-systeem met behulp van uw voorkeur terminologie en stijl.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 05/10/2018
ms.author: v-jansko
ms.openlocfilehash: 9c7083ed0147b221d3d400afe83bb3bd6b6db86e
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574020"
---
# <a name="customize-your-text-translations"></a>Pas uw tekstomzettingen

De preview van Microsoft aangepaste Translator is een functie van de service Microsoft Translator, waarmee gebruikers van Microsoft Translator geavanceerde neurale automatische vertalingen aanpassen tijdens het vertalen van tekst met behulp van de Microsoft Translator Text-API (alleen versie 3). 

De functie kan ook worden gebruikt om aan te passen spraakomzetting gebruikt in combinatie met [Cognitive Services Speech preview](https://docs.microsoft.com/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Aangepaste Translator
U kunt met aangepaste Translator, neurale vertalingssystemen die inzicht in de terminologie die wordt gebruikt in uw eigen zakelijke en branche bouwen. De aangepaste vertaalsysteem wordt vervolgens integreren in bestaande toepassingen, werkstromen en websites. 

### <a name="how-does-it-work"></a>Hoe werkt het?
Gebruik uw eerder vertaalde documenten (folders, webpagina's, documentatie, enzovoort) om te maken van een NAT-systeem die overeenkomt met uw domeinspecifieke terminologie en stijl, beter dan een algemene vertaalsysteem. Gebruikers kunnen TMX, XLIFF, TXT, DOCX en XLSX documenten uploaden.  

Het systeem ook accepteert gegevens die op het niveau van parallelle is, maar nog niet is afgestemd op het niveau van de zin. Als gebruikers toegang tot versies van dezelfde inhoud in meerdere talen, maar in afzonderlijke documenten hebben worden aangepaste Translator kunnen automatisch koppelen van zinnen documenten.  Het systeem kunt ééntalig gegevens ook gebruiken in een van beide of beide talen als aanvulling op de parallelle trainingsgegevens ter verbetering van de vertalingen. 

Het aangepaste systeem is beschikbaar via een gewone aanroep van de Microsoft Translator Text-API met behulp van de categorieparameter.

Gezien het juiste type en de hoeveelheid trainingsgegevens het is niet ongebruikelijk dat u kunt verwachten winsten tussen 5 en 10, of zelfs nog meer BLEU verwijst op vertaalkwaliteit met behulp van aangepaste Translator.

Meer informatie over de verschillende niveaus van aanpassen op basis van beschikbare gegevens kunnen u vinden in de [aangepaste Translator-gebruikershandleiding](http://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Translator-Hub

De verouderde Microsoft Translator-Hub kan worden gebruikt voor de statistische automatische omzetting. [Meer informatie](https://www.microsoft.com/en-us/translator/hub.aspx) 

## <a name="custom-translator-versus-hub"></a>Aangepaste Translator ten opzichte van de Hub

|   | **Hub** | **Aangepaste Translator**|
|:-----|:----:|:----:|
|Functiestatus aanpassen   | Algemene beschikbaarheid  | Preview |
| Tekst-API-versie  | Alleen v2   | Alleen v3 |
| SMT aanpassen | Ja   | Nee | 
| NMT aanpassen | Nee    | Ja |
| Nieuwe uniforme spraakherkenning services aanpassen | Nee    | Ja | 
| [Geen tracering](http://www.aka.ms/notrace) | Ja   | Ja | 

## <a name="collaborative-translations-framework"></a>Gezamenlijke vertalingen Framework

> [!NOTE]
> Vanaf 1 februari 2018 AddTranslation() en AddTranslationArray() zijn niet langer beschikbaar voor gebruik met het V2.0-API van Translator tekst. Deze methoden mislukt en er niets worden geschreven. De V3.0 Translator Text-API biedt geen ondersteuning voor deze methoden.

>Vergelijkbare functionaliteit is beschikbaar in het Hub-API van Translator. Zie [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger). 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een aangepaste taalsysteem met aangepaste Translator instellen](http://aka.ms/CustomTranslatorDocs)
