---
title: Azure SQL Data Warehouse op kolomniveau beveiliging | Microsoft Docs
description: Beveiliging op kolomniveau (CLS) kunnen klanten toegang tot de database tabelkolommen op basis van de uitvoeringscontext van de gebruiker of het groepslidmaatschap. CLS vereenvoudigt het ontwerp en het coderen van beveiliging in uw toepassing. CLS kunt u beperkingen op kolom toegang implementeren.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 06/15/2018
ms.author: kavithaj
ms.reviewer: igorstan, carlrab
ms.openlocfilehash: 5a916132f705f3c517ee6789b61a3972b2445b62
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938422"
---
# <a name="column-level-security"></a>Beveiligingsniveau voor de kolom 
Beveiliging op kolomniveau (CLS) kunnen klanten toegang tot de database tabelkolommen op basis van de uitvoeringscontext van de gebruiker of het groepslidmaatschap.  

CLS vereenvoudigt het ontwerp en het coderen van beveiliging in uw toepassing. CLS kunt u beperkingen op kolom toegang tot gevoelige gegevens beveiligen te implementeren. Bijvoorbeeld, ervoor zorgen dat die specifieke gebruikers toegang krijgen alleen bepaalde kolommen van een tabel die relevant zijn voor hun afdeling tot. De beperking toegang logica is gevonden in de databaselaag in plaats van opslag van de gegevens in een andere toepassingslaag. De database van de toegangsbeperkingen toepassing telkens wanneer de toegang tot die gegevens van elke categorie wordt uitgevoerd. Dit maakt het beveiligingssysteem betrouwbaarder en robuuste doordat het serveroppervlaktegebied van uw algehele beveiligingssysteem. Bovendien ook hierdoor nodig voor de introductie van weergaven voor het filteren van de kolommen voor de toegangsbeperkingen voor de gebruikers om op te leggen. 

Kan het implementeren van CLS met de [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) T-SQL-instructie. Met dit mechanisme verificatie zowel SQL en Azure Active Directory (AAD) worden ondersteund.

![CLS](./media/column-level-security/cls.png)

## <a name="syntax"></a>Syntaxis 

```sql
GRANT <permission> [ ,...n ] ON    
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]   
    TO <database_principal> [ ,...n ]    
    [ WITH GRANT OPTION ]   
    [ AS <database_principal> ]   
<permission> ::=   
    SELECT 
  | UPDATE  
<database_principal> ::=    
      Database_user    
    | Database_role    
    | Database_user_mapped_to_Windows_User    
    | Database_user_mapped_to_Windows_Group    
```

## <a name="example"></a>Voorbeeld 
Het volgende voorbeeld ziet u hoe 'Testgebruiker' toegang tot de kolom 'SSN' van tabel 'Lidmaatschap' beperken: 

Maak 'Lidmaatschap' tabel met SSN kolom gebruikt voor het opslaan van sofi-nummer:

```sql
CREATE TABLE Membership   
  (MemberID int IDENTITY,   
   FirstName varchar(100) NULL,   
   SSN char(9) NOT NULL, 
   LastName varchar(100) NOT NULL,   
   Phone varchar(12) NULL,   
   Email varchar(100) NULL);  
```

'Testgebruiker' toestaan voor toegang tot alle kolommen behalve SSN kolom met gevoelige gegevens: 

```sql  
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;   
``` 

Query's uitgevoerd, zoals 'Testgebruiker' mislukken als ze de SSN-kolom is opgenomen:

```sql  
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'. 
``` 

## <a name="use-cases"></a>Gebruiksvoorbeelden
Enkele voorbeelden van hoe CLS vandaag nog wordt gebruikt: 
- Een onderneming in financiële diensten kunt enige account managers toegang hebben tot de klant sofi-nummer (Sofinummer), telefoonnummers en andere persoonsgegevens (PII).
- Een gezondheidszorg provider kan alleen arts en verpleegkundigen toegang hebben tot gevoelige medische gegevens tijdens de leden van de facturering afdeling is niet toegestaan om deze gegevens weer te geven.
