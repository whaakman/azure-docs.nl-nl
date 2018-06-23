---
title: De querysyntaxis expressie in de Academic Knowledge API | Microsoft Docs
description: Informatie over het gebruik van de syntaxis van de query-expressie in de Academic Knowledge API voor cognitieve Microsoft-Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 6ec338fff09954e2f14066ce2b83bc1228794af8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344425"
---
# <a name="query-expression-syntax"></a>Syntaxis van de query-expressie

Wij hebben die het antwoord op een **interpreteren** aanvraag bevat een query-expressie. De grammatica die geïnterpreteerd query van de gebruiker een query-expressie voor elke interpretatie gemaakt. Een query-expressie kan vervolgens worden gebruikt om een **evalueren** aanvraag zoekresultaten entiteit ophalen.

U kunt ook uw eigen query-expressies maken en gebruiken ze op in een **evalueren** aanvraag. Dit is handig als u uw eigen gebruikersinterface die u een query-expressie in reactie op acties van de gebruiker maakt wilt maken. Om dit te doen, moet u weten de syntaxis voor query-expressies.  

Elke entiteit-kenmerk dat kan worden opgenomen in een query-expressie heeft een specifiek gegevenstype en een aantal mogelijke voor query's. De set entiteitskenmerken en ondersteunde operators voor elk kenmerk is opgegeven in [entiteitskenmerken](EntityAttributes.md). Een query met één waarde moet het kenmerk voor de ondersteuning van de *gelijk is aan* bewerking. Een query voorvoegsel moet het kenmerk voor de ondersteuning van de *StartsWith* bewerking. Query's numeriek bereik moet het kenmerk voor de ondersteuning van de *IsBetween* bewerking.

Sommige van de entiteitsgegevens worden opgeslagen als samengestelde kenmerken, zoals aangegeven door een punt '.' in de naam van het kenmerk. Bijvoorbeeld wordt auteur/lidmaatschap van de informatie weergegeven als een samengesteld kenmerk. Deze 4 onderdelen bevat: AuN, AuId, AfN, AfId. Deze onderdelen zijn afzonderlijke gegevenselementen die een kenmerkwaarde één entiteit vormen.


**Tekenreekskenmerk: Één waarde** (inclusief overeenkomsten met synoniemen)  
TI = 'indexeren door latente semantische analyse'  
Samengestelde (AA. AuN = 'verrichten dumais')

**Tekenreekskenmerk: Exacte één waarde** (komt overeen met alleen canonieke waarden)  
TI == 'indexeren door latente semantische analyse'  
Samengestelde (AA. AuN == 'susan t dumais')
     
**: Tekenreeks voorvoegsel kenmerkwaarde**   
TI = 'indexeren door latente seman'...  
Samengestelde (AA. AuN =... 'verrichten du')

**Numerieke kenmerk: De enkele waarde**  
Y = 2010
 
**Numerieke kenmerk: De waarde binnen het bereik**  
Y &GT; 2005  
Y &GT; = 2005  
Y &LT; 2010  
Y &LT; = 2010  
Y =\[2010, 2012\) (bevat alleen links grenswaarde: 2010, 2011)  
Y =\[2010, 2012\] (beide waarden bevat: 2010, 2011, 2012)
 
**Numerieke kenmerk: Waarde van het voorvoegsel**  
Y = '19'... (een numerieke waarde die met 19 begint) 
 
**Datumkenmerk: Enkele waarde**  
D ='2010-02-04'

**Datumkenmerk: De waarde binnen het bereik**  
D &GT;'2010-02-03'  
D = ['2010-02-03', ' 2010-02-05']

**En/of query's:**  
En (Y = 1985, Ti = 'disordered elektronische systemen')  
Of (Ti = 'disordered elektronische systemen', Ti = 'fouttolerantie beginselen en practice')  
And(OR(Y=1985,Y=2008), Ti = 'disordered elektronische systemen')
 
**Samengestelde query's:**  
U moet voor query-componenten van een samengesteld kenmerk, plaatst u het deel van de queryexpressie die naar het samengestelde kenmerk in de functie Composite() verwijst. 

Bijvoorbeeld wilt zoeken naar documenten door de naam van auteur, gebruikt u de volgende query:
```
Composite(AA.AuN='mike smith')
```
<br>Om te vragen voor documenten van een bepaalde auteur tijdens de auteur van het bij een bepaalde instelling, gebruik de volgende query:
```
Composite(And(AA.AuN='mike smith',AA.AfN='harvard university'))
```
<br>De functie Composite() verbindt de twee delen van het samengestelde kenmerk samen. Dit betekent dat er alleen papers waarbij een van de auteurs 'Mike Smith' is ophalen terwijl hij op Harvard. 

Als u wilt zoeken naar documenten van een bepaalde auteur in verwantschappen met (andere) auteurs van een bepaalde instelling, gebruik de volgende query:
```
And(Composite(AA.AuN='mike smith'),Composite(AA.AfN='harvard university'))
```
<br>In deze versie omdat Composite() wordt toegepast op de auteur en de relatie afzonderlijk voordat And(), krijgen we alle documenten, waarbij een van de auteurs 'Mike Smith' en een van de auteurs verwantschappen 'Harvard' is. Dat klinkt vergelijkbaar met het vorige voorbeeld van de query, maar het is niet hetzelfde.

In het algemeen Bekijk het volgende voorbeeld: We hebben een samengestelde kenmerk C dat bestaat uit twee onderdelen A en B. Een entiteit kan meerdere waarden hebben voor C. Dit zijn de entiteiten:
```
E1: C={A=1, B=1}  C={A=1,B=2}  C={A=2,B=3}
E2: C={A=1, B=3}  C={A=3,B=2}
```

<br>De query 
```
Composite(And(C.A=1, C.B=2))
```

<br>komt overeen met alleen de entiteiten die een waarde hebben voor C waar het onderdeel C.A is 1 en het onderdeel C.B 2. Alleen E1 komt overeen met deze query.

De query 
```
And(Composite(C.A=1), Composite(C.B=2))
```
<br>komt overeen met entiteiten met een waarde voor C waarbij C.A 1 is ook een waarde hebben voor C waarbij C.B 2 is. Zowel E1 en E2 overeen met deze query.

Opmerking:  
- U kan niet verwijzen naar een deel van een samengesteld kenmerk buiten een Composite()-functie.
- U kunt delen van twee verschillende samengestelde kenmerken binnen dezelfde Composite()-functie kan niet verwijzen naar.
- U kan niet verwijzen naar een kenmerk die geen deel uitmaakt van een samengesteld kenmerk binnen een functie Composite().
