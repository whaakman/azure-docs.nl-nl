---
title: Azure SQL Database dynamische gegevensmaskering | Microsoft docs
description: SQL-Database dynamische-gegevensmaskering blootstelling van gevoelige gegevens beperkt door het maskeren voor onbevoegde gebruikers
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: 4b36d78e-7749-4f26-9774-eed1120a9182
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/09/2017
ms.author: ronitr; ronmat
ms.openlocfilehash: 83deadce3cbdd30aa50d22d99378bd86133677c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="sql-database-dynamic-data-masking"></a>De dynamische gegevensmaskering SQL-Database

Gevoelige gegevens blootstelling SQL-Database dynamische-gegevensmaskering beperkt door het maskeren voor onbevoegde gebruikers. 

Dynamische gegevensmaskering helpt onbevoegde toegang tot gevoelige gegevens te voorkomen, doordat klanten kunnen aangeven hoeveel van de gevoelige gegevens mag worden vrijgegeven, met minimale gevolgen voor de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd.

Bijvoorbeeld, een medewerker van de in het midden van een aanroep van aanroepfuncties door verschillende cijfers van het nummer van hun creditcard kan worden geïdentificeerd, maar deze gegevensitems niet volledig worden blootgesteld aan de klantenservice. Een maskeringsregel kan worden gedefinieerd dat alle maskers, maar de laatste vier cijfers van een willekeurig aantal creditcard in het resultaat van een query moet worden ingesteld. Als een ander voorbeeld kan invoermasker van de juiste gegevens worden gedefinieerd als persoonsgegevens (PII)-gegevens beveiligen zodat een ontwikkelaar productieomgevingen opvragen kan voor het oplossen van problemen regelgeving overtreden.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL-Database dynamische-gegevensmaskering basisbeginselen
U instellen kunt een dynamische-gegevensmaskering beleid in de Azure-portal door te selecteren van de bewerking in de blade SQL Database-configuratie of de instellingenblade voor dynamische gegevensmaskering.

### <a name="dynamic-data-masking-permissions"></a>Machtigingen voor maskering van dynamische gegevens
Dynamische-gegevensmaskering kan worden geconfigureerd door de beheerder van de Azure-Database, serverbeheerder of officer beveiligingsrollen.

### <a name="dynamic-data-masking-policy"></a>Beleid voor maskering van dynamische gegevens
* **SQL-gebruikers uitgesloten van maskering** : een set van SQL-gebruikers of AAD-identiteiten die toegankelijk gegevens in de SQL ophalen-queryresultaten. Gebruikers met administrator-bevoegdheden zijn altijd uitgesloten van maskering en overzicht van de oorspronkelijke gegevens zonder eventuele masker.
* **Regels maskeren** -een set regels die definiëren de aangewezen velden om te maskeren en de maskeringsfunctie die wordt gebruikt. De aangewezen velden kunnen worden gedefinieerd met een schema databasenaam, de tabelnaam en de kolomnaam.
* **Functies maskeren** -een aantal methoden waarmee de blootstelling van gegevens voor verschillende scenario's.

| Maskeringsfunctie | Logica maskeren |
| --- | --- |
| **Standaard** |**Volledige maskering volgens de gegevenstypen van de desbetreffende velden**<br/><br/>• Gebruik XXXX of minder Xs als de grootte van het veld minder dan 4 tekens voor de gegevenstypen string (nchar, ntext, nvarchar is).<br/>• Gebruik een nulwaarde voor numerieke gegevenstypen (bigint, bits, decimal, int, money, numerieke, smallint, smallmoney, tinyint, float, real).<br/>• Gebruik 01-01-1900 voor datum/tijd-gegevenstypen (datum, datetime2, datetime, datetimeoffset, smalldatetime, tijd).<br/>• Voor SQL-variant, de standaardwaarde van het huidige type wordt gebruikt.<br/>• Voor het document XML <masked/> wordt gebruikt.<br/>• Gebruik een lege waarde voor speciale gegevenstypen (tijdstempel tabel, hierarchyid, GUID, binary, image, varbinary ruimtelijke typen). |
| **Creditcard** |**Methode waarmee de laatste vier cijfers van de desbetreffende velden maskeren** en voegt u een constante tekenreeks als voorvoegsel in de vorm van een creditcard toe.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-mail** |**Methode, waarbij de eerste letter beschrijft en vervangt het domein door XXX.com maskeren** met behulp van een constante tekenreeks voorvoegsel in de vorm van een e-mailadres.<br/><br/>aXX@XXXX.com |
| **Willekeurig getal** |**Methode, waarmee een willekeurig getal genereert maskeren** volgens de geselecteerde grenzen en de werkelijke gegevenstypen. Als de opgegeven grenzen gelijk zijn, is de maskeringsfunctie een constante waarde.<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Aangepaste tekst** |**Methode waarmee de eerste en laatste tekens maskeren** en wordt een tekenreeks met aangepaste opvulling toegevoegd in het midden. Als de oorspronkelijke reeks korter dan de blootgestelde voorvoegsel en het achtervoegsel is, wordt alleen de opvulling tekenreeks gebruikt. <br/>voorvoegsel [opvulling] achtervoegsel<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Aanbevolen velden om te maskeren
De engine van de aanbevelingen DDM vlaggen bepaalde velden uit de database als potentieel gevoelige velden, die mogelijk geschikt voor maskering. In de blade voor dynamische Gegevensmaskering in de portal ziet u de aanbevolen kolommen voor uw database. U hoeft te doen is, klikt u op **masker toevoegen** voor een of meer kolommen en vervolgens **opslaan** toepassen van een masker voor deze velden.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Instellen van de dynamische-gegevensmaskering voor de database met de Powershell-cmdlets
Zie [Cmdlets van Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Instellen van dynamische gegevens maskeren voor de database met de REST-API
Zie [bewerkingen voor Azure SQL-Databases](https://msdn.microsoft.com/library/dn505719.aspx).

