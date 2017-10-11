---
title: Verwijzing naar de SQLRuleAction in Azure | Microsoft Docs
description: Gegevens over SQLRuleAction grammatica.
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
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 7379b7f58563675f28d77928d933c0d9c7992e71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="sqlruleaction-syntax"></a>SQLRuleAction syntaxis

Een *SqlRuleAction* is een exemplaar van de [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) klasse en vertegenwoordigt set acties die zijn geschreven in SQL-taal gebaseerd syntaxis die wordt uitgevoerd op een [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).   
  
Dit onderwerp bevat informatie over de SQL-grammatica regel in te grijpen.  
  
```  
<statements> ::=
    <statement> [, ...n]  
  
```  
  
```  
<statement> ::=
    <action> [;]
    Remarks
    -------
    Semicolon is optional.  
  
```  
  
```  
<action> ::=
    SET <property> = <expression>
    REMOVE <property>  
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
  
### <a name="remarks"></a>Opmerkingen  

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
  
 Dit betekent dat een tekenreeks die begint met een letter en wordt gevolgd door een of meer onderstrepingsteken/letter/cijfers.  
  
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
  
     Hier volgen enkele voorbeelden van lange-constanten zijn:  
  
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
  
Boole-constanten worden vertegenwoordigd door de trefwoorden `TRUE` of `FALSE`. De waarden worden opgeslagen als `System.Boolean`.  
  
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

- Een nieuwe eigenschap maken of bijwerken van de waarde van een bestaande eigenschap worden gebruikt.
- VERWIJDEREN wordt gebruikt om een eigenschap te verwijderen.
- SET voert impliciete conversie indien mogelijk wanneer het Expressietype en het bestaande eigenschapstype verschillen.
- Actie mislukt als niet-bestaande Systeemeigenschappen zijn waarnaar wordt verwezen.
- Actie mislukt niet als niet-bestaande gebruikerseigenschappen zijn waarnaar wordt verwezen.
- Een eigenschap van een niet-bestaande wordt geëvalueerd als 'Onbekend' intern, volgt dezelfde heeft als [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) bij het evalueren van operators.

## <a name="next-steps"></a>Volgende stappen

- [SQLRuleAction klasse](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter klasse](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
