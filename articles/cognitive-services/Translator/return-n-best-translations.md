---
title: Retourneren van N beste vertalingen met de tekst van Microsoft Translator API | Microsoft Docs
description: N beste vertalingen met behulp van de API van Microsoft Translator tekst retourneren.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 3eafe50f69ae1a6748342e64a414ecee4467d0d1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344813"
---
# <a name="how-to-return-n-best-translations"></a>Hoe N beste vertalingen retourneren

> [!NOTE]
> Deze methode is afgeschaft. Het is niet beschikbaar in V3.0 van de API van de tekst conversieprogramma.

De methoden GetTranslations() en GetTranslationsArray() van de API van Microsoft Translator omvatten een optionele Booleaanse vlag 'IncludeMultipleMTAlternatives'.
De methode retourneert maximaal maxTranslations alternatieven waarin de verschillen in de lijst N beste van de NAT-engine wordt geleverd.

De handtekening is:

**Syntaxis**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parameters**

| Parameter | Beschrijving |
|:---|:---|
| appId | **Vereist** als de autorisatie-header wordt gebruikt, laat het veld appid leeg anders geeft u een tekenreeks met 'Bearer' + ' ' + toegangstoken.|
| tekst | **Vereist** een tekenreeks voor de tekst die moet worden omgezet. De grootte van de tekst moet 10000 tekens niet overschrijden.|
| uit | **Vereist** een tekenreeks voor de taalcode van de tekst die moet worden omgezet. |
| tot | **Vereist** een tekenreeks voor de taalcode omzetten van de tekst in. |
| maxTranslations | **Vereist** een geheel getal dat het maximale aantal vertalingen om terug te keren. |
| opties | **Optionele** A TranslateOptions-object dat de onderstaande waarden bevat. Ze zijn optioneel en standaard ingesteld op de meest voorkomende instellingen.

* Categorie: De enige ondersteunde, en de standaard optie 'algemene'.
* ContentType: De enige ondersteunde, en de standaard optie ' text/plain'.
* Status: Gebruikersstatus om te vergelijken aanvraag en -antwoord. Dezelfde inhoud wordt in het antwoord geretourneerd.
* IncludeMultipleMTAlternatives: de vlag om te bepalen of retourneren van meer dan één alternatieven op de MT-engine. Standaard is ingesteld op false en slechts 1 alternatieve bevat.

## <a name="ratings"></a>De classificaties
De classificaties als volgt worden toegepast: de beste automatische vertaling is een beoordeling van 5.
De automatisch gegenereerde (N beste) vertaling alternatieven een classificatie van 0 hebben en een overeenkomst mate van 100 hebben.

## <a name="number-of-alternatives"></a>Aantal alternatieven
Het aantal geretourneerde alternatieven is maximaal maxTranslations, maar kan kleiner zijn.

## <a name="language-pairs"></a>Taal paren
Deze functionaliteit is niet beschikbaar voor vertalingen tussen vereenvoudigd en traditioneel Chinees, beide richtingen. Het is beschikbaar voor alle andere paren van Microsoft Translator ondersteunde taal.
