---
title: Azure SQL Data Warehouse op kolomniveau-beveiliging | Microsoft Docs
description: Beveiliging op kolomniveau (CLS) kunnen klanten voor het beheren van toegang tot database tabelkolommen op basis van de uitvoeringscontext van de gebruiker of hun groepslidmaatschap. CLS vereenvoudigt het ontwerp en de code van de beveiliging in uw toepassing. CLS kunt u beperkingen op kolom toegang implementeert.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 06/15/2018
ms.author: kavithaj
ms.reviewer: igorstan, carlrab
ms.openlocfilehash: 379425a4c0b21234fb4e48dc1c8a56cfea645045
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55727378"
---
# <a name="column-level-security"></a>Beveiliging op kolomniveau
Beveiliging op kolomniveau (CLS) kunnen klanten voor het beheren van toegang tot database tabelkolommen op basis van de uitvoeringscontext van de gebruiker of hun groepslidmaatschap.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]

CLS vereenvoudigt het ontwerp en de code van de beveiliging in uw toepassing. CLS kunt u voor het implementeren van beperkingen op de kolom toegang tot gevoelige gegevens te beschermen. Bijvoorbeeld, ervoor te zorgen dat bepaalde gebruikers kunnen alleen bepaalde kolommen van een tabel die relevant zijn voor hun afdeling. De logica van de beperking van toegang is gevonden in de databaselaag in plaats van opslag van de gegevens in een andere toepassingslaag. De database is van toepassing de toegangsbeperkingen telkens wanneer die toegang tot gegevens van elke laag wordt uitgevoerd. Deze beperking maakt het beveiligingssysteem betrouwbaarder en robuuste door te verminderen van het gebied van de algehele beveiligingssysteem. Bovendien hoeft CLS ook u voor de invoering van weergaven uit de kolommen voor de toepassing van toegangsbeperkingen voor de gebruikers te filteren.

Kan u CLS met implementeert de [verlenen](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) T-SQL-instructie. Met dit mechanisme verificatie zowel SQL en Azure Active Directory (AAD) worden ondersteund.

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
Het volgende voorbeeld laat zien hoe om te voorkomen dat 'TestUser' toegang tot de kolom 'SSN' van 'Lidmaatschap' tabel:

'Lidmaatschap'-tabel maken met SSN-kolom die wordt gebruikt voor het opslaan van sociale-fiscale nummers bevatten:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

'TestUser' toestaan voor toegang tot alle kolommen behalve SSN kolom dat gevoelige gegevens bevat:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

Query's uitgevoerd, zoals 'TestUser' mislukken als ze de SSN-kolom is opgenomen:

```sql
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Gebruiksvoorbeelden
Enkele voorbeelden van hoe CLS vandaag nog wordt gebruikt:
- Een bedrijf voor financiële dienstverlening kunt enige account managers toegang hebben tot de klant BSN-nummers (SSN), telefoonnummers en andere persoonlijk identificeerbare informatie (PII).
- Een provider van de gezondheidszorg kan alleen artsen en verpleegkundigen hebben toegang tot gevoelige medische records bij leden van de facturering afdeling is niet toegestaan om deze gegevens weer te geven.
