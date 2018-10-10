---
title: Query-expressies - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over het gebruik van de query-expressies in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: bf6dbde725670030046aad4fccf41554b8d917fe
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901274"
---
# <a name="query-expression-syntax"></a>De syntaxis van de query-expressie

Hebben we gezien dat het antwoord op een **interpreteren** aanvraag bevat een query-expressie. De grammatica die geïnterpreteerd query van de gebruiker een query-expressie voor elke interpretatie gemaakt. Een query-expressie kan vervolgens worden gebruikt voor probleem een **evalueren** aanvraag voor het ophalen van de zoekresultaten van entiteiten.

U kunt ook uw eigen query-expressies maken en ze gebruiken in een **evalueren** aanvraag. Dit is handig als u het bouwen van uw eigen gebruikersinterface die u een query-expressie in reactie op de acties van de gebruiker maakt. Om dit te doen, moet u weten de syntaxis voor query-expressies.  

Elke entiteit-kenmerk dat kan worden opgenomen in een query-expressie heeft een type specifieke gegevens en een set van mogelijke query-operators. De set kenmerken en de ondersteunde operators voor elk kenmerk is opgegeven in [Entiteitkenmerken](EntityAttributes.md). Een query met één waarde moet het kenmerk voor de ondersteuning van de *gelijk is aan* bewerking. Een query voorvoegsel moet het kenmerk voor de ondersteuning van de *StartsWith* bewerking. Numerieke bereik-query's moet het kenmerk voor de ondersteuning van de *IsBetween* bewerking.

Sommige van de entiteitsgegevens worden opgeslagen als samengestelde kenmerken, zoals aangegeven door een punt '.' naam van het kenmerk. Bijvoorbeeld, wordt auteur/relatie informatie weergegeven als een samengesteld kenmerk. Deze 4 onderdelen bevat: AuN, AuId, AfN, AfId. Deze onderdelen zijn verschillende soorten gegevens die een waarde van het kenmerk één entiteit vormen.


**Tekenreeks-kenmerk: Één waarde** (met inbegrip van overeenkomsten op basis van synoniemen)  
TI = 'indexering door latente semantische analyse'  
Samengestelde (AA. AuN = 'gerechtelijke procedure dumais')

**Tekenreeks-kenmerk: Exact één waarde** (komt overeen met alleen de canonieke waarden)  
TI == 'indexering door latente semantische analyse'  
Samengestelde (AA. AuN == 'susan t dumais')
     
**Tekenreekskenmerk: Voorvoegsel**   
TI = 'indexering door latente seman'...  
Samengestelde (AA. AuN =... 'gerechtelijke procedure du')

**Numerieke kenmerk: Één waarde**  
Y = 2010
 
**Numerieke kenmerk: Waarde van het bereik**  
Y &GT; 2005  
Y &GT; = 2005  
Y &LT; 2010  
Y &LT; = 2010  
Y =\[2010, 2012\) (alleen links grenswaarde bevat: 2010, 2011)  
Y =\[2010, 2012\] (beide waarden bevat: 2010, 2011, 2012)
 
**Numerieke kenmerk: Waarde van het Aanpassingsvoorvoegsel**  
Y = '19'... (een numerieke waarde die met 19 begint) 
 
**Datumkenmerk: Één waarde**  
D = "2010-02-04'

**Datumkenmerk: Waarde voor het bereik**  
D &GT; "2010-02-03'  
D = ["2010-02-03', ' 2010-02-05']

**En/of query's:**  
En (Y = 1985, Ti = 'disordered elektronische systemen')  
Of (Ti = 'disordered elektronische systemen', Ti = "fault tolerance principes en praktijken")  
And(OR(Y=1985,Y=2008), Ti = 'disordered elektronische systemen')
 
**Samengestelde query's:**  
Op queryonderdelen van een samengestelde kenmerk moet u het deel van de query-expressie die naar het samengestelde kenmerk in de functie Composite() verwijst. 

Bijvoorbeeld wilt zoeken naar documenten door de naam van auteur, gebruikt u de volgende query:
```
Composite(AA.AuN='mike smith')
```
<br>Om te vragen voor documenten door de auteur van een bepaalde terwijl de auteur bij een bepaalde instelling is, gebruik de volgende query:
```
Composite(And(AA.AuN='mike smith',AA.AfN='harvard university'))
```
<br>De functie Composite() de twee onderdelen van het samengestelde kenmerk gebundeld. Dit betekent dat we alleen documenten waarin een van de auteurs 'Mike Smith' is terwijl hij bij Harvard werkte. 

Om te vragen voor documenten door de auteur van een bepaalde in connecties met (andere) auteurs van een bepaalde instelling, gebruikt u de volgende query:
```
And(Composite(AA.AuN='mike smith'),Composite(AA.AfN='harvard university'))
```
<br>In deze versie omdat Composite() wordt toegepast op de auteur en de relatie afzonderlijk voordat And(), krijgen we alle documenten waarbij een van de auteurs 'Mike Smith' en een van de auteurs connecties 'Harvard' is. Dit klinkt die vergelijkbaar is met het vorige voorbeeld, maar het is niet hetzelfde te doen.

In het algemeen, bekijk het volgende voorbeeld: Er is een samengestelde kenmerk C dat bestaat uit twee onderdelen A en B. Een entiteit kan meerdere waarden hebben voor C. Dit zijn onze entiteiten:
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
<br>komt overeen met entiteiten die een waarde hebben voor C waar C.A is 1 en ook een waarde hebben voor C waar C.B is 2. E1- en E2 komen overeen met deze query.

Opmerking:  
- U kan niet verwijzen naar een deel van een samengestelde kenmerk buiten een Composite()-functie.
- U kunt delen van twee verschillende samengestelde kenmerken binnen dezelfde Composite() functie kan niet verwijzen naar.
- U kunt een kenmerk die geen deel uitmaakt van een samengestelde kenmerk binnen een functie Composite() kan niet verwijzen naar.
