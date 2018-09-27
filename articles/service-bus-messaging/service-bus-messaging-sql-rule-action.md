---
title: De verwijzing naar de SQLRuleAction-syntaxis in Azure | Microsoft Docs
description: Gegevens over SQLRuleAction-grammatica.
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
ms.openlocfilehash: cd5bc1fb959872cfb98236feb5c32517ac7472ac
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394179"
---
# <a name="sqlruleaction-syntax"></a>SQLRuleAction-syntaxis

Een *SqlRuleAction* is een exemplaar van de [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) klasse en vertegenwoordigt set acties die zijn geschreven in SQL-taal op basis van syntaxis die is uitgevoerd op een [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).   
  
In dit artikel bevat informatie over de SQL-grammatica regel actie.  
  
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
  
-   `<scope>` is een optionele tekenreeks die aangeeft van het bereik van de `<property_name>`. Geldige waarden zijn `sys` of `user`. De `sys` waarde geeft aan dat systeem bereik waar `<property_name>` is de naam van een openbare eigenschap van de [BrokeredMessage klasse](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user` gebruikersbereik van geeft aan waar `<property_name>` is een sleutel van de [BrokeredMessage klasse](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) woordenlijst. `user` bereik is het bereik van standaard als `<scope>` is niet opgegeven.  
  
### <a name="remarks"></a>Opmerkingen  

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
  
 Dit betekent dat een tekenreeks die begint met een letter en wordt gevolgd door een of meer onderstrepingsteken/letter/cijfers.  
  
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
  
-   `%`: Een reeks nul of meer tekens.  
  
-   `_`: Eén willekeurig teken.  
  
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
  
     Hier volgen enkele voorbeelden van lange constanten:  
  
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
  
Booleaanse constanten worden vertegenwoordigd door de trefwoorden `TRUE` of `FALSE`. De waarden worden opgeslagen als `System.Boolean`.  
  
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

- SET wordt gebruikt om een nieuwe eigenschap maken of bijwerken van de waarde van een bestaande eigenschap.
- VERWIJDEREN wordt gebruikt om een eigenschap te verwijderen.
- SET worden impliciete conversie indien mogelijk uitgevoerd als het Expressietype en de bestaande eigenschap verschillend zijn.
- Actie mislukt als niet-bestaande Systeemeigenschappen werd verwezen.
- Actie mislukt niet als niet-bestaande gebruikerseigenschappen werd verwezen.
- Een eigenschap van een niet-bestaande wordt geëvalueerd als 'Onbekend' intern, volgt dezelfde semantiek als [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) bij het evalueren van operators.

## <a name="next-steps"></a>Volgende stappen

- [SQLRuleAction-klasse](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter-klasse](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
