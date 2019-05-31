---
title: Azure SQL Database-functie-beperkingen | Microsoft Docs
description: Beperkingen voor Azure SQL Database-functie verbetert de databasebeveiliging van uw door functies in de database die door aanvallers toegang kunnen krijgen tot informatie in deze kan worden te beperken.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: ac7a074e78def504a10b4daa07971f919f414a88
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259450"
---
# <a name="azure-sql-database-feature-restrictions"></a>Beperkingen voor Azure SQL Database-functie

Een algemene gegevensbron van SQL Server-aanvallen, is via web-apps die toegang hebben tot de database waarbij verschillende vormen van SQL-injectieaanvallen worden gebruikt voor het verzamelen van informatie over de database.  In het ideale geval wordt toepassingscode ontwikkeld, zodat deze niet toe voor SQL-injectie dat staat.  In grote-codebases met verouderde en externe code, kan een echter nooit zijn ervoor dat alle gevallen zijn opgelost, zodat een feit van het leven die wij hebben om te beveiligen tegen SQL-injecties.  Het doel van de functiebeperkingen is om te voorkomen dat sommige formulieren van de SQL-injectie uit het lekken van informatie over de database, zelfs als de SQL-injectie geslaagd is.

## <a name="enabling-feature-restrictions"></a>Inschakelen van functiebeperkingen

Inschakelen van functiebeperkingen wordt uitgevoerd met behulp van de `sp_add_feature_restriction` opgeslagen procedure als volgt:

```sql
EXEC sp_add_feature_restriction <feature>, <object_class>, <object_name>
```

De volgende functies kunnen worden beperkt:

| Functie          | Description |
|------------------|-------------|
| N'ErrorMessages' | Als beperkt, worden alle gebruikersgegevens binnen het foutbericht wordt gemaskeerd. Zie [foutberichten functie beperking](#error-messages-feature-restriction) |
| N'Waitfor'       | Als beperkt, wordt de opdracht onmiddellijk geretourneerd zonder enige vertraging. Zie [WAITFOR functie beperking](#waitfor-feature-restriction) |

De waarde van `object_class` kan `N'User'` of `N'Role'` om aan te duiden of `object_name` is de naam van een gebruiker of de naam van een rol in de database.

Het volgende voorbeeld zorgt ervoor dat alle foutberichten voor gebruiker `MyUser` moet worden gemaskeerd:

```sql
EXEC sp_add_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="disabling-feature-restrictions"></a>Functiebeperkingen uitschakelen

Functiebeperkingen uitschakelen wordt gedaan met behulp van de `sp_drop_feature_restriction` opgeslagen procedure als volgt:

```sql
EXEC sp_drop_feature_restriction <feature>, <object_class>, <object_name>
```

Het volgende voorbeeld wordt een fout bericht maskeren voor gebruiker `MyUser`:

```sql
EXEC sp_drop_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="viewing-feature-restrictions"></a>Functiebeperkingen weergeven

De `sys.sql_feature_restrictions` weergave geeft alle functiebeperkingen van het momenteel gedefinieerde op de database. Het heeft de volgende kolommen:

| Kolomnaam | Gegevenstype | Description |
|-------------|-----------|-------------|
| Klasse       | nvarchar(128) | Klasse van het object waarop de beperking van toepassing is |
| object      | nvarchar(256) | Naam van het object waarop de beperking van toepassing is |
| Functie     | nvarchar(128) | Functie die is beperkt |

## <a name="feature-restrictions"></a>Functiebeperkingen

### <a name="error-messages-feature-restriction"></a>Fout berichten functie beperking

Een algemene SQL-injectie-aanval methode is het invoeren van code die een fout veroorzaakt.  Door het foutbericht, een aanvaller voor meer informatie over het systeem inschakelen van aanvullende meer gerichte aanvallen.  Deze aanval kan met name nuttig zijn wanneer de toepassing de resultaten van een query niet wordt weergegeven, maar er foutberichten worden weergegeven.

Houd rekening met een web-App met een aanvraag in de vorm van:

```html
http://www.contoso.com/employee.php?id=1
```

Die wordt uitgevoerd de volgende databasequery:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Als de waarde die is doorgegeven als de `id` parameter voor de aanvraag van de web ter vervanging van $EmpId in de databasequery is gekopieerd, een aanvaller kan maken van de volgende aanvraag:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(DB_NAME() AS INT)=0
```

En de volgende fout wordt geretourneerd, waardoor de aanvaller voor meer informatie over de naam van de database:

```sql
Conversion failed when converting the nvarchar value 'HR_Data' to data type int.
```

Na het inschakelen van de foutberichten functie beperking voor de gebruiker van de toepassing in de database, wordt het foutbericht geretourneerd zodat er geen interne informatie over de database is gelekt gemaskeerd:

```sql
Conversion failed when converting the ****** value '******' to data type ******.
```

De aanvaller kan op dezelfde manier, maken van de volgende aanvraag:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(Salary AS TINYINT)=0
```

En de volgende fout wordt geretourneerd, waardoor de aanvaller voor meer informatie over het salaris van de werknemer:

```sql
Arithmetic overflow error for data type tinyint, value = 140000.
```

Met behulp van fout berichten functie beperking, de database geretourneerd:

```sql
Arithmetic overflow error for data type ******, value = ******.
```

### <a name="waitfor-feature-restriction"></a>WAITFOR-functie beperking

Een Blind SQL-injectie is wanneer een toepassing heeft niet beschikt over een aanvaller met de resultaten van de geïnjecteerde SQL of met een foutbericht weergegeven, maar de aanvaller gegevens uit de database afleiden door een voorwaardelijke query waarin de twee Voorwaardelijke vertakkingen duren voordat een andere hoeveelheid tijd om uit te voeren. Door het vergelijken van de reactietijd, weet de aanvaller welke vertakking is uitgevoerd en waardoor meer informatie over het systeem. De eenvoudigste variant van deze aanval wordt met behulp van de `WAITFOR` instructie om te introduceren de vertraging.

Houd rekening met een web-App met een aanvraag in de vorm van:

```html
http://www.contoso.com/employee.php?id=1
```

die wordt uitgevoerd de volgende databasequery:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Als de waarde als de id-parameter doorgegeven aan de webtoepassingsaanvragen vast ter vervanging van $EmpId in de databasequery is gekopieerd, kan een aanvaller maken van de volgende aanvraag:

```html
http://www.contoso.com/employee.php?id=1; IF SYSTEM_USER='sa' WAITFOR DELAY '00:00:05'
```

En de query zou een extra 5 seconden duren als de `sa` account wordt gebruikt. Als `WAITFOR` beperking van de functie is uitgeschakeld in de database, de `WAITFOR` instructie worden genegeerd en niet informatie wordt gelekt met behulp van deze aanval.