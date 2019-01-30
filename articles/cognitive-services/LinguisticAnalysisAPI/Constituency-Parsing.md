---
title: Frasestructuur parseren - linguïstische analyse-API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de manier waarop frasestructuur parseren, ook wel bekend als 'woordgroep structuur parseren,"zinnen in de tekst identificeert.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 778005ee321c981fec1d1271cee54229bb3a522f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214168"
---
# <a name="constituency-parsing"></a>Frasestructuur parseren

> [!IMPORTANT]
> De preview van Linguistic Analysis is op 9 augustus 2018 uit gebruik genomen. We raden u aan [Azure Machine Learning-tekstanalysemodules](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) te gebruiken voor tekstverwerking en -analyse.

Het doel van frasestructuur parseren (ook wel bekend als ' woordgroep structuur parseren') is het identificeren van de items in de tekst.
Dit kan nuttig zijn bij het uitpakken van informatie uit tekst.
Klanten kunnen ervoor om onderdeelnamen of sleuteltermen uit een grote instantie van de tekst te vinden en om te zien van de parameters en acties rondom elke dergelijke woordgroep.

## <a name="phrases"></a>Zinnen

Op een taalkundige een *woordgroep* is meer dan alleen een reeks woorden.
Als u een zin, heeft een reeks woorden samen tot een specifieke rol spelen in de zin bent gekomen.
Groep woorden die kan worden samen verplaatst of vervangen als geheel, en de zin moet blijven beheersen en grammaticale.

Houd rekening met de zin

> Ik wil een nieuwe hybride auto met Bluetooth vinden.

Deze zin bevat de woordgroep zelfstandig naamwoord: 'een met Bluetooth nieuwe hybride voertuig'.
Hoe weet ik dat dit een wachtwoordzin is?
We kunnen de zin (enigszins poetically) herschrijven door over te stappen van hele woordgroep aan het begin:

> Een nieuwe hybride auto met Bluetooth-ik wil vinden.

Of we woordgroep kan vervangen door een wachtwoordzin met een vergelijkbare functie en betekenis zoals 'een fraai nieuwe auto':

> Ik wil een fraai nieuwe auto.

Als we opgehaald in plaats daarvan verschillende subset van woorden, worden deze taken vervanging zou leiden tot vreemd of onleesbare zinnen.
Houd rekening met wat er gebeurt wanneer we 'vinden een nieuwe' naar voren verplaatsen:

> Zoek een nieuwe die ik wil hybride auto met Bluetooth.

De resultaten is het zeer moeilijk worden gelezen en begrepen.

Het doel van een parser is om alle dergelijke woordgroepen vinden.
Opmerkelijk, in natuurlijke taal, de zinnen zijn meestal zijn genest binnen elkaar.
Een natuurlijke weergave van de volgende zinnen is een structuur, zoals het volgende:

![Boomstructuur](./Images/tree.png)

In deze structuur zijn de branches die zijn gemarkeerd als "NP" zelfstandig naamwoord zinnen.
Er zijn verschillende dergelijke zinnen: *Ik*, *een nieuwe hybride auto*, *Bluetooth*, en *een nieuwe hybride auto met Bluetooth*.

## <a name="phrase-types"></a>Woordgroep typen

| Label | Beschrijving | Voorbeeld |
|-------|-------------|---------|
|ADJP   | Bijvoeglijke naamwoorden woordgroep | "daarom ruwe" |
|ADVP   | Bewerkingsparameter woordgroep | "wissen via" |
|CONJP  | Combinatie woordgroep | ", evenals" |
|FRAG   | Fragment, die wordt gebruikt voor invoer onvolledig is of een verloop | ' Ten zeerste aangeraden...' |
|INTJ   | Tussenwerpsel | "Zeer Blije" |
|LST    | Markering van de lijst, met inbegrip van interpunctie | "#4)" |
|NAC    | Niet A samenstellende, die wordt gebruikt om aan te geven van een zin niet alle scopes |  'en voor een groot deel' in 'u alles en bieden voor een goede' |
|NP | Zelfstandig naamwoord woordgroep | "een uw achtergebleven pancake" |
|NX | In bepaalde complexe NPs gebruikt voor het markeren van de kop| |
|PP | Prepositional woordgroep| "in de groep" |
|PRN    | Tussen haakjes| "(dus genoemd)" |
|PRT    | Particle| 'uit' in 'geripte uit" |
|QP | Aantal woordgroep (dat wil zeggen, complexe meting/bedrag) binnen een zelfstandig naamwoord woordgroep| 'rond $75" |
|RRC    | Verminderde relatieve-component.| 'nog steeds niet-omgezette' in 'aantal problemen met nog steeds niet-omgezette' |
|S  | Component of zin. | "Dit is een zin."
|SBAR   | Onderliggend niveau component, vaak geïntroduceerd door een subordinating combinatie | "als ik naar links' in"Ik hebben wat als ik."|
|SBARQ  | Directe vraag geïntroduceerd door een w- of - meer woorden | "Wat was het punt?" |
|SINV   | Omgekeerde declaratieve zin | "Op geen enkel moment zijn ze op de hoogte." (Houd er rekening mee "hoe het onderwerp van de normale deze" is verplaatst naar nadat de term 'zijn") |
|SQ | Ja/Nee omgekeerde vraag of de main-component van een w-vraag | "Ze krijg de auto?" |
|UCP    | In tegenstelling tot gecoördineerde woordgroep| 'kleine en met fouten' (Houd er rekening mee hoe geadjectiveerde en een woordgroep voorzetsel zijn conjoined met 'en')|
|VP | Term woordgroep | "uitgevoerd in het bos" |
|WHADJP | W-bijvoeglijk naamwoord woordgroep | "het voelt" |
|WHADVP | W-bewerkingsparameter woordgroep| "when" |
|WHNP   | W-zelfstandig naamwoord woordgroep| "welke achtergebleven", "hoeveel soep"|
|WHPP   | W prepositional woordgroep| 'in welk land"|
|X  | Onbekend, niet zeker of unbracketable.| eerste 'de' in ' de... de soep " |


## <a name="specification"></a>Specificatie

Structuren hier gebruik van de S-expressies uit de [Penn Treebank](https://catalog.ldc.upenn.edu/LDC99T42).
