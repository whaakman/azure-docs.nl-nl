---
title: Naslaginformatie over Azure Service Bus SQLFilter syntaxis | Microsoft Docs
description: Gegevens over SQLFilter grammatica.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: sethm
ms.openlocfilehash: 159bb0500dd98a428d60d6aeb78767a3460b40dc
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="sqlfilter-syntax"></a>SQLFilter syntaxis

Een *SqlFilter* object is een exemplaar van de [SqlFilter klasse](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), en geeft u een SQL-filter op basis van taal-expressie die wordt geëvalueerd op basis van een [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Een SqlFilter ondersteunt een subset van de SQL-92-standaard.  
  
 Dit onderwerp bevat informatie over SqlFilter grammatica.  
  
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
  
-   `<scope>`is een optionele tekenreeks die aangeeft van het bereik van de `<property_name>`. Geldige waarden zijn `sys` of `user`. De `sys` waarde geeft aan dat systeem bereik waar `<property_name>` is de naam van een openbare eigenschap van de [BrokeredMessage klasse](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user`bereik van de gebruiker geeft aan waar `<property_name>` is een sleutel van de [BrokeredMessage klasse](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) woordenlijst. `user`bereik is het standaardbereik als `<scope>` is niet opgegeven.  
  
## <a name="remarks"></a>Opmerkingen

Een poging tot toegang tot een niet-bestaande systeemeigenschap is een fout tijdens een poging tot toegang tot een niet-bestaande gebruikerseigenschap geen fout is. In plaats daarvan wordt de eigenschap van een niet-bestaande intern geëvalueerd als een onbekende waarde. Een onbekende waarde behandeld speciaal tijdens de evaluatie van de operator.  
  
## <a name="propertyname"></a>kubuskenmerkbinding  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Argumenten  

 `<regular_identifier>`een tekenreeks is vertegenwoordigd door de volgende reguliere expressie:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
Deze grammatica betekent een tekenreeks die begint met een letter en wordt gevolgd door een of meer onderstrepingsteken/letter/cijfers.  
  
`[:IsLetter:]`betekent een Unicode-teken is aangemerkt als een Unicode-letter. `System.Char.IsLetter(c)`retourneert `true` als `c` is een Unicode-letter.  
  
`[:IsDigit:]`betekent een Unicode-teken is aangemerkt als een decimaal cijfer. `System.Char.IsDigit(c)`retourneert `true` als `c` een Unicode-teken.  
  
Een `<regular_identifier>` mag geen gereserveerd trefwoord.  
  
`<delimited_identifier>`is een tekenreeks die is ingesloten door horizontaal vierkante haken ([]). Een vierkant haakje sluiten wordt weergegeven als twee rechts vierkante haken. Hieronder vindt u voorbeelden van `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
`<quoted_identifier>`is een tekenreeks die is ingesloten tussen dubbele aanhalingstekens. Dubbele aanhalingstekens in id wordt weergegeven als dubbele aanhalingstekens. Dit wordt niet aanbevolen voor id's tussen aanhalingstekens omdat kunnen eenvoudig worden verward met een tekenreeksconstante. Gebruik indien mogelijk een gescheiden id. Hieronder volgt een voorbeeld van `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>patroon  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Opmerkingen
  
`<pattern>`moet een expressie die wordt geëvalueerd als een tekenreeks zijn. Deze wordt gebruikt als een patroon voor de LIKE-operator.      Dit kan de volgende tekens bevatten:  
  
-   `%`: Een tekenreeks van nul of meer tekens.  
  
-   `_`: Een enkel teken.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Opmerkingen  

`<escape_char>`moet een expressie die wordt geëvalueerd als een tekenreeks met lengte 1. Deze wordt gebruikt als een escape-teken voor de LIKE-operator.  
  
 Bijvoorbeeld: `property LIKE 'ABC\%' ESCAPE '\'` overeenkomt met `ABC%` in plaats van een tekenreeks die met begint `ABC`.  
  
## <a name="constant"></a>constante  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Argumenten  
  
-   `<integer_constant>`is een reeks cijfers die niet tussen aanhalingstekens worden geplaatst en bevatten geen decimalen. De waarden worden opgeslagen als `System.Int64` intern en doe hetzelfde bereik.  
  
     Dit zijn voorbeelden van lange constanten:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`is een reeks cijfers die niet tussen dubbele aanhalingstekens, en bevatten een decimaalteken. De waarden worden opgeslagen als `System.Double` intern, en volgt u de hetzelfde bereik/precisie.  
  
     Dit nummer in een toekomstige versie worden opgeslagen in een ander gegevenstype voor de ondersteuning van exacte aantal semantiek, dus u dient niet te vertrouwen op het feit de onderliggende gegevenstype is `System.Double` voor `<decimal_constant>`.  
  
     Hier volgen enkele voorbeelden van decimal-constanten zijn:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`is een aantal geschreven in wetenschappelijke notatie. De waarden worden opgeslagen als `System.Double` intern, en volgt u de hetzelfde bereik/precisie. Hier volgen enkele voorbeelden van het geschatte aantal constanten:  
  
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

Boole-constanten worden vertegenwoordigd door de trefwoorden **TRUE** of **FALSE**. De waarden worden opgeslagen als `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Opmerkingen  

Tekenreeksconstanten zijn ingesloten in enkele aanhalingstekens en geen geldige unicodetekens bevatten. Een enkel aanhalingsteken ingesloten in een tekenreeksconstante wordt vertegenwoordigd als twee enkele aanhalingstekens.  
  
## <a name="function"></a>Functie  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Opmerkingen
  
De `newid()` functie retourneert een **System.Guid** die worden gegenereerd door de `System.Guid.NewGuid()` methode.  
  
De `property(name)` functie retourneert de waarde van de eigenschap waarnaar wordt verwezen door `name`. De `name` waarde kan een geldige expressie die een string-waarde zijn.  
  
## <a name="considerations"></a>Overwegingen
  
Overweeg het volgende [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantiek:  
  
-   De namen van eigenschappen zijn niet hoofdlettergevoelig.  
  
-   Operators Volg C# impliciete conversie semantiek indien mogelijk.  
  
-   Systeem zijn openbare eigenschappen beschikbaar zijn in [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) exemplaren.  
  
    Overweeg het volgende `IS [NOT] NULL` semantiek:  
  
    -   `property IS NULL`wordt geëvalueerd als `true` als de eigenschap bestaat niet of de waarde van de eigenschap is `null`.  
  
### <a name="property-evaluation-semantics"></a>Eigenschap evaluatie semantiek  
  
-   Een poging om te evalueren van een niet-bestaande systeemeigenschap genereert een [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) uitzondering.  
  
-   Een eigenschap die niet intern wordt geëvalueerd als **onbekende**.  
  
 Onbekende evaluatie in rekenkundige operatoren:  
  
-   Voor binaire operators, als ofwel de linkerkant en/of rechts van operanden wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
-   Voor unaire operators, als een operand wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
 Onbekende evaluatie in binaire vergelijkingsoperators:  
  
-   Als op de links en/of rechts van operanden wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
 Onbekende evaluatie in `[NOT] LIKE`:  
  
-   Als een operand wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
 Onbekende evaluatie in `[NOT] IN`:  
  
-   Als de linkeroperand wordt geëvalueerd als **onbekende**, dan is het resultaat **onbekende**.  
  
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
  
-   Vergelijkingsoperators zoals `>`, `>=`, `<`, `<=`, `!=`, en `=` volgt u dezelfde heeft als de C#-operator in gegevens type aanbiedingen en impliciete conversies binding.  
  
-   Rekenkundige operatoren zoals `+`, `-`, `*`, `/`, en `%` volgt u dezelfde heeft als de C#-operator in gegevens type aanbiedingen en impliciete conversies binding.

## <a name="next-steps"></a>Volgende stappen

- [SQLFilter klasse (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLFilter klasse (.NET Standard)](/dotnet/api/microsoft.azure.servicebus.filters.sqlfilter)
- [SQLRuleAction klasse](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)