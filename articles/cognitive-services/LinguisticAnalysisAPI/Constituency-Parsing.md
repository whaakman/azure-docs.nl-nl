---
title: Kieskring geparseerd in de taalkundige Analysis-API | Microsoft Docs
description: Meer informatie over hoe kieskring parseren, ook wel bekend als 'woorden parseren van de structuur' zinnen tekst identificeert.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/21/2016
ms.author: lesun
ms.openlocfilehash: bff5e587621e1278c260d555aec280a0f4c7c8a1
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082169"
---
# <a name="constituency-parsing"></a>Kieskring parseren

Het doel van kieskring parseren (ook wel bekend als ' woordgroep structuur parseren') is het identificeren van de items in de tekst.
Dit kan nuttig zijn bij het uitpakken van de informatie uit de tekst.
Klanten mogelijk wilt vinden onderdeelnamen of sleutel zinnen van een grote organisatie van de tekst en de parameters en acties rondom elke dergelijke woordgroep zien.

## <a name="phrases"></a>Woordgroepen

Aan de linguïst een *woordgroep* is meer dan alleen een reeks woorden.
Als u een wachtwoordzin, heeft een groep woorden samen tot een specifieke rol spelen in de zin bent gekomen.
Groep woorden die kan worden verplaatst samen of vervangen als geheel en zinnen beheersen en grammaticafouten moet blijven.

Houd rekening met de zin

> Ik wil een nieuwe hybride auto met Bluetooth vinden.

Deze zin bevat de woordgroep zelfstandig naamwoord: 'een met Bluetooth nieuwe hybride auto'.
Hoe weet we dit is een wachtwoordzin?
We kunnen de zin (enigszins poetically) herschrijven door hele woordgroep verplaatsen op de voorgrond:

> Een nieuwe hybride auto met Bluetooth die ik wil vinden.

Of we kan woordgroep vervangen door een wachtwoordzin met een vergelijkbare functie en betekenis zoals 'een fraai nieuwe auto':

> Ik wil een fraai nieuwe auto vinden.

Als we verzameld in plaats daarvan andere subset van woorden, worden deze taken vervanging zou leiden tot vreemd of onleesbare zinnen.
Houd rekening met wat er gebeurt wanneer we 'vinden een nieuw' op de voorgrond verplaatsen:

> Zoeken naar een nieuwe die ik wil hybride auto met Bluetooth.

De resultaten is zeer moeilijk worden gelezen en begrepen.

Het doel van een parser is alle dergelijke woordgroepen vinden.
Opmerkelijk is in natuurlijke taal vaak de zinnen worden genest in een andere.
Een natuurlijke representatie van deze zinnen is een structuur, zoals de volgende:

![Boomstructuur](./Images/tree.png)

In deze structuur zijn gemarkeerd als 'NP' vertakkingen zelfstandig naamwoord zinnen.
Er zijn verschillende dergelijke zinnen: *ik*, *een nieuwe hybride auto*, *Bluetooth*, en *een nieuwe hybride auto met Bluetooth*.

## <a name="phrase-types"></a>Woordgroep typen

| Label | Beschrijving | Voorbeeld |
|-------|-------------|---------|
|ADJP   | Bijvoeglijke naamwoorden woordgroep | 'dus ruwe' |
|ADVP   | Bewerkingsparameter woordgroep | 'wissen via' |
|CONJP  | Combinatie woordgroep | ', evenals' |
|TOTAAL AANTAL   | Fragment, gebruikt voor onvolledige of verloop invoer | 'Ten zeerste aanbevolen...' |
|INTJ   | Tussenwerpsel | 'Prettige kerstdagen' |
|OVERZICHT    | Lijst met markering, met interpunctie | "#4) ' |
|NAC    | Geen A samenstellende, gebruikt om aan te geven met het bereik van een wachtwoordzin niet van toepassing |  'en voor een groot deel' in 'u alles en getroffen voor een goede' |
|NP | Zelfstandig naamwoord woordgroep | 'een uw achtergebleven pancake' |
|NX | In bepaalde complexe NPs gebruikt voor het hoofd markeren| |
|PP | Prepositional woordgroep| 'in de groep' |
|PRN    | Tussen haakjes| "(dus genoemd)" |
|PRT    | Partikel| 'out' in 'geripte out' |
|QP | Aantal woordgroep (dat wil zeggen, complexe meting/bedrag) binnen een wachtwoordzin zelfstandig naamwoord| 'rond $75' |
|RRC    | Verminderde relatieve-component.| 'nog steeds niet-omgezette' in 'veel problemen nog steeds niet-omgezette' |
|S  | Zin of -component. | "Dit is een zin."
|SBAR   | Onderliggend niveau component, vaak geïntroduceerd door een combinatie van subordinating | "als ik links' in"Bekeken rond als ik links."|
|SBARQ  | Directe vraag geïntroduceerd door een w- of meer-woorden | 'Wat is het punt?' |
|SINV   | Omgekeerde declaratieve zin | "Op geen enkel moment zijn ze op de hoogte." (Houd er rekening mee "hoe het normale onderwerp deze" is verplaatst naar nadat de term 'zijn') |
|SQ | Ja/Nee omgekeerd vraag of main-component van een w-vraag | 'Is ze de auto ophalen?' |
|UCP    | In tegenstelling tot gecoördineerde woordgroep| 'klein en met fouten' (Houd er rekening mee hoe geadjectiveerde en een wachtwoordzin voorzetsel zijn conjoined met 'en')|
|MELDEN VAN STEMPATROONGEBRUIKERS | Term woordgroep | "uitgevoerd in de schoonen" |
|WHADJP | W-bijvoeglijk naamwoord woordgroep | 'hoe ' |
|WHADVP | W bewerkingsparameter woordgroep| 'als' |
|WHNP   | W-zelfstandig naamwoord woordgroep| 'welke achtergebleven', 'hoeveel soep'|
|WHPP   | W prepositional woordgroep| in welk land"'|
|X  | Onbekend, niet zeker of unbracketable.| eerste 'de' in ' de... de soep ' |


## <a name="specification"></a>Specificatie

Structuren hier gebruiken de S-expressies uit de [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42).
