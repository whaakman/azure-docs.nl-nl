---
title: 'Bing aangepaste zoekactie: Decoration markeringen gebruiken om te markeren van tekst | Microsoft Docs'
description: Laat zien hoe tekst orden in zoekresultaten inschakelen.
services: cognitive-services
author: brapel
manager: ehansen
ms.assetid: 5365B568-EA55-4D97-8FBE-0AF60158D4D5
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: d2d0070865aa29257ac827bbb4fc313d87ea7282
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344482"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Met behulp van decoration markeringen tekst markeren

Bing ondersteunt hit markeren die querytermen markeert (of andere voorwaarden die Bing vindt relevante) in de tekenreeksen voor weergave van een deel van de antwoorden. Bijvoorbeeld, een webpagina van `name`, `displayUrl`, en `snippet` velden kunnen de querytermen markeren.

Standaard bevat Bing geen markeringen in weergavereeksen markeren. Zodanig dat de markeringen, omvatten de `textDecorations` queryparameter in uw aanvraag en wordt ingesteld op **true**. Bing markeert de querytermen markeert het begin en einde van de termijn met behulp van de E000 tot en E001 Unicode-tekens. Bijvoorbeeld, als de zoekterm Dinghy varen is en een van beide termen in het veld bestaat, is de term ingesloten in treffers markeren tekens zoals weergegeven in het volgende voorbeeld:  
  
![Markeren](./media/bing-hit-highlighting.PNG) 

Voordat de tekenreeks in de gebruikersinterface wordt weergegeven, zou u de Unicode-tekens vervangen door de tekens die geschikt voor de weergave-indeling zijn. Bijvoorbeeld, als de tekst worden weergegeven als HTML, u mogelijk markeert de zoekterm door vervangen E000 tot met < b\> en E001 met < /b\>. Als u niet wilt opmaak toe te passen, verwijdert u de markeringen van de tekenreeks. 

Bing biedt de mogelijkheid van het gebruik van Unicode-tekens of HTML-codes als markeringen. Als u wilt opgeven welke markeringen te gebruiken, zijn de `textFormat` queryparameter. Als u de inhoud met Unicode-tekens, ingesteld `textFormat` in Raw (de standaardinstelling) en markeert de inhoud met HTML-codes ingesteld `textFormat` in HTML. 
  
Als `textDecorations` is **true**, Bing kan de volgende markeringen in weergavereeksen van antwoorden opnemen. Als er geen HTML-equivalent is, moet de HTML-tabelcel is leeg.

|Unicode|HTML|Beschrijving
|-|-|-
|U + E000 TOT|\<b >|Het markeert het begin van de zoekterm (treffers markering)
|U + E001|\</b >|Het markeert het einde van de zoekterm
|U + E002|\<ik >|Het markeert het begin van cursief inhoud 
|U + E003|\</i >|Het markeert het einde van cursief inhoud
|U + E004|\<br / >|Een regeleinde markeert
|U + E005||Het markeert het begin van een telefoonnummer
|U + E006||Het markeert het einde van een telefoonnummer
|U + E007||Het markeert het begin van een adres
|U + E008||Het markeert het einde van een adres
|U + E009|\&nbsp;|Een vaste spatie markeert
|U + E00C|\<strong >|Het markeert het begin van vet inhoud
|U + E00D|\</ strong >|Het markeert het einde van vet inhoud
|U + E00E||Het markeert het begin van de inhoud waarvan de achtergrond minder zijn dan de omringende achtergrond moet
|U + E00F||Het markeert het einde van inhoud waarvan de achtergrond minder zijn dan de omringende achtergrond moet
|U + E010||Het markeert het begin van de inhoud waarvan achtergrond dan de omringende achtergrond Donkere zijn mag
|U + E011||Het markeert het einde van inhoud waarvan achtergrond dan de omringende achtergrond Donkere zijn mag
|U + E012|\<DEL >|Het markeert het begin van de inhoud die moet worden gevonden door
|U + E013|\</DEL >|Het markeert het einde van inhoud die moet worden gevonden door
|U + E016|\<Sub >|Het markeert het begin van subscript inhoud
|U + E017|\</ sub >|Het markeert het einde van subscript inhoud
|U + E018|\<sup >|Het markeert het begin van superscript inhoud
|U + E019|\</ sup >|Het markeert het einde van superscript inhoud

Het volgende voorbeeld wordt een `Computation` antwoord dat subscript markeringen voor een zoekterm log(2) bevat. De `expression` veld bevat de markering alleen als ' textDecoration is **true**.

![berekening markeringen](./media/bing-markers-computation.PNG) 

Als de aanvraag orden niet hebt aangevraagd, kan de expressie log10(2) zou zijn. 
  
