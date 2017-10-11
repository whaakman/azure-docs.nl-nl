---
title: Meld u reguliere expressies in de OMS-logboekanalyse zoekopdrachten | Microsoft Docs
description: U kunt de RegEx-sleutelwoord in logboekanalyse logboek zoekopdrachten aan het filter de resultaten volgens een reguliere expressie.  Dit artikel bevat de syntaxis voor deze expressies met enkele voorbeelden.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: bwren
ms.openlocfilehash: 9746170f157ed5065adc953a31687ff18bd73708
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="using-regular-expressions-to-filter-log-searches-in-log-analytics"></a>Reguliere expressies gebruiken voor het filteren van logboek zoekt in Log Analytics

[Meld u zoekopdrachten](log-analytics-log-searches.md) kunt u gegevens ophalen uit de opslagplaats voor logboekanalyse.  [Filterexpressies](log-analytics-search-reference.md#filter-expressions) kunt u de resultaten van de zoekopdracht volgens specifieke criteria filteren.  De **RegEx** sleutelwoord kunt u een reguliere expressie voor dit filter opgeven.  

Dit artikel bevat informatie over de syntaxis van de reguliere expressie die wordt gebruikt door logboekanalyse.

> [!NOTE]
> U kunt alleen RegEx met doorzoekbare velden.  Zie voor meer informatie over de doorzoekbare velden **veldtypen** in [vinden van gegevens met behulp van logboek zoekopdrachten in logboekanalyse](log-analytics-log-searches.md#use-additional-filters).


## <a name="regex-keyword"></a>RegEx-sleutelwoord

Gebruik de volgende syntaxis gebruiken de **RegEx** -sleutelwoord in een logboek zoekopdracht.  U kunt de andere secties in dit artikel gebruiken om te bepalen van de syntaxis van de reguliere expressie zelf.

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

Als u bijvoorbeeld een reguliere expressie gebruiken om te retourneren van waarschuwing records met een type *waarschuwing* of *fout*, gebruikt u de volgende logboek zoekopdracht.

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Gedeeltelijke overeenkomsten
Houd er rekening mee dat de volledige tekst van de eigenschap moet overeenkomen met de reguliere expressie.  Gedeeltelijke overeenkomsten worden geen records geretourneerd.  Bijvoorbeeld, als u probeert te retourneren van records op een computer met de naam srv01.contoso.com, het volgende logboek zoeken zou **niet** records geretourneerd.

    Computer=RegEx("srv..")

Dit is omdat alleen het eerste deel van de naam overeenkomt met de reguliere expressie.  De volgende twee logboek zoekopdrachten zou records retourneren vanaf deze computer omdat ze overeenkomen met de volledige naam.

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com")

## <a name="characters"></a>Tekens
Geef andere tekens.

| Teken | Beschrijving | Voorbeeld | Voorbeeld komt overeen met |
|:--|:--|:--|:--|
| een | Een exemplaar van het teken. | Computer=RegEx("Srv01.contoso.com") | Srv01.contoso.com |
| . | Een willekeurig teken. | Computer=RegEx("SRV...contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| een? | Nul of één exemplaar van het teken. | Computer RegEx = (' srv01?. Contoso.com') | srv0.contoso.com<br>Srv01.contoso.com |
| een * | Nul of meer exemplaren van het teken. | Computer=RegEx("Srv01*.contoso.com") | srv0.contoso.com<br>Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| een + | Een of meer exemplaren van het teken. | Computer=RegEx("Srv01+.contoso.com") | Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Overeenkomen met een willekeurig teken in de vierkante haken | Computer=RegEx("srv0[123].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [*een*-*z*] | Overeen met een enkel teken in het bereik.  Kan bestaan uit meerdere adresbereiken. | Computer=RegEx("srv0[1-3].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Geen van de tekens in de vierkante haken | Computer=RegEx("srv0[^123].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [^*een*-*z*] | Geen van de tekens in het bereik. | Computer=RegEx("srv0[^1-3].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Overeen met een reeks numerieke tekens. | Computer=RegEx("SRV[01-03].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| @ | Een tekenreeks. | Computer RegEx = ('srv@.contoso.com') | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Meerdere exemplaren van teken
Geef meerdere exemplaren van een bepaalde tekens.

| Teken | Beschrijving | Voorbeeld | Voorbeeld komt overeen met |
|:--|:--|:--|:--|
| een {n} |  *n*instanties van het teken. | Computer=RegEx("BW-Win-sc01{3}.bwren.Lab") | BW-win-sc0111.bwren.lab |
| een {n} |  *n*of meer exemplaren van het teken. | Computer=RegEx("BW-Win-sc01{3,}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab<br>BW-win-sc0111111.bwren.lab |
| {n, m} |  *n*naar *m* exemplaren van het teken. | Computer=RegEx("BW-Win-sc01{3,5}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Logische expressies
Selecteer in meerdere waarden.

| Teken | Beschrijving | Voorbeeld | Voorbeeld komt overeen met |
|:--|:--|:--|:--|
| &#124; | Logische OR.  Resultaat wordt weergegeven als op een van de expressies overeenkomen. | Type waarschuwing AlertSeverity = = RegEx ('waarschuwing &#124; Fout") | Waarschuwing<br>Fout |
| & | Logische AND.  Resultaat wordt weergegeven als op beide expressies overeenkomen | EventData = regex (' (beveiliging.\* &. \*geslaagd. \*)") | Beveiligingscontrole geslaagd |


## <a name="literals"></a>Letterlijke waarden
Speciale tekens niet converteren naar letterlijke tekens.  Deze tekens bevat die voorzien in functionaliteit om reguliere expressies zoals?-\*^\[\]{}\(\)+\|. &.

| Teken | Beschrijving | Voorbeeld | Voorbeeld komt overeen met |
|:--|:--|:--|:--|
| \\ | Zet een speciaal teken om een letterlijke waarde. | Status_CF =\\[fout\\] @<br>Status_CF fout =\\-@ | [Fout] Bestand niet gevonden.<br>Fout-bestand niet gevonden. |


## <a name="next-steps"></a>Volgende stappen

* Raken met [Meld zoekopdrachten](log-analytics-log-searches.md) weergeven en analyseren van gegevens in de opslagplaats logboekanalyse.
