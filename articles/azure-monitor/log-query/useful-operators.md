---
title: Nuttige operators in Azure Monitor query's bijgehouden | Microsoft Docs
description: Algemene functies te gebruiken voor verschillende scenario's in Azure Monitor logboeken-query's.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: d11445c3f31f9aced6fdb9783575d10a026de1f0
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000145"
---
# <a name="useful-operators-in-azure-monitor-log-queries"></a>Nuttige operators in Logboeken-query's van Azure Monitor

De onderstaande tabel bevat enkele algemene functies te gebruiken voor verschillende scenario's in Azure Monitor logboeken-query's.

## <a name="useful-operators"></a>Nuttige operatoren

Categorie                                |Functie van de relevante Analytics
----------------------------------------|----------------------------------------
Selectie en in de kolom aliassen            |`project`, `project-away`, `extend`
Tijdelijke tabellen en constanten          |`let scalar_alias_name = …;` <br> `let table_alias_name =  …  …  … ;`| 
Vergelijking en Tekenreeksoperators         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Algemene tekenreeksfuncties                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Veelgebruikte wiskundige functies                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Het parseren van tekst                            |`extract()`, `extractjson()`, `parse`, `split()`
Uitvoer te beperken                         |`take`, `limit`, `top`, `sample`
Datumfuncties                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Groeperen en cumulatie-instellingen                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Samenvoegingen en samenvoegingen                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Volgorde sorteren                             |`sort`, `order` 
Dynamische object (JSON en matrix)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Logische operators                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Machine learning                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Volgende stappen

- Ga via een les op de [logboeken-query's schrijven in Azure Monitor](get-started-queries.md).
