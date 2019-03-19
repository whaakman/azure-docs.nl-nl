---
title: Naslaginformatie over Azure Service Bus SQLFilter-syntaxis | Microsoft Docs
description: Gegevens over SQLFilter grammatica.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/05/2018
ms.author: spelluru
ms.openlocfilehash: e490c7c24ed38e2988c1f097b09b508746f08178
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58118132"
---
# <a name="sqlfilter-syntax"></a>SQLFilter-syntaxis

Een *SqlFilter* object is een exemplaar van de [SqlFilter klasse](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), en staat voor een SQL-taal op basis van filter-expressie die wordt geëvalueerd op basis van een [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Een SqlFilter ondersteunt een subset van de SQL-92-standaard.  
  
 In dit onderwerp bevat informatie over SqlFilter grammatica.  
  
```  
<predicate ::=  
      { NOT <predicate> }  
      | <predicate> AND <predicate>  
      | <predicate> OR <predicate>  
      | <expression> { = | <> | != | > | >= | < | <= } <expression>  
      | <property> IS [NOT] NULL  
      | <expression> [NOT] IN ( <expression> [, ...n] )  
      | <expression> [NOT] LIKE <pattern> [ESCAPE <escape_char>]  
      | EXISTS ( <property> )  
      | ( <predicate> )  
  
```  
  
```  
<expression> ::=  
      <constant>   
      | <function>  
      | <property>  
      | <expression> { + | - | * | / | % } <expression>  
      | { + | - } <expression>  
      | ( <expression> )  
  
```  
  
```  
<property> :=   
       [<scope> .] <property_name>  
  
```  
  
## <a name="arguments"></a>Argumenten  
  
-   `<scope>` is een optionele tekenreeks die aangeeft van het bereik van de `<property_name>`. Geldige waarden zijn `sys` of `user`. De `sys` waarde geeft aan dat systeem bereik waar `<property_name>` is de naam van een openbare eigenschap van de [BrokeredMessage klasse](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user` gebruikersbereik van geeft aan waar `<property_name>` is een sleutel van de [BrokeredMessage klasse](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) woordenlijst. `user` bereik is het bereik van standaard als `<scope>` is niet opgegeven.  
  
## <a name="remarks"></a>Opmerkingen

Een poging tot toegang tot een niet-bestaande systeemeigenschap is een fout tijdens een poging tot toegang tot de gebruikerseigenschap van een niet-bestaande geen fout is. In plaats daarvan wordt een eigenschap van een niet-bestaande intern geëvalueerd als een onbekende waarde. Een onbekende waarde wordt beschouwd speciaal tijdens de evaluatie van de operator.  
  
## <a name="propertyname"></a>%{Property_Name/  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Argumenten  

 `<regular_identifier>` een tekenreeks is vertegenwoordigd door de volgende reguliere expressie:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
Deze grammatica betekent een tekenreeks die begint met een letter en wordt gevolgd door een of meer onderstrepingsteken/letter/cijfers.  
  
`[:IsLetter:]` betekent dat alle Unicode-tekens die zijn gecategoriseerd als een Unicode-letter. `System.Char.IsLetter(c)` retourneert `true` als `c` een Unicode-letter is.  
  
`[:IsDigit:]` betekent dat alle Unicode-tekens die zijn gecategoriseerd als een decimale cijfers. `System.Char.IsDigit(c)` retourneert `true` als `c` een Unicode-teken.  
  
Een `<regular_identifier>` mag niet een gereserveerd trefwoord.  
  
`<delimited_identifier>` is een tekenreeks die is ingesloten met links/rechts vierkante haken ([]). Een vierkant haakje sluiten wordt weergegeven als twee juiste vierkante haken. Hieronder ziet u voorbeelden van `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
`<quoted_identifier>` is een tekenreeks dubbele aanhalingstekens tussen. Dubbele aanhalingstekens in id wordt weergegeven als twee dubbele aanhalingstekens. Het wordt niet aanbevolen voor id's tussen aanhalingstekens omdat kunnen eenvoudig worden verward met een tekenreeksconstante. Gebruik indien mogelijk een door tekens gescheiden id. Hier volgt een voorbeeld van `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>Patroon  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Opmerkingen
  
`<pattern>` moet een expressie die wordt geëvalueerd als een tekenreeks zijn. Deze wordt gebruikt als een patroon voor de operator LIKE.      Dit kan de volgende tekens bevatten:  
  
-   `%`:  Een willekeurige tekenreeks van nul of meer tekens.  
  
-   `_`: Een willekeurig teken.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Opmerkingen  

`<escape_char>` moet een expressie die wordt geëvalueerd als een tekenreeks met lengte 1. Deze wordt gebruikt als een escape-teken voor de operator LIKE.  
  
 Bijvoorbeeld, `property LIKE 'ABC\%' ESCAPE '\'` komt overeen met `ABC%` in plaats van een tekenreeks is die met begint `ABC`.  
  
## <a name="constant"></a>constante  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Argumenten  
  
-   `<integer_constant>` is een reeks cijfers die zijn niet tussen aanhalingstekens en geen decimale punten bevatten. De waarden worden opgeslagen als `System.Int64` intern, en volgt u de hetzelfde bereik.  
  
     Dit zijn voorbeelden van lange constanten:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>` is een reeks cijfers die zijn niet tussen aanhalingstekens, en een decimaalteken bevatten. De waarden worden opgeslagen als `System.Double` intern, en volgt u de hetzelfde bereik/precisie.  
  
     In een toekomstige versie zal dit nummer kan worden opgeslagen in een ander gegevenstype voor de ondersteuning van exacte aantal semantiek, zodat u niet afhankelijk zijn van het feit de onderliggende gegevens van het type is `System.Double` voor `<decimal_constant>`.  
  
     Hier volgen enkele voorbeelden van decimaal constanten:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>` is een aantal geschreven in wetenschappelijke notatie. De waarden worden opgeslagen als `System.Double` intern, en volgt u de hetzelfde bereik/precisie. Hier volgen enkele voorbeelden van geschatte aantal constanten:  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a>boolean_constant  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a>Opmerkingen  

Booleaanse constanten worden vertegenwoordigd door de trefwoorden **waar** of **FALSE**. De waarden worden opgeslagen als `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Opmerkingen  

Tekenreeksconstanten tussen enkele aanhalingstekens zijn en geen geldige unicodetekens bevatten. Een enkel aanhalingsteken ingesloten in een tekenreeksconstante wordt weergegeven als twee enkele aanhalingstekens.  
  
## <a name="function"></a>functie  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Opmerkingen
  
De `newid()` functie retourneert een **System.Guid** die worden gegenereerd door de `System.Guid.NewGuid()` methode.  
  
De `property(name)` functie retourneert de waarde van de eigenschap waarnaar wordt verwezen door `name`. De `name` waarde kan een geldige expressie die een string-waarde retourneert zijn.  
  
## <a name="considerations"></a>Overwegingen
  
Houd rekening met de volgende [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantiek:  
  
-   Eigenschapnamen zijn niet hoofdlettergevoelig.  
  
-   Operators Volg C# impliciete conversie semantiek indien mogelijk.  
  
-   Systeemeigenschappen zijn openbare eigenschappen beschikbaar zijn in [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) exemplaren.  
  
    Houd rekening met de volgende `IS [NOT] NULL` semantiek:  
  
    -   `property IS NULL` wordt geëvalueerd als `true` als de eigenschap bestaat niet of de waarde van de eigenschap is `null`.  
  
### <a name="property-evaluation-semantics"></a>De eigenschap evaluatie semantiek  
  
- Een poging om te evalueren van een niet-bestaande systeemeigenschap genereert een [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) uitzondering.  
  
- Een eigenschap die niet bestaat, wordt als intern worden geëvalueerd **onbekende**.  
  
  Onbekende evaluatie in rekenkundige operators:  
  
- Voor binaire operators, als op de links en/of rechts van de operands wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
- Voor de unitaire operators, als een operand wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
  Onbekende evaluatie in binaire vergelijkingsoperators:  
  
- Als zowel de linkerkant en/of rechts van de operands wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
  Onbekende evaluatie in `[NOT] LIKE`:  
  
- Als een operand wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
  Onbekende evaluatie in `[NOT] IN`:  
  
- Als de linkeroperand wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
  Onbekende evaluatie in **en** operator:  
  
```  
+---+---+---+---+  
|AND| T | F | U |  
+---+---+---+---+  
| T | T | F | U |  
+---+---+---+---+  
| F | F | F | F |  
+---+---+---+---+  
| U | U | F | U |  
+---+---+---+---+  
```  
  
 Onbekende evaluatie in **of** operator:  
  
```  
+---+---+---+---+  
|OR | T | F | U |  
+---+---+---+---+  
| T | T | T | T |  
+---+---+---+---+  
| F | T | F | U |  
+---+---+---+---+  
| U | T | U | U |  
+---+---+---+---+  
```  
  
### <a name="operator-binding-semantics"></a>Operator binding semantiek
  
-   Vergelijkingsoperators zoals `>`, `>=`, `<`, `<=`, `!=`, en `=` volgt u dezelfde semantiek als de C#-operator binding in de impliciete conversies en speciale aanbiedingen voor het type van gegevens.  
  
-   Rekenkundige operators zoals `+`, `-`, `*`, `/`, en `%` volgt u dezelfde semantiek als de C#-operator binding in de impliciete conversies en speciale aanbiedingen voor het type van gegevens.

## <a name="next-steps"></a>Volgende stappen

- [SQLFilter-klasse (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLFilter-klasse (.NET Standard)](/dotnet/api/microsoft.azure.servicebus.sqlfilter)
- [SQLRuleAction-klasse](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
