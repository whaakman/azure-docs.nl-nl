---
title: Decoration markeringen gebruiken om te markeren van tekst - Bing Custom Search
titlesuffix: Azure Cognitive Services
description: Laat zien hoe om in te schakelen in de reacties op webzoekopdrachten grafische tekstelementen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: maheshb
ms.openlocfilehash: 17297b651d672cee746076d563b5c410390c0fa2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862799"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Met behulp van markeringen decoration markeren van tekst

Bing ondersteunt die querytermen markeert markeren van treffers (of andere voorwaarden die Bing vindt relevante) in de tekenreeksen voor weergave van een aantal van de antwoorden. Bijvoorbeeld een webpagina van `name`, `displayUrl`, en `snippet` velden de querytermen kunnen markeren.

Standaard geen Bing markeren markeringen in tekenreeksen voor weergave. Om op te nemen de markeringen, bevatten de `textDecorations` queryparameter in uw aanvraag en stel deze in op **waar**. Bing markeert de querytermen met behulp van de E000 tot en E001 Unicode-tekens markeert het begin en einde van de termijn. Bijvoorbeeld, als de zoekterm varen Dinghy is en een van beide termen in het veld bestaat, is de term ingesloten in treffers markeren tekens zoals wordt weergegeven in het volgende voorbeeld:  
  
![Treffers markeren](./media/bing-hit-highlighting.PNG) 

Voordat de tekenreeks wordt weergegeven in de gebruikersinterface, zou u de Unicode-tekens vervangen door de tekens die geschikt voor de weergave-indeling zijn. Bijvoorbeeld, als u de tekst als HTML weergeeft, u mogelijk markeren de zoekterm door te vervangen E000 tot met < b\> en E001 met < /b\>. Als u niet wilt opmaak toe te passen, verwijdert u de markeringen van de tekenreeks. 

Bing biedt de mogelijkheid van het gebruik van Unicode-tekens of HTML-codes als markeringen. Als u wilt opgeven welke markeringen te gebruiken, zijn de `textFormat` queryparameter. Als u wilt markeren de inhoud met Unicode-tekens, stel `textFormat` Raw (de standaardinstelling) en de inhoud met HTML-codes markeren, stel `textFormat` in HTML. 
  
Als `textDecorations` is **waar**, Bing omvat mogelijk de volgende markeringen in tekenreeksen voor weergave van antwoorden. Als er geen HTML-equivalent, moet de cel in de HTML-tabel is leeg.

|Unicode|HTML|Description
|-|-|-
|U+E000|\<b>|Het markeert het begin van de queryterm die (treffers markeren)
|U+E001|\</b>|Het markeert het einde van de queryterm
|U+E002|\<i>|Het markeert het begin van cursieve inhoud 
|U+E003|\</i>|Het markeert het einde van cursieve inhoud
|U+E004|\<br/>|Een regeleinde markeert
|U+E005||Het markeert het begin van een telefoonnummer
|U+E006||Het einde van een telefoonnummer
|U+E007||Het markeert het begin van een adres
|U+E008||Het markeert het einde van een adres
|U+E009|\&nbsp;|Markeert een vaste spatie
|U+E00C|\<strong>|Het markeert het begin van vet inhoud
|U + E00D|\</strong>|Het markeert het einde van vet inhoud
|U+E00E||Het markeert het begin van de inhoud waarvan achtergrond minder dan de omringende achtergrond moet
|U + E00F||Het markeert het einde van de inhoud waarvan achtergrond minder dan de omringende achtergrond moet
|U+E010||Het markeert het begin van de inhoud waarvan achtergrond donkerder zijn dan de omringende achtergrond moet
|U+E011||Het markeert het einde van inhoud waarvan achtergrond donkerder zijn dan de omringende achtergrond moet
|U+E012|\<del>|Het markeert het begin van de inhoud die moet worden gevonden door
|U+E013|\</del>|Het markeert het einde van de inhoud die moet worden gevonden door
|U+E016|\<sub>|Het markeert het begin van subscript inhoud
|U+E017|\</sub>|Het markeert het einde van subscript inhoud
|U+E018|\<sup>|Het markeert het begin van superscript inhoud
|U+E019|\</sup>|Het markeert het einde van superscript inhoud

Het volgende voorbeeld wordt een `Computation` antwoord dat subscript markeringen voor een termijn van de query log(2) bevat. De `expression` bevat de markeringen alleen als het veld ' textDecoration is **waar**.

![berekening markeringen](./media/bing-markers-computation.PNG) 

Als de aanvraag geen voor versieringen weergegeven aanvragen heeft, worden de expressie log10(2). 
  
